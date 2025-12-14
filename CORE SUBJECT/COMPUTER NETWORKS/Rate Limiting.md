
>  Definition **Rate Limiting** is a strategy for limiting network traffic. It puts a cap on how often someone can repeat an action within a certain timeframe—for instance, trying to log in to an account or making requests to an API.

---


It controls the rate of incoming requests to **prevent the server from being overwhelmed**.

### Placement

It usually sits at the **Application Layer (Layer 7)**, often implemented in:

- **API Gateway**
    
- **Load Balancer**
    

---

## 2. Why do we need it? (The "Why")

There are three main reasons interviewers look for:

1. **Prevent DDoS Attacks:** It stops malicious bots from sending millions of fake requests to crash your server.
    
2. **Fair Usage:** It prevents one heavy user from eating up all the server resources, ensuring speed remains good for other users.
    
3. **Cost Control:** If you use a paid service (like an SMS API that charges per request), rate limiting ensures you don't accidentally spend a fortune.
    

---


### Q: What happens when a user is rate-limited?

> **Answer:** "The server responds with **HTTP Status Code 429: Too Many Requests**."

It usually comes with a specific header telling the client when they can try again:

- `Retry-After: 3600` (Try again in 1 hour)