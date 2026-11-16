# Cryptography aur AWS KMS --- Roman English Notes

## 1. Cryptography kya hai?

Cryptography information ko secure karne ka method hai. Iska maqsad data
ko unauthorized logon se protect karna hota hai.

**College analogy:** Socho college ke principal ke paas students ke
marks ka record hai. Principal chahta hai ke sirf authorized teachers
aur staff hi record read kar saken. Is liye record ko secret code mein
convert kiya jata hai. Ye process cryptography kehlata hai.

## 2. Plaintext aur Ciphertext

-   **Plaintext:** Original readable data, jaise `Student marks = 90`.
-   **Ciphertext:** Encrypted form jo normal insan ko samajh nahi aati,
    jaise `X7#K9P...`.

**Analogy:** Plaintext woh letter hai jo aap normally parh sakte ho.
Ciphertext woh letter hai jo locked secret code mein likha gaya ho.

## 3. Encryption aur Decryption

-   **Encryption:** Plaintext ko ciphertext mein convert karna.
-   **Decryption:** Ciphertext ko wapas plaintext mein convert karna.
-   **Key:** Secret ya cryptographic key jo encryption/decryption mein
    use hoti hai.

**College analogy:** Teacher marks ki file ko lock kar deta hai. Lock
karna encryption hai. Correct key se file kholna decryption hai.

## 4. AWS KMS kya hai?

**AWS KMS (Key Management Service)** AWS ki service hai jo cryptographic
keys ko create, store/protect, manage aur control karne mein help karti
hai. KMS keys ka use data ko encrypt/decrypt karne ke liye hota hai. KMS
key material ko aap normally directly dekh ya export nahi karte.

**College analogy:** College mein ek secure key room imagine karo.
Principal decide karta hai: - Kaunsi key banani hai. - Kis staff member
ko key use karne ki permission deni hai. - Key kab rotate ya disable
karni hai. - Kis ne key use ki aur kab use ki.

AWS KMS isi tarah keys ka management karta hai.

## 5. KMS ke important kaam: Create, Control, Audit

### Create

KMS mein cryptographic key create karna.

**Analogy:** College ke record room ke liye naya secure lock/key banana.

### Control

IAM policies aur KMS key policies ke zariye control karna ke kaun key ko
use ya manage kar sakta hai. Permissions least privilege ke principle ke
mutabiq deni chahiye.

**Analogy:** Har teacher ko master key nahi milti. Sirf authorized staff
ko required access milta hai.

### Audit

AWS CloudTrail ke through KMS API activity ka record dekh sakte hain,
jaise kis identity ne key use karne ki request ki aur kab.

**Analogy:** College ke gate par entry register hota hai jahan record
hota hai ke kis ne record room access kiya.

**Yaad rakho:** KMS keys manage karta hai; CloudTrail KMS-related API
activity ka audit trail provide karta hai.

## 6. Encryption at Rest vs Encryption in Transit

### Encryption at Rest

Jab data storage mein saved ho aur us waqt encrypted ho.

Examples: - S3 bucket mein stored files. - EBS volume par stored data. -
RDS database mein stored data.

**College analogy:** Students ke records filing cabinet mein rakhe hain
aur cabinet locked hai. Ye encryption at rest hai.

### Encryption in Transit

Jab data ek jagah se doosri jagah network ke through travel kar raha ho
aur encrypted ho.

Example: - Browser aur website ke darmiyan HTTPS/TLS connection. -
Application aur server ke darmiyan secure network communication.

**College analogy:** Teacher marks ki file doosre campus bhej raha hai.
Raaste mein file ko secure envelope mein rakha gaya hai. Ye encryption
in transit hai.

  -----------------------------------------------------------------------
  Difference              At Rest                 In Transit
  ----------------------- ----------------------- -----------------------
  Data kahan hai?         Storage mein            Network par travel kar
                                                  raha hai

  Example                 S3, EBS, RDS            HTTPS/TLS

  Maqsad                  Stored data protect     Transfer hota data
                          karna                   protect karna
  -----------------------------------------------------------------------

## 7. Client-Side Encryption

Client-side encryption mein data AWS service ko bhejne se pehle client
side par encrypt hota hai. Client application encryption perform karti
hai; keys ka control client/application ke paas reh sakta hai, design
par depend karta hai.

**College analogy:** Teacher file ko college ke bahar bhejne se pehle
apne office mein encrypt karta hai. File college server tak encrypted
form mein pohanchti hai.

**Important:** Client-side encryption aur server-side encryption ek
jaisi cheezein nahi hain. Server-side encryption mein AWS service data
receive karne ke baad storage se pehle encryption handle karti hai.
Exact key handling selected service aur configuration par depend karti
hai.

## 8. Symmetric Encryption

Symmetric encryption mein encryption aur decryption ke liye **same
secret key** use hoti hai.

Example: AES.

**College analogy:** Principal aur authorized teacher ke paas same
locker ki duplicate key hai. Principal file lock karta hai aur teacher
same type ki key se unlock karta hai.

**Benefit:** Fast hoti hai aur large amounts of data encrypt karne ke
liye useful hai.

**Challenge:** Secret key ko securely share aur protect karna zaroori
hai.

## 9. Asymmetric Encryption

Asymmetric encryption mein **key pair** hota hai: - Public key: share ki
ja sakti hai. - Private key: secret rakhi jati hai.

RSA aur ECC is category ke examples hain.

Encryption use case mein public key se encrypt kiya gaya data
corresponding private key se decrypt ho sakta hai. Digital signatures
mein private key se sign aur public key se signature verify kiya jata
hai.

**College analogy:** College ka locked suggestion box imagine karo. Box
mein koi bhi message daal sakta hai kyun ke opening slot public hai,
lekin box kholne ki private key sirf principal ke paas hai.

**Benefit:** Secret key ko pehle se dono parties ke darmiyan share karna
zaroori nahi hota.

**Challenge:** Symmetric encryption ke muqable mein aam tor par slower
hoti hai.

## 10. Symmetric vs Asymmetric

  -----------------------------------------------------------------------
  Feature                 Symmetric               Asymmetric
  ----------------------- ----------------------- -----------------------
  Keys                    Same secret key         Public + private key
                                                  pair

  Speed                   Generally faster        Generally slower

  Example                 AES                     RSA, ECC

  Common use              Bulk data encryption    Key exchange, digital
                                                  signatures, selected
                                                  encryption use cases

  Analogy                 Same locker key         Public lock / private
                                                  unlock key
  -----------------------------------------------------------------------

## 11. AWS KMS key types --- important distinction

AWS KMS mein keys ko mukhtalif angles se samjha jata hai:

### A. Key ownership / management

-   **AWS owned keys:** AWS manage karta hai; customer ke account mein
    normal customer-managed key ki tarah manage nahi hoti.
-   **AWS managed keys:** AWS service ke liye AWS manage karta hai;
    customer account mein service-specific alias ke sath nazar aa sakti
    hain.
-   **Customer managed keys:** Customer AWS KMS mein create aur manage
    karta hai. Policies, permissions, rotation settings aur lifecycle
    par zyada control hota hai.

### B. Key type / cryptographic use

-   **Symmetric KMS key:** Same key material encryption aur decryption
    ke liye. AWS KMS mein common data encryption use case.
-   **Asymmetric KMS key:** Public aur private key pair.
    Encryption/decryption ya signing/verification ke liye key usage ke
    mutabiq.
-   **HMAC KMS key:** Message authentication code generate aur verify
    karne ke liye; normal encryption/decryption key nahi.

### C. Key material origin

-   **AWS_KMS:** Key material AWS KMS generate aur manage karta hai.
-   **EXTERNAL:** Customer apna key material import kar sakta hai,
    supported key types aur conditions ke under.
-   **Custom key store:** AWS CloudHSM-backed custom key store mein keys
    use ki ja sakti hain.

**Note:** Ye categories ek doosre ka replacement nahi.
Ownership/management, cryptographic key type aur key material origin
alag concepts hain.

## 12. Quick revision

-   Cryptography = data ko secure karne ke techniques.
-   Plaintext = readable original data.
-   Ciphertext = encrypted data.
-   Encryption = readable data ko encrypted banana.
-   Decryption = encrypted data ko readable banana.
-   KMS = AWS par cryptographic keys manage karne ki service.
-   Create = key banana.
-   Control = permissions aur access manage karna.
-   Audit = key-related activity ka record dekhna, commonly CloudTrail
    se.
-   At rest = storage mein saved data protect karna.
-   In transit = network par travel karte data ko protect karna.
-   Client-side = AWS ko bhejne se pehle client par encryption.
-   Symmetric = same secret key.
-   Asymmetric = public/private key pair.

## 13. Practice questions

1.  Plaintext aur ciphertext mein kya difference hai?
2.  Encryption aur decryption kya hain?
3.  AWS KMS ka main purpose kya hai?
4.  KMS mein Create, Control aur Audit ka kya matlab hai?
5.  S3 mein saved encrypted file at rest hai ya in transit?
6.  HTTPS data ko kis state mein protect karta hai?
7.  AES symmetric hai ya asymmetric?
8.  RSA asymmetric hai ya symmetric?
9.  Client-side encryption kab hoti hai?
10. CloudTrail KMS activity ke audit mein kaise help karta hai?
