# Lesson 21: ER Diagrams Intro


**ER** = Entity Relationship
**ER Diagram** = A great way to let us know data storage requirements (like business requirements) and convert them into an actual database schema.

---

## 1. Core Components

### **Entity**
> **Definition:** An object we want to model & store information about.
* **Example:** `Student`
* **Notation:** Represented by a **Rectangle**.

### **Attributes**
> **Definition:** Specific pieces of information about an entity.
* **Example:** `name`, `grade`, `gpa`
* **Notation:** Represented by **Ovals** connected to the entity.

### **Primary Key**
> **Definition:** An attribute that uniquely identifies an entry in the database table.
* **Example:** `student_id`
* **Notation:** Text is **Underlined** inside the oval.

---

## 2. Attribute Types

### **Composite Attribute**
> **Definition:** An attribute that can be broken up into sub-attributes.
* **Example:** The attribute `name` can be broken down into `fname` (first name) and `lname` (last name).
* **Notation:** Ovals connected to another attribute oval.

### **Multi-valued Attribute**
> **Definition:** An attribute that can have more than one value.
* **Example:** `clubs` (A student can be in many clubs).
* **Notation:** Represented by a **Double Oval**.

### **Derived Attribute**
> **Definition:** An attribute that can be derived from the other attributes.
* **Example:** `has_honors` (Calculated based on GPA, not stored directly).
* **Notation:** Represented by a **Dashed Oval**.

---

## 3. Relationships

### **Multiple Entities**
You can define more than one entity in the diagram.
* **Example:** `Student` entity and `Class` entity.

### **The Relationship**
> **Definition:** Defines a relationship between two entities.
* **Example:** A Student **Takes** a Class.
* **Notation:** Represented by a **Diamond** shape connecting two entities.

### **Participation**
* **Partial Participation:** Not all entities must partake in the relationship.
    * *Notation:* **Single Line** connection (e.g., Student side).
* **Total Participation:** All entities must partake in the relationship.
    * *Notation:* **Double Line** connection (e.g., Class side).

### **Relationship Attribute**
> **Definition:** An attribute specific to the relationship itself, not just one entity.
* **Example:** `grade` (The grade is specific to a student taking a specific class).
* **Notation:** Oval connected to the **Diamond**.

### **Relationship Cardinality**
> **Definition:** The number of instances of an entity from a relation that can be associated with the relation.
* **Types:**
    * **1 : 1** (One to One)
    * **1 : N** (One to Many)
    * **M : N** (Many to Many)

---

## 4. Advanced Entities

### **Weak Entity**
> **Definition:** An entity that cannot be uniquely identified by its attributes alone. It depends on a strong entity (owner).
* **Example:** `Exam` (might share names like "Midterm" across different classes).
* **Notation:** Represented by a **Double Rectangle**.
* **Partial Key:** The attribute used to identify it locally (e.g., `exam_name`) is represented by a **Dashed Underline**.

### **Identifying Relationship**
> **Definition:** A relationship that serves to uniquely identify the weak entity.
* **Example:** `Has` (Class *Has* Exam).
* **Notation:** Represented by a **Double Diamond**.

---

## Summary: ER Diagram Template

A complete ER Diagram combines these elements to tell a story about the data:
1.  **Entities** (Rectangles) hold data.
2.  **Attributes** (Ovals) describe the data.
3.  **Relationships** (Diamonds) connect the data.
4.  **Lines** (Single/Double) show participation requirements.
5.  **Letters** (1, N, M) show cardinality constraints.

flowchart TD
    %% Entities
    Student[Student]
    Class[Class]
    Exam[[Exam]]:::weak

    %% Relationships
    Takes{Takes}
    Has{Has}:::weakRel

    %% Attributes for Student
    name([name])
    fname([fname])
    lname([lname])
    sid([<u>student_id</u>])
    gpa([gpa])
    clubs([clubs]):::multi
    honors([has_honors]):::derived

    %% Attributes for Class
    cid([<u>class_id</u>])

    %% Attributes for Exam
    ename([exam_name]):::dashedUnderline

    %% Relationship Attributes
    grade([grade])

    %% Connections
    Student ---|M| Takes
    Takes ---|N| Class
    Class ===|1| Has
    Has ===|N| Exam

    %% Attribute Connections
    Student --- name
    name --- fname
    name --- lname
    Student --- sid
    Student --- gpa
    Student --- clubs
    Student -.- honors

    Class --- cid
    
    Takes --- grade

    Exam --- ename

    %% Styling to match ER Notation
    classDef weak fill:#f9f,stroke:#333,stroke-width:4px;
    classDef weakRel fill:#ff9,stroke:#333,stroke-width:4px,shape:diamond;
    classDef multi stroke:#333,stroke-width:4px;
    classDef derived stroke-dasharray: 5 5;
    classDef dashedUnderline stroke-dasharray: 0;