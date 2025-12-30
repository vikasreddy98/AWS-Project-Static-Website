Secure Static Website on AWS (S3 + CloudFront + OAC)

This project demonstrates how to host a fully secure, globally cached static website using **Amazon S3** and **Amazon CloudFront** with **Origin Access Control (OAC)** — all without needing a custom domain.


---

Project Overview

- Website Content Stored in a Private S3 Bucket  
- Served Securely via CloudFront CDN  
- CloudFront’s Default HTTPS Certificate Used  
- S3 Bucket Locked Down Using OAC  
- Direct S3 Access Fully Blocked  
- Website Accessible via CloudFront HTTPS URL

---

Architecture Diagram

Below is the high-level architecture representing the request flow:

![Architecture Diagram](Architecture.png)

---

AWS Services Used

- **Amazon S3** — private static storage  
- **Amazon CloudFront** — global CDN & HTTPS  
- **Origin Access Control (OAC)** — secure S3 access  
- **IAM** — bucket policies  
- *(No custom domain, Route 53, or ACM required)*

---

##  Project Structure

```
AWS-Project-Static-Website/
├── docs/
│   ├── cleanup.md
│   └── howto.md
│
├── images/
│   ├── phase1/
│   │   ├── s3_create_bucket.png
│   │   ├── s3_empty_bucket.png
│   │   ├── s3_permissions_initial.png
│   │   ├── s3_static_hosting_disabled.png
│   │   └── s3_upload_files.png
│   │
│   └── phase2/
│       ├── cloudfront_default_root.png
│       ├── cloudfront_deployed.png
│       ├── cloudfront_settings.png
│       ├── cloudfront_site_working.png
│       ├── s3_access_denied.png
│       └── s3_bucket_policy_oac.png
│
├── site/
│   └── index.html
│
├── Architecture.png
└── README.md
```

---

##  Deployment Steps

The detailed deployment instructions are in:

 ![howto.md](docs/howto.md)
 
(Contains Phase 1 + Phase 2 with screenshots and full explanations)

---

##  Security Highlights

- S3 bucket is **fully private** (not publicly accessible)
- CloudFront uses OAC to fetch content securely  
- Direct S3 URLs return **AccessDenied**  
- HTTPS enforced end-to-end automatically

---

##  Validation

To verify the deployment:

- Access the CloudFront URL:  
  `https://d********.cloudfront.net`  
- Confirm the **HTTPS lock icon**  
- Attempt to access S3 directly → should return **AccessDenied**

---

##  Cleanup

Cleanup instructions are available in:

![cleanup.md](docs/cleanup.md)

---

##  Lessons Learned

- Modern static hosting best practices on AWS  
- CloudFront OAC vs. old S3 “static website hosting”  
- Private S3 design patterns  
- CDN caching & invalidation behavior  
- End-to-end secure delivery with minimal services

---

##  Contact

Your Name  
- LinkedIn: *www.linkedin.com/in/raghu-vikas-reddy-yadavalli-3b23a41ab*  
- Email: *reddy.vikas8987@gmail.com*

