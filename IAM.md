# AWS IAM — Identity and Access Management 🔐

## 1. What Is IAM?

**AWS IAM (Identity and Access Management)** is a service that helps control who can access AWS resources and what actions they are allowed to perform.

**Analogy:** Imagine an office where employees have ID cards and different access permissions. IAM works similarly by managing identities and permissions in AWS.

## 2. Five IAM Building Blocks

### 1. Root User
The Root User is the original identity created when an AWS account is established. It has extensive permissions over the account.

- Enable MFA.
- Do not use it for everyday tasks.
- Protect its credentials carefully.

**Analogy:** The main owner of a building with a master key.

### 2. IAM User
An IAM User is an identity that can represent a person or workload requiring specific access.

- Grant only the permissions required.
- Secure its credentials.
- Avoid unnecessary long-term access keys.

**Analogy:** An employee with an individual ID card.

### 3. IAM Group
An IAM Group is a collection of IAM users. Policies attached to a group can provide common permissions to its members.

**Example:** A Developers group can receive permissions needed for development work.

**Analogy:** An office department where employees share common access rules.

### 4. IAM Role
An IAM Role is an identity with permissions that a trusted user or AWS service can assume. It commonly provides temporary credentials.

**Example:** An EC2 instance assumes an IAM role to access specific S3 objects.

**Analogy:** A temporary duty pass that grants access for an assigned task.

### 5. IAM Policy
An IAM Policy is a document that defines permissions, including which actions are allowed or denied on specified resources.

**Example:** Allow a user to read objects in an S3 bucket without granting permission to delete them.

**Analogy:** Written office rules that explain what an employee can and cannot do.

## 3. Least Privilege Principle

**Least Privilege** means granting only the minimum permissions required to perform a task.

**Example:** If a user only needs to read files in an S3 bucket, they should not receive full Administrator access.

### Why Is It Important?

- Reduces unnecessary access.
- Limits potential damage from compromised credentials.
- Helps protect sensitive data and resources.

**Analogy:** Give an employee the key to the room they need, not the keys to the entire building.

## 4. What Happens If One Password Is Stolen?

If an attacker steals an IAM user's password, they may attempt to access the account.

The potential damage depends on the identity's permissions and the security controls in place.

**Without Least Privilege:**
- The attacker may access sensitive resources.
- They may modify or delete data if permitted.
- Excessive permissions can increase the impact.

**With Least Privilege:**
- The attacker's permitted actions are more limited.
- Unnecessary access to other resources is reduced.

### How Can We Reduce the Risk?

1. Enable MFA.
2. Apply Least Privilege.
3. Secure credentials and remove unused access keys.
4. Use IAM roles and temporary credentials where appropriate.
5. Monitor suspicious activity using AWS CloudTrail and relevant security alerts.

**Analogy:** If a thief obtains one room's key, that key should not automatically open every room.

## 5. Every Access Request: Two Important Questions

### Question 1: Who Are You?

**Authentication** verifies an identity.

**Example:** Signing in with credentials and completing an MFA challenge.

**Analogy:** A security guard checks your ID card.

### Question 2: What Are You Allowed to Do?

**Authorization** determines whether the requested action is permitted.

**Example:** Checking whether a user has permission to read an S3 object.

**Analogy:** The guard checks whether your ID allows you to enter the server room.

**Remember:** Authentication does not automatically grant permission to perform every action.

## 6. Additional IAM Security Practices

- **MFA:** Adds an extra verification step.
- **Credential Management:** Protect credentials and remove unused access keys.
- **Temporary Credentials:** Prefer temporary credentials through IAM roles when appropriate.
- **Monitoring:** Review AWS CloudTrail events and relevant security alerts.
- **Regular Reviews:** Review users, roles, groups, and policies to remove unnecessary access.

## 7. Quick Revision

| Concept | Purpose |
|---|---|
| Root User | Original AWS account identity |
| IAM User | Identity for a person or workload |
| IAM Group | Organizes users and provides common permissions |
| IAM Role | Assumable identity with defined permissions |
| IAM Policy | Defines allowed and denied actions |
| Least Privilege | Grants only necessary permissions |
| Authentication | Verifies who you are |
| Authorization | Checks what you can do |
| MFA | Adds extra verification |
| CloudTrail | Records AWS account activity |

## 8. Practice Questions

**Q1. What is IAM?**  
IAM manages identities and access permissions in AWS.

**Q2. What is Least Privilege?**  
Granting only the permissions necessary for a task.

**Q3. What is the risk of a stolen password?**  
An attacker may perform actions allowed by the compromised identity.

**Q4. How does Least Privilege help?**  
It limits the actions the compromised identity is permitted to perform.

**Q5. What is Authentication?**  
Verifying who you are.

**Q6. What is Authorization?**  
Determining what actions you are allowed to perform.

**Q7. Why is MFA important?**  
It adds an extra verification step beyond a password.

**Q8. Why use IAM Roles with EC2?**  
They allow EC2 workloads to obtain appropriate AWS permissions without embedding long-term access keys in the application.

---

## Key Takeaway

**Secure identities + Least Privilege + MFA + Monitoring = Stronger AWS Security.**

Always protect identities, grant only necessary permissions, and review access regularly.

*Note: These controls reduce risk but do not guarantee complete protection against every attack.*
