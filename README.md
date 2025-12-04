Secure Static Website on AWS (S3 + CloudFront + OAC)

This project demonstrates how to host a fully secure, globally cached static website using **Amazon S3** and **Amazon CloudFront** with **Origin Access Control (OAC)** — all without needing a custom domain.

This is a production-grade, zero-coding AWS architecture built with industry best practices.

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

