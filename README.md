# 💣 Campo Minado Console - Java OOP & Streams
Uma implementação robusta e orientada a objetos do clássico jogo **Campo Minado (Minesweeper)** desenvolvida em Java para execução direta no terminal (Console). O projeto destaca-se pela aplicação de algoritmos de varredura recursiva de vizinhança e uso avançado de recursos modernos do Java, como processamento paralelo e expressões lambda.

---

## 📌 Sumário
- [Funcionalidades e Diferenciais](#-funcionalidades-e-diferenciais)
- [Algoritmos e Conceitos Aplicados](#-algoritmos-e-conceitos-aplicados)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Como Executar o Jogo](#-como-executar-o-jogo)
- [Instruções de Gameplay](#-instruções-de-gameplay)

---

## 🚀 Funcionalidades e Diferenciais

* **Abertura em Cascata:** Sempre que um campo seguro e sem minas ao redor é aberto, o sistema propaga a abertura automaticamente para todos os vizinhos elegíveis de forma recursiva.
* **Alta Performance com Parallel Streams:** As buscas de filtragem e modificação de status no tabuleiro utilizam `parallelStream()`, distribuindo a carga de varredura por múltiplas threads de hardware da CPU.
* **Mecanismo de Reinicialização Continuada:** Fluxo de jogo contínuo que interroga o usuário ao final de cada partida se deseja iniciar um novo ciclo sem precisar encerrar a aplicação.
* **Tratamento Elegante de Fluxo:** Utilização de exceções de runtime customizadas (`ExplosaoException` e `SairException`) para desacoplar as interações do console da lógica pura do modelo de domínio.

---

## 🧠 Algoritmos e Conceitos Aplicados

### 1. Cálculo de Distância Euclidiana e Vizinhança
A classe `Campo` avalia de forma autônoma se outro campo é seu vizinho (adjacente direto ou diagonal) calculando o deslocamento absoluto cartesiano através da fórmula:

```java
int deltaLinha = Math.abs(linha - vizinho.linha);
int deltaColuna = Math.abs(coluna - vizinho.coluna);
int deltaGeral = deltaLinha + deltaColuna;
```
Se deltaGeral == 1 (vizinho ortogonal) ou deltaGeral == 2 com diagonal ativa, a associação é estabelecida dinamicamente na inicialização.

### 2. Paradigma Funcional e Streams
Filtros complexos de encerramento de partida e sorteio de bombas são validados usando expressões lambda legíveis combinadas com predicados nativos:
```java
Predicate<Campo> minado = c -> c.isMinado();
minasArmadas = campos.stream().filter(minado).count();
```

---

## 🗂️ Estrutura do Projeto
O código-fonte está modularizado de acordo com suas responsabilidades arquiteturais:
```text
📂 src/
└── 📂 projeto/
    ├── 📄 Aplicacao.java              # Ponto de entrada (Main) e instanciação
    ├── 📂 excecao/
    │   ├── 📄 ExplosaoException.java  # Disparada ao abrir uma mina
    │   └── 📄 SairException.java      # Disparada caso o usuário aborte a sessão
    ├── 📂 modelo/
    │   ├── 📄 Campo.java              # Lógica atômica de cada célula e vizinhos
    │   └── 📄 Tabuleiro.java          # Matriz de jogo, sorteios e manipulação em lote
    └── 📂 visao/
        └── 📄 TabuleiroConsole.java    # Captura de inputs (Scanner) e renderização do HUD
```

---

## 💻 Como Executar o Jogo
Pré-requisitos:
- Java Development Kit (JDK) 11 ou superior instalado.

Passo a Passo pelo Terminal:
1. Clone o repositório:
```bash
git clone https://github.com/VitorBarateli/Campo-Minado.git
cd nome-do-repositorio
```
2. Compile o projeto:
Crie um diretório de saída e compile todos os arquivos .java inclusos nos pacotes:
```bash
javac -d bin src/projeto/*.java src/projeto/excecao/*.java src/projeto/modelo/*.java src/projeto/visao/*.java
```
3. Inicie o jogo:
Execute a classe Aplicacao que inicializa o tabuleiro padrão (configurado para dimensões de 6x6 e com 3 minas terrestres ocultas):
```bash
java -cp bin projeto.Aplicacao
```

---

## 🎮 Instruções de Gameplay
Ao iniciar, uma grade textual será desenhada no terminal (onde ? representa campos fechados e x representa marcações de segurança).
1. Coordenadas: O jogo solicitará a coordenada desejada no formato x, y (onde X é a Linha e Y é a Coluna). Exemplo: 2, 4.
2. Ações: Na sequência, informe a numeração do comando que deseja efetuar:
    - Digite 1 para Abrir a célula escolhida.
    - Digite 2 para (Des)Marcar a célula com uma bandeira (sinalizando que ali há uma mina).
3. Condição de Vitória: O jogo é ganho com sucesso quando todos os campos seguros forem abertos e todas as minas forem corretamente marcadas ou isoladas.
4. Sair: A qualquer momento, você pode digitar sair no prompt de coordenadas para encerrar a partida prematuramente.
