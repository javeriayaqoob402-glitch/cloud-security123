# AWS IAM Policy — Beginner Notes

## 1. What is an IAM Policy?

An **IAM Policy** is a JSON document that defines permissions in AWS. It tells AWS which actions are allowed or denied on specific resources.

**Analogy:** Imagine a security guard in a building. The guard follows rules to decide who can enter, what they can do, and which rooms they can access.

## 2. Main Elements of an IAM Policy

### 1. Statement — The Complete Rule
`Statement` contains the permission rule or rules.

**Analogy:** Security guard ki rulebook mein likha hua poora rule.

### 2. Effect — Allow or Deny
`Effect` defines whether an action is allowed or denied.

- `Allow` = Permission dena.
- `Deny` = Permission rokna.

**Analogy:** Guard kahe, "Andar jaane ki ijazat hai" ya "Andar jaana mana hai."

### 3. Action — What Can You Do?
`Action` defines which operation is allowed or denied.

Examples:
- `s3:GetObject` = Read or download an S3 object.
- `s3:PutObject` = Upload an object to S3.
- `s3:DeleteObject` = Delete an S3 object.

**Analogy:** Room mein file dekhna, file rakhna, ya file delete karna.

### 4. Resource — On Which AWS Resource?
`Resource` identifies the AWS resource to which the rule applies.

Example:
`arn:aws:s3:::javeria-s3/*`

This identifies objects inside the S3 bucket named `javeria-s3`.

**Analogy:** Permission kis room ya file ke liye hai.

### 5. Principal — Who Can Access?
`Principal` identifies who or what can access a resource in a resource-based policy, or which identity a role trusts in a trust policy.

Examples:
- An AWS account
- An IAM user or role
- An AWS service, such as EC2

**Analogy:** Kaun sa person ya employee building mein aa sakta hai?

**Important:** Principal is not normally included in a standard identity-based IAM policy attached to a user, group, or role.

### 6. Condition — Under What Conditions?
`Condition` adds requirements that must be met for a policy statement to apply.

Examples:
- Request must come from a particular IP address.
- Request must meet a specified condition, such as using MFA where configured.

**Analogy:** Sirf tab entry mile jab employee ke paas valid ID ho ya woh allowed time par aaye.

## 3. JSON Policy Example

This example allows reading objects in the `javeria-s3` bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::javeria-s3/*"
    }
  ]
}
```

### Understanding the Example

- `Version` = Policy language version.
- `Statement` = Permission rule.
- `Effect: Allow` = Allow the specified action.
- `Action: s3:GetObject` = Read or download objects.
- `Resource` = Objects inside the `javeria-s3` bucket.

This statement does not grant upload or delete permission.

## 4. Two Important Security Questions

Every access request should be evaluated around these questions:

1. **Authentication:** Who are you?
2. **Authorization:** What are you allowed to do?

**Analogy:**
- Authentication = Guard checks your identity card.
- Authorization = Guard checks whether your ID allows you into a particular room.

AWS evaluates applicable policies and other access controls. An explicit `Deny` overrides an `Allow`.

## 5. Least Privilege Principle

**Least Privilege** means giving a user or service only the permissions required to perform its task.

Example:
If an application only needs to read files from S3, do not give it full administrator access. Give it only the necessary read permissions.

**Analogy:** Employee ko sirf uske kaam wale room ki key do, poori building ki master key nahi.

## 6. Quick Revision

| Element | Meaning |
|---|---|
| Statement | Complete rule |
| Effect | Allow or Deny |
| Action | What can be done? |
| Resource | Which resource? |
| Principal | Who or what is trusted or granted access? |
| Condition | Under what conditions? |

**Remember:** Statement = Rule, Effect = Permission decision, Action = Task, Resource = Target, Principal = Identity, Condition = Requirement.
