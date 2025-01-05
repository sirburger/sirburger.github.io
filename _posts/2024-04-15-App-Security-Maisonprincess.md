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
---

> 🛡️ **MaisonPrincess**: A sustainable thrifting platform fortified with robust security measures.  
> Explore how we secured our application against the **OWASP Top 10 vulnerabilities** as part of our **App Security Project**.

---

## 🌟 The Project: App Security at Its Core  

**MaisonPrincess** is a hybrid thrift marketplace where users can buy and sell second-hand clothing. This project was part of my **Application Security assignment**, where I implemented measures to secure the platform against the **OWASP Top 10** vulnerabilities.

The challenge:  
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
Improperly handled inputs could lead to SQL injection or other attacks.  

- **A07 - Broken Access Control**  
Attackers might misuse tokens to access payment endpoints.

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

#### Key Features  

1. **Structured Logging**  
Logs are stored in **JSONL format** to make them easy to analyze.  

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

2. **Log Integrity Verification**  
Each log entry references the previous log’s hash, ensuring tamper-proof records.  

**Code Snippet:**  
```python
def compute_hash(log_entry):
    log_entry_str = json.dumps(log_entry, sort_keys=True, default=str)
    return hashlib.sha256(log_entry_str.encode('utf-8')).hexdigest()
```

3. **Anomaly Detection**  
Detects path traversal (`../../etc/passwd`) and SQL injection (`' OR '1'='1`) attempts.  

**Code Snippet:**  
```python
def detect_sql_injection(log_entry):
    query = log_entry['extra'].get('action', '')
    if re.search(r'\b(union|select|insert|update|delete|drop|truncate)\b', query, re.IGNORECASE):
        return True
    return False
```

---

### 4. Distributed Log Storage  

**OWASP Threat Addressed:**  
- **A09 - Security Logging and Monitoring Failures**  
Local logs could be deleted during an attack.  

**Mitigation:**  
- Sent logs to an **off-site location** (e.g., Loggly) for redundancy.  

**Code Snippet:**  
```python
requests.post(LOGGLY_ENDPOINT, json=log_entry)
```

---

### 5. Log Retention Policy  

**Threat Addressed:**  
- **A09 - Security Logging and Monitoring Failures**  
Attackers might overwhelm the logging system to trigger log deletions.  

**Mitigation:**  
- Compressed logs exceeding **50MB** into ZIP files for archival purposes.  

**Code Snippet:**  
```python
def zip_log_file(file_path):
    with zipfile.ZipFile(file_path + ".zip", 'w') as zipf:
        zipf.write(file_path)
    clear_log_file(file_path)
```

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
