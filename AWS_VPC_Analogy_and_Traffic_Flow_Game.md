# AWS VPC — Society Analogy + Traffic Flow Game

## 1. VPC kya hota hai?

**VPC (Virtual Private Cloud)** AWS ke andar tumhara apna private network hota hai.

### Society wali analogy

Socho tumne ek private society banayi hai:

- **VPC:** Tumhari poori private society.
- **Subnet:** Society ke andar alag block ya street.
- **EC2:** Society ke andar server wala ghar.
- **Laptop:** Tumhara ghar jahan se website open hoti hai.
- **ISP:** Internet Service Provider — internet connection deta hai.
- **Internet Gateway (IGW):** Society ka internet se connect hone wala main gate.
- **Route Table:** Direction board — traffic ko batata hai kahan jana hai.
- **NACL:** Subnet ke entrance ka checkpoint; subnet-level traffic filter karta hai.
- **Security Group:** EC2 ka personal guard / virtual firewall; instance-level traffic filter karta hai.

**Yaad rakho:** VPC society hai, subnet uska block hai, aur EC2 us block ke andar server wala ghar hai.

## 2. Laptop se EC2 tak request kaise jati hai?

Example: Tum browser mein `http://EC2-PUBLIC-IP` open karti ho.

1. **Laptop:** Browser HTTP request send karta hai.
2. **ISP:** Tumhari internet connection request ko internet tak pohanchati hai.
3. **IGW:** VPC ko internet ke saath communicate karne ka raasta deta hai.
4. **Route Table:** Check hota hai ke internet traffic ke liye route available hai ya nahi. Public subnet mein aam tor par `0.0.0.0/0 → IGW` route hota hai.
5. **NACL:** Subnet level par inbound aur outbound rules check karta hai.
6. **Security Group:** EC2 ke inbound rules check karta hai. Misal ke taur par HTTP port 80 allow hai ya nahi.
7. **EC2 + Nginx:** Request server tak pohanchti hai. Nginx website ka response deta hai.
8. **Response:** Response internet ke zariye wapas tumhare laptop tak aata hai.

### Simple flow

```text
Your Laptop
    |
    v
ISP / Internet
    |
    v
Internet Gateway (IGW)
    |
    v
VPC
    |
    v
Route Table (route available?)
    |
    v
NACL (subnet firewall)
    |
    v
Security Group (EC2 firewall)
    |
    v
EC2 Server -> Nginx -> Website
```

## 3. Har component ka kaam

| Component | Society analogy | Main kaam |
|---|---|---|
| VPC | Private society | AWS mein isolated network |
| Subnet | Society ka block | VPC ka IP range wala section |
| ISP | Internet provider | Internet connection |
| IGW | Society ka internet gate | VPC aur internet ke darmiyan gateway |
| Route Table | Direction board | Traffic ka next route decide karta hai |
| NACL | Block ka checkpoint | Subnet-level inbound/outbound filtering |
| Security Group | Ghar ka guard | EC2-level traffic filtering |
| EC2 | Server wala ghar | Application / website run karta hai |

## 4. NACL vs Security Group

| Feature | NACL | Security Group |
|---|---|---|
| Applies to | Subnet | EC2 network interface |
| Rules | Allow and Deny | Allow only |
| Stateful? | Stateless | Stateful |
| Return traffic | Rules mein allow karna hota hai | Allowed connection ka response automatically allowed hota hai |

- **NACL:** Society ke block ka checkpoint.
- **Security Group:** Tumhare server ke ghar ka guard.

## 5. Internet se EC2 tak kya required hai?

Public internet se EC2 website access karne ke liye aam tor par:

- EC2 ke paas public IPv4 address ya suitable public connectivity ho.
- EC2 public subnet mein ho.
- Subnet ki Route Table mein `0.0.0.0/0 → IGW` route ho.
- NACL required traffic allow kare.
- Security Group inbound mein required port allow ho, jaise HTTP `80` aur HTTPS `443`.
- EC2 par Nginx jaisa web server running ho.

**Security tip:** SSH port 22 ko `0.0.0.0/0` se open na rakho. Apne trusted IP address tak restrict karo.

## 6. VPC Traffic Flow — Mini Game

Pehle khud answers do. Answer key neeche hai.

### Level 1 — Identify the component

1. Tumhari private AWS society ka naam kya hai?
2. Society ko internet se connect karne wala gate kaunsa hai?
3. Traffic ko direction dene wala board kaunsa hai?
4. Subnet level par traffic filter kaun karta hai?
5. EC2 level par virtual firewall kaunsa hai?
6. Website host karne wala server kaunsa hai?

### Level 2 — Situation game

**Situation A:** Website port 80 par chal rahi hai, lekin Security Group mein inbound HTTP allow nahi. Kya website internet se open hogi?

**Situation B:** Security Group port 80 allow karta hai, lekin Route Table mein IGW ka default route nahi. Kya public internet se request EC2 tak aa sakti hai?

**Situation C:** NACL inbound HTTP allow karta hai, lekin stateless NACL mein required outbound response traffic allow nahi. Kya response wapas ja sakta hai?

**Situation D:** Security Group port 22 ko sirf tumhare public IP ke liye allow karta hai. Kya ye `0.0.0.0/0` se SSH open karne ke muqable mein safer hai?

### Answer key

<details>
<summary>Level 1 answers</summary>

1. VPC
2. Internet Gateway (IGW)
3. Route Table
4. Network ACL (NACL)
5. Security Group
6. EC2

</details>

<details>
<summary>Level 2 answers</summary>

- **A:** Nahi. Security Group inbound HTTP allow nahi kar raha.
- **B:** Nahi. Public internet ka route available nahi.
- **C:** Response fail ho sakta hai. NACL stateless hai, is liye return traffic ke liye bhi suitable rule chahiye.
- **D:** Haan. SSH ko trusted IP tak restrict karna zyada secure hai.

</details>

## 7. Quick revision

- **VPC =** Private society
- **Subnet =** Society ka block
- **IGW =** Internet ka gate
- **Route Table =** Direction board
- **NACL =** Subnet checkpoint
- **Security Group =** EC2 ka guard
- **EC2 =** Server
- **Nginx =** Website serve karne wala web server

**One-line memory trick:** IGW raasta deta hai, Route Table direction batati hai, NACL subnet ko filter karta hai, Security Group EC2 ko protect karta hai, aur EC2 website host karta hai.
