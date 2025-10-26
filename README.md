# 📧 Konuşarak Öğren - Email Automation

This project is an **AI Growth Internship Pre-Study** developed for **Konuşarak Öğren**, designed as an automated email follow-up system.  
The system is built entirely with **n8n**, **Google Sheets**, **Gmail**, and **OpenAI** integrations.

---

## 🚀 General Purpose

To automate the manual process of preparing follow-up emails for potential customers, monitor email performance, and generate reports.  
The goals are to **reduce email preparation time by 80%**, **ensure consistency in the follow-up process**, and **increase conversion rates by 15–20%**.

---

## ⚙️ Flow Summary

### **1️⃣ Daily Trigger**
- `Daily Trigger 10:00`: Runs every morning at 10:00 AM.

### **2️⃣ Data Fetching**
- `Fetch Leads`: Retrieves data from the `Leads!A:D` range in Google Sheets.  
  > 🟨 **Note:** Replace `your_google_sheet_id` with your own Google Sheet ID.

### **3️⃣ Data Preparation**
- `Normalize Leads`: Converts row data into Name, Email, Status, and LastContact format.  
- `Segment by Status`: Segregates users based on their status:
  - `TrialNotPurchased`
  - `FormNoTrial`
  - `Other`

### **4️⃣ Email Type Assignment**
- `Set Type: TrialNotPurchased`
- `Set Type: FormNoTrial`
- `Set Type: Default`

### **5️⃣ Tracking Link Generation**
- `Build Tracking URLs`: Generates a unique tracking ID (`trackingId`), open pixel (`openPixel`), and click URL (`clickUrl`) for each recipient.  
  > 🟨 **Note:** Replace `https://your-n8n-host` with your own n8n host address.

### **6️⃣ AI-Based Content Generation**
- `AI: Generate Subject & Body`: Uses OpenAI API to generate a Turkish HTML email (80–120 words) with a subject line.  
- `Merge AI + Tracking`: Embeds tracking links into the HTML content.  
- `Send Email - Day 0`: Sends the email via Gmail.  
- `Log Email Day 0`: Writes sending data to the `EmailLogs!A:D` sheet.

---

## ⏳ Time-Based Follow-Ups

### **Day 2 (After 2 Days)**
- `Wait 2 Days` → `Set Type: Day 2` → `AI: Day 2 Content` → `Merge Day 2` → `Send Email - Day 2` → `Log Email Day 2`

### **Day 5 (After 5 Days)**
- `Wait 3 More Days` → `Set Type: Day 5` → `AI: Day 5 Content` → `Merge Day 5` → `Send Email - Day 5` → `Log Email Day 5`

---

## 📊 Tracking & Analytics

### **Open and Click Tracking**
- `Webhook - Open` → `Log Open` → `OpenLogs!A:D`  
- `Webhook - Click` → `Log Click` → `ClickLogs!A:E`

Each email includes a 1×1 invisible GIF pixel for open tracking.  
Click links are logged through a webhook before redirection.

---

## 📈 Reporting

- `Get Email Logs` and `Get Click Logs` nodes read the logs.  
- `Calculate Stats (by messageType)`:
  - Number of emails sent  
  - Number of clicks  
  - Click-through rate (%)  
  - Best-performing message type (`best_message`)  
- `Append Report`: Appends results to the `Report!A:D` sheet.

### 📋 Example Google Sheet Table
🔗 [Example Sheet (Google Sheets)](https://docs.google.com/spreadsheets/d/1Xu4a1L4Ot2eGeNiebp10nfzxAlR6aJMS6ufNzoxtHMM/edit?usp=sharing)

---

## 📒 Sticky Notes (Visible Comments in n8n README)

| Node | Description |
|------|--------------|
| **Fetch Leads** | Don’t forget to replace `your_google_sheet_id`. |
| **Build Tracking URLs** | Add your own n8n URL in the `your-n8n-host` field. |
| **AI: Generate Subject & Body** | Update your OpenAI credential ID (`YOUR_OPENAI_CRED_ID`). |
| **Send Email - Day 0/2/5** | Update your Gmail OAuth2 credential ID (`YOUR_GMAIL_CRED_ID`). |
| **Google Sheets nodes** | All use the same credential (`YOUR_GOOGLE_SHEETS_CRED_ID`). |
| **Webhook - Click / Open** | These are public endpoints; SSL usage is recommended. |

---

## 🧠 AI Components Used

- **AI Tool:** ChatGPT  
  Used during **planning**, **learning**, and **JSON structure analysis** phases.  
  ChatGPT also assisted in preparing this README file.

- **OpenAI GPT (n8n-nodes-base.openAi):**  
  - Generates email content for each segment.  
  - Produces short, CTA-focused HTML templates.

- **AI-Assisted Analysis (function node):**  
  - Determines the best-performing email type.

---

## 🎥 Learning Resources

Before building this project, I studied the n8n platform using these tutorials:

1. [📺 Build and Sell AI Agents with n8n (5-Hour No-Code Course)](https://www.youtube.com/watch?v=PiOuEEBvY6A)  
2. [📺 Use n8n with Free Hosting – Unlimited Usage](https://www.youtube.com/watch?v=WbH3GDBw13A)

---

## ✅ Setup Steps

1. Import the `n8n-konusarak-ogren-mail-case.json` file into n8n (**Import Workflow**).  
2. From the **Credentials** tab, add:
   - Google Sheets (OAuth2)  
   - Gmail (OAuth2)  
   - OpenAI API keys  
3. Update placeholders in sticky notes (`your_google_sheet_id`, `your-n8n-host`).  
4. Activate the workflow.  
5. In Google Sheets, create sheets named:
   - **Leads**, **EmailLogs**, **ClickLogs**, **OpenLogs**, **Report**  
6. The system will start running automatically after the first trigger.

---

## 🧩 Expected Impact

| Metric | Before | After | Change |
|--------|---------|--------|---------|
| Email preparation time | 10 min | 2 min | ⬇️ 80% |
| Follow-up consistency | Low | High | ✅ |
| Conversion rate | 5% | 6–7% | ⬆️ +15–20% |

---

## 👤 Author

**Ömer Faruk Özer**  
AI Growth Intern Case Study – Konuşarak Öğren  
📧 ozeromerfaruk@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/omerozerf/) | [GitHub](https://github.com/omerozerf)
