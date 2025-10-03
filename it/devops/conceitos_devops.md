# 🔑 Os 12 Fatores

> O **12-Factor App** é um conjunto de **boas práticas para desenvolvimento de aplicações modernas** criado em 2011 por engenheiros da Heroku. Ele virou uma **referência em DevOps e arquitetura de software**, especialmente para apps que vão rodar em ambientes de **CI/CD, containers e cloud**. A ideia é simples: cada "fator" é um princípio que ajuda a criar apps **portáveis, resilientes, escaláveis e fáceis de manter**.

### 1. **Codebase** – Uma base de código, múltiplos deploys

* Uma aplicação = um repositório de código versionado.
* Pode ter vários ambientes (dev, staging, prod), mas o código é único.
  👉 Exemplo: um repo Git no GitHub, usado em dev e em produção.

---

### 2. **Dependencies** – Dependências explícitas e isoladas

* Nunca depender do sistema host.
* Declarar dependências em arquivos (`requirements.txt`, `package.json`, `CMakeLists.txt`, etc.).
  👉 Exemplo: no Go, usar `go.mod`; no C++ com CMake, declarar libs externas.

---

### 3. **Config** – Configuração em variáveis de ambiente

* Separar **configuração de código**.
* Nada de senhas/URLs hardcoded.
  👉 Exemplo: `DB_HOST=prod-db.local APP_ENV=production`.

---

### 4. **Backing Services** – Tratar serviços externos como recursos anexados

* Banco de dados, fila, cache, storage → tudo acessado via URL/credenciais, nunca embutido.
  👉 Exemplo: trocar Redis local por Redis em cloud apenas mudando a URL.

---

### 5. **Build, Release, Run** – Separar etapas do ciclo de vida

* **Build**: compila o app em um artefato (binário, imagem Docker).
* **Release**: combina build + config do ambiente.
* **Run**: executa a aplicação.
  👉 Importante em CI/CD: o *build* deve ser único e imutável.

---

### 6. **Processes** – Executar como processos sem estado

* A aplicação não deve guardar estado em memória/disco local.
* Estado deve estar em serviços externos (DB, cache, S3, etc.).
  👉 Exemplo: em C++/Go, não salvar sessões em arquivo local → usar Redis.

---

### 7. **Port Binding** – Exportar serviços via porta

* O app deve ser independente de servidor web externo.
  👉 Exemplo: Go roda direto `http.ListenAndServe(":8080", nil)` em vez de depender de Apache/Nginx embutido.

---

### 8. **Concurrency** – Escalar com processos

* Melhor escalar horizontalmente (vários processos/containers) do que inflar um único processo.
  👉 Exemplo: Kubernetes subindo várias réplicas do mesmo app.

---

### 9. **Disposability** – Inicialização e desligamento rápidos

* O app deve iniciar rápido e encerrar sem problemas.
* Facilita **deploys frequentes**, **auto-scaling** e **recuperação de falhas**.
  👉 Exemplo: Go apps geralmente iniciam em milissegundos, ideal para containers.

---

### 10. **Dev/Prod Parity** – Manter ambientes o mais parecido possível

* Evitar diferenças entre dev e prod.
* Usar Docker ou pipelines para que todos os ambientes rodem da mesma forma.
  👉 Exemplo: build imutável que roda em dev, staging e prod sem recompilar.

---

### 11. **Logs** – Tratar logs como fluxo de eventos

* O app escreve logs em **stdout/stderr**.
* Sistemas externos (ELK, Loki, Splunk) coletam e processam.
  👉 Exemplo: `std::cout` em C++ ou `fmt.Println` em Go.

---

### 12. **Admin Processes** – Tarefas administrativas como processos únicos

* Scripts de migração de banco, jobs pontuais → devem rodar no mesmo ambiente e código da app.
  👉 Exemplo: rodar `go run migrate.go` no mesmo container que roda a aplicação.

---

# 📌 Em resumo

O **12-Factor App** garante que sua aplicação seja:

* **Portável** (roda em qualquer ambiente).
* **Escalável** (cresce fácil em cloud e containers).
* **Resiliente** (menos acoplada a infra).
* **Amigável ao CI/CD** (build único, config externa, logs corretos).

## Recursos Teóricos
* ⭐⭐⭐⭐⭐ https://12factor.net/pt_br/build-release-run