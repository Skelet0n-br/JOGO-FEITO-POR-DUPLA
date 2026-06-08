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

Inteligência Artificial (IA) dos Inimigos
Os monstros comportam-se de 3 formas distintas através da função move_monsters():

X (Monstro Tipo 1): Movimentação completamente aleatória pelo mapa.

Y (Monstro Tipo 2): Persegue ativamente o jogador calculando a menor distância absoluta nos eixos X e Y.

Z (Boss Final): Realiza uma patrulha horizontal na arena e, a cada 5 turnos, teletransporta-se para uma posição aleatória adjacente ao jogador.

⚔️ Sistema de Combate (Armas)Ao interagir com o NPC (N) na Vila, o jogador pode alterar sua classe de arma, modificando a área de dano calculada pela função in_attack_area():Espada (WEAPON_SWORD): Causa dano em área de $3 \times 2$ à frente do jogador (foco em combate corpo a corpo).Arco e Flecha (WEAPON_BOW): Dispara em linha reta atingindo até 4 células de distância na direção em que o jogador está olhando.Cajado (WEAPON_STAFF): Cria uma onda mística que atinge simultaneamente todas as 8 células adjacentes ao redor do jogador.

🕹️ Controles
W, A, S, D - Movimentar personagem / Mudar direção do olhar.

🗺️ Sumário e Estrutura das Funções
Para facilitar a navegação no código-fonte, a arquitetura do arquivo está dividida da seguinte forma:

🧩 Inicialização e Dados
Includes e defines: Importação de bibliotecas padrão e definição de constantes globais (fases, ID de armas, tamanhos máximos de matriz).

Estruturas de dados:

Entity: Estrutura para controle de monstros e do NPC (posição, vida, comportamento).

Player: Estrutura do herói (coordenadas, direção do olhar, vidas, arma ativa, chaves).

Map: Estrutura do cenário (grid de células, vetor de entidades, estados dos botões).

📐 Construção dos Mapas
build_village(): Inicializa a vila pacífica onde se localiza o NPC de armas.

build_floor1(): Cria o primeiro andar da masmorra (foco em chaves e portas).

build_floor2(): Cria o segundo andar introduzindo o primeiro monstro e botões.

build_floor3(): Gera a arena final com o Boss e armadilhas complexas.

🔄 Loop Principal de Jogo
draw(): Renderiza o mapa atualizado e o HUD de status no terminal.

handle_input(): Captura a tecla pressionada e atualiza a posição ou ação do jogador.

do_attack(): Executa e calcula a área de dano do ataque dependendo da arma equipada.

do_interact(): Gerencia interações físicas (coleta de chaves, abertura de portas, diálogos com NPC, ativação de botões e descida de escadas).

move_monsters(): Aciona o motor de Inteligência Artificial para movimentar os monstros.

check_monster_contact(): Verifica se o jogador colidiu com algum perigo ou pisou em espinhos, aplicando dano.

🖥️ Telas Especiais (Menus)
show_menu(): Menu principal de seleção.

show_tutorial(): Exibe a tabela descritiva de símbolos e comandos do jogo.

show_credits(): Exibe as informações de desenvolvimento.

show_gameover(): Tela exibida quando o jogador esgota suas vidas.

show_win(): Tela de encerramento celebrando a derrota do Boss.

🛠️ Detalhes de Implementação
1. Compatibilidade Multiplataforma
O jogo gerencia a captura de teclas em tempo real (sem precisar apertar "Enter") de forma nativa usando diretivas de pré-processador (#ifdef _WIN32):

Windows: Utiliza a biblioteca <conio.h> (_getch()) e o comando cls.

Linux / macOS: Modifica os atributos do terminal em tempo de execução (termios.h e unistd.h) para desativar o modo canônico e o ecoamento de caracteres, limpando a tela com o comando clear.

2. Elementos do Mapa e Símbolos ASCIIÍconeSignificadoComportamento^ v < >JogadorIndica a direção para onde o personagem se moveu ou atacou.*ParedeObstáculo intransponível.#EspinhoArmadilha. Pisar aqui custa uma vida instantaneamente.kCaixaObstáculo destrutível utilizando o comando de ataque.OBotãoGatilho físico para abrir passagens ocultas ou invocar inimigos.DPorta FechadaBloqueia o caminho. Consome 1 Chave do inventário para abrir.@ChaveItem essencial coletável espalhado pelas salas.LEscadaTransiciona o jogador de forma segura para o próximo andar.


I - Interagir (Conversar com NPC, coletar chaves, abrir portas, ativar botões).

O - Atacar (Aplica dano baseado na arma atual).

Q - Sair para o Menu Principal.
