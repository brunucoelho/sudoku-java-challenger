# Sudoku Java Challenger

Projeto desenvolvido para a conclusão do desafio proposto pelo bootcamp **Java** da **CI&T**, que consiste na implementação de um jogo de Sudoku executado via linha de comando (CLI).

> ⚠️ **Projeto em desenvolvimento.** As regras de domínio (modelo do tabuleiro) já estão implementadas, mas a interação via terminal (menu, leitura de comandos, exibição do tabuleiro) ainda está em construção.

## 📋 Sobre o projeto

O objetivo é implementar um Sudoku jogável pelo terminal, permitindo ao usuário:

- Iniciar um novo jogo a partir de um tabuleiro pré-definido, com números fixos (não editáveis) e espaços em branco a preencher;
- Inserir e remover números nas posições livres;
- Visualizar o tabuleiro atual formatado em uma grade 9x9;
- Verificar o status do jogo (não iniciado, incompleto ou completo);
- Verificar se o tabuleiro atual possui erros (números que não conferem com o gabarito);
- Limpar o tabuleiro, mantendo os números fixos;
- Finalizar o jogo quando todas as posições estiverem preenchidas corretamente.

## 🚀 Tecnologias utilizadas

- **Java 17**
- **Maven** (gerenciamento de build e dependências)

## 📁 Estrutura do projeto

```
src/main/java/com/suduku
├── Main.java                    # Ponto de entrada da aplicação (CLI)
├── model
│   ├── Board.java                # Regras de negócio do tabuleiro
│   ├── Space.java                 # Representa uma célula do tabuleiro (valor esperado, atual e se é fixa)
│   └── GameStatusEnum.java        # Status possíveis do jogo (NON_STARTED, INCOMPLETE, COMPLETE)
└── util
    └── BoardTemplate.java         # Template de texto usado para desenhar o tabuleiro no console
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
- **`BoardTemplate`**: template textual (`String.format`-ready) usado para renderizar o tabuleiro 9x9 no console, dividido em 9 blocos 3x3.

## ▶️ Como executar

Pré-requisitos: **JDK 17** e **Maven** instalados.

```bash
mvn compile exec:java -Dexec.mainClass="com.suduku.Main"
```

Ou, gerando o `.class` manualmente:

```bash
mvn compile
java -cp target/classes com.suduku.Main
```

> No estado atual, a aplicação apenas imprime `Hello world!` — a leitura do tabuleiro inicial e o loop de interação via `Scanner` ainda serão implementados em `Main.java`.

## 🛠️ Próximos passos

- [ ] Carregar um tabuleiro inicial (posições fixas x posições livres) a partir de argumentos ou arquivo;
- [ ] Implementar o menu interativo no `Main` (novo jogo, colocar número, remover número, visualizar tabuleiro, verificar status, limpar, finalizar, sair);
- [ ] Renderizar o tabuleiro no console usando `BoardTemplate`;
- [ ] Validar entradas do usuário (posição e valor).

## 🎓 Contexto

Este repositório faz parte do desafio de projeto do bootcamp **Java** da **CI&T**.
