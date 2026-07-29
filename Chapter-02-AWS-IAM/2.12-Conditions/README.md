# 2.12 Conditions in IAM Policies

## 🎯 Learning Objectives
- Understand the purpose of the `Condition` block in IAM policies.
- Learn how to implement context-aware security.
- Explore various condition types: Time, IP, MFA, and Tags.
- Read and interpret JSON Condition syntax.

---

## 📚 Detailed Topic Coverage

The `Condition` element lets you specify *when* a policy is in effect. While `Action` and `Resource` define *what* can be done and to *which* resources, `Condition` defines the *circumstances* under which the action is allowed or denied. This enables fine-grained, context-aware access control.

### Types of Conditions

1. **Time-based (`aws:CurrentTime`)**: Restrict access to specific time windows (e.g., business hours).
2. **IP-based (`aws:SourceIp`)**: Restrict access so it can only originate from known IP addresses (e.g., a corporate VPN or office network).
3. **MFA-based (`aws:MultiFactorAuthPresent`)**: Enforce Multi-Factor Authentication for highly sensitive actions.
4. **Tag-based (`aws:RequestedRegion` / `aws:ResourceTag`)**: Restrict access based on resource tags or specific geographic AWS regions.

---

## 💡 Real-World Analogy

Imagine a highly secure bank vault:
- **Action**: Open the vault door.
- **Resource**: Vault #5.
- **Condition**: Only between 9 AM and 5 PM (`Time`), only if you are standing in the manager's office (`IP`), and only if you insert both your key and the manager's key (`MFA`).

Without all conditions met, the door will not open, even if you are authorized.

---

## 🛠️ Practical Examples

### Example 1: IP-Based Restriction (Office Network Only)
Allows S3 actions only if the request comes from the `192.168.100.0/24` network.
```json
{
  "Effect": "Allow",
  "Action": "s3:*",
  "Resource": "*",
  "Condition": {
    "IpAddress": {
      "aws:SourceIp": "192.168.100.0/24"
    }
  }
}
```

### Example 2: Time-Based Restriction (Business Hours Only)
Allows EC2 actions only after Jan 1, 2024.
```json
{
  "Effect": "Allow",
  "Action": "ec2:*",
  "Resource": "*",
  "Condition": {
    "DateGreaterThan": {
      "aws:CurrentTime": "2024-01-01T00:00:00Z"
    }
  }
}
```

### Example 3: MFA Enforcement
Allows deleting an S3 bucket only if the user has authenticated with MFA.
```json
{
  "Effect": "Allow",
  "Action": "s3:DeleteBucket",
  "Resource": "arn:aws:s3:::critical-data-bucket",
  "Condition": {
    "Bool": {
      "aws:MultiFactorAuthPresent": "true"
    }
  }
}
```

### Example 4: Region Restriction
Allows creating EC2 instances only in the `ap-south-1` region.
```json
{
  "Effect": "Allow",
  "Action": "ec2:RunInstances",
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "aws:RequestedRegion": "ap-south-1"
    }
  }
}
```

---

## ❓ Doubts Discussed

> **Student:** "Give me Condition examples with real scenarios. When would I use `StringLike` vs `StringEquals`?"
> 
> **Answer:** 
> - **StringEquals**: Exact match. Use it when you know the exact value. E.g., `{"StringEquals": {"aws:username": "johndoe"}}`
> - **StringLike**: Wildcard match. Use it when you want to match a pattern. E.g., `{"StringLike": {"s3:prefix": "projects/*"}}` to allow access to any folder starting with "projects/".

---

## 📝 Key Takeaways
📌 Conditions add an extra layer of security beyond basic permissions.
📌 You can chain multiple conditions; all must be true (logical AND) for the condition block to pass.
📌 MFA conditions are essential for protecting destructive actions (like deleting databases or buckets).

---

## 🎤 Interview Questions

<details>
<summary><strong>Basic: What is the purpose of the Condition element in an IAM policy?</strong></summary>
The Condition element determines the circumstances under which the policy grants or denies permission, such as time of day, source IP address, or whether MFA is present.
</details>

<details>
<summary><strong>Intermediate: How would you ensure developers can only access AWS resources from the corporate VPN?</strong></summary>
I would create an IAM policy with a Condition block using the `IpAddress` operator and the `aws:SourceIp` key, specifying the CIDR block of the corporate VPN.
</details>

<details>
<summary><strong>Scenario-Based: A user has full Admin access, but you want to ensure they cannot delete an RDS database unless they used MFA to log in. How do you implement this?</strong></summary>
I would create an IAM policy with `Effect: Deny`, `Action: rds:DeleteDBInstance`, and a Condition using the `Bool` operator checking if `aws:MultiFactorAuthPresent` is `false`. Since Explicit Deny overrides Allow, this effectively blocks the deletion if MFA wasn't used.
</details>
