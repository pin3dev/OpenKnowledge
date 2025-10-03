# 🌳 Estrutura de um Workflow no GitHub Actions

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

# 🔑 Explicação Tag por Tag

### **Nível raiz**

* `name` → Nome amigável do workflow.
* `on` → Define **quando** o workflow deve rodar.

  * `push` → a cada commit.
  * `pull_request` → ao abrir/atualizar PR.
  * `workflow_dispatch` → execução manual.
* `jobs` → Conjunto de jobs (cada job é uma sequência de passos).

---

### **Dentro de jobs**

* `<nome_do_job>` → Identificador do job (`build`, `test`, `deploy`, etc.).
* `runs-on` → Define em qual runner (máquina virtual) o job será executado (`ubuntu-latest`, `windows-latest`, `macos-latest`).
* `env` → Variáveis de ambiente disponíveis para todos os steps do job.
* `needs` → Dependência entre jobs (ex.: `test` só roda se `build` passar).
* `steps` → Lista de passos que compõem o job.

---

### **Dentro de steps**

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

