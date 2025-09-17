# wisamfolio.com -- Cloud-Hosted Resume Website
Live : **https://wisamfolio.com**

---
## Architecture
- **Amazon S3**
- Stored built React resume site files
- Kept private, accessed only through CloudFront(OAC)
- **Amazon CloudFront**
  - Global CDN for fast delivery
  - Configured with Origin Access Control (OAC)
  - Default root object: `index.html`
- **AWS Certificate Manager (ACM)**
  - SSL/TLS certificates for HTTPS
  - DNS-based validation
- **Amazon Route 53**
  - Manages DNS for `wisamfolio.com`
  - `A (Alias)` record points root domain to CloudFront
 ---
## ⚙️ Tech Stack
- **Frontend**: React (static build with `npm run build`)
- **AWS Services**: S3, CloudFront, ACM, Route 53
- **Tools**: AWS CLI for deployment and cache invalidation
## 🚀 Deployment Steps
- **Build**
  - `npm install`
  - `npm run build`
- **Upload to S3**
  - `aws s3 sync ./build s3://YOUR_BUCKET --delete`
- **Optimize Caching**
  - Long cache for assets (`max-age=31536000`)
  - Short cache for `index.html` (`max-age=60`)
- **Invalidate CloudFront**
  - `aws cloudfront create-invalidation --distribution-id YOUR_DIST_ID --paths "/*"`

---

## 🔒 DNS & HTTPS Setup
- Requested certificate in **ACM (us-east-1)** for `wisamfolio.com`
- Added DNS CNAME validation in Route 53
- Attached certificate to CloudFront
- Created Route 53 **A (Alias)** record → CloudFront

---

## 🛠 Troubleshooting
- **AccessDenied (403)** → OAC not attached or bucket policy not updated
- **ERR_SSL_PROTOCOL_ERROR** → ACM certificate not attached or still deploying
- **404 on page refresh (React app)** → add CloudFront custom error responses:
  - 403 → `/index.html` → 200
  - 404 → `/index.html` → 200
- **Stale content** → run CloudFront invalidation (`/index.html` usually enough)



