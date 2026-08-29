# S3 Production Security Checklist

- [ ] Enable Block Public Access
- [ ] Remove unnecessary wildcard permissions
- [ ] Apply least privilege
- [ ] Use IAM roles for application access
- [ ] Restrict access to the required bucket
- [ ] Enforce HTTPS with `aws:SecureTransport`
- [ ] Use server-side encryption
- [ ] Consider SSE-KMS for stronger key-management control
- [ ] Consider S3 Bucket Keys to reduce KMS calls
- [ ] Enable S3 Versioning where recovery is required
- [ ] Consider S3 Object Lock where immutability is required
- [ ] Use lifecycle rules for older object versions
- [ ] Consider VPC Endpoints for private S3 access
- [ ] Use CloudWatch for monitoring
- [ ] Use CloudTrail for auditing
- [ ] Monitor access patterns for anomalies

## Important Versioning Note

After S3 Versioning has been enabled, it can be suspended but not returned to a never-enabled state.

Suspending versioning means:

- New uploads no longer create additional versions.
- Existing versions remain until manually deleted or removed through lifecycle rules.
