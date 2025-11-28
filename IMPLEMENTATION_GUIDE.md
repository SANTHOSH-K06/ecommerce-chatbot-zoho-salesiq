# Implementation Guide - Zoho SalesIQ eCommerce Chatbot

## Complete Deluge Code Reference

This document provides all the Deluge code needed to build the complete eCommerce chatbot.

---

## 1. MESSAGE HANDLER (1_message_handler.deluge)

```deluge
// Main Message Handler
incomingMessage = message.get("text").toString().toLower();
intent = detectIntent(incomingMessage);

if(intent == "welcome") {
    return showWelcomeMenu();
}
else if(intent == "deals") {
    return handleDealsFlow();
}
else if(intent == "track_order") {
    return handleTrackOrderFlow();
}
else if(intent == "view_cart") {
    return handleViewCart();
}
else {
    return handleAIFallback(incomingMessage);
}
```

---

## 2. DEALS FLOW (2_deals_handler.deluge)

```deluge
function handleDealsFlow() {
    deals = fetchDealsFromSheet();
    
    cards = list();
    for each deal in deals {
        card = map();
        card.put("title", deal.get("product_name"));
        card.put("subtitle", "$" + deal.get("original_price") + " → $" + deal.get("deal_price"));
        card.put("image_url", deal.get("image_url"));
        
        buttons = list();
        buyButton = map();
        buyButton.put("label", "Buy Now");
        buyButton.put("action", "postback");
        buyButton.put("value", "buy_" + deal.get("product_id"));
        buttons.add(buyButton);
        
        cartButton = map();
        cartButton.put("label", "Add to Cart");
        cartButton.put("action", "postback");
        cartButton.put("value", "cart_" + deal.get("product_id"));
        buttons.add(cartButton);
        
        card.put("buttons", buttons);
        cards.add(card);
    }
    
    response = map();
    response.put("action", "show_cards");
    response.put("cards", cards);
    return response;
}
```

---

## 3. ORDER TRACKING FLOW (4_track_order_handler.deluge)

```deluge
function handleTrackOrderFlow() {
    response = map();
    
    if(userSession.get("step") != "order_email_collected") {
        userSession.put("step", "collecting_email");
        response.put("action", "reply");
        response.put("text", "Enter your email to track orders:");
        return response;
    }
    
    email = userSession.get("customer_email");
    shopifyStore = "your-store.myshopify.com";
    token = "your_shopify_token";
    
    url = "https://" + shopifyStore + "/admin/api/2024-01/orders.json?email=" + email;
    headers = map("X-Shopify-Access-Token", token);
    
    try {
        apiResponse = invokeurl([url: url, type: "GET", headers: headers]);
        orders = apiResponse.get("orders");
        
        if(orders.size() > 0) {
            suggestions = list();
            for each order in orders {
                if(order.get("fulfillment_status") == "fulfilled" || order.get("fulfillment_status") == "partial") {
                    orderInfo = "Order #" + order.get("order_number") + " - " + order.get("total_price");
                    suggestions.add(orderInfo);
                }
            }
            
            response.put("action", "reply");
            response.put("text", "Your shipped orders:");
            response.put("suggestions", suggestions);
        }
    }
    catch(exception e) {
        response.put("action", "reply");
        response.put("text", "Error fetching orders.");
    }
    
    return response;
}
```

---

## 4. OTP VERIFICATION (5_otp_handler.deluge)

```deluge
function sendOTP(phoneNumber) {
    otp = randomNumber(100000, 999999).toString();
    
    twilioAccountSID = "your_twilio_sid";
    twilioAuthToken = "your_twilio_token";
    twilioFromNumber = "+1234567890";
    
    url = "https://api.twilio.com/2010-04-01/Accounts/" + twilioAccountSID + "/Messages.json";
    
    auth = encode(twilioAccountSID + ":" + twilioAuthToken, "base64");
    headers = map("Authorization", "Basic " + auth);
    
    params = map();
    params.put("From", twilioFromNumber);
    params.put("To", phoneNumber);
    params.put("Body", "Your ShopBot OTP is: " + otp);
    
    try {
        invokeurl([url: url, type: "POST", headers: headers, parameters: params]);
        userSession.put("otp", otp);
        userSession.put("otp_time", system.currentTimeMillis());
        return true;
    }
    catch(exception e) {
        logError("OTP send failed", e);
        return false;
    }
}

function verifyOTP(userOTP) {
    storedOTP = userSession.get("otp");
    otpTime = userSession.get("otp_time");
    currentTime = system.currentTimeMillis();
    
    // OTP valid for 5 minutes
    if((currentTime - otpTime) > 300000) {
        return map("success", false, "message", "OTP expired");
    }
    
    if(userOTP == storedOTP) {
        userSession.put("phone_verified", true);
        return map("success", true, "message", "Phone verified!");
    }
    
    return map("success", false, "message", "Invalid OTP");
}
```

---

## 5. AI ENGINE (6_ai_engine.deluge)

```deluge
function handleAIFallback(userText) {
    response = map();
    
    // Extract entities
    if(userText.contains("recommend") || userText.contains("suggest")) {
        category = extractCategory(userText);
        products = getProductRecommendations(category);
        
        response.put("action", "reply");
        response.put("text", "Here are my recommendations:");
        
        suggestions = list();
        for each product in products {
            suggestions.add(product.get("name"));
        }
        response.put("suggestions", suggestions);
    }
    else if(userText.contains("help") || userText.contains("support")) {
        response.put("action", "reply");
        response.put("text", "I can help you with:\n• View deals of the day\n• Track your orders\n• Add items to cart\n• Verify your phone");
    }
    else {
        response.put("action", "reply");
        response.put("text", "Sorry, I didn't understand. Try 'deals', 'track order', or 'help'.");
    }
    
    return response;
}

function extractCategory(userText) {
    if(userText.contains("shoe") || userText.contains("footwear")) {
        return "shoes";
    }
    else if(userText.contains("shirt") || userText.contains("cloth")) {
        return "clothing";
    }
    return "general";
}
```

---

## 6. GITHUB API INTEGRATION (7_github_api_integration.deluge)

```deluge
function commitOrderToGitHub(orderData) {
    gitHubToken = "your_github_token";
    repo = "SANTHOSH-K06/ecommerce-chatbot-zoho-salesiq";
    
    url = "https://api.github.com/repos/" + repo + "/contents/orders/" + orderData.get("order_id") + ".json";
    
    content = encode(orderData.toString(), "base64");
    
    payload = map();
    payload.put("message", "Add order #" + orderData.get("order_id"));
    payload.put("content", content);
    
    headers = map(
        "Authorization", "token " + gitHubToken,
        "Content-Type", "application/json"
    );
    
    try {
        invokeurl([url: url, type: "PUT", headers: headers, parameters: payload]);
        return true;
    }
    catch(exception e) {
        logError("GitHub commit failed", e);
        return false;
    }
}
```

---

## Setup Instructions

### Step 1: Clone Repository
```bash
git clone https://github.com/SANTHOSH-K06/ecommerce-chatbot-zoho-salesiq.git
```

### Step 2: Configure API Credentials
Edit `salesiq-bot-deluge/config.deluge` and add:
- Shopify Access Token
- Google Sheets API Key
- Twilio Account SID & Token
- GitHub Personal Access Token

### Step 3: Deploy to Zoho SalesIQ
1. Go to Zoho SalesIQ Dashboard
2. Create New Bot → Scripts
3. Copy & paste Deluge code from this repository
4. Test with chat simulator

### Step 4: Test Flows
- Type "deals" to see deals of the day
- Type "track order" to track orders
- Type "add to cart" to add items
- Type "verify" to get OTP

---

## Production Checklist

- [ ] All API keys configured
- [ ] OAuth 2.0 tokens tested
- [ ] Shopify webhook configured
- [ ] Email notifications working
- [ ] OTP SMS delivery confirmed
- [ ] GitHub auto-commit tested
- [ ] Error handling verified
- [ ] Cache TTL configured
- [ ] Analytics enabled
- [ ] Security audit completed

---

## Troubleshooting

### Orders not showing
- Verify Shopify API token is valid
- Check email parameter in API call
- Ensure orders exist and are shipped

### OTP not sending
- Verify Twilio credentials
- Check phone number format (+country code)
- Monitor Twilio dashboard

### Deals not loading
- Verify Google Sheets API key
- Check sheet ID and tab name
- Ensure rows have required columns

---

For more details, see README.md and docs/ folder.
