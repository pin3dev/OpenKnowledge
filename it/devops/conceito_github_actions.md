# ⚙️ GitHub Actions

### Conceito

O **GitHub Actions** é uma **plataforma de automação de workflows** integrada diretamente ao GitHub.
Ele permite que desenvolvedores configurem pipelines de **CI/CD** (Integração Contínua e Entrega/Implantação Contínua) sem precisar de servidores externos ou ferramentas adicionais.

Os workflows são descritos em arquivos **YAML** no diretório `.github/workflows/` e são executados em resposta a eventos como `push`, `pull request`, criação de `tag`, entre outros.

---

### GitHub Actions como CI (Continuous Integration)

* Atua como um **servidor de integração contínua hospedado pelo GitHub**.
* Em cada commit ou PR, é possível:

  * Compilar o projeto.
  * Rodar testes automatizados.
  * Analisar a qualidade do código (lint, segurança, style check).
* Isso garante que a branch principal (`main/master`) esteja sempre estável.

---

### GitHub Actions como CD (Continuous Delivery/Deployment)

* Pode ser usado para **automatizar deploys** do software.
* Exemplos comuns:

  * Publicar imagens Docker em registries.
  * Criar releases automáticas no GitHub.
  * Implantar diretamente em serviços de cloud (AWS, Azure, GCP, Kubernetes).
* Dependendo da configuração, o deploy pode exigir aprovação manual (Entrega Contínua) ou ser automático (Implantação Contínua).

---

### Estrutura de um Workflow

Um workflow é composto por:

* **Evento (trigger)** → define quando será executado (ex.: `push`, `pull_request`).
* **Jobs** → conjunto de tarefas que podem rodar em paralelo ou sequência.
* **Steps** → comandos individuais (ex.: build, teste, deploy).
* **Runners** → máquinas que executam os jobs (GitHub fornece runners Linux, Windows e macOS; também é possível usar runners self-hosted).

---

### Comparação com outras ferramentas

* **Jenkins** → requer instalação e manutenção de um servidor dedicado.
* **GitLab CI/CD** → integrado ao GitLab, mas exige configuração própria de runners.
* **GitHub Actions** → nativo do GitHub, simples de usar e já integrado ao repositório.

---

### ⚡ Resumindo

O **GitHub Actions** é uma solução de **CI/CD nativa do GitHub**, que:

* Automatiza builds, testes e análises de código (CI).
* Gerencia releases e deploys em múltiplos ambientes (CD).
* Substitui a necessidade de servidores externos dedicados de integração contínua.


## Recursos Teóricos
* Alura Cursos:
   - ⭐⭐⭐⭐⭐ **Integração Contínua: pipelines e testes automatizados com Github Actions (Vinícius Dias)**
   - 