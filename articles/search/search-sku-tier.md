---
title: Escolher um tipo de preço
titleSuffix: Azure Cognitive Search
description: 'Os Pesquisa Cognitiva do Azure podem ser provisionados nessas camadas: gratuita, básica e Standard, e Standard está disponível em várias configurações de recursos e níveis de capacidade.'
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: 0acd0d1d463280cddc8c1f4bb389a056d474ea38
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92101266"
---
# <a name="choose-a-pricing-tier-for-azure-cognitive-search"></a>Escolha um tipo de preço para o Azure Pesquisa Cognitiva

Quando você cria um serviço de Pesquisa Cognitiva do Azure, um [recurso é criado](search-create-service-portal.md) em um tipo de preço que é corrigido durante o tempo de vida do serviço. As camadas incluem gratuito, básico, Standard e otimizado para armazenamento. O padrão e o armazenamento otimizado estão disponíveis com várias configurações e capacidades.

A maioria dos clientes começa com a camada gratuita para que eles possam avaliar o serviço. Após a avaliação, é comum criar um segundo serviço em uma das camadas mais altas para implantações de desenvolvimento e produção.

## <a name="feature-availability-by-tier"></a>Disponibilidade de recursos por camada

A tabela a seguir descreve as restrições de recurso relacionadas à camada.

| Recurso | Limitações |
|---------|-------------|
| [indexadores](search-indexer-overview.md) | Os indexadores não estão disponíveis no S3 HD.  |
| [Enriquecimento de IA](search-security-manage-encryption-keys.md) | É executado na camada gratuita, mas não é recomendado. |
| [Chaves de criptografia gerenciadas pelo cliente](search-security-manage-encryption-keys.md) | Não disponível na camada gratuita. |
| [Acesso de firewall IP](service-configure-firewall.md) | Não disponível na camada gratuita. |
| [Ponto de extremidade privado (integração com o link privado do Azure)](service-create-private-endpoint.md) | Para conexões de entrada para um serviço de pesquisa, não disponível na camada gratuita. Para conexões de saída por indexadores para outros recursos do Azure, não disponível em alta ou S3 HD. Para indexadores que usam habilidades, não disponível em gratuito, básico, S1 ou S3 HD.|

A maioria dos recursos está disponível em todas as camadas, incluindo recursos gratuitos, mas que usam muitos recursos podem não funcionar bem, a menos que você forneça capacidade suficiente. Por exemplo, o [enriquecimento de ia](cognitive-search-concept-intro.md) tem habilidades de longa execução que atingirão o tempo limite em um serviço gratuito, a menos que o conjunto de informações seja pequeno.

## <a name="tiers"></a>Tiers

As camadas são diferenciadas por:

+ Quantidade de índices e indexadores (limites máximos)
+ Tamanho e velocidade de partições (armazenamento físico)

A camada selecionada determina a taxa faturável. A captura de tela a seguir de portal do Azure mostra as camadas disponíveis, menos preços (que você pode encontrar no portal e na [página de preços](https://azure.microsoft.com/pricing/details/search/). **Gratuita**, **básica**e **Standard** são as camadas mais comuns.

**Gratuito** cria um serviço de pesquisa limitado para projetos menores, incluindo guias de início rápido e tutoriais. Internamente, as réplicas e as partições são compartilhadas entre vários assinantes. Você não pode dimensionar um serviço gratuito ou executar cargas de trabalho significativas.

**Básico** e **Standard** são as camadas faturáveis usadas com mais frequência, com **padrão** sendo o padrão. Com recursos dedicados sob seu controle, você pode implantar projetos maiores, otimizar o desempenho e definir a capacidade.

![Tipos de preço do Azure Pesquisa Cognitiva](media/search-sku-tier/tiers.png "Tipos de preço do Azure Pesquisa Cognitiva")

Algumas camadas são otimizadas para determinados tipos de trabalho. Por exemplo, a **alta densidade padrão (S3 HD)** é um *modo de hospedagem* para S3, em que o hardware subjacente é otimizado para um grande número de índices menores e destina-se a cenários de multilocação. S3 HD tem o mesmo encargo por unidade que S3, mas o hardware é otimizado para leituras rápidas de arquivos em um grande número de índices menores.

As camadas de **armazenamento otimizado** oferecem maior capacidade de armazenamento a um preço menor por TB do que as camadas padrão. A compensação primária é maior latência de consulta, que você deve validar para os requisitos de aplicativo específicos.  Para saber mais sobre as considerações de desempenho dessa camada, consulte [Considerações sobre desempenho e otimização](search-performance-optimization.md).

Você pode saber mais sobre as várias camadas na [página de preços](https://azure.microsoft.com/pricing/details/search/), no artigo limites de [serviço no Azure pesquisa cognitiva](search-limits-quotas-capacity.md) e na página do portal quando estiver provisionando um serviço.

## <a name="billable-events"></a>Eventos faturáveis

Uma solução criada no Azure Pesquisa Cognitiva pode gerar custos das seguintes maneiras:

+ Custo do próprio serviço, executando 24x7, na configuração mínima (uma partição e réplica)

+ Adicionando capacidade (réplicas ou partições)

+ Encargos de largura de banda (transferência de dados de saída)

+ Serviços de complemento necessários para recursos ou recursos específicos:

  + Enriquecimento de ia (requer [Serviços cognitivas](https://azure.microsoft.com/pricing/details/cognitive-services/))
  + loja de conhecimento (requer o [armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/))
  + enriquecimento incremental (requer o [armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/), aplica-se ao enriquecimento de ia)
  + chaves gerenciadas pelo cliente e criptografia dupla (requer [Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/))
  + pontos de extremidade privados para um modelo de acesso sem Internet (requer o [link privado do Azure](https://azure.microsoft.com/pricing/details/private-link/))

### <a name="service-costs"></a>Custos de serviço

Ao contrário das máquinas virtuais ou outros recursos que podem ser "pausados" para evitar cobranças, um serviço de Pesquisa Cognitiva do Azure está sempre disponível em hardware dedicado para seu uso exclusivo. Dessa forma, a criação de um serviço é um evento cobrável que começa quando você cria o serviço e termina quando você exclui o serviço. 

O encargo mínimo é a primeira unidade de pesquisa (uma réplica x uma partição) na taxa faturável. Esse mínimo é fixo durante o tempo de vida do serviço, pois o serviço não pode ser executado em nada menor que essa configuração. Além do mínimo, você pode adicionar réplicas e partições independentemente umas das outras. Aumentos incrementais na capacidade por meio de réplicas e partições aumentarão sua fatura com base na fórmula a seguir: [(réplicas x partições x taxa)](#search-units), em que a taxa cobrada depende do tipo de preço selecionado.

Quando estiver estimando o custo de uma solução de pesquisa, tenha em mente que os preços e a capacidade não são lineares. (Dobrar a capacidade mais do que o dobro do custo.) Para obter um exemplo de como a fórmula funciona, consulte [como alocar réplicas e partições](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).

### <a name="bandwidth-charges"></a>Cobranças de largura de banda

O uso de [indexadores](search-indexer-overview.md) pode afetar a cobrança, dependendo do local dos seus serviços. Você pode eliminar os encargos de saída de dados inteiramente se criar o serviço de Pesquisa Cognitiva do Azure na mesma região que seus dados. Aqui estão algumas informações da [página de preços de largura de banda](https://azure.microsoft.com/pricing/details/bandwidth/):

+ A Microsoft não cobra por nenhum dado de entrada para nenhum serviço no Azure, ou para qualquer dado de saída do Azure Pesquisa Cognitiva.
+ Em soluções de multiatendimento, não há nenhum encargo para dados que cruzam a conexão quando todos os serviços estão na mesma região.

Os encargos se aplicam a dados de saída se os serviços estiverem em regiões diferentes. Esses encargos não são, na verdade, parte da sua conta de Pesquisa Cognitiva do Azure. Eles são mencionados aqui porque, se você estiver usando os dados ou indexadores aprimorados de ia para efetuar pull de dados de regiões diferentes, verá os custos refletidos na sua fatura geral.

### <a name="ai-enrichment-with-cognitive-services"></a>Enriquecimento de ia com serviços cognitivas

Para [aprimorar o ia](cognitive-search-concept-intro.md), você deve planejar [anexar um recurso de serviços cognitivas do Azure cobrável](cognitive-search-attach-cognitive-services.md), na mesma região que o Azure pesquisa cognitiva, no tipo de preço S0 para processamento pago conforme o uso. Não há nenhum custo fixo associado à anexação de serviços cognitivas. Você paga apenas pelo processamento de que precisa.

| Operação | Impacto de cobrança |
|-----------|----------------|
| Quebra de documentos, extração de texto | Grátis |
| Quebra de documento, extração de imagem | Cobrado de acordo com o número de imagens extraídas dos seus documentos. Em uma [configuração de indexador](/rest/api/searchservice/create-indexer#indexer-parameters), **imageaction** é o parâmetro que dispara a extração de imagem. Se **imageaction** for definido como "None" (o padrão), você não será cobrado pela extração de imagem. A taxa de extração de imagem está documentada na página de [detalhes de preços](https://azure.microsoft.com/pricing/details/search/) do Azure pesquisa cognitiva.|
| [Habilidades cognitivas internas](cognitive-search-predefined-skills.md) | Cobrado na mesma taxa que se você executou a tarefa usando serviços cognitivas diretamente. |
| Habilidades personalizadas | Uma habilidade personalizada é A funcionalidade que você fornece. O custo de usar uma habilidade personalizada depende totalmente de se o código personalizado está chamando outros serviços medidos. |

O recurso de [enriquecimento incremental (versão prévia)](cognitive-search-incremental-indexing-conceptual.md) permite que você forneça um cache que permite que o indexador seja mais eficiente na execução apenas das habilidades cognitivas necessárias se você modificar o seu Skill no futuro, economizando tempo e dinheiro.

<a name="search-units"></a>

## <a name="billing-formula-r-x-p--su"></a>Fórmula de cobrança (R x P = SU)

O conceito de cobrança mais importante a ser compreendido para as operações de Pesquisa Cognitiva do Azure é a Su ( *unidade de pesquisa* ). Como o Azure Pesquisa Cognitiva depende de réplicas e partições para indexação e consultas, não faz sentido cobrar por apenas um ou outro. Em vez disso, a cobrança baseia-se em uma combinação de ambos.

SU é o produto das *réplicas* e *partições* usadas por um serviço: **(R x P = su)**.

Cada serviço começa com uma UA (uma réplica multiplicada por uma partição) como o mínimo. O máximo de qualquer serviço é o SUs 36. Esse máximo pode ser alcançado de várias maneiras: 6 partições x 6 réplicas ou 3 partições x 12 réplicas, por exemplo. É comum usar a capacidade menor que o total (por exemplo, um serviço de 3 réplicas, de três partições cobrado como 9 SUs). Consulte o gráfico de [combinações de partição e réplica](search-capacity-planning.md#chart) para obter combinações válidas.

A taxa de cobrança é por hora por SU. Cada camada tem uma taxa progressivamente maior. As camadas mais altas vêm com partições maiores e speediers, e isso contribui para uma taxa geral maior por hora para essa camada. Você pode exibir as taxas de cada camada na página de [detalhes de preços](https://azure.microsoft.com/pricing/details/search/) .

A maioria dos clientes coloca apenas uma parte da capacidade total online, mantendo o restante em reserva. Para cobrança, o número de partições e réplicas que você coloca online, calculadas pela fórmula SU, determina o que você paga por hora.

## <a name="how-to-manage-costs"></a>Como gerenciar custos

As sugestões a seguir podem ajudá-lo a reduzir os custos ou gerenciar custos com mais eficiência:

+ Crie todos os recursos na mesma região, ou no menor número de regiões possível, para minimizar ou eliminar encargos de largura de banda.

+ Consolide todos os serviços em um grupo de recursos, como o Azure Pesquisa Cognitiva, serviços cognitivas e quaisquer outros serviços do Azure usados em sua solução. No portal do Azure, encontre o grupo de recursos e use os comandos de **Gerenciamento de custos** para obter informações sobre gastos reais e projetados.

+ Considere o aplicativo Web do Azure para seu aplicativo front-end para que as solicitações e respostas permaneçam dentro do limite de data center.

+ Escalar verticalmente para operações com uso intensivo de recursos, como indexação, e reajustar verticalmente para cargas de trabalho de consulta regular. Comece com a configuração mínima do Azure Pesquisa Cognitiva (uma SU composta de uma partição e uma réplica) e, em seguida, monitore a atividade do usuário para identificar os padrões de uso que indicariam a necessidade de mais capacidade. Se houver um padrão previsível, você poderá sincronizar a escala com a atividade (você precisaria escrever código para automatizar isso).

Além disso, visite [cobrança e gerenciamento de custos](../cost-management-billing/cost-management-billing-overview.md) para ferramentas e recursos internos relacionados a gastos.

Não é possível desligar um serviço de pesquisa em uma base temporária. Os recursos dedicados estão sempre operacionais, alocados para seu uso exclusivo durante o tempo de vida do seu serviço. Excluir um serviço é permanente e também exclui seus dados associados.

Em termos do próprio serviço, a única maneira de reduzir sua conta é reduzir as réplicas e partições para um nível que ainda fornece desempenho aceitável e [conformidade de SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), ou criar um serviço em uma camada mais baixa (as tarifas de S1 por hora são menores do que as taxas S2 ou S3). Supondo que você provisione seu serviço na extremidade inferior de suas projeções de carga, se você ultrapassar o serviço, poderá criar um segundo serviço em camadas maior, recriar os índices no segundo serviço e, em seguida, excluir o primeiro.

## <a name="how-to-evaluate-capacity-requirements"></a>Como avaliar os requisitos de capacidade

No Azure Pesquisa Cognitiva, a capacidade é estruturada como *réplicas* e *partições*.

+ Réplicas são instâncias do serviço de pesquisa. Cada réplica hospeda uma cópia com balanceamento de carga de um índice. Por exemplo, um serviço com seis réplicas tem seis cópias de cada índice carregado no serviço.

+ As partições armazenam índices e dividem automaticamente os dados pesquisáveis. Duas partições dividem o índice na metade, três partições as dividem em terços e assim por diante. Em termos de capacidade, o *tamanho da partição* é o principal recurso de diferenciação entre camadas.

> [!NOTE]
> Todas as camadas padrão e de armazenamento otimizadas dão suporte a [combinações flexíveis de réplicas e partições](search-capacity-planning.md#chart) para que você possa [otimizar seu sistema para velocidade ou armazenamento](search-performance-optimization.md) alterando o equilíbrio. A camada básica oferece até três réplicas para alta disponibilidade, mas tem apenas uma partição. As camadas gratuitas não fornecem recursos dedicados: os recursos de computação são compartilhados por vários assinantes.

### <a name="evaluating-capacity"></a>Avaliando a capacidade

A capacidade e os custos de executar o serviço são disponibilizados em mãos. As camadas impõem limites em dois níveis: armazenamento e recursos. Você deve pensar em ambos porque qualquer limite que atingir primeiro é o limite efetivo.

Os requisitos de negócios normalmente ditam o número de índices que você precisará. Por exemplo, você pode precisar de um índice global para um repositório de documentos grande. Ou talvez você precise de vários índices com base na região, no aplicativo ou no nicho de negócios.

Para determinar o tamanho de um índice, você precisa [compilar um](search-what-is-an-index.md). Seu tamanho será baseado em dados importados e na configuração de índice, como se você habilitar sugestores, filtragem e classificação.

Para pesquisa de texto completo, a estrutura de dados primária é uma estrutura de [índice invertida](https://en.wikipedia.org/wiki/Inverted_index) , que tem características diferentes dos dados de origem. Para um índice invertido, o tamanho e a complexidade são determinados pelo conteúdo, não necessariamente pela quantidade de dados que você feedu nele. Uma fonte de dados grande com alta redundância pode resultar em um índice menor do que um conjunto de dado menor que contém conteúdo altamente variável. Portanto, raramente é possível inferir o tamanho do índice com base no tamanho do conjunto de datas original.

> [!NOTE] 
> Mesmo que a estimativa de necessidades futuras de índices e armazenamento possa parecer uma tarefa de adivinhação, vale a pena fazer isso. Se a capacidade de uma camada for muito baixa, você precisará provisionar um novo serviço em uma camada mais alta e, em seguida, [recarregar os índices](search-howto-reindex.md). Não há nenhuma atualização in-loco de um serviço de uma camada para outra.
>

### <a name="estimate-with-the-free-tier"></a>Estimar com a camada gratuita

Uma abordagem para calcular a capacidade é iniciar com a camada Livre. Lembre-se de que o serviço gratuito oferece até três índices, 50 MB de armazenamento e 2 minutos de tempo de indexação. Pode ser desafiador estimar um tamanho de índice projetado com essas restrições, mas estas são as etapas:

+ [Crie um serviço gratuito](search-create-service-portal.md).
+ Prepare um DataSet pequeno e representativo.
+ [Crie um índice inicial no portal](search-get-started-portal.md) e anote seu tamanho. Os recursos e atributos têm um impacto no armazenamento. Por exemplo, a adição de sugestores (consultas de pesquisa conforme o tipo) aumentará os requisitos de armazenamento. Usando o mesmo conjunto de dados, você pode tentar criar várias versões de um índice, com atributos diferentes em cada campo, para ver como os requisitos de armazenamento variam. Para obter mais informações, consulte ["implicações de armazenamento" em criar um índice básico](search-what-is-an-index.md#index-size).

Com uma estimativa aproximada em mãos, você pode dobrar esse valor para o orçamento de dois índices (desenvolvimento e produção) e, em seguida, escolher a camada adequadamente.

### <a name="estimate-with-a-billable-tier"></a>Estimar com uma camada Faturável

Os recursos dedicados podem acomodar grandes períodos de amostragem e processamento para estimativas mais realistas de quantidade de índice, tamanho e volumes de consulta durante o desenvolvimento. Alguns clientes saltam diretamente com uma camada Faturável e, em seguida, reavaliam à medida que o projeto de desenvolvimento amadurece.

1. [Examine os limites de serviço em cada camada](./search-limits-quotas-capacity.md#index-limits) para determinar se as camadas inferiores podem dar suporte ao número de índices de que você precisa. Nas camadas básica, S1 e S2, os limites de índice são 15, 50 e 200, respectivamente. A camada de armazenamento otimizado tem um limite de 10 índices porque ele foi projetado para dar suporte a um número baixo de índices muito grandes.

1. [Criar um serviço em uma camada faturável](search-create-service-portal.md):

    + Comece de baixo, em básico ou S1, se você não tiver certeza sobre a carga projetada.
    + Comece de alto, em S2 ou mesmo S3, se você souber que vai ter carregamentos de consulta e indexação em larga escala.
    + Comece com o armazenamento otimizado, em L1 ou L2, se você estiver indexando uma grande quantidade de dados e a carga de consulta for relativamente baixa, assim como com um aplicativo de negócios interno.

1. [Criar um índice inicial](search-what-is-an-index.md) para determinar como a fonte de dados traduz para um índice. Essa é a única maneira de estimar o tamanho do índice.

1. [Monitorar armazenamento, limites de serviço, volume de consulta e latência](search-monitor-usage.md) no portal. O portal mostra as consultas por segundo, as consultas limitadas e a latência de pesquisa. Todos esses valores podem ajudá-lo a decidir se selecionou a camada certa. 

O número de índice e o tamanho são igualmente importantes para sua análise. Isso ocorre porque os limites máximos são atingidos por meio da utilização total de armazenamento (partições) ou por limites máximos de recursos (índices, indexadores e assim por diante), o que vier primeiro. O portal ajuda a manter o controle de ambos, mostrando o uso atual e os limites máximos lado a lado na página Visão geral.

> [!NOTE]
> Os requisitos de armazenamento podem ser inalterados se os documentos contiverem dados estranhos. O ideal é que os documentos contenham apenas os dados de que você precisa para a experiência de pesquisa. Os dados binários não são pesquisáveis e devem ser armazenados separadamente (talvez em um armazenamento de BLOBs ou de tabelas do Azure). Em seguida, um campo deve ser adicionado no índice para manter uma referência de URL aos dados externos. O tamanho máximo de um documento individual é de 16 MB (ou menos se você estiver carregando vários documentos em massa em uma solicitação). Para obter mais informações, consulte [limites de serviço no Azure pesquisa cognitiva](search-limits-quotas-capacity.md).
>

**Considerações sobre volume de consultas**

Consultas por segundo (QPS) é uma métrica importante durante o ajuste de desempenho, mas geralmente é apenas uma consideração de camada se você espera um alto volume de consulta no início.

As camadas padrão podem fornecer um equilíbrio entre réplicas e partições. Você pode aumentar o retorno da consulta adicionando réplicas para balanceamento de carga ou adicionar partições para processamento paralelo. Em seguida, você pode ajustar o desempenho depois que o serviço for provisionado.

Se você espera grandes volumes de consulta sustentados desde o início, considere as camadas mais altas Standard, apoiadas por hardware mais potente. Você pode pegar partições e réplicas offline ou até mesmo alternar para um serviço de camada inferior, se esses volumes de consulta não ocorrerem. Para obter mais informações sobre como calcular a taxa de transferência de consulta, consulte [desempenho e otimização do Azure pesquisa cognitiva](search-performance-optimization.md).

As camadas de armazenamento otimizadas são úteis para cargas de trabalho de dados grandes, dando suporte a armazenamento de índice mais geral disponível para quando os requisitos de latência de consulta são menos importantes. Você ainda deve usar réplicas adicionais para balanceamento de carga e partições adicionais para processamento paralelo. Em seguida, você pode ajustar o desempenho depois que o serviço for provisionado.

**Contratos de nível de serviço**

Os recursos de camada gratuita e visualização não fornecem [contratos de nível de serviço (SLAs)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Para todas as camadas faturáveis, os SLAs entram em vigor quando você provisiona redundância suficiente para o serviço. Você precisa ter duas ou mais réplicas para SLAs de consulta (leitura). Você precisa ter três ou mais réplicas para os SLAs de consulta e indexação (leitura/gravação). O número de partições não afeta os SLAs.

## <a name="tips-for-tier-evaluation"></a>Dicas para avaliação de camada

+ Permitir que as métricas sejam compiladas em consultas e coletar dados sobre padrões de uso (consultas durante o horário comercial, indexação fora do horário de pico). Use esses dados para informar as decisões de provisionamento do serviço. Embora não seja prático em uma cadência de hora ou diária, você pode ajustar dinamicamente as partições e os recursos para acomodar as alterações planejadas nos volumes de consulta. Você também pode acomodar alterações não planejadas, mas sustentadas, se os níveis tiverem tempo suficiente para garantir a tomada de ações.

+ Lembre-se de que a única desvantagem do no provisionamento é que você pode precisar subdividir um serviço se os requisitos reais forem maiores do que suas previsões. Para evitar a interrupção do serviço, você deve criar um novo serviço em uma camada superior e executá-lo lado a lado até que todos os aplicativos e solicitações tenham como destino o novo ponto de extremidade.

## <a name="next-steps"></a>Próximas etapas

Comece com uma camada gratuita e crie um índice inicial usando um subconjunto de seus dados para entender suas características. A estrutura de dados no Azure Pesquisa Cognitiva é uma estrutura de índice invertida. O tamanho e a complexidade de um índice invertido são determinados pelo conteúdo. Lembre-se de que o conteúdo altamente redundante tende a resultar em um índice menor que o conteúdo altamente irregular. Portanto, as características de conteúdo, em vez do tamanho do conjunto de os, determinam os requisitos de armazenamento de índice

Depois de ter uma estimativa inicial do tamanho do índice, [provisione um serviço Faturável](search-create-service-portal.md) em uma das camadas discutidas neste artigo: básico, Standard ou armazenamento otimizado. Relaxe quaisquer restrições artificiais no dimensionamento de dados e [recompile o índice](search-howto-reindex.md) para incluir todos os dados que você deseja que sejam pesquisáveis.

[Alocar partições e réplicas](search-capacity-planning.md) conforme necessário para obter o desempenho e escalabilidade que você precisa.

Se o desempenho e a capacidade estiverem corretos, você estará pronto. Caso contrário, crie novamente um serviço de pesquisa em uma outra camada mais estreitamente alinhada a suas necessidades.

> [!NOTE]
> Se você tiver dúvidas, poste no [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) ou [entre em contato com o suporte do Azure](https://azure.microsoft.com/support/options/).