# EC2 Restart and Development Guide

This document explains how to restart your stopped EC2 instance and resume development, including connecting via SSH, starting Jupyter Notebook, and accessing S3.

---

## ✅ Step 1: Start Your EC2 Instance

1. Go to: [https://console.aws.amazon.com/ec2](https://console.aws.amazon.com/ec2)
2. On the left panel, click **"Instances"**
3. Select your stopped instance
4. Click **"Instance state → Start instance"**
5. After it starts, note the new **Public IPv4 address** (e.g., `3.88.xxx.xxx`)

---

## ✅ Step 2: Connect to EC2 via Terminal or VSCode

In your local terminal, run:

```bash
ssh -i ~/Downloads/owenkey.pem ec2-user@<your-public-ip>
```

Example:

```bash
ssh -i ~/Downloads/owenkey.pem ec2-user@3.88.125.64
```

---

## ✅ Step 3: Restart Jupyter Notebook

Once inside the EC2 terminal, run:

```bash
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser
```

Then, open your browser and go to:

```
http://<your-public-ip>:8888
```

---

## ✅ Step 4: Verify boto3 + S3 Access

In the terminal or a Jupyter cell:

```python
import boto3

s3 = boto3.client('s3')
buckets = s3.list_buckets()

for bucket in buckets['Buckets']:
    print("S3 Bucket:", bucket['Name'])
```

---

## ✅ Quick Reference Table

| Task             | Command or Location                                      |
| ---------------- | -------------------------------------------------------- |
| Connect to EC2   | `ssh -i ~/Downloads/owenkey.pem ec2-user@<IP>`           |
| Start Jupyter    | `jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser` |
| View Public IP   | EC2 Console → Instance → IPv4 Address                    |
| Download from S3 | `boto3.download_file()`                                  |
| Upload to S3     | `boto3.upload_file()`                                    |

---

## 📅 Reminder

Stopping your EC2 instance pauses billing. Next time you restart it, the public IP will change unless you use an Elastic IP.

---

Need help with model deployment, API hosting, or auto-saving models to S3? Let me know!
