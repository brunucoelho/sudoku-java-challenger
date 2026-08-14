# Sudoku Java Challenger

Projeto desenvolvido para a conclusão do desafio proposto pelo bootcamp **Java** da **CI&T**, que consiste na implementação de um jogo de Sudoku em Java. O projeto conta com **duas formas de jogar**: via linha de comando (CLI) e via interface gráfica (Swing).

## 📋 Sobre o projeto

O jogo permite ao usuário:

- Iniciar uma partida a partir de um tabuleiro pré-definido, com números fixos (não editáveis) e espaços em branco a preencher;
- Inserir e remover números nas posições livres;
- Visualizar o tabuleiro atual, formatado em uma grade 9x9 dividida em setores 3x3;
- Verificar o status do jogo (não iniciado, incompleto ou completo);
- Verificar se o tabuleiro atual possui erros (números que não conferem com o gabarito);
- Limpar o tabuleiro, mantendo os números fixos;
- Finalizar o jogo quando todas as posições estiverem preenchidas corretamente.

## 🚀 Tecnologias utilizadas

- **Java 17**
- **Swing** (interface gráfica)
- **Maven** (gerenciamento de build e dependências)

## 📁 Estrutura do projeto

```
src/main/java/com/suduku
├── Main.java                          # Ponto de entrada da versão CLI (terminal)
├── UIMain.java                        # Ponto de entrada da versão gráfica (Swing)
├── model
│   ├── Board.java                     # Regras de negócio do tabuleiro
│   ├── Space.java                     # Representa uma célula do tabuleiro (valor esperado, atual e se é fixa)
│   └── GameStatusEnum.java            # Status possíveis do jogo (NON_STARTED, INCOMPLETE, COMPLETE)
├── service
│   ├── BoardService.java              # Camada de serviço que monta o Board a partir da configuração recebida
│   ├── NotifierService.java           # Barramento simples de eventos (publish/subscribe)
│   ├── EventEnum.java                 # Tipos de evento suportados (ex.: CLEAR_SPACE)
│   └── EventListener.java             # Contrato para componentes que reagem a eventos
├── ui
│   └── custom
│       ├── frame
│       │   └── MainFrame.java         # JFrame principal da aplicação
│       ├── panel
│       │   ├── MainPanel.java         # Painel principal que organiza os setores do tabuleiro
│       │   └── SudokuSector.java      # Painel de um setor 3x3 do Sudoku
│       ├── input
│       │   ├── NumberText.java        # Campo de texto vinculado a um Space do tabuleiro
│       │   └── NumberTextLimit.java   # Document que restringe o campo a um único dígito de 1 a 9
│       ├── button
│       │   ├── ResetButton.java       # Botão "Reiniciar jogo"
│       │   ├── CheckGameStatusButton.java  # Botão "Verificar jogo"
│       │   └── FinishGameButton.java  # Botão "Concluir"
│       └── screen
│           └── MainScreen.java        # Monta a tela principal, distribuindo os Spaces em 9 setores 3x3
└── util
    └── BoardTemplate.java             # Template de texto usado para desenhar o tabuleiro no console (versão CLI)
```

### Modelo de domínio

- **`Space`**: representa uma célula do Sudoku. Guarda o valor esperado (`expected`), o valor atual informado pelo jogador (`actual`) e se a célula é fixa (`fixed`, ou seja, faz parte do desafio original e não pode ser alterada).
- **`Board`**: representa o tabuleiro completo (matriz de `Space`) e concentra as regras do jogo:
  - `getStatus()`: calcula se o jogo está `NON_STARTED`, `INCOMPLETE` ou `COMPLETE`;
  - `hasErrors()`: verifica se algum valor preenchido diverge do valor esperado;
  - `changeValue(col, row, value)`: altera o valor de uma célula não fixa;
  - `clearValue(col, row)`: limpa o valor de uma célula não fixa;
  - `reset()`: limpa todas as células não fixas do tabuleiro;
  - `gameIsFinished()`: indica se o jogo foi concluído sem erros.
- **`BoardService`**: monta a matriz de `Space` a partir do mapa de configuração recebido via argumentos e expõe as operações do `Board` para as camadas de interface (CLI e UI).
- **`NotifierService`**: implementação simples de publish/subscribe usada pela UI — por exemplo, para notificar todos os campos (`NumberText`) quando o jogo é reiniciado (`CLEAR_SPACE`), limpando o texto exibido.

### Interface gráfica (Swing)

A `MainScreen` percorre o tabuleiro em blocos de 3x3, monta um `SudokuSector` para cada bloco (cada um com 9 `NumberText`, um por `Space`) e os organiza dentro do `MainPanel`, exibido no `MainFrame`. Os botões `ResetButton`, `CheckGameStatusButton` e `FinishGameButton` disparam as ações de reiniciar, verificar status e concluir o jogo, respectivamente.

## ▶️ Como executar

Pré-requisitos: **JDK 17** e **Maven** instalados.

Compile o projeto antes de rodar qualquer uma das versões:

```bash
mvn compile
```

### Configuração do tabuleiro

Ambas as versões (CLI e UI) recebem o tabuleiro inicial via argumentos de linha de comando. Cada argumento representa uma célula no formato:

```
linha,coluna;valorEsperado,fixo
```

Exemplo: `0,0;4,true` define que a célula da linha 0, coluna 0 tem valor esperado `4` e é fixa (não editável). É necessário informar as 81 células (9x9) para que o tabuleiro seja montado corretamente.

### Versão gráfica (Swing)

```bash
mvn compile exec:java -Dexec.mainClass="com.suduku.UIMain" -Dexec.args="0,0;4,true 0,1;9,false ..."
```

### Versão terminal (CLI)

```bash
mvn compile exec:java -Dexec.mainClass="com.suduku.Main" -Dexec.args="0,0;4,true 0,1;9,false ..."
```

Ou, gerando o `.class` manualmente:

```bash
mvn compile
java -cp target/classes com.suduku.UIMain 0,0;4,true 0,1;9,false ...
java -cp target/classes com.suduku.Main 0,0;4,true 0,1;9,false ...
```

Na versão CLI, o menu interativo (via `Scanner`) permite iniciar o jogo, inserir/remover números, visualizar o tabuleiro, verificar status, limpar e finalizar a partida.

## 🎓 Contexto

Este repositório faz parte do desafio de projeto do bootcamp **Java** da **CI&T**.