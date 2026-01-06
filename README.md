
# Kubernetes Multi-Node Cluster with NFS Storage, PHP Applications & Monitoring Stack

This project demonstrates a self-hosted multi-node Kubernetes environment that mimics a small production-grade setup.  
It includes a containerized application stack, persistent storage, load balancing, and a full monitoring & alerting system.

---

## 🚀 Project Overview

The system consists of:

- **Application Layer:** PHP applications deployed as Kubernetes Deployments.
- **Database Layer:** MariaDB running as a StatefulSet with NFS-backed persistent volumes.
- **Storage Layer:** NFS Subdir External Provisioner providing dynamic RWX storage.
- **Monitoring Layer:** Prometheus, Grafana, Alertmanager (installed via Helm).
- **Networking Layer:** MetalLB for LoadBalancer IPs and NodePort access.
- **Cluster Architecture:** 1 master node + 2 worker nodes.

This setup replicates essential concepts of production Kubernetes clusters, including persistent storage, service exposure, monitoring, and application orchestration.

---

## 📝 Project Summary

Key capabilities:

- Multi-node Kubernetes cluster (1 control plane, 2 workers)
- Dynamic NFS storage provisioning (RWX)
- MariaDB StatefulSet with persistent volume storage
- PHP applications served via NodePorts / LoadBalancer IPs
- Monitoring stack:
  - Prometheus (metrics collection)
  - Grafana (dashboards)
  - Alertmanager (alert routing)
- MetalLB LoadBalancer for external network access
- Helm-managed deployments with persistent storage configuration

---

## 🎯 Project Goals

- Deploy a fully functional Kubernetes cluster for learning & testing.
- Run a multi-container PHP + MariaDB application stack.
- Implement persistent storage using an NFS server.
- Install a metrics, visualization, and alerting stack.
- Validate real-world Kubernetes concepts: StatefulSets, PVCs, LoadBalancers, Helm, and monitoring.

---

##	High-Level Workflow

<img width="562" height="672" alt="image" src="https://github.com/user-attachments/assets/d9c3769a-89d5-4104-863c-e6cb37ce88c3" />

---

## 🛠 System Architecture

### **Components**
- **Kubernetes Nodes:**  
  - 1 master, 2 workers  
- **Applications:** PHP apps (Deployments)
- **Database:** MariaDB (StatefulSet)
- **Monitoring:** Prometheus + Grafana + Alertmanager
- **Storage:** NFS Subdir External Provisioner
- **Networking:** MetalLB (Layer 2)
- **StorageClasses:** `nfs-storage`, `local-path`, `standard`

### **Devices & Network**
- **Nodes:** Ubuntu 24.04 LTS (2 CPU, 4GB RAM each)
- **Network:** `192.168.1.0/24`
- **MetalLB range:** `192.168.1.240–250`
- **NFS folders:**  
  - `/srv/nfs/k8s-storage` (monitoring)  
  - `/srv/nfs/mariadb` (database)

---

## 📦 Cluster Node Overview

| Node Name      | Role          | IP Address       | OS                  | Runtime        | K8s Version |
|----------------|---------------|------------------|----------------------|----------------|-------------|
| k8s-master     | Control Plane | 192.168.1.197    | Ubuntu 24.04.3 LTS  | containerd 2.1 | v1.30.14    |
| k8s-worker1    | Worker        | 192.168.1.199    | Ubuntu 24.04.3 LTS  | containerd 2.1 | v1.30.14    |
| k8s-worker2    | Worker        | 192.168.1.198    | Ubuntu 24.04.3 LTS  | containerd 2.1 | v1.30.14    |

---


## ⚙️ Technical Specs

- **Kubernetes:** v1.30.14  
- **Runtime:** containerd 2.1  
- **OS:** Ubuntu 24.04  
- **Database:** MariaDB with NFS PVC  
- **Monitoring:** Prometheus, Grafana, Alertmanager  
- **Storage:** NFS dynamic provisioning  
- **Networking:** MetalLB + NodePort  

### **Persistence Configuration**
- **Grafana:** 10Gi NFS PVC, NodePort 32000  
- **Prometheus:** 20Gi NFS PVC, 15-day retention  
- **Alertmanager:** 5Gi NFS PVC (RWX)
- **StorageClasses:** local-path, nfs-storage, standard

---

## Topology Diagram

<img width="572" height="431" alt="image" src="https://github.com/user-attachments/assets/a8c64f61-b43d-4bb5-af37-9299e1fc0077" />

* Master Node: Hosts control-plane components (API server, scheduler, controller manager).
*	Worker Nodes: Host application workloads, MariaDB, and monitoring components.

Node Details
*	k8s-master: Control-plane node with Kubernetes API, scheduler, controller, and NFS server.
*	k8s-worker1 & k8s-worker2: Run application containers, MariaDB pods, and monitoring pods.
  
---

## 📊 Flowcharts

### **Deployment Flow**

      [NFS Server Ready] --> [Deploy StorageClass] --> [Deploy PVC/PV] --> [Deploy StatefulSet/Deployment] --> [Configure Services] --> [Monitor via Grafana]

### **Application Access Flow**

      [User Browser] --> [MetalLB IP / NodePort] --> [PHP App Service] --> [PHP Pod]

### **Monitoring Flow**

      [Cluster Metrics] --> [Prometheus] --> [Alertmanager] --> [Alerts/Notifications]

--- 

## 📁 Project File Structure 

<img width="768" height="592" alt="image" src="https://github.com/user-attachments/assets/d0d17b06-31bb-412e-8847-e4a4d1b47cf7" />

_Uses: The files define deployment specifications, storage provisioning, services, ingress, and Helm chart configurations_


---

## ✔️ Outcomes & Validation

- Multi-node Kubernetes cluster deployed successfully.
- Persistent storage fully functional via NFS dynamic provisioning.
- MariaDB StatefulSet & PHP apps validated with working PVCs.
- Prometheus, Grafana, and Alertmanager fully operational.
- External access enabled using MetalLB and NodePort services.
- Helm used to manage monitoring stack with persistent storage enabled.

### **Screenshots**
### 🎯 Cluster Overview

      *	kubectl get nodes -o wide → shows Master + Workers Ready.
      *	kubectl get pods -A → all pods Running

<img width="813" height="430" alt="image" src="https://github.com/user-attachments/assets/be151ad9-2891-45ae-a1f3-d168d953c38d" />

### 📈 Application Deployment

    kubectl get pods and kubectl get svc → show PHP & MariaDB pods.

<img width="828" height="219" alt="image" src="https://github.com/user-attachments/assets/7df57fd5-f0d1-472e-abce-d71efc7e4fe6" />

### 📁 Persistent Storage

    kubectl get pv and kubectl get pvc → show PVs bound to MariaDB.
 
<img width="865" height="173" alt="image" src="https://github.com/user-attachments/assets/7af9e331-6662-4317-9307-d2b631bbe58d" />

### 🚪External Access
 
<img width="840" height="112" alt="image" src="https://github.com/user-attachments/assets/f5e4f74f-3599-4cb7-bb02-438c7fbfb5f5" />
<img width="800" height="99" alt="image" src="https://github.com/user-attachments/assets/eee1dea6-d5d3-42b5-a1dd-80dda8d663be" />
<img width="795" height="498" alt="image" src="https://github.com/user-attachments/assets/b90e4760-2363-406d-98e7-72d6d815c065" />

### 📊 Monitoring

**Grafana Dashboard**
<img width="825" height="328" alt="image" src="https://github.com/user-attachments/assets/253e072e-6e1c-4540-b030-c746af54f299" />
<img width="818" height="305" alt="image" src="https://github.com/user-attachments/assets/3c20bbf8-ca2e-489e-a785-45a49972f0a8" />
<img width="820" height="484" alt="image" src="https://github.com/user-attachments/assets/5a77011d-85a6-4140-b496-18cdb657215f" />

---






 
 

