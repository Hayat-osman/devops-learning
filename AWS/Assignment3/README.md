# Assignment 3: S3 Static Website + CloudFront CDN + Route53 

### **Objective**

This document outlines the deployment of a secure, scalable, and highly available static website using AWS infrastructure. The architecture leverages S3, CloudFront, and Route 53 to create a production-grade hosting solution.

### **Architecture diagram**

![Architecture[] Diagram](/AWS/Assignment3/Architecturediagram-3.png)


### **1. S3 Bucket Configuration**

An S3 bucket was configured to store and serve the website's source files.

**Static Website Hosting:** Enabled on the bucket, with `index.html` as the index document and `error.html` as the error document.

![Static Hosting Enabled](/AWS/Assignment3/Screenshots/staticwebhost.png)

**File Upload:** Source files (`index.html`, `error.html`) were uploaded to the bucket root.

![Uploaded Objects](/AWS/Assignment3/Screenshots/html.png)

**Permissions:** A public-read `GetObject` bucket policy was attached to allow access from the CloudFront distribution.

![Bucket Policy](/AWS/Assignment3/Screenshots/S3bucketpolicy.png)

### **2. CloudFront Distribution**

A CloudFront distribution was deployed to serve content globally and enforce security.

**Origin:** The S3 bucket's static website endpoint was set as the origin.
**Configuration:**
Alternate Domain Name was configured.
An SSL certificate from AWS Certificate Manager (ACM) was attached.
Viewer policy was set to redirect all HTTP traffic to HTTPS.

![CloudFront Configuration](/AWS/Assignment3/Screenshots/cloudfront.png)

### **3. Route 53 DNS Configuration**

DNS records were created to point the custom domain to the CloudFront distribution.

**Hosted Zone:** Managed the DNS for `my-static-site.com`.
**DNS Records:** 'A' Alias records were created for both the root domain and the `www` subdomain, routing traffic to the CloudFront distribution.
    ![Route 53 A Record](/AWS/Assignment3/Screenshots/Route53-1.png)
![Route 53 Alias Record](/AWS/Assignment3/Screenshots/Route53.png)

---

### **4. Validation**

The deployment was verified through the following tests:

 **Domain Resolution & HTTPS:** The custom domain successfully resolved to the website. The connection was secured with HTTPS, confirming the ACM certificate and CloudFront configuration are active.

![HTTPS Enabled](/AWS/Assignment3/Screenshots/mainpage1.png)

**Custom Error Page:** Navigating to a non-existent path correctly served the `error.html` page.

![Custom Error Page](/AWS/Assignment3/Screenshots/errorpage.png)

**Cache Invalidation:** An update to `index.html` in S3 was successfully deployed by creating a CloudFront invalidation (`/*`), with changes reflected immediately upon browser refresh.
 ![Cache Invalidation](/AWS/Assignment3/Screenshots/Test.png)