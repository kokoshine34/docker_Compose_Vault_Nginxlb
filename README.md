# 🚪 Vault Cluster with NGINX Load Balancer (Docker Compose)

This project sets up a **3-node HashiCorp Vault cluster** using **Raft storage** with an **NGINX load balancer** for high availability and request routing. It uses Docker Compose for easy orchestration and local development.

---

## 📦 What's Included

- `vault-server1`, `vault-server2`, `vault-server3`: Vault nodes running in HA mode with Raft storage
- `vault-nginx`: Load balancer routing client traffic to the Vault leader using `/v1/sys/health`
- Persistent volume mappings for Vault data, logs, and config
- A custom NGINX config that detects the active Vault leader via health checks

---

## 🛠 Prerequisites

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

---

## Design
![image](https://github.com/user-attachments/assets/c19cb662-692d-4d0d-9c7d-7548943d4192)

## 🚀 Getting Started

### 1. Clone the repo

git clone [https://github.com/your-username/vault-cluster-nginx.git](https://github.com/kokoshine34/docker_Compose_Vault_Nginxlb)
cd vault-cluster-nginx

-------------------------------------------------------------------------------------------------------------
2. Directory Structure
Make sure you have the following directories set up:

vault-cluster-nginx/
├── docker-compose.yml
├── vault-nginx/
│   └── nginx.conf
├── vault-server1/
│   ├── config/
│   ├── data/
│   ├── logs/
│   └── file/
├── vault-server2/
│   ├── config/
│   ├── data/
│   ├── logs/
│   └── file/
├── vault-server3/
│   ├── config/
│   ├── data/
│   ├── logs/
│   └── file/

--------------------------------------------------------------------------------------------------------------
3. Run the cluster

docker-compose up -d


⚙️ Vault Configuration Overview
Each Vault server is configured with:

Raft storage backend for consensus

Cluster and API addresses for HA mode

Listener on port 8200 (no TLS for local use)

IPC_LOCK capability for secure memory locking

Retry join logic (on non-leader nodes)

----------------------------------------------------------------------------------------------------------------
🔀 NGINX Load Balancer
NGINX uses the /v1/sys/health endpoint to detect the active Vault leader and routes traffic accordingly.

Sample block from nginx.conf:

----------------------------------------------------------------------------------------------------------------
🧪 Initialize & Unseal Vault
After the cluster is running, initialize Vault (only once) and unseal each node:

docker exec -it vault-server1 vault operator init

This will output unseal keys and root token — save them securely.

Then unseal:

docker exec -it vault-server1 vault operator unseal <key>
docker exec -it vault-server2 vault operator unseal <key>
docker exec -it vault-server3 vault operator unseal <key>

-------------------------------------------------------------------------------------------------------------------
✅ Tips
Make sure each Vault node has a data/ directory mapped for Raft

You can scale the cluster or simulate failovers

Set VAULT_ADDR=http://localhost:8200 when using the CLI

✨ Acknowledgements
HashiCorp Vault

NGINX

Docker

Lesson Learn
   Should nit reuse existing folder and database file. It will refresh like restore the old database might need the old unsealed keys.

This scenairo is not using config.hcl file. it directly config under conteiners enviroment VAULT_LOCAL_CONFIG.




