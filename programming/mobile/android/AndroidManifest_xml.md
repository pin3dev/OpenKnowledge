## Função do `AndroidManifest.xml`
`O AndroidManifest.xml` é um arquivo obrigatório e fundamental que todo projeto Android deve ter, localizado na raiz da hierarquia do projeto `src/main/java/`.  
Ele serve como o **detentor** ou **contêiner** da aplicação inteira, resumindo o que sua aplicação terá e do que precisará.

> [!TIP]
> As principais funções do arquivo manifest são:
> * Definir Estrutura e Metadados:  
  >> Ele define a estrutura e os metadados da aplicação, seus componentes e requisitos.
> * Identificador Único:  
  >> Declara o nome do pacote (package) da aplicação, que funciona como seu identificador único. Nenhuma aplicação pode ter o mesmo pacote.
> * Declarar Componentes:
  >> É essencial para declarar todos os componentes da aplicação, como atividades (<activity>), serviços (<service>), receptores de broadcast (<receiver>) e provedores de conteúdo (<provider>). Se um componente não for declarado, o sistema não poderá iniciá-lo, e a aplicação irá falhar.
> * Gerenciar Permissões:
  >> Lista todas as permissões que a aplicação exige para acessar partes protegidas do sistema (como câmera ou internet) ou dados sensíveis do usuário (como contatos e SMS). Também pode declarar permissões que outras aplicações devem ter para acessar o conteúdo desta aplicação, ou mesmo definir permissões personalizadas.
> * Especificar Requisitos de Hardware/Software:
  >> Declara os recursos de hardware e software que a aplicação requer, o que influencia a compatibilidade com dispositivos e a disponibilidade na Google Play Store.
> * Metadados Adicionais:
  >> Inclui informações como ícone da aplicação, número de versão, tema e bibliotecas às quais a aplicação está vinculada.

Em resumo, o AndroidManifest.xml é o coração de um projeto Android, fornecendo ao sistema operacional todas as informações essenciais para executar a aplicação corretamente, gerenciar seus componentes e interagir com o usuário e o dispositivo de forma segura.


## Estrutura Hierárquica do `AndroidManifest.xml`

* `<?xml … ?>`: ponto de partida do arquivo.

  * `version`: versão do XML.
  * `encoding`: encoding usado (ex: UTF-8).

* `<manifest>`: raiz do manifest, identifica o app.

  * **Atributos**

    * `xmlns:android`: namespace Android.
    * `package`: identificador único da aplicação.
    * `android:versionCode`: número inteiro da versão (controle interno).
    * `android:versionName`: versão legível para o usuário.
    * `android:installLocation`: local de instalação (interno/SD/auto).
  * **Filhos**

    * `<uses-sdk>`

      * `android:minSdkVersion`: nível mínimo de API suportado.
      * `android:targetSdkVersion`: nível de API alvo/testado.
      * `android:maxSdkVersion`: nível máximo aceito (raro).
    * `<uses-permission>`

      * `android:name`: nome da permissão (ex: `android.permission.CAMERA`).
      * `android:maxSdkVersion`: limita permissão até certo nível de API.
    * `<permission>`

      * `android:name`: nome da permissão definida.
      * `android:protectionLevel`: nível de proteção (normal, dangerous, signature).
      * `android:label`: nome exibido.
      * `android:description`: descrição.
    * `<uses-feature>`

      * `android:name`: recurso de hardware/software (ex: NFC, bluetooth).
      * `android:required`: se é obrigatório (`true|false`).
    * `<uses-configuration>`

      * `android:reqTouchScreen`: tipo de touchscreen exigido.
      * `android:reqKeyboardType`: tipo de teclado exigido.
      * `android:reqNavigation`: tipo de navegação exigido.
    * `<supports-screens>` (legado)

      * `android:smallScreens|normalScreens|largeScreens|xlargeScreens`: quais tamanhos são suportados.
      * `android:anyDensity`: suporta múltiplas densidades.
    * `<compatible-screens>` (legado, muito raro).
    * `<queries>` (Android 11+)

      * `<package android:name="…"/>`: declara pacotes visíveis.
      * `<intent>`: descreve intents que podem ser consultadas.
      * `<provider android:authorities="…"/>`: declara providers visíveis.
    * `<application>`: container da aplicação.

      * **Atributos**

        * `android:icon`: ícone do app.
        * `android:label`: nome do app exibido.
        * `android:theme`: tema visual padrão.
        * `android:name`: classe `Application` customizada.
        * `android:allowBackup`: habilita/desabilita backup de dados.
        * `android:supportsRtl`: habilita suporte a layout RTL.
        * `android:usesCleartextTraffic`: permite tráfego HTTP não criptografado.
        * `android:networkSecurityConfig`: define políticas de rede.
        * `android:debuggable`: habilita depuração (só em builds de debug).
      * **Filhos**

        * `<activity>`: tela da aplicação.

          * **Atributos**

            * `android:name`: classe da Activity.
            * `android:exported`: se pode ser acessada fora do app (Android 12+ obrigatório).
            * `android:label`: título exibido.
            * `android:theme`: tema específico da activity.
            * `android:launchMode`: modo de lançamento (singleTop, etc).
            * `android:parentActivityName`: hierarquia de navegação.
          * **Filhos**

            * `<intent-filter>`: define intenções aceitas.

              * `<action android:name="…"/>`: ação da intent (ex: MAIN).
              * `<category android:name="…"/>`: categoria (ex: LAUNCHER).
              * `<data android:scheme="…" android:host="…" …/>`: especifica dados aceitos.
            * `<meta-data android:name="…" android:value="…"/>`: dados extras.
        * `<activity-alias>`: cria atalho/alias de uma activity.
        * `<service>`: executa operações em background.

          * `android:name`: classe do serviço.
          * `android:exported`: se é acessível externamente.
          * `android:foregroundServiceType`: tipo de serviço em foreground.
        * `<receiver>`: recebe broadcasts.

          * `android:name`: classe do receiver.
          * `android:exported`: se pode receber intents externas.
        * `<provider>`: provê dados estruturados.

          * `android:name`: classe do provider.
          * `android:authorities`: autoridade de identificação.
          * `android:exported`: controle de visibilidade.
          * **Filhos**

            * `<grant-uri-permission>`: permissões temporárias de URI.
            * `<path-permission>`: define acesso por caminho.
        * `<uses-library>`

          * `android:name`: nome da biblioteca.
          * `android:required`: se é obrigatória.
        * `<meta-data>`: chave/valor de configuração global.
        * `<profileable>`: habilita profiling via shell.


[App manifest overview](https://developer.android.com/guide/topics/manifest/manifest-intro)  
[Android Manifest XML](https://www.youtube.com/watch?v=HA126TkqvDM)  
[Android Manifest File in Android](https://www.geeksforgeeks.org/android/application-manifest-file-in-android/)  
