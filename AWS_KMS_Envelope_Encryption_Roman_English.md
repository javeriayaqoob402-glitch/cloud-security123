# AWS KMS Envelope Encryption --- Roman English Notes

## 1. Envelope Encryption kya hai?

Envelope encryption mein **Data Key** actual data/file ko encrypt karti
hai, aur **AWS KMS Key** us Data Key ko encrypt/protect karti hai.

**College analogy:** Student record ek locked box mein hai. File ko lock
karne wali choti key Data Key hai. Us choti key ko protect karne wali
master key KMS Key hai.

-   **Data Key:** Actual data encrypt/decrypt karti hai.
-   **KMS Key:** Data Key ko encrypt/decrypt karne mein use hoti hai.
-   **Encrypted Data Key:** Data Key ka protected version jo encrypted
    data ke saath store ho sakta hai.

KMS key aam tor par large file ko directly encrypt nahi karti.

## 2. Envelope encryption kyun use karte hain?

1.  **Security:** Application ko KMS key material directly handle karne
    ki zaroorat nahi.
2.  **Performance:** Large files ko Data Key se encrypt karna efficient
    hai.
3.  **Central control:** KMS permissions aur lifecycle manage karta hai.
4.  **Audit:** KMS API activity CloudTrail mein record ho sakti hai.
5.  **Scalability:** Har data item ke liye alag Data Key use ki ja sakti
    hai.

## 3. File upload/encryption ke waqt kya hota hai?

Example: College staff S3 mein `student-record.txt` upload karta hai
with SSE-KMS.

1.  S3/application KMS se Data Key request karti hai.
2.  KMS plaintext Data Key aur encrypted Data Key return karta hai.
3.  Plaintext Data Key file ko encrypt karti hai.
4.  Plaintext Data Key use ke baad discard/remove kar di jati hai.
5.  S3 encrypted file aur encrypted Data Key store karta hai.

**Analogy:** Staff file ko choti key se lock karta hai, phir choti key
ko master key se locked safe mein rakh deta hai.

## 4. Jab authorized user file read/download karta hai?

1.  User S3 se object read/download request karta hai.
2.  S3 check karta hai ke user ke paas `s3:GetObject` permission hai.
3.  S3 KMS se encrypted Data Key decrypt karne ki request karta hai.
4.  KMS IAM permissions aur KMS key policy evaluate karta hai.
5.  Access allowed ho to KMS plaintext Data Key return karta hai.
6.  S3 Data Key se file decrypt karta hai.
7.  User ko readable file milti hai.

User ko normally KMS key ya Data Key manually copy karne ki zaroorat
nahi hoti.

## 5. IAM Policy vs KMS Key Policy

### IAM Policy

Identity (user, group, role) ko permissions deti hai.

Example: Staff ko S3 bucket mein object read karne dena.

### KMS Key Policy

Define karti hai ke KMS key ko kaun use ya manage kar sakta hai. Ye KMS
key access ka important central control hai.

SSE-KMS object read ke liye aam tor par: - `s3:GetObject` -
`kms:Decrypt` - KMS key policy mein required authorization

Upload ke liye aam tor par: - `s3:PutObject` - KMS key use permission,
commonly `kms:GenerateDataKey` - KMS key policy mein required
authorization

Exact permissions configuration par depend karti hain. Broad `kms:*`
access avoid karo.

## 6. Separate Key Administration / Separation of Duties

Iska matlab hai key manage karne wala person zaroori nahi ke sensitive
data bhi read kar sake.

**College analogy:** - IT Security Admin: keys create, disable aur
manage karta hai. - Records Officer: approved student records read karta
hai. - Auditor: activity logs check karta hai.

AWS mein separate roles/policies bana kar ye separation apply kar sakte
hain: - Key administrator: key management permissions. - Data
user/application: required encrypt/decrypt permissions. - Auditor:
CloudTrail logs read permissions.

Key administration aur key usage ko alag rakhne se ek compromised
account ka risk kam hota hai.

## 7. KMS Key Lifecycle

Key lifecycle ka matlab key create hone se lekar uske end tak ka safar
hai.

1.  **Create:** Customer managed KMS key create karo.
2.  **Configure:** Alias, key policy, administrators aur users set karo.
3.  **Use:** Applications encryption-related operations mein key use
    karti hain.
4.  **Monitor and Audit:** CloudTrail mein KMS API activity review karo.
5.  **Rotate:** Supported key ke liye rotation configure karo. Rotation
    purane encrypted data ko automatically re-encrypt nahi karti; purana
    key material decrypt ke liye available rehta hai.
6.  **Disable:** Key temporarily disable ki ja sakti hai; cryptographic
    operations fail ho sakti hain.
7.  **Schedule deletion:** Key deletion waiting period ke saath schedule
    hoti hai. Waiting period mein deletion cancel ki ja sakti hai.

**Warning:** Key permanently delete ho jaye to us key se protected data
decrypt nahi ho sakta, jab tak koi doosra valid recovery method/key
available na ho. Delete karne se pehle dependencies aur backups check
karo.

## 8. Quick Revision

-   Envelope encryption = Data Key data encrypt karti hai; KMS Key Data
    Key protect karti hai.
-   Data Key = actual file/data encryption key.
-   KMS Key = Data Key ko protect karne wali key.
-   Upload = Generate Data Key, encrypt file, plaintext key discard,
    encrypted key store.
-   Download = S3 access check, KMS se Data Key decrypt, file decrypt.
-   IAM Policy = identity permissions.
-   KMS Key Policy = key access permissions.
-   Separation of duties = key admin, data user aur auditor ke roles
    alag.
-   Lifecycle = create, configure, use, monitor, rotate, disable,
    schedule deletion.

## 9. Practice Questions

1.  Envelope encryption mein actual file ko kaunsi key encrypt karti
    hai?
2.  KMS Key ka Data Key ke saath kya role hai?
3.  Plaintext Data Key ko use ke baad kyun discard karte hain?
4.  SSE-KMS file download par KMS kya karta hai?
5.  IAM policy aur KMS key policy mein kya difference hai?
6.  `s3:GetObject` aur `kms:Decrypt` kis kaam aate hain?
7.  Key administrator aur data reader ko separate kyun rakhte hain?
8.  KMS key disable aur delete karne mein kya difference hai?
9.  Key rotation ka matlab kya hai?
10. Agar KMS key permanently delete ho jaye to encrypted file ke saath
    kya risk hai?
