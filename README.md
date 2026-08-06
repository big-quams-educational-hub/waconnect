# WhatsApp Contact Bundle Platform

A PHP + MySQL app for business owners to collect WhatsApp contacts via a shareable link/QR code, then export them as VCF, CSV, or PDF. Built as a clone of the GainMoreViews workflow, rebrandable to your own name.

## What's included
- Business owner registration/login (`register.php`, `login.php`)
- Dashboard listing "VCF Bundles" (`dashboard.php`)
- Create a bundle with release period, max members, suffix, optional WhatsApp link (`create_bundle.php`)
- Public contact form shared via a unique link `form.php?ref=CODE` (`form.php`)
- Add/view/edit/delete individual contacts (`add_contact.php`, `view_contacts.php`, `edit_contact.php`, `delete_contact.php`)
- Copy link + QR code generation (via qrserver.com, no API key needed)
- Export: VCF, CSV, and a self-contained PDF (no external library required)
- Combine multiple bundles into one merged VCF (`combine.php`)
- Edit/delete a bundle (`edit_bundle.php`, `delete_bundle.php`)

## Setup (any standard PHP/MySQL host — cPanel, etc.)
1. Create a MySQL database, then import `schema.sql` (phpMyAdmin: Import tab, or `mysql -u user -p dbname < schema.sql`).
2. Open `config.php` and fill in:
   - `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASS`
   - `BRAND_NAME` — your platform's name, shown in the header everywhere
   - `BASE_URL` — your live domain, e.g. `https://yourdomain.com` (used to build shareable links)
3. Upload all files to your host (e.g. `public_html/`).
4. Requires PHP 7.4+ with the `pdo_mysql` extension (standard on almost all hosts).
5. Visit your domain — you'll land on the login/register page.

## How the flow works
1. A business owner registers and logs in.
2. They create a "VCF Bundle" (a named contact-collection campaign) with a release period (days the link stays live) and optional max members cap.
3. The app generates a unique link: `yourdomain.com/form.php?ref=XXXXXXXX` — shareable via WhatsApp, social media, QR code.
4. Anyone who fills the form gets added to that bundle's contact list.
5. The owner can view/edit contacts, and export the whole list as VCF (importable to a phone), CSV (for spreadsheets), or PDF.
6. Multiple bundles can be combined into a single VCF export.

## Notes / things to decide before going live
- **Password reset** isn't wired up (`forgot_password.php` is a placeholder) — needs an SMTP service (e.g. PHPMailer + a transactional email provider) if you want real reset emails.
- **Rate limiting / spam protection** on the public form isn't included — consider adding a simple CAPTCHA (e.g. Google reCAPTCHA) if you expect abuse.
- **Phone number validation** is minimal (digits only, country code stripped of leading zero). Tighten this if you need strict E.164 formatting.
- **HTTPS** is assumed for `BASE_URL` — get an SSL cert (most hosts offer free Let's Encrypt) before launch, especially since real contact data is collected.
- The PDF export is hand-built without a library to avoid dependencies — fine for a simple list; if you want fancier formatting/branding later, swapping in a library like Dompdf is a clean upgrade path.
