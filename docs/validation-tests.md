# Validation Tests

These checks mirror the learning flow from the challenge.

## 1. Public object access

Before fixing the bucket, a copied object URL could expose sensitive data.

After enabling **Block all public access**, test the same URL again.

Expected result:

```text
403 Forbidden
```

## 2. CloudShell access

Test direct access with:

```bash
aws s3 cp s3://YOUR_BUCKET_NAME/sensitive-data/user-passwords.json -
```

Before the restrictive bucket policy, the object can still be returned to an authenticated account user.

After restricting the bucket to the approved Lambda path:

```text
AccessDenied
```

## 3. Lambda access

Run the demo Lambda function.

Expected result:

```text
Successfully accessed S3 bucket using IAM role!
```

The function should be able to list files in `sensitive-data/` and read the sample object because its IAM role has the required S3 permissions.

## What this proves

```text
CloudShell -> Denied
Lambda IAM Role -> Allowed
```

This demonstrates defense in depth using both the bucket policy and IAM role.
