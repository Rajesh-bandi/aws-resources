# 2.5 IAM Users 👤

Welcome to Section 2.5! Now that we have secured our Root User, it's time to start creating identities for the actual people and applications that will interact with our AWS environment. These identities are called **IAM Users**.

---

## 🎯 Learning Objectives
By the end of this section, you will be able to:
- Create IAM Users via the AWS Management Console.
- Configure Console vs. Programmatic Access for a user.
- Understand the limits and lifecycle of Access Keys.
- Implement Password Policies to enforce security standards.
- Successfully configure the AWS CLI on your local machine using IAM User credentials.

---

## 📚 Detailed Topic Coverage

### What is an IAM User?
An IAM User represents a specific person or application that interacts with AWS. Unlike the root account, an IAM User starts with **absolutely no permissions** (Implicit Deny). You must explicitly attach policies to the user to grant them access.

### Types of Access
When you create a user, you define how they can access AWS:
1. **Console Access (Management Console):**
   - Grants a password that allows the user to sign in to the AWS Web UI.
   - You can auto-generate a password or set a custom one.
   - You can require the user to change their password on their next sign-in (highly recommended).
2. **Programmatic Access (CLI / API / SDK):**
   - Generates an **Access Key ID** and **Secret Access Key**.
   - Used for the AWS Command Line Interface (CLI) or writing code (Python/Boto3, Node.js, etc.).

### Access Key Lifecycle and Limits
- **Limit:** An IAM User can have a maximum of **two (2)** active access keys at any given time. This allows for seamless key rotation without downtime.
- **State:** Keys can be set to `Active` or `Inactive`. If you suspect a key is compromised, make it Inactive immediately before deleting it, to ensure nothing critical breaks.
- **Visibility:** The Secret Access Key is shown **only once** upon creation. If lost, it cannot be recovered.

### IAM Password Policy
To ensure humans are using strong passwords, AWS allows you to configure an Account-Level Password Policy.
You can enforce:
- Minimum length (e.g., 14 characters).
- Requirement for specific character types (uppercase, lowercase, numbers, symbols).
- Password expiration (e.g., must rotate every 90 days).
- Password reuse prevention (e.g., cannot use the last 5 passwords).

---

## 🏗️ Architecture / Diagrams

```mermaid
flowchart TD
    User([New Employee]) --> IAM["Create IAM User"]

    IAM --> Console["Console Access"]
    IAM --> Prog["Programmatic Access"]

    Console --> PWD["Password"]
    PWD -. Regulated by .-> Policy["Account Password Policy"]
    Policy -. Enforces .-> Rules["Length, Complexity, Rotation"]

    Prog --> Keys["Access Key Pair"]
    Keys --> Max["Max 2 Keys per User"]
    Max --> Rotate["Allows seamless Key Rotation"]
```

---

## 💡 Real-World Analogies

- **IAM User:** An employee profile in a company's HR system.
- **Console Access:** The physical badge to enter the office and use the computers.
- **Programmatic Access:** The API token the developer's automated script uses to push logs to the central server.
- **Max 2 Keys Limit:** Like having two keys to your apartment. When you want to change locks, you give your roommate the new key (Key 2) while they are still using the old key (Key 1). Once they confirm Key 2 works, you destroy Key 1.

---

## 🛠️ Practical / Hands-On

Let's do this step-by-step to create a user and configure the AWS CLI.

### Step 1: Create the IAM User
1. Log in to AWS Console > IAM Dashboard.
2. Click **Users** on the left pane > **Add users**.
3. User name: `developer-rajesh`
4. Select **Provide user access to the AWS Management Console**.
5. Choose **I want to create an IAM user**.
6. Select **Custom password** (enter a password) and uncheck "User must create a new password" for this lab.
7. Click Next. Under Permissions, select **Attach policies directly**.
8. Search for and select `AmazonS3ReadOnlyAccess`.
9. Click **Next**, then **Create user**.

### Step 2: Generate Access Keys
1. Click on the newly created user `developer-rajesh`.
2. Go to the **Security credentials** tab.
3. Scroll down to **Access keys** and click **Create access key**.
4. Select **Command Line Interface (CLI)**.
5. Click Next, then **Create access key**.
6. **IMPORTANT:** Copy the Access Key ID and Secret Access Key to a text file temporarily. Do not close this window yet.

### Step 3: Configure AWS CLI
Open your terminal (Command Prompt, PowerShell, or Mac/Linux Terminal).
```bash
# Type the configure command
aws configure

# AWS will prompt you for the following:
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: ap-south-1
Default output format [None]: json
```

### Step 4: Verify the Configuration
Run this command to check WHO the CLI is logged in as:
```bash
aws sts get-caller-identity
```
*Output should show the ARN ending in `user/developer-rajesh`.*

Run this command to test permissions:
```bash
aws s3 ls
```
*Output will list your S3 buckets because we attached `AmazonS3ReadOnlyAccess`.*

---

## ❓ Doubts Discussed

> **Student:** "Can I create an IAM user that ONLY has programmatic access and no console access?"
**Rajesh:** "Yes, absolutely. This is best practice for service accounts or applications. You just don't enable console access or assign a password during user creation."

> **Student:** "Why does AWS limit us to 2 access keys per user? Why not 10?"
**Rajesh:** "Having too many access keys increases the attack surface. Two is the perfect number: it's exactly enough to allow key rotation without application downtime, but small enough to prevent credential sprawl."

---

## ⚠️ Common Mistakes
❌ **Mistake:** Generating access keys and leaving the `.csv` file in your Downloads folder forever.
❌ **Mistake:** Creating one IAM user (e.g., `dev-team`) and sharing the credentials among 5 different developers. (Always create individual users for accountability!).
❌ **Mistake:** Forgetting to set a strict password policy, allowing users to set passwords like `Password123`.

---

## 📝 Key Takeaways
- IAM Users have no permissions by default.
- You can enable Console access (Password) or Programmatic access (Access Keys).
- Max 2 access keys per user for safe rotation.
- Never share IAM user credentials. One user = one physical person/app.
- Enforce a strong password policy at the account level.

---

## 🎤 Interview Questions

### 🟢 Basic
<details>
<summary>1. What is the maximum number of active access keys an IAM user can have?</summary>
An IAM user can have a maximum of two access keys at any given time.
</details>

<details>
<summary>2. You created a new IAM user but didn't attach any policies to it. What can the user do?</summary>
Nothing. Because of Implicit Deny, a new IAM user starts with zero permissions.
</details>

### 🟡 Intermediate
<details>
<summary>3. Why does AWS allow an IAM user to have two access keys instead of just one?</summary>
Having two keys allows for seamless credential rotation. You can generate a new key (Key 2), update your applications to use Key 2, verify it works, and then safely delete the old key (Key 1) without experiencing any application downtime.
</details>

### 🔴 Scenario-Based
<details>
<summary>4. You notice suspicious API activity originating from an IAM user's Access Key. However, this access key is actively being used by a critical production application. How do you handle this securely?</summary>
First, I would generate a second access key for the user and immediately update the production application to use the new key. Once confirmed working, I would mark the compromised key as 'Inactive' (disabling it). I would monitor the logs to ensure the attack stops and the application functions normally. Finally, I would delete the compromised key and investigate the source of the leak (e.g., checking GitHub for exposed credentials).
</details>

---
*Ready for the next step? Proceed to [2.6 IAM Groups](../2.06-IAM-Groups/README.md)*
