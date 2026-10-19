**1️⃣ VPC (Virtual Private Cloud)**

VPC AWS ma hamari apni private network space hoti ha jahan hum EC2 instances, databases or applications rakh sakty han.

*Analogy:* VPC ko ak housing society samjho jis ma hamary apny blocks or security rules hoty han.

**2️⃣ IPv4 Address**

IPv4 ak address hota ha jo network ma device ko identify karta ha.

Example: `192.168.1.10`

*Analogy:* Jis tarah har ghar ka apna address hota ha, isi tarah har device ka IP address hota ha.

**3️⃣ CIDR**

CIDR batata ha ka hamary network ma IP addresses ka range kitna bara ha.

Example: `192.168.1.0/24`

`/24` ma total 256 IP addresses hoty han. AWS ma 5 reserve hoty han, is liya 251 assignable hoty han.

*Analogy:* Society ma kitny gharon ki jagah ha, CIDR hamen iska size batata ha.

**4️⃣ Subnet**

Subnet VPC ka chota sa network area hota ha.

Example:
VPC: `10.0.0.0/16`
Public subnet: `10.0.1.0/24`
Private subnet: `10.0.2.0/24`

*Analogy:* VPC ak society ha or subnet us society ka ak block ha.

**5️⃣ Router**

Router data ko ak network sa dosry network tak pohanchany ma help karta ha.

*Analogy:* Router traffic police ki tarah ha jo data ko sahi rasty par bhejta ha.

**6️⃣ Route Table**

Route table batati ha ka network traffic ko kahan jana ha.

*Analogy:* Google Maps jaisy hamen rasta batata ha, route table data ko rasta batati ha.

**7️⃣ Internet Gateway (IGW)**

Internet Gateway VPC or internet ka connection provide karta ha jab required settings complete hon.

*Analogy:* Ye society ka main gate ha jahan sa internet ki taraf traffic ja sakta ha.

Sirf IGW attach karny sa EC2 public nahi hota. Route table, public IP or security rules bhi sahi hony chahiye.

**8️⃣ Security Group**

Security Group AWS resources ka virtual firewall hota ha jo inbound or outbound traffic control karta ha.

Common ports:
SSH = 22
HTTP = 80
HTTPS = 443

*Analogy:* Building ka security guard jo decide karta ha ka kis ko entry deni ha.

**9️⃣ NACL (Network ACL)**

NACL subnet level par traffic ko allow ya deny karta ha.

*Analogy:* Security Group building ka guard ha or NACL poory block ka security check ha.

Security Group stateful hota ha, jabka NACL stateless hota ha.

**🔟 Public Subnet**

Public subnet ki route table ma Internet Gateway ki taraf route hota ha.

Example: Public website ka EC2 server.

Lekin internet sa access k liya public IP or correct security rules bhi zaroori hoty han.

**1️⃣1️⃣ Private Subnet**

Private subnet ma direct public internet access ka route nahi hota.

Example: Database server.

*Analogy:* Society ka private block jahan public logon ko direct entry nahi hoti.

Yaad rakho: AWS ma separate Public VPC or Private VPC type nahi hoti. Ak hi VPC ma dono subnets ho sakty han.

**1️⃣2️⃣ Availability Zone (AZ)**

Availability Zone AWS Region ka isolated location hota ha jahan data centers hoty han.

Example:
Region: `eu-north-1`
AZs: `eu-north-1a`, `eu-north-1b`

*Analogy:* Region ak shehar ha or AZs us shehar k alag isolated areas ki tarah han.

Multiple AZs use karny sa high availability improve hoti ha.

**💡 Complete Example**

VPC = Society
Subnet = Society ka block
IP Address = Ghar ka address
Route Table = Rasty ka map
Internet Gateway = Main gate
Security Group = Building ka guard
EC2 = Server jo website host karta ha
Private Database = Andar rakha hua secure record room

**Final concept:**
VPC → Subnets → Route Tables → Internet Gateway → Security Rules → AWS Resources
