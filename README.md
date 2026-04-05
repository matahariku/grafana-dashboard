# Grafana Email Alerting - Multi-Environment Guide

## 📧 Gmail App Password (PREREQUISITE)
1. https://myaccount.google.com/apppasswords
2. Login febdx33000@gmail.com  
3. 2FA ON → App passwords → Generate → **COPY 16-char**

## 🎯 METHOD 1: Native Install (grafana.ini)
```bash
sudo nano /etc/grafana/grafana.ini
# [smtp] section → restart grafana-server
```

## 🎯 METHOD 2: Kubernetes (Secret + ENV) ⭐ RECOMMENDED
```bash
# Secret + patch deployment (yang udah sukses!)
kubectl create secret generic grafana-smtp ...
kubectl patch deployment grafana ...
```

## ✅ VERIFICATION
```bash
kubectl logs deployment/grafana -n monitoring | grep smtp
Grafana UI → Contact Points → Test ✅
```


## 🎯 PRODUCTION SETUP (monitoring namespace)

### **1. Buat SMTP Secret**
```bash
APP_PASS="abcd1234efgh5678"  # ← 16-char App Password
kubectl create secret generic grafana-smtp \
  --from-literal=smtp-user=febdx33000@gmail.com \
  --from-literal=smtp-password=$APP_PASS \
  -n monitoring
```

### **2. Patch Grafana Deployment ENV**
```bash
kubectl patch deployment grafana -n monitoring --type='json' -p='[
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_ENABLED","value":"true"}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_HOST","value":"smtp.gmail.com:587"}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_USER","valueFrom":{"secretKeyRef":{"name":"grafana-smtp","key":"smtp-user"}}}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_PASSWORD","valueFrom":{"secretKeyRef":{"name":"grafana-smtp","key":"smtp-password"}}}},
  {"op":"add","path":"/spec/template/spec/containers/0/env/-","value":{"name":"GF_SMTP_FROM_ADDRESS","value":"febdx33000@gmail.com"}}
]'
```

### **3. Restart & Verify**
```bash
kubectl rollout restart deployment grafana -n monitoring
kubectl rollout status deployment/grafana -n monitoring
kubectl logs deployment/grafana -n monitoring | grep smtp  # ✅ GF_SMTP_HOST loaded
```

### **4. Grafana UI Contact Point**
