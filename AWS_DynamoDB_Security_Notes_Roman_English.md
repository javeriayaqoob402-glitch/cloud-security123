# AWS DynamoDB & Security Notes

## Roman English + Real-Life Analogies

------------------------------------------------------------------------

## 1. What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed NoSQL database service provided by
AWS.

It stores data in **Tables, Items, and Attributes**.

### Real-life analogy: School register

-   **Table:** Complete student register.
-   **Item:** One student's complete record (one row).
-   **Attributes:** Individual details such as Name, Age, Class, and
    Email.

  StudentID   Name      Age City
  ----------- ------- ----- -----------
  101         Ali        20 Lahore
  102         Sara       22 Islamabad
  103         Ahmed      21 Karachi

Here, `Students` is the table, each row is an item, and each column is
an attribute.

------------------------------------------------------------------------

## 2. CRUD Operations

CRUD means the four basic operations used to manage database data.

  -----------------------------------------------------------------------
  Operation         Meaning           DynamoDB example  Analogy
  ----------------- ----------------- ----------------- -----------------
  Create            Add new data      `PutItem`         Add a new student
                                                        to the register

  Read              Retrieve data     `GetItem`,        Look up a
                                      `Query`, `Scan`   student's record

  Update            Change existing   `UpdateItem`      Change a
                    data                                student's address

  Delete            Remove data       `DeleteItem`      Remove a record
  -----------------------------------------------------------------------

**Remember:** CRUD = Create, Read, Update, Delete.

------------------------------------------------------------------------

## 3. DynamoDB Data Types

DynamoDB supports several data types.

### Scalar types

  Type               Meaning         Example
  ------------------ --------------- ------------------
  String (`S`)       Text            `"Javeria"`
  Number (`N`)       Numeric value   `25`
  Binary (`B`)       Binary data     File/image bytes
  Boolean (`BOOL`)   True or false   `true`
  Null (`NULL`)      Null value      `null`

### Document types

**List (`L`)** stores an ordered collection of values.

``` json
{
  "Skills": ["AWS", "Linux", "Python"]
}
```

**Map (`M`)** stores nested attributes as key-value pairs.

``` json
{
  "Address": {
    "City": "Gujranwala",
    "Country": "Pakistan"
  }
}
```

### Set types

-   String Set (`SS`)
-   Number Set (`NS`)
-   Binary Set (`BS`)

Sets store unique values of the relevant type.

**Analogy:** A school register may contain text, numbers, yes/no values,
lists, and grouped information. These are different data types.

------------------------------------------------------------------------

## 4. Primary Key

A primary key uniquely identifies an item in a DynamoDB table. DynamoDB
supports two primary-key designs.

### A. Simple primary key (Partition Key only)

A table can use one Partition Key.

  StudentID (Partition Key)   Name
  --------------------------- -------
  101                         Ali
  102                         Sara
  103                         Ahmed

`StudentID` identifies each student. Its value must be unique for every
item in this table.

**Analogy:** A student's unique registration number.

### B. Composite primary key (Partition Key + Sort Key)

A table can use both a Partition Key and a Sort Key.

  CustomerID (Partition Key)   OrderID (Sort Key)   Product
  ---------------------------- -------------------- ----------
  C101                         O001                 Laptop
  C101                         O002                 Mobile
  C102                         O003                 Keyboard

The combination of Partition Key and Sort Key must be unique.

### What is a Partition Key?

The Partition Key is the key attribute DynamoDB uses to determine the
partition where an item is stored. It is also part of the item's primary
key.

**Analogy:** CustomerID groups a customer's orders together.

### What is a Sort Key?

The Sort Key is an optional second key attribute. It lets you
distinguish and query related items that share the same Partition Key,
including by sort-key conditions.

**Analogy:** OrderID distinguishes the different orders belonging to the
same customer.

------------------------------------------------------------------------

## 5. Secondary Indexes

A Secondary Index lets you query table data using an alternative key.

Imagine your table's primary key is `CustomerID`, but you also want to
find customers by `City`. An index on City can support that access
pattern.

### Global Secondary Index (GSI)

-   Can use a different Partition Key from the base table.
-   Can also have its own optional Sort Key.
-   Supports an alternative query pattern.

### Local Secondary Index (LSI)

-   Uses the same Partition Key as the base table.
-   Uses a different Sort Key.
-   Must be created when the table is created.

**Analogy:** - GSI: A separate way to look up records, such as searching
by City. - LSI: Keep the same customer grouping but organize or query
that customer's records using another sort field.

------------------------------------------------------------------------

## 6. On-Demand vs Provisioned Capacity

DynamoDB offers two read/write capacity modes.

### A. On-Demand

-   AWS automatically accommodates read and write request traffic.
-   You do not configure RCU and WCU capacity in advance.
-   Useful for unpredictable or variable workloads.
-   Charges are based on requests.

**Analogy:** A restaurant where you pay for the meals you actually
order.

### B. Provisioned

-   You specify read capacity units (RCU) and write capacity units
    (WCU).
-   You plan capacity for the expected workload.
-   Useful when traffic is predictable.
-   You can enable auto scaling to adjust provisioned capacity.

**Analogy:** Booking a set number of meals in advance.

  -----------------------------------------------------------------------
  On-Demand                           Provisioned
  ----------------------------------- -----------------------------------
  No manual RCU/WCU capacity planning You configure RCU/WCU

  Adapts to changing request traffic  Capacity is planned for expected
                                      traffic

  Pay per request                     Pay for provisioned capacity
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 7. Encryption at Rest

Encryption at Rest protects data while it is stored in DynamoDB.

DynamoDB encrypts data at rest by default using AWS-owned encryption
keys. You can also choose supported AWS KMS key options.

**Analogy:** Important documents are stored in a locked cabinet. The
lock represents encryption protecting stored data.

**Remember:** At Rest = Data is saved/stored.

------------------------------------------------------------------------

## 8. Encryption in Transit

Encryption in Transit protects data while it moves between an
application and DynamoDB over a network.

DynamoDB uses HTTPS/TLS to protect network communication.

**Analogy:** You send a confidential letter in a protected, sealed
envelope so its contents are not easily read while travelling.

  Encryption at Rest               Encryption in Transit
  -------------------------------- -------------------------------------
  Protects stored data             Protects data moving over a network
  Data on storage                  Data travelling between systems
  AWS-managed or KMS key options   HTTPS/TLS

------------------------------------------------------------------------

## 9. DynamoDB Backups

A backup is a recoverable copy of data that helps protect against
accidental deletion or unwanted changes.

### On-demand backup

You manually create a backup when you need one. It is retained until you
delete it.

### Point-in-Time Recovery (PITR)

PITR allows recovery to a selected point in time within the supported
recovery window.

**Analogy:** Keep a safe copy of the school register. If the original is
damaged or records are accidentally changed, you can restore from a
backup.

**Important:** Backup and encryption are different controls: - Backup
helps with recovery. - Encryption helps protect confidentiality.

------------------------------------------------------------------------

## 10. Least Privilege Principle

Least Privilege means giving a user, application, or service only the
permissions needed to do its job --- no unnecessary permissions.

### Example

  User         Required permission
  ------------ -------------------------------
  Employee A   Read data
  Employee B   Read and write data
  Employee C   Delete data, only if required

If Employee A only needs to read data, do not give them permission to
delete it.

### AWS IAM example

If an application only needs to retrieve individual DynamoDB items,
grant only the required actions, such as: - `dynamodb:GetItem` -
`dynamodb:Query` (only if the application needs queries)

Do not grant `PutItem`, `UpdateItem`, or `DeleteItem` unless they are
genuinely required. Also restrict the policy's Resource to the required
table or index.

**Analogy:** A bank cashier gets access to the tasks needed for their
job, not administrator access to the entire bank system.

### Why is Least Privilege important?

-   Reduces security risks.
-   Limits unauthorized actions.
-   Helps protect sensitive data.
-   Reduces potential damage if credentials are compromised.

------------------------------------------------------------------------

## 11. Complete Security Example

Imagine an online shopping application stores customer and order
information in DynamoDB.

1.  **CRUD:** Create, read, update, and delete records as required.
2.  **Data Types:** Store names, email addresses, numbers, lists, and
    nested address details.
3.  **Partition Key:** Use `CustomerID` to identify and group a
    customer's records.
4.  **Sort Key:** Use `OrderID` to distinguish that customer's orders.
5.  **Secondary Index:** Add an index if the application needs to query
    records by another attribute, such as City.
6.  **Capacity mode:** Choose On-Demand for variable traffic or
    Provisioned for predictable traffic.
7.  **Encryption at Rest:** Protect stored database data.
8.  **Encryption in Transit:** Protect data travelling over the network.
9.  **Backup:** Enable appropriate backup and recovery options.
10. **Least Privilege:** Give the application only the DynamoDB actions
    and resources it needs.

------------------------------------------------------------------------

## 12. Interview Questions

**Q1. What is DynamoDB?**\
Amazon DynamoDB is a fully managed NoSQL database service provided by
AWS.

**Q2. What is CRUD?**\
CRUD stands for Create, Read, Update, and Delete.

**Q3. What is a Partition Key?**\
It is the key attribute DynamoDB uses to determine an item's partition.
It is part of the table's primary key.

**Q4. What is a Sort Key?**\
It is an optional second key attribute used with the Partition Key to
distinguish and query related items.

**Q5. What is a GSI?**\
A Global Secondary Index lets you query a table using an alternative
partition key.

**Q6. What is an LSI?**\
A Local Secondary Index uses the same Partition Key as the base table
but a different Sort Key.

**Q7. On-Demand vs Provisioned?**\
On-Demand charges for requests without advance RCU/WCU capacity
configuration. Provisioned requires you to configure read and write
capacity.

**Q8. Encryption at Rest vs In Transit?**\
At Rest protects stored data; In Transit protects data moving across a
network.

**Q9. Why are backups important?**\
Backups help recover data after accidental deletion, damage, or unwanted
changes.

**Q10. What is Least Privilege?**\
Giving users and applications only the permissions they need to perform
their tasks.

------------------------------------------------------------------------

## Final Revision

-   CRUD = Manage data.
-   Data Types = Types of information.
-   Partition Key = Key used to identify and distribute items.
-   Sort Key = Distinguish and query related items.
-   GSI = Alternative partition key for querying.
-   LSI = Same partition key, different sort key.
-   On-Demand = Pay per request; no advance capacity configuration.
-   Provisioned = Configure read/write capacity.
-   Encryption at Rest = Protect stored data.
-   Encryption in Transit = Protect moving data.
-   Backup = Help recover data.
-   Least Privilege = Minimum permissions required.

### Main Security Rule

Protect data at rest and in transit, maintain backups, and never grant
unnecessary permissions.
