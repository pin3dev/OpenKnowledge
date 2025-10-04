# ☁️ Visão Geral dos Serviços da AWS

A **Amazon Web Services (AWS)** é a plataforma de nuvem mais completa e madura do mercado, com **centenas de serviços** agrupados por categoria.
Esses serviços permitem construir desde uma simples aplicação web até sistemas distribuídos globais e escaláveis.

---

## 🧱 1. **Infraestrutura e Computação**

Serviços que **executam aplicações, containers e funções**:

| Categoria | Serviço | Descrição |
| :-: | :-: | :-: |
| 🖥️ Máquinas Virtuais | **EC2 (Elastic Compute Cloud)** | Permite criar e gerenciar VMs configuráveis (CPU, RAM, SO). É a base da AWS. |
| ⚙️ Containers | **ECS (Elastic Container Service)** | Gerencia e executa containers Docker, sem precisar de orquestrador próprio. |
| ☸️ Orquestração | **EKS (Elastic Kubernetes Service)** | Serviço gerenciado de **Kubernetes**. AWS cuida dos nodes e do control plane. |
| 🔥 Serverless | **Lambda** | Executa funções sob demanda, sem servidor fixo (paga só pelo tempo de execução). Ideal para automações e eventos. |
| 🧰 Automação de infraestrutura | **Elastic Beanstalk** | Faz deploy automático de aplicações (gera EC2, Load Balancer e Auto Scaling automaticamente). |
| 🧮 HPC | **Batch** | Executa workloads pesadas (ex.: simulações científicas, renderizações) em lote. |

---

## 📦 2. **Armazenamento**

Serviços voltados a **guardar arquivos, blocos e backups**:

| Categoria | Serviço | Descrição |
| :-: | :-: | :-: |
| 🪣 Objetos | **S3 (Simple Storage Service)** | Armazenamento de objetos (arquivos), altamente escalável e redundante. Muito usado para imagens, logs, backups, artefatos. |
| 💽 Blocos | **EBS (Elastic Block Store)**   | Volumes de bloco anexados a EC2 (equivalente a discos SSD). |
| 📂 Arquivos | **EFS (Elastic File System)** | Sistema de arquivos NFS escalável e compartilhado entre instâncias. |
| 🧊 Arquivamento | **Glacier** | Armazenamento barato para backup de longo prazo. |
| 📦 Transferência | **Storage Gateway** | Integra armazenamento local com a nuvem (para migrações ou backup híbrido). |

---

## 🌐 3. **Rede e Entrega de Conteúdo**

Serviços que conectam e distribuem o tráfego globalmente:

| Categoria | Serviço | Descrição |
| :-:| :-: | :-: |
| 🌍 Redes privadas  | **VPC (Virtual Private Cloud)** | Rede isolada na AWS onde ficam suas instâncias, sub-redes e firewalls. |
| 🔁 DNS | **Route 53** | Serviço de DNS gerenciado com balanceamento de carga e failover. |
| 🌐 CDN | **CloudFront** | Rede de entrega de conteúdo (cache global para sites e APIs). |
| ⚖️ Balanceamento  | **ELB (Elastic Load Balancer)** | Distribui tráfego entre múltiplas instâncias (Application, Network ou Gateway LB). |
| 🔒 Conexões seguras | **VPN / Direct Connect** | Conectam data centers locais à AWS com segurança. |

---

## 🧠 4. **Banco de Dados e Cache**

Serviços para **armazenar dados estruturados ou não estruturados**:

| Tipo | Serviço | Descrição                                                                |
| :-: | :-: | :-: |
| 🧮 Relacional | **RDS (Relational Database Service)** | Bancos SQL gerenciados (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server). |
| ⚙️ Escalável | **Aurora** | Banco relacional otimizado da AWS (compatível com MySQL/PostgreSQL). |
| 📄 NoSQL | **DynamoDB** | Banco de dados key-value totalmente gerenciado e serverless. |
| 🧩 Cache | **ElastiCache** | Cache in-memory (Redis ou Memcached). |
| 📊 Analytics  | **Redshift** | Data warehouse para análises de grandes volumes de dados.|

---

## 🧰 5. **DevOps e Automação**

Ferramentas para **CI/CD, monitoramento e gestão de infra como código**:

| Categoria | Serviço | Descrição |
| :-: | :-: | :-: |
| ⚙️ Infraestrutura como Código | **CloudFormation** | Descreve e provisiona recursos AWS via templates YAML/JSON. |
| ☁️ Gerenciamento de estados | **Terraform (terceiro, usado com AWS)** | Alternativa open source para IaC. |
| 🔄 Pipelines | **CodePipeline** | Orquestra pipelines de CI/CD (integra CodeBuild e CodeDeploy). |
| 🧱 Build | **CodeBuild** | Serviço gerenciado de build (compila, testa, empacota). |
| 🚀 Deploy | **CodeDeploy** | Automatiza deploys em EC2, Lambda ou ECS. |
| 🕵️ Monitoramento | **CloudWatch** | Métricas, logs e alarmes centralizados. |
| 🧩 Observabilidade | **X-Ray** | Faz tracing distribuído de aplicações. |
| 🛠️ Configuração | **Systems Manager (SSM)** | Automatiza tarefas e gerencia parâmetros/segredos. |

---

## 🧑‍💻 6. **Segurança, Identidade e Acesso**

Serviços para **controle de acesso e proteção**:

| Categoria | Serviço | Descrição |
| :-: | :-: | :-: |
| 🔐 Identidade | **IAM (Identity and Access Management)** | Controla permissões de usuários, grupos e roles. |
| 🧩 Credenciais centralizadas | **AWS Organizations** | Gerencia múltiplas contas AWS com políticas unificadas. |
| 🧱 Firewall | **WAF (Web Application Firewall)** | Protege aplicações web contra ataques (SQLi, XSS, etc.). |
| 🧯 Segurança de rede | **Shield** | Proteção contra DDoS para apps na AWS. |
| 🕵️ Logs e auditoria | **CloudTrail** | Registra todas as ações e chamadas na conta AWS. |
| 🔑 Segredos e chaves | **Secrets Manager / KMS** | Armazena segredos e gerencia criptografia. |

---

## 📊 7. **Análise de Dados e IA**

| Categoria | Serviço | Descrição |
| :-: | :-: | :-: |
| 📈 Análise de dados | **Athena** | SQL direto em dados do S3. |
| 🚰 Pipeline de dados | **Glue** | ETL (extração, transformação e carga) serverless. |
| 🤖 Machine Learning | **SageMaker** | Treina e implanta modelos de ML em escala. |
| 🧠 Serviços prontos de IA | **Rekognition, Polly, Comprehend, Lex** | Reconhecimento de imagem, voz, texto e NLP. |

---

## 🧩 8. **Ferramentas de Integração e Mensageria**

| Serviço | Descrição |
| :-: | :-: |
| **SQS (Simple Queue Service)** | Fila para comunicação assíncrona entre sistemas. |
| **SNS (Simple Notification Service)** | Sistema de publicação/assinatura para notificações (pub/sub). |
| **EventBridge** | Roteia eventos entre serviços AWS e aplicações. |
| **Step Functions** | Orquestra fluxos de tarefas (workflows serverless). |

---

## 🧠 Resumo visual (visão mental para DevOps/SRE)

```
Infraestrutura  →  EC2 / ECS / EKS / Lambda
Rede            →  VPC / ELB / Route53 / CloudFront
Armazenamento   →  S3 / EBS / EFS / Glacier
Banco de Dados  →  RDS / DynamoDB / Aurora / ElastiCache
DevOps          →  CodePipeline / CodeBuild / CodeDeploy / CloudWatch / CloudFormation
Segurança       →  IAM / WAF / Shield / Secrets Manager / CloudTrail
```


