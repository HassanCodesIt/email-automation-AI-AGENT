# email-automation-AI-AGENT

![WhatsApp Image 2025-04-15 at 11 05 32_7d23d80c](https://github.com/user-attachments/assets/611421f0-0db3-4a09-8651-eff46903c735)



### ⚙️ Workflow Breakdown

This workflow uses **🧠 LLaMA 3.3-70B Versatile** (served via **⚡️ Groq API**) to generate professional emails from chat-like instructions. It also connects to **📊 Google Sheets** for contact data and sends emails via **📧 Gmail**.

---

#### 1. 🟢 **Start / Trigger: `When chat message received`**
- 🗨️ Accepts natural language input (like a message from a chatbot or UI).
- Example:
  > "Email raj@softmatrix.com and tell him the launch is postponed. Subject: Launch Update"

---

#### 2. 🤖 **AI Agent Node (LLM: LLaMA 3.3-70B via Groq)**
- Uses **Meta’s LLaMA 3.3-70B Versatile** model.
- Prompted to:
  - 📥 Extract `emailRecipient`
  - 📝 Create a clean `messageSubject`
  - ✍️ Generate a structured and professional `messageBody`
- All emails must end with:

  ```
  Best regards,

  Hassan
  ```

- Returns a response like:

  ```json
  {
    "emailRecipient": "raj@softmatrix.com",
    "messageSubject": "Launch Update",
    "messageBody": "Dear Raj,\n\nThe launch is postponed to next week due to some final tweaks.\n\nBest regards,\nHassan"
  }
  ```

---

#### 3. 🧼 **Function Node: `Clean & Parse JSON`**
- 🧹 Cleans up the raw AI output:
  - Removes ``` backticks
  - Escapes line breaks
  - Parses it into real JSON
- ✅ Makes the data usable for downstream nodes

---

#### 4. 🧱 **Set Node: `Set Fields`**
- Extracts values like:
  - `emailRecipient`
  - `messageSubject`
  - `messageBody`
- Prepares them for sending through Gmail or saving elsewhere.

---

#### 5. 📊 **Google Sheets Node: `Contact Database`**
- Looks up or stores contacts in a Google Sheet.
- Expected format:
  | Name | Email | Company | Location |
- Can:
  - Autofill missing info
  - Store new contacts
  - Log sent emails

---

#### 6. 📬 **Gmail Node: `Send Email`**
- Sends the email using Gmail:
  - ✉️ **To**: `emailRecipient`
  - 🏷️ **Subject**: `messageSubject`
  - 📝 **Body**: `messageBody`
- Result: A polished, AI-crafted email delivered automatically!

---

#### 7. 🧠 **(Optional) Simple Memory Node**
- Temporarily stores context during a session.
- Useful for:
  - Remembering names or tone
  - Improving flow in multi-step instructions

---


# Email Automation with AI Agent and Google Sheets

This repository contains a visual workflow built using [n8n](https://n8n.io/), an open-source workflow automation tool. It uses an AI agent to extract and compose emails based on user input and sends them automatically using Gmail. Additionally, it reads contact data from a Google Sheet.

---

## 📌 Workflow Overview

![Workflow Image](./workflow image.jpg)

### 🔗 Workflow Structure

1. **Trigger**: `When chat message received`
2. **AI Agent** (LLM):
   - Powered by Groq's LLaMA 3.3
   - Extracts:
     - `emailRecipient`
     - `messageSubject`
     - `messageBody`
   - Composes professional email content
3. **Google Sheets Node**: `Contact Database` (read mode)
   - Reads existing contacts (name, email, company, location)
4. **Simple Memory**:
   - Used to maintain short-term context between steps
5. **Function Node**:
   - Cleans up and parses AI output from the chat input
6. **Split/Set Node**:
   - Maps parsed JSON fields to individual values
7. **Gmail Node**:
   - Sends the generated email

---

## 🧠 AI Agent Prompt Logic

The AI is instructed to return data in the following format:

```json
{
  "emailRecipient": "recipient@example.com",
  "messageSubject": "Subject of the Email",
  "messageBody": "Body of the email message."
}
```

Guidelines the AI follows:
- Use formal greeting: `Dear [Name],`
- Maintain professional tone and clear structure:
  - Introduction
  - Message content
  - Conclusion
- End with a formal closing:

  ```
  Best regards,
  Hassan
  ```

---

## 📁 Included Files

- `My_workflow.json` – n8n exportable workflow
- `WORKFLOW DIAGRAM.jpg` – Workflow screenshot

---

## 📋 Sample Google Sheet Format

| Name        | Email                  | Company        | Location     |
|-------------|------------------------|----------------|--------------|
| Aisha Khan  | aisha@example.com      | NovaTech       | Dubai        |
| Leo Wang    | leo.wang@greenhub.io   | GreenHub Inc.  | Singapore    |
| Maria Lopez | maria@smartapps.mx     | SmartApps      | Mexico City  |
| Raj Patel   | raj@softmatrix.com     | SoftMatrix     | Mumbai       |

---

## 🚀 How to Use

1. Clone this repository:

   ```bash
   git clone https://github.com/HassanCodesIt/Email-automation-AI-AGENT.git
   cd Email-automation-AI-AGENT


2. Open [n8n](https://n8n.io) (self-hosted or cloud).

3. Import `My_workflow.json` into n8n.

4. Configure credentials:
   - Gmail API
   - Google Sheets API
   - Groq or OpenAI API (for AI agent)

5. Make sure your Google Sheet matches the format above.

6. Trigger the workflow by sending a message through the chat trigger.

---

## ⚙️ Built With

- [n8n](https://n8n.io)
- [Groq](https://groq.com)
- [Google Sheets API](https://developers.google.com/sheets/api)
- [Gmail API](https://developers.google.com/gmail/api)

---

## 📄 License

MIT © 2025 Hassan
