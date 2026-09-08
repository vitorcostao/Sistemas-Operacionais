# Programação Concorrente

Programação concorrente trata da execução de processos ou atividades que possuem potencial para ocorrer ao mesmo tempo. O objetivo não é apenas obter ganho de desempenho, mas também organizar corretamente a comunicação e a sincronização entre tarefas que compartilham recursos.

Quando duas ou mais tarefas são executadas concorrentemente, a ordem exata das operações pode variar. Por isso, um programa concorrente precisa ser projetado de forma que continue correto independentemente da ordem em que os processos sejam escalonados.

## Concorrência e paralelismo

Concorrência significa que diferentes processos podem progredir durante o mesmo intervalo de tempo. Paralelismo significa que eles realmente executam simultaneamente, normalmente em processadores ou núcleos diferentes.
Em pseudocódigo, construções como `cobegin` e `coend` representam trechos que podem ser executados concorrentemente. O sistema de hardware e software decide se essa execução ocorrerá de forma realmente paralela ou apenas intercalada.
Um exemplo comum é dividir um problema em partes independentes, executar essas partes concorrentemente e, ao final, combinar seus resultados. Porém, quando uma etapa depende do término de outra, é necessário algum mecanismo de sincronização.

## Exclusão Mútua

Exclusão mútua é uma forma de sincronização usada quando dois ou mais processos compartilham um recurso que não pode ser utilizado simultaneamente. O trecho do programa que acessa esse recurso é chamado de **seção crítica**. A ideia fundamental é simples: se dois processos tentarem entrar em suas seções críticas ao mesmo tempo, apenas um deles deve ter sucesso. O outro precisa aguardar até que o recurso seja liberado.

Um processo pode ser visto de forma geral como:

```text
comandos
pré-protocolo
seção crítica
pós-protocolo
comandos
```

O pré-protocolo controla a entrada na seção crítica, enquanto o pós-protocolo informa que o processo terminou de utilizá-la.

## Atomicidade

Uma operação é atômica quando não pode ser interrompida no meio de sua execução. Isso é importante porque instruções aparentemente simples, como:

```text
n = n + 1
```

podem envolver várias etapas internas, como ler `n`, somar `1` e armazenar o novo valor. Se dois processos realizarem essas etapas ao mesmo tempo sem sincronização, o resultado pode ser incorreto.

## Problemas de Sincronização

Uma solução de exclusão mútua não deve apenas impedir a entrada simultânea na seção crítica. Ela também deve evitar situações em que os processos deixam de progredir.

## Deadlock

Deadlock ocorre quando dois ou mais processos ficam esperando indefinidamente uns pelos outros. Nenhum consegue continuar. Um exemplo simples acontece quando `P1` espera `P2` liberar um recurso enquanto `P2`, ao mesmo tempo, espera `P1` liberar outro recurso.

### Lockout e Starvation

Lockout, também chamado de starvation, ocorre quando um processo pode permanecer esperando indefinidamente enquanto outros continuam executando. Nesse caso, o sistema continua funcionando, mas um processo específico pode nunca conseguir acessar o recurso desejado.

## Busy Waiting

Busy waiting, ou espera ocupada, é uma técnica na qual um processo verifica repetidamente se já pode entrar na seção crítica.
Um exemplo genérico é:

```text
while outro_processo_esta_na_secao_critica do
    ;
```

O processo permanece ativo, testando continuamente uma condição. A técnica pode resolver alguns problemas de sincronização, mas desperdiça tempo de CPU enquanto o processo está apenas esperando.

## Algoritmo de Dekker

O algoritmo de Dekker é uma solução clássica de exclusão mútua para dois processos. Ele combina duas ideias principais: cada processo informa sua intenção de entrar na seção crítica e uma variável `turn` é utilizada para decidir quem possui prioridade quando ambos querem entrar ao mesmo tempo.
Usando uma convenção mais intuitiva, podemos considerar:

```text
c1 = 1  -> P1 quer entrar
c1 = 0  -> P1 não quer entrar

c2 = 1  -> P2 quer entrar
c2 = 0  -> P2 não quer entrar
```

Quando apenas um processo deseja entrar, ele pode prosseguir normalmente. Quando os dois demonstram interesse ao mesmo tempo, `turn` funciona como um critério de desempate. Se `turn = 1`, `P1` recebe prioridade e `P2` recua temporariamente. Quando `P1` termina sua seção crítica, ele passa a vez para `P2`. O comportamento é simétrico quando `turn = 2`. Essa combinação evita que os dois processos entrem simultaneamente e também evita que ambos fiquem esperando para sempre. Assim, o algoritmo procura garantir exclusão mútua, ausência de deadlock e ausência de lockout.
Uma forma simplificada para `P1` é:

```text
c1 = 1

while c2 = 1 do
    if turn = 2 then
    begin
        c1 = 0

        while turn = 2 do
            ;

        c1 = 1
    end

secao_critica

turn = 2
c1 = 0
```

O processo `P2` possui a mesma lógica, trocando os papéis de `1` e `2`.

## Semáforos

Semáforos fornecem uma forma mais direta de implementar sincronização. Um semáforo é uma variável controlada por operações especiais, normalmente chamadas de `wait` e `signal`. A operação `wait` tenta adquirir o recurso. Se o recurso estiver disponível, o processo pode continuar; caso contrário, ele é suspenso. A operação `signal` libera o recurso e pode acordar algum processo que esteja esperando.
Para exclusão mútua, pode-se utilizar um semáforo binário inicializado com `1`:

```text
s = 1
```

Cada processo executa:

```text
wait(s)
secao_critica
signal(s)
```

O primeiro processo que executa `wait(s)` entra na seção crítica. Os demais ficam impedidos de entrar até que seja executado `signal(s)`. A principal vantagem em relação ao busy waiting é que um processo bloqueado pode ser suspenso em vez de consumir CPU verificando continuamente uma condição.

## Problema Produtor-Consumidor

O problema produtor-consumidor representa uma situação em que um processo produz dados e outro processo os consome. Os dados normalmente são armazenados em um buffer compartilhado. O produtor precisa inserir dados no buffer, enquanto o consumidor só pode retirar dados que já tenham sido produzidos. Se o buffer estiver vazio, o consumidor deve esperar. Um semáforo pode representar a quantidade de itens disponíveis no buffer. O produtor executa `signal` depois de inserir um novo item, enquanto o consumidor executa `wait` antes de retirar um item.

Quando a inserção e a retirada também são operações críticas, outro semáforo pode ser utilizado para garantir exclusão mútua no acesso ao buffer. Assim, o problema produtor-consumidor mostra que sincronização pode ter dois objetivos diferentes: controlar **quando** uma operação pode acontecer e controlar **quem** pode acessar um recurso compartilhado em determinado momento.

## Conclusão

Programação concorrente permite que diferentes atividades progridam de maneira independente, mas exige cuidado quando elas compartilham dados ou recursos. A ordem de execução dos processos não é totalmente previsível, e pequenas alterações nessa ordem podem produzir resultados diferentes quando não existe sincronização adequada. A exclusão mútua garante que apenas um processo utilize uma seção crítica por vez. Soluções baseadas apenas em variáveis compartilhadas podem apresentar problemas como deadlock, lockout ou consumo excessivo de CPU por busy waiting. 

O algoritmo de Dekker demonstra como esses problemas podem ser tratados para dois processos usando intenção de entrada e prioridade. Semáforos oferecem uma abstração mais simples e prática, permitindo controlar exclusão mútua e sincronização entre processos. Eles também são fundamentais para problemas clássicos, como produtor-consumidor, nos quais diferentes tarefas precisam coordenar o acesso a dados compartilhados.
