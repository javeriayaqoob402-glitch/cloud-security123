# AWS Lambda — How It Works
### Beginner Notes | AWS Cloud Security

## 1. What Is AWS Lambda?

AWS Lambda is a **serverless compute service** that runs your code when a specific event occurs or when the function is invoked.

Simple words mein: Tum Python code likhti ho aur AWS Lambda us code ko run karta hai. Underlying servers ki management AWS handle karta hai.

**Important:** Serverless ka matlab ye nahi ke servers exist nahi karte. Iska matlab hai ke tumhein un servers ko khud manage nahi karna padta.

## 2. Real-Life Analogy — Automatic Worker

Socho tumhari ek shop hai jahan automatic bell lagi hui hai.

- **Shop ka storage room = S3 Bucket**
- **Koi nayi file aana = Event**
- **Automatic bell = Trigger**
- **Worker = AWS Lambda**
- **Worker ki instructions = Python Code**
- **Worker ko di gayi authority = IAM Permissions**
- **Worker ne kya kiya uska record = CloudWatch Logs**

### Example

Jab koi customer shop mein aata hai, bell bajti hai. Worker ko signal milta hai aur woh apna kaam shuru kar deta hai.

Isi tarah, jab S3 bucket mein koi file upload hoti hai, configured event notification Lambda ko invoke kar sakti hai. Lambda phir tumhara Python code run karta hai.

## 3. How Does AWS Lambda Work?

### Step 1 — Create a Lambda Function

AWS Console mein Lambda service open karo aur function create karo.

Function mein tum Python code likhti ho jo specific task perform karega.

### Step 2 — Configure an Event or Trigger

Tum decide karti ho ke function kab run hona chahiye.

Examples:

- S3 mein file upload ho.
- EC2 instance launch hone ki API activity record ho.
- Scheduled time par security check run ho.
- API request receive ho.

Har event source ka configuration alag ho sakta hai.

### Step 3 — Configure Permissions

IAM permissions aur invocation permissions ka kaam different hai.

**Execution Role:** Lambda ko batata hai ke uska code kin AWS resources ko access kar sakta hai.

**Invocation Permission:** Required event source ya service ko Lambda invoke karne ki permission deti hai.

Example: Lambda ko S3 object read karna ho to uske execution role mein suitable `s3:GetObject` permission honi chahiye.

### Step 4 — Event Occurs

Suppose tum `javeria-s3` bucket mein `image.jpg` upload karti ho.

S3 configured event notification ke through Lambda ko event bhejta hai.

Event mein bucket name aur object key jaisi information ho sakti hai.

### Step 5 — Lambda Runs Python Code

Lambda event ko `event` parameter mein receive karta hai aur tumhara handler execute karta hai.

Example:

```python
def lambda_handler(event, context):
    for record in event["Records"]:
        bucket = record["s3"]["bucket"]["name"]
        file_name = record["s3"]["object"]["key"]

        print("Bucket:", bucket)
        print("Uploaded file:", file_name)

    return {
        "statusCode": 200,
        "message": "Event processed"
    }
```

**Code ka kaam:**
1. Event mein se bucket ka naam read karna.
2. Uploaded file ka naam read karna.
3. Dono values ko execution logs mein print karna.

Ye example S3 object ko download ya scan nahi karta. Ye sirf event ki information read karta hai.

Note: Ye handler S3 notification ke `Records` format ke liye hai; har AWS event ka JSON format same nahi hota.

### Step 6 — Check the Logs

Amazon CloudWatch Logs mein tum Lambda execution ke logs dekh sakti ho.

Agar code ne bucket aur file ka naam print kiya, to woh output logs mein nazar aa sakta hai.

CloudTrail alag purpose ke liye hai: woh AWS API activity aur identity information record kar sakta hai, jaise kis user ya role ne EC2 launch request ki.

## 4. AWS Cloud Security Example — EC2 Instance Launch

Suppose tum chahti ho ke jab bhi koi EC2 instance launch kare, tumhein pata chale:

- Kis identity ne launch request ki.
- Event kis time hua.
- Kaunsa AWS region use hua.
- Instance ki ID kya hai, jab event data mein available ho.

Iske liye ye workflow use kar sakti ho:

**CloudTrail → EventBridge Rule → Lambda → CloudWatch Logs**

1. CloudTrail EC2 API activity record karta hai.
2. EventBridge rule `RunInstances` event ko match karta hai.
3. EventBridge Lambda ko invoke karta hai.
4. Lambda Python code event ki details read karta hai.
5. CloudWatch Logs mein selected details record ho sakti hain.

Is tarah Lambda cloud security automation mein help karta hai.

## 5. Lambda vs Trigger vs Event vs Permission

| Component | Simple meaning |
|---|---|
| Event | Kya activity hui? |
| Trigger / Rule | Kis event par function run karna hai? |
| Lambda Function | Kaunsa code execute karna hai? |
| Python Code | Actual task ki instructions |
| IAM Execution Role | Code ko kya access mil sakta hai? |
| Invocation Permission | Kaunsi service function ko invoke kar sakti hai? |
| CloudWatch Logs | Function ke execution logs kahan dekhne hain? |
| CloudTrail | AWS API activities ka audit record |

### Easy Formula

**Event = What happened?**

**Trigger = When should Lambda run?**

**Python Code = What should Lambda do?**

**Permission = What is allowed?**

**CloudWatch = What did the function log?**

## 6. Important Things to Remember

- Lambda code ko automatically har AWS activity ka pata nahi chalta; event source ya invocation method configure karna hota hai.
- Trigger configure karne se required IAM permissions automatically har case mein complete nahi hoti.
- Lambda ka code event ki information process kar sakta hai, logs likh sakta hai, alerts bhej sakta hai ya authorized actions perform kar sakta hai.
- Lambda khud permanent file storage service nahi hai.
- Production security automation mein least privilege, error handling, retries aur duplicate events ka khayal rakhna chahiye.

## 7. Quick Revision

AWS Lambda ek event-driven compute service hai. Tum Python code likhti ho, event source ya trigger configure karti ho, aur required permissions deti ho. Event hone par Lambda function invoke hota hai aur code apna task perform karta hai.

**Cloud Security mein Lambda ka main benefit:** Repetitive security tasks ko automatically perform karna, taake har kaam manually karne ki zaroorat kam ho.
