
variáveis:
- quando não iniciadas, n permitem que o programa seja compilado
- tipos: int, double, boolean, char, String* (classe, por isso é o único com letra maiúscula)
- nomes: aceita letras, digitos (exceto no primeiro caracter), e underscore (de preferencia exceto no primeiro caracter).
- case sensitive
- norma: costuma-se usar mais o camelCase, mas há possibilidade de snake_case
- norma2: separaçao clara entre palavras, com camelCase ou snake_case
constantes:
- aceita mesmas regras das variáveis
- norma: sempre em caixa alta
input & output:
- tipos de inputs & outputs: consola, ficheiros e sockets
- função main: `public static void main (String [] args)`
- stdout: 
    -`System.out.println("");`
    -`System.out.print("");`
    -`System.out.println("");`
- concatenação de stdout: `System.out.println("string1" + var + "string2");`
- scapes: iguais a c/c++
- biblioteca: `java.util.Scanner` para input de consola
- habilita input do terminal: `Scanner input = new Scanner(System.in);`
- parser de tipos no input: 
    - `varNumero = input.nextInt();`
    - `varNumero = input.nextDouble();`
    - `varNumero = input.next();` //pega o \0 ? 

IntelliJ
- botao: New Projeto
- name: FichaPraticas
- location: dir local (ivany_dir)
- build system: [x]Intellij (iniciante, com gestao de pastas), []Maven, []Gradle
- JDK: Download JDK25
- dir main: src/
- estrutura do projeto: src/Package/Java-Class-Ex0x
    - um Package para cada FichaPratica0x
    - um Java class para cada exercício
    - um main para cada Java class exercício

- Resolver a Ficha técnica 1