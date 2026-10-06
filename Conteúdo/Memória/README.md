# Gerenciamento de memória

## Fundamentos

Para que um programa possa ser executado, ele precisa ser trazido do armazenamento para a memória principal e associado a um processador. Os processos que estão no disco esperando para serem carregados e executados formam a **fila de entrada**. O gerenciamento de memória é responsável por controlar como os processos ocupam e utilizam a memória durante sua execução.

## Espaço de endereçamento lógico e físico

O sistema diferencia o **espaço de endereçamento lógico** do **espaço de endereçamento físico**. O endereço lógico, também chamado de endereço virtual, é aquele gerado pela CPU e utilizado pelo processo. Já o endereço físico corresponde ao endereço efetivamente utilizado pela memória. Essa separação permite que o programa trabalhe com endereços lógicos sem precisar conhecer diretamente a posição física onde seus dados estão armazenados.

## Unidade de Gerenciamento de Memória (MMU)

A **MMU (Memory Management Unit)** é um dispositivo de hardware responsável por realizar o mapeamento de endereços virtuais para endereços físicos. No esquema apresentado, um valor armazenado no registrador de relocação é adicionado ao endereço produzido pelo processo antes que ele seja enviado para a memória. Dessa forma, o programa trabalha apenas com endereços lógicos e não precisa saber quais endereços físicos correspondem a eles.

## Swapping

O **swapping** permite remover temporariamente um processo da memória principal e armazená-lo em um dispositivo de armazenamento auxiliar. Quando o processo precisa continuar sua execução, ele pode ser trazido novamente para a memória. O armazenamento auxiliar deve ser grande o suficiente para manter cópias das imagens de memória dos processos e permitir acesso direto a essas imagens.

Uma variação é o **roll out, roll in**, utilizada em algoritmos de escalonamento baseados em prioridades. Nesse caso, processos de menor prioridade podem ser retirados da memória para liberar espaço para processos de maior prioridade. Como a maior parte do tempo gasto no swapping corresponde à transferência de dados, o tempo necessário está diretamente relacionado à quantidade de memória transferida.

## Alocação contígua

Na **alocação contígua**, a memória principal normalmente é dividida entre o sistema operacional e os processos de usuário. O sistema operacional costuma ocupar uma região da memória, enquanto os processos de usuário ocupam outra região.

Nas **partições simples**, o registrador de relocação ajuda a proteger o sistema operacional contra os processos de usuário e também impede que um processo acesse indevidamente a memória de outro. O registrador de relocação contém o menor endereço físico permitido, enquanto o registrador de limite determina a faixa de endereços lógicos que pode ser utilizada.

## Alocação de partições múltiplas

Na alocação de múltiplas partições, existem diferentes espaços livres espalhados pela memória, chamados de **buracos**. Quando um processo chega, o sistema operacional procura um buraco suficientemente grande para armazená-lo. Dessa forma, o sistema precisa manter informações sobre quais regiões estão ocupadas e quais regiões estão livres.

## Alocação dinâmica de memória

Para escolher um buraco para um novo processo, existem diferentes estratégias de alocação. O **First-fit** escolhe o primeiro buraco encontrado que seja grande o suficiente para atender à solicitação. O **Best-fit** escolhe o menor buraco que seja capaz de acomodar o processo, sendo necessário pesquisar a lista de espaços livres, a menos que ela esteja organizada de maneira adequada. O **Worst-fit** escolhe o maior buraco disponível.

De acordo com o material, **First-fit e Best-fit apresentam resultados melhores que Worst-fit**, tanto em relação à velocidade quanto à utilização da memória.

## Fragmentação

A fragmentação ocorre quando o espaço de memória não é utilizado de maneira eficiente. Na **fragmentação externa**, existe memória livre suficiente para atender a uma requisição, mas ela está dividida em diferentes espaços não contíguos. Assim, mesmo havendo memória suficiente no total, não existe um único bloco contínuo capaz de atender à solicitação.

Na **fragmentação interna**, a quantidade de memória alocada para um processo é maior do que a quantidade realmente necessária. A parte excedente permanece ociosa dentro da partição alocada.

## Compactação

A **compactação** é uma técnica utilizada para reduzir a fragmentação externa. Ela consiste em deslocar o conteúdo dos processos na memória para reunir todos os espaços livres em uma única região contígua. Dessa forma, vários pequenos buracos podem ser transformados em um espaço livre maior.

A compactação só é possível quando a relocação dos processos pode ser realizada de maneira dinâmica durante a execução.

## Paginação

A **paginação** permite que o espaço de endereçamento lógico de um processo não seja necessariamente contíguo. Para isso, a memória física é dividida em blocos de tamanho fixo chamados **quadros (frames)**, enquanto a memória lógica é dividida em blocos do mesmo tamanho chamados **páginas**.

Para executar um programa, o sistema precisa encontrar uma quantidade suficiente de quadros livres para armazenar suas páginas. Uma **tabela de páginas** é utilizada para relacionar as páginas do espaço lógico aos quadros correspondentes na memória física.

A paginação elimina a necessidade de alocar um processo inteiro em uma região contígua da memória, mas pode provocar **fragmentação interna**.

## Tradução de endereços na paginação

Um endereço lógico produzido pela CPU é dividido em duas partes: o **número da página (p)** e o **deslocamento da página (d)**. O número da página é utilizado como índice na tabela de páginas, que contém o endereço base do quadro correspondente na memória física.

O deslocamento identifica a posição específica dentro daquela página. Assim, o endereço físico é obtido combinando o endereço base do quadro encontrado na tabela com o deslocamento do endereço lógico.

## Implementação da tabela de páginas

A tabela de páginas é mantida na memória principal. O **PTBR (Page Table Base Register)** aponta para o início da tabela de páginas, enquanto o **PTLR (Page Table Length Register)** indica o tamanho da tabela.

Nesse modelo, cada acesso a uma instrução ou dado pode exigir dois acessos à memória: primeiro é necessário acessar a tabela de páginas para descobrir o quadro correspondente e, depois, acessar efetivamente o dado ou a instrução.

## TLB e registradores associativos

Para reduzir o custo dos dois acessos à memória, pode ser utilizada uma memória cache especializada chamada **Translation Lookaside Buffer (TLB)**, também apresentada no material como **registradores associativos**.

A TLB mantém algumas traduções de páginas que foram utilizadas recentemente. Quando a página procurada está na TLB, o número do quadro pode ser obtido rapidamente. Caso contrário, é necessário consultar a tabela de páginas armazenada na memória.

Os registradores associativos realizam uma **busca paralela**, verificando simultaneamente as entradas disponíveis para encontrar o número da página correspondente.

## Tempo efetivo de acesso

O desempenho da TLB depende da **taxa de acerto**, que representa a porcentagem de vezes em que o número da página procurado é encontrado nos registradores associativos.

Quando ocorre um acerto, a tradução do endereço pode ser obtida rapidamente. Quando ocorre uma falha, é necessário consultar a tabela de páginas na memória antes de acessar o dado ou a instrução. Portanto, quanto maior a taxa de acerto da TLB, menor tende a ser o tempo efetivo de acesso.

No material, o tempo efetivo de acesso é representado pela expressão:

**EAT = (1 + ε)α + (2 + ε)(1 − α)**

onde **α** representa a taxa de acerto e **ε** representa o tempo necessário para analisar os registradores associativos.

## Proteção de memória

A proteção de memória pode ser implementada associando um **bit de proteção** a cada quadro. Na tabela de páginas, utiliza-se também o bit **válido-inválido**.

Uma entrada marcada como **válida** indica que a página correspondente pertence ao espaço de endereçamento lógico do processo e representa uma página legal. Uma entrada marcada como **inválida** indica que aquela página não pertence ao espaço de endereçamento lógico do processo.

## Paginação de dois níveis

Quando as tabelas de páginas ficam muito grandes, pode-se utilizar uma estrutura de **paginação de dois níveis**. Nesse modelo, a própria tabela de páginas é dividida em páginas, reduzindo a necessidade de manter uma grande tabela inteira em memória.

No exemplo apresentado para uma arquitetura de 32 bits com páginas de 4 KB, o endereço lógico possui 20 bits destinados ao número da página e 12 bits destinados ao deslocamento. O número da página pode ser dividido em duas partes de 10 bits: uma parte funciona como índice da tabela externa e outra como índice dentro da tabela correspondente.

O endereço lógico assume, portanto, a estrutura:

**pi | p2 | d**

onde **pi** representa o índice da tabela de páginas externa, **p2** representa o índice dentro da tabela de páginas e **d** representa o deslocamento dentro da página.

## Paginação de múltiplos níveis

A paginação também pode utilizar mais de dois níveis. No exemplo apresentado para o **Motorola 68030**, é utilizada uma paginação de quatro níveis.

Como cada nível é armazenado como uma tabela separada na memória, a tradução de um endereço lógico para um endereço físico pode exigir vários acessos à memória. Nesse exemplo, a conversão pode levar até quatro acessos.

Mesmo com esse custo, o uso de uma cache permite manter o desempenho em um nível razoável. O material apresenta um exemplo em que, com uma taxa de acerto de cache de 98%, o tempo de acesso efetivo é de 128 nanossegundos, apenas 28% maior que o tempo de acesso direto à memória.

## Páginas compartilhadas

A paginação também permite que processos compartilhem determinadas páginas de memória. Um exemplo é o **código compartilhado**, em que uma única cópia de código, mantida como somente leitura, pode ser compartilhada por vários processos.

Programas como editores de texto e compiladores podem utilizar esse mecanismo. Por outro lado, o código e os dados privados de cada processo permanecem separados, de modo que cada processo mantém sua própria cópia dessas informações.

As páginas destinadas ao código e aos dados privados podem aparecer em diferentes posições dentro do espaço de endereçamento lógico de cada processo.
