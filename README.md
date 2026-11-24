# SWYNEX Technologies — Task 4: Final Cloud Deployment Case Study

- **Intern Name:** Yogiraj Solanki
- **Intern ID:** SWX-APP-2026-C3794
- **Offer ID:** SWX-OFR-2026-5DC45
- **Domain:** Cloud & Emerging Technology
- **Cloud Provider:** Amazon Web Services (AWS) — `ap-southeast-2`
- **Live Production URL:** http://yogiraj-swynex-portfolio-3157.s3-website-ap-southeast-2.amazonaws.com
- **Case Study & Code Repository:** https://github.com/yogirajsolanki3157-byte/SWYNEX-Final-Cloud-Deployment

---

## 1. Executive Summary
This case study documents the end-to-end cloud architecture, production deployment, operational monitoring, and cost-governance lifecycle of my **Cloud Computing Portfolio** (`HTML5`, `CSS3`, `JavaScript`) hosted on **Amazon Web Services (AWS)** during my 30-day internship at **SWYNEX Technologies**.

---

## 2. Cloud Architecture Overview (Compute, Storage, Networking & Identity)
- **Compute / Hosting Model:** Serverless static web hosting via **Amazon S3 Static Website Hosting**, eliminating idle VM/EC2 compute overhead.
- **Storage Layer:**
  - **Primary Hosting Bucket:** `yogiraj-swynex-portfolio-3157` (`ap-southeast-2`) storing `index.html`, `style.css`, and `script.js`.
  - **Audit & Access Logs Bucket:** `yogiraj-swynex-logs-3157` (`ap-southeast-2`) storing S3 server access logs.
- **Networking & Delivery:** Public HTTP routing via the AWS S3 regional website endpoint (`http://yogiraj-swynex-portfolio-3157.s3-website-ap-southeast-2.amazonaws.com`) with `index.html` configured as the root entry document.
- **Identity & Access Management (IAM / Bucket Policies):**
  - Enforced least-privilege access by granting public read (`s3:GetObject`) strictly to `arn:aws:s3:::yogiraj-swynex-portfolio-3157/*` while blocking public writes, deletes, and directory listings.
  - Enforced strict bucket isolation (`Block all public access: ON`) on the `yogiraj-swynex-logs-3157` log bucket.

---

## 3. Deployment Walkthrough

### Phase A: S3 Bucket Provisioning & Static Website Hosting
1. Created `yogiraj-swynex-portfolio-3157` in `ap-southeast-2` and uploaded `index.html`, `style.css`, and `script.js`.
2. Enabled **Static website hosting** under bucket Properties and configured `index.html` as the index document.

![S3 Static Website Hosting](screenshots/01-s3-static-hosting.png)

### Phase B: Public Access & Security Policy Configuration
1. Disabled bucket-level *Block all public access* on the hosting bucket.
2. Attached a scoped JSON **Bucket Policy** permitting only `s3:GetObject` requests:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadForWebSiteFiles",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::yogiraj-swynex-portfolio-3157/*"
        }
    ]
}
