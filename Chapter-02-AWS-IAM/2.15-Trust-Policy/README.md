# 2.15 Trust Policy

## 🎯 Learning Objectives
- Understand the function of a Trust Policy within an IAM Role.
- Define "Principal" in the context of IAM.
- Create Trust Policies for AWS Services (EC2, Lambda).
- Create Trust Policies for Cross-Account access.

---

## 📚 Detailed Topic Coverage

While a Permission Policy answers the question *"What can this role do?"*, the **Trust Policy** answers the question *"Who is allowed to assume this role?"*. 

A Trust Policy is a resource-based policy attached directly to the IAM Role. It uses the `Principal` element to define the trusted entity.

### The `Principal` Element
The Principal is the entity (user, service, or account) that is allowed or denied access to a resource. In a Trust Policy, the Principal defines who can call `sts:AssumeRole`.

---

## 💡 Real-World Analogy

> **Student:** "What is the difference between Trust Policy and Permission Policy?"

**Answer:** 
Think of a high-security facility and a master keycard (the IAM Role).
- **Trust Policy**: The logbook at the front desk that lists *WHO* is authorized to pick up the master keycard. (e.g., "Only the Night Manager can pick up this keycard").
- **Permission Policy**: The programming inside the keycard that dictates *WHICH DOORS* it can open. (e.g., "Opens the Server Room and the Electrical Room").

You need both: If you aren't on the list (Trust Policy), you can't get the card. If you get the card but try to open the Vault, the card won't work (Permission Policy).

---

## 🛠️ JSON Examples

### Example 1: EC2 Trust Policy
Allows the EC2 service to assume the role. This is required when attaching a role to an EC2 instance.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Example 2: Lambda Trust Policy
Allows the Lambda service to assume the role (Execution Role).
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Example 3: Cross-Account Trust Policy
Allows any authenticated user/role in AWS Account `111122223333` to assume this role.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
*(Note: Specifying `root` means trusting the entire account. The administrator of `111122223333` must still grant their users permission to actually call assume-role).*

---

## ⚠️ Common Mistakes

> [!WARNING]
> A very common error when setting up an EC2 role is getting a "Not Authorized to perform sts:AssumeRole" error. This usually means the Trust Policy is missing the `ec2.amazonaws.com` service principal, or you accidentally pasted a Permission Policy into the Trust Policy field.

---

## 📝 Key Takeaways
📌 Trust Policy = **WHO** can wear the hat.
📌 Permission Policy = **WHAT** the hat lets you do.
📌 The `Action` in a Trust Policy is almost always `sts:AssumeRole`.

---

## 🎤 Interview Questions

<details>
<summary><strong>Basic: What is a Trust Policy?</strong></summary>
A Trust Policy is a JSON document attached to an IAM Role that defines which principals (users, AWS services, or other accounts) are allowed to assume that role.
</details>

<details>
<summary><strong>Intermediate: You created a role with full S3 access, but when you attach it to an EC2 instance, the instance cannot access S3. What might be wrong?</strong></summary>
The Trust Policy on the IAM Role might be misconfigured. It needs to explicitly list `"Service": "ec2.amazonaws.com"` as the Principal allowed to perform `sts:AssumeRole`.
</details>

<details>
<summary><strong>Scenario-Based: Can you restrict a Cross-Account Trust Policy so that only a specific IAM User in the remote account can assume it, rather than the whole account?</strong></summary>
Yes. Instead of using the remote account's root ARN in the Principal block (e.g., `"AWS": "arn:aws:iam::111122223333:root"`), you can specify the exact ARN of the remote IAM user (e.g., `"AWS": "arn:aws:iam::111122223333:user/Bob"`).
</details>
