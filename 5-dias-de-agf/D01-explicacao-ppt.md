

## D01: Introdução IA e Agentforce

###  Introdução do vídeo

#### Assuntos

Os assuntos discutidos aqui serão: Conceitos de IA, Visão geral Agentforce e Arquitetura;
O conteúdo está dividido em
- Porque IA
- Porque Agentes
- Conceitos de IA
- Porque Agentforce
- Visão geral arquitetura Agentforce
Eventuais conteúdos adicionais serão gravados em vídeos apartados.

#### Porque IA

##### Porque IA

A escolha de IA para um desenho de solução não pode ter relação somente com o hype. Temos que considerar que as capacidades da IA podem ajudar muito em pontos críticos como por exemplo

- Aumento de eficiencia
    - na automação de tarefas repetitivas e muito estruturadas
    - em atividades recorrentes onde o que muda são somente os dados
    - tarefa em que se entra no "modo automatico"
- Qualidade na tomada de decisão
    - permite escalar analise de grandes volumes e variedades de dados
        - tornando-as + rapidas e eficientes 
- Personalização
    - Criando conteúdos, conversas, mensagens adaptadas
    - analisando dados e dando o feedback realtime

>Mas será que só saber essas coisas pe suficiente?

##### Porque IA checklist

>No material escrito temos um checklist com uma lista de perguntas chave para nos ajudar a entender se é viavel e/ou recomendavel o uso de IA.

Elas estão divididas em 5 grupos: em linhas gerais , eu sei 
- qual é o problema que eu tenho que resolver, 
- o que eu sei sobre os dados relacionados a esse problema, 
- como o problema pode ser resolvido – utilizando regra, automação, agente, prompt –, 
- qual é a sensibilidade e a governança necessária para aqueles dados relacionados ao problema.
- Com base nas respostas anteriores, fazemos uma avaliação de custo-benefício versus urgência.

Dependendo de como for esse retorno, eventualmente pode ser necessário reavaliar algum direcionamento de solução e pensar em outras possibilidades.

>E estamos aqui falando de IA e de agentes como fosse sinonimos, e isso acontece porque os agentes estão em alta

#### Porque Agentes

Mas so que agentes sao bons? No que eu teria maior ganho com eles?

- Em situações que eu analise muitos dados,
- Que tenho um raciocinio estruturado, onde eu consigo imaginar quais os passos eu tenho que seguir
- Em tarefas multietapas que eu não precise de supervisão

E no que a ajuda com tarefas com essas caracteristicas pode ajudar?

- A aumentar a eficiencia porque eu ganho escalabilidade, afinal é muito mais rapido um agente buscar dados em 10 documentos do que um analista.

O que eu preciso para poder pensar em usar um agente?

- De um ecossistema conectado (nao necessariamente, no caso do agentforce, na mesma org, mas tenho que ter maneiras de conectar os dados e outros que sejam necessarios para o agente)

E do que eu nao posso esquecer?

- De manter a execução do agente e seus dados seguros para não afetar a confiabilidade da minha solução e impactar a imagem da minha empresa.

>Independente de como vc quiser imaginar a solução, se você está falando de IA, existem uma série de conceitos importantes (uns mais academicos) importantes de se conhecer e NUNCA perder do radar

#### Conceitos de IA

##### Conceitos de IA

Esse slide é um pouco mais denso, porque tem bastante informação relacionada.

Eu tenho uma caixinha principal, que é a de RAG, que é uma sigla que tem aparecido muito em discussões relacionadas à inteligência artificial.

- **Mas porque isso tem acontecido? O que é, afinal, o RAG? Ele é o raciocinio?**
    - CUIDADO com esses entendimentos simplificados, eles podem confundir em configurações mais especificas ou em debugs e soluções de problemas
    - Significado de RAG: geração aumentada por recuperação, é uma estratégia de recuperação de dados que faz o LLM gerar a saída se baseando en dados personalizados, dados esses usados para contextualizar a resposta
    - Se eu tivesse que definir o RAG, eu vou dizer que ele uma arquitetura de interação com modelos de IA generativa que permite personalização / contextualização da resposta da interação com o modelo usando conhecimento externo ao modelo.
    - Pode parecer complexo, mas quando formos falando dos demais conceitos eu imagino que vai ficar mais claro.

- **Eu mencionei aqui o uso de conhecimento externo ao modelo, certo? Isso porque existe uma fase que está diretamente relacionada, mas NÃO ACONTECE NO RAG.**
    - CLIQUE: que é a INGESTÃO de dados personalizados para serem usados no RAG.
    - É a etapa responsável pela memória e contexto do rag. Sem essa etapa, nao tem rag.
    - Nessa etapa eu seleciono quais dados eu quero
    - É feita a fragmentação desse dado, que vocês podem ouvir como chunkenização / **chunking**
        - o **chunk** é um pedaço da informação da sua base de conhecimento que serão recuperados quando necessário
    - Após a geração do chunk, acontece o **embeeding**, ou seja, 
        - de acordo com o significado 'semantico' daquele chunk, ele ganha um vetor numerico
        - isso é armazenado com metadados (informações do chunk)
    - E finalizado isso, é feito o **indexing**
        - **Indexing** é organizar esse dado, esse chunk que você acabou de fragmentar, com base na sua base de conhecimento.
        - vetor / metadados / chunk é armazenado em um banco de dados do tipo **vector db**
        - É um banco otimizado para guardar vetores e encontrar os mais parecidos com um vetor de consulta
        - a estrutura desse banco permite fazer a busca por similaridade usando a proximidade matemática entre vetores
- CLIQUE: **Então vamos agora imaginar o seguinte: Eu quero que meu agente me responda quais documentos eu preciso para solicitar a cidadania italiana sendo uma cidadã peruana. Como vai acontecer o RAG?**
    - CLIQUE: Dessa solicitação vai derivar uma query, que passa **pelo modelo de embbeding para ser convertida em vetores**, que são usados na busca, mas quem retorna é o chunk e o metadado
    - Que são usados no retrieval do dado, usando para buscar os chunks com maior similaridade (numeros vetoriais mais proximos)
        - Ou seja, **Retrieval** É a recuperação de dados: você utiliza conectores chamados de retrievers para usar esses dados recuperados dentro do seu RAG.
    - CLIQUE: Os dados com nova mais alta ficam juntos para serem enviados para **GROUNDING** da pergunta,
        - “Grounding”, que significa você basear a sua resposta: você inclui os seus dados na solicitação para a LLM para que a resposta da tarefa considere esses dados na hora de hipercontextualizar a resposta.
        - que é fazer com que a minha tarefa tenha a sua resposta gerada (pelo modelo) BASEADA/ANCORADA nos dados do retrieval
    - CLIQUE: então são 'empacotados'
        - query
        - chunks recuperados
        - instruções da tarefa
    - O pacote é medido (são contabilizados os tokens do pacote)
        - Token é um fragmento de texto utilizado pela LLM; cada modelo tem o seu limite, que é a chamada janela de contexto.
        - Token: unidade de medida usada para contabilizar o tamanho dos chunks
            - medir o tamanho do texto (pergunta + chunks + instruções), não só dos chunks
        - verifica se o total de tokens do pacote esta dentro da janela de contexto (que é o limite de tokens que os modelos podem processar)
    - CLIQUE: e são enviados ao LLM
        - Reasoning: planejamento de como trabalhar as coisas do pacote
        - Gera a resposta

>Vamos voltar em chunk e token dada a complexidade dos conceitos...

##### EXTRA: CHUNK/TOKEN

Recapitulando porque falamos muitas coisas:
- Chunking = fragmentação de dado
- Chunk = fragmento
- Quando eu faço o Chunk, eu gero o embed, e depois o vector (no indexing)

Eu tenho várias estratégias de chunk:

<!-- 
1. **Chunking por tamanho fixo** (tokens ou caracteres): Tamanho típico entre 128–512 tokens, ajustado ao contexto do modelo
2. **Chunk overlap / sliding window:**  Repetir 10–20% do final de um chunk no início do próximo para não cortar uma ideia no meio (ex.: 300 tokens com 30–60 de overlap), isso o ajuda a não se perder
    - Sem overlap: - Chunk 1 termina no meio de uma explicação. / - Chunk 2 começa já no meio da frase seguinte. /  - Cada chunk, sozinho, perde parte do contexto. / 
    - Com overlap (ex.: 300 tokens com 50 de overlap): - Os últimos 50 tokens do chunk 1 aparecem também no início do chunk 2 /  - Se o retriever trouxer o chunk 2, ele ainda carrega um pedacinho do que vinha antes, ajudando o modelo a entender a continuação /  Preserva o contexto entre chunks vizinhos.
3. **Chunking estruturado:** - Cortar por seções, headings, parágrafos, bullets, mantendo unidades “semânticas” inteiras (legal, políticas, docs técnicos). Ou seja, você respeita a estrutura do documento. Algumas ferramentas já sugerem isso, porém se houver muito conteúdo dentro das seções e etc, corre o risco de se perder.
    - Benefício: Cada chunk tende a ser uma “unidade de sentido” (por exemplo, uma cláusula de contrato, uma seção de documentação).
    - Se uma seção é enorme (ex.: política inteira em um único H2 de 2–3 páginas), esse chunk fica gigante, logo, cai no mesmo problema dos chunks grandes: embedding genérico, muito ruído e estouro de tokens. Um ponto de atenção aqui é limitar a quantidade de conteúdo dessas seções e etc.
4. **Semantic / LLM‑based chunking:** 
    - Usar embeddings ou o próprio LLM para encontrar pontos naturais de mudança de tópico e fazer splits “onde o assunto muda”
    - Traduzindo: em vez de cortar “na força bruta” (tamanho fixo) ou só em títulos, você tenta cortar “onde o assunto muda de verdade" (o chunk é feito por similaridade!)
        - Com embeddings (mais comum):
            - Gera embedding para cada sentença ou parágrafo.
            - Calcula a similaridade entre parágrafos vizinhos (cosine, dot etc.).
            - Enquanto a similaridade se mantém alta, você continua agregando no mesmo chunk.
            - Quando a similaridade cai abaixo de um limiar (assunto mudou), você corta e começa um novo chunk.
    - Com o próprio LLM:
        - Você passa um texto grande e pede para o modelo: “Separe esse documento em blocos coerentes, cada um tratando de um subtema, com no máximo X tokens cada.” Aí o modelo decide, com base em semântica, onde cortar.
        - É mais caro (chamadas ao LLM), mas bem flexível.
    não existia no agentforce ate março 2026

-->
Vamos falar um pouco mais de chunking e tokens, porque esse conhecimento é importante para fazer corretamente os debugs dentro do Agentforce.

Eu tenho algumas técnicas de chunking:

- TAMANHO FIXO: Por tamanho fixo, que pode ser de 128 ou 512 tokens.
- OVERLAP: Overlap, que é quando eu repito o final do chunk anterior no próximo, para não perder contexto.
- ESTRUTURADO: Chunking estruturado, quando eu quero quebrar por título, por exemplo, mas posso ter o problema de ter muitos parágrafos sob o mesmo título.
- SEMANTICO: Chunking semântico, ou LLM-based chunking, em que a própria LLM escolhe como quebrar para formar cada chunk.

Na hora de subir a minha base de conhecimento no Data Cloud, isso será mencionado e mostrado em visão geral: em que momento o chunking acontece ao subir a base e indexá-la, associando cada pedaço de texto a um endereço de índice.

CHUNK: FRAGMENTO DE TEXTO || CHUNKING: QUEBRAR BASES DE CONHECIMENTO EM CHUNKS
TOKEN: UNIDADE DE MEDIDA PARA CONTROLAR O TAMANHO DOS FRAGMENTOS
PODE SER CHAMADO TAMBEM DE CRITERIO DE CORTE. QUANDO A BASE ULTRAPASSA O TAMANHO COM BASE NO LIMITE DE TOKENS, ELA É DIVIDIDA EM MAIS DE UM CHUNK.

1 CHUNK É COMPORTO POR TOKENS || 1 TOKEN É A UNIDADE DE MEDIDA QUE LIMITA O CHUNK*

NA INDEXACAO, CADA CHUNK GERA UM EMBBEDING 
QUE EH O ENDEREÇO VETORIAL DO CHUNK
DC: 
CHUNK = TRECHO
INDEX = EMBBEDING

Essas informações serão utilizadas nos tipos de busca: palavra chave, vetorial/semântica ou híbrida.

Busca palavra-chave procura exatamente ou quase (equivale ao like do sql) igual, semântica busca por similaridade e híbrida combina as duas.

- ele é mais efetivo quando o chunk preserva a ideia e não perde o contexto, sendo grande o suficiente para preservar a ideia, mas pequeno o suficiente para ser bem ranqueado na busca por similaridade.
- O RAG pode ser atrapalhado quando tenho chunks muito grandes, com “de tudo um pouco”, tornando a similaridade difusa e piorando a resposta (o modelo pode alucinar), ou chunks muito pequenos, que não permitem um bom entendimento do que aquele pedaço trata.

Por isso é importante conhecer esses conceitos para subir e estruturar bases não estruturadas (como repositórios de documentos) de um jeito que considere esses pontos, para ser mais efetivo.

É muito importante conhecer esses conceitos para fazer debug do que está sendo desenvolvido no Agentforce, para você estar mais familiarizada com isso e, ao olhar o log de debug, não ficar perdida quando aparecerem várias representações numéricas, por exemplo.

Token: unidade de medida para controlar o tamanho dos chunks
Janela de contexto: contabiliza em tokens a quantidade de caracteres que podem ser processados pelo llm

>Independente de como vc quiser imaginar a solução, se você está falando de IA, existem uma série de conceitos importantes (uns mais academicos) importantes de se conhecer e NUNCA perder do radar

#### Porque Agentforce?

##### PORQUE AGENTFORCE?

Beleza, agora eu vou começar a falar de fato do Agentforce.
A ideia é: por que falar de Agentforce?

Quando falamos de agentes em organizações Salesforce, temos uma série de facilidades: já estamos em uma organização Salesforce, supondo que você também já atue com organizações Salesforce.
Há alguns diferenciais: é possível criar agentes low-code/no-code, sem precisar criar programas ou até automações complexas.

Eu tenho facilidades quando falo de Agentforce e de agentes que serão utilizados por pessoas em organizações Salesforce.
Se eu tenho um ecossistema conectado, tenho possibilidade de integrar com sistemas externos, conectar informações de orgs diferentes da minha organização, etc.

##### ECOSSISTEMA

Mas, principalmente, o mais interessante de usar Agentforce é saber que no ecossistema eu já tenho uma peça responsável por guardrails essenciais em soluções de IA, principalmente de agentes e prompts.
No ecossistema do Agentforce, eu já tenho uma peça que me traz garantias de segurança de dados, mascaramento, proteção contra prompt injection, detecção de toxicidade, tudo configurado em tela no setup, sem precisar codificar nada.

Eu tenho N possibilidades de agentes especificos para os produtos do ecossistema sf, todos eles intimamente conectados com os o salesforce platform, produtos, com o data cloud e com o einstein trust layer.

>Isso faz gancho com o slide de arquitetura.

#### Visão geral Agentforce Arquitetura

Em linhas gerais, a arquitetura do Agentforce pode ser definida como quatro grandes pilares.

O ecossistema do Agentforce foi batizado de Agentforce 360 Platform, e o Data Cloud, no último Dreamforce, foi rebatizado como Data 360.

No slide, eu tenho três pilares com linha sólida:

Data 360
Quando falo de Agentforce, tenho como camada principal o Data 360, que é a memória do agente, o arquivo de memórias, tudo o que ele conhece.
É a memória usada pelo Atlas Reasoning Engine, que é o cérebro do agente: entende o que será realizado, a intenção, planeja passos e automações, orquestra a execução, é o raciocínio lógico do agente.

Atlas Reasoning Engine
Ele define o que vai ser executado, em que ordem, e faz a execução em conjunto com o Einstein Trust Layer, que garante os guardrails mínimos necessários à construção de agentes na organização.

Einstein Trust Layer
Ele trabalha em conjunto com o Atlas, fazendo o papel de sistema nervoso (enviando informações do Atlas para as LLMs) e sistema imunológico (protegendo o que está sendo executado pelo agente).

E um pilar tracejado de LLMs, porque elas de fato não estão dentro do ecossistema.

Por fim, temos as LLMs, que ajudam a fazer a ponte do Atlas com os modelos que processam as solicitações.
Eles ficam fora do ecossistema Salesforce, mesmo os modelos próprios dos clientes, e são acionados via integração; são eles que processam as solicitações do Atlas.

#### Visão geral Agentforce Funcionamento

Em um nível mais granular, eu mostro uma imagem em que o usuário manda uma solicitação ao Atlas, que entende a intenção, lógica, regras, etc., faz buscas de dados no Data Cloud, entra em um ciclo até verificar que a resposta está de acordo com o solicitado e só então devolve ao usuário.

O usuário interage com o agente, que entende a intenção, identifica tópico, ação, instruções, planeja a orquestração das tarefas, verifica quais ações/automations estão relacionadas, e precisa de dados em memória para executar tudo isso.
Nesse momento, aciona o Data Cloud para buscas de dados, tanto de fontes estruturadas quanto não estruturadas, bases com chunks e embeddings (vector DB).

Quando falamos de “Ensemble RAG”, é porque faço o RAG usando várias fontes de dados: há inclusive, no Agentforce, um tipo chamado “Ensemble Retriever”, que permite criar um retriever com várias fontes de dados.

#### Visão geral Agentforce (ponte com conceitos)

Em outro slide, trago o mesmo fluxo com foco em Data Cloud, destacando:

Dados estruturados e não estruturados.

É aqui que eu faço
chunk: fragmentação

embed: emdereço

vector: vetorial

Indexing, porque o vetor sozinho não serve para nada: ele precisa estar indexado para que a busca seja eficiente.

Nessa fase eu tenho 
- a otimização de chunk: cada estrategia pode ser melhor em determinados cenaris
- multi-representation... a separação em duas tabelas:
    - vector
    - chunks
- modelos diferentes de embedding
- hierarquical indexing: 

Então, indo para o próximo item na tela, que é a parte de indexação, Eh... o que eu posso falar aqui aqui eu tenho vários conceitos também que vão aparecer algumas coisas eu não vou detalhar muito para não ficar confuso né é e eu tenho a otimização de blocos, certo? Novamente, lembrando que foi mostrado lá no conceito, quais são minhas estratégias de blocos? Eu tenho por caractere, que o limite é de até 512. Eu tenho por seção, que é por título, certo? Por título, independentemente de ser um h1, h2, h3, de qualquer forma, certo? Eu tenho a semântica, então ela respeitará uma passagem semântica, ou eu a tenho delimitada. É como o delimitado quando eu faço a quebra de linha, minha...
quebra "brutal", quando eu quebro e posso quebrar no meio de um parágrafo, o que é até mencionado lá no conceito. Então eu tenho diferentes estratégias para poder fazer o bloco da minha base de conhecimento. E mesmo antes de março, eu só podia escolher essa estratégia de fragmentação quando ia criar recuperadores com base em DMOs da nuvem de dados. Eu não conseguia fazer isso, por exemplo, para documentos da Biblioteca de Dados ou quando criava uma biblioteca de dados de conhecimento de artigos; eu não tinha essa possibilidade.
Vou verificar se algo mudou agora com esta foto do Agents Force de março, mas entraremos em detalhes quando falarmos sobre isso, certo? Então, não vou dar detalhes agora. Indexação de múltiplas representações. Texto, certo? Associado a esse bloco que está aqui, desculpe, esse embedding que está aqui está no índice, certo? Aqui está representado pelo Sumari, certo? Então, essa conversão que ele faz e a multi-representação serão divididas aqui em duas tabelas para tentar acelerar essa busca nos bastidores, certo?
Quando estou fazendo uma busca semântica. Embedding especializado, certo? Então, tenho diferentes estratégias para poder fazer o embedding de dados quando estou fazendo o upload. Não vou detalhar isso aqui. Vou avaliar como trazer isso para cá quando estivermos falando sobre o Data Cloud com o AgentForce, que é onde isso aparece para configuração, para não confundir vocês ainda mais. Há muita informação aqui nesses cinco dias do AgentForce e acho que é complicado.
Agora que tenho um pouco mais de informação, preciso entender melhor antes de pensar em trazer isso para cá, certo? Eventualmente, trarei para lá apenas os links da ajuda da Salesforce e outros para aqueles que desejam entender um pouco mais em detalhes. Quando fiz a certificação, que foi na virada de 2025 para 2026, esse cara não apareceu, mas é interessante saber disso diariamente, não é?
Quando falo de uma pessoa que trabalha muito com o Engine Force, ela precisa ser muito especializada no assunto. E aqui, finalmente, nesta imagem recortada, temos a Indexação Hierárquica. Um cluster é um grupo de chunkings semelhantes com base em seus embeddings vetoriais. Ele cria clusters com os embeddings mais próximos para que, quando eu for fazer a busca pelo Hag end, eu possa encontrar um cluster que tenha todos os dados que eu preciso e encontrar um pouco mais rápido.
Então, é como se os blocos fossem divididos em clusters com base em uma similaridade semântica. Cada cluster representa um grupo de documentos semelhantes, certo? Relacionados, quando falo de similaridade, certo? Também nesse caso, em termos de questões semânticas. E esses clusters terão as representações, então eles vão falar aqui de uma forma, tentar explicar de uma maneira mais simples, como se fossem temas relacionados, sabe?
Então, é como se eu estivesse tentando deixar blocos, com embeddings de temas relacionados em um único cluster, para que eu possa ir diretamente para esse cluster quando for a hora de fazer a pesquisa. E essa pesquisa é a pesquisa Haptor, que é uma implementação específica de indexação hierárquica usada pela Salesforce. Mas não vou entrar em outros detalhes sobre isso agora. e se ficar alguma dúvida sinalize nos comentários ou me com me comuniquem para eu tentar trazer de uma maneira talvez um pouco mais claro porque de fato isso aqui é bem bem específico e aí eu tô eu já trazendo algumas coisas do agente forte sem nem tá tanto assim no agente forte né e e mas para poder fazer a ponte aqui com os conceitos mas quando a gente para entrar no detalhe disso aqui né o que for realmente detalhado vou fazer novamente a ponte com Poder recordar, né?
