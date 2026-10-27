# AWS CloudTrail Notes (Roman Urdu)

## 1. What is AWS CloudTrail?

**AWS CloudTrail records activity in your AWS account**, especially actions performed through AWS APIs.

**Analogy:** Socho tumhari AWS account ek office hai aur CloudTrail us office ka activity register hai. Ye record karta hai kis ne kya action kiya aur kab kiya.

**Example:** Kisi ne AWS account mein EC2 instance launch kiya. CloudTrail mein `RunInstances` jaisa API event record ho sakta hai.

## 2. Common Uses of CloudTrail

| Use case | Example |
|---|---|
| Security investigation | Kis ne S3 object delete kiya? |
| Auditing | Kis ne IAM policy change ki? |
| Troubleshooting | EC2 instance kis identity ne stop kiya? |
| Compliance | AWS account activity ka record review karna |
| Unauthorized activity investigation | Unknown IP se API request aayi thi? |
| Change tracking | Security group ya bucket configuration kab change hui? |

**Important:** CloudTrail investigation mein help karta hai, lekin har event ki details available hon, ye guaranteed nahi. S3 object-level actions record karne ke liye relevant data-event logging enable karni par sakti hai.

## 3. How Does CloudTrail Work?

Example: Tum AWS Console se EC2 instance stop karti ho.

1. Tum `StopInstances` action perform karti ho.
2. CloudTrail event record karta hai. Is mein identity, time aur action ki details ho sakti hain.
3. Tum CloudTrail ki **Event history** mein event search karti ho.
4. Agar trail configure hai, events Amazon S3 mein store aur optionally CloudWatch Logs mein deliver ho sakte hain.

### Two important event types

- **Management events:** AWS resources create, modify ya delete karna. Example: EC2 launch karna ya IAM policy change karna.
- **Data events:** Resource ke andar data par actions. Example: S3 object read, upload ya delete karna.

**Note:** Data events by default log nahi hote; inhein explicitly enable karna hota hai.

## 4. What CloudTrail Does NOT Do by Itself

| CloudTrail khud se kya nahi karta? | Kis cheez ki zaroorat ho sakti hai? |
|---|---|
| Automatically attacker ko block nahi karta | IAM policies aur doosre security controls |
| Har S3 object access automatically record nahi karta | S3 data-event logging enable karna |
| EC2 CPU performance monitor nahi karta | CloudWatch Metrics |
| Har suspicious activity par automatic notification nahi bhejta | EventBridge rules, SNS ya alerting setup |
| Har event ka root cause automatically nahi batata | Logs, investigation aur analysis |
| Har log ko permanently store nahi karta | Trail aur S3 storage/retention configuration |

CloudTrail ka main kaam activity ka record dena hai. Detection, notification aur prevention ke liye additional services ya configurations ki zaroorat ho sakti hai.

## 5. CloudTrail and Related AWS Services

### Amazon CloudWatch
CloudTrail events ko CloudWatch Logs mein bhej kar search, monitoring aur metric filters ke through alarms setup kar sakti ho.

### Amazon EventBridge
Matching events par rule trigger kar sakta hai. Example: EC2 instance terminate hone par notification workflow start karna.

### Amazon S3
CloudTrail log files ko store karne ke liye use ho sakta hai. Logs ko configured retention aur access policies ke mutabiq rakh sakti ho.

### Amazon SNS
Configured workflow ke through email ya doosri supported destinations par notification bhejne mein help karta hai.

### AWS IAM
Users, roles aur permissions manage karta hai. CloudTrail mein IAM-related actions investigate kar sakti ho.

## 6. Cloud Security Lab Example

Socho tumhari S3 bucket `javeria-s3` hai:

- **CloudTrail:** Kis identity ne bucket ki configuration change ki?
- **CloudWatch:** Application ke logs mein error aa raha hai?
- **EventBridge:** Agar bucket policy change ho, to matching event par action trigger karo.
- **SNS:** Configured workflow ke through notification bhejo.
- **IAM:** Decide karta hai kis identity ko bucket ki policy change karne ki permission hai.

## 7. Quick Memory Trick

- **CloudTrail** = Who did what?
- **CloudWatch** = System mein kya ho raha hai?
- **EventBridge** = Event par kya action lena hai?
- **SNS** = Notification kahan bhejni hai?
- **IAM** = Kis ko permission hai?

## Official AWS Reference

- [How AWS CloudTrail works](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/how-cloudtrail-works.html)
- [CloudTrail Event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)
