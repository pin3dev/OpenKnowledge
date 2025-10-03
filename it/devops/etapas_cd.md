# 🚀 Etapas que garantem estabilidade no CD

### 1. **Artefato Imutável**

* O deploy sempre parte de um **artefato já validado pelo CI** (binário, imagem Docker, pacote).
* Esse artefato é o mesmo em **staging** e **produção**.
  👉 Exemplo: `docker pull minhaapp:v1.2.3`

---

### 2. **Ambientes Separados (Staging/UAT → Produção)**

* Nunca deployar direto em produção sem passar por staging/UAT.
* O staging deve ser o mais próximo possível do real (mesma infra, variáveis de ambiente parecidas).
* Pode incluir **testes de aceitação** automatizados ou manuais.

---

### 3. **Deploy Automatizado**

* O pipeline de CD deve implantar o artefato sem etapas manuais perigosas.
* Ferramentas:

  * Docker + Compose / Kubernetes
  * Terraform / Ansible (infra)
  * GitHub Actions, GitLab CI/CD, ArgoCD (fluxo de CD)

---

### 4. **Configuração Segura e Isolada**

* Secrets/variáveis de ambiente nunca vão no código.
* Usar **Vaults ou Secrets Manager** (AWS/GCP/Azure ou nativo do GitHub/GitLab).
* Configuração por ambiente: `APP_ENV=staging` / `APP_ENV=prod`.

---

### 5. **Estratégias de Deploy Seguras**

* **Blue-Green**: duas versões, troca instantânea sem downtime.
* **Canary Release**: liberar para % dos usuários antes de 100%.
* **Rolling Update**: atualizar instâncias gradualmente.
* Sempre com rollback rápido.

---

### 6. **Monitoramento Pós-Deploy**

* Após subir, validar **logs, métricas e alertas**.
* Ferramentas: Prometheus, Grafana, ELK, Datadog.
* Se falhar → rollback automático ou manual imediato.

---

### 7. **Aprovação Manual (opcional, mas recomendada)**

* Em sistemas críticos, o CD pode exigir um **“botão de aprovar”** antes de ir pra produção.
* Garante que só humanos liberam versões sensíveis.

---

### 8. **Rollback Rápido**

* Todo pipeline de CD deve ter rollback pronto:
  👉 Exemplo: `kubectl rollout undo deployment minhaapp`
* Se a versão nova der problema, voltar para última estável em minutos.

---

#### 🔎 Resumindo o fluxo

1. CI gera **artefato imutável** (binário/imagem).
2. CD pega esse artefato → deploy em **staging/UAT**.
3. Testes de aceitação / QA → aprovados.
4. CD executa deploy em **produção** (automático ou com aprovação manual).
5. Monitoramento pós-deploy → rollback se necessário.


## Recursos Teóricos

* Alura Cursos:

  * ⭐⭐⭐⭐⭐ **Docker: Construindo imagem para produção (Vinícius Dias)**
  * ⭐⭐⭐⭐⭐ **GitHub Actions: Integrando e Entregando código com segurança (Vinícius Dias)**

