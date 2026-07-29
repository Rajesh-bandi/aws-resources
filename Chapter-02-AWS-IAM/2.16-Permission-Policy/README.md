# 2.16 Permission Policy

## 🎯 Learning Objectives
- Define the role of a Permission Policy.
- Write Permission Policies restricting access to specific resources.
- Differentiate Permission Policies from Trust Policies.

---

## 📚 Detailed Topic Coverage

The **Permission Policy** defines the actual permissions (Allow/Deny, Actions, Resources, Conditions) granted to whoever successfully assumes the IAM Role. It uses the exact same JSON format as the policies you attach to IAM Users or Groups.

### Applying Least Privilege

When writing a Permission Policy, it is critical to follow the **Principle of Least Privilege**. Never use `*` (wildcard) for actions or resources unless absolutely necessary.

Instead of:
`"Action": "s3:*", "Resource": "*"`

Use specific actions and resources:
`"Action": ["s3:GetObject", "s3:ListBucket"], "Resource": ["arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*"]`

---

## 🛠️ JSON Examples

### Example 1: S3 Read-Only (Specific Bucket)
Allows listing a specific bucket and downloading its objects.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::app-data-bucket"
    },
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::app-data-bucket/*"
    }
  ]
}
```

### Example 2: DynamoDB Full Access (Specific Table)
Allows all DynamoDB operations, but ONLY on the `UsersTable`.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "dynamodb:*",
      "Resource": "arn:aws:dynamodb:us-east-1:123456789012:table/UsersTable"
    }
  ]
}
```

---

## 📝 Key Takeaways
📌 The Permission Policy defines **WHAT** actions are allowed when the role is assumed.
📌 Always restrict the `Resource` element to specific ARNs when possible.
📌 Permission policies attached to roles behave exactly like those attached to users.

---

## 🎤 Interview Questions

<details>
<summary><strong>Basic: What does a Permission Policy do when attached to an IAM Role?</strong></summary>
It dictates what API actions the entity assuming the role is allowed to perform, and on which specific AWS resources.
</details>

<details>
<summary><strong>Intermediate: A role has a permission policy allowing `s3:PutObject` on `*`. Is this secure?</strong></summary>
No, this violates the principle of least privilege. It allows the role to upload files to *any* S3 bucket in the account. The `Resource` should be restricted to the specific bucket ARN(s) required.
</details>

<details>
<summary><strong>Scenario-Based: If an IAM User has an explicit Deny on S3 in their own user policies, but they assume a role that has an Allow on S3 in its permission policy, can they access S3?</strong></summary>
Yes. When a user assumes a role, they temporarily drop their original user permissions and take on the permissions of the role. The user's original Explicit Deny no longer applies during the role session.
</details>
