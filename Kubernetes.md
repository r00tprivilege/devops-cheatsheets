```markdown
# ☸️ Kubernetes Essentials for DevSecOps Engineers

> A practical set of Kubernetes commands focused on secure deployments, runtime visibility, and CI/CD integration within DevOps workflows.

---

## 📋 Cluster Context & Namespaces

- **Check current context**
  ```bash
  kubectl config current-context
  ```
- **List all contexts**
  ```bash
  kubectl config get-contexts
  ```
- **Switch context**
  ```bash
  kubectl config use-context <context-name>
  ```
- **List namespaces**
  ```bash
  kubectl get ns
  ```

---

## 🚀 Deployment & Pod Operations

- **Deploy application using manifest**
  ```bash
  kubectl apply -f deployment.yaml
  ```
- **Scale deployment**
  ```bash
  kubectl scale deployment secure-app --replicas=3 -n devsecops
  ```
- **Restart deployment**
  ```bash
  kubectl rollout restart deployment/secure-app -n devsecops
  ```
- **Delete pod**
  ```bash
  kubectl delete pod <pod-name> -n devsecops
  ```

---

## 🔍 Monitoring & Troubleshooting

- **View pod status**
  ```bash
  kubectl get pods -n devsecops
  ```
- **Describe pod (inspect events and configuration)**
  ```bash
  kubectl describe pod <pod-name> -n devsecops
  ```
- **Check logs**
  ```bash
  kubectl logs <pod-name> -n devsecops
  ```
- **Exec into a running container**
  ```bash
  kubectl exec -it <pod-name> -c <container-name> -- /bin/sh
  ```

---

## 🔐 Security & RBAC Checks

- **List service accounts**
  ```bash
  kubectl get serviceaccounts -n devsecops
  ```
- **Review role bindings**
  ```bash
  kubectl get rolebindings -n devsecops
  kubectl describe rolebinding <binding-name> -n devsecops
  ```
- **Audit security context of a pod**
  ```bash
  kubectl get pod <pod-name> -o jsonpath='{.spec.securityContext}'
  ```

---

## 🧪 DevSecOps Checks

- **Run vulnerability scan using Trivy K8s plugin**
  ```bash
  trivy k8s --namespace devsecops cluster
  ```
- **Scan workload configuration files**
  ```bash
  trivy config ./k8s/
  ```

---

## 🧼 Resource Cleanup

- **Delete all pods in namespace**
  ```bash
  kubectl delete pods --all -n devsecops
  ```
- **Delete deployment**
  ```bash
  kubectl delete deployment secure-app -n devsecops
  ```

---

## 🔄 CI/CD Automation Example (GitLab)

```yaml
deploy:
  stage: deploy
  script:
    - kubectl apply -f k8s/deployment.yaml
  only:
    - main
```

---

## 🔒 Hardening Recommendations

- Enforce PodSecurity standards via Admission Controllers
- Use NetworkPolicies to segment workloads
- Limit container capabilities with `securityContext`
- Prefer Secrets and ConfigMaps over hardcoded env variables
```

