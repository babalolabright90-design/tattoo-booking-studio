# Vercel Deployment Guide

## Prerequisites

- GitHub repository (already created)
- Vercel account (free at https://vercel.com)
- PostgreSQL database (Supabase, Railway, or other provider)
- Environment variables configured

## Step-by-Step Deployment

### 1. Create Vercel Account

1. Go to [Vercel](https://vercel.com)
2. Sign up with GitHub
3. Grant Vercel access to your GitHub account

### 2. Create PostgreSQL Database

Choose one of these options:

#### Option A: Supabase (Recommended)

1. Go to [Supabase](https://supabase.com)
2. Create new project
3. Copy connection string from Settings > Database > Connection String
4. Use this as `DATABASE_URL`

#### Option B: Railway

1. Go to [Railway](https://railway.app)
2. Create new project
3. Add PostgreSQL
4. Copy database URL

#### Option C: Render

1. Go to [Render](https://render.com)
2. Create PostgreSQL database
3. Copy external database URL

### 3. Import Project to Vercel

1. In Vercel Dashboard, click "Add New" → "Project"
2. Select your `tattoo-booking-studio` repository
3. Click "Import"

### 4. Configure Environment Variables

In Vercel Project Settings → Environment Variables, add:

```
DATABASE_URL = postgresql://user:password@host:5432/dbname
AUTH_SECRET = (generate with: openssl rand -base64 32)
NEXTAUTH_URL = https://your-domain.vercel.app
NEXT_PUBLIC_SITE_URL = https://your-domain.vercel.app
NEXT_PUBLIC_SITE_NAME = Your Studio Name

# Stripe (optional)
STRIPE_SECRET_KEY = sk_live_...
STRIPE_WEBHOOK_SECRET = whsec_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY = pk_live_...

# Resend (optional)
RESEND_API_KEY = re_...
EMAIL_FROM = noreply@yourdomain.com
ADMIN_EMAIL = admin@yourdomain.com

# Cloudinary (optional)
NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME = your_cloud_name
CLOUDINARY_API_KEY = your_api_key
CLOUDINARY_API_SECRET = your_api_secret
```

**Important**: Set environment variables for all deployment environments (Production, Preview, Development).

### 5. Configure Database Connection

For Supabase + Vercel:

1. In Supabase, go to Settings > Database
2. Copy "Connection String" (not "Connection Pooler")
3. Replace `[YOUR-PASSWORD]` with your actual password
4. Add to Vercel as `DATABASE_URL`

**Note**: Use Connection Pooling if you have many concurrent connections:
- Copy "Connection Pooler" instead
- Change port from 5432 to 6543 (or as shown)

### 6. Deploy

1. Back in Vercel, click "Deploy"
2. Wait for build to complete (usually 2-3 minutes)
3. Once deployment succeeds, you'll get your URL: `https://your-project.vercel.app`

### 7. Run Database Migrations

After first deployment:

```bash
# In your local project
NEXTAUTH_URL=https://your-domain.vercel.app npm run db:push

# Or seed data:
NEXTAUTH_URL=https://your-domain.vercel.app npm run db:seed
```

Alternatively, use Vercel CLI:

```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Link project
vercel link

# Run migrations on production
vercel env pull
npm run db:push
```

### 8. Verify Deployment

1. Open your Vercel URL in browser
2. Check homepage loads
3. Try accessing `/admin` (should redirect to login)
4. Check console for errors

### 9. Configure Custom Domain

1. In Vercel Dashboard → Project Settings → Domains
2. Add your custom domain (e.g., `yourstudio.com`)
3. Add DNS records as shown by Vercel
4. Wait for DNS to propagate (can take up to 48 hours)
5. Update `NEXTAUTH_URL` and `NEXT_PUBLIC_SITE_URL` to custom domain

### 10. Set Up Stripe Webhooks

1. In Stripe Dashboard → Developers → Webhooks
2. Add new endpoint:
   - URL: `https://your-domain.com/api/webhooks/stripe`
   - Events: `payment_intent.succeeded`, `payment_intent.payment_failed`
3. Get signing secret (starts with `whsec_`)
4. Add to Vercel as `STRIPE_WEBHOOK_SECRET`

### 11. Configure Email (Resend)

1. In Resend Dashboard → API Keys
2. Copy your API key
3. Add to Vercel as `RESEND_API_KEY`
4. Set `EMAIL_FROM` to your domain email
5. Verify domain in Resend settings

### 12. Set Up Monitoring

#### Error Tracking
1. Install Sentry (optional)
```bash
npm install @sentry/nextjs
```

#### Analytics
1. Add Google Analytics or Vercel Analytics
2. Update tracking ID in environment variables

## Automatic Deployments

Vercel automatically deploys when you push to GitHub:

```bash
# This triggers automatic deployment
git push origin main
```

### Preview Deployments

Every pull request gets a preview deployment:
1. Push to new branch
2. Create pull request on GitHub
3. Vercel automatically creates preview URL
4. Test before merging to main

## Environment Variables by Stage

Set different values for each stage:

| Variable | Production | Preview | Development |
|----------|------------|---------|-------------|
| `NEXTAUTH_URL` | https://yourdomain.com | https://pr-preview-*.vercel.app | http://localhost:3000 |
| `DATABASE_URL` | Production DB | Same or staging | Local DB |
| `STRIPE_SECRET_KEY` | Live key | Test key | Test key |
| Debug variables | false | true | true |

## Troubleshooting

### Build Fails

```bash
# Check build logs in Vercel Dashboard
# Common issues:

# 1. TypeScript errors
npm run type-check

# 2. Missing environment variables
vercel env pull

# 3. Prisma issues
npm run db:generate
```

### Database Connection Fails

```bash
# Verify DATABASE_URL
echo $DATABASE_URL

# Test connection
prisma db execute --stdin

# Check if database exists
```

### Authentication Not Working

```bash
# Ensure AUTH_SECRET is set
echo $AUTH_SECRET | wc -c  # Should be 32+ characters

# Ensure NEXTAUTH_URL is correct
echo $NEXTAUTH_URL

# Check browser cookies
```

### Images Not Loading

```bash
# Verify Cloudinary credentials
echo $NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME

# Check image URLs in Cloudinary dashboard
```

## Performance Optimization

### Enable Caching

Vercel automatically caches:
- Next.js builds
- Static assets
- API responses (with appropriate headers)

### Optimize Database

```bash
# Add indexes for common queries
prisma db execute --stdin < optimize.sql
```

### Image Optimization

Cloudinary handles:
- Format optimization (WebP, AVIF)
- Responsive images
- CDN delivery

## Monitoring & Logs

### View Logs

1. Vercel Dashboard → Project → Deployments
2. Click deployment → View Logs
3. Filter by type: Build, Edge, Function, Runtime

### Set Up Alerts

1. Vercel Dashboard → Project Settings → Analytics
2. Enable Web Vitals
3. Set alert thresholds

## Database Backups

### Supabase

1. Settings → Database → Backups
2. Automatic daily backups (7-day retention)
3. Manual backups available

### Railway

1. Dashboard → Database → Backups
2. Automatic backups

## Disaster Recovery

### Restore from Backup

```bash
# 1. Get backup URL from your DB provider
# 2. Create new database from backup
# 3. Update DATABASE_URL in Vercel
# 4. Redeploy
vercel redeploy
```

## Scaling

### When to Scale

- Free plan: <100 daily active users
- Pro plan: 100-1000 DAU
- Enterprise: 1000+ DAU + custom requirements

### Upgrade Plan

1. Vercel Dashboard → Account Settings → Plan
2. Click "Upgrade"
3. Select Pro or Enterprise
4. Billing updates automatically

### Database Scaling

For high traffic:
- Use connection pooling (Supabase)
- Add read replicas
- Increase compute resources
- Consider sharding strategy

## Cost Optimization

### Vercel
- Free: Up to 100GB/month bandwidth
- Pro: $20/month + $0.15/GB overage
- Optimize:
  - Enable automatic image optimization
  - Use static generation
  - Minimize serverless function size

### PostgreSQL (Supabase)
- Free: 500 MB storage, 2GB bandwidth/month
- Pro: $25/month
- Optimize:
  - Add indexes
  - Archive old data
  - Use connection pooling

### Stripe
- 2.9% + $0.30 per transaction
- No monthly fee
- Optimize:
  - Batch small payments
  - Use Stripe Tax for automatic tax

### Resend
- Free: 100 emails/day
- Pro: $20/month for unlimited
- Optimize:
  - Queue emails
  - Batch send where possible

## Production Checklist

- [ ] All environment variables set for production
- [ ] Custom domain configured
- [ ] SSL certificate active (auto with Vercel)
- [ ] Database backups enabled
- [ ] Admin password changed
- [ ] Stripe live keys configured
- [ ] Webhooks configured
- [ ] Email service verified
- [ ] Cloudinary production account
- [ ] Monitoring and alerts set up
- [ ] Error tracking enabled
- [ ] Analytics enabled
- [ ] Robots.txt and sitemap ready
- [ ] Security headers configured
- [ ] Rate limiting enabled
- [ ] Demo data removed/archived
- [ ] HTTPS enforced
- [ ] Tested full user flow
- [ ] Tested admin flow
- [ ] Tested payment flow
- [ ] Tested email notifications
- [ ] Mobile responsiveness verified
- [ ] SEO checked
- [ ] Performance audited
- [ ] Accessibility tested

## Continuous Deployment Workflow

1. **Local Development**
   ```bash
   git checkout -b feature/new-feature
   npm run dev
   # Make changes
   ```

2. **Test Locally**
   ```bash
   npm run build
   npm run lint
   npm run type-check
   ```

3. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Add new feature"
   git push origin feature/new-feature
   ```

4. **Create Pull Request**
   - Vercel automatically creates preview deployment
   - Review and test preview URL

5. **Merge to Main**
   ```bash
   # After approval
   git checkout main
   git merge feature/new-feature
   git push origin main
   ```

6. **Automatic Production Deployment**
   - Vercel detects push to main
   - Runs build and tests
   - Deploys to production
   - Creates deployment URL

## Support

- [Vercel Docs](https://vercel.com/docs)
- [Vercel Support](https://vercel.com/support)
- [Supabase Docs](https://supabase.com/docs)
- [Next.js Docs](https://nextjs.org/docs)

---

Your tattoo studio website is now deployed and ready for production! 🎨
