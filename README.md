# S3 Security Defense in Depth

A small hands-on showcase of the security controls covered in the **S3 Security Challenge - "Secure the Bucket"**.

The challenge starts with a deliberately vulnerable S3 bucket and progressively applies multiple layers of protection.

## What this repo demonstrates

1. **Block Public Access**
   - Prevent direct public access to sensitive S3 objects.
   - Validation: public object URL should return `403 Forbidden`.

2. **Least-Privilege Bucket Policies**
   - Remove permissive access such as `"Principal": "*"`.
   - Restrict access to the approved application role.
   - Validation: direct CloudShell access should return `AccessDenied`.

3. **IAM Role-Based Lambda Access**
   - Allow Lambda to access S3 using its execution role.
   - Limit permissions to:
     - `s3:GetObject`
     - `s3:ListBucket`
   - Scope access to one specific bucket.

4. **HTTPS Enforcement**
   - Deny requests when:
     - `aws:SecureTransport = false`
   - This blocks non-HTTPS access to the bucket.

5. **Encryption at Rest**
   - Use S3 default encryption.
   - The challenge moves from default SSE-S3 to **SSE-KMS**.
   - S3 Bucket Keys can reduce calls to AWS KMS and lower KMS-related cost.

## Defense in Depth

```text
Internet / User
      |
      v
Block Public Access
      |
      v
Bucket Policy
      |
      v
IAM Role
      |
      v
HTTPS Enforcement
      |
      v
SSE-KMS Encryption
      |
      v
     S3
```

The key lesson is that S3 security should not depend on one control.

**Bucket Policy = WHO can access**

**IAM Role = WHAT the workload can do**

Both must allow access for the application to work.

## Repository Structure

```text
s3-security-defense-in-depth/
├── README.md
├── policies/
│   ├── lambda-role-access.json
│   └── deny-insecure-transport.json
└── docs/
    ├── production-checklist.md
    └── key-takeaways.md
```

## Policy Examples

### Restrict access to a Lambda IAM role

See:

`policies/lambda-role-access.json`

This example demonstrates the allow statement used for the approved Lambda execution role.

### Enforce HTTPS

See:

`policies/deny-insecure-transport.json`

The important condition is:

```json
"Condition": {
  "Bool": {
    "aws:SecureTransport": "false"
  }
}
```

Requests using insecure transport are explicitly denied.

## Validation Flow

```text
Before controls:
Public URL -> Sensitive object visible

After Block Public Access:
Public URL -> 403 Forbidden

After restrictive bucket policy:
CloudShell -> AccessDenied

With approved Lambda IAM role:
Lambda -> S3 access succeeds
```

## Additional Production Considerations

The challenge also highlights:

- S3 Object Lock
- S3 Versioning
- Lifecycle Policies
- VPC Endpoints for S3
- CloudWatch monitoring
- CloudTrail auditing
- Avoiding wildcard permissions in production
- Monitoring access patterns for anomalies

## Versioning Reminder

Once S3 Versioning has been enabled, it cannot be fully disabled.

It can be **suspended**, but existing object versions remain until they are deleted manually or by lifecycle rules.
