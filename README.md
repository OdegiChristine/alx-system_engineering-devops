# Web Infrastructure Design

This repository contains whiteboard diagrams and explanations of three progressively complex web infrastructure setups for hosting `www.foobar.com`.

---

## 1. Single Server Infrastructure (Basic)
**Components:**
- **1 Server** (hosts everything)
- **Nginx** as the web server
- **Application server** to execute code
- **Application files** (codebase)
- **MySQL database**
- **Domain name** `foobar.com` with a `www` A record pointing to `8.8.8.8`

**Flow:**
1. User requests `www.foobar.com`.
2. DNS resolves to server IP `8.8.8.8`.
3. Nginx handles HTTP requests, passes them to the app server.
4. App server executes code, queries MySQL if needed, and returns response.

**Issues:**
- Single Point of Failure (SPOF)
- Downtime during maintenance
- Cannot handle large traffic spikes

---

## 2. Three Server Infrastructure (Load Balanced)
**Additional Components:**
- **HAProxy load balancer**
- **Two backend servers** (Nginx + App Server + MySQL)
- **Database replication** (Primary-Replica)

**Load Balancing:**
- **Algorithm:** Round Robin (distributes requests evenly)
- **Setup:** Active-Active (both servers handle traffic)

**Database Setup:**
- **Primary node:** Accepts writes
- **Replica node:** Read-only for scaling reads

**Issues:**
- Still has SPOF at the load balancer
- No firewall or HTTPS
- No monitoring

---

## 3. Three Server Infrastructure (Secured & Monitored)
**Additional Components:**
- **3 Firewalls** (one per server)
- **SSL certificate** (HTTPS for secure traffic)
- **3 Monitoring clients** (e.g., Sumologic data collectors)

**Security & Monitoring:**
- **Firewalls:** Control inbound/outbound network traffic
- **HTTPS:** Encrypts communication, protects user data
- **Monitoring:** Collects metrics/logs to detect issues

**Issues:**
- SSL termination at load balancer can expose internal traffic
- Only one MySQL node can accept writes
- All-in-one servers reduce specialization and scalability

---

## Monitoring QPS Example
To monitor Queries Per Second (QPS) for the web server:
- Configure the monitoring client to collect Nginx metrics.
- Parse access logs or enable Nginx status module.
- Send data to monitoring service for dashboarding and alerts.
