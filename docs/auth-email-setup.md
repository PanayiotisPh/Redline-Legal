# Auth email setup (custom SMTP)

Password reset in the app uses a **6-digit code**, not a link — see
`requestPasswordReset` / `confirmPasswordReset` in `lib/auth.ts` and the
`forgot` / `reset` modes in `components/AuthScreen.tsx`.

That requires editing Supabase's email template to emit `{{ .Token }}`, and
**Supabase only allows template editing once custom SMTP is configured.** On
the default email service you get Supabase's built-in template, which sends a
`ConfirmationURL` link instead of a code, so the app's code field has nothing
to accept.

There are two other reasons to do this regardless of the reset flow:

- **The default email service is rate limited to a handful of messages per
  hour** across the whole project. That is fine for solo testing and will
  block real users the moment more than a couple sign up at once.
- Default emails come from Supabase's domain, not yours, and land in spam
  more often.

## 1. Pick an SMTP provider

Any SMTP provider works. Free tiers that are enough to start:

| Provider | Free tier | Notes |
| --- | --- | --- |
| [Resend](https://resend.com) | 3,000/month, 100/day | Simplest setup |
| [Brevo](https://brevo.com) | 300/day | No custom domain required to start |
| [Mailgun](https://mailgun.com) | Trial, then paid | |
| [SendGrid](https://sendgrid.com) | 100/day | |

Sending from your own domain needs DNS records (SPF/DKIM) that the provider
supplies. Deliverability is much better with them than without.

### Resend without a domain — testing only

Resend's shared `onboarding@resend.dev` sender needs no DNS setup, but it
**only delivers to the address the Resend account was registered with.**
Sending to anyone else fails.

That is enough to test the reset flow end to end with your own account, and
it is a hard blocker for real users: nobody else will be able to reset their
password until a domain is verified. Before launch, add a domain in Resend →
**Domains**, publish the SPF/DKIM records it gives you, and change Supabase's
Sender email to `noreply@yourdomain.com`. No other setting or app code
changes.

## 2. Configure Supabase

**Authentication → Emails → SMTP Settings → Enable custom SMTP**, then fill in
the host, port, username and password from your provider. Typical values:

```
Host:        smtp.resend.com
Port:        465        (or 587 for STARTTLS)
Username:    resend     (provider-specific)
Password:    <API key from the provider>
Sender email: noreply@yourdomain.com
Sender name:  Tripper
```

Send a test email from the provider's dashboard first — a wrong API key here
surfaces as "email never arrives" with no error in the app.

## 3. Edit the reset template

Templates unlock once SMTP is on. Go to **Authentication → Emails →
Templates → Reset Password** and replace the body:

```html
<h2>Reset your password</h2>

<p>Enter this code in the Tripper app to choose a new password:</p>

<p style="font-size:28px;font-weight:bold;letter-spacing:4px">{{ .Token }}</p>

<p>This code expires in 1 hour. If you didn't request it, ignore this email.</p>
```

`{{ .Token }}` is the 6-digit OTP the app expects. **Remove the
`{{ .ConfirmationURL }}` link** — leaving it in means users click it instead
of reading the code, and it takes them to the Site URL rather than the app.

## 4. Fix the Site URL

**Authentication → URL Configuration → Site URL** defaults to
`http://localhost:3000`, which is why the original reset email pointed there.
It does not affect the OTP flow, but it does affect the **signup confirmation**
email, which still uses a link. Set it to a real URL you control.

## Verifying it works

1. In the app: Sign in → *Forgot your password?* → enter your email → *Send code*
2. The email should contain a plain 6-digit number and no link
3. Enter the code and a new password → you should land signed in

If the email still contains a link, the template did not save — templates
silently revert if SMTP validation fails, so re-check the SMTP test first.

## Note on the app code

No app changes are needed for any of this. `confirmPasswordReset` calls
`verifyOtp({ type: 'recovery' })`, which expects exactly the value
`{{ .Token }}` emits. This is purely Supabase-side configuration.
