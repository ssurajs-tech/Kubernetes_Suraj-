
# 📌 MongoDB Deployment & Service on Kubernetes

This YAML manifest defines a **MongoDB Deployment** and a **Service** to expose it within a Kubernetes cluster. It ensures that MongoDB runs in a containerized environment and is securely accessible for our MERN application.

---

## 🚀 Deployment: `mongo-deployment`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: mongo-deployment  
  labels: 
    app: mongo
```

* **apiVersion: apps/v1** → Uses the Kubernetes API for managing Deployments.
* **kind: Deployment** → Ensures MongoDB pods run continuously and are automatically restarted if they fail.
* **metadata** → Defines basic information:

  * **name: mongo-deployment** → Unique identifier for this Deployment.
  * **labels** → Adds a label `app: mongo` so Kubernetes objects can find and connect with it.

---

### 🛠️ Deployment Specification

```yaml
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
```

* **replicas: 1** → Runs only **one MongoDB pod** (suitable for dev/testing).
* **selector** → Ensures the Deployment manages pods with the label `app: mongo`.
* **template.metadata.labels** → Applies the same label `app: mongo` to pods so they match the Deployment’s selector.

---

### 📦 Container Definition

```yaml
spec:
  containers:
  - name: mongo
    image: mongo
    ports:
    - containerPort: 27017
    env:
      - name: MONGO_INITDB_ROOT_USERNAME
        valueFrom:
          secretKeyRef:
            name: secret
            key: mongo-user
      - name: MONGO_INITDB_ROOT_PASSWORD
        valueFrom:
          secretKeyRef:
            name: secret
            key: mongo-password
```

* **containers** → Defines the container that will run inside the pod.
* **name: mongo** → Container name.
* **image: mongo** → Pulls the **official MongoDB image** from Docker Hub.
* **ports** → Exposes port `27017` (MongoDB’s default port).
* **env** → Securely injects **MongoDB root credentials**:

  * `MONGO_INITDB_ROOT_USERNAME` and `MONGO_INITDB_ROOT_PASSWORD` are **referenced from a Kubernetes Secret** (not hardcoded for security).
  * `secretKeyRef` ensures credentials are taken from a `Secret` named **secret** with keys:

    * `mongo-user`
    * `mongo-password`

✅ This way, sensitive information is never stored in plain text inside the manifest.

---

## 🌐 Service: `mongo-service`

```yaml
apiVersion: v1
kind: Service 
metadata:
  name: mongo-service
spec: 
  selector:
    app: mongo
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```

* **kind: Service** → Creates a stable networking endpoint to access MongoDB pods.
* **name: mongo-service** → Other applications (like Express in the MERN stack) will use this service name to connect to MongoDB.
* **selector: app: mongo** → Routes traffic only to pods labeled `app: mongo`.
* **ports**:

  * **port: 27017** → Exposes MongoDB inside the cluster on port 27017.
  * **targetPort: 27017** → Forwards requests to the same port inside the container.

✅ The Service ensures that even if the MongoDB pod is restarted or rescheduled, the **service name (`mongo-service`) remains constant**, so the MERN app always connects without interruption.

---

## 🔐 Security with Kubernetes Secret

This Deployment expects a `Secret` object named **secret** with keys `mongo-user` and `mongo-password`.
Here’s an example of how you can create it:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret
type: Opaque
data:
  mongo-user: bW9uZ291c2Vy   # base64 encoded username
  mongo-password: c2VjdXJlcGFzcw==   # base64 encoded password
```

> ⚠️ Encode values using `echo -n 'your-value' | base64` before adding them.

---

## 📌 How It Fits in MERN Project

* **MongoDB** stores application data (users, products, sessions, etc.).
* **Express/Node.js API** will connect to MongoDB using:

```
mongodb://<MONGO_INITDB_ROOT_USERNAME>:<MONGO_INITDB_ROOT_PASSWORD>@mongo-service:27017
```

* **Kubernetes Service (`mongo-service`)** ensures reliable communication between the backend (Express API) and MongoDB.

---

## ✅ Summary

* **Deployment (`mongo-deployment`)** → Runs MongoDB in a pod with secure credentials.
* **Service (`mongo-service`)** → Exposes MongoDB internally for other services (Express backend).
* **Secret** → Stores sensitive credentials securely.
* **Integration** → Ensures MERN stack apps can always connect to MongoDB in Kubernetes.

# ⚙️ ConfigMap: `mongo-config`

```yaml
apiVersion: apps/v1
kind: ConfigMap
metadata:
  name: mongo-config
data:
  mongo-url: mongo-service
```

---

## 📝 Explanation

* **kind: ConfigMap** → Stores **non-sensitive configuration data** in key-value pairs. Unlike `Secret`, it’s not encrypted, so it should only be used for non-sensitive information.
* **metadata.name: mongo-config** → The ConfigMap is named `mongo-config`.
* **data.mongo-url: mongo-service** → This sets the `mongo-url` key with the value `mongo-service`.

✅ This is especially useful when other applications (like the **Express backend**) need to know how to connect to MongoDB. Instead of hardcoding the service name in deployments, we use a ConfigMap for better flexibility and maintainability.

---

## 🔗 How It Fits in the MERN Project

* In your **backend deployment (Express/Node.js)**, you can load this ConfigMap as an **environment variable**. Example:

```yaml
env:
  - name: MONGO_URL
    valueFrom:
      configMapKeyRef:
        name: mongo-config
        key: mongo-url
```

This way, your backend pod will have an environment variable `MONGO_URL=mongo-service`.

* If you later change the MongoDB service name or connection string, you only update the ConfigMap, **not all deployments**.

---

## 📌 Complete Flow Now

1. **Secret (`secret`)** → Stores MongoDB username & password securely.
2. **Deployment (`mongo-deployment`)** → Runs MongoDB pod using credentials from Secret.
3. **Service (`mongo-service`)** → Exposes MongoDB within the cluster at `mongo-service:27017`.
4. **ConfigMap (`mongo-config`)** → Stores MongoDB service name (`mongo-service`) so backend apps can reference it easily.
5. **Backend Deployment (Express/Node.js)** → Reads `mongo-url` from ConfigMap + credentials from Secret to connect to MongoDB.

---

📊 This separation of **Secrets** and **ConfigMaps** is a **Kubernetes best practice**:

* ConfigMap → General configuration (non-sensitive).
* Secret → Sensitive credentials.

Nice 👍 you’ve now added the **Secret** manifest for MongoDB credentials. Let me break this down for your **GitHub README** so it matches the Deployment + Service + ConfigMap explanation.

---

# 🔐 Secret: `mongo-secret`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mongo-secret
type: Opaque
data:
  mongo-user: dXNlcm5hbWU=
  mongo-password: cGFzc3dvcmQ=
```

---


* **kind: Secret** → Stores **sensitive information** such as usernames, passwords, and tokens.
* **metadata.name: mongo-secret** → The Secret is named `mongo-secret`.
* **type: Opaque** → Default type for arbitrary user-defined data.
* **data:** → Contains key-value pairs, but **values are base64 encoded** for transport.

### Keys:

* `mongo-user: dXNlcm5hbWU=` → Decodes to `username`.
* `mongo-password: cGFzc3dvcmQ=` → Decodes to `password`.

👉 Use the command below to encode your own credentials:

```bash
echo -n 'your-username' | base64
echo -n 'your-password' | base64
```

---

## 🔗 How This Secret is Used

In the **MongoDB Deployment**, we reference this Secret to inject credentials into the MongoDB container:

```yaml
env:
  - name: MONGO_INITDB_ROOT_USERNAME
    valueFrom:
      secretKeyRef:
        name: mongo-secret
        key: mongo-user
  - name: MONGO_INITDB_ROOT_PASSWORD
    valueFrom:
      secretKeyRef:
        name: mongo-secret
        key: mongo-password
```

This ensures that MongoDB starts with a **root user** and **password**, but the actual credentials never appear in plain text in the Deployment YAML.

---

## 📌 Security Best Practices

* **Do not commit real credentials** to GitHub. Instead:

  * Use placeholder values like `base64-encoded-username` in the repo.
  * Apply real secrets separately in your deployment environment.
* Rotate your secrets regularly.
* Use **sealed-secrets** or an external secret manager (e.g., HashiCorp Vault, AWS Secrets Manager) for production setups.

---

## ✅ Final MongoDB Setup (So Far)

1. **Secret (`mongo-secret`)** → Stores MongoDB username & password securely.
2. **ConfigMap (`mongo-config`)** → Stores MongoDB service name (`mongo-service`).
3. **Deployment (`mongo-deployment`)** → Runs MongoDB pod with environment variables from Secret.
4. **Service (`mongo-service`)** → Exposes MongoDB internally in the cluster.

---

👉 I noticed in your YAML you wrote **`mango-user`** (with an extra "a"). Did you mean **`mongo-user`** (to match your Deployment)? If not fixed, your Deployment will fail because the key won’t match.

---

# 🌐 Mongo Express Deployment & Service

This manifest defines a **Deployment** and a **Service** for [Mongo Express](https://hub.docker.com/_/mongo-express), a simple web-based MongoDB admin interface. It connects to MongoDB using the **credentials from the Secret** and the **service name from the ConfigMap**.

---

## 🚀 Deployment: `webapp-deployment`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: webapp-deployment  
  labels: 
    app: webapp
```

* **kind: Deployment** → Ensures the Mongo Express pod is always running and restarts if it fails.
* **metadata.name: webapp-deployment** → The Deployment name is `webapp-deployment`.
* **labels: app: webapp** → Tags this Deployment and its pods for selection.

---

### 🛠️ Deployment Specification

```yaml
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
```

* **replicas: 1** → Runs a single Mongo Express pod (enough for dev/testing).
* **selector.matchLabels** → Ensures the Deployment manages pods labeled `app: webapp`.
* **template.metadata.labels** → Assigns the same label to the pods so they match the selector.

---

### 📦 Container Definition

```yaml
containers:
- name: mongo-express 
  image: mongo-express:latest
  ports:
    - containerPort: 8081
  env:
    - name: ME_CONFIG_MONGODB_ADMINUSERNAME 
      valueFrom:
        secretKeyRef:
          name: secret
          key: mongo-user
    - name: ME_CONFIG_MONGODB_ADMINPASSWORD 
      valueFrom:
        secretKeyRef:
          name: secret
          key: mongo-password
    - name: ME_CONFIG_MONGODB_SERVER         
      valueFrom:
        configMapKeyRef: 
          name: mongo-config
          key: mongo-url
```

* **image: mongo-express\:latest** → Runs the latest Mongo Express image.
* **ports: 8081** → Exposes Mongo Express on port 8081 (its default UI port).
* **env (environment variables):**

  * **ME\_CONFIG\_MONGODB\_ADMINUSERNAME** → Injected from `mongo-secret` (`mongo-user`).
  * **ME\_CONFIG\_MONGODB\_ADMINPASSWORD** → Injected from `mongo-secret` (`mongo-password`).
  * **ME\_CONFIG\_MONGODB\_SERVER** → Injected from `mongo-config` (`mongo-url`), which points to the MongoDB Service (`mongo-service`).

✅ This setup ensures Mongo Express can securely connect to MongoDB without hardcoding any sensitive values.

---

## 🌐 Service: `webapp-service`

```yaml
apiVersion: v1
kind: Service 
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - protocol: TCP
      port: 8081
      targetPort: 8081
      nodePort: 30100
```

* **kind: Service** → Exposes Mongo Express so we can access it externally.
* **type: NodePort** → Opens a port on each worker node to forward traffic to the pod.
* **selector: app: webapp** → Routes traffic only to pods labeled `app: webapp`.
* **ports:**

  * **port: 8081** → Exposes Mongo Express inside the cluster.
  * **targetPort: 8081** → Forwards traffic to the container’s port.
  * **nodePort: 30100** → Exposes the app externally on `<NodeIP>:30100`.

✅ You can now access Mongo Express in your browser at:

```
http://<NodeIP>:30100
```

---

## 📌 Complete Flow (MongoDB + Mongo Express)

1. **Secret (`mongo-secret`)** → Stores MongoDB username & password.
2. **ConfigMap (`mongo-config`)** → Stores MongoDB service name (`mongo-service`).
3. **MongoDB Deployment (`mongo-deployment`)** → Runs MongoDB using credentials from Secret.
4. **MongoDB Service (`mongo-service`)** → Exposes MongoDB internally to other services.
5. **Mongo Express Deployment (`webapp-deployment`)** → Runs Mongo Express using credentials from Secret + MongoDB Service from ConfigMap.
6. **Mongo Express Service (`webapp-service`)** → Exposes the Mongo Express web UI externally on `NodePort 30100`.

---

