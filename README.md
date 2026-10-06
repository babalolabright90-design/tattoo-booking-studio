# Tattoo Studio Booking Website

A complete, production-ready tattoo artist booking system built with Next.js 15, TypeScript, Prisma, and Stripe.

## Features

✅ **Public Site**
- Premium dark luxury design
- Home, Portfolio, Artists, Services, Pricing pages
- Individual tattoo detail pages
- About, Contact, FAQ, Legal pages
- SEO optimized with metadata and structured data
- Fast, mobile-first responsive design

✅ **Booking System**
- Complete appointment booking form
- Artist availability management
- Prevent double-booking
- Multiple booking statuses
- Reference image uploads
- Email confirmations

✅ **Admin Dashboard**
- Secure authentication
- Booking management (approve, reject, reschedule, cancel)
- Customer profiles and history
- Artist management
- Portfolio image upload and management
- Availability and blocked dates
- Services management
- Calendar view (day, week, month)
- Business settings
- Analytics and dashboard

✅ **Payment Integration**
- Stripe deposits (optional)
- Fixed or percentage deposits
- Payment status tracking
- Webhooks for payment updates

✅ **Image Management**
- Cloudinary integration for portfolio images
- Optimized image delivery
- Secure upload handling

✅ **Email Notifications**
- Resend email service integration
- Professional HTML templates
- Multiple notification types
- Admin and customer emails

## Tech Stack

- **Framework**: Next.js 15+ with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: shadcn/ui
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: Auth.js (NextAuth v5)
- **Payments**: Stripe
- **Email**: Resend
- **Images**: Cloudinary
- **Deployment**: Vercel

## Project Structure

```
tatoo-booking-studio/
├── src/
│   ├── app/                    # Next.js 15 App Router
│   │   ├── (public)/          # Public pages
│   │   ├── (auth)/            # Auth pages
│   │   ├── admin/             # Admin dashboard
│   │   ├── api/               # API routes and webhooks
│   │   ├── layout.tsx         # Root layout
│   │   └── globals.css        # Global styles
│   ├── components/            # Reusable React components
│   ├── lib/                   # Utilities and helpers
│   ├── actions/               # Server actions
│   ├── schemas/               # Zod validation schemas
│   └── types/                 # TypeScript types
├── prisma/
│   ├── schema.prisma          # Database schema
│   ├── seed.ts                # Database seeding script
│   └── migrations/            # Database migrations
├── public/                     # Static assets
├── .env.example               # Environment variables template
├── .eslintrc.json             # ESLint config
├── prettier.config.js         # Prettier config
├── tailwind.config.ts         # Tailwind config
├── tsconfig.json              # TypeScript config
├── next.config.ts             # Next.js config
└── package.json               # Dependencies
```

## Getting Started

### Prerequisites

- Node.js 18+ and npm/yarn
- PostgreSQL database
- Accounts (optional but recommended):
  - Stripe (for payments)
  - Resend (for emails)
  - Cloudinary (for images)

### 1. Clone and Install

```bash
git clone https://github.com/babalolabright90-design/tattoo-booking-studio.git
cd tattoo-booking-studio
npm install
```

### 2. Set Up Environment Variables

```bash
cp .env.example .env.local
```

Edit `.env.local` and add your configuration:

```env
DATABASE_URL="postgresql://user:password@localhost:5432/tattoo_studio"
AUTH_SECRET="generate-a-secure-random-string-min-32-chars"
NEXTAUTH_URL="http://localhost:3000"

# Optional but recommended
STRIPE_SECRET_KEY="sk_test_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."
RESEND_API_KEY="re_..."
NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME="your_cloud_name"
```

### 3. Set Up Database

```bash
# Create database
npm run db:push

# Seed with demo data
npm run db:seed

# View database
npm run db:studio
```

### 4. Create Admin Account

The database seed includes a demo admin account:
- Email: `admin@example.com`
- Password: `admin123`

**⚠️ Change this password immediately in production!**

### 5. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

**Admin Dashboard**: [http://localhost:3000/admin](http://localhost:3000/admin)

## Configuration

### Business Settings

Edit settings in the admin dashboard at `/admin/settings` or via the database:

```sql
UPDATE "SiteSettings"
SET "siteName" = 'Your Studio Name',
    "businessEmail" = 'contact@yourstudio.com',
    "businessPhone" = '+1234567890',
    "businessAddress" = '123 Main St',
    "depositType" = 'percentage',
    "depositPercentage" = 20
WHERE id = '1';
```

### Stripe Integration

1. Get your keys from [Stripe Dashboard](https://dashboard.stripe.com/apikeys)
2. Add to `.env.local`:
   ```env
   STRIPE_SECRET_KEY=sk_test_...
   NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
   STRIPE_WEBHOOK_SECRET=whsec_...
   ```
3. Set webhook endpoint in Stripe Dashboard to: `https://yourdomain.com/api/webhooks/stripe`

### Email Configuration (Resend)

1. Get API key from [Resend](https://resend.com)
2. Add to `.env.local`:
   ```env
   RESEND_API_KEY=re_...
   EMAIL_FROM=noreply@yourstudio.com
   ```
3. Verify your domain in Resend

### Cloudinary (Image Storage)

1. Create account at [Cloudinary](https://cloudinary.com)
2. Get credentials from dashboard
3. Add to `.env.local`:
   ```env
   NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME=your_cloud_name
   CLOUDINARY_API_KEY=your_api_key
   CLOUDINARY_API_SECRET=your_api_secret
   ```

## Database Management

### Migrations

```bash
# Create new migration
npm run db:migrate

# Apply migrations
npm run db:push

# Reset database (development only)
prisma migrate reset
```

### Seed Data

The seed script includes:
- 3 demo artists
- 10 portfolio tattoos
- 6 services
- Example bookings
- Sample testimonials
- Business settings

To re-seed:

```bash
npm run db:seed
```

## Deployment

### Deploy to Vercel

1. Push your code to GitHub
2. Connect GitHub repo to [Vercel](https://vercel.com)
3. Add environment variables in Vercel Dashboard
4. Deploy

```bash
# Vercel automatically runs:
git push origin main
```

### Production Checklist

- [ ] Change admin password
- [ ] Set `NEXTAUTH_URL` to your production domain
- [ ] Generate secure `AUTH_SECRET` (min 32 chars)
- [ ] Configure Stripe production keys
- [ ] Set up Resend with verified domain
- [ ] Configure Cloudinary production account
- [ ] Update business settings (name, email, phone, address)
- [ ] Remove demo data or mark as archived
- [ ] Set up custom domain in Vercel
- [ ] Enable HTTPS
- [ ] Configure SSL certificate
- [ ] Set up monitoring and error tracking
- [ ] Test booking flow end-to-end
- [ ] Test payment flow with Stripe test mode
- [ ] Verify email notifications are sending
- [ ] Test on mobile devices
- [ ] Run accessibility audit
- [ ] Test SEO (metadata, structured data)
- [ ] Set up analytics (Google Analytics, etc.)
- [ ] Configure backup strategy for database

## API Routes

### Public
- `GET /api/portfolio` - Get all portfolio items
- `GET /api/artists` - Get all artists
- `GET /api/services` - Get all services
- `POST /api/bookings` - Submit booking request
- `GET /api/availability` - Get artist availability

### Admin (Protected)
- `GET /api/admin/bookings` - List bookings
- `PUT /api/admin/bookings/:id` - Update booking
- `GET /api/admin/customers` - List customers
- `POST /api/admin/artists` - Create artist
- `GET /api/admin/settings` - Get settings
- `PUT /api/admin/settings` - Update settings

### Webhooks
- `POST /api/webhooks/stripe` - Stripe payment events

## Security

✅ Environment variables for all secrets
✅ Server-side validation with Zod
✅ Protected admin routes
✅ Secure file uploads to Cloudinary
✅ CSRF protection
✅ Secure HTTP headers
✅ No secrets in version control
✅ Rate limiting on booking form
✅ Input sanitization

## Performance

- Image optimization with Cloudinary
- Next.js Image component for fast loading
- Server components for reduced client JS
- Static site generation where possible
- Database query optimization
- Caching strategies
- Mobile-first responsive design

## SEO

✅ Metadata and Open Graph tags
✅ JSON-LD structured data
✅ Sitemap generation
✅ Robots.txt
✅ Canonical URLs
✅ SEO-friendly URL structure
✅ Mobile responsiveness
✅ Fast page load times
✅ Semantic HTML

## Troubleshooting

### Database Connection Issues

```bash
# Test connection
prisma db execute --stdin

# Check DATABASE_URL format
echo $DATABASE_URL
```

### Authentication Issues

Ensure:
- `AUTH_SECRET` is set and 32+ characters
- `NEXTAUTH_URL` matches your domain
- Cookies are enabled in browser

### Payment Issues

- Check Stripe webhook configuration
- Verify API keys are correct
- Check Stripe Dashboard for errors
- Monitor webhook logs

### Email Not Sending

- Verify Resend API key
- Check email from domain is verified
- Monitor Resend Dashboard
- Check spam folder

## Support

For issues and questions:
1. Check GitHub Issues
2. Review documentation
3. Check environment variables
4. Review logs in Vercel Dashboard

## License

MIT License - See LICENSE file for details

## Contributing

Contributions welcome! Please create a pull request.

---

**Built with ❤️ for tattoo artists everywhere**
