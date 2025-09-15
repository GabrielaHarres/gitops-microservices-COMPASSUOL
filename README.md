# gitops-microservices-COMPASSUOL

# GitOps – Online Boutique (Argo CD + Kubernetes local)

Deploy da aplicação **Online Boutique** no Kubernetes (Rancher Desktop) usando **Argo CD** e um único manifesto versionado em Git.

## 🧰 Tecnologias utilizadas

- **Kubernetes (k3s via Rancher Desktop)** – Deployments, Services (ClusterIP/LoadBalancer), Namespaces, `port-forward`.
- **Argo CD** – CRD `Application`, conexão de repositório, *Auto-Sync*, *Prune* e *Self-Heal* (GitOps).
- **Docker (dockerd/moby)** – runtime de contêineres usado pelo Rancher Desktop.
- **Git & GitHub** – versionamento dos manifests e *source of truth* do GitOps.
- **Online Boutique (Google microservices-demo)** – conjunto de microserviços containerizados em **múltiplas linguagens** (Go, Java, Node.js/TypeScript, Python, C#), com imagens públicas em:
  `us-central1-docker.pkg.dev/google-samples/microservices-demo/*`
- **YAML** para definição declarativa de recursos.
- **Windows CMD/PowerShell** + ferramentas (`kubectl`, `curl`/`Invoke-WebRequest`).


## ✅ Pré-requisitos

- **Rancher Desktop** com Kubernetes habilitado (engine `dockerd (moby)`)
- **kubectl** configurado (contexto do Rancher Desktop)
- **Git** instalado
- **Repositório no github**

## ⚡ TL;DR (passos rápidos)

1) Verificar o cluster:
```bash
kubectl get nodes
```

2) Instalar ArgoCD:
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

3) Abrir a UI do ArgoCD:
```bash
kubectl port-forward -n argocd svc/argocd-server 8080:443
```
Acesse https://localhost:8080

Usuário: admin

A senha é gerada automaticamente. Use este comando para vê-la:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
Acesse este site para decodificar a senha: https://www.base64decode.org/

4) Conectar o repositório na UI
Settings → Repositories → CONNECT REPO

Repo URL: https://github.com/<seu-usuario>/<seu-repositorio>.git

Auth: No auth (se público)

5) Criar a Application na UI
Applications → NEW APP

Name: online-boutique

Project: default

Repository URL: (seu repo)

Revision: HEAD

Path: k8s

Destination Cluster: https://kubernetes.default.svc

Namespace: default

Auto-sync: Enable

Create → Sync

6) Verificar:
   ```bash
   kubectl get pods -n default
   kubectl get svc  -n default
   ```

7) Acessar o frontend:
```bash
kubectl port-forward -n default svc/frontend 8081:80
```

Abra http://localhost:8081

# 🔍 Comandos úteis

Aguardar Deployments prontos:
```bash
kubectl wait --for=condition=Available deployment --all -n default --timeout=600s
```

Listar pods e services:
```bash
kubectl get pods -n default
kubectl get svc  -n default
```

AUTHOR: Gabriela Harres
