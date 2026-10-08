# EC2 SSH Hardening — Easy Roman Urdu Notes with Analogies

## 1. SSH kya hai?

**SSH (Secure Shell)** ek secure method hai jiske through hum remotely apne EC2 instance mein login karke terminal se kaam karte hain.

Example:

```bash
ssh -i mykey.pem username@EC2_PUBLIC_IP
```

### 🏠 Analogy

Socho **EC2 tumhara ghar hai**.

- EC2 = ghar
- SSH = ghar ka main door
- SSH key = ghar ki key
- Security Group = gate par security guard
- Trusted IP = jin logon ko andar aane ki permission hai

---

# 2. SSH Hardening kya hai?

**SSH Hardening** ka matlab hai SSH ko zyada secure banana taake unauthorized person EC2 mein login na kar sake.

Simple words:

> **SSH Hardening = EC2 ke SSH door ko secure karna.**

### 🏠 Analogy

Ghar ko secure karne ke liye:

- Strong lock lagao
- Unknown logon ko entry na do
- Extra doors band karo
- Sirf trusted logon ko entry do

Bilkul isi tarah EC2 ke SSH ko harden karte hain.

---

# 3. Direct Root SSH Login avoid karo

`root` Linux ka highest-privilege account hota hai.

Direct root se SSH login karne ke bajaye normal user use karo aur zaroorat par `sudo` use karo.

Example:

```bash
sudo apt update
```

### Kyun?

Agar attacker ko direct root access mil gaya to uske paas system par bohat high-level control ho sakta hai.

### 🏠 Analogy

Socho ghar ki **master key** hai.

Tum har visitor ko master key nahi dogi.

Instead:
- Normal user = normal key
- `sudo` = zaroorat par special permission

---

# 4. SSH Keys use karo

SSH key authentication password ke muqable mein secure method hai.

Example:

```bash
ssh -i mykey.pem username@EC2_PUBLIC_IP
```

AWS EC2 mein `.pem` private key ko bohat carefully protect karna chahiye.

Linux/macOS par:

```bash
chmod 400 mykey.pem
```

### 🔑 Analogy

SSH key ko ghar ki **special physical key** samjho.

Jiske paas key hai woh door open kar sakta hai.

Isliye:

> **Private SSH key kisi ke saath share nahi karni.**

---

# 5. Password Authentication

Agar tum successfully SSH key se login kar rahi ho, to password-based SSH login ko disable kiya ja sakta hai.

Common SSH setting:

```text
PasswordAuthentication no
```

### Kyun?

Isse password guessing aur brute-force attacks ka risk kam hota hai.

### ⚠️ Important

Password authentication disable karne se **pehle SSH key se new login test karo**.

### 🏠 Analogy

Pehle check karo:

> "Meri new key se door khul raha hai?"

Agar haan ✅

Tab old lock ko disable karo.

---

# 6. Port 22 kya hai?

SSH normally **Port 22** use karta hai.

```text
SSH → Port 22
```

AWS Security Group decide karta hai ke port 22 tak kaun pohanch sakta hai.

### ❌ Less Secure Example

```text
SSH | TCP | 22 | 0.0.0.0/0
```

Iska matlab internet se kahin se bhi SSH connection attempts aa sakti hain.

### ✅ Better

Sirf apne trusted public IP ko allow karo:

```text
SSH | TCP | 22 | YOUR_PUBLIC_IP/32
```

### 🏠 Analogy

Society ka gate sab ke liye open rakhne ke bajaye:

> "Sirf meri known list ke log andar aa sakte hain."

---

# 7. Khud ko Lock Out kaise na karo?

Ye SSH hardening ka bohat important part hai.

**Lockout** ka matlab:

> Tumne security settings change kar di aur ab tum khud EC2 mein SSH se login nahi kar pa rahi.

### Lockout ki common mistakes

- Port 22 close kar dena
- Apna IP block kar dena
- SSH configuration galat kar dena
- Apna only login user remove kar dena
- Password login disable kar dena before testing SSH key
- Security Group mein wrong rule laga dena

---

## 🏠 Golden Rule

> **Pehle naya door test karo, phir purana door lock karo.**

### Safe process

**Step 1:** Current SSH session open rakho.

**Step 2:** Ek security change karo.

**Step 3:** New terminal kholo.

**Step 4:** New SSH connection test karo.

**Step 5:** Confirm karo ke login ho raha hai.

**Step 6:** Ab old session close karo.

### Example

Terminal 1:

```text
EC2 mein already logged in
```

Terminal 2:

```bash
ssh -i mykey.pem username@EC2_PUBLIC_IP
```

Agar Terminal 2 se login successful hai:

```text
✅ New access works
```

Tab tum old session close kar sakti ho.

---

# 8. SSH Configuration Check karo

SSH configuration change karne ke baad syntax check kar sakti ho:

```bash
sudo sshd -t
```

Agar koi output nahi aata, usually configuration syntax valid hoti hai.

### 🏠 Analogy

Ye aisa hai jaise ghar ka lock install karne ke baad check karna:

> "Lock properly fit hua hai ya nahi?"

---

# 9. SSH Users

Sirf un users ko SSH access milna chahiye jinhein actually zaroorat hai.

Users/accounts review karne ke liye:

```bash
cat /etc/passwd
```

⚠️ System accounts ko bina samjhe remove mat karo, kyun ke kuch accounts OS ya applications ke liye required ho sakte hain.

### 🏠 Analogy

Ghar mein har person ko key dene ki zaroorat nahi.

Sirf required family members ko key do.

---

# 10. SSH Service Check karna

SSH service running hai ya nahi:

```bash
sudo systemctl status ssh
```

Kuch Linux distributions mein service ka naam:

```bash
sudo systemctl status sshd
```

### 🏠 Analogy

Ye check karna hai:

> "Ghar ka main door service active hai ya nahi?"

---

# 11. Listening Ports Check karna

EC2 par kaun se ports listening hain:

```bash
sudo ss -tuln
```

Ye command tumhein batati hai ke system par kaun se network ports listening state mein hain.

### 🏠 Analogy

Socho ghar mein multiple doors/windows hain.

Tum check kar rahi ho:

> "Kaun se doors currently open/listening hain?"

Agar koi unnecessary door ho to usko secure ya disable karna consider kar sakti ho.

---

# 12. AWS Security Group + SSH

SSH access ka simple flow:

```text
Internet
   ↓
Security Group
   ↓
EC2 Instance
   ↓
SSH Service
```

### Security Group

AWS level par network traffic ko control karta hai.

### SSH Configuration

EC2 ke andar SSH authentication aur behavior ko control karti hai.

### 🏠 Analogy

- Security Group = society ka main gate guard
- SSH = ghar ka door
- SSH key = ghar ki key

Agar guard ne entry allow hi nahi ki, to ghar ke door tak koi nahi pohanch sakta.

---

# 13. SSH Hardening Checklist

- [ ] SSH keys use karo
- [ ] Private `.pem` key secret rakho
- [ ] Security Group mein port 22 restrict karo
- [ ] SSH ke liye `0.0.0.0/0` avoid karo jab possible ho
- [ ] Direct root SSH login avoid karo
- [ ] SSH key login test karne ke baad password authentication disable karo
- [ ] Unnecessary SSH users/access remove karo
- [ ] `sshd -t` se configuration check karo
- [ ] Changes karte waqt current SSH session open rakho
- [ ] Second SSH connection test karo
- [ ] EC2 OS ko regularly update karo

---

# 14. Important Commands

### SSH connection

```bash
ssh -i mykey.pem username@EC2_PUBLIC_IP
```

### Private key secure permissions

```bash
chmod 400 mykey.pem
```

### SSH configuration test

```bash
sudo sshd -t
```

### SSH service check

```bash
sudo systemctl status ssh
```

### Listening ports

```bash
sudo ss -tuln
```

### System update

```bash
sudo apt update
sudo apt upgrade
```

---

# 15. Quick Revision

| Topic | Easy Meaning | Analogy |
|---|---|---|
| SSH | EC2 mein remote secure login | Ghar ka main door |
| SSH Key | Login authentication key | Ghar ki key |
| Port 22 | Default SSH port | Door ka entry number |
| Root Login | Highest privileges wala login | Master key |
| Password Authentication | Password se login | Normal lock |
| Security Group | AWS network access control | Society ka gate guard |
| Trusted IP | Allowed known IP | Known visitor |
| SSH Hardening | SSH ko secure banana | Ghar ko secure karna |
| Lockout | Khud login na kar pana | Apne ghar ke bahar reh jana |

---

# 16. Easy Memory Trick

## K + R + P + I + T

### K — Key 🔑
SSH keys use karo.

### R — Root 🚫
Direct root SSH login avoid karo.

### P — Password 🔒
Key login test karne ke baad password login disable karo.

### I — IP 🌐
SSH ko trusted IPs tak restrict karo.

### T — Test ✅
New SSH connection ko test karo before old access close karne ke.

> **K + R + P + I + T = Secure SSH**

---

# 17. One-Line Summary

> **EC2 SSH Hardening = SSH ko secure karna by using keys, restricting IPs, avoiding root/password login, and testing access before making changes.**

### 🏠 Final Analogy

EC2 = **Ghar**

Security Group = **Society ka gate**

SSH = **Ghar ka door**

SSH Key = **Ghar ki key**

Root = **Master key**

Trusted IP = **Known visitor**

SSH Hardening = **Ghar ke doors aur keys ko properly secure karna**

Lockout prevention = **Naya door test kiye baghair purana door lock na karna.**
