# Sistemas Operacionais - Processos

## Conceito de processo

Os processos, em sistemas operacionais, representam **programas em execução**. A execução deve ser **sequencial**.

Os termos **processo** e **job** podem ser utilizados como sinônimos.

Um processo inclui:

* **Contador de programa:** indica a próxima instrução que deverá ser executada.
* **Pilha:** armazena informações relacionadas às funções, como parâmetros e retornos.
* **Seção de dados:** representa os dados utilizados pelo processo.

Os sistemas operacionais executam diferentes tipos de programas:

* **Sistemas em batch:** utilizam o conceito de **jobs**.
* **Sistemas de tempo compartilhado:** executam programas de usuários ou tarefas.

---

## Estados de um processo

Durante sua execução, o processo pode mudar de estado. Os principais estados são:

* **Novo:** o processo está sendo criado.
* **Em execução:** as instruções do processo estão sendo executadas.
* **Em espera:** o processo está esperando pela ocorrência de algum evento.
* **Pronto:** o processo está esperando para ser atribuído a um processador.
* **Encerrado:** o processo terminou sua execução.

O processo pode passar por diferentes estados durante sua execução, de acordo com o que está acontecendo com ele e com a disponibilidade da CPU.

---

## Bloco de Controle de Processos (PCB)

O **PCB (Process Control Block)** é o bloco que contém as informações associadas a cada processo.

Ele possui informações como:

* **Estado do processo**
* **Contador de programa**
* **Registradores da CPU**
* **Informações de escalonamento da CPU**
* **Informações de gerência de memória**
* **Informações de contabilidade**
* **Informações de status de I/O**

O PCB permite que o sistema operacional mantenha as informações necessárias para controlar e acompanhar cada processo.

---

## Filas de escalonamento de processos

Os processos podem permanecer em diferentes filas dentro do sistema operacional.

### Fila de Jobs

É o conjunto de **todos os processos existentes no sistema**.

### Fila de processos prontos

É o conjunto dos processos que:

* estão na memória;
* estão prontos para executar;
* estão esperando para serem executados pela CPU.

### Fila de dispositivos

É formada pelos processos que estão **esperando por algum dispositivo de I/O**.

O processo pode **migrar entre as diferentes filas** durante sua execução.

---

## Escalonamento de processos

O **escalonamento de processos** é responsável por determinar quais processos serão selecionados para serem executados.

Existem diferentes tipos de escalonadores:

### Escalonador de longo prazo

Também chamado de **escalonador de jobs**.

Sua função é selecionar quais processos serão levados para a memória e colocados na **fila de processos prontos**.

Ele é executado com menor frequência, normalmente em intervalos de **segundos ou minutos**. Por isso, pode ser mais lento.

Uma de suas funções é controlar o **grau de multiprogramação**, ou seja, a quantidade de processos que estarão disponíveis para execução.

### Escalonador de curto prazo

Também chamado de **escalonador de CPU**.

Sua função é selecionar **qual processo será executado e receberá a CPU**.

Ele é executado com muita frequência, normalmente em intervalos de **milissegundos**. Por isso, precisa ser rápido.

### Escalonador de médio prazo

Pode existir um nível intermediário de escalonamento responsável por **reduzir o grau de multiprogramação**.

Ele realiza a inserção e remoção de processos da memória, processo conhecido como **swapping**.

---

## Processos limitados por I/O e por CPU

Os processos podem ser classificados de acordo com o tipo de atividade que consome mais tempo.

### Limitado por I/O

É o processo que passa **mais tempo realizando operações de entrada e saída (I/O)** do que realizando computação.

### Limitado por CPU

É o processo que passa **mais tempo realizando computação** do que operações de entrada e saída.

---

## Troca de contexto

A **troca de contexto** acontece quando a CPU deixa de executar um processo e passa a executar outro.

Quando a CPU recebe um novo processo:

1. O estado do processo anterior precisa ser **salvo**.
2. O estado do novo processo precisa ser **carregado**.
3. A CPU passa a executar o novo processo.

O tempo utilizado para realizar essa troca é considerado um **overhead**, pois durante esse período nenhum trabalho útil do processo é realizado.

O tempo necessário para realizar a troca de contexto depende do **suporte oferecido pelo hardware**.

---

# Resumo para prova

**Processo:** programa em execução, cuja execução deve ser sequencial.

**Processo = Job:** os termos podem ser usados como sinônimos.

**Um processo possui:**

* Contador de programa
* Pilha
* Seção de dados

**Estados do processo:**

* Novo
* Em execução
* Em espera
* Pronto
* Encerrado

**PCB:** bloco que armazena informações sobre o processo, como estado, contador de programa, registradores, escalonamento, memória, contabilidade e I/O.

**Filas:**

* Fila de Jobs → todos os processos do sistema.
* Fila de processos prontos → processos na memória esperando pela CPU.
* Fila de dispositivos → processos esperando por I/O.

**Escalonadores:**

* **Longo prazo:** escolhe processos que irão para a memória; controla o grau de multiprogramação.
* **Curto prazo:** escolhe qual processo receberá a CPU; precisa ser rápido.
* **Médio prazo:** reduz o grau de multiprogramação por meio de inserção/remoção de processos da memória (**swapping**).

**Processo limitado por I/O:** passa mais tempo realizando I/O.

**Processo limitado por CPU:** passa mais tempo realizando computação.

**Troca de contexto:** salva o estado do processo anterior e carrega o estado do novo processo. É um **overhead**, pois não realiza trabalho útil durante a troca.
