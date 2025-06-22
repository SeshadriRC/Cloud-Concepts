In OpenShift (and Kubernetes), **ConfigMaps** and **Secrets** are used to **externalize configuration** and **sensitive data**, so that your application pods can be configured **without changing code**.

---

## 🔹 What is a **ConfigMap**?

A **ConfigMap** is used to store **non-sensitive configuration data** in **key-value pairs**.
Think of it like a **settings file** (like `.env` or `app.config`).

### ✅ Examples of what you store in a ConfigMap:

* Application settings (`ENV=prod`)
* Config files
* URLs, ports, file paths
* Feature flags

---

### 🔧 Sample ConfigMap YAML:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app-config
data:
  APP_ENV: production
  APP_PORT: "8080"
  LOG_LEVEL: debug
```

---

## 🔹 What is a **Secret**?

A **Secret** is like a ConfigMap but used to store **sensitive data**, such as:

* Passwords
* Tokens
* TLS certificates
* Database credentials

Secrets are **base64 encoded** (not encrypted by default, but safer than plain text).

---

### 🔐 Sample Secret YAML:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: dXNlcm5hbWU=     # base64 of 'username'
  password: cGFzc3dvcmQ=     # base64 of 'password'
```

> You can create secrets using plain text too, via `kubectl create secret`.

---

## 📦 How to Use ConfigMaps & Secrets in Pods

You can use them in 3 ways:

| Method            | Use Case                       | Example                            |
| ----------------- | ------------------------------ | ---------------------------------- |
| **Env Variable**  | Inject as environment variable | `env:` in pod spec                 |
| **Mounted File**  | Mount as files in container    | `volumeMounts` and `volumes`       |
| **CLI Reference** | Directly in app logic          | App reads from environment or path |

---

### ✅ Example: Use ConfigMap in Pod

```yaml
env:
  - name: APP_ENV
    valueFrom:
      configMapKeyRef:
        name: my-app-config
        key: APP_ENV
```

---

### ✅ Example: Use Secret in Pod

```yaml
env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: password
```

---

## 🔁 Key Differences

| Feature        | ConfigMap                  | Secret                             |
| -------------- | -------------------------- | ---------------------------------- |
| Stores         | Non-sensitive data         | Sensitive/confidential data        |
| Encoding       | Plain text                 | Base64-encoded                     |
| Type           | `ConfigMap`                | `Opaque`, `dockerconfigjson`, etc. |
| Use case       | App configs, env variables | Passwords, keys, tokens            |
| Access control | Normal                     | More restricted (RBAC recommended) |

---
