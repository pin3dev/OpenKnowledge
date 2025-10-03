# 🎨 Continuous Integration (CI)

### Conceito

Integração Contínua (CI – *Continuous Integration*) é uma **prática de desenvolvimento de software** cujo conceito central é integrar o código produzido por diferentes desenvolvedores de forma frequente (idealmente várias vezes ao dia) em um repositório compartilhado.

### Objetivo

Na **teoria**, a CI busca minimizar problemas de integração e garantir que o software esteja sempre em um estado funcional. O conceito se apoia em alguns pilares:

1. **Integração frequente**

   * Cada alteração realizada no código é rapidamente incorporada ao projeto principal.
   * Isso evita o chamado “*integration hell*” (quando branches permanecem separados por muito tempo, dificultando a fusão e aumentando a chance de conflitos).

2. **Automação**

   * Assim que o código é integrado, ferramentas de CI executam automaticamente:

     * compilação/build,
     * testes unitários,
     * verificações estáticas (lint, análise de segurança, etc.).
   * Em caso de falha, o desenvolvedor é notificado de imediato.

3. **Feedback rápido**

   * O objetivo é identificar erros o mais cedo possível, quando ainda são baratos e fáceis de corrigir.

4. **Repositório central e versionado**

   * O código-fonte deve estar em um sistema de controle de versão (como Git).
   * Todos os desenvolvedores trabalham no mesmo repositório, garantindo visibilidade e rastreabilidade.

5. **Entrega de um artefato confiável**

   * Ao final de cada integração, o sistema deve estar em um estado que **poderia ser entregue** (mesmo que ainda não seja colocado em produção).

👉 Em resumo, **na teoria, CI é a prática de manter o software constantemente integrado, testado e validado automaticamente**, permitindo que a equipe evolua o sistema de forma colaborativa e com menos riscos.


# 📦 Continuous Delivery (CD)

### Conceito

Entrega Contínua (CD – *Continuous Delivery*) é uma **prática de operações** cujo conceito central é garantir que o software esteja sempre em um estado **pronto para ser entregue em produção**, embora a liberação final ainda dependa de uma decisão humana.

### Objetivo

Na **teoria**, o CD busca reduzir riscos no produto final, garantir previsibilidade e manter um *pipeline* de software confiável. O conceito se baseia em alguns pilares:

1. **Geração de Artefatos**

   * Após a Integração Contínua (CI), o sistema produz artefatos testados e validados (ex.: executável, imagem Docker).
   * Esses artefatos podem ser implantados em ambientes de teste, homologação ou UAT (*User Acceptance Test*).

2. **Deploy Controlado**

   * A última etapa de deploy para produção pode ser manual ou automática, mas o momento da liberação é definido pelo time ou pelo cliente — geralmente por questões de negócio, não técnicas.

> [!WARNING]
> Na **teoria**, quando falamos de **CD**, podemos estar nos referindo a dois conceitos distintos: **Entrega Contínua (*Continuous Delivery*)** e **Implantação Contínua (*Continuous Deployment*)**.

# 🚀 Continuous Deployment (CD)

### Conceito

Implantação Contínua (CD – *Continuous Deployment*) é uma **prática de operações** cujo conceito central é automatizar o deploy do software em produção. Assim, cada mudança aprovada no pipeline é automaticamente disponibilizada ao público, sem necessidade de intervenção manual.

### Objetivo

Na **teoria**, o CD busca automatizar por completo a entrega em produção como estágio final do *pipeline*, acelerando ao máximo a disponibilização de valor. Essa prática permite lançamentos frequentes e pequenos, reduzindo os riscos associados a grandes *releases*. O conceito se apoia em um pilar principal:

1. **Automação da Entrega de Artefatos**

   * Não há "pausa" entre a validação do código e o deploy.
   * Tudo que passa nos testes automatizados é implantado diretamente em produção.

> [TIP]
> Diferenças:
>
> * **Continuous Delivery** → sempre pronto para ir à produção, mas exige **gatilho manual** para o deploy final.
> * **Continuous Deployment** → vai direto para produção de forma **automática**, sem intervenção humana.

