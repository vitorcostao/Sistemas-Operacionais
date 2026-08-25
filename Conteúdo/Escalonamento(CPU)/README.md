# Sistemas Operacionais - Escalonamento de CPU

## Conceitos básicos

A **multiprogramação** permite aumentar a utilização da CPU. Em um sistema com apenas um processador, somente **um processo pode estar em execução por vez**.

A execução de um processo ocorre através da alternância entre:

* **Surto de CPU:** período em que o processo está utilizando a CPU.
* **Espera de I/O:** período em que o processo aguarda uma operação de entrada/saída.

Portanto, a execução de um processo consiste na alternância entre **ciclos de CPU e ciclos de espera de I/O**.

---

## Escalonador de CPU

O **escalonador de CPU** seleciona um dos processos que estão na memória e prontos para execução e **aloca a CPU para ele**.

Existem quatro situações em que pode ocorrer uma decisão de escalonamento:

1. O processo passa de **execução → espera**.
2. O processo passa de **execução → pronto**.
3. O processo passa de **espera → pronto**.
4. O processo **termina**.

### Escalonamento não-preemptivo

Quando o escalonamento ocorre somente nos casos **1 e 4**, temos um escalonamento **não-preemptivo**.

Nesse caso, o processo não é interrompido enquanto está utilizando a CPU.

### Escalonamento preemptivo

Quando o escalonamento também pode ocorrer nos casos **2 e 3**, temos um escalonamento **preemptivo**.

Nesse caso, o processo pode ser interrompido para que outro processo seja executado.

---

## Dispatcher

O **dispatcher** é o módulo responsável por entregar o controle da CPU ao processo escolhido pelo escalonador de curto prazo.

Ele realiza:

* **Mudança de contexto**
* **Mudança para o modo usuário**
* Salto para a posição adequada do programa do usuário
* Reinício da execução do programa

### Latência de dispatch

É o **tempo necessário para o dispatcher interromper um processo e iniciar a execução do próximo processo**.

---

# Critérios de escalonamento

Os critérios são utilizados para comparar diferentes algoritmos de escalonamento.

### Utilização da CPU

Busca manter a CPU **o mais ocupada possível**.

### Throughput

É o **número de processos que completam sua execução por unidade de tempo**.

### Tempo de retorno

É a **quantidade de tempo necessária para executar um processo**.

### Tempo de espera

É a **quantidade de tempo que um processo permanece na fila de processos prontos**.

### Tempo de resposta

É o tempo entre uma **requisição e a produção da primeira resposta**.

## Objetivos de otimização

Um bom algoritmo de escalonamento busca:

* **Máxima utilização da CPU**
* **Máximo throughput**
* **Mínimo tempo de retorno**
* **Mínimo tempo de espera**
* **Mínimo tempo de resposta**

---

# FCFS — Primeiro a chegar é servido

**FCFS (First-Come, First-Served)** executa os processos na ordem em que eles chegam.

Exemplo:

| Processo | Duração do surto |
| -------- | ---------------: |
| P1       |               24 |
| P2       |                3 |
| P3       |                3 |

Se a ordem de chegada for:

**P1 → P2 → P3**

O tempo de espera será:

* P1 = 0
* P2 = 24
* P3 = 27

Tempo médio de espera:

**(0 + 24 + 27) / 3 = 17**

### Efeito comboio

O FCFS pode apresentar o **efeito comboio**, no qual processos pequenos ficam esperando atrás de um processo longo.

Por exemplo, se a ordem for:

**P2 → P3 → P1**

Os tempos de espera serão:

* P1 = 6
* P2 = 0
* P3 = 3

Tempo médio:

**(6 + 0 + 3) / 3 = 3**

Nesse caso, o resultado é muito melhor.

O **efeito comboio** ocorre quando processos pequenos ficam atrás de processos longos.

---

# SJF — Job mais curto primeiro

**SJF (Shortest Job First)** associa a cada processo o tamanho do seu próximo surto de CPU e escolhe o processo com o **menor tempo**.

Existem dois tipos:

### SJF não-preemptivo

Depois que a CPU é atribuída ao processo, ela **não pode ser retirada até que o surto seja concluído**.

### SJF preemptivo

Se um novo processo chegar com um tempo de surto menor que o tempo restante do processo que está executando, ocorre uma troca.

Esse esquema é chamado de:

**SRTF — Shortest Remaining Time First**

ou

**Menor-tempo-restante-primeiro**.

O SJF é considerado **ótimo**, pois produz o **menor tempo médio de espera** para o conjunto de processos.

---

# Escalonamento por prioridade

Nesse algoritmo, cada processo recebe um **número de prioridade**.

A CPU é alocada para o processo de **maior prioridade**.

No slide:

**menor valor = maior prioridade**

O escalonamento por prioridade pode ser:

* **Preemptivo**
* **Não-preemptivo**

O **SJF também pode ser considerado um escalonamento por prioridade**, em que a prioridade é determinada pelo tempo do surto de CPU.

### Starvation

Um problema do escalonamento por prioridade é a **starvation**.

Isso acontece quando processos de baixa prioridade podem **nunca serem executados**, pois processos de maior prioridade continuam recebendo a CPU.

### Aging — Envelhecimento

Uma solução é o **aging (envelhecimento)**.

Nesse mecanismo, a prioridade de um processo **aumenta conforme ele passa mais tempo esperando**.

Assim, processos que estão esperando há muito tempo podem eventualmente receber a CPU.

---

# Round Robin (RR)

No **Round Robin**, cada processo recebe uma pequena quantidade de tempo de CPU chamada **time quantum**.

O quantum geralmente fica entre **10 e 100 ms**.

Quando o tempo do quantum termina:

1. O processo é retirado da CPU.
2. Ele é colocado no final da fila de processos prontos.
3. O próximo processo recebe a CPU.

Se existem **n processos** na fila e o quantum é **q**, cada processo recebe aproximadamente **1/n do tempo da CPU**, em parcelas de no máximo **q unidades de tempo**.

Nenhum processo deverá esperar mais que:

**(n - 1)q**

### Relação entre quantum e desempenho

* **Quantum grande → comportamento semelhante ao FIFO/FCFS**
* **Quantum pequeno → mais trocas de contexto**

Se o quantum for muito pequeno em relação ao tempo necessário para realizar uma troca de contexto, o **overhead será muito alto**.

## O Round Robin normalmente apresenta **maior tempo médio de retorno que o SJF**, mas proporciona uma **melhor resposta**.

# Filas múltiplas

A fila de processos prontos pode ser dividida em diferentes filas.

Por exemplo:

* **Primeiro plano:** processos interativos.
* **Segundo plano:** processos batch.

Cada fila pode possuir seu próprio algoritmo de escalonamento.

Exemplo:

* Primeiro plano → **Round Robin**
* Segundo plano → **FCFS**

Também é necessário definir como será feito o escalonamento **entre as filas**.

### Escalonamento por prioridade fixa

Uma fila pode possuir prioridade sobre outra.

Por exemplo:

**Primeiro plano > Segundo plano**

O problema é que a fila de menor prioridade pode sofrer **starvation**.

### Fatia de tempo

Outra possibilidade é dividir o tempo da CPU entre as filas.

Exemplo:

* 80% da CPU → primeiro plano usando RR
* 20% da CPU → segundo plano usando FCFS

---

# Filas múltiplas com realimentação

Nas **filas múltiplas com realimentação**, um processo pode **passar de uma fila para outra**.

Esse mecanismo também pode ser utilizado para implementar o **envelhecimento (aging)**.

O escalonador é definido por alguns parâmetros:

* Número de filas.
* Algoritmo de escalonamento utilizado em cada fila.
* Método utilizado para promover um processo para uma fila de maior prioridade.
* Método utilizado para rebaixar um processo.
* Método utilizado para determinar em qual fila um processo deve entrar quando precisar de serviço.

### Exemplo

Existem três filas:

* **Q0:** quantum de 8 ms
* **Q1:** quantum de 16 ms
* **Q2:** FCFS

Um novo processo entra na **Q0**.

Ele recebe até **8 ms** de CPU.

Se não terminar nesse tempo:

**Q0 → Q1**

Na Q1, recebe mais **16 ms**.

Se ainda não terminar:

**Q1 → Q2**

Na Q2, utiliza o algoritmo **FCFS**.
