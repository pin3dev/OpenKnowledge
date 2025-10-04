# 🛠️ Construção do Workflow

Conforme explicado nas etapas do CI, veremos a seguir como colocá-las em prática utilizando o **GitHub Actions**.
O objetivo do build automático é garantir que o código compila sem erros e que o artefato gerado pode ser executado em ambiente controlado.

---

## ⚙️ Build Automático

### 🔹 1. Build de executável na VM do Actions

Compila o projeto diretamente no runner do GitHub Actions (Linux, Windows ou macOS), como se fosse um servidor físico/VM local.

Nesse caso:

* Todas as **dependências** (bibliotecas, serviços externos, bancos de dados, arquivos de configuração) precisam estar disponíveis.
* Em ambiente de produção, essas dependências podem estar em outros servidores (ex.: banco externo).
* Durante o CI, é comum usar **containers auxiliares (Docker)** para simular essas dependências.

**Exemplo:**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Instalar dependências
        run: sudo apt-get update && sudo apt-get install -y g++ make

        # Alternativamente, para linguagens específicas:
        # uses: actions/setup-go@v4
        # with:
        #   go-version: '1.21'

      - name: Compilar projeto
        run: make
        # ou
        # run: <comando_para_compilar_na_linguagem>
# ...
```

**Observações:**

* `actions/checkout@v3` → Faz o checkout do repositório no runner (baixa o código-fonte).
* `actions/setup-<linguagem>` → Actions prontas para instalar compiladores/interpretadores (ex.: `setup-go`, `setup-python`, `setup-node`, `setup-java`).
* O runner `ubuntu-latest` **já vem com `gcc`, `g++`, `make` e `cmake` instalados**. Use `apt-get` apenas para dependências extras.

---

### 🔹 2. Build de Docker na VM do Actions

Constroi uma ou mais imagens Docker a partir dos `Dockerfile`s presentes no repositório.
Essa abordagem é útil quando o artefato final em produção será um container.

**Exemplo com Action oficial do Docker:**

```yaml
jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Build imagem Docker
        uses: docker/build-push-action@v5
        with:
          context: .              # Caminho do diretório com Dockerfile
          push: false             # Não publica no DockerHub, apenas builda
          tags: meu-app:ci        # Nome/tag da imagem (ex.: versão de CI)

        # Alternativa com comando manual:
        # run: docker build -t meuuser/meuapp:${{ github.sha }} .
# ...
```

**Observações:**

* `actions/checkout@v3` → Faz checkout do código no runner.
* `docker/build-push-action@v5` → Action oficial para **buildar e publicar imagens Docker**.

  * `push: false` → apenas builda.
  * `push: true` → builda e envia a imagem para o registry (ex.: Docker Hub, GHCR, AWS ECR).


## Controle de branches e PRs/MRs*

Defina uma branch principal estável e restrinja pushes diretos a ela, permitindo apenas merges via Pull Request revisados por outro desenvolvedor (quando houver mais de um colaborador no projeto).

### Restrição de Branch por Classic Protect Rules

- **Branch name pattern**
    - `<nome_branch_a_proteger>`
- **Protect matching branches**
    - [x] Require a pull request before merging
         - [x] Require approvals (limita PR a aprovação de outros)
    - [x] Require status checks to pass before merging 
    - Searching for status in the last week for this repository: 
        - `<nome_do_job_no_workflow>`

### Restrição de Branch por Rulesets

- **Ruleset Name** 
    - `<nome_para_o_conjunto_de_regras_de_uma_ou_mais_branches>`. 
- **Enforcement status**
    - `Active`. 
- **Bypass List**
    - `<users_groups_orgs_que_podem_ignorar_regras>`. 
- **Targets**
    - Target branches:  
        - Add target:  
            `<criterio_padrao_de_branches_para_aplicacao_de_regra>` ex.: `default`. 
- **Rules**
    - [x] **Require a pull request before merging**  
        - Require approvals:  
            - `<n_de_approvals>`

    - [x] **Require status checks to pass**  
        - Add checks:  
            - `<nome_do_verificador_de_status>` (ex.: `<nome_do_job_do_github_actions>`)  
        - Any source:  
            - `githubActions`
    - [x] **Block force pushes**

## 🔍 Análise Estática e Qualidade de Código

A análise estática garante que o código segue padrões de qualidade e boas práticas, detectando erros cedo no pipeline. Essa etapa varia conforme a linguagem utilizada e pode ser integrada no GitHub Actions com ferramentas específicas.

```yml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      # C/C++ → clang-tidy, cppcheck
      - name: Rodar clang-tidy
        run: clang-tidy src/*.cpp -- -I./include

      # Go → golangci-lint
      # uses: golangci/golangci-lint-action@v3
      # with:
      #   version: latest
      #   args: run ./...
#...
```

**Observações:**

* `actions/checkout@v3` → baixa o repositório no runner.
* Ferramentas variam por linguagem, mas seguem o mesmo padrão: instalar → rodar análise.


## ✅ Testes Automatizados (com Environments)

Testes automatizados podem ser de **unidade** (isolados) ou de **integração** (com serviços externos).
Nos testes unitários, é necessário apenas compilar/rodar o código em teste.
Nos de integração, é comum precisar de variáveis e serviços auxiliares, que mudam conforme o ambiente (dev, staging, prod).

No GitHub Actions, podemos usar **Environments** para organizar essas configurações.
Cada ambiente (ex.: `staging`, `production`) pode ter **variáveis e secrets próprios**, definidos no repositório.

**Exemplo de Teste Unitários:**
```yml
jobs:
    test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Rodar testes
        run: make test
```

**Exemplo de Teste de Integração com DB descartável:**
```yml
jobs:
  integration-test:
    runs-on: ubuntu-latest
    environment: test   # Grupo de vars/secrets do ambiente de teste
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_USER: user
          POSTGRES_PASSWORD: pass
          POSTGRES_DB: testdb
        ports:
          - 5432:5432
        options: >-
          --health-cmd="pg_isready -U user"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=5

    steps:
      - uses: actions/checkout@v3

      - name: Rodar testes de integração
        env:
          APP_ENV: test
          DB_HOST: localhost
          DB_USER: user
          DB_PASS: pass
          DB_PORT: 5432
        run: |
          export DATABASE_URL=postgres://${DB_USER}:${DB_PASS}@${DB_HOST}:${DB_PORT}/testdb
          ./run_integration_tests.sh
```

**Exemplo de Teste de Integração com DB em Staging:**
```yml
jobs:
  integration-test:
    runs-on: ubuntu-latest
    environment: test   # Define o grupo de variáveis/secrets que será usado
    steps:
      - uses: actions/checkout@v3

      - name: Rodar testes de integração contra Staging
        env:               # Variáveis herdadas do environment "staging"
          APP_ENV: staging
          DB_HOST: ${{ secrets.DB_HOST }}
          DB_USER: ${{ secrets.DB_USER }}
          DB_PASS: ${{ secrets.DB_PASS }}
          DB_PORT: ${{ secrets.DB_PORT }}
        run: |
          export DATABASE_URL=postgres://${DB_USER}:${DB_PASS}@${DB_HOST}:${DB_PORT}/stagingdb
          ./run_integration_tests.sh
```

**Observações:**

* `environment` → escolhe o grupo de variáveis/secrets já configurado no GitHub (Settings → Environments).
* `secrets` → usados para valores sensíveis (senhas, tokens, chaves de API).
* `env` → usados para valores não sensíveis (flags, nomes de ambiente, log level).
* Permite separar bem os contextos:

  * **development** → usado em builds/testes locais.
  * **staging** → ambiente de homologação, simula produção.
  * **production** → ambiente real, com secrets críticos.


## 📦 Artefato Imutável

Para linguagens compiladas, o resultado do build (binário, executável ou imagem Docker) deve ser tratado como um **artefato imutável**.
Isso significa:

* O mesmo build deve ser usado em **todos os ambientes** (test, staging, prod).
* O artefato não deve ser recompilado a cada deploy.
* Apenas **configuração** muda entre ambientes (via variáveis de ambiente/secrets).

Essa prática garante rastreabilidade, previsibilidade e elimina o clássico problema do “funciona na minha máquina”.

### 🔹 1. Exemplo com executável compilado

```yml
jobs:
  build-artifact:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Compilar binário
        run: make build     # Gera ./app-executavel

      - name: Upload binário como artefato
        uses: actions/upload-artifact@v3
        with:
          name: app-binary
          path: ./app-executavel
```

> [!TIP]
> O binário gerado (`./app-executavel`) é salvo como **artefato imutável**.
> Esse mesmo binário pode ser baixado em outros jobs ou pipelines (staging/prod).


### 🔹 2. Exemplo com imagem Docker

```yml
jobs:
  docker-build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Build e tag da imagem
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: meuuser/meuapp:${{ github.sha }}
```

> [!TIP]
> Aqui o **artefato imutável** é a imagem Docker, versionada com o hash do commit.
> Essa imagem é a mesma que vai para staging e produção.


**Observações:**

* **Imutável** significa que o artefato não sofre alterações após o build.
* O que muda entre ambientes é **somente configuração** (ex.: `DATABASE_URL`, `LOG_LEVEL`).
* Cada artefato deve ser versionado (ex.: `v1.0.0`, `build-20251003`, `sha-1234567`).
* Isso facilita rollback rápido e auditoria: você sabe exatamente qual versão está em produção.

---

Conforme explicado nas etapas do CD, veremos a seguir como colocá-las em prática utilizando o **GitHub Actions**.


## 🧱 Deploy Automatizado (Docker Hub)

Quando o artefato é uma imagem Docker, o GitHub Actions pode automatizar o push para o Docker Hub.
Esse é o ponto mais direto de CD dentro da pipeline do Actions.

**Exemplo completo de job de deploy:**
```yml
jobs:
  deploy-docker:
    runs-on: ubuntu-latest
    needs: docker-build   # job de build no CI
    environment: production
    steps:
      - uses: actions/checkout@v3

      - name: Login no Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build e push da imagem para Docker Hub
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ${{ secrets.DOCKERHUB_USERNAME }}/meuapp:latest
            ${{ secrets.DOCKERHUB_USERNAME }}/meuapp:${{ github.sha }}
```

**Observações**

* `docker/login-action@v3` → autentica no Docker Hub usando secrets.
* `docker/build-push-action@v5` → constrói e envia a imagem para o registry.
* `tags:` → define versões rastreáveis (latest e commit hash).
* Esse job pode ser disparado automaticamente após o merge na main, ou manualmente via workflow_dispatch.

## 🧩 Separação de Ambientes (Staging → Production)

Embora o GitHub Actions não hospede o app em si, ele pode gerenciar diferentes fluxos de deploy, cada um com seus secrets e variáveis.

**Exemplo de deploy para dois ambientes**
```yml
jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - name: Push imagem para Docker Hub (staging)
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: meuuser/meuapp:staging

  deploy-prod:
    runs-on: ubuntu-latest
    environment: production
    needs: deploy-staging
    steps:
      - name: Push imagem para Docker Hub (prod)
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: meuuser/meuapp:latest, ${{ github.sha }}
```

**Observações**

* Cada environment no GitHub pode ter secrets próprios, como tokens de produção ou staging.
* A ordem dos jobs (needs:) garante que staging seja feito antes da produção.
* Você pode restringir o ambiente de production para exigir aprovação manual antes do deploy.


## 🔐 Configuração Segura
As configurações e credenciais (usuário, senha, tokens, endpoints etc.) devem ficar armazenadas nos secrets do repositório ou do ambiente, nunca no YAML.

```yml
env:
  DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}
  API_KEY: ${{ secrets.PROD_API_KEY }}
```

## 🧭 Aprovação Manual (gates de segurança)
O GitHub Actions permite gates manuais via Environment Protection Rules, úteis para o deploy em produção.

**Como configurar**

* Vá em: `Settings → Environments → production → Required reviewers`.
* Adicione os usuários que precisam aprovar o deploy.
* O job de deploy fica pendente até aprovação manual.

## 🔁 Rollback Rápido (via tags Docker)

Se uma versão falhar, é possível restaurar uma imagem anterior apenas reaplicando uma tag.

**Exemplo Manual**
```yml
docker pull meuuser/meuapp:sha-antigo
docker tag meuuser/meuapp:sha-antigo meuuser/meuapp:latest
docker push meuuser/meuapp:latest
```

**GitHub Actions**
```yml
jobs:
  rollback:
    runs-on: ubuntu-latest
    steps:
      - name: Retag imagem anterior como latest
        run: |
          docker pull meuuser/meuapp:${{ inputs.rollback_tag }}
          docker tag meuuser/meuapp:${{ inputs.rollback_tag }} meuuser/meuapp:latest
          docker push meuuser/meuapp:latest
```

##### Recursos Teóricos
* Alura Cursos:
   - ⭐⭐⭐⭐⭐ **Integração Contínua: pipelines e testes automatizados com Github Actions (Vinícius Dias)**
   - 