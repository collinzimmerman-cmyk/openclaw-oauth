# openclaw-oauth

Static pages used as the homepage and privacy policy URLs for the **Openclaw**
OAuth consent screen (Google Cloud Console).

- `index.html` — application home page
- `privacy.html` — privacy policy

Live at: **https://rocketfood.org/**

## Deploy

```bash
git init
git add .
git commit -m "Openclaw OAuth site"
git branch -M main
git remote add origin git@github.com:collinzimmerman-cmyk/openclaw-oauth.git
git push -u origin main
```

Then: repo **Settings → Pages → Source: Deploy from a branch → main / root**,
and **Custom domain** → `rocketfood.org` → Save. Tick **Enforce HTTPS** once the
certificate is provisioned.

The `CNAME` file in this repo pins the custom domain.

## DNS records (Hostinger → Domains → rocketfood.org → DNS records)

Delete the two parked records first:
- `CNAME www → rocketfood.org`
- `A @ → 2.57.91.91`

Then add exactly these five:

| Type | Name | Value | TTL |
|---|---|---|---|
| A | @ | 185.199.108.153 | 14400 |
| A | @ | 185.199.109.153 | 14400 |
| A | @ | 185.199.110.153 | 14400 |
| A | @ | 185.199.111.153 | 14400 |
| CNAME | www | collinzimmerman-cmyk.github.io | 14400 |

No AAAA records needed. Do not touch MX records (email) or DNSSEC.
Ignore the "Quick setup" presets — they are for Hostinger hosting/email products.

## Google Cloud Console → OAuth consent screen → Branding

| Field | Value |
|---|---|
| Application home page | `https://rocketfood.org/` |
| Application privacy policy link | `https://rocketfood.org/privacy.html` |
| Authorized domains | `rocketfood.org` |

Leave **Application terms of service link** blank and **do not upload a logo**
(uploading one triggers a Google verification requirement).

## Then

Publish the app on the **Audience** page (Testing → In production), which removes
the 7-day refresh-token expiry, then re-auth:

```bash
export GOG_KEYRING_PASSWORD="$(cat /data/.openclaw/credentials/gog-keyring-password.txt)"
gog auth add collinzimmerman@gmail.com --force-consent
```
