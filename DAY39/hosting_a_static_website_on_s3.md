# AWS S3 Static Website Hosting Lab

This project demonstrates how to host a **static website using Amazon S3**. Static websites contain only client-side files such as HTML, CSS, JavaScript, and images, which can be directly served from an S3 bucket without requiring a web server.

This lab was completed as part of a cloud engineering challenge on **KodeKloud** to practice core AWS storage and web hosting concepts.

---

## Technologies Used

* Amazon S3
* AWS CLI
* HTML

---

## Project Architecture

User Browser
↓
Internet
↓
Amazon S3 Static Website Endpoint
↓
HTML Files Stored in S3 Bucket

Instead of running a web server such as Apache or Nginx on EC2, the website files are directly served by Amazon S3.

---

## Implementation Steps

### 1. Create an S3 Bucket

A bucket was created to store the website files.

Example bucket name:

xfusion-web-3911

---

### 2. Upload Website Files Using AWS CLI

The main webpage file `index.html` was uploaded to the bucket using the AWS CLI:

```bash
aws s3 cp index.html s3://xfusion-web-3911/
```

This command copies the local HTML file into the S3 bucket.

---

### 3. Enable Static Website Hosting

Inside the bucket settings:

Properties → Static Website Hosting → Enable

Configuration:

Index document: index.html

This allows the bucket to serve web content through an HTTP endpoint.

---

### 4. Configure Public Access

Since S3 buckets are private by default, public read access must be enabled so that users can access the website through a browser.

A bucket policy was added to allow public read access to objects in the bucket:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::xfusion-web-3911/*"
    }
  ]
}
```

---

## Prerequisites for the Website URL to Work

For the S3 website endpoint to successfully load the website, the following conditions must be met:

1. Static website hosting must be enabled in the bucket properties.
2. The index document must be configured as `index.html`.
3. Public access blocking must be disabled for the bucket.
4. A bucket policy must allow `s3:GetObject` for all users.
5. The `index.html` file must exist in the root of the bucket.

If any of these conditions are missing, the browser will return a **403 Access Denied** error.

---

## Example Website Endpoint

Once configured correctly, the website can be accessed using the S3 website endpoint:

http://xfusion-web-3911.s3-website-us-east-1.amazonaws.com

---

## Key Concepts Learned

* Hosting static websites using Amazon S3
* Managing S3 bucket policies
* Using the AWS CLI to upload files
* Understanding public vs private object access
* Troubleshooting S3 403 Access Denied errors

---

