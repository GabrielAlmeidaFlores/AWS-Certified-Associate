## Policy Interpretation

### Example 1

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectAcl",
        "s3:GetObject",
        "s3:GetObjectAcl",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::holidaygifts/*"
    },
    {
      "Effect": "Deny",
      "Action": ["s3:GetObject", "s3:GetObjectAcl"],
      "Resource": "arn:aws:s3:::holidaygifts/*",
      "Condition": {
        "DateGreaterThan": { "aws:CurrentTime": "2022-12-01T00:00:00Z" },
        "DateLessThan": { "aws:CurrentTime": "2022-12-25T06:00:00Z" }
      }
    }
  ]
}
```

#### First Statement: Allow Access

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:PutObject",
    "s3:PutObjectAcl",
    "s3:GetObject",
    "s3:GetObjectAcl",
    "s3:DeleteObject"
  ],
  "Resource": "arn:aws:s3:::holidaygifts/*"
}
```

- Effect: `Allow`
- Actions: The user is allowed to:
  - Upload objects (`s3:PutObject`),
  - Modify the permissions (ACL) of objects (`s3:PutObjectAcl`),
  - Read objects (`s3:GetObject`),
  - Read ACLs (`s3:GetObjectAcl`), and
  - Delete objects (`s3:DeleteObject`).
- Resource: The policy applies to all objects within the holidaygifts bucket (`arn:aws:s3:::holidaygifts/*`).

#### Second Statement: Deny Access with a Condition

```json
{
  "Effect": "Deny",
  "Action": ["s3:GetObject", "s3:GetObjectAcl"],
  "Resource": "arn:aws:s3:::holidaygifts/*",
  "Condition": {
    "DateGreaterThan": { "aws:CurrentTime": "2022-12-01T00:00:00Z" },
    "DateLessThan": { "aws:CurrentTime": "2022-12-25T06:00:00Z" }
  }
}
```

- Effect: `Deny`
- Actions: The user is denied:
  - Reading objects (`s3:GetObject`),
  - Reading ACLs of objects (`s3:GetObjectAcl`).
  - Resource: Applies to all objects within the holidaygifts bucket.
- Condition: The deny applies only between:
  - Start Date: 2022-12-01T00:00:00Z (December 1, 2022, at midnight UTC),
  - End Date: 2022-12-25T06:00:00Z (December 25, 2022, at 6:00 AM UTC).

#### Policy Behavior

- Outside the specified date range (before December 1, 2022, or after December 25, 2022, 6:00 AM UTC):
  - The `Allow` statement applies, so the user can perform all specified actions (`PutObject`, `GetObject`, `DeleteObject`, etc.).
- Within the specified date range (from December 1 to December 25, 2022, 6:00 AM UTC):
  - The `Deny` statement takes precedence over the `Allow` because `Deny` always overrides Allow in AWS policies.
  - The user is denied access to `GetObject` and `GetObjectAcl` during this period, meaning they cannot read objects or view their ACLs.

### Example 2

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyNonApprovedRegions",
      "Effect": "Deny",
      "NotAction": ["cloudfront:*", "iam:*", "route53:*", "support:*"],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": ["ap-southeast-2", "eu-west-1"]
        }
      }
    }
  ]
}
```

This AWS policy aims to deny access to AWS services that are not in the list of explicitly allowed regions, with some exceptions for specific AWS services. Here's a detailed breakdown:

- `NotAction` specifies actions that are excluded from the deny. In this case, the policy will not deny actions for:

  - CloudFront (cloudfront:\*)
  - IAM (iam:\*)
  - Route 53 (route53:\*)
  - AWS Support (support:\*)

- The policy applies to all resources across the account.

- The `Condition` specifies that the deny applies if the requested AWS region is not one of the approved regions:
  - ap-southeast-2 (Sydney),
  - eu-west-1 (Ireland).

### Example 3

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListAllMyBuckets", "s3:GetBucketLocation"],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::cl-animals4life",
      "Condition": {
        "StringLike": {
          "s3:prefix": ["", "home/", "home/${aws:username}/*"]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::cl-animals4life/home/${aws:username}",
        "arn:aws:s3:::cl-animals4life/home/${aws:username}/*"
      ]
    }
  ]
}
```

This AWS IAM policy defines permissions related to the S3 bucket cl-animals4life and provides fine-grained control for users based on their identity.

#### Statement 1: General Permissions

```json
{
  "Effect": "Allow",
  "Action": ["s3:ListAllMyBuckets", "s3:GetBucketLocation"],
  "Resource": "*"
}
```

- Effect: `Allow`
- Actions:
  - `s3:ListAllMyBuckets`: Lets the user list all S3 buckets in their account.
  - `s3:GetBucketLocation`: Lets the user retrieve the region where a bucket is hosted.
- Resource: "\*" – Applies to all S3 buckets.
- This statement provides global permissions to access bucket metadata but not the contents of any bucket.

#### Statement 2: Listing Contents of a Specific Bucket

```json
{
  "Effect": "Allow",
  "Action": "s3:ListBucket",
  "Resource": "arn:aws:s3:::cl-animals4life",
  "Condition": {
    "StringLike": {
      "s3:prefix": ["", "home/", "home/${aws:username}/*"]
    }
  }
}
```

- Effect: `Allow`
- Action: `s3:ListBucket` – Lets the user list objects within the bucket.
- Resource: `arn:aws:s3:::cl-animals4life` – Applies specifically to the cl-animals4life bucket.
- Condition:
  - Restricts the listing to objects that match the specified prefixes:
    - "": The bucket root.
    - "home/": The home directory.
    - "home/${aws:username}/\*": The user's personal directory within home/, where ${aws:username} dynamically resolves to the IAM user's name.
- This ensures users can only list the root, the home directory, and their own subdirectory under home.

#### Statement 3: Full Access to Personal Directory

```json
{
  "Effect": "Allow",
  "Action": "s3:*",
  "Resource": [
    "arn:aws:s3:::cl-animals4life/home/${aws:username}",
    "arn:aws:s3:::cl-animals4life/home/${aws:username}/*"
  ]
}
```

- Effect: `Allow`
- Action: `s3:*` – Grants all S3 actions (e.g., `PutObject`, `GetObject`, `DeleteObject`) for the specified resources.
- Resource:
  - The user's directory (`home/${aws:username}`).
  - All objects within their directory (`home/${aws:username}/*`).
- This gives users full control over their own directory and its contents within the `cl-animals4life` bucket.

#### Policy Behavior

- Users can list all buckets in their account (`s3:ListAllMyBuckets`).
- Users can retrieve the region of any bucket (`s3:GetBucketLocation`).
- Users can list objects within:
  - The bucket root (`""`),
  - The `home/` directory,
  - Their own subdirectory under `home/` (e.g., `home/johndoe`).
- Users have full permissions (`s3:*`) for their specific subdirectory in `home/` and all objects within it.
