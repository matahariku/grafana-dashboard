i# 🚀 Grafana Production Monitoring Stack
## Kubernetes + GitOps + Email Alerting | SRE febdx

![Grafana](https://img.shields.io/badge/Grafana-v12.3.1-orange)
![Kubernetes](https://img.shields.io/badge/K8s-monitoring-blue) 
![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-green)
![Email](https://img.shields.io/badge/Email-Gmail-brightgreen)

---

## 📁 Repository Structure

📁 grafana-dashboard/          ⭐ 100% GitOps Ready!  /
├── README.md                 ✅ Email alerting guide  /
├── argocd-app.yaml           ✅ ArgoCD GitOps /
├── dashboards/
│   ├── cluster/              ✅ K8s monitoring /
│   ├── golang/               ✅ Golang observability  /
│   └── laravel/              ✅ Laravel FPM prod /
└── provisioning/
    ├── dashboards.yaml       ✅ Auto-provision dashboards /
    └── datasources.yaml      ✅ Prometheus datasource 


---

## 🚨 Email Alerting - Production Setup

### 📧 **Gmail App Password (PREREQUISITE)**

https://myaccount.google.com/apppasswords

Login: xxxx@gmail.com

2FA ON → App passwords → Generate

Select: Mail → "Grafana" → COPY 16-char code



### 🎯 **Kubernetes Setup (monitoring namespace)** ⭐ RECOMMENDED

#### **1. Create SMTP Secret**
```bash
APP_PASS="abcd1234efgh5678"  # Your 16-char App Password
kubectl create secret generic grafana-smtp \
  --from-literal=smtp-user=febdx33000@gmail.com \
  --from-literal=smtp-password=$APP_PASS \
  -n monitoring
```

#### **2. Patch Grafana Deployment ENV**
```bash
kubectl patch deployment grafana -n monitoring --type='json' -p='[
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_ENABLED","value":"true"}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_HOST","value":"smtp.gmail.com:587"}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_USER","valueFrom":{"secretKeyRef":{"name":"grafana-smtp","key":"smtp-user"}}}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_PASSWORD","valueFrom":{"secretKeyRef":{"name":"grafana-smtp","key":"smtp-password"}}}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_FROM_ADDRESS","value":"febdx33000@gmail.com"}}
]'
```

#### **3. Restart & Verify**
```bash
kubectl rollout restart deployment grafana -n monitoring
kubectl rollout status deployment/grafana -n monitoring
kubectl logs deployment/grafana -n monitoring | grep smtp
# ✅ logger=settings: "GF_SMTP_HOST=smtp.gmail.com:587"
```

#### **4. Test Contact Point**

Grafana UI → Alerting → Contact Points → + New → Email
Name: fe-email-alerts | Addresses: febdx33000@gmail.com
→ Test → ✅ "Test notification sent!"


---

## 📊 **Production Dashboards**

✅ CRD Validation Ratcheting Latency
✅ Golang Observability
✅ Kubernetes Monitoring Dashboard
✅ Laravel FPM Production v2.0
✅ Toko Nani Revenue


## 🎯 **Production Alerts LIVE**

🚨 Laravel Memory > 200MB (CRITICAL, FOR 5m)
⚠️ Laravel CPU > 80% (WARNING, FOR 2m)
🚨 Prometheus Down (CRITICAL, FOR 1m)


---

## ✅ **Verification (05-Apr-2026)**

✅ [x] 5 Production dashboards provisioned /
✅ [x] Email alerting Gmail LIVE /
✅ [x] GitOps ArgoCD ready /
✅ [x] Secret grafana-smtp created /
✅ [x] ENV patch deployment.grafana /
✅ [x] Logs: GF_SMTP_HOST loaded /
✅ [x] Email test successful



---

## 🎖️ **SRE Architecture**

🔒 Kubernetes Secret (credentials) /
⚙️ ENV Variables (Grafana native) /
📦 Provisioning (dashboards + datasources) /
🚀 ArgoCD GitOps (zero-downtime) /
📧 Gmail SMTP (production alerting)


---

## 🔄 **GitOps Deployment**
```bash
kubectl apply -f argocd-app.yaml
argocd app sync grafana-dashboard
```

---

**`Production SRE Monitoring Stack - Scale Ready!`**

*Deployed: 05-Apr-2026 | SRE: xxx | monitoring namespace*

FIXES: /
✅ Secret syntax correct /
✅ Struktur clean /
✅ Repo info COMPLETE /
✅ GitHub READY /
✅ Portfolio PRO

## 🚨 **Dual Channel Auto-Alerting PRODUCTION LIVE** ⭐

**06-Apr-2026 16:06 CEST - VERIFIED:**
[Grafana Alerts SRE Channel - LIVE!]
Firing
Value: B=22, C=1
alertname = TestAlert
Dashboard: https://grafana.xxx.net/d/...

text

**Production Alerts (OTOMATIK):**
🚨 Laravel Memory > 200MB → Email + Telegram
⚠️ Laravel CPU > 80% → Email + Telegram

text

**Notification Routing:**
Custom annotation: telegram-sre-febdx → DUAL CHANNEL
CHAT_ID: -1003804987196 | Bot: @xx_grafana_bot
