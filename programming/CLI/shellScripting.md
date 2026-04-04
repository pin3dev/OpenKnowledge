De maneira bastante similar ao que aprendemos em lógica de programação, quando implementamos um script no shell também podemos testar uma condição para direcionar a execução de diferentes blocos de instruções.

Usamos o comando condicional ifpara avaliar uma condição e direcionar o próximo passo na execução do código. O trecho de código a seguir apresenta a sintaxe adotada no Bash para execução do comando.

if [ condição ]; then
  # Comandos a serem executados se a condição testada for verdadeira.
elif [ outra condição ]; then
  # Comandos a serem executados se a primeira condição testada for falsa e a segunda condição testada for verdadeira.
else
  # Comandos a serem executados se nenhuma das condições testadas for verdadeira.
fi
Copiar código
Repare que a sintaxe do comando possibilita o teste de várias condições, permitindo a execução de diferentes blocos de comandos com base nesses testes.

Na criação dos testes adotamos operadores relacionais e lógicos de diferentes maneiras, como vemos nos exemplos a seguir:

Igualdade entre duas strings

if [ "$string1" = "$string2" ]; then
  # Comandos a serem executados se as strings forem iguais.
fi
Copiar código
Desigualdade entre duas strings

if [ "$string1" != "$string2" ]; then
  # Comandos a serem executados se as strings forem distintas.
fi
Copiar código
Igualdade entre dois números

if [ "$numero1" -eq "$numero2" ]; then
  # Comandos a serem executados se os números forem iguais.
fi
Copiar código
Desigualdade entre dois números

if [ "$numero1" -ne "$numero2" ]; then
  # Comandos a serem executados se os números forem distintos.
fi
Copiar código
Testando se um número é maior que outro

if [ "$numero1" -gt "$numero2" ]; then
  # Comandos a serem executados se o primeiro número for maior que o segundo.
fi
Copiar código
Testando se um número é menor que outro

if [ "$numero1" -lt "$numero2" ]; then
  # Comandos a serem executados se o primeiro número for menor que o segundo.
fi
Copiar código
Testando se um número é maior ou igual a outro

if [ "$numero1" -ge "$numero2" ]; then
  # Comandos a serem executados se o primeiro número for maior ou igual ao segundo.
fi
Copiar código
Verificando a existência de um arquivo ou diretório

if [  -e "/caminho/do/arquivo" ]; then
  # Comandos a serem executados caso seja constatada a existência do diretório ou arquivo.
fi
Copiar código
Note que as expressões condicionais devem estar entre [ ] e os espaços em branco são importantes na sintaxe.Os valores de strings devem ser colocados entre aspas para evitar problemas com espaços e caracteres especiais.




A passagem de parâmetros em scripts em Bash no Ubuntu é uma forma de fornecer informações ou argumentos para o script durante sua execução. Isso torna os scripts mais flexíveis e reutilizáveis, pois seu comportamento é ajustado de acordo com os argumentos fornecidos.

Essa passagem de parâmetros é realizada por meio de variáveis especiais, conhecidas como variáveis de posição. Elas são numeradas de 1 a 9, com $1 representando o primeiro argumento, $2 representando o segundo, e assim por diante. Além disso, todos os argumentos posicionais podem ser acessados através do $@.

A seguir, temos um exemplo de script que verifica se foram fornecidos exatamente dois argumentos na linha de comando. Se não, ele exibe uma mensagem de erro. Caso contrário, ele atribui os valores dos argumentos às variáveis arg1 e arg2 e os imprime.

#!/bin/bash

if [ $# -ne 2 ]; then
  echo "Erro! Nao foram fornecidos dois argumentos"
  exit 1
fi

arg1=$1
arg2=$2

echo "O primeiro argumento é: $arg1"
echo "O segundo argumento é: $arg2"

