# 🔑 Etapas que garantem estabilidade no CI

### 1. **Controle de branches e PRs/MRs**

* Nunca commitar direto na `master`.
* Criar branch de feature/fix → abrir **Pull Request (GitHub)** ou **Merge Request (GitLab)**.
* O merge só acontece após passar no pipeline de CI.

---

### 2. **Build Automático**

* O CI deve **compilar** ou **buildar** o projeto em cada commit/PR.
* Se não compilar, não pode ir para `master`.
  👉 Exemplo (C++ com CMake): `cmake -DCMAKE_BUILD_TYPE=Release .`

---

### 3. **Testes Automatizados**

* **Testes unitários** → garantem que cada módulo funciona.
* **Testes de integração** → garantem que módulos se falam direito.
* (opcional) **Testes end-to-end** → simulam uso real.
  👉 No pipeline: `ctest` (C/C++) ou `go test ./...` (Go).

---

### 4. **Análise Estática e Qualidade de Código**

* Ferramentas que analisam bugs, segurança e estilo antes de mergear.
* Exemplo em C++: `clang-tidy`, `cppcheck`.
* Exemplo em Go: `golangci-lint`.

---

### 5. **Testes de Segurança / Dependências**

* Verificar vulnerabilidades em libs.
* Ferramentas: `trivy` (Docker/containers), `dependabot` (GitHub).

---

### 6. **Artefato Imutável**

* O pipeline deve gerar o **mesmo binário/imagem** que pode ir para dev/staging/prod.
* Isso evita o clássico “na minha máquina funciona”.

---

### 7. **Ambiente de Teste (Staging/UAT)**

* Antes de chegar em produção, o artefato é implantado num ambiente de teste que simula o real.
* Pode rodar testes de aceitação automatizados ou manuais.

---

### 8. **Revisão de Código + Aprovação Humana**

* Além do CI, o PR/MR precisa de **review de outro dev**.
* Isso pega falhas que o CI sozinho não detecta (design ruim, lógica estranha, etc.).

---

### 9. **Merge Protegido na Master**

* Regras no repositório:

  * `master` só aceita PR/MR.
  * CI precisa passar ✅.
  * Pelo menos 1 review aprovado.
  * (opcional) “sem commits quebrados”: só squash ou rebase.

---

## 🔎 Resumindo o fluxo

1. Dev cria branch → faz commit.
2. Abre PR/MR → CI roda automaticamente:

   * Build
   * Testes (unitários, integração)
   * Linter/Análise estática
   * Segurança básica
3. Se tudo passar → review humano.
4. Se aprovado → merge na `master`.
5. `master` fica sempre em estado estável.

---

👉 Em termos de **garantia de estabilidade**, o mais crítico é:

* **CI bloqueando merge se algo falhar**.
* **master protegida (nunca commit direto)**.
* **build/testes rodando em cada commit**.

## Recursos Teóricos
* Alura Cursos:
   - ⭐⭐⭐⭐⭐ **Integração Contínua: pipelines e testes automatizados com Github Actions (Vinícius Dias)**
   - 