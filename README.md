# 📅 WhatsApp Appointment Booking AI Chatbot

A fully conversational AI chatbot on WhatsApp that collects client details, checks availability, books appointments into Google Calendar, and logs everything to Google Sheets — all automatically.

---

## 🔄 Workflow Overview

```
User sends WhatsApp message
         ↓
  WhatsApp Webhook (Meta Cloud API)
         ↓
  Parse Incoming Message
         ↓
  Load Conversation History (Redis)
         ↓
  Build Full Context (multi-turn memory)
         ↓
  Claude AI — Booking Agent (Aria)
  (collects name, date, time, service, email)
         ↓
  Parse AI Response
   ↙          ↓            ↘
Save to     Send        Booking
Redis      Reply       Detected?
                          ↓ Yes
                  ┌───────────────┐
             Google          Google
             Calendar         Sheets
             (Event)          (Log)
```

---

## 🤖 How the Chatbot Works

The AI agent named **Aria** guides users through a natural conversation:

1. **Greets** the user and asks what service they need
2. **Collects** name, preferred date, time, service type, and email — one question at a time
3. **Confirms** all details before booking
4. **Triggers booking** by outputting a structured JSON signal
5. **Creates** a Google Calendar event and logs the appointment

### Example Conversation

```
User:  Hi, I'd like to book an appointment
Aria:  Hi! I'm Aria 👋 I'd love to help you book an appointment.
       What service are you interested in?
       - Consultation (Free, 30 min)
       - Strategy Session ($150, 60 min)
       - Full Audit ($300, 90 min)

User:  Strategy session please
Aria:  Great choice! What's your full name?

User:  Muhammad Ali
Aria:  Thanks Muhammad! What date works best for you?
       We're available Mon–Fri, 9AM–5PM.

User:  Next Monday
Aria:  And what time on Monday? Available slots: 9AM, 10AM, 11AM, 2PM, 3PM, 4PM

User:  10AM
Aria:  Perfect! Last thing — what's your email address for the calendar invite?

User:  muhammad@email.com
Aria:  ✅ All set! Here's your booking summary:
       📅 Date: Monday, [date]
       ⏰ Time: 10:00 AM
       🛎 Service: Strategy Session
       👤 Name: Muhammad Ali
       📧 muhammad@email.com

       Shall I confirm this booking?

User:  Yes
Aria:  🎉 Booked! You'll receive a calendar invite shortly.
       See you on Monday at 10AM!
```

---

## ⚙️ Tools & Nodes Used

| Tool | Purpose |
|------|---------|
| **Meta WhatsApp Cloud API** | Receive and send WhatsApp messages |
| **Claude Sonnet (Anthropic)** | Conversational AI booking agent |
| **Redis** | Store multi-turn conversation memory (24hr TTL) |
| **Google Calendar** | Create appointment events with invites |
| **Google Sheets** | Log all booked appointments |

---

## 🚀 How to Import

1. Open your n8n instance
2. Go to **Workflows → Import from File**
3. Upload `workflow.json`
4. Add credentials (see below)
5. Copy the webhook URL from the trigger node
6. Paste it into your Meta WhatsApp App webhook settings
7. Activate the workflow

---

## 🔑 Credentials Required

| Credential | Where to Get It |
|-----------|----------------|
| WhatsApp Access Token | [Meta Developer Portal](https://developers.facebook.com/) → WhatsApp → API Setup |
| WhatsApp Phone Number ID | Meta Developer Portal → WhatsApp → Getting Started |
| Anthropic API Key | [console.anthropic.com](https://console.anthropic.com/) |
| Redis | Self-hosted or [Redis Cloud](https://redis.io/cloud/) (free tier) |
| Google Calendar OAuth | n8n built-in Google OAuth |
| Google Sheets OAuth | n8n built-in Google OAuth |

---

## 📝 Configuration

Replace these placeholders in the workflow:

```
YOUR_WHATSAPP_ACCESS_TOKEN   → Meta Cloud API permanent token
YOUR_GOOGLE_CALENDAR_ID      → Calendar ID (found in Google Calendar settings)
YOUR_GOOGLE_SHEET_ID         → Sheet ID from the URL
```

---

## 📸 Workflow Preview

> Import the JSON into n8n and take a screenshot of your canvas here.

---

## 🔧 Customization Tips

- **Change services/pricing**: Edit the system prompt in the Claude AI node
- **Change business hours**: Update available slot logic in the system prompt
- **Add SMS fallback**: Connect a Twilio node alongside WhatsApp
- **Connect to Calendly**: Replace Google Calendar with Calendly API for advanced scheduling
- **Add CRM push**: Connect HubSpot or Pipedrive after booking confirmation
- **Multi-language**: Add a language detection step before the AI node

---

## ⚠️ Security Note

This workflow JSON has been scrubbed of all API tokens and credentials. Never commit real tokens to GitHub. Use n8n's credentials manager and environment variables for all secrets.

---

**Built with:** n8n • WhatsApp Business API • Claude Sonnet • Redis • Google Calendar • Google Sheets
