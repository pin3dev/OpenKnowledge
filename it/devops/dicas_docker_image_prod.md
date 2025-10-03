# 🐳 Dicas de Imagens Docker para Produção (PROD)

### 1. **Aproveitamento de Layers e Cache**

* O Docker constrói imagens em **camadas (layers)**, reutilizando cache sempre que possível.
* Por isso, **instruções que mudam pouco** (ex.: instalação de dependências do sistema) devem ficar no **topo do `Dockerfile`**.
* Já as **instruções que mudam com frequência** (ex.: cópia de código fonte) devem ficar **no final**, evitando que pequenas alterações quebrem o cache inteiro.

👉 **Exemplo:**

```dockerfile
# Bom exemplo
FROM ubuntu:22.04

# Passo estável: instalar dependências
RUN apt-get update && apt-get install -y libssl-dev

# Passo que muda sempre: copiar código
COPY . /app
```

---

### 2. **Artefato Imutável**

* A imagem Docker deve ser tratada como **um artefato imutável**, ou seja, não deve ser alterada manualmente após construída.
* Isso garante **reprodutibilidade**: a mesma imagem que passou nos testes será usada em produção.
* O processo de build deve ser **automatizado no pipeline de CI/CD**, para que cada versão gere uma imagem única.

👉 **Exemplo:**

```bash
# Build com tag única (versão ou commit hash)
docker build -t minhaapp:1.0.3 .

# Push para o registry
docker push minha-registry/minhaapp:1.0.3
```

---

### 3. **Multi-Stage Builds**

* Separar a imagem de **construção** da imagem **final** torna o processo mais limpo e a imagem final **mais leve**.
* A primeira stage contém ferramentas de build, compiladores, dependências pesadas etc.
* A segunda stage recebe apenas o **artefato final**, eliminando tudo que não é necessário para rodar a aplicação.

👉 **Exemplo:**

```dockerfile
# Stage 1: Build
FROM gcc:12 AS build
WORKDIR /src
COPY . .
RUN make

# Stage 2: Runtime
FROM debian:stable-slim
WORKDIR /app
COPY --from=build /src/bin/minhaapp /app/
CMD ["./minhaapp"]
```

➡️ Resultado: A imagem final contém apenas o executável e bibliotecas mínimas necessárias, reduzindo **tamanho** e **superfícies de ataque**.
