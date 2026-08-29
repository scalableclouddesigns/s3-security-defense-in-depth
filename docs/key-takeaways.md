# Key Takeaways

## 1. Private does not automatically mean secure

Blocking public access removes anonymous internet exposure, but authenticated access must still be controlled.

## 2. Avoid broad principals

A bucket policy using:

```json
"Principal": "*"
```

can grant access far more broadly than intended.

## 3. Bucket policy and IAM role work together

```text
Bucket Policy -> WHO can access
IAM Role      -> WHAT the workload can do
```

Both must allow the request.

## 4. Use least privilege

The challenge limits the Lambda role to:

```text
s3:GetObject
s3:ListBucket
```

and scopes it to one specific bucket.

## 5. Encrypt data in transit

Use an explicit deny when:

```text
aws:SecureTransport = false
```

to block HTTP access.

## 6. Encrypt data at rest

S3 uses server-side encryption by default. The challenge then moves to SSE-KMS and highlights S3 Bucket Keys as a way to reduce KMS calls.

## 7. Add supporting controls

For production workloads, also consider:

- Object Lock
- Versioning
- Lifecycle Policies
- VPC Endpoints
- CloudWatch
- CloudTrail
