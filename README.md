**So_Long (42 Project)**

- **Descrição:**: Projeto 42 que implementa um pequeno jogo 2D chamado `so_long`. O objetivo é criar um jogo onde o jogador coleta todos os itens do mapa e alcança a saída, com validação de mapas, movimentação, e renderização usando MiniLibX.

**Visão Geral**
- **Objetivo do jogo**: Navegar por um mapa em grade, coletar todos os objetos (`C`) e chegar à saída (`E`) sem atravessar paredes (`1`).
- **Formato do mapa**: Arquivos `.ber` na pasta `maps/`. Cada caractere tem significado:
	- `1`: parede (wall)
	- `0`: piso (floor)
	- `P`: posição inicial do jogador (apenas 1 permitida)
	- `C`: coletável (deve haver pelo menos 1)
	- `E`: saída (apenas 1 permitida)

**Como o projeto foi realizado**
- **Leitura e validação do mapa**: O programa lê o arquivo `.ber` (linha a linha) e monta uma matriz de caracteres em memória. Em seguida valida:
	- se o mapa é retangular;
	- se o mapa é cercado por paredes (paredes na borda externa);
	- se contém exatamente 1 jogador (`P`), exatamente 1 saída (`E`) e pelo menos 1 coletável (`C`);
	- se existe um caminho válido do jogador para todos os coletáveis e para a saída.

- **Validação de caminho**: Implementada com uma cópia do mapa e um algoritmo de flood-fill (recursivo) partindo da posição do jogador. Se após o preenchimento ainda restarem `C` ou `E` inatingíveis, a validação falha.

- **Renderização e controles**: A renderização é feita com a biblioteca MiniLibX. Cada tile tem tamanho definido (`TILE_SIZE = 32`). O jogo carrega imagens/texturas (paredes, chão, jogador, coletáveis e saída) e desenha o mapa na janela.
	- **Controles**: `W`, `A`, `S`, `D` para mover; `ESC` para sair.

**Decisões de implementação**
- **Estruturas**: Um `struct t_game` centraliza estado do jogo (mapa, dimensões, posição do jogador, quantidade de coletáveis, movimentos, ponteiros da MLX e imagens).
- **Modularidade**: Código dividido em módulos: leitura do mapa, validação, validação de caminhos, lógica do jogo (movimentação), renderização (gráficos) e limpeza de memória.
- **Bibliotecas**: Uso de `libft` (implementação pessoal das funções utilitárias da 42) e `MiniLibX` para gráficos e eventos.

**Estrutura do repositório**
- **`includes/so_long.h`**: header com definições, structs e protótipos.
- **`src/`**: código-fonte dividido em arquivos (ex.: `map_reader.c`, `map_validation.c`, `path_validation.c`, `graphics.c`, `hooks.c`, `game_logic.c`, `cleanup.c`, `main.c`).
- **`libft/`**: biblioteca utilitária local (`libft.a`).
- **`maps/`**: mapas de exemplo (válidos e inválidos) para teste.
- **`textures/`**: imagens usadas no jogo.
- **`Makefile`**: instruções de build e integração com `libft` e MiniLibX.

**Como compilar e executar**
- **Pré-requisitos**: Em sistemas Linux, instale as dependências do X11 e MiniLibX apropriadas. Este Makefile espera que o `mlx.h` esteja em `/usr/local/include` e as libs em `/usr/local/lib`.
- **Compilar**:

```
make
```

- **Executar** (exemplo usando um mapa de `maps/`):

```
./so_long maps/valid.ber
```

- **Limpar**:

```
make clean
make fclean
```

**Arquitetura e pontos técnicos relevantes**
- **Validação de mapa**: `map_validation.c` garante formato e componentes corretos.
- **Validação de caminho**: `path_validation.c` usa flood-fill recursivo sobre uma cópia do mapa (função `flood_fill`) para garantir alcançabilidade de todos os `C` e da `E` a partir da posição de `P`.
- **Eventos e renderização**: `graphics.c` e `hooks.c` configuram a janela, carregam imagens em `t_img` e registram callbacks para teclado e fechamento da janela.
- **Gerenciamento de memória**: todo o mapa é armazenado dinamicamente (vetor de strings); há funções para liberar (`free_map`) e tratar erros de forma segura (`error_exit`).

**Competências desenvolvidas**
- **Linguagem C avançada**: manipulação de ponteiros, alocação dinâmica, strings e arrays dinâmicos.
- **Estruturas de dados**: representação de mapas como matrizes 2D e cópia/duplicaçãode mapas para algoritmos.
- **Algoritmos**: flood-fill (recursão) para checagem de caminhos/alcance.
- **Programação modular**: separação clara entre leitura, validação, lógica e renderização.
- **Integração com bibliotecas**: compilação e link com `libft` e MiniLibX.
- **Depuração e tratamento de erros**: validações robustas e mensagens de erro úteis.
- **Boas práticas**: gerenciamento de memória e freeing em caminhos de erro.

**Testes e mapas de exemplo**
- Use os mapas na pasta `maps/` para testar casos:
	- `maps/valid.ber`, `maps/valid2.ber`, etc. (casos válidos);
	- `maps/invalid_*.ber` (casos que demonstram mensagens de erro e validações distintas).


**Exemplos de comando**
- Compilar e executar um mapa válido:

```
make
./so_long maps/valid.ber
```

- Executar um mapa inválido para ver as mensagens de validação:

```
./so_long maps/invalid_walls.ber
```
