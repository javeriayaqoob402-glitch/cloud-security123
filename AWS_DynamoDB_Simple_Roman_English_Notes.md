# AWS DynamoDB --- Simple Roman English Notes

## 1. DynamoDB kya hai?

**Amazon DynamoDB** AWS ki fully managed NoSQL database service hai. Is
mein application ka data tables mein store hota hai. AWS infrastructure,
maintenance aur scaling ka bohat saara kaam manage karta hai.

**Analogy:** Socho school ka record room hai. Har student ka record aik
card jaisa hai. DynamoDB mein har record ko **item** aur uski details ko
**attributes** kehte hain.

## 2. Database kya hota hai?

Database aik organized jagah hai jahan data store, manage aur retrieve
kiya jata hai. Example: school ke students ke names, roll numbers aur
classes.

## 3. SQL vs NoSQL

### SQL

-   Data rows aur columns wali tables mein hota hai.
-   Aam tor par defined schema hota hai.
-   Tables ke darmiyan relationships aur joins use ho sakte hain.
-   Examples: MySQL, PostgreSQL, SQL Server.

**Analogy:** School register jahan har student ke liye same columns hon:
Roll No, Name, Class.

### NoSQL

-   Non-relational database approach hai.
-   Data key-value ya document jaisi flexible form mein ho sakta hai.
-   Har item mein same attributes hona zaroori nahi.
-   DynamoDB aik NoSQL database hai.

**Analogy:** Har student ka alag card. Kisi card par phone number ho
sakta hai aur kisi par nahi.

## 4. DynamoDB ke basic terms

Example table: `Students`

  StudentID   Name   Class   City
  ----------- ------ ------- ------------
  S101        Ali    10      Lahore
  S102        Sara   9       Gujranwala

-   **Table:** Data ka collection, e.g. `Students`.
-   **Item:** Aik complete record, e.g. Sara ka record.
-   **Attribute:** Item ki aik detail, e.g. `Name`, `Class`, `City`.
-   **Primary Key:** Key jo har item ko uniquely identify karti hai.

**Analogy:** Table = school register; Item = student ka record;
Attribute = student ki detail; Primary Key = unique roll number.

## 5. Primary Key, Partition Key aur Sort Key

DynamoDB mein primary key ki 2 types hain:

### A. Simple Primary Key

Sirf **Partition Key** hoti hai.

Example: `Students` table mein `StudentID` partition key hai. Har
student ka StudentID unique hoga.

### B. Composite Primary Key

Do parts hotay hain: 1. **Partition Key** 2. **Sort Key**

Example: `Orders` table:

  CustomerID (Partition Key)   OrderDate (Sort Key)   Product
  ---------------------------- ---------------------- ----------
  C01                          2026-11-01             Laptop
  C01                          2026-11-03             Mouse
  C02                          2026-11-02             Keyboard

-   **Partition Key:** DynamoDB is value ko use karke decide karta hai
    ke item kis partition mein store hoga. Same partition-key value wale
    items aik logical group mein aate hain.
-   **Sort Key:** Same partition key ke items ko alag identify aur order
    karne mein help karti hai. Is se aik customer ke multiple orders
    store ho sakte hain.

**Important:** - Simple key mein partition key ki value unique honi
chahiye. - Composite key mein partition key + sort key ka combination
unique hona chahiye. - Sort key optional hai.

**Analogy:** CustomerID batata hai kis customer ka data hai; OrderDate
us customer ke orders ko alag karti hai.

## 6. DynamoDB Data Types

### A. Scalar Types --- aik single value

-   **String (S):** Text, e.g. `"Javeria"`
-   **Number (N):** Number, e.g. `25` or `99.5`
-   **Binary (B):** Binary data
-   **Boolean (BOOL):** `true` ya `false`
-   **Null (NULL):** Value null hai

### B. Document Types --- structured/multiple values

-   **List (L):** Ordered list, e.g. `["AWS", "Linux", "Python"]`
-   **Map (M):** Key-value details ka group,
    e.g. `{ "city": "Gujranwala", "level": "Beginner" }`

### C. Set Types --- unique values ka group

-   **String Set (SS):** Unique text values, e.g. `{"AWS", "Linux"}`
-   **Number Set (NS):** Unique numbers, e.g. `{10, 20, 30}`
-   **Binary Set (BS):** Unique binary values

**Note:** Partition key aur sort key ka data type String, Number ya
Binary ho sakta hai. List, Map aur Set ko key ke taur par use nahi kar
sakte. Sets empty nahi ho sakte aur duplicate values store nahi karte.

## 7. AWS Console mein DynamoDB table create karna

1.  AWS Management Console open karein.
2.  Search bar mein `DynamoDB` likhein aur service open karein.
3.  **Tables** par ja kar **Create table** click karein.
4.  Table name dein, e.g. `Students`.
5.  Partition key ka naam dein, e.g. `StudentID`, aur type `String`
    select karein.
6.  Zaroorat ho to sort key add karein.
7.  Beginner practice ke liye baqi settings default rehne dein.
8.  **Create table** click karein.
9.  Table active ho jaye to **Explore table items** open karein aur
    **Create item** se record add karein.

Example item: - `StudentID` = `S101` - `Name` = `Ali` - `Class` = `10` -
`City` = `Lahore`

**Important:** Table create karne se pehle primary key design soch lein.
DynamoDB table ki primary key ko baad mein simply edit nahi kiya ja
sakta. Different key design chahiye ho to aam tor par new table aur data
migration ki zaroorat hoti hai.

## 8. Quick Revision

  -----------------------------------------------------------------------
  Term                                Simple meaning
  ----------------------------------- -----------------------------------
  Database                            Data store karne ki organized jagah

  SQL                                 Relational database approach

  NoSQL                               Non-relational database approach

  DynamoDB                            AWS managed NoSQL database

  Table                               Records ka collection

  Item                                Aik complete record

  Attribute                           Record ki aik detail

  Primary Key                         Item ko uniquely identify karti hai

  Partition Key                       Data ko partition/group karne mein
                                      help karti hai

  Sort Key                            Same partition key ke items ko
                                      distinguish/order karti hai
  -----------------------------------------------------------------------

## 9. One-minute Analogy

Socho aik online store hai: - `Customers` = customers ki table -
`Orders` = orders ki table - Har customer/order ka record = item - Name,
city, price, date = attributes - CustomerID = partition key - OrderDate
= sort key - DynamoDB = online store ka digital record room

**Remember:** Table = collection, Item = record, Attribute = detail,
Primary Key = unique identity.
