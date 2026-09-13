# 🎓 Student Enrollment ETL Automation with n8n

An automated **ETL (Extract, Transform, Load) pipeline** built using **n8n** to clean, validate, and categorize messy student enrollment data.

The workflow takes a raw CSV file containing student enrollment records, uses an AI model to clean and standardize the data, validates email information, separates students based on their city, and generates four cleaned CSV files.

---

## 🚀 Project Overview

Student enrollment data collected through online forms can contain:

* Incorrect names
* Invalid email addresses
* Incorrect phone numbers
* Inconsistent course names
* Different formats for Yes/No values
* Inconsistent city names
* Different date formats

This project automates the cleaning process using **n8n + AI**.

### ETL Flow

```text
Raw CSV
   ↓
Extract Data
   ↓
AI Data Cleaning
   ↓
Parse Cleaned Data
   ↓
Check City
   ↓
Check Email
   ↓
Generate 4 CSV Files
```

---

## 🏗️ Workflow Architecture

```text
                    CSV Upload
                         │
                         ↓
                Extract From File
                         │
                         ↓
                 Basic LLM Chain
                         ↑
                 OpenAI Chat Model
                         │
                         ↓
                Code in JavaScript
                         │
                         ↓
                   IF - Chennai?
                    /           \
                  YES            NO
                   ↓              ↓
            IF - Email?      IF - Email?
              /     \          /       \
            YES      NO      YES       NO
             ↓        ↓       ↓         ↓
           CSV      CSV     CSV       CSV
             ↓        ↓       ↓         ↓
        Chennai    Chennai   Other     Other
         Valid     Invalid   Valid    Invalid
```

---

## 🔧 Technologies Used

| Technology            | Purpose                                |
| --------------------- | -------------------------------------- |
| **n8n**               | Workflow automation                    |
| **OpenAI Chat Model** | AI-based data cleaning                 |
| **JavaScript**        | Parse and process AI output            |
| **CSV**               | Input and output data format           |
| **LLM Chain**         | Connect student data with the AI model |

---

## 📥 Input Data

The workflow accepts a CSV file containing student enrollment information.

Example:

```text
Student_ID,Name,Email,Phone,Course,Fee_Paid,City,Enrolled_Date
STU1001,manoj k,manojk@gmail,9876543210,python,yes, chennai,23-05-2025
```

The input data may contain errors and inconsistent formatting.

---

## 🤖 AI Data Cleaning

The **Basic LLM Chain** sends each student record to the AI model with predefined cleaning rules.

### Cleaning Rules

* Convert names to Title Case
* Detect invalid email addresses
* Standardize phone numbers
* Standardize course names
* Convert Yes/No values into Boolean values
* Convert city names to Title Case
* Standardize enrollment dates
* Return the cleaned data as JSON

### Example

### Before

```json
{
  "Name": "manoj k",
  "Email": "manojk@gmail",
  "Course": "python",
  "Fee_Paid": "yes",
  "City": "chennai"
}
```

### After

```json
{
  "Name": "Manoj K",
  "Email": "INVALID_EMAIL",
  "Course": "Python",
  "Fee_Paid": true,
  "City": "Chennai"
}
```

---

## 🔀 Data Routing

After cleaning, the workflow uses **IF nodes** to categorize the students.

### First Condition

```text
Is City = Chennai?
```

This separates the records into:

```text
Chennai
Other Cities
```

### Second Condition

The email is checked:

```text
Is Email valid?
```

This produces four categories:

1. Chennai + Valid Email
2. Chennai + Invalid Email
3. Other City + Valid Email
4. Other City + Invalid Email

---

## 📤 Output Files

The workflow generates four CSV files:

```text
chennai_valid_email.csv
chennai_invalid_email.csv
other_valid_email.csv
other_invalid_email.csv
```

### Output Structure

```text
                 50 Students
                     │
              ┌──────┴──────┐
              ↓             ↓
           Chennai       Other City
              │             │
          ┌───┴───┐     ┌───┴───┐
          ↓       ↓     ↓       ↓
        Valid   Invalid Valid  Invalid
```

The total number of records across the four output files should equal the original number of records.

```text
Chennai Valid
+ Chennai Invalid
+ Other Valid
+ Other Invalid
= Total Input Records
```

---

## 📂 Project Structure

```text
student-enrollment-etl/
│
├── README.md
├── student_enrollment_raw.csv
│
├── output/
│   ├── chennai_valid_email.csv
│   ├── chennai_invalid_email.csv
│   ├── other_valid_email.csv
│   └── other_invalid_email.csv
│
└── n8n/
    └── student-enrollment-etl.json
```

> The n8n workflow JSON can be exported from n8n and added to the `n8n/` folder.

---

## ⚙️ How to Run

### 1. Open n8n

Start your n8n instance.

### 2. Import the workflow

Import:

```text
student-enrollment-etl.json
```

### 3. Add the CSV

Use:

```text
student_enrollment_raw.csv
```

### 4. Configure the AI Model

Connect your OpenAI credentials to the **OpenAI Chat Model** node.

### 5. Execute the workflow

Run the workflow.

The pipeline will:

```text
Read CSV
   ↓
Clean data with AI
   ↓
Validate data
   ↓
Route records
   ↓
Generate CSV files
```

---

## 🧩 n8n Nodes Used

The workflow contains the following main nodes:

```text
1. CSV Upload
2. Extract From File
3. Basic LLM Chain
4. OpenAI Chat Model
5. Code in JavaScript
6. IF - Chennai?
7. IF - Chennai Email?
8. IF - Other Email?
9. Chennai Valid → Convert to File
10. Chennai Invalid → Convert to File
11. Other Valid → Convert to File
12. Other Invalid → Convert to File
```

---

## ✅ Validation

The workflow validates important fields before generating the final files.

### Email

```text
Valid Email
    ↓
Valid output

Invalid Email
    ↓
INVALID_EMAIL
    ↓
Invalid output
```

### City

```text
Chennai
    ↓
Chennai branch

Any other city
    ↓
Other City branch
```

---

## 🎯 Learning Objectives

This project demonstrates practical knowledge of:

* ETL concepts
* Data cleaning
* Data validation
* Workflow automation
* n8n
* LLM integration
* Prompt engineering
* JavaScript data processing
* Conditional routing
* CSV processing
* AI-assisted data transformation

---

## 🌟 Key Features

* ✅ Automated ETL pipeline
* ✅ Processes multiple student records
* ✅ AI-powered data cleaning
* ✅ Email validation
* ✅ City-based routing
* ✅ Structured JSON output
* ✅ Automated CSV generation
* ✅ No manual data cleaning required
* ✅ Easy to extend with additional validation rules

---

## 📊 Example Workflow

```text
Raw Student Data
       ↓
   Extract CSV
       ↓
    AI Cleaner
       ↓
   JSON Parser
       ↓
   City Check
       ↓
 ┌─────┴─────┐
 ↓           ↓
Chennai     Other
 ↓           ↓
Email       Email
Check       Check
 ↓           ↓
┌─┴─┐       ┌─┴─┐
↓   ↓       ↓   ↓
✓   ✗       ✓   ✗
↓   ↓       ↓   ↓
CSV CSV     CSV CSV
```

---

## 🔮 Future Improvements

Possible improvements include:

* Add phone number validation
* Add duplicate student detection
* Add course validation
* Add automatic Google Sheets upload
* Add database storage
* Add email notifications
* Add error logging
* Add data quality reports
* Replace cloud LLM with a local open-source model
* Add a dashboard for ETL statistics

---

## 👨‍💻 Author

**Allwyn George**

Built as an AI & Data Engineering learning project using **n8n and LLM-powered automation**.

---

## 📌 Project Goal

The goal of this project is to demonstrate how **AI can be integrated into traditional ETL workflows** to reduce manual data-cleaning work and create a repeatable automated data pipeline.

> **Raw Data → AI Transformation → Validation → Structured Output**

---
