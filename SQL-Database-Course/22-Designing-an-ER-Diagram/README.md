# Lesson 22: Designing an ER Diagram


**Goal:** Create an ER diagram based on a specific set of business requirements for a company.

-----

## 1\. Company Data Storage Requirements

### **Branch Information**

> "The company is organized into **branches**. Each branch has a **unique number**, and a **name**."

  * **Entity:** `Branch`
  * **Attributes:** `branch_id`, `branch_name`

### **Client Information**

> "The company makes its money by selling to **clients**. Each client has a **name** and a **unique number** to identify it."

  * **Entity:** `Client`
  * **Attributes:** `client_id`, `client_name`

### **Employee Information**

> "The foundation of the company is its **employees**. Each employee has a **name**, **birthday**, **sex**, **salary** and a **unique number** to identify it."

  * **Entity:** `Employee`
  * **Attributes:** `emp_id`, `name` (first\_name, last\_name), `birth_date`, `sex`, `salary`

### **Supplier Information**

> "Many branches will need to work with **suppliers** to buy inventory. For each supplier we'll keep track of their **name** and the **type of product** they're selling the branch."

  * **Entity:** `Branch Supplier`
  * **Attributes:** `supplier_name`, `supply_type`

-----

## 2\. Defining Relationships

### **Branch & Employee Relationships**

1.  **Works For:** "An **employee** can *work for* one **branch** at a time."
      * *Cardinality:* Employee (N) to Branch (1).
2.  **Manages:** "Each **branch** will be *managed* by one of the **employees** that work there."
      * "We'll also want to keep track of when the current manager *started as manager*."
      * *Attribute:* `start_date` on the relationship.
3.  **Supervision:** "An **employee** can act as a *supervisor* for other **employees** at the branch... An employee can have at most one supervisor."
      * *Type:* Recursive Relationship (Employee relates to Employee).

### **Client Relationships**

1.  **Handles:** "A **branch** may *handle* a number of **clients**... however a single **client** may only be handled by one **branch** at a time."
      * *Cardinality:* Branch (1) to Client (N).
2.  **Works With:** "Employees can *work with* **clients** controlled by their branch to sell them stuff... If necessary multiple employees can work with the same client."
      * "We'll want to keep track of **how many dollars worth of stuff** each employee *sells* to each client they work with."
      * *Attribute:* `total_sales` (on the relationship).

### **Supplier Relationships**

1.  **Supplies:** "A single **supplier** may *supply* products to multiple **branches**."
      * *Cardinality:* Supplier (M) to Branch (N).

-----

## 3\. The Final Diagram

Here is the complete ER Diagram based on the requirements above, rendered using Mermaid.js.

```mermaid
flowchart TD
    %% --- Entities ---
    Branch[Branch]
    Employee[Employee]
    Client[Client]
    Supplier[Branch Supplier]

    %% --- Relationships ---
    Works_For{Works For}
    Manages{Manages}
    Handles{Handles}
    Works_With{Works With}
    Supplies{Supplies}
    Supervision{Supervision}

    %% --- Attributes for Branch ---
    branch_name([branch_name])
    branch_id([<u>branch_id</u>])

    %% --- Attributes for Employee ---
    emp_id([<u>emp_id</u>])
    emp_name([name])
    fname([first_name])
    lname([last_name])
    dob([birth_date])
    sex([sex])
    salary([salary])

    %% --- Attributes for Client ---
    client_id([<u>client_id</u>])
    client_name([client_name])

    %% --- Attributes for Supplier ---
    sup_name([<u>supplier_name</u>])
    sup_type([supply_type])

    %% --- Relationship Attributes ---
    start_date([start_date])
    sales([total_sales])

    %% --- Connections: Entities to Relationships ---
    %% Employee Works For Branch
    Employee ---|N| Works_For
    Works_For ---|1| Branch

    %% Employee Manages Branch
    Employee ---|1| Manages
    Manages ---|1| Branch
    Manages --- start_date

    %% Employee Supervises Employee (Recursive)
    Employee ---|1| Supervision
    Supervision ---|N| Employee

    %% Branch Handles Client
    Branch ---|1| Handles
    Handles ---|N| Client

    %% Employee Works With Client
    Employee ---|M| Works_With
    Works_With ---|N| Client
    Works_With --- sales

    %% Branch Supplier Supplies Branch
    Supplier ---|M| Supplies
    Supplies ---|N| Branch

    %% --- Connecting Attributes to Entities ---
    Branch --- branch_id
    Branch --- branch_name

    Employee --- emp_id
    Employee --- emp_name
    emp_name --- fname
    emp_name --- lname
    Employee --- dob
    Employee --- sex
    Employee --- salary

    Client --- client_id
    Client --- client_name

    Supplier --- sup_name
    Supplier --- sup_type
```
