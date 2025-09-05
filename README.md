# Bitnami Keycloak Deployment on AWS EKS

This repository provides a **streamlined Kubernetes deployment** for **Bitnami Keycloak**, a leading open-source identity and access management solution.  
Unlike the **bulky and complex Helm chart installations**, this setup is intentionally designed to be:

- **Lightweight** – Only the essential manifests you actually need  
- **Transparent** – Every resource is clear and customizable, no hidden complexity  
- **Flexible** – Easy to extend for themes, providers, storage, and scaling  
- **Production-Ready** – Works seamlessly with AWS EKS, ALB Ingress, and optional EFS persistence  

By using simple YAML manifests instead of large Helm templates, this solution gives you **full control and clarity** over your Keycloak deployment while still supporting advanced features like high availability, autoscaling, and persistence.


---

## 📌 Solution Architecture
- **Keycloak** with PostgreSQL database backend  
- **High Availability** using multiple replicas  
- **Horizontal Pod Autoscaling (HPA)** based on CPU and memory utilization  
- **Ingress** via AWS Application Load Balancer (ALB)  
- **Persistent Storage (optional)** using AWS EFS for:
  - Themes  
  - Providers / custom JARs  

---

## ✅ Prerequisites
Before deploying, ensure you have:

1. **AWS EKS Cluster** – A running Kubernetes cluster on EKS  
2. **PostgreSQL Database** – RDS, Aurora, or self-managed PostgreSQL  
3. **AWS Load Balancer Controller** – Installed in your cluster  

**Optional (for persistence):**
- **EFS CSI Driver** – Installed in your cluster  
- **AWS EFS File System** – Existing filesystem with known FileSystem ID  

> ℹ️ If you don’t want persistence, you can skip the EFS setup and remove the volume sections from `deployment.yaml`.

---

## 🚀 Deployment Steps
1. **Clone this repository:**
   ```bash
   git clone <your-repo-url>
   cd <your-repo-folder>


---

## 🌐 Accessing Keycloak
- **If hostname is configured in Ingress:**
  - Open the hostname you specified in your ingress configuration  

- **If hostname is not configured:**
  1. Get the ALB DNS name:
     ```bash
     kubectl get ingress -n identity keycloak -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
     ```
  2. Open in your browser:
     ```
     https://<ALB-DNS-NAME>
     ```

---

## 🔑 Default Credentials
- Initial admin credentials are set via environment variables in `deployment.yaml`.  
- **Best Practices:**
  - Change the default credentials immediately after first login  
  - Use **Kubernetes Secrets** for secure credential management in production  

---

## ⚙️ Customization

### Without Persistent Storage
- Remove `volumes` and `volumeMounts` sections from `deployment.yaml`
- As well as `PVCs` resources from your current directory by
- Delete or comment out:
  - `pvc.yaml`  
  - `storageClass.yaml`  
- Re-apply:
  ```bash
  kubectl apply -f .
  ```

### Updating Configuration
- Modify `configMap.yaml` as needed.  
- Restart Keycloak:
  ```bash
  kubectl rollout restart deployment/keycloak -n identity
  ```

## 📊 Monitoring & Scaling
HPA automatically scales pods:
- Target CPU: ~70%  
- Target Memory: ~75%  
- Replicas: 2 → 8  
Check HPA status:
```bash
kubectl get hpa -n identity
```

## 🛠️ Troubleshooting
Check resource status:
```bash
kubectl get all -n identity
```

## 🧹 Cleanup
To remove all resources:
```bash
kubectl delete -f .
```

## 📬 Support
For issues or support, feel free to reach out:  
- **LinkedIn:** [linkedin.com/in/fahad-muzzamil-849262194](https://www.linkedin.com/in/fahad-muzzamil-849262194)  
- **Email:** [fahadmuzzamil533@gmail.com](mailto:fahadmuzzamil533@gmail.com)  
---

## ⚠️ Notes
This deployment is designed for production use, but every environment has different requirements.  
- Test each manifest file carefully.  
- Adjust configuration before running critical workloads.  

  
