1. [Application Deployment without using Jenkins - Python](#application-deployment-without-using-jenkins---python)
2. [Application Deployment flow with using Jenkins](#Application-Deployment-flow-with-using-Jenkins)

## **Application Deployment without using Jenkins - Python**

Absolutely! Let’s break it down **step-by-step** from scratch — no prior containerization knowledge required.

---

## 🧠 What is Containerization?

Think of **containerization** like boxing your application with everything it needs (code, runtime, libraries) — so it can run **anywhere** (your laptop, OpenShift, cloud) **without breaking**.

This "box" is called a **Docker image**, and when you run it, it's called a **container**.

---

## ✅ Real-World Analogy

* Your app → A meal you cooked
* Container → A lunchbox (with your meal + spoon + napkin)
* Dockerfile → The recipe to build your lunchbox
* Docker image → The lunchbox
* Container → The meal being eaten
* JFrog → A lunch storage fridge
* OpenShift → A canteen serving the lunchbox

---

## 💻 Let’s Containerize a Simple Web App

We’ll take a basic Python web app and:

1. Build it into a Docker image
2. Push it to JFrog
3. Deploy it in OpenShift
4. Make it accessible to users

---

## 🔨 Step 1: Create Your Simple Web App

Create a folder:

```bash
mkdir my-webapp && cd my-webapp
```

Now create a file `app.py`:

```python
# app.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from containerized app!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

Now create a file `requirements.txt`:

```
flask
```

---

## 📦 Step 2: Create the Dockerfile (the recipe)

Create a file named `Dockerfile` (no extension):

```Dockerfile
# Use Python image from Docker Hub
FROM python:3.9

# Set working directory inside the container
WORKDIR /app

# Copy code and requirements
COPY . .

# Install required Python packages
RUN pip install -r requirements.txt

# Expose the port
EXPOSE 8080

# Command to run the app
CMD ["python", "app.py"]
```

This file tells Docker:

* Use Python
* Copy the app inside
* Install Flask
* Run `app.py`

---

## 🏗️ Step 3: Build the Docker Image

Make sure Docker is installed and running.

Run:

```bash
docker build -t my-webapp:latest .
```

✅ This creates the Docker image on your local system.

---

## 🔐 Step 4: Tag and Push to JFrog

### Step 4.1: Tag the image for your JFrog repository

Example:

```bash
docker tag my-webapp:latest mycompany.jfrog.io/my-docker-repo/my-webapp:v1
```

Replace with your JFrog domain and repo name.

### Step 4.2: Login to JFrog

```bash
docker login mycompany.jfrog.io
```

Use your JFrog **username** and **API key** or **password**.

### Step 4.3: Push the image

```bash
docker push mycompany.jfrog.io/my-docker-repo/my-webapp:v1
```

✅ This stores your image in JFrog.

---

## 🚀 Step 5: Deploy to OpenShift

We’ll deploy it to your `application-stage` namespace.

### Step 5.1: Create image pull secret

```bash
oc create secret docker-registry jfrog-secret \
  --docker-server=mycompany.jfrog.io \
  --docker-username=<your-username> \
  --docker-password=<your-password-or-api-key> \
  --docker-email=<your-email> \
  -n application-stage
```

### Step 5.2: Link the secret to default service account

```bash
oc secrets link default jfrog-secret --for=pull -n application-stage
```

### Step 5.3: Create deployment YAML (`deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-webapp
  namespace: application-stage
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-webapp
  template:
    metadata:
      labels:
        app: my-webapp
    spec:
      containers:
        - name: my-webapp
          image: mycompany.jfrog.io/my-docker-repo/my-webapp:v1
          ports:
            - containerPort: 8080
```

```bash
oc apply -f deployment.yaml
```

### Step 5.4: Create a service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
  namespace: application-stage
spec:
  selector:
    app: my-webapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

```bash
oc apply -f service.yaml
```

### Step 5.5: Expose it via Route

```bash
oc expose service my-webapp-service -n application-stage
```

Get the route:

```bash
oc get route -n application-stage
```

Open the URL in your browser — 🎉 You should see:

```
Hello from containerized app!
```

---

## 🧾 Summary

| Step | Task                 |
| ---- | -------------------- |
| 1    | Write app (`app.py`) |
| 2    | Create `Dockerfile`  |
| 3    | Build Docker image   |
| 4    | Push image to JFrog  |
| 5    | Deploy to OpenShift  |
| 6    | Access app via Route |

---

Let me know:

* If you want me to generate all the files for you
* Or if your app is in a different language (like Java, Node.js, etc.)

I’ll guide you accordingly.

## **Application Deployment flow with using Jenkins**

Excellent follow-up! You're thinking like a DevOps engineer now. Let’s walk through how **CI/CD** and **Jenkins** fit into this picture.

---

## 🔁 What is CI/CD?

* **CI (Continuous Integration):**
  Automatically builds and tests your app every time you push code (e.g., to GitHub).

* **CD (Continuous Deployment/Delivery):**
  Automatically builds a Docker image, pushes it to JFrog, and deploys it to OpenShift.

---

## 🧩 Where Jenkins Fits

Without Jenkins, you built the Docker image manually and deployed it step by step.

But with Jenkins, all these steps are **automated**.

---

## 🔄 Workflow with Jenkins CI/CD

Here's a typical CI/CD pipeline using Jenkins for your use case:

```plaintext
                GitHub (or GitLab)
                       │
                       ▼
            ┌────────────────────┐
            │  Jenkins Pipeline  │
            └────────────────────┘
               │      │       │
               ▼      ▼       ▼
         Build App  Run Tests  Build Docker Image
                                 │
                                 ▼
                      Push to JFrog Repository
                                 │
                                 ▼
                         Deploy to OpenShift
                                 │
                                 ▼
                        App exposed via Route
```

---

## ⚙️ Jenkins Pipeline Steps (Real Example)

1. **Checkout Code**
2. **Build App (Optional if Python/Node)**
3. **Run Unit Tests**
4. **Build Docker Image**
5. **Push Docker Image to JFrog**
6. **Deploy to OpenShift using `oc` CLI**

---

## 📝 Sample Jenkinsfile (Declarative Pipeline)

```groovy
pipeline {
    agent any

    environment {
        IMAGE = "mycompany.jfrog.io/my-docker-repo/my-webapp:${BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = 'jfrog-creds' // Defined in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-org/my-webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE} ."
                }
            }
        }

        stage('Push to JFrog') {
            steps {
                script {
                    docker.withRegistry('https://mycompany.jfrog.io', 'jfrog-creds') {
                        sh "docker push ${IMAGE}"
                    }
                }
            }
        }

        stage('Deploy to OpenShift') {
            steps {
                script {
                    sh """
                    oc project application-stage
                    oc set image deployment/my-webapp my-webapp=${IMAGE}
                    """
                }
            }
        }
    }
}
```

---

## 🧾 Prerequisites for Jenkins CI/CD

| Requirement       | Description                                                 |
| ----------------- | ----------------------------------------------------------- |
| Jenkins installed | On VM, OpenShift, etc.                                      |
| Jenkins plugins   | Docker, Git, Pipeline, Credentials                          |
| JFrog credentials | Stored in Jenkins as secret (`jfrog-creds`)                 |
| OpenShift access  | Jenkins must have `oc` CLI and access token                 |
| Git repo          | Your code must be version-controlled (GitHub, GitLab, etc.) |

---

## 💡 Benefits of CI/CD

| Without Jenkins | With Jenkins CI/CD |
| --------------- | ------------------ |
| Manual steps    | Fully automated    |
| Prone to errors | Consistent builds  |
| Slower releases | Fast delivery      |
| No traceability | Clear audit trail  |

---

## 🔚 Summary

You use **Jenkins CI/CD** to **automate**:

* Building your app
* Creating Docker image
* Pushing to JFrog
* Deploying to OpenShift

Once it's set up, every code change automatically goes to production (or staging)!

---

Let me know:

* If you want me to generate a **complete Jenkinsfile** for your Python app
* Or if you're using **GitHub Actions** or **GitLab CI** instead of Jenkins

I'll tailor the pipeline for that.


Image


## **Image**

The **image name** refers to the **full path of the container image** stored in your **private JFrog registry**. The format is:

```
<registry-url>/<repository>/<image-name>:<tag>
```

---

### 🔍 Example (JFrog-hosted image):

If your JFrog Artifactory is hosted at:
`mycompany.jfrog.io`

And your Docker repository in JFrog is:
`my-docker-repo`

And your image is:
`my-webapp`
And tag is:
`v1.0.3`

Then the full **image name** is:

```bash
mycompany.jfrog.io/my-docker-repo/my-webapp:v1.0.3
```

---

### 🔐 Important:

* This image must be **pullable** using your credentials.
* You can verify this by doing a local `docker pull`:

```bash
docker login mycompany.jfrog.io
docker pull mycompany.jfrog.io/my-docker-repo/my-webapp:v1.0.3
```

---



To deploy a web application in OpenShift from a **private JFrog repository**, you need to follow these steps in **sequence**. This assumes your application is packaged as a container image and stored in **JFrog Artifactory**.

---

### ✅ **Prerequisites**

* Namespace: `application-stage` exists.
* You have access to the private JFrog Docker registry (username/password or token).
* The application image is already pushed to JFrog.
* You have the image name and tag, e.g., `mycompany.jfrog.io/my-repo/my-app:latest`.

---

### 🚀 **Step-by-Step Deployment**

#### **Step 1: Login to JFrog and Create Secret**

Create a **Docker registry secret** that allows OpenShift to pull from your private JFrog registry.

```bash
oc create secret docker-registry jfrog-secret \
  --docker-server=mycompany.jfrog.io \
  --docker-username=<your-username> \
  --docker-password=<your-password-or-api-key> \
  --docker-email=<your-email> \
  -n application-stage
```

---

#### **Step 2: Link the Secret to Default Service Account**

This allows OpenShift pods in this namespace to use the secret when pulling images.

```bash
oc secrets link default jfrog-secret --for=pull -n application-stage
```

---

#### **Step 3: Create Deployment YAML (Example)**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-webapp
  namespace: application-stage
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-webapp
  template:
    metadata:
      labels:
        app: my-webapp
    spec:
      containers:
        - name: my-webapp
          image: mycompany.jfrog.io/my-repo/my-app:latest
          ports:
            - containerPort: 8080
      imagePullSecrets:
        - name: jfrog-secret
```

Save this as `deployment.yaml`.

---

#### **Step 4: Deploy the Application**

```bash
oc apply -f deployment.yaml
```

---

#### **Step 5: Create a Service (Expose the Pod)**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
  namespace: application-stage
spec:
  selector:
    app: my-webapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

Save as `service.yaml` and apply:

```bash
oc apply -f service.yaml
```

---

#### **Step 6: Expose the Service via Route (Optional for External Access)**

```bash
oc expose service my-webapp-service -n application-stage
```

It will create a route you can access externally.

---

### ✅ **Verify**

```bash
oc get pods -n application-stage
oc get svc -n application-stage
oc get routes -n application-stage
```

---

### 🧠 Notes

* If your image requires SSL validation, make sure OpenShift nodes trust JFrog's certificate.
* You can also use **Helm** or **Kustomize** if the deployment is complex.
* You can define resources, environment variables, liveness/readiness probes as needed.

---

Let me know if your app is not in container form and needs to be built from source instead.

The command you posted:

```sh
oc create secret docker-registry jfrog-secret \
  --docker-server=mycompany.jfrog.io \
  --docker-username=<your-username> \
  --docker-password=<your-password-or-api-key> \
  --docker-email=<your-email> \
  -n application-stage
```

is used to **create a Docker registry secret** in OpenShift (or Kubernetes) that stores credentials for pulling images from a **private container registry**, in this case, **JFrog Artifactory**.

---

### 🔍 Explanation of Each Part:

| Part                                           | Meaning                                                                                 |
| ---------------------------------------------- | --------------------------------------------------------------------------------------- |
| `oc create secret docker-registry`             | Command to create a secret of type `docker-registry`.                                   |
| `jfrog-secret`                                 | Name of the secret you’re creating. You can refer to this in your deployment/pod YAMLs. |
| `--docker-server=mycompany.jfrog.io`           | URL of your JFrog Artifactory (or Docker registry).                                     |
| `--docker-username=<your-username>`            | Your Artifactory username.                                                              |
| `--docker-password=<your-password-or-api-key>` | Your password or API key for authentication.                                            |
| `--docker-email=<your-email>`                  | Your email address (required by the Docker config, but not validated).                  |
| `-n application-stage`                         | The namespace where the secret should be created.                                       |

---

### 📦 Use Case:

If your image is stored in a **private JFrog registry**, OpenShift needs credentials to pull it. This secret provides those credentials.

---

### ✅ Next Step: Link the Secret to Your Deployment

Once this secret is created, you need to reference it in your deployment YAML like this:

```yaml
spec:
  template:
    spec:
      imagePullSecrets:
        - name: jfrog-secret
```

This tells the pod to use `jfrog-secret` when pulling images.

---

Let me know if you want a full deployment YAML with the secret referenced.

