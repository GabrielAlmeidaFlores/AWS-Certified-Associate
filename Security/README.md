# Policy Interpretation

## Example 1

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

### First Statement: Allow Access

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

### Second Statement: Deny Access with a Condition

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

### Policy Behavior

- Outside the specified date range (before December 1, 2022, or after December 25, 2022, 6:00 AM UTC):
  - The `Allow` statement applies, so the user can perform all specified actions (`PutObject`, `GetObject`, `DeleteObject`, etc.).
- Within the specified date range (from December 1 to December 25, 2022, 6:00 AM UTC):
  - The `Deny` statement takes precedence over the `Allow` because `Deny` always overrides Allow in AWS policies.
  - The user is denied access to `GetObject` and `GetObjectAcl` during this period, meaning they cannot read objects or view their ACLs.
