# 🛒 eCommerce Chatbot for Zoho SalesIQ

## Overview

A production-ready AI-powered chatbot built exclusively for the eCommerce industry using **Zoho SalesIQ** bot-building platform with **Deluge** scripting language. This chatbot provides intelligent features like deals management, order tracking, cart operations, OTP verification, and AI-driven product recommendations.

## ✨ Key Features

### 1. **Deals of the Day** 🎉
- Fetch real-time deals from Google Sheets, Zoho CRM, or external APIs
- Display products in interactive carousel cards
- One-click "Buy Now" and "Add to Cart" actions
- Complete checkout flow with customer information collection

### 2. **Order Tracking** 📦
- Email-based order lookup
- Display shipped orders in user-friendly format
- Real-time order status updates from Shopify/WooCommerce
- Return/cancel item options for non-dispatched orders

### 3. **Shopping Cart Management** 🛍️
- Add/remove items from session-based cart
- Persistent cart across conversation sessions
- Quick checkout with one-click option

### 4. **OTP Verification** 🔐
- Twilio/SMS-based phone number verification
- Secure customer authentication
- Session-based verification tracking

### 5. **AI Recommendations** 🤖
- Intent recognition and NLP-based response generation
- Smart fallback using AI models
- Product suggestions based on user behavior
- Context-aware recommendations

### 6. **Third-Party Integrations** 🔗
- **Shopify API**: Order management, product data
- **WooCommerce API**: Alternative eCommerce platform support
- **Google Sheets API**: Deals management with OAuth 2.0
- **GitHub API**: Auto-commit and deployment automation

## 🏗️ Architecture

```
ecommerce-chatbot-zoho-salesiq/
├── salesiq-bot-deluge/              # Core bot scripts
│   ├── 1_message_handler.deluge     # Main intent router
│   ├── 2_deals_handler.deluge       # Deals flow logic
│   ├── 3_order_handler.deluge       # Checkout & order creation
│   ├── 4_track_order_handler.deluge # Order tracking logic
│   ├── 5_otp_handler.deluge         # OTP verification
│   ├── 6_ai_engine.deluge           # AI & NLP processing
│   ├── 7_github_api_integration.deluge  # GitHub automation
│   └── config.deluge                # Configuration & constants
├── codeless-plugs/                  # Reusable bot plugs
│   ├── plug_1_fetch_deals.json      # Fetch deals from APIs
│   ├── plug_2_create_order.json     # Create orders in Shopify
│   └── plug_3_track_order.json      # Track orders by email
├── frontend/                        # React frontend
│   ├── src/
│   │   ├── components/
│   │   │   ├── ProductCarousel.jsx
│   │   │   ├── OrderTracker.jsx
│   │   │   ├── Cart.jsx
│   │   │   └── ChatWidget.jsx
│   │   └── App.jsx
│   └── package.json
├── backend/                         # Optional Python backend
│   ├── shopify_integration.py
│   └── github_webhook_handler.py
└── docs/                            # Documentation
    ├── SETUP_GUIDE.md
    ├── API_REFERENCE.md
    ├── SHOPIFY_OAUTH_SETUP.md
    └── DEPLOYMENT.md
```

## 🚀 Quick Start

### Prerequisites
- Zoho SalesIQ account (free or paid)
- Shopify/WooCommerce store (or any supported eCommerce platform)
- Google Sheets API key for deals management
- GitHub account for repository management

### Installation

1. **Clone the Repository**
   ```bash
   git clone https://github.com/SANTHOSH-K06/ecommerce-chatbot-zoho-salesiq.git
   cd ecommerce-chatbot-zoho-salesiq
   ```

2. **Set Up Zoho SalesIQ Bot**
   - Log into Zoho SalesIQ
   - Create a new bot using the "Scripts" platform
   - Copy Deluge code from `salesiq-bot-deluge/` directory

3. **Configure API Keys**
   - Update `config.deluge` with your API keys
   - Shopify access token
   - Google Sheets API key
   - GitHub personal access token

4. **Deploy Frontend** (Optional)
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

## 📋 Brownie Points Implementation

✅ **OTP Verification**: Phone number verification with SMS using Twilio  
✅ **OAuth 2.0**: Secure authentication for Shopify, Google APIs  
✅ **Return Item Option**: Display return button for non-dispatched orders  
✅ **AI Functionalities**:  
  - Intent recognition and NLP
  - Smart product recommendations
  - Contextual conversational responses
  - Dynamic fallback handling

## 🔌 Codeless Bot Plugs

### Plug 1: Fetch Deals
**Purpose**: Retrieve deals from external data sources  
**Inputs**: Source URL, authentication tokens  
**Outputs**: List of deal products with metadata

### Plug 2: Create Order
**Purpose**: Create orders in Shopify/WooCommerce  
**Inputs**: Product ID, customer info, quantity  
**Outputs**: Order confirmation with order ID

### Plug 3: Track Order
**Purpose**: Fetch shipped orders by email  
**Inputs**: Customer email, store credentials  
**Outputs**: List of orders with shipping status

## 📖 Deluge Code Examples

### Intent Detection
```deluge
function detectIntent(userText) {
    if(userText.contains("deal") || userText.contains("offer")) {
        return "deals";
    }
    else if(userText.contains("track") || userText.contains("order")) {
        return "track_order";
    }
    return "general";
}
```

### Fetch Deals from Shopify
```deluge
function fetchDealsFromSheet() {
    sheetId = "YOUR_GOOGLE_SHEET_ID";
    url = "https://sheets.googleapis.com/v4/spreadsheets/" + sheetId + "/values/Deals";
    response = invokeurl([url: url, type: "GET", headers: map()]);
    return response.get("values");
}
```

## 🔐 OAuth 2.0 Integration

The chatbot uses OAuth 2.0 for secure API authentication:

- **Shopify**: Store access tokens securely
- **Google APIs**: Automatic token refresh
- **GitHub**: Personal access tokens for CI/CD

## 🤖 AI Engine Features

- **Intent Classification**: Categorize user queries automatically
- **Entity Extraction**: Identify products, emails, phone numbers
- **Sentiment Analysis**: Understand customer satisfaction
- **Dynamic Responses**: Generate contextual replies

## 📊 GitHub API Integration

Automate repository management directly from Deluge:

```deluge
function createGitHubCommit(message, content) {
    gitHubToken = "YOUR_GITHUB_TOKEN";
    url = "https://api.github.com/repos/SANTHOSH-K06/ecommerce-chatbot-zoho-salesiq/contents/data/orders.json";
    headers = map("Authorization", "token " + gitHubToken);
    invokeurl([url: url, type: "PUT", headers: headers]);
}
```

## 🧪 Testing

### Unit Tests
```bash
cd backend
python -m pytest tests/ -v
```

### Bot Testing in SalesIQ
1. Open SalesIQ chat simulator
2. Test each intent: "deals", "track my order", "add to cart"
3. Verify API responses and error handling

## 📱 Frontend Features

- Responsive design (mobile & desktop)
- Real-time cart updates
- Order status tracking dashboard
- Secure checkout interface
- Built with React + Vite for optimal performance

## 📝 API Documentation

For detailed API endpoints, response formats, and webhook handlers, see:
- `docs/API_REFERENCE.md`
- `docs/SHOPIFY_OAUTH_SETUP.md`

## 🛠️ Customization

### Add Custom Intents
Edit `salesiq-bot-deluge/1_message_handler.deluge` and add:
```deluge
else if(intent == "custom_intent") {
    botResponse = handleCustomFlow();
}
```

### Connect Different eCommerce Platforms
1. Update API endpoints in `config.deluge`
2. Modify API response parsers in handlers
3. Test with platform-specific test data

## 🌟 Advanced Features

- **Visitor Session Persistence**: Maintain conversation state across multiple interactions
- **Dynamic Carousel Cards**: Product display with rich formatting
- **Multi-Step Forms**: Guided customer information collection
- **Email Notifications**: Automated order confirmations
- **Webhook Support**: Real-time updates from eCommerce platforms

## 🔗 Supported Platforms

✅ Shopify  
✅ WooCommerce  
✅ Ecwid  
✅ BigCommerce  
✅ Magento (v2.0+)  

## 📧 Email Integration

Automatic email notifications for:
- Order confirmations with invoice
- Shipping status updates
- Delivery notifications
- Return/refund confirmations

## 🚨 Error Handling

The bot includes comprehensive error handling:
- API timeout fallbacks
- Graceful error messages
- Retry logic with exponential backoff
- Error logging to external services

## 🔄 Deployment Pipeline

1. **GitHub Actions CI/CD**: Automated testing on push
2. **Zoho Portal Integration**: Direct bot deployment
3. **Version Control**: Track bot configuration changes
4. **Rollback Support**: Quick rollback to previous versions

## 📊 Monitoring & Analytics

- Track conversation metrics in SalesIQ dashboard
- Monitor API response times
- Analyze user intents and satisfaction
- Visualize conversion funnel

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request with documentation

## 📄 License

MIT License - see LICENSE file for details

## 🎓 Educational Value

This project demonstrates:
- Advanced Deluge scripting for chatbots
- RESTful API integration patterns
- OAuth 2.0 security implementation
- eCommerce workflow automation
- Production-ready error handling
- CI/CD with GitHub API

## 📞 Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Email: support@ecommercechatbot.dev
- Zoho Forum: Community discussions

## 🎯 Roadmap

- [ ] Multi-language support (Hindi, Tamil, etc.)
- [ ] WhatsApp integration
- [ ] Advanced inventory management
- [ ] Loyalty program integration
- [ ] Predictive analytics
- [ ] Voice-based order placement

---

**Built for Zoho Cliqtrix 2025 Bot Building Contest**

**By**: SANTHOSH-K06  
**College**: Rathinam Technical Campus  
**Contact**: GitHub Issues
