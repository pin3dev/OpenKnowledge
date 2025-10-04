## AWS
1. fazer conta gratuita na aws 
2. criar instacia de ec2
    [executar instancias]
        nome: <nome_da_instancia>
        imagem: Amazon Linux t2.micro(Qualificado para nivel gratuito)
        par de chaves: Criar novo par de chaves
            nome do par de chaves: <nome_par_chaves>
            tipo de par de chaves: RSA
            Formato de arquivo de chave privada: .pem(para linux e MacOS) .ppk (para windows)
            * salvar par de chaves em pasta no pc local
            [Criar par de chaves]
        configuração de rede
            Firewall: Selecionar grupo de segurança existente
            Grupos de segurança comuns: <default>
        configurar armazenamento
            1x 8GiB gp3 volume raiz (padrão)
        resumo
            numero de instâncias: 1
    [executar instancia]
3. criar instância de rds
    [criar banco de dados]
        escollher um método de criação de banco de dados: criação padrão
        opção de mecanismo: postgreSQL (nivel gratuito)
        versão do mecanismo: <versão_mais_recente>
        modelo: nivel gratuito
        configurações
            identificador de instancia de dados: <nome_da_instancia>
            nome do usuário principal: postgres
            gerenciamento de credenciais
                [x]gerar senha automaticamente
            armazenamento
                tipo de armazenamento: <minimo_disponivel_versao_gratuita>
                armazenamento alocado: <minimo_disponivel_versao_gratuita>
            conectividade
                recurso de computação: não se conectar a um recurso de computação do EC2
                tipo de rede: IPv4
                nuvem pivada virtual (VPC): default
                grupo de sub-redesde banco de dados: dafault
                Acesso publico: não
                grupo de segurança
                    selecionar grupo existente
                        grupos de segurança da VPC existentes: default
                porta do banco de dados: 5432
            autenticação de banco de dados: autenticação de senha
            monitoramento
                []ativar o Perfomance Insights
            configuração adicional
                opções de banco de dados
                    nome do banco de dados inicial: <nome_do_banco>
                    grupo de parâmetros do banco de dados: <default> **mas pode ser configurado depois
            backup: []habilitar backups automaticos
            criptografia: []habilitar criptografia
            manutenção: []habilitar o upgrade automatico da versao secundária
    [criar banco de dados]
4. copie a senha do DB cliando em [visualizar detalhes de credencial] e salve no pc
5. configurar parâmetros de RDS
    [criar grupo de parâmetros]
        detalhes do grupo de parametros
            nome do grupo de parametros: <nome_do_grupo>
            descrição: <descricao_sobre_parametros>
            tipo de mecanismo: <postgres> *nesse caso
            familia: <versao_mais_recente_do_postgres>
            tipo: DB parameter Group
6. modificar parametros rds
    [editar]
    parametros modificaveis
        buscar: ssl
        rds.force_ssl = 0
        [salvar alterações]
7. atualizar grupo de parametros do RDS
    [modificar]
        opcoes de banco de dados
            grupo de parametros do banco de dados: <nome_do_grupo_criado>
    [continuar]
        [x]aplicar imediatamente
    [reinicializar] -> [confirmar]
8. configuração de acesso
    [rede e segurança]
        security group
            selecionar o grupo <default>
                regras de entrada
                    [editar regras de segurança]
                    [adicionar regra]
                        regra de entrada 2
                            tipo: SSH
                            tipo de origem: qualquer local-IPv4
                            origem: 0.0.0.0/0
                            descrição: <nome_regra>
                        regra de entrada 3
                            tipo: TCP personalizado
                            intervalo de porta: 8080
                            tipo de origem: qualquer local-IPv4
                            origem: 0.0.0.0/0
                            descrição: <nome_regra>
        [salvar]
## LOCAL PARA EC2
1. copie a chave `.pem` para `~/.ssh/`
2. mude as permissões para `sudo chmod 400 ~/.ssh/<nome_da_chave.pem>`
3. cole o comando disposto em conexão da instância do ec2 na aws, para acessar a instância da ec2 pelo terminal, algo como `ssh -i "chave..."`
4. com o comando `scp -i <chave.pem> <arquivo_local_a_ser_copiado> <endereço_do_ec2>:<path_dentro_do_ec2_para_copia_do_arq_local>` vc copia um arquivo local para a instancia ec2 na aws
5. com o comando `scp -ri <chave.pem> <diretorio_local_a_ser_copiado> <endereço_do_ec2>:<path_dentro_do_ec2_para_copia_do_dir_local>` vc copia um diretório local para a instancia ec2 na aws
6. para rodar o executavel em ambiente cloud, devem ser passados todas as variaveis de ambiente necessarias no sistema antes de executar o executavel, exemplo: `DB_HOST=<endepoint_db> ... ./main`

>[!IMPORTANT]
> Quando só é usado teste unitários, eles não precisam do artefato final, pois usam a funções e simular um funcionamento por outro ponto de entrada, então a etapa de build não precisa ser a primeira nesse pipeline, podendo ser testes (unitários e lint) -> build -> deploy
> Quando há etapa de teste integração então é necessário a estrutura padrão de build -> testes -> deploy para o teste com a aplicação. 

## AUTOMAÇÃO COM GITHUB ACTION
para fazer deploy automatico do artefato na ec2 pelo github action os mesmo passo a passo deve ser seguido pelo actions build -> copia para ec2 por `scp`

```yml
jobs:
   deploy_Ec2:
       runs-on: ubuntu-latest
       steps:
       - uses: actions/checkout@v4

       - name: Set Up and Go
         uses: actions/setup-go@v4
         with:
           go-version: '1.22'

       - name: Build
         run: go build main.go

       - name: SCP to EC2
         # Action muito usada pela comunidade para envio de arquivos para instancia ec2
         uses: appleboy/scp-action@v0.1.7
           with:
              host: ${{ secrets.EC2_HOST }}
              username: ${{ secrets.EC2_USERNAME }}
              key: ${{ secrets.ECS_ SECRET_KEY}}
              source: "arquivo1, arquivo2"
              target: "diretorio_dentro_do_ec2_destino"

        # OPÇÃO MANUAL 
        # run: |
        #   scp ...
```

## AUTOMAÇÃO DA EC2 LINUX
Até o momento mesmo que o executavel da aplicação já se encontre disponivel noec2 da aws quando atualizado ele não é capaz de reiniciar a aplicação com a nova versão, tendo que ser feito manualmente. Nessa etapa vamos tentar automatizar esse feito através de configuração de serviços no ambiente Linux.

serviços em Linux são processos que são gerenciados pelo SO através de utilitários que fica rodando em loop e caso o processo finalize são reiniciados automaticamente, assim podemos configurar serviços para rodarem no linux nesses parametros.

o diretorio `/etc/systemd/system` guarda os arquivos de definição de todos os serviços `*.service` no linux

---

### ⚙️ Estrutura de um Arquivo `.service`

Um arquivo `.service` define **como um serviço (processo)** deve ser **iniciado, parado, reiniciado** e **monitorado** pelo `systemd`.

Local comum:

* `/etc/systemd/system/` → para serviços do sistema
* `~/.config/systemd/user/` → para serviços de usuário

---

#### 🧩 Estrutura Geral

Um `.service` é dividido em **seções**:

```ini
[Unit]
Description=Meu serviço de exemplo
After=network.target

[Service]
ExecStart=/usr/local/bin/meu_programa
Restart=always
User=www-data
WorkingDirectory=/var/www/app

[Install]
WantedBy=multi-user.target
```

Vamos destrinchar **cada parte** 👇

---

#### 🧱 `[Unit]` — Metadados e dependências

Essa seção **descreve o serviço** e **define ordem de inicialização**.

| Diretiva               | Função                                                                                         |
| ---------------------- | ---------------------------------------------------------------------------------------------- |
| `Description=`         | Texto descritivo (aparece em `systemctl status`).                                              |
| `Documentation=`       | Links ou caminhos de documentação (ex: `man:nginx(8)`).                                        |
| `After=`               | Garante que este serviço inicie **depois** de outro alvo/serviço (ex: `After=network.target`). |
| `Before=`              | O inverso: inicia **antes** de outro.                                                          |
| `Requires=`            | Define uma **dependência obrigatória**: se o serviço listado falhar, este também será parado.  |
| `Wants=`               | Dependência **opcional**: é iniciado junto, mas se falhar, não afeta este.                     |
| `ConditionPathExists=` | Só inicia se o arquivo/caminho existir.                                                        |
| `Conflicts=`           | Impede execução simultânea de outro serviço.                                                   |

---

## ⚙️ `[Service]` — Comportamento do processo

Aqui você define **como o processo é executado** e **gerenciado**.

| Diretiva                             | Função                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Type=`                              | Define o **tipo de inicialização** do processo. Os principais são:<br> - `simple`: inicia e assume que o processo principal é o `ExecStart`.<br> - `forking`: usado por daemons que se auto-desanexam (como scripts antigos).<br> - `oneshot`: executa e termina (ex: scripts de inicialização).<br> - `notify`: o processo sinaliza quando está pronto (ex: systemd-notify). |
| `ExecStart=`                         | Comando para iniciar o serviço. (obrigatório)                                                                                                                                                                                                                                                                                                                                 |
| `ExecStop=`                          | Comando para parar o serviço.                                                                                                                                                                                                                                                                                                                                                 |
| `ExecReload=`                        | Comando para recarregar configuração sem reiniciar.                                                                                                                                                                                                                                                                                                                           |
| `Restart=`                           | Política de reinício. Opções:<br> `no`, `on-success`, `on-failure`, `on-abnormal`, `always`.                                                                                                                                                                                                                                                                                  |
| `RestartSec=`                        | Tempo (em segundos) antes de reiniciar.                                                                                                                                                                                                                                                                                                                                       |
| `User=`                              | Usuário que executará o processo.                                                                                                                                                                                                                                                                                                                                             |
| `Group=`                             | Grupo do processo.                                                                                                                                                                                                                                                                                                                                                            |
| `WorkingDirectory=`                  | Define o diretório de trabalho.                                                                                                                                                                                                                                                                                                                                               |
| `Environment=`                       | Define variáveis de ambiente. Ex: `Environment="APP_ENV=prod"`.                                                                                                                                                                                                                                                                                                               |
| `PIDFile=`                           | Caminho do arquivo de PID, se o processo gerar um.                                                                                                                                                                                                                                                                                                                            |
| `TimeoutStartSec=`                   | Tempo máximo para iniciar antes de marcar falha.                                                                                                                                                                                                                                                                                                                              |
| `TimeoutStopSec=`                    | Tempo máximo para parar antes de forçar kill.                                                                                                                                                                                                                                                                                                                                 |
| `KillMode=`                          | Como os processos filhos são terminados (`control-group`, `process`, `mixed`, etc).                                                                                                                                                                                                                                                                                           |
| `StandardOutput=` / `StandardError=` | Redirecionamento de logs (`journal`, `syslog`, `null`, `file:/path`).                                                                                                                                                                                                                                                                                                         |

---

#### 🚀 `[Install]` — Ativação e destino

Essa parte **liga o serviço a um alvo (target)**, ou seja, quando ele será iniciado automaticamente.

| Diretiva      | Função                                                                                |
| ------------- | ------------------------------------------------------------------------------------- |
| `WantedBy=`   | Define o “alvo” de ativação — o mais comum é `multi-user.target` (modo multiusuário). |
| `RequiredBy=` | Similar a `WantedBy`, mas cria dependência obrigatória.                               |
| `Alias=`      | Permite nomes alternativos para o serviço.                                            |

---

#### 🧠 Exemplo completo

```ini
[Unit]
Description=Servidor Web em Go
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/var/www/myapp
ExecStart=/usr/local/bin/myapp
Restart=on-failure
RestartSec=5
Environment="PORT=8080" "APP_ENV=production"
StandardOutput=file:/var/log/journal.out.log
StandardError=file:/var/log/journal.err.log

[Install]
WantedBy=multi-user.target
```

---

#### 🔍 Comandos úteis

| Comando                                     | Função                                         |
| ------------------------------------------- | ---------------------------------------------- |
| `sudo systemctl daemon-reload`              | Recarrega as configurações dos serviços.       |
| `sudo systemctl start meu-servico.service`  | Inicia o serviço.                              |
| `sudo systemctl stop meu-servico.service`   | Para o serviço.                                |
| `sudo systemctl status meu-servico.service` | Mostra o status e logs.                        |
| `sudo systemctl enable meu-servico.service` | Habilita para iniciar automaticamente no boot. |
| `sudo journalctl -u meu-servico`            | Mostra logs do serviço.                        |


#### Automação de serviços por SSH Actions

para automatizar o processo de habilitar e desabilitar, (ligar/desligar) o serviço no linux através do github actions é necessário habilitar o uso de ssh para acesso a ec2 pelo action, com actions que já existam.

```yml
       - name: Restart service
         # Action muito usada pela comunidade para envio de arquivos para instancia ec2
         uses: appleboy/ssh-action@v1.0.3
           with:
              host: ${{ secrets.EC2_HOST }}
              username: ${{ secrets.EC2_USERNAME }}
              key: ${{ secrets.ECS_ SECRET_KEY}}
              script: sudo systemctl restart projeto #vai reiniciar o serviço no ec2 e vai dar um downtime
```
##### Recursos Teóricos
* Alura Cursos:
   - ⭐⭐⭐⭐⭐ **Github Actions: integrando e entregando código com segurança (Vinícius Dias)**