# Tattoo Studio Security & Production Checklist

## Pre-Deployment Security

### Environment Variables

- [ ] No hardcoded secrets in code
- [ ] All secrets in `.env.local` (local) and Vercel (production)
- [ ] `.env.example` has no real values
- [ ] `.env*` files in `.gitignore`
- [ ] `AUTH_SECRET` is 32+ random characters
- [ ] Generated with: `openssl rand -base64 32`

### Authentication

- [ ] Admin login protected
- [ ] Protected routes use middleware
- [ ] Sessions stored securely
- [ ] Cookies httpOnly and secure
- [ ] CSRF tokens on forms
- [ ] Admin password strong (min 12 chars)
- [ ] Password changed after first login
- [ ] No demo credentials in production

### Database Security

- [ ] Database URL contains strong password
- [ ] SSL connection enforced
- [ ] Database backups enabled
- [ ] Access restricted to application only
- [ ] No direct database access exposed
- [ ] Row-level security policies (if using Supabase)
- [ ] Regular backups tested for restore

### API Security

- [ ] All admin endpoints protected
- [ ] Server-side validation on all inputs
- [ ] Input sanitization implemented
- [ ] SQL injection prevention (Prisma)
- [ ] Rate limiting on booking form
- [ ] Rate limiting on auth endpoints
- [ ] CORS headers properly configured
- [ ] API keys not exposed in client code

### File Uploads

- [ ] Cloudinary API keys secure
- [ ] Upload signatures generated server-side
- [ ] File type validation
- [ ] File size limits enforced
- [ ] Malicious file detection
- [ ] Original filenames not used
- [ ] Uploaded files scanned for malware

### Payment Security (Stripe)

- [ ] Stripe keys in environment only
- [ ] Webhook signature verification
- [ ] Webhook URL is HTTPS
- [ ] PCI compliance level 1
- [ ] Never store card details
- [ ] Use Stripe Payment Intent API
- [ ] Webhook secret configured in Vercel
- [ ] Payment logging doesn't include sensitive data

### Email Security (Resend)

- [ ] API key in environment variables
- [ ] From domain verified
- [ ] SPF records configured
- [ ] DKIM records configured
- [ ] DMARC policy configured
- [ ] Unsubscribe links in emails
- [ ] No passwords in emails
- [ ] Email templates sanitized

### HTTPS & SSL

- [ ] HTTPS enforced site-wide
- [ ] SSL certificate auto-renewed
- [ ] Redirect HTTP to HTTPS
- [ ] HSTS headers enabled
- [ ] No mixed content warnings

### Headers & Security Policies

- [ ] X-Content-Type-Options: nosniff
- [ ] X-Frame-Options: DENY
- [ ] X-XSS-Protection: 1; mode=block
- [ ] Referrer-Policy: strict-origin-when-cross-origin
- [ ] Content-Security-Policy headers
- [ ] Permissions-Policy headers

### Input Validation

- [ ] Email addresses validated
- [ ] Phone numbers validated
- [ ] Dates validated
- [ ] File types validated
- [ ] File sizes limited
- [ ] Text fields length limited
- [ ] Zod schemas on all forms
- [ ] Client AND server-side validation

### Access Control

- [ ] Admin-only routes protected
- [ ] User can only see own bookings
- [ ] Admin can see all bookings
- [ ] Artists can only manage own availability
- [ ] Role-based access control (RBAC)
- [ ] No privilege escalation possible

### Logging & Monitoring

- [ ] Admin actions logged
- [ ] Payment transactions logged
- [ ] Authentication attempts logged
- [ ] Failed auth attempts blocked
- [ ] Error logs don't expose sensitive data
- [ ] Logs retained 30+ days
- [ ] Real-time alerts configured

### Data Privacy

- [ ] Privacy Policy published
- [ ] Terms & Conditions published
- [ ] GDPR compliance (if EU customers)
- [ ] Customer data never shared
- [ ] Right to deletion implemented
- [ ] Right to data export implemented
- [ ] Data retention policy defined
- [ ] Cookies consent banner (if applicable)

## Deployment Checklist

### Code Quality

- [ ] No console.log() with sensitive data
- [ ] No TODO comments in production code
- [ ] No commented-out code
- [ ] TypeScript strict mode enabled
- [ ] ESLint passes
- [ ] All tests pass
- [ ] Build completes without warnings
- [ ] npm audit passes (no vulnerabilities)

### Configuration

- [ ] next.config.ts optimized
- [ ] tsconfig.json configured correctly
- [ ] tailwind.config.ts includes all pages
- [ ] .env.example matches required variables
- [ ] vercel.json configured
- [ ] robots.txt created
- [ ] sitemap.xml created
- [ ] favicon configured

### Performance

- [ ] Images optimized
- [ ] Lazy loading implemented
- [ ] Code splitting optimized
- [ ] Bundle size < 500KB (gzipped)
- [ ] First Contentful Paint < 2s
- [ ] Lighthouse score > 90
- [ ] Mobile performance tested
- [ ] Database queries optimized

### SEO

- [ ] Metadata on all pages
- [ ] Open Graph tags configured
- [ ] Twitter cards configured
- [ ] Schema.org markup added
- [ ] LocalBusiness schema implemented
- [ ] Service schema implemented
- [ ] Canonical URLs set
- [ ] Mobile-friendly design
- [ ] Page titles descriptive
- [ ] Meta descriptions present

### Testing

- [ ] Manual testing on desktop
- [ ] Manual testing on mobile
- [ ] Manual testing on tablet
- [ ] Browser compatibility tested
- [ ] Form validation tested
- [ ] Booking flow tested end-to-end
- [ ] Admin flow tested end-to-end
- [ ] Payment flow tested (Stripe test mode)
- [ ] Email notifications tested
- [ ] Image upload tested

### Accessibility

- [ ] WCAG 2.1 Level AA compliance
- [ ] Keyboard navigation works
- [ ] Screen reader compatible
- [ ] Color contrast adequate
- [ ] Focus indicators visible
- [ ] Form labels associated
- [ ] Alt text on images
- [ ] ARIA labels where needed

### Documentation

- [ ] README complete and accurate
- [ ] VERCEL_DEPLOYMENT.md complete
- [ ] SECURITY_CHECKLIST.md complete
- [ ] Environment variables documented
- [ ] API endpoints documented
- [ ] Database schema documented
- [ ] Deployment procedure documented
- [ ] Admin guide created
- [ ] Troubleshooting guide included

### Monitoring Setup

- [ ] Error tracking configured (Sentry optional)
- [ ] Analytics configured (Google Analytics optional)
- [ ] Uptime monitoring enabled
- [ ] Performance monitoring enabled
- [ ] Alerts configured for errors
- [ ] Alerts configured for downtime
- [ ] Alerts configured for security issues
- [ ] Dashboard created for monitoring

### Backup & Recovery

- [ ] Database backups enabled
- [ ] Backups tested for restore
- [ ] Backup retention policy defined
- [ ] Disaster recovery plan documented
- [ ] RTO (Recovery Time Objective) defined
- [ ] RPO (Recovery Point Objective) defined
- [ ] Team trained on recovery procedure

### Domain & SSL

- [ ] Custom domain configured
- [ ] DNS records propagated
- [ ] SSL certificate active
- [ ] Certificate auto-renewal enabled
- [ ] HTTPS enforced
- [ ] Redirects from old domain configured (if applicable)

### Third-Party Services

- [ ] Stripe account configured
- [ ] Stripe webhooks tested
- [ ] Resend domain verified
- [ ] Cloudinary quotas reviewed
- [ ] GitHub repository access configured
- [ ] Vercel project settings optimized

### Admin Setup

- [ ] Initial admin account created
- [ ] Admin password changed
- [ ] Admin email verified
- [ ] Admin 2FA enabled (optional but recommended)
- [ ] Admin dashboard accessible
- [ ] Admin settings configured
- [ ] Business information updated
- [ ] Artists created
- [ ] Services created
- [ ] Availability set
- [ ] Demo data removed/archived

### Final Verification

- [ ] Homepage loads correctly
- [ ] Portfolio displays images
- [ ] Booking form accessible
- [ ] Admin login works
- [ ] Admin dashboard loads
- [ ] All navigation links work
- [ ] Contact form works
- [ ] Email notifications send
- [ ] Payment flow works (test mode)
- [ ] Mobile responsive
- [ ] No console errors
- [ ] No security warnings
- [ ] Performance acceptable
- [ ] No broken images/links

## Post-Deployment

### Week 1

- [ ] Monitor error logs daily
- [ ] Check performance metrics
- [ ] Test all booking workflows
- [ ] Verify email delivery
- [ ] Monitor database performance
- [ ] Check payment processing
- [ ] Review user feedback
- [ ] Fix any issues immediately

### Monthly

- [ ] Review security logs
- [ ] Update dependencies (npm audit)
- [ ] Backup verification
- [ ] Performance analysis
- [ ] User analytics review
- [ ] Update documentation if needed
- [ ] Test disaster recovery
- [ ] Security audit

### Quarterly

- [ ] Full security assessment
- [ ] Penetration testing (optional)
- [ ] Code review
- [ ] Performance optimization
- [ ] Scalability assessment
- [ ] Update dependencies
- [ ] Review and update policies

## Incident Response

### Security Breach

1. Immediately disable affected accounts
2. Change all passwords and secrets
3. Review logs for unauthorized access
4. Notify affected users
5. Update security measures
6. Document incident
7. Review and improve security

### Payment Issues

1. Check Stripe Dashboard
2. Review webhook logs
3. Verify database consistency
4. Contact Stripe support if needed
5. Notify customers if necessary
6. Implement fix

### Database Issues

1. Check database status
2. Review error logs
3. Restore from backup if needed
4. Verify data integrity
5. Update database provider if needed
6. Document issue and resolution

### Downtime

1. Check Vercel Dashboard
2. Review build logs
3. Check database connectivity
4. Revert recent changes if applicable
5. Check third-party services
6. Communicate with users
7. Document root cause

## Regulatory Compliance

### GDPR (EU customers)

- [ ] Privacy Policy covers data processing
- [ ] Legitimate interest basis documented
- [ ] Consent management implemented
- [ ] Data subject rights implemented
- [ ] DPA with vendors (Stripe, Cloudinary, etc.)
- [ ] Data processing audit completed

### CCPA (California customers)

- [ ] Privacy Policy updated
- [ ] Consumer rights implemented
- [ ] Opt-out mechanism available
- [ ] Data sale disclosures made

### PCI DSS (Payment Processing)

- [ ] Level 1 compliance with Stripe
- [ ] No direct card handling
- [ ] Secure data transmission
- [ ] Access controls implemented

## Continuous Improvement

- [ ] Collect user feedback
- [ ] Monitor performance metrics
- [ ] Track error rates
- [ ] Review security incidents
- [ ] Update threat model quarterly
- [ ] Stay informed about vulnerabilities
- [ ] Update dependencies regularly
- [ ] Improve documentation
- [ ] Train team on security
- [ ] Schedule security audits

---

**Security is ongoing. Review this checklist quarterly and update as needed.**
