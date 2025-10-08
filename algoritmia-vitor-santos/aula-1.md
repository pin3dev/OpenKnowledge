
# Aula 01 — Introdução à Programação em Java

---

## Variáveis

**Regras e boas práticas:**

1. **Inicialização obrigatória:**
    - Variáveis não inicializadas impedem a compilação do programa.
2. **Tipos principais:**
    - `int`, `double`, `boolean`, `char`, `String` *(classe, por isso começa com maiúscula)*
3. **Nomes de variáveis:**
    - Aceita letras, dígitos (exceto no primeiro caractere) e underscore `_` (preferencialmente não no início).
    - **Case sensitive** (diferencia maiúsculas de minúsculas).
4. **Normas de nomenclatura:**
    - Preferência por `camelCase`, mas `snake_case` também é aceito.
    - Separação clara entre palavras.

---

## Constantes

- Seguem as mesmas regras das variáveis.
- **Norma:** Sempre em caixa alta (exemplo: `PI`, `MAX_VALUE`).

---

## Entrada e Saída (Input & Output)

**Tipos de entrada/saída:**
- Consola (terminal)
- Ficheiros
- Sockets

**Função principal:**
```java
public static void main(String[] args)
```

**Saída padrão (stdout):**
```java
System.out.println("");
System.out.print("");
System.out.println("");
```

**Concatenação:**
```java
System.out.println("string1" + var + "string2");
```

**Caracteres de escape:**
- Iguais ao C/C++ (`\n`, `\t`, etc.)

**Entrada pelo terminal:**
```java
import java.util.Scanner;
Scanner input = new Scanner(System.in);
```

**Leitura de tipos:**
```java
int varNumero = input.nextInt();
double varNumero = input.nextDouble();
String varTexto = input.next(); // lê até espaço
```

---

## IntelliJ IDEA — Estrutura de Projeto

1. **Novo Projeto:**
    - Botão: *New Project*
    - Nome: `FichaPraticas`
    - Localização: diretório local (`ivany_dir`)
2. **Build System:**
    - [x] IntelliJ (iniciante, com gestão de pastas)
    - [ ] Maven
    - [ ] Gradle
3. **JDK:**
    - Download JDK 25
4. **Diretório principal:**
    - `src/`
5. **Estrutura recomendada:**
    - `src/Package/Java-Class-Ex0x`
    - Um *Package* para cada `FichaPratica0x`
    - Uma *Java class* para cada exercício
    - Um `main` para cada classe de exercício

---

<!-- **Dica:** Use comentários e organização visual para facilitar a leitura do código e dos arquivos do projeto.-->

- Resolver a Ficha técnica 1