# Kubernetes Microservices Demo

This repository contains a simple microservices setup deployed on Kubernetes using **Minikube**.  
It demonstrates internal/external communication, service-to-service dependencies, and the use of **NetworkPolicies** for security.

---

##  Architecture

- **Database (MySQL / MongoDB)**  
  - Runs internally as a ClusterIP service.  
  - Not exposed outside the cluster.  
  - Accessible only by backend services (Product & Order).  

- **Product Service**  
  - Lightweight backend (`hashicorp/http-echo`).  
  - Displays `"Product Service Connected to DB"`.  
  - Talks to Database via `db-service`.  

- **Order Service**  
  - Lightweight backend (`hashicorp/http-echo`).  
  - Displays `"Order Service Connected to Product + DB"`.  
  - Talks to both `product-service` and `db-service`.  

- **Frontend (Nginx / React App)**  
  - Exposed externally using a **NodePort**.  
  - Talks only to `product-service` and `order-service`.  
  - Blocked from accessing the Database (via NetworkPolicy).  

---

##  Network Policies

* Allow **Product** and **Order** → Database ✅
* Deny **Frontend** → Database ❌
* Allow **Frontend** → Product + Order ✅

---

##  Verification Steps

* [x] Product → DB connection works.
* [x] Order → DB + Product connections work.
* [x] Frontend → Product + Order connections work.
* [x] Frontend → DB is blocked (after NetworkPolicy).
* [x] Frontend is reachable externally via Minikube NodePort.

Access frontend:

```bash
minikube service frontend-service
```

---

##  Results

* Before applying **NetworkPolicies** → all services can reach each other.
* After applying **NetworkPolicies** → Database is secured, only backend services can connect.

---

