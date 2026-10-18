# LeadFlow CRM – Complete Lead-to-Opportunity Management System

**Salesforce Administrator Portfolio Project | 2026**

A complete, production-style Salesforce Sales Cloud solution that combines lead capture, scoring, SLA tracking, automation, security, and reporting into one cohesive system.

---

## Project Overview

**LeadFlow CRM** was built to solve a real business problem: a mid-size B2B company was losing high-value leads because of slow follow-up, lack of prioritization, and no visibility for managers.

This project brings together **nine separate Salesforce configuration areas** into one unified, professional solution.

| Area                           | What Was Implemented                            |
| ------------------------------ | ----------------------------------------------- |
| Lead Capture                   | Web-to-Lead form with custom fields             |
| Lead Enrichment & Segmentation | Custom fields + Lead Segment                    |
| Lead Scoring                   | Automated scoring logic                         |
| High-Value Lead Identification | Automatic flagging of high-value leads          |
| SLA Tracking                   | Due dates + status tracking                     |
| Workflow Automation            | Three Record-Triggered Flows                    |
| Security Model                 | Roles, Profiles, Permission Sets, Sharing Rules |
| Contact & Account Management   | Standard + enhanced relationships               |
| Reporting & Dashboards         | Executive dashboard + operational reports       |

---

## Business Problem

- Sales team was tracking leads in spreadsheets
- High-value leads were not prioritized
- No SLA enforcement for first contact
- Managers had zero real-time visibility
- Marketing could not measure which sources converted best

---

## Solution Delivered

A fully configured Salesforce org that:

- Captures leads automatically from the website
- Scores and prioritizes every lead
- Flags high-value leads and enforces a 4-hour SLA
- Creates tasks and sends email alerts automatically
- Restricts data access based on role
- Gives managers a clear dashboard of pipeline health and SLA compliance

---

## Skills Demonstrated

- Salesforce Administration
- Data Modeling (Custom Fields, Relationships, Validation Rules)
- Flow Builder (Record-Triggered Flows)
- Security Architecture (Roles, Profiles, Permission Sets, OWD, Sharing Rules)
- Reports & Dashboards
- Web-to-Lead
- Business Process Design
- Documentation & Portfolio Presentation

---

## Complete Step-by-Step Setup Guide (All 9 Phases)

Follow these phases in order inside a free **Salesforce Developer Edition** org.

### Phase 0 – Create Your Salesforce Org

1. Go to [https://developer.salesforce.com/signup](https://developer.salesforce.com/signup)
2. Sign up for a free Developer Edition
3. Verify your email and log in
4. Switch to **Lightning Experience** if needed

---

### Phase 1 – Create Custom Fields on Lead

**Path:** Setup → Object Manager → Lead → Fields & Relationships → New

Create the following fields:

| Field Label        | Type      | Values / Notes                                          |
| ------------------ | --------- | ------------------------------------------------------- |
| Company Size       | Picklist  | 1-10, 11-50, 51-200, 201-1000, 1000+                    |
| Job Title Level    | Picklist  | C-Level, VP / Director, Manager, Individual Contributor |
| Lead Score         | Number    | Length 3, Decimal Places 0                              |
| High Value Lead    | Checkbox  | Default = Unchecked                                     |
| SLA Status         | Picklist  | On Track, At Risk, Breached, Completed                  |
| SLA Due Date       | Date/Time | -                                                       |
| First Contact Date | Date/Time | -                                                       |
| Lead Segment       | Picklist  | Enterprise, Mid-Market, SMB                             |

After creating each field, add it to the **Lead Page Layout**.

---

### Phase 2 – Validation Rules

**Path:** Setup → Object Manager → Lead → Validation Rules → New

**Rule 1 – Company Required**
- Name: `Company_Required`
- Formula: `ISBLANK(Company)`
- Error Message: `Company is required.`

**Rule 2 – Email Required for High-Value Leads**
- Name: `Email_Required_on_High_Value`
- Formula: `AND(High_Value_Lead__c = TRUE, ISBLANK(Email))`
- Error Message: `Email is required for High-Value Leads.`

---

### Phase 3 – Security Model

#### 3.1 Role Hierarchy
**Path:** Setup → Roles → Set Up Roles

Create this hierarchy:
CEO
└── VP Sales
├── Sales Manager - East
│     ├── Sales Associate East 1
│     └── Sales Associate East 2
└── Sales Manager - West
├── Sales Associate West 1
└── Sales Associate West 2
text#### 3.2 Profiles
Clone the **Standard User** profile three times:
- `Sales Manager`
- `Sales Associate`
- `Marketing User` (remove Edit access on Lead)

#### 3.3 Organization-Wide Defaults
**Path:** Setup → Sharing Settings

Set these objects to **Private**:
- Lead
- Account
- Opportunity

#### 3.4 Sharing Rules
Create two criteria-based sharing rules:
1. High Value Leads → Share with Role and Subordinates of Sales Managers (Read/Write)
2. All Leads → Share with Marketing User role (Read Only)

---

### Phase 4 – Automation (Flows)

**Path:** Setup → Flows → New Flow

#### Flow 1: Lead Scoring & High-Value Flag
- Type: Record-Triggered Flow
- Object: Lead
- Trigger: A record is created or updated
- Logic:
  - Calculate Lead Score based on Industry, Company Size, Job Title Level, and Lead Source
  - Update `Lead_Score__c`
  - If Score ≥ 70 → Set `High_Value_Lead__c = True`, set `SLA_Due_Date__c`, set `SLA_Status__c = On Track`

#### Flow 2: High-Value Lead SLA Reminder
- Create a Task assigned to the Lead Owner
- Subject: `Follow up with High-Value Lead – SLA`
- Priority: High
- Due Date = SLA Due Date
- Send Email Alert to Lead Owner (and Manager if needed)

#### Flow 3: Mark SLA Completed
- When `First_Contact_Date__c` is populated → Update `SLA_Status__c = Completed`

**Activate all three flows after testing.**

---

### Phase 5 – Web-to-Lead

**Path:** Setup → Web-to-Lead

1. Enable Web-to-Lead
2. Select the custom fields created in Phase 1
3. Generate the HTML form
4. Save the HTML for testing

Default values:
- Lead Source = Website
- Lead Status = Open - Not Contacted

---

### Phase 6 – Reports & Dashboard

Create the following reports:

1. **High-Value Leads – SLA Compliance**
2. **Lead Conversion by Source**
3. **Sales Pipeline by Stage**
4. **My Open High-Value Leads**

Then create a Dashboard named **Sales Leadership Dashboard** and add the above reports as components (charts + tables).

---

### Phase 7 – List Views

On the Lead tab, create these List Views:
- All High-Value Leads
- My High-Value Leads
- SLA At Risk
- Recently Created Leads

---

### Phase 8 – Screenshots

Take clear screenshots of:
- Custom fields on Lead
- Role Hierarchy
- All three Flow canvases
- Sales Leadership Dashboard
- High-Value Lead list view
- Web-to-Lead form (optional)

Place all screenshots inside the `Screenshots` folder.

---

### Phase 9 – Push to GitHub

```bash
# Navigate to the project folder
cd LeadFlow-CRM-Salesforce-Admin

# Initialize git (if not already done)
git init

# Add all files
git add .

# Commit
git commit -m "Complete LeadFlow CRM - Salesforce Admin Portfolio Project"

# Rename branch to main
git branch -M main

# Add your GitHub repository
git remote add origin https://github.com/theankitamahajan/LeadFlow-CRM-Salesforce-Admin.git

# Push
git push -u origin main
Note: Create the empty repository on GitHub first (do not initialize with README).

How to Test the Complete Solution

Create a new Lead with high Company Size + C-Level title → Score should be ≥ 70
Confirm High Value Lead checkbox is checked
Confirm a Task was created
Confirm SLA Due Date is set
Fill First Contact Date → SLA Status should change to Completed
Open the Sales Leadership Dashboard and verify data appears

