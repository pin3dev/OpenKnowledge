https://www.youtube.com/watch?v=QQDGb4AkY4c&list=PLx4x_zx8csUhzAyii9-cY-IJwo00p_5AC&index=1

# Começar um Novo projeto
> `(New Project)` > Project Type: `Application` >  Project Subtype: `Qt Widgets Application` > seleciona dir para armazenar projeto > nomeia o projeto > kit selection: selecone a IDE que vc usa > class information: deixa noome padrao `main` > project management: visualizacao de resumo do projeto > `(finish)` 

> [!TIP]

# Estrutura do Creator
- uma arquivo `mainwindow.cpp` é criado automaticamente, usando dos headers `ui_mainwindow.h` (padrão) e `mainwindow.h` que tbm é criado automaticamente assim como um arquivo `main.cpp` que roda o programa final, e que por sua vez importa as configurações do `mainWindow.h` e uma biblioteca chamada <QApplication>

> [!TIP]
> A biblioteca <QApplication> é responsavel por disponibilizar métodos de execução do programa com `QApplication` object e metodos como `exec()` assim como object  `MainWindow` com metodos tipo o `.show()` que  mostra a janela/formulario do projeto, entre outros metodos tipo `.bip()` que soa o bip na saida de audio do dispositivo...

- - Project_dir/
    - Project_name.pro
    - Headers/
      - mainwindow.h (
    - Source/
      - main.cpp (codigo principal que instancia o programa rodando ele)
      - mainwindow.cpp (definição de metodos para classe MainWindow, no caso, do formulario/janela) e sempre é criado um novo formulario/janela para o programa um novo .cpp e .ui será automaticamente criado
    - Form/
      - mainwindow.ui (codigo com padrao xml que define os itens graficos)
     
# Visual
* todos os elementos visuais são inseridos na janela grafica apenas com o clica e arrasta
* as propriedades dos elementos em projeto podem ser alteradas na secção de `Properity` no lado inferior direito
* há uma secção de objetos presentes no projeto organizado em estrutura de arvore (já vem organizado dentro do MainWindow um menuBar, mainToolBar e um statusBar

### Lista de propriedades comuns aos objetos
- Name: nome do objeto no projeto
- Text: texto que aparece visualmente no projeto onde está o elemento
- ** Slots: são eventos que podem ser disparados ao acionar o elemento visual (para acessa-los e configura-los temos que clicar com o botao direito em cima do elemento e depois `go to slot`

> [!TIP]
> 1. ao inserir um slot de click a um botão um método `on_pushButton_clicked()` será adicionado a classe do elemento MainWindow  (MainWindow.h) chamado na configuração `private slot:` e pode ser configurada por codigo no MainWindow.cpp
> 2. tudo que precisamos relacionar com o formulario/janela (item visual) seja ele o MainWindow ou outro elemento/formualario, será referenciado pela classe `ui`

# Execução
* Para ver o programa em execução basta clicar na `seta verde` e aguardar a compilação/construção do programa e a execução do mesmo

## Slots vs Signals
- sinais são eventos que programamos, exemplo: eventos de clique, mouse pressionado...
- slots são ações/respostas que acontecem quando os eventos são acionados...
- 
