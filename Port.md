### 🔹 What is a Port in Networking?

A **port** in networking is a **logical endpoint** that helps identify a specific process or service running on a device (usually a server) over a network. While an **IP address** identifies a machine, the **port number** identifies a specific application or service on that machine.

---

### 🔸 Example:

Suppose you have a web server (like Apache or Nginx) running on a machine with IP `192.168.1.10`.

* If a user wants to access a website hosted on that server, the request goes to:

  ```
  192.168.1.10:80
  ```

  Here:

  * `192.168.1.10` is the **IP address**.
  * `80` is the **port number** for HTTP traffic (web).

If the server also runs an SSH service (for remote login), it listens on:

```
192.168.1.10:22
```

Here, `22` is the standard port for SSH.

---

### 🔸 Common Port Numbers

| Port | Protocol | Service            |
| ---- | -------- | ------------------ |
| 22   | TCP      | SSH (Secure Shell) |
| 80   | TCP      | HTTP (Web traffic) |
| 443  | TCP      | HTTPS (Secure Web) |
| 3306 | TCP      | MySQL database     |
| 5432 | TCP      | PostgreSQL         |

---

### 🔸 Real-World Analogy

Think of a **hotel**:

* The **hotel address** = IP address (location of the building).
* The **room number** = Port (specific service/room inside the hotel).
* The **guest** = A user or application trying to access the service.

---

Let me know if you want to visualize this in a diagram or need more examples.
