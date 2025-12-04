#  Cleanup Guide — Static Website (S3 + CloudFront + OAC)

This document explains how to safely delete all AWS resources created for this project.  
Cleaning up properly ensures you **avoid future charges**, especially from CloudFront and S3 storage.

Follow the steps **in this exact order** to avoid dependency errors.

---

#  IMPORTANT CLEANUP ORDER

CloudFront depends on the S3 bucket policy, so deleting the bucket first will cause errors.

**Correct order:**

- Disable CloudFront distribution  
- Delete CloudFront distribution  
- Remove S3 bucket policy (OAC policy)  
- Delete objects and versions from S3  
- Delete S3 bucket  
- Verify OAC is deleted  
- Remove any IAM artifacts (if created)

---

#  Disable the CloudFront Distribution

1. Go to **CloudFront → Distributions**
2. Select your distribution
3. Click **Actions → Disable**
4. Confirm disable

Wait until **Status = Disabled**.

### Why?
CloudFront must be disabled before deletion — AWS does not allow deleting an active distribution.

---

#  Delete the CloudFront Distribution

Once disabled:

1. Select the distribution
2. Click **Actions → Delete**
3. Confirm deletion

It may take **5–15 minutes** to fully remove.

---

#  Remove the S3 Bucket Policy (OAC Policy)

1. Go to **S3 → Your Bucket → Permissions**
2. Scroll to **Bucket Policy**
3. Delete the entire policy JSON added by CloudFront OAC
4. Save changes

### Why?
This policy grants CloudFront access.  
Once CloudFront is deleted, the policy is no longer valid and should be removed.

---

#  Delete All S3 Objects (and Versions)

### If versioning was **ON**:
1. Go to the **Objects** tab
2. Click **Versions**
   - Select all versions → Delete
3. Select current objects → Delete again

### If versioning was **OFF**:
- Simply select all objects and delete

### Why?
S3 buckets cannot be deleted unless completely empty.

---

#  Delete the S3 Bucket

After all objects and versions are removed:

1. Go to S3 bucket list
2. Select your bucket
3. Click **Delete**
4. Type the bucket name to confirm

---

#  Delete the OAC (Origin Access Control)

1. Open **CloudFront**
2. Left menu → **Origin access**
3. If your OAC still exists:
   - Select it → **Delete**

### Why?
CloudFront usually deletes it automatically, but not always.  
Leaving an unused OAC is harmless but unnecessary.

---

#  Double-Check for Any Stray Resources

Review these services to ensure nothing remains:

###  S3  
- No buckets left behind

###  CloudFront  
- No distributions in “In Progress” or “Enabled” state

###  CloudWatch (optional)  
- Check if logging was enabled  
- Delete unused log groups

###  Billing Check  
Go to **Billing → Cost Explorer**:
- Ensure CloudFront and S3 show *zero* daily usage after cleanup

---

#  Cleanup Completed Successfully

After completing this guide:

- No CloudFront charges  
- No S3 storage charges  
- No lingering policies  
- No unused IAM resources  

Your AWS account is now clean and cost-free.

---
