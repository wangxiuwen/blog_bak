---
layout: posts
---

# helm tls 证书配置


```sh
export HELM_TLS_ENABLE=true
export HELM_TLS_CA_CERT=/etc/kubernetes/pki/ca.pem
export HELM_TLS_CERT=/etc/kubernetes/pki/admin.pem
export HELM_TLS_KEY=/etc/kubernetes/pki/admin-key.pem
```
