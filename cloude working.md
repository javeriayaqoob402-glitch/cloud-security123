# ☁️ How Cloud Works — How a Request Reaches AWS

## 1. What is Cloud Computing?

Cloud computing ka matlab hai internet ke through servers, storage, databases aur doosray computing resources use karna.

Hum apna physical server khareednay ke bajaye AWS jaisi cloud service use kar sakte hain.

**Example:** Hum AWS EC2 par apni website host kar sakte hain aur users internet ke through us website ko access kar sakte hain.

**Analogy:** Jaise apni building bananay ke bajaye hum kisi building mein space rent par lete hain, waise hi AWS se server rent par use kar sakte hain.

---

## 2. How Does a Request Reach the Cloud?

Jab hum browser mein website open karte hain, to request different networking steps se guzarti hai.

### Step 1: User Sends a Request

Jab hum browser mein `example.com` type karte hain, to humari device website ko request bhejnay ki tayyari karti hai.

**Example:** Hum Google ya kisi online shopping website ko open karte hain.

**Analogy:** Jaise hum kisi shop ka address pooch kar wahan jana chahte hain.

### Step 2: DNS Finds the IP Address

DNS ka full form Domain Name System hai.

DNS domain name, jaise `example.com`, ka corresponding IP address find karne mein help karta hai.

Computer network par IP address use karke destination tak traffic pohanchata hai.

**Analogy:** DNS phone directory ki tarah hai. Hum naam search karte hain aur humein us se related number mil jata hai.

**Important:** Agar DNS result pehle se cache mein ho, to har request par DNS lookup dobara karna zaroori nahi hota.

### Step 3: Request Travels Through the Network

Browser HTTP ya HTTPS request banata hai. Device network ke through data packets bhejti hai.

Packets router aur doosray network devices se guzarte hue destination ki taraf travel karte hain.

**Analogy:** Jaise parcel ek shehar se doosray shehar tak different roads aur distribution centers se guzarta hai.

### Step 4: Request Reaches AWS

Agar website AWS par hosted hai, to request AWS ke network tak pohanchti hai.

AWS mein VPC, route tables, Internet Gateway aur security rules configured hotay hain.

Ye settings decide karti hain ke traffic kis raaste se jayega aur server tak pohanch sakta hai ya nahi.

**Analogy:** AWS VPC ek housing society hai, route table uska road map hai aur Internet Gateway internet ki taraf gate hai.

**Important:** Internet Gateway sirf VPC ke saath attach karna kaafi nahi. Routing aur security configuration bhi sahi honi chahiye.

### Step 5: Security Rules Check Traffic

Agar request EC2 instance tak pohanchna chahti hai, to network routing aur security controls important hotay hain.

- Security Group: EC2 ke inbound aur outbound traffic ko control karta hai.
- Network ACL: Subnet ke inbound aur outbound traffic ko control karti hai.
- Route Table: Traffic ka next destination decide karti hai.

**Example:** HTTPS website ke liye TCP port `443` allow hona chahiye, saath hi baqi required configuration bhi sahi honi chahiye.

**Analogy:** Jaise building ka security guard check karta hai ke visitor ko entry ki permission hai ya nahi.

### Step 6: Web Server Processes the Request

Jab request web server tak pohanchti hai, to web server usay process karta hai.

AWS EC2 ek virtual server hai jahan hum Nginx, Apache ya apni application run kar sakte hain.

Server website ka HTML, CSS, JavaScript ya doosra required response return kar sakta hai.

Agar application ko database ya S3 se data chahiye, to woh configured permissions aur network rules ke mutabiq un services ko request bhej sakti hai.

**Analogy:** Jaise restaurant mein order kitchen tak pohanchta hai aur kitchen order ke mutabiq food tayyar karta hai.

### Step 7: Response Returns to the User

Server HTTP response user ke browser ko bhejta hai.

Response mein website ka content, status code aur doosra data ho sakta hai.

Browser response ko process karta hai aur website screen par display karta hai.

**Analogy:** Restaurant mein order tayyar honay ke baad food customer tak wapas aata hai.

---

## 3. Complete Request Flow

User's Laptop
↓
DNS — Domain ka IP address find karta hai
↓
Internet — Request network ke through travel karti hai
↓
AWS VPC — Request AWS network tak pohanchti hai
↓
Route Table + Security Controls — Traffic ka raasta aur permissions
↓
EC2 Web Server — Request process karta hai
↓
HTTP Response — Browser ko wapas milta hai
↓
Website Displayed on Screen

**Note:** Ye simplified flow hai. Real architecture mein CDN, load balancer, firewall aur doosray components bhi ho sakte hain.

---

## 4. AWS Components Used in This Process

| Component | Purpose |
|---|---|
| DNS | Domain name ka IP address find karna |
| Internet | Devices aur servers ke darmiyan communication |
| VPC | AWS ka logically isolated virtual network |
| Subnet | VPC ke andar IP network ka chhota segment |
| Route Table | Traffic ka route decide karna |
| Internet Gateway | VPC aur internet ke darmiyan connectivity |
| Security Group | EC2 ke traffic ko control karna |
| EC2 | Virtual server jahan application run hoti hai |
| Nginx | Web requests handle karne wala web server |
| S3 | Files, images aur backups store karna |

---

## 5. What Happens if Something Is Misconfigured?

Agar website open nahi ho rahi, to possible issues ye ho sakte hain:

- DNS ka IP address incorrect ho.
- Route Table mein required route missing ho.
- Internet Gateway attach na ho.
- EC2 ke paas suitable public IP na ho.
- Security Group required port allow na karta ho.
- Nginx ya application running na ho.
- Website ka domain incorrect IP ko point kar raha ho.

Cloud Security mein hum in settings ko check karte hain taake website secure bhi rahe aur properly work bhi kare.

---

## 6. Final Concept

Cloud ek remote computing environment hai jahan hum servers aur doosri services internet ke through use karte hain.

Jab user website open karta hai, to request DNS resolution, networking, routing aur security controls se guzarti hui server tak pohanchti hai. Server request process karke response browser ko wapas bhejta hai.

**Remember:**

DNS = Address find karta hai

Router = Packets ko aglay network ki taraf forward karta hai

Route Table = Traffic ka raasta batati hai

Internet Gateway = VPC ko internet connectivity provide karta hai

Security Group = Server ke traffic ko control karta hai

EC2 = Application ya website run karta hai

HTTP Response = Website ka data user tak wapas pohanchata hai
