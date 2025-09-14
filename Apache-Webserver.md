Great question 👍

### **Apache Server in Linux**

**Apache HTTP Server** (often just called **Apache**) is the most widely used **web server software** on Linux.

---

### **What it does**

* It listens for **HTTP/HTTPS requests** (like when you visit a website).
* Serves **web pages** (HTML, PHP, images, CSS, etc.) from the server’s filesystem to clients (browsers).
* Can run **dynamic applications** using modules like **mod\_php**, or by working with PHP-FPM, Python, Perl, etc.

---

### **Key points**

* **Package name**: `httpd` on RHEL/CentOS, `apache2` on Debian/Ubuntu.
* **Default web root**:

  * `/var/www/html` on most systems.
* **Config files**:

  * `/etc/httpd/` (RHEL/CentOS)
  * `/etc/apache2/` (Debian/Ubuntu)
* **Service management**:

  ```bash
  sudo systemctl start httpd     # start Apache (RHEL/CentOS)
  sudo systemctl enable httpd    # enable on boot
  sudo systemctl status httpd    # check status
  ```

---

### **How it works (simple flow)**

1. Browser requests `http://server-ip/index.html`.
2. Apache receives the request on **port 80** (or 443 for HTTPS).
3. Apache looks in its **document root** (default `/var/www/html`).
4. Finds the file → sends it back to the browser.

---

🔑 **In short**: Apache is the **web server software** that delivers web pages and runs web apps on Linux.

Do you also want me to explain **difference between Apache and Nginx**, since in your task you’re using **nginx instead of Apache**?
