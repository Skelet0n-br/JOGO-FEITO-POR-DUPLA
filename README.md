# ⚔️ Crônicas da Masmorra (Dungeon Crawler)

![C](https://img.shields.io/badge/Language-C-blue.svg)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey.svg)

Um jogo clássico de **Dungeon Crawler** baseado em texto (ASCII Art) desenvolvido inteiramente na linguagem **C**. O jogo roda diretamente no terminal, possui múltiplos andares, IA de monstros diferenciada, sistema de armas intercambiáveis e suporte nativo multiplataforma.

---

## 🚀 Como Compilar e Jogar

Para compilar e rodar o projeto, você precisará de um compilador C (como o `gcc`). Abra o terminal na pasta do arquivo e execute os comandos abaixo:

```bash
# Compilar o código
gcc -o game game.c

# Executar o jogo
./game
🛠️ Arquitetura do Projeto
O código foi projetado de forma modular, dividindo as responsabilidades em Estruturas de Dados, Controle de Fluxo de Estados e um Game Loop baseado em turnos.

1. Compatibilidade Multiplataforma
O jogo gerencia a captura de teclas em tempo real (sem precisar apertar "Enter") e a limpeza de tela de forma nativa usando diretivas de pré-processador (#ifdef _WIN32):

Windows: Utiliza a biblioteca <conio.h> (_getch()) e o comando cls.

Linux / macOS: Modifica temporariamente os atributos do terminal (termios.h e unistd.h) para desativar o modo canônico e o ecoamento de teclas, utilizando o comando clear.

2. Estruturas de Dados Principais (structs)Entity: Define qualquer elemento dinâmico no mapa (Monstros e NPCs). Guarda a posição $(x, y)$, estado de vida, o caractere representativo, pontos de vida (HP) e variáveis de patrulha.Player: Controla as propriedades do herói: coordenadas, direção para onde está olhando (^, v, <, >), vidas restantes, arma equipada e chaves no inventário.Map: Representa o tabuleiro. Armazena a matriz de caracteres do cenário atual, uma cópia da matriz original (orig) para reinicialização e a lista de entidades ativas.

🎮 Mecânicas do JogoO ambiente reage dinamicamente a cada ação do jogador (movimentação ou ataque).Elementos do Mapa e Símbolos ASCIIÍconeSignificadoComportamento^ v < >JogadorO caractere muda indicando a direção do último movimento/olhar.*ParedeBloqueia a movimentação do jogador e dos monstros.#EspinhoArmadilha fatal. Pisar aqui faz o jogador perder uma vida imediatamente.kCaixaObstáculo destrutível utilizando a ação de ataque (o).OBotãoGatilho que altera o mapa (abre passagens ou invoca inimigos).DPorta FechadaBloqueia o avanço. Consome 1 Chave ao interagir para abrir.@ChaveItem coletável necessário para abrir portas.LEscadaPonto de transição que avança o jogador para a próxima fase.

nteligência Artificial (IA) dos Inimigos
Os monstros comportam-se de 3 formas distintas através da função move_monsters():

X (Monstro Tipo 1): Movimentação completamente aleatória pelo mapa.

Y (Monstro Tipo 2): Persegue ativamente o jogador calculando a menor distância absoluta nos eixos X e Y.

Z (Boss Final): Realiza uma patrulha horizontal na arena e, a cada 5 turnos, teletransporta-se para uma posição aleatória adjacente ao jogador.

⚔️ Sistema de Combate (Armas)Ao interagir com o NPC (N) na Vila, o jogador pode alterar sua classe de arma, modificando a área de dano calculada pela função in_attack_area():Espada (WEAPON_SWORD): Causa dano em área de $3 \times 2$ à frente do jogador (foco em combate corpo a corpo).Arco e Flecha (WEAPON_BOW): Dispara em linha reta atingindo até 4 células de distância na direção em que o jogador está olhando.Cajado (WEAPON_STAFF): Cria uma onda mística que atinge simultaneamente todas as 8 células adjacentes ao redor do jogador.

🕹️ Controles
W, A, S, D - Movimentar personagem / Mudar direção do olhar.

I - Interagir (Conversar com NPC, coletar chaves, abrir portas, ativar botões).

O - Atacar (Aplica dano baseado na arma atual).

Q - Sair para o Menu Principal.
