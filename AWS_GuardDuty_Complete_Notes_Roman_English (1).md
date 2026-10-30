# AWS GuardDuty — Complete Notes (Roman English)

## 1. What is AWS GuardDuty?

AWS GuardDuty aik managed cloud threat detection service hai. Yeh AWS environment mein supported security-related activities ko continuously monitor aur analyze karta hai. Agar suspicious ya potentially malicious activity detect ho, to GuardDuty security finding generate karta hai.

**Simple definition:** GuardDuty aik digital security guard hai jo AWS resources par nazar rakhta hai, threats detect karta hai aur security team ko alert deta hai.

## 2. Real-Life Analogy

Socho aik bank hai:
- CCTV aur security systems activities observe karte hain.
- Security guard unusual activity notice karta hai.
- Guard incident ki details collect karta hai.
- Activity ko suspicious samajh kar report banata hai.
- Manager ya security team investigation karti hai.
- Zaroorat par security team action leti hai.

AWS GuardDuty bhi isi tarah kaam karta hai. Yeh digital guard hai, lekin khud har attack ko automatically block nahi karta.

## 3. How GuardDuty Works — Main Workflow

### Step 1: Collect
GuardDuty supported AWS data sources se security-related information use karta hai.

Examples:
- CloudTrail events
- VPC Flow Logs
- DNS activity
- Supported S3 data events
- Supported EKS and runtime activity

### Step 2: Analyze
GuardDuty collected activity ko analyze karta hai. Is mein machine learning, anomaly detection aur threat intelligence jaise methods use ho sakte hain.

Example: IAM user ki activity normal pattern se different ho ya EC2 unusual network destination se communicate kare.

### Step 3: Detect
GuardDuty suspicious ya potentially malicious behaviour identify karta hai.

### Step 4: Create a Finding
GuardDuty aik **Finding** generate karta hai. Finding aik security report/alert hoti hai jo detected activity ki details batati hai.

Finding mein aam tor par:
- Finding type
- Severity
- Affected AWS resource
- Detection time
- Description
- Relevant activity details

### Step 5: Investigate
Security analyst finding ko review karta hai:
- Kya activity expected thi?
- Kaunsa AWS resource affected hai?
- Kya account ya resource compromise ho sakta hai?
- Kya additional evidence available hai?

### Step 6: Respond
Security team appropriate action leti hai. Example:
- Compromised credentials rotate ya revoke karna.
- IAM permissions review karna.
- EC2 ko approved process ke mutabiq isolate karna.
- Security Group rules update karna.
- Incident document karna aur escalate karna.

**Important:** Investigation aur response security team ya configured automation karti hai. GuardDuty ka primary role detection aur findings generate karna hai.

## 4. What Does GuardDuty Watch?

Yahan “watch” ka matlab supported AWS activities ko monitor karna hai.

| Data source / protection | Kya information observe hoti hai? |
|---|---|
| CloudTrail | AWS API calls aur account activity |
| VPC Flow Logs | Network traffic metadata |
| DNS activity | DNS requests aur domains se related activity |
| S3 Protection | Supported S3 object access activity |
| EKS Protection | Kubernetes audit activity |
| Runtime Monitoring | Supported workloads ki runtime activity |

**Note:** Har protection feature har account mein automatically enabled nahi hota. Kuch features ko separately configure/enable karna hota hai. Exact coverage AWS configuration aur supported resources par depend karti hai.

### Analogy
- CloudTrail = Bank ka transaction register.
- VPC Flow Logs = Network traffic ka record.
- DNS = Kis domain/address ko contact kiya gaya.
- S3 Protection = Locker/file access ka record.

## 5. What Can GuardDuty Detect?

GuardDuty supported data aur enabled protections ke zariye mukhtalif threats detect kar sakta hai:

1. **Suspicious access:** Unusual API activity ya potentially compromised credentials.
2. **Reconnaissance:** Port scanning ya probing jaisi activity.
3. **Cryptocurrency mining:** Supported workloads par suspicious mining activity.
4. **Data exfiltration:** Data ko environment se bahar transfer karne ke suspicious patterns.
5. **Malware:** Supported EC2, container/runtime ya S3 protection capabilities ke zariye malware-related activity.
6. **Command and Control (C2):** Compromised system ka suspicious external control infrastructure se communicate karna.

GuardDuty har possible attack detect karne ki guarantee nahi deta. Detection enabled features, available telemetry aur AWS coverage par depend karti hai.

## 6. What is a Finding?

Finding GuardDuty ki generated security report hoti hai.

Example: Tumhara EC2 instance `JAVERIA-EC2-1` hai. Agar GuardDuty ko suspicious network communication detect hoti hai, to woh finding generate kar sakta hai.

Finding analyst ko batati hai:
- Kya suspicious activity detect hui?
- Kis resource par hui?
- Kab hui?
- Severity kya hai?
- Kis type ka threat ho sakta hai?

Finding ka matlab hamesha yeh nahi ke attack 100% confirm hai. Analyst ko context check karke investigation karni hoti hai.

## 7. Severity Levels

GuardDuty findings ki severity 3 main categories mein hoti hai:

| Severity | Score | Meaning |
|---|---:|---|
| Low | 1.0–3.9 | Relatively low-risk finding; review karein |
| Medium | 4.0–6.9 | Potential security concern; investigate karein |
| High | 7.0–8.9 | Serious potential threat; urgent investigation |

**Important:** GuardDuty ki standard severity categories Low, Medium aur High hain. Iski severity range 0.1 se 10.0 tak hoti hai. Critical ko separate standard GuardDuty severity category na samjhein.

### Analogy
Hospital triage ki tarah:
- Low = Kam urgency.
- Medium = Check karna zaroori.
- High = Urgent attention.

Severity potential risk/urgency indicate karti hai; yeh akelay attack ki certainty prove nahi karti.

## 8. Does GuardDuty Block Attacks?

**No. GuardDuty ka primary kaam detection aur alerting hai, direct blocking nahi.**

Example:
- Attacker EC2 ko suspicious traffic bhejta hai.
- GuardDuty suspicious activity detect karta hai.
- GuardDuty finding generate karta hai.
- Security analyst finding investigate karta hai.
- Security team Security Group ya doosre suitable controls ke zariye traffic restrict kar sakti hai.

GuardDuty ko firewall ke saath confuse na karein.

## 9. Automated Response — EventBridge + Lambda

GuardDuty findings par automated response configure kiya ja sakta hai.

Workflow:
1. **GuardDuty:** Finding generate karta hai.
2. **Amazon EventBridge:** Finding event receive karta hai aur configured rule match karta hai.
3. **AWS Lambda:** EventBridge rule ke through Lambda function invoke hota hai.
4. **Response action:** Lambda configured logic ke mutabiq action perform kar sakta hai, jaise notification bhejna ya approved isolation workflow start karna.

Automation tabhi kaam karegi jab EventBridge rule, Lambda permissions, code aur required IAM permissions sahi configure hon. High-impact actions ko carefully test aur approve karna chahiye.

## 10. GuardDuty vs Other AWS Security Services

| AWS Service | Main responsibility |
|---|---|
| GuardDuty | Threat detection aur findings |
| Security Groups | EC2/network interface traffic allow ya block |
| Network ACL | Subnet-level traffic filtering |
| AWS WAF | Web requests ko filter/block karna |
| Amazon Detective | Security findings aur related data ki investigation mein help |
| EventBridge | Events ko rules ke zariye route/trigger karna |
| Lambda | Code aur configured response actions execute karna |

Yeh services aik doosre ka replacement nahi hain. Inke roles different hain.

## 11. Practical Example — Javeria's EC2

Tumhara EC2 instance `JAVERIA-EC2-1` hai aur us par Nginx website hosted hai.

Imagine:
1. EC2 par unusual network activity hoti hai.
2. GuardDuty available telemetry analyze karta hai.
3. Suspicious pattern milne par finding generate ho sakti hai.
4. Tum AWS Console mein GuardDuty → Findings open karti ho.
5. Finding type, severity, affected resource aur details check karti ho.
6. Investigation ke baad suitable response leti ho.

## 12. Interview Questions and Answers

### Q1. What is AWS GuardDuty?
AWS GuardDuty is a managed threat detection service that monitors supported AWS data sources and generates security findings for suspicious activity.

### Q2. Does GuardDuty block attacks?
No. Its primary purpose is threat detection and alerting. Blocking requires separate security controls or configured automation.

### Q3. What is a GuardDuty finding?
A finding is a security report that describes potentially suspicious or malicious activity, the affected resource and its severity.

### Q4. What is severity?
Severity indicates the potential risk level of a finding. GuardDuty uses Low, Medium and High categories.

### Q5. What is the role of EventBridge?
EventBridge can receive GuardDuty finding events and trigger configured targets, such as Lambda.

### Q6. What is the difference between detection and prevention?
Detection identifies suspicious activity. Prevention uses controls to stop or restrict unwanted activity.

### Q7. What does investigation mean?
Investigation means reviewing a finding, checking the affected resource and related evidence, and deciding whether the activity is malicious or expected.

## 13. Quick Revision

- GuardDuty = AWS threat detection service.
- Watch = Supported security activity monitor karna.
- Collect = Security-related data sources se information lena.
- Analyze = Activity ko examine karna.
- Detect = Suspicious behaviour identify karna.
- Finding = Security report/alert.
- Severity = Potential risk level.
- Investigate = Finding aur evidence ko review karna.
- Respond = Suitable security action lena.
- GuardDuty = Primarily detects; it does not directly block by default.
- EventBridge = Finding event par rule trigger karta hai.
- Lambda = Configured response code execute kar sakta hai.

**One-line workflow:**
Collect → Analyze → Detect → Create Finding → Investigate → Respond

## 14. Self-Test

1. AWS GuardDuty kya hai?
2. GuardDuty ka primary purpose kya hai?
3. Finding ka kya matlab hai?
4. GuardDuty kin data sources ko use kar sakta hai?
5. Low, Medium aur High severity ka kya matlab hai?
6. Kya GuardDuty by default attack ko block karta hai?
7. EventBridge aur Lambda ka role kya hai?
8. Detection aur prevention mein kya difference hai?
9. Investigation mein analyst kya check karta hai?
10. GuardDuty aur Security Group mein kya difference hai?

---

## Study Note

Yeh learning notes hain. AWS features, supported data sources aur configuration waqt ke saath change ho sakte hain. Practical implementation ke liye official AWS documentation aur apne account ki enabled protections check karein.

Official documentation:
- https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html
- https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings.html
- https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings-severity.html
