# Sistemas Operacionais - Introdução

O **Sistema Operacional (SO)** é um programa que age como intermediário entre o usuário e o hardware. Desse modo,
este programa é responsável por alocar e gerenciar os recursos da máquina de modo que, dependendo das tarefas que
estão sendo executadas e do tipo dessas tarefas, alguns deles serão direcionados para componentes específicos da máquina.

Um **SO** garante a manutenção de diversos recurso, entre eles, os principais são: CPU, memória, dispositivos de I/O.
Nesse contexto, existe o programa de controle que é responsável pela manutenção das operações e programas de entrada e saída da máquina.

Além disso, existem programas que são executados de forma initerrupta, esses programas são chamados de Kernel e 
representam a parte central e mais importante do SO.

---

## Componentes de um Sistema Computacional

- **Hardware**: Provê recursos básico de computação (CPU, memória, I/O).
- **Sistema Operacional**: Controla e coordena o uso do harware entre as diversas aplicações dos usuários.
- **Aplicativos**: Definem a forma com a qual os recursos são usados para resolverem os problemas dos usuários (Compiladores, bancos de dados, editores e etc).
- **Usuários**: Podem ser pessoas, dispositivos e até mesmo outras máquinas.

Desse modo, é possível representar um sistema computacional de forma abstrata de acordo com a imagem abaixo.

<img width="560" height="418" alt="image" src="https://github.com/user-attachments/assets/bcbaa96b-3a66-44d3-98e1-ca71fb6a1b53" />

---

## Tipos de sistemas

### Sistema em Lotes (Batch)

Em um sistema de lotes, a memória pode ser representada em um layout de duas camadas, no topo a memória do SO, e abaixo a memória dos programas do usuário.
Além disso, o operador era responsável por submeter e organizar a execução dos jobs, enquanto o usuário apenas preparava suas tarefas, geralmente em cartões perfurados.
Para otimizar o tempo, tarefas semelhantes eram agrupadas e executadas em sequência.

Com o sequenciamento automático, o controle passava de um job para outro sem intervenção manual, 
utilizando um monitor residente, que iniciava o sistema, transferia o controle ao job e o retomava
ao final de cada execução.

Apesar disso, havia baixa performance, pois CPU e operações de entrada/saída não eram sobrepostas,
deixando a CPU ociosa. Para melhorar, surgiu a operação off-line, em que jobs eram carregados por 
fitas magnéticas, enquanto leitura e impressão ocorriam separadamente. Além disso, o uso de um job pool 
permitiu escolher melhor o próximo job, aumentando o aproveitamento da CPU.

### Sistema em batch multiprogramado

Nos sistemas em batch multiprogramados, vários jobs são mantidos simultaneamente na memória principal, 
permitindo que a CPU seja compartilhada entre eles por meio da multiprogramação. Isso aumenta o
aproveitamento do processador, pois enquanto um job aguarda operações de entrada/saída, outro pode 
utilizar a CPU.

Para suportar esse modelo, o sistema operacional precisa oferecer algumas funcionalidades essenciais.
Entre elas, estão as rotinas de I/O, que passam a ser gerenciadas pelo próprio sistema; 
a gerência de memória, responsável por alocar espaço para múltiplos jobs ao mesmo tempo;
o escalonamento de CPU, que decide qual job pronto será executado em determinado momento; 
e a alocação de dispositivos, garantindo que os recursos de hardware sejam distribuídos de
forma eficiente entre os jobs.

