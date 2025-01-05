---
layout: post
title: "MaisonPrincess: Securing a Thrifting Marketplace Against the OWASP Top 10"
subtitle: "First time securitng a website against the OWASP Top 10"
author: "Junhui"
header-style: text
tags:
  - Python
  - Flask
  - SQL
  - Stripe
  - OWASP Top 10
  - E-commerce
  - App Security
  - Sustainability
  - Cybersecurity
---

> 🛡️ **MaisonPrincess**: A sustainable thrifting platform fortified with robust security measures.  
> Explore how we secured our application against the **OWASP Top 10 vulnerabilities** as part of our **App Security Project**.

---

## 🌟 The Project: App Security at Its Core  

**MaisonPrincess** is a hybrid thrift marketplace where users can buy and sell second-hand clothing. This project was part of my **Application Security assignment**, where I implemented measures to secure the platform against the **OWASP Top 10 vulnerabilities**.  

### Home Page  
Here's a sneak peek at the MaisonPrincess home page:  
![Home Page of MaisonPrincess](/img/in-post/Appsec/home.png)

### The Challenge:  
1. Build a functional platform that supports thrifting.  
2. Secure it against the **OWASP Top 10 vulnerabilities** using industry best practices.

---

## 🛠️ Technology Stack  

### Backend  
- **Flask**: Web framework for handling requests and routing.  
- **SQLite**: Lightweight relational database.  

### Frontend  
- **HTML, CSS, JavaScript, Bootstrap**: Creating a responsive and user-friendly UI.  

### Hosting  
- **Google Cloud Platform (GCP)**: Ensuring reliability and scalability.  

---

## 🔒 Security Features  

To align with the **OWASP Top 10**, I implemented a range of features, from **Role-Based Access Control (RBAC)** to a **secure logging system**.  

---

### 1. Role-Based Access Control (RBAC)  

**OWASP Threat Addressed:**  
- **A01 - Broken Access Control**  
Unintended or unauthorized access to sensitive data can compromise the system.

**Mitigation:**  
- Implemented **RBAC** to limit access based on user roles.  
- Unauthorized attempts are logged with context like IP and username.

**Code Snippet:**  
```python
def require_role(*roles):
    def wrapper(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            user = session.query(User).filter(User.username == flask_session.get('username')).first()
            if user and user.role in roles:
                return f(*args, **kwargs)
            else:
                logger.bind(username=flask_session.get("username", "anonymous"),
                            ip=request.remote_addr,
                            action=request.path,
                            outcome="failed").warning(
                    f"Unauthorized access attempt by user: {user.username if user else 'unknown'} with role: {user.role if user else 'unknown'} to endpoint: {request.endpoint}")
                return jsonify({"error": "Access denied"}), 403
        return decorated_function
    return wrapper
```

---

### 2. Secure Payments with Stripe  

**OWASP Threat Addressed:**  
- **A03 - Injection**  
- **A07 - Broken Access Control**

**Mitigation:**  
- Used **JWT tokens** to authenticate Stripe API requests.  
- Sanitized form inputs and implemented **server-side rendering** to prevent injection attacks.

**Code Snippet:**  
```python
@app.route('/create-payment-intent', methods=['POST'])
@login_required
def create_payment():
    token = request.headers.get('Authorization')
    if not token:
        return jsonify({'error': 'Token is missing!'}), 403
    user_id = verify_jwt(token.split(" ")[1])
    if not user_id:
        return jsonify({'error': 'Invalid token!'}), 403
     
    data = json.loads(request.data)
    cart = flask_session.get('cart', [])
    if not cart:
        return jsonify({'error': 'Cart is empty'}), 400

    # Create a PaymentIntent
    price = calculate_order_amount(cart)
    intent = stripe.PaymentIntent.create(
        amount=price,
        currency='sgd',
        automatic_payment_methods={'enabled': True},
    )
    return jsonify({'clientSecret': intent['client_secret']})
```

---

### 3. Comprehensive Logging System  

Logging is critical for monitoring, threat detection, and incident response. The logging system in MaisonPrincess aligns with **OWASP A09 - Security Logging and Monitoring Failures**.

#### Log Dashboard  
Logs are displayed in an intuitive dashboard for monitoring and analysis:  
![Log Dashboard](/img/in-post/Appsec/log_dashboard.png)

#### Key Features  

| **Feature**                  | **Description**                                                                                       |
|------------------------------|-------------------------------------------------------------------------------------------------------|
| **Structured Logging**        | Logs stored in **JSONL format** for easier parsing and analysis.                                     |
| **Log Integrity Verification**| Ensures each log references the hash of the previous log, forming a secure chain.                   |
| **Detailed Log Context**      | Logs include IP address, username, HTTP request paths, and socket information.                     |
| **Anomaly Detection**         | Flags SQL injection and path traversal attempts.                                                   |
| **Distributed Log Storage**   | Sends logs to an off-site storage (e.g., Loggly) for redundancy.                                   |
| **Log Retention Policy**      | Archives logs exceeding **50MB** into ZIP files for storage efficiency.                             |

**Example Log Entry:**  
```json
{
  "time": "2024-08-16 10:47:45",
  "level": "INFO",
  "message": "Login success for user: admin from IP: 127.0.0.1",
  "extra": {
    "username": "admin",
    "ip": "127.0.0.1",
    "action": "/login",
    "outcome": "success"
  },
  "socket": {
    "server_ip": "127.0.0.1",
    "server_port": "5000",
    "hostname": "exa",
    "local_ip": "172.26.180.183"
  },
  "module": "routes",
  "process": {"id": 43084, "name": "MainProcess"},
  "function": "login",
  "line": 618,
  "previous_hash": "0000000000000000000000000000000000000000000000000000000000000000",
  "hash": "eb73b813ab8ed930091370856fb68a82d4563278f1fa4673a42fc059e0a3830f"
}
```

#### Log Configuration Table  
![Log Configuration Table](/img/in-post/Appsec/Untitled-2024-07-19-2058.svg)

---

### 4. Warning Management Panel  

The **Warning Management Panel** enables admins to monitor and ban suspicious users.  

#### Key Features:  
- Users with repeated violations are flagged and banned.  
- Bans can be managed by the admin in real-time.

![Warning Management Panel](/img/in-post/Appsec/Warning_dahboard.png)
---

## 🎤 Demonstration  

During the live demo, I showcased how MaisonPrincess handles:  
1. Secure payments using JWT and Stripe.  
2. Role-based access to sensitive inventory data.  
3. Detailed and tamper-proof logging for effective monitoring.

---

## 🌟 Conclusion  

MaisonPrincess exemplifies how to merge **functionality**, **sustainability**, and **security**. By addressing the **OWASP Top 10 vulnerabilities**, this project is a step toward creating a safer digital ecosystem.

If you have any questions or feedback, feel free to reach out. Let’s continue building secure applications together!

---
