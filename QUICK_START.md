# 🚀 Quick Start Guide

## 5 Minutes to Working Bot

### Step 1: Clone Repository
```bash
git clone https://github.com/SANTHOSH-K06/ecommerce-chatbot-zoho-salesiq.git
cd ecommerce-chatbot-zoho-salesiq
```

### Step 2: Get Your API Keys

#### Shopify
1. Go to Shopify Admin → Apps → App settings → API credentials
2. Generate access token with scopes: read:orders, write:orders
3. Copy token to config.deluge

#### Google Sheets
1. Enable Sheets API: console.cloud.google.com
2. Create API key
3. Copy Sheet ID from URL
4. Add to config.deluge

#### Twilio (for OTP)
1. Create account at twilio.com
2. Get Account SID, Auth Token, Phone Number
3. Add to config.deluge

#### GitHub
1. Go to Settings → Developer settings → Personal access tokens
2. Create token with repo scope
3. Add to config.deluge

### Step 3: Deploy to Zoho SalesIQ

1. Log into **Zoho SalesIQ**
2. Create **New Bot** → Select **Scripts**
3. Copy code from `salesiq-bot-deluge/1_message_handler.deluge`
4. Paste into bot editor
5. Add other handler files
6. Click **Save & Deploy**

### Step 4: Test Your Bot

Open SalesIQ Chat Simulator and type:

```
deals
→ Shows 10 deals in carousel cards

track order
→ Asks for email, then shows shipped orders

add to cart
→ Adds item to session cart

verify
→ Sends OTP to phone via SMS

help
→ Shows bot capabilities
```

---

## File Structure

```
├── salesiq-bot-deluge/          (Deluge code)
│   ├── config.deluge
│   ├── 1_message_handler.deluge
│   ├── 2_deals_handler.deluge
│   ├── 3_order_handler.deluge
│   ├── 4_track_order_handler.deluge
│   ├── 5_otp_handler.deluge
│   ├── 6_ai_engine.deluge
│   └── 7_github_api_integration.deluge
├── README.md                    (Overview)
├── IMPLEMENTATION_GUIDE.md      (Full code)
├── QUICK_START.md              (This file)
└── codeless-plugs/             (3 reusable plugs)
```

---

## Configuration Checklist

- [ ] Clone repository
- [ ] Get Shopify API token
- [ ] Get Google Sheets API key
- [ ] Get Twilio credentials
- [ ] Get GitHub personal token
- [ ] Update config.deluge with credentials
- [ ] Copy Deluge scripts to Zoho SalesIQ
- [ ] Test "deals" command
- [ ] Test "track order" command
- [ ] Test OTP verification
- [ ] Deploy bot to production

---

## Common Issues

### Orders not showing
Fix: Verify Shopify token has correct scopes

### Deals not loading
Fix: Check Google Sheets API key and Sheet ID

### OTP not sending
Fix: Verify Twilio credentials and phone format

### Bot not responding
Fix: Check Deluge syntax in SalesIQ editor

---

## Features

✅ Deals of the Day - Carousel with Buy Now & Add to Cart
✅ Order Tracking - Email lookup with shipped orders
✅ Shopping Cart - Session-persistent
✅ OTP Verification - Twilio SMS
✅ AI Recommendations - Intent recognition
✅ OAuth 2.0 - Secure API auth
✅ GitHub Integration - Auto-commit

---

For full documentation, see IMPLEMENTATION_GUIDE.md

**Built for Zoho Cliqtrix 2025**
