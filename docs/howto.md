# How to Deploy a Secure Static Website Using S3 + CloudFront (No Domain Version)

This guide documents the exact steps used to deploy a secure, highly available, and globally cached static website using **Amazon S3**, **Amazon CloudFront**, and **Origin Access Control (OAC)**.

No domain is required. The website will be served securely over HTTPS using CloudFront’s default TLS certificate.

---

PHASE 1 — S3 BUCKET SETUP (Website Origin)

## Step 1 — Create the S3 Bucket
1. Go to AWS Console → **S3**
2. Click **Create bucket**
3. Configure the following:
   - **Bucket name:** `static-website` (or any unique name)
   - **AWS Region:** your preferred region (e.g., `ap-south-1`)
   - **Block Public Access:** **UNCHECK** "Block all public access"
   - **Versioning:** Enable (optional, but recommended)

![s3_create_bucket](https://github.com/vikasreddy98/AWS-Project-Static-Website/blob/be103565b504559be9901c3e8d923093e114b36c/images/phase1/s3_create_bucket.png)

### Why this step?
CloudFront needs to read objects from your bucket. The bucket remains private, but public access must be unblocked so CloudFront’s OAC can attach the correct policy later.

---

## Step 2 — Open Your New Bucket
1. Click the bucket name.
2. Confirm it is empty.

![](https://github.com/vikasreddy98/AWS-Project-Static-Website/blob/995f53174720b5d1f8535f8417507761cc3c9605/images/phase1/s3_empty_bucket.png)

---

## Step 3 — Upload Your Website Files
1. Inside the bucket → Click **Upload**
2. Add:
   - `index.html`
   - Any assets (CSS, images, JS)
3. Click **Upload**

![](https://github.com/vikasreddy98/AWS-Project-Static-Website/blob/94244dde6ddb98fb0b55b9e1fdfc7e14e4f37328/images/phase1/s3_upload_files.png)

### Why this step?
These uploaded files become the static content CloudFront will serve globally.

---

## Step 4 — Ensure Bucket Remains Private
Go to **Permissions** tab and confirm:
- Block Public Access → Off  
- Bucket Policy → *Empty*  
- ACL → Do not modify anything

![](https://github.com/vikasreddy98/AWS-Project-Static-Website/blob/94244dde6ddb98fb0b55b9e1fdfc7e14e4f37328/images/phase1/s3_permissions_initial.png)

### Why this step?
We want CloudFront to be the ONLY way to access the content.  
This makes the architecture secure and production-ready.

---

## Step 5 — Static Website Hosting MUST Stay Disabled
Go to **Properties** → **Static Website Hosting**  
Ensure it is **Disabled**.

![](https://github.com/vikasreddy98/AWS-Project-Static-Website/blob/94244dde6ddb98fb0b55b9e1fdfc7e14e4f37328/images/phase1/s3_static_hosting_disabled.png)

### Why this step?
Unlike older tutorials, modern best practices use CloudFront + OAC directly.  
We do **not** expose the S3 bucket as a public website.

---

# PHASE 1 COMPLETE  
Your S3 bucket now securely stores your site content and is ready to be connected to CloudFront.

---

#  PHASE 2 — CLOUDFRONT DISTRIBUTION + OAC SETUP

## Step 6 — Create a New CloudFront Distribution
1. Go to AWS Console → **CloudFront**
2. Click **Create distribution**

---

## Step 7 — Configure Origin Settings
- **Origin domain:** Select your S3 bucket
- **Origin access:**  
  Choose → **Origin access control settings (recommended)**
- Click **Create new OAC**
  - Name: `project-static-site`
  - Save

![](https://github.com/vikasreddy98/AWS-Project-Static-Website/blob/8a6e68d4bae93c29886cac4dc98a9379110d09cd/images/phase2/cloudfront_settings.png)

### Why this step?
OAC allows CloudFront to securely access your private S3 bucket.  
This enforces “CloudFront-only” access to your content.

---

## Step 8 — Viewer Settings (Security)
- **Viewer protocol policy:**  
  ✔️ Redirect HTTP → HTTPS  
- **Allowed HTTP methods:**  
  ✔️ GET, HEAD

### Why this step?
Only static GET requests are needed, reducing attack surface.  
All traffic is forced over HTTPS.

---

## Step 9 — Cache Settings
- Cache policy: **CachingOptimized (recommended)**
- Origin request policy: Default
- Enable: ✔️ **Compress objects automatically**

### Why this step?
Optimized caching + compression =  
Faster loads + lower CloudFront cost.

---

## Step 10 — Default Root Object
Under **Distribution settings**:

- **Default root object:** `index.html`

![](images/phase2/cloudfront_default_root.png)

### Why this step?
Allows users to access the site without typing `/index.html`.

---

## Step 11 — Price Class
Choose:
- Price Class 100 (cheapest)  
or  
- Price Class 200 (includes India, recommended for you)

---

## Step 12 — Create Distribution
Click **Create distribution**.

📸 **Screenshot Placeholder:**  
`images/cloudfront-deploying.png`

Your distribution will take 5–10 minutes to deploy.

---

# 📌 PHASE 2.5 — APPLY OAC BUCKET POLICY

## Step 13 — Copy the Recommended Bucket Policy
1. CloudFront → Your Distribution → **Origins**
2. You will see a banner: “Update S3 bucket policy”
3. Click **Copy policy**

---

## Step 14 — Paste Policy Into S3 Bucket
1. Go to S3 → Bucket → **Permissions** → **Bucket policy**
2. Paste the copied JSON
3. Save

📸 **Screenshot Placeholder:**  
`images/s3-bucket-policy-oac.png`

### Why this step?
This makes S3 **private to the world**, but **readable by CloudFront**.

It is the core of secure architecture.

---

# 📌 PHASE 2.6 — VALIDATION TESTS

## Step 15 — Test CloudFront URL
Copy your CloudFront domain:

