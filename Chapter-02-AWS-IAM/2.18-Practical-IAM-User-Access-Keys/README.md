# 2.18 Practical — IAM User with Access Keys (⭐)

## 🎯 Objective
Understand how to access AWS services programmatically using long-term credentials (Access Keys) and understand the security implications.

---

## 🛠️ Step-by-Step Lab

### Step 1: Create IAM User in AWS Console
1. Navigate to the **IAM Console**.
2. Click on **Users** in the left sidebar, then **Create User**.
3. **User name**: `cli-user`
4. Do **not** select "Provide user access to the AWS Management Console" (we only want programmatic access). Click Next.

### Step 2: Attach Permissions
1. Choose **Attach policies directly**.
2. Search for and select the **AmazonS3ReadOnlyAccess** managed policy.
3. Click Next, then **Create user**.

### Step 3: Generate Access Key
1. Click on your newly created `cli-user`.
2. Go to the **Security credentials** tab.
3. Scroll down to **Access keys** and click **Create access key**.
4. Select **Command Line Interface (CLI)**, check the confirmation box, and click Next.
5. Click **Create access key**.
6. ⚠️ **CRITICAL:** Copy the **Access Key ID** and **Secret Access Key**. *This is the ONLY time you will ever see the Secret Key!* If you lose it, you must generate a new one.

### Step 4: Configure AWS CLI
Open your terminal (Command Prompt, PowerShell, or bash) and run:

```bash
aws configure
```

You will be prompted to enter the details:
```
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: ap-south-1
Default output format [None]: json
```

### Step 5: Verify Access
Let's verify that the CLI is using our new user and can list S3 buckets.

```bash
aws sts get-caller-identity
```
*Output should show the ARN ending in `user/cli-user`.*

```bash
aws s3 ls
```
*Output should list your S3 buckets (or return nothing if you have no buckets, but it should not return an Access Denied error).*

### Step 6: Where are credentials stored?
AWS CLI stores these long-term credentials in plain text files on your local machine. Let's inspect them:

**On Linux/Mac:**
```bash
cat ~/.aws/credentials
cat ~/.aws/config
```

**On Windows:**
```powershell
type %USERPROFILE%\.aws\credentials
type %USERPROFILE%\.aws\config
```

> [!CAUTION]
> If a hacker gains access to your `~/.aws/credentials` file, they have complete access to your AWS account based on those permissions, FOREVER (until you manually delete the keys in the console). This is why you should never hardcode these keys in scripts or put them on EC2 instances!

---

## 📝 Key Learnings
- Long-term credentials consist of an **Access Key ID** and **Secret Access Key**.
- `aws configure` stores these locally in `~/.aws/credentials`.
- It is highly insecure to store these credentials on servers or in source code.

---

## 🎤 Interview Questions

<details>
<summary><strong>Basic: What is the purpose of `aws configure`?</strong></summary>
It is used to set up the AWS CLI with credentials (Access Key, Secret Key) and default settings (Region, Output format) so the CLI can authenticate and make API calls to AWS.
</details>

<details>
<summary><strong>Intermediate: Where does the AWS CLI store credentials configured via `aws configure` on a Linux system?</strong></summary>
They are stored in plaintext in the `~/.aws/credentials` file.
</details>

<details>
<summary><strong>Scenario-Based: A developer accidentally committed their `~/.aws/credentials` file to a public GitHub repository. What is the immediate course of action?</strong></summary>
Immediately log into the AWS IAM console, find the user associated with those keys, and Delete or Deactivate the exposed Access Keys. Then, rotate the keys, audit CloudTrail logs to see if the exposed keys were used maliciously, and remove the file from Git history.
</details>
