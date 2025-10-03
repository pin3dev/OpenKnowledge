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

### 🌳 Estrutura de um Workflow no GitHub Actions

```yaml
name: Nome do Workflow                # Nome do pipeline (opcional, mas útil)

on:                                   # Eventos que disparam o workflow
  push:                               # Dispara quando há commit (push)
    branches: [ "main" ]              # Especifica branch(es) monitoradas
  pull_request:                       # Dispara em Pull Requests
    branches: [ "main" ]              # Especifica branch(es) monitoradas
  workflow_dispatch:                  # Permite execução manual

jobs:                                 # Conjunto de jobs (tarefas independentes)
  build:                              # Nome do job (exemplo: build)
    runs-on: ubuntu-latest            # Runner (SO onde o job roda)
    
    env:                              # Variáveis de ambiente do job
      APP_ENV: test

    steps:                            # Lista de passos do job
      - name: Checkout código         # Nome do passo (descritivo)
        uses: actions/checkout@v3     # Usa uma action existente (action oficial)

      - name: Instalar dependências
        run: |                        # Comando(s) de shell
          make deps
          make build

      - name: Rodar testes
        run: make test

  test:                               # Outro job
    runs-on: ubuntu-latest
    needs: build                      # Executa apenas depois do job "build"
    steps:
      - name: Exemplo de variáveis
        run: echo "Ambiente: $APP_ENV"
```

---

#### 🔑 Explicação Tag por Tag

##### **Nível raiz**

* `name` → Nome amigável do workflow.
* `on` → Define **quando** o workflow deve rodar.

  * `push` → a cada commit.
  * `pull_request` → ao abrir/atualizar PR.
  * `workflow_dispatch` → execução manual.
* `jobs` → Conjunto de jobs (cada job é uma sequência de passos).

---

##### **Dentro de jobs**

* `<nome_do_job>` → Identificador do job (`build`, `test`, `deploy`, etc.).
* `runs-on` → Define em qual runner (máquina virtual) o job será executado (`ubuntu-latest`, `windows-latest`, `macos-latest`).
* `env` → Variáveis de ambiente disponíveis para todos os steps do job.
* `needs` → Dependência entre jobs (ex.: `test` só roda se `build` passar).
* `steps` → Lista de passos que compõem o job.

---

##### **Dentro de steps**

* `name` → Nome amigável do step (descritivo no painel do Actions).
* `uses` → Indica que o step vai usar uma **action existente** (ex.: `actions/checkout@v3`).
* `run` → Executa comandos de shell diretamente no runner.
* `with` → Passa parâmetros para a action usada no `uses`.
* `env` (opcional) → Variáveis de ambiente só para esse step.

---

É importante observar a estrutura, já que o indexamento é essencial para seu correto funcionamento.

```
Workflow (arquivo YAML)
│
├── name:           # Nome do workflow
├── on:             # Eventos que disparam
│   ├── push
│   ├── pull_request
│   └── workflow_dispatch
│
└── jobs:
    ├── <job_name>:
    │   ├── runs-on   # Sistema operacional
    │   ├── needs     # Dependência entre jobs
    │   ├── env       # Variáveis de ambiente do job
    │   └── steps:
    │       ├── name  # Nome do step
    │       ├── uses  # Action pronta
    │       ├── run   # Comando de shell
    │       ├── with  # Parâmetros da action
    │       └── env   # Variáveis só desse step
    │
    └── <outro_job>...
```

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