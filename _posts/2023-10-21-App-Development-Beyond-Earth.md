---
layout: post
title: "Beyond Earth: Secure Payments and AI-Powered Chatbot"
subtitle: "A Deep Dive into E-Commerce Innovations"
author: "Junhui"
header-style: text
tags:
  - Python
  - Flask
  - SQL
  - Stripe
  - OpenAI
  - E-commerce
  - App Development
  - Sustainability
---

> 🌏 **Beyond Earth**: Secure payments and AI-driven engagement!  
> Explore how we enhanced the e-commerce experience through cutting-edge payment solutions and intelligent chatbots.

---

## 🌟 Beyond Earth: The Project

As part of **Beyond Earth**, I played a pivotal role in implementing a secure payment solution using **Stripe JS** and developing an AI-powered chatbot to recommend recipes based on real-time stock data.  

---

## 🛠️ Technology Stack

- **Backend Framework**: Python Flask  
- **Payment Integration**: Stripe JS  
- **AI Chatbot**: OpenAI API  
- **Database**: SQLite & Shelve  
- **Frontend**: HTML, CSS  
- **APIs**: Stripe Webhooks, OpenAI GPT  

---

## 💳 Payment Integration: Secure and Seamless Transactions  

### Overview
To streamline the customer checkout process, I implemented a secure payment system using **Stripe JS**. The system ensures sensitive data is handled securely while providing a seamless user experience.

### Features
1. **Dynamic Checkout Route**:  
   - Captures payment details through a secure form.  
   - Validates input and processes transactions.  

2. **Webhook Integration**:  
   - Handles Stripe's real-time event notifications, ensuring payment statuses are updated dynamically.  

3. **Database Persistence**:  
   - Stores payment details securely in an SQLite database for record-keeping.

### 🔑 Key Contributions  

#### 1. Checkout Endpoint

```python
@app.route('/checkout/<total>', methods=['GET', 'POST'])
def checkout(total):
    payment_form = CreatePaymentForm(request.form)

    if request.method == 'POST' and payment_form.validate():
        payment_dict = {}
        db = shelve.open("payment.db", 'c')
        try:
            payment_dict = db['Payment']
        except KeyError:
            print("Error retrieving payment details.")
        payment = PaymentDetail(
            payment_form.card_type.data,
            payment_form.payment_name.data,
            payment_form.card_number.data,
            payment_form.expiration_date.data,
            payment_form.card_cvv.data
        )
        payment_dict[payment.count_id] = payment
        db['Payment'] = payment_dict
        db.close()
        return render_template("payment_success.html")
    return render_template('checkout.html', payment_form=payment_form, total=total)
```

#### 2. Stripe Webhook Handler

```python
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    payload = request.get_data(as_text=True)
    sig_header = request.headers.get('Stripe-Signature')

    try:
        event = stripe.Webhook.construct_event(payload, sig_header, endpoint_secret)
    except ValueError:
        return 'Invalid payload', 400
    except stripe.error.SignatureVerificationError:
        return 'Invalid signature', 400

    if event['type'] == 'payment_intent.succeeded':
        return render_template('payment_success.html')
    elif event['type'] == 'payment_intent.payment_failed':
        return render_template('payment_failure.html')
    return 'Unhandled event type', 200
```

---

## 🤖 AI-Powered Chatbot: Personalized Engagement

### Overview
The chatbot recommends healthy recipes tailored to customer preferences and current stock availability. Built using **OpenAI's GPT API**, it delivers personalized, dynamic responses.

### Features
1. **Session Management**:  
   - Maintains conversation history using Flask sessions.  
2. **Real-Time Recommendations**:  
   - Suggests recipes based on fresh stock.  
3. **Error Handling**:  
   - Gracefully manages API errors and user inputs.  

### 🔑 Key Contributions  

#### 1. Chatbot Endpoint

```python
@app.route('/chatbot', methods=["GET", "POST"])
@login_required
def chatbot():
    if 'chatbot' not in session:
        session['chatbot'] = {'messages': [], 'timestamps': []}

    chatbot_info = session['chatbot']
    client = ChatBot()

    if request.method == "POST":
        content = request.form.get('content')
        client.send_message("user", content)

        try:
            response = client.get_message()
            chatbot_info['messages'] = client.format_messages()
            chatbot_info['timestamps'] = client.get_timestamps()
        except Exception as e:
            print("Error:", str(e))

    return render_template("chatbot.html",
                           messages_and_timestamps=zip(chatbot_info['messages'], chatbot_info['timestamps']))
```

#### 2. ChatBot Class

```python
class ChatBot:
    def __init__(self, model="gpt-3.5-turbo"):
        self.client = OpenAI(api_key="sk-your-api-key")
        self.__model = model
        self.__messages = []
        self.__timestamps = [datetime.datetime.now().strftime("%H:%M")]

    def send_message(self, role, content):
        timestamp = datetime.datetime.now().strftime("%H:%M")
        self.__messages.append({"role": role, "content": content})
        self.__timestamps.append(timestamp)

    def get_message(self):
        if not self.__messages:
            return "Please start the conversation by sending a message."
        response = self.client.chat.completions.create(
            model=self.__model, messages=self.__messages
        )
        reply = response.choices[0].message
        self.send_message(reply.role, reply.content)
        return reply

    def format_messages(self):
        return [{"role": "assistant", "content": "How may I assist you today?"}, *self.__messages]

    def get_timestamps(self):
        return self.__timestamps
```

---

## 📈 Challenges & Solutions  

### Challenge 1: Payment Security  
**Solution**: Leveraged Stripe’s secure API and implemented validation at multiple layers, ensuring sensitive data is never exposed.  

### Challenge 2: Chatbot Optimization  
**Solution**: Cached frequent queries to reduce API calls and improve response times while managing API rate limits effectively.  

### Challenge 3: Complex JSON Parsing  
**Solution**: Utilized Python’s JSON library to parse, structure, and process large API responses efficiently.  

---

## 🎯 Key Takeaways  

1. **API Integration Expertise**:  
   Gained in-depth knowledge of integrating and optimizing third-party APIs like Stripe and OpenAI.  

2. **Enhanced Security Awareness**:  
   Implemented best practices for handling sensitive data in payment solutions.  

3. **Dynamic User Engagement**:  
   Designed a chatbot that combines real-time inputs with tailored responses to enrich user experience.  

---

## Reflection ✨

The **Beyond Earth** project was a rewarding journey that challenged me to blend secure backend solutions with engaging frontend innovations. This experience sharpened my technical expertise and reaffirmed my passion for creating impactful, user-centric applications.

---