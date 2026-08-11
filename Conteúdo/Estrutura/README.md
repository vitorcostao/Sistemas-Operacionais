# Sistemas Operacionais - Estrutura

## Sistema de Computação Moderno

Um sistema de computação moderno é composto por uma CPU conectada, por meio de um barramento do sistema, a diversos dispositivos de I/O (disco, impressora, unidades de fita, etc.), cada um gerenciado por sua própria controladora e módulo de memória.

## Operação dos Sistemas de Computação

- A CPU e os dispositivos de I/O podem operar de forma concorrente.
- Cada controladora de dispositivo é responsável por um tipo específico de dispositivo e possui um buffer local próprio.
- A CPU transfere dados entre os buffers locais das controladoras e a memória principal.
- A transferência de dados (I/O) ocorre entre o dispositivo e seu buffer local.
- Ao concluir uma operação, a controladora avisa a CPU por meio de uma interrupção.

## Interrupções

- Os sistemas operacionais modernos são baseados em interrupções.
- Uma interrupção transfere o controle da execução para uma rotina de serviço específica, localizada por meio do vetor de interrupções, que contém os endereços de todas as rotinas de serviço.
- Segmentos de código separados definem a ação a ser tomada para cada tipo de interrupção.
- O sistema operacional preserva o estado da CPU salvando os valores dos registradores e o contador de programa.
- Enquanto uma interrupção está sendo processada, outras interrupções permanecem desabilitadas.
- Uma exceção (ou trap) é um tipo de interrupção gerada por software, sinalizando um erro ou uma requisição do usuário.

## Tabela de Status de Dispositivos

O sistema operacional mantém uma tabela com o status de cada dispositivo conectado (por exemplo: leitora de cartões, impressoras, unidades de disco), indicando se estão ociosos (idle) ou ocupados (busy), além de informações sobre requisições pendentes, como endereço, tamanho, arquivo e tipo de operação (leitura ou escrita).

## Acesso Direto à Memória (DMA)

- Utilizado para permitir que dispositivos de I/O de alta velocidade transfiram dados em velocidade comparável à da memória.
- A controladora do dispositivo transfere blocos de dados do buffer diretamente para a memória principal, sem intervenção da CPU.
- Gera-se apenas uma interrupção por bloco transferido, em vez de uma interrupção por byte, reduzindo significativamente a sobrecarga sobre a CPU.

## Estrutura de Armazenamento

- **Memória Principal**: única grande área de memória acessada diretamente pela CPU.
- **Memória Secundária**: extensão da memória principal, não volátil (mantém os dados mesmo sem energia).
- Os sistemas de armazenamento são organizados hierarquicamente, considerando critérios como velocidade, custo e volatilidade.
- **Caching**: técnica que consiste em copiar informações entre diferentes níveis de memória, buscando maior desempenho.

## Proteção de Hardware: Operação em Modo Dual

- O compartilhamento de recursos exige que o sistema operacional garanta que um programa não interfira na execução dos demais.
- Um bit adicional no hardware permite diferenciar dois modos de operação:
  1. **Modo Usuário** – execução em favor do programa do usuário.
  2. **Modo Monitor** (também chamado Modo Supervisor ou Modo Sistema) – execução em favor do sistema operacional.
- O bit de modo indica o modo corrente: monitor (0) ou usuário (1).
- Quando ocorre uma interrupção ou exceção, o hardware muda automaticamente para o modo monitor.
- Instruções privilegiadas só podem ser executadas em modo monitor.

## Proteção de I/O

- Todas as instruções de I/O são consideradas instruções privilegiadas.
- É necessário garantir que um programa de usuário nunca obtenha controle do computador em modo monitor.
- Toda operação de I/O deve ser solicitada ao sistema operacional.

## Proteção de Memória

- A proteção de memória deve garantir a integridade do vetor de interrupções e de suas rotinas associadas.
- É implementada com o auxílio de dois registradores que determinam a faixa legal de memória que pode ser acessada por um programa de usuário (espaço de endereçamento):
  - **Registrador base**: define o menor endereço físico legal.
  - **Registrador de limite**: define o tamanho da faixa legal de endereços.
- Qualquer tentativa de acesso à memória fora desse espaço de endereçamento é proibida.

## Hardware de Proteção

- A CPU verifica, a cada acesso à memória, se o endereço solicitado está dentro do intervalo definido por `base` e `base + limite`.
- Se o endereço estiver fora dessa faixa, é gerado um trap (erro de endereçamento) para o sistema operacional.
- Quando o sistema está executando em modo monitor, o sistema operacional tem acesso irrestrito à memória.
- As instruções de carga (load) dos registradores de base e limite são instruções privilegiadas.

## Proteção de CPU

- O **Timer** (temporizador) interrompe o processador após um período de tempo específico, garantindo que o sistema operacional mantenha o controle da execução.
- O timer é decrementado a cada pulso de clock; quando atinge o valor zero, gera uma interrupção.
- O timer também é utilizado para implementar o compartilhamento de tempo de CPU entre processos.
- Carregar um valor no timer é uma instrução privilegiada.

## Arquitetura Geral do Sistema

- Como as instruções de I/O são privilegiadas, um programa de usuário não pode executar I/O diretamente.
- **Chamadas ao Sistema (System Calls)**: método utilizado por um processo para requisitar uma ação específica ao sistema operacional.
  - Geralmente são implementadas como uma exceção que aponta para uma posição específica do vetor de interrupções.
  - O controle é transferido para uma rotina de serviço do sistema operacional, e o modo de operação passa para modo monitor.
  - O sistema operacional verifica se os parâmetros da chamada estão corretos, executa a requisição solicitada e, em seguida, retorna o controle para a instrução seguinte à chamada ao sistema.