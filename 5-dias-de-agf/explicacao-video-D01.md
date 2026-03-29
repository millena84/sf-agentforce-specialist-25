# D01: Introdução IA e Agentforce

## Introdução do vídeo

### Assuntos

Os assuntos discutidos aqui serão: Conceitos de IA, Visão geral Agentforce e Arquitetura.
O conteúdo está dividido em:

- Porque IA
- Porque Agentes
- Conceitos de IA
- Porque Agentforce
- Visão geral arquitetura Agentforce

Eventuais conteúdos adicionais serão gravados em vídeos apartados.

### Porque IA

#### Mensagem principal

A escolha de IA para um desenho de solução não pode ter relação somente com o hype. Antes de pensar em IA, eu preciso entender se ela realmente é o melhor caminho para o problema que tenho na mão.

As capacidades da IA podem ajudar muito em pontos como:

- Aumento de eficiencia
  - na automação de tarefas repetitivas e muito estruturadas
  - em atividades recorrentes onde o processo se repete e o que muda são os dados
  - em tarefas em que normalmente entramos no "modo automatico"
- Qualidade na tomada de decisão
  - permite escalar a analise de grandes volumes e variedades de dados
  - tornando essa analise mais rapida e mais eficiente
- Personalização
  - criando conteúdos, conversas e mensagens adaptadas ao contexto
  - analisando dados e gerando feedback mais direcionado

>Mas será que só saber essas vantagens já é suficiente?

##### Porque IA checklist

>No material escrito temos um checklist com uma lista de perguntas-chave para nos ajudar a entender se é viavel e/ou recomendavel usar IA.

Elas estão divididas em 5 grupos. Em linhas gerais, eu preciso saber:

- qual é o problema que eu tenho que resolver;
- o que eu sei sobre os dados relacionados a esse problema;
- como esse problema pode ser resolvido, seja com regra, automação, agente ou prompt;
- qual é a sensibilidade e a governança necessária para aqueles dados;
- e, por fim, qual é a relação entre custo-benefício e urgência.

Dependendo desse retorno, eventualmente pode ser necessário reavaliar o direcionamento da solução e pensar em outras possibilidades.

>E estamos aqui falando de IA e de agentes quase como se fossem sinonimos, e isso acontece porque os agentes estao muito em alta.

### Porque Agentes

Mas afinal: em que tipo de situação um agente costuma trazer mais ganho?

- Em situações em que eu preciso analisar muitos dados
- Em situações em que existe um raciocinio estruturado, com etapas que eu consigo imaginar
- Em tarefas multietapas nas quais eu preciso combinar contexto, decisão e execução

E qual é o ganho prático disso?

- Escalabilidade de execução, porque um agente consegue buscar, cruzar e organizar informações muito mais rapido do que um analista faria manualmente em muitos documentos e sistemas

O que eu preciso para poder pensar em usar um agente?

- De um ecossistema conectado, com acesso aos dados e sistemas que esse agente vai precisar consultar ou acionar

E do que eu não posso esquecer?

- De manter a execução do agente e os dados usados por ele seguros, para não comprometer a confiabilidade da solução nem a imagem da empresa

>Independente de como voce imaginar a solucao, se voce esta falando de IA, existem conceitos importantes que precisam ficar no radar.

### Conceitos de IA

#### Slide principal

Esse slide é mais denso porque ele concentra vários termos que aparecem com frequência nas discussões de IA.

Eu tenho uma caixinha principal, que é a de RAG, porque essa é uma sigla que aparece muito quando falamos de IA generativa aplicada a dados de negócio.

- **Mas o que é, afinal, o RAG? Ele é o raciocinio do agente?**
    - Cuidado com esse entendimento simplificado, porque ele pode atrapalhar quando formos configurar ou debugar soluções
    - RAG significa geração aumentada por recuperação
    - Em termos simples, é uma forma de fazer o modelo responder usando dados externos ao treinamento dele
    - Ou seja: em vez de responder só com base no que o modelo aprendeu no treinamento, eu trago dados confiáveis da minha base e envio esses dados junto da solicitação

- **E de onde vêm esses dados externos?**
    - CLIQUE: Eles vêm de uma etapa anterior, que é a etapa de ingestão e preparação desses dados
    - Essa etapa não é o RAG em si, mas sem ela eu não tenho como fazer RAG
    - É nessa fase que eu seleciono as fontes de dados que quero usar
    - Depois eu fragmento esse conteúdo em pedaços menores: isso é o **chunking**
        - o **chunk** é cada pedaço da informação que poderá ser recuperado depois
    - Depois disso, cada chunk passa por um processo de **embedding**
        - ou seja, ele ganha uma representação numérica baseada no seu significado semântico
    - E então acontece o **indexing**
        - **Indexing** é a organização desses chunks e embeddings em uma estrutura preparada para busca
        - essa estrutura permite encontrar rapidamente os trechos mais parecidos com a solicitação feita

- CLIQUE: **Então vamos imaginar o seguinte: eu quero que meu agente me responda quais documentos preciso para solicitar a cidadania italiana sendo uma cidadã peruana. Como esse RAG acontece?**
    - CLIQUE: dessa solicitação deriva uma query
    - essa query também é transformada em embedding, para que a busca consiga comparar a pergunta com os chunks da base
    - o sistema faz o **retrieval**, ou seja, recupera os chunks mais relevantes
        - **Retrieval** é justamente essa etapa de recuperação de dados
    - CLIQUE: depois esses dados seguem para o **grounding**
        - “Grounding” é quando eu ancoro a resposta do modelo nos dados recuperados
        - em outras palavras: eu incluo esses trechos no contexto da solicitação para que a resposta venha baseada neles
    - CLIQUE: então eu empacoto
        - a query
        - os chunks recuperados
        - as instruções da tarefa
    - esse pacote é medido em **tokens**
        - token é a unidade de medida que usamos para contabilizar o tamanho do texto processado pelo modelo
        - aqui estamos medindo pergunta, contexto, instruções e depois também a resposta
        - tudo isso precisa caber dentro da janela de contexto do modelo
    - CLIQUE: e só então esse pacote é enviado ao LLM
        - o modelo processa a solicitação considerando esse contexto
        - e gera a resposta

>Vamos voltar em chunk e token porque esses conceitos costumam gerar bastante confusão.

#### EXTRA: CHUNK/TOKEN

Recapitulando de forma mais direta:

- Chunking = fragmentação dos dados
- Chunk = cada fragmento gerado dessa base
- Depois do chunk, eu gero o embedding e organizo isso na etapa de indexing

Eu tenho algumas estratégias de chunking:

- TAMANHO FIXO: quando corto por uma quantidade definida de tokens ou caracteres
- OVERLAP: quando repito um pedaço do final do chunk anterior no início do próximo para não perder contexto
- ESTRUTURADO: quando corto por seções, títulos ou blocos do documento
- SEMANTICO: quando a quebra tenta respeitar a mudança natural de assunto

Na hora de subir a base de conhecimento no Data Cloud, esse processo vai aparecer porque ele impacta diretamente a qualidade da busca.

Resumindo:

CHUNK: FRAGMENTO DE TEXTO
CHUNKING: QUEBRAR A BASE DE CONHECIMENTO EM FRAGMENTOS
TOKEN: UNIDADE DE MEDIDA PARA CONTROLAR O TAMANHO DO QUE O MODELO VAI PROCESSAR

Um chunk é ~~composto~~ medido por tokens, que não medem só os chunks: eles medem o pacote inteiro da solicitação.

Na indexação, cada chunk gera um embedding.
Esse embedding funciona como uma representação vetorial daquele trecho para permitir busca por similaridade.

Essas informações serão usadas nos tipos de busca:

- palavra-chave, quando eu quero correspondência mais literal
- vetorial/semântica, quando eu quero proximidade de significado
- híbrida, quando eu combino as duas abordagens

Por isso o chunk precisa ser bem configurado:

- se ele for grande demais, mistura assunto demais e piora a busca
- se ele for pequeno demais, perde contexto e a resposta pode sair ruim

É por isso que esse assunto aparece tanto em debug e configuração de base de conhecimento no Agentforce.

Token: unidade de medida usada para controlar o tamanho do pacote processado pelo LLM

A janela de contexto é o limite total de tokens em uma chamada — e esse espaço é compartilhado entre a pergunta do usuário, os chunks recuperados pelo retriever e as instruções do agente. Por isso, chunks grandes demais podem 'encher' a janela e deixar menos espaço para instrução ou resposta.

>Independente de como voce imaginar a solucao, se voce esta falando de IA, esses conceitos precisam continuar no radar.

### Porque Agentforce?

#### PORQUE AGENTFORCE?

Beleza, agora eu começo a falar de fato do Agentforce.

A pergunta aqui é: por que falar de Agentforce quando pensamos em agentes dentro de organizações Salesforce?

Porque, nesse cenário, eu já tenho uma série de facilidades prontas no ecossistema:

- possibilidade de criar agentes low-code ou no-code
- integração com dados e sistemas do ecossistema Salesforce
- possibilidade de conectar informações de outras orgs e sistemas externos

Ou seja, se eu já trabalho nesse ecossistema, o Agentforce reduz parte do esforço de construção da solução.

##### ECOSSISTEMA

Mas o principal diferencial é que o ecossistema já traz componentes importantes para soluções de IA com mais segurança e governança.

No Agentforce eu já tenho uma peça responsável por guardrails essenciais, como:

- segurança de dados
- mascaramento
- proteção contra prompt injection
- detecção de toxicidade

E tudo isso aparece de forma configurável no setup, sem exigir que eu codifique tudo do zero.

Além disso, eu tenho várias possibilidades de agentes ligados ao ecossistema Salesforce, sempre em conjunto com Platform, Data Cloud e Einstein Trust Layer.

>Isso faz gancho com o slide de arquitetura.

### Visão geral Agentforce Arquitetura

Em linhas gerais, a arquitetura do Agentforce pode ser entendida como quatro grandes pilares.

O ecossistema do Agentforce foi batizado de Agentforce 360 Platform, e o Data Cloud passou a ser chamado também de Data 360.

No slide, eu tenho três pilares principais com linha sólida:

Data 360
Quando falo de Agentforce, o Data 360 é a principal camada de dados e memória. É dali que vêm os dados estruturados, não estruturados e o conhecimento que o agente usa para agir com contexto.

Atlas Reasoning Engine
É o cérebro do agente. Ele entende intenção, planeja passos, decide o que precisa ser feito, aciona automações e orquestra a execução.

Einstein Trust Layer
Trabalha em conjunto com o Atlas, protegendo dados, aplicando guardrails e fazendo a ponte segura entre o ecossistema Salesforce e os modelos.

E eu também tenho um pilar tracejado de LLMs, porque os modelos não ficam dentro do ecossistema Salesforce.

Por fim, os LLMs processam as solicitações enviadas pelo Atlas, sempre mediados por essa arquitetura.

### Visão geral Agentforce Funcionamento

Em um nível mais granular, a imagem mostra o seguinte fluxo:

- o usuário faz uma solicitação
- o agente entende a intenção
- identifica tópico, ação, instruções e contexto
- planeja a orquestração do que precisa ser feito
- consulta dados no Data Cloud quando necessário
- executa ações e devolve a resposta

Ou seja, não é só uma pergunta sendo enviada para um modelo. Existe uma orquestração entre intenção, dados, ações e segurança.

Quando falamos de “Ensemble RAG”, é porque o Agentforce pode fazer recuperação de dados usando várias fontes diferentes dentro do mesmo fluxo.

### Visão geral Agentforce (ponte com conceitos)

Nesse outro slide, eu volto ao fluxo com foco em Data Cloud para conectar o que vimos de conceito com o que aparece na prática.

Aqui entram:

- dados estruturados e não estruturados
- chunk, que é a fragmentação do conteúdo
- embedding, que é a representação vetorial desse conteúdo
- indexing, que organiza isso para que a busca seja eficiente

Também aparecem alguns termos mais específicos que eu não vou aprofundar agora, mas quero que vocês reconheçam:

- otimização de chunk, porque cada estratégia pode funcionar melhor em um tipo de base
- multi-representation indexing, que separa estruturas usadas para acelerar a busca
- modelos diferentes de embedding
- hierarchical indexing

O ponto importante aqui não é decorar cada termo agora. O ponto importante é entender que, para o Agentforce buscar bem, a base precisa ser preparada e organizada antes.

Então, quando essa imagem mostrar esses nomes, a leitura que eu quero que vocês façam é:

- primeiro eu preparo a base
- depois eu fragmento
- depois eu gero representações vetoriais
- depois eu indexo isso para busca
- e só então esse conteúdo fica pronto para ser recuperado no RAG

Quando entrarmos no detalhe de Data Cloud, eu volto nesses itens com mais calma.
