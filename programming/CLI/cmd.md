# BASH
* Abrir automaticamente o terminal = ctrl + shift + t
* divisão de path de diretórios = ../<dir>/<dir>/.
* limpeza de terminal = clear (ou ctrl + K)
* listar arquivos do diretório = ls
* mudança de diretório = cd <novo_dir> (.. funciona para diretório pai)
* ir direto para diretório raiz = cd ~
* criar novo diretório = mkdir <novo_dir>
* mostrar o conteúdo do arquivo = cat <nome_arquivo.ext>
* ajuda com qualquer comando = <comando> --help
* mover arquivos = mv <caminho_arquivo_atual> <caminho_arquivo_destino> 
* copiar arquivos = cp <caminho_arquivo_atual> <caminho_arquivo_destino> 
* apagar arquivos = rm <nome_arquivo>
* apagar dir vazios = rmdir <nome_dir>
* apagar dir não vazios = rm -r <nome_dir>
* seleção multipla com coringa = <padrão_antes>*<padrão_depois>
* busca sequencia de texto em conteúdo de arquivo = grep <"sequencia_texto"> <arquivo>
* exibição de texto em terminal = echo <texto_a_ser_exibido>
* editor de texto = vi <nome_arquivo.txt>
* mudança de permissões = chmod +x (o simbolo de + é para adição de permissões, e x é para permissão de execução)
* script bash = <nome_script>.sh (deve ser autorizado para execução)
* compactar arquivos:
  * com zip = zip <nome_do_arquivo_zip>.zip <arquivo_a_ser_zipado> ou zip -r <nome_do_arquivo_zip>.zip <diretorio_a_ser_zipado>
  * com tar = tar -czf <nome_do_arquivo_zip>.tar.gz <arquivo_ou_dir_a_ser_zipado> (create zipped file)
* ver dentro de arquivos compactados = less <nome_do_arquivo>
* suprimir linha de comando na execução do script = @<nome_comando> (dentro do script)
* redirecionamento de erros: 2> <nome_arquivo>
* definir variáveis temporárias de ambiente ou alterar variável de ambiente existente: export <nome_var>=<valor_var>
* visualizar conteúdo de variáveis de ambiente: echo $<nome_var>
* executar comandos como admin = sudo <comando>
* gerenciador de pacotes = apt-get, apt

# PROMPT
* Abrir automaticamente o terminal = Win + R (cmd + enter)
* divisão de path de diretórios = ..\<dir>\<dir>\.
* listar arquivos do diretório = dir
* mudança de diretório = cd <novo_dir> (.. funciona para diretório pai)
* ir direto para diretório raiz = cd /
* criar novo diretório = mkdir <novo_dir>
* mostrar o conteúdo do arquivo = type <nome_arquivo.ext>
* ajuda com qualquer comando = help <comando>
* mover arquivos = move <caminho_arquivo_atual> <caminho_arquivo_destino> 
* copiar arquivos = copy <caminho_arquivo_atual> <caminho_arquivo_destino> 
* renomear arquivos ou dir = rename <nome_antigo_arquivo> <novo_nome_arquivo>
* apagar arquivos ou dir vazios = del <nome_arquivo>
* apagar diretórios = rmdir <nome_dir>
* seleção multipla com coringa = <padrão_antes>*<padrão_depois>
* limpeza de terminal = cls
* comparação de conteúdo de dois arquivos = fc <arquivo1> <arquivo2>
* desligar o computador = shutdown
* visulizar e altarar data = date 
* busca sequencia de texto em conteúdo de arquivo = find <"sequencia_texto"> <arquivo>
* exibição de texto em terminal = echo <texto_a_ser_exibido>
* editor de texto = notepad <nome_arquivo.txt>
* script = <nome_script>.bat (com o pause dentro do script ele aguarda a interação do usuário com o terminal para continuar a execução)
* compactar arquivos:
  * com tar = tar -cf <nome_do_arquivo_zip>.zip <arquivos_ou_dir_a_ser_zipado> (create file)
* ver dentro de arquivos compactados = tar -tf <nome_do_arquivo>
* suprimir linha de comando na execução do script = @<nome_comando> off (dentro do script)
* redirecionamento de erros: 2> <nome_arquivo>
* definir variáveis temporárias de ambiente: set <nome_var>=<valor_var>
* visualizar conteúdo de variáveis de ambiente: echo %<nome_var>%
* alterar variável de ambiente existente: setx <nome_variável> <conteúdo> (setx path "%path%;C:Users\.." /M)
* executar comandos como admin = abrar o terminal como admin clicando no botão direito e selecionando como admin
* gerenciador de pacotes = winget (para windows 10 e 11), chocolatey (windows antes do 10, tem que ser instalado)
* procurar pacotes a serem instalados = winget search <nome_pacote>
* instalar pacotes = winget install -e --id <id_exato_do_pacote>


