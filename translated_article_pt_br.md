# An Analysis of Annotated Corpora for Emotion Classification in Text

## Resumo

> Vários conjuntos de dados foram anotados e publicados para classificação de emoções. Eles diferem de várias maneiras: (1) o uso de diferentes esquemas de anotação (por exemplo, conjuntos de rótulos discretos, incluindo alegria, raiva, medo ou tristeza ou valores contínuos, incluindo valência ou excitação), (2) o domínio, e, ( 3) os formatos de arquivo. Isso leva a várias lacunas de pesquisa: os modelos supervisionados geralmente usam apenas um conjunto limitado de recursos disponíveis. Além disso, nenhum trabalho anterior comparou corpora de emoções de maneira sistemática. Pretendemos contribuir para esta situação com um levantamento dos conjuntos de dados e agregá-los em um formato de arquivo comum com um esquema de anotação comum. Com base nessa agregação, realizamos os primeiros experimentos de classificação entre corpus no espírito das pesquisas futuras possibilitadas por este artigo, a fim de obter uma visão e um melhor entendimento das diferenças dos modelos inferidos a partir dos dados. Este trabalho também simplifica a escolha dos recursos mais adequados para o desenvolvimento de um modelo para um novo domínio. Um resultado de nossa análise é que um subconjunto de corpora é melhor classificado com modelos treinados em um corpus diferente. Para nenhum dos corpora, treinar em todos os dados é melhor do que usar uma subseleção dos recursos. Nosso corpus unificado está disponível em http://www.ims.uni-stuttgart.de/data/unifyemotion.

## 1 Introdução

A detecção e classificação de emoções no texto concentra-se no mapeamento de palavras, frases e documentos para um conjunto de emoções seguindo um modelo psicológico como os propostos por Ekman (1992), Plutchik (1980) ou Russell (1980), entre outros. A tarefa emergiu de um tópico puramente orientado para pesquisa para desempenhar um papel em uma variedade de aplicações, que incluem sistemas de diálogo (chatbots, sistemas de tutoria), agentes inteligentes, diagnósticos clínicos de transtornos mentais (Calvo et al., 2017) ou social mineração de mídia.

Como a variedade de aplicativos é grande, o conjunto de domínios e diferenças no texto é grande. Um dos primeiros trabalhos, motivado pelo objetivo de desenvolver um contador de histórias empático para histórias infantis, é a criação de corpus e modelagem de emoções em contos de Alm et al. (2005). Posteriormente, a ideia foi transferida para a Web, nomeadamente blogs (Aman e Szpakowicz, 2007) e microblogs (Schuff et al., 2017; Mohammad, 2012; Wang et al., 2012). Um domínio diferente em consideração são os artigos de notícias: Strapparava e Mihalcea (2007) enfocam as emoções nas manchetes. Pode-se duvidar que as emoções sejam expressas de maneira comparável nesses diferentes domínios: idealmente, os jornalistas tendem a ser objetivos ao escrever artigos, os autores de postagens de microblog precisam se concentrar na brevidade e pode-se supor que as expressões de emoção em contos são mais sutis e implícito do que, por exemplo, em blogs. Portanto, a transferência entre os modelos de reconhecimento de emoção é, presumivelmente, desafiadora. A alternativa mais direta é, no entanto, construir recursos do zero, um processo caro. Dada esta situação, ainda não está claro, dado um novo domínio para o qual um sistema de reconhecimento de emoções deve ser desenvolvido, como começar. Ao lado das questões metodológicas que acabamos de discutir, outro desafio é adquirir e comparar os corpora disponíveis.

Com este artigo, pretendemos contribuir para todos esses desafios. Apoiamos pesquisas futuras comparando conjuntos de dados existentes, explorando como eles se complementam e mapeando-os em um formato comum de modo que a detecção de emoções futuras possa se beneficiar do desenvolvimento em diferentes domínios e esquemas de anotação. Além disso, realizamos experimentos entre corpus treinando classificadores em cada conjunto de dados e avaliando-os em outros. Um desafio aqui é que os corpora são de domínios diferentes e são anotados de acordo com diferentes diretrizes e esquemas. Nosso objetivo é ajudar a tomar decisões que apoiem o melhor desenvolvimento do modelo para um domínio futuro, selecionando corpora adequados. Os parâmetros a serem levados em consideração são a fonte do texto (por exemplo, blogs, notícias, mídia social), o esquema de anotação (por exemplo, Plutchik, Ekman, subconjuntos deles) e o procedimento de anotação (por exemplo, crowdsourcing, autorrelato, rotulagem à distância baseada em especialistas). Nossas principais contribuições são, portanto, (1) para descrever os recursos existentes, (2) para avaliar quais classificadores de última geração funcionam bem quando treinados em um conjunto de dados e aplicados em outro, (3) para avaliar quais conjuntos de dados generalizam melhor para outro domínios, e (4) para comparar os conjuntos de dados qualitativa e quantitativamente. Para atingir nossos objetivos e como suporte adicional para pesquisas futuras, unificamos todos os conjuntos de dados disponíveis em um formato de arquivo comum. Fornecemos um script que baixa e converte os conjuntos de dados e instrui sobre como obter conjuntos de dados onde a licença não exige redistribuições. Nossos recursos estão disponíveis em http://www.ims.uni-stuttgart.de/data/unifyemotion. Nosso objetivo é manter o corpus unificado atualizado no futuro.

## 2 Antecedentes e trabalhos relacionados

A seguir, discutiremos as diferenças nos modelos psicológicos e nos esquemas de anotação (Seção 2.1), procedimentos de anotação (Seção 2.2), diferentes domínios e tópicos (Seção 2.3) e diferentes métodos de previsão (Seção 2.4). Uma visão geral dos recursos e trabalhos anteriores é apresentada na Tabela 1. Além disso, recomendamos as pesquisas de Munezero et al. (2014) e Santos e Maia (2018).

### 2.1 Modelos de emoção em psicologia e esquemas de anotação

Os modelos de emoção ainda são debatidos na psicologia (Barrett et al., 2018; Cowen e Keltner, 2018). Não contribuímos para esses debates, mas nos concentramos nas principais teorias da psicologia e do processamento da linguagem natural (PNL): conjuntos discretos e finitos de emoções (modelos categóricos) e combinações de diferentes dimensões contínuas (modelos dimensionais).

Os primeiros trabalhos sobre detecção de emoções (Alm et al., 2005; Strapparava e Mihalcea, 2007) focaram na conceituação das emoções seguindo o modelo de Ekman, que assume as seguintes seis emoções básicas: raiva, nojo, medo, alegria, tristeza e surpresa (Ekman, 1992 ) Suttles e Ide (2013), Meo e Sulis (2017) e Abdul-Mageed e Ungar (2017) seguem a Roda da Emoção (Plutchik, 1980; Plutchik, 2001) que também considera as emoções como um conjunto discreto de oito emoções básicas em quatro pares opostos: alegria-tristeza, raiva-medo, confiança-repulsa e antecipação-surpresa, junto com combinações de emoções.

Modelos dimensionais foram mais recentemente adotados em PNL (Preot¸iuc-Pietro et al., 2016; Buechel e Hahn, 2017a; Buechel e Hahn, 2017b): O modelo circunplexo (Russell e Mehrabian, 1977) coloca estados afetivos em um espaço vetorial de valência (correspondendo ao sentimento/polaridade), excitação (correspondendo a um grau de calma ou excitação) e dominância (grau percebido de controle sobre uma determinada situação). Qualquer emoção é uma combinação linear dessas.

### 2.2 Procedimentos de anotação

Uma maneira padrão de criar conjuntos de dados anotados é por meio de anotações de especialistas (Aman e Szpakowicz, 2007; Strapparava e Mihalcea, 2007; Ghazi et al., 2015; Li et al., 2017; Schuff et al., 2017; Li et al., 2017). No entanto, ter um especialista para fazer anotações em uma declaração significa que ele deve estimar o estado privado do autor. Portanto, os criadores do conjunto de dados ISEAR seguem uma rota diferente, mas semelhante, fazendo uso do auto-relato: os sujeitos são solicitados a descrever situações associadas a uma emoção específica (Scherer e Wallbott, 1994). Essa abordagem pode ser considerada uma anotação por especialistas por direito próprio.

Crowdsourcing, por exemplo, usando as plataformas Amazon’s Mechanical Turk1 ou CrowdFlower2, é outra maneira de adquirir julgamentos humanos. O crowdsourcing muitas vezes carece de controle de qualidade suficiente, mas alguns conjuntos de dados populares foram adquiridos com sucesso com esta abordagem, e. g., o conjunto de dados lançado pela Crowdflower para Cortana3 ou os conjuntos de dados construídos por Milnea et al. (2015) e Lapitan et al. (2016). Outro exemplo é o conjunto de dados de Mohammad (2012), que desenha dois questionários on-line detalhados e anota tweets por crowdsourcing.

Por fim, as redes sociais desempenham um papel central na aquisição de dados com supervisão distante (também chamada de autorrotulagem neste contexto), porque fornecem uma maneira rápida e barata de obter grandes quantidades de dados ruidosos anotados por escritores ou leitores (Mohammad e Kiritchenko, 2015; Abdul-Mageed e Ungar, 2017; De Choudhury et al., 2012; Liu et al., 2017). Por exemplo, no Twitter pode-se adicionar uma hashtag “#joy” a um tweet feliz ou no Facebook pode-se marcar posts pessoais com um “sentimento” e as pessoas podem mostrar uma “reação de surpresa” emocional. Neste último exemplo, dois níveis de anotação são fornecidos que são relevantes para a análise da emoção, ou seja, o estado emocional do leitor e do escritor. O acesso a essas informações é comparativamente simples nessas plataformas de redes sociais. Mais desafiador é adquirir esses dados para outros domínios. Buechel e Hahn (2017a) e Buechel e Hahn (2017b) procuram especificamente distinguir entre as expressões emocionais dos escritores e dos leitores.

Deve-se notar que algumas dessas abordagens existem em paralelo com pesquisas anteriores na avaliação dos estados emocionais das pessoas, apesar do fato de que existem instrumentos psicológicos padronizados (Bradley e Lang, 1994).

### 2.3 Domínios e tópicos

Trabalhos anteriores sobre detecção de emoções enfocam diferentes domínios e tópicos, e. g., descrições de eventos emocionais relatados pelo próprio (Scherer e Wallbott, 1994), notícias (Lei et al., 2014; Buechel et al., 2016), manchetes de notícias (Strapparava e Mihalcea, 2007), blogs (Aman e Szpakowicz, 2007), contos (Alm et al., 2005), postagens de microblog (ou seja, tweets) (Wang et al., 2012) para diferentes domínios, como saúde, política (Mohammad, 2012) e mercados de ações (Bollen et al., 2011).

Um exemplo precoce e uma das primeiras iniciativas de classificação de emoções é o trabalho de Aman e Szpakowicz (2007), que usam postagens de blog, amostradas sem levar em conta um tópico específico. Eles identificam a emoção, categoria, intensidade e palavras-chave e frases. Mishne e de Rijke (2005), Balog et al. (2006) e Nguyen et al. (2014) trabalha com dados do LiveJournal4 para desenvolver modelos preditivos de humores.

Da mesma forma, dados gerados por usuários nas mídias sociais têm sido objeto de pesquisa. Mohammad et al. (2015) e Mohammad e Bravo-Marquez (2017b) anotam tuítes eleitorais para sentimento, intensidade, papéis semânticos, estilo, propósito e emoções. De Choudhury et al. (2012) identificam mais de 200 humores frequentes no Twitter. Mohammad (2012), Mohammad et al. (2015), Wang et al. (2012), Volkova e Bachrach (2016) fazem uso de dados rotulados remotamente do Twitter. Recentemente, Liu et al. (2017) analisou o papel do contexto que fundamenta o sentimento nos tweets e investigou se o efeito do clima e dos eventos de notícias se relacionam com a emoção expressa em um determinado tweet. EmoNet é considerado o maior conjunto de dados construído de tweets (Abdul-Mageed e Ungar, 2017).

O Twitter costuma ser o assunto preferido de pesquisa, pois é fácil de usar e tem uma API bem documentada. No entanto, o Facebook também é usado, e. g., Preot¸iuc-Pietro et al. (2016) criam um conjunto de dados de postagens no Facebook e treinam modelos de previsão para valência e excitação. Pool e Nissim (2016) e Krebs et al. (2017) fazem uso do recurso de reação no Facebook para coletar dados rotulados para supervisão distante de um classificador. Uma abordagem diferente dentro do mesmo domínio foi usada por Polignano et al. (2017) que rotulou as postagens com emoticons mapeados para o modelo de Ekman.

Os dados nas redes sociais podem ser na forma de diálogos. Li et al. (2017) rotular manualmente um conjunto de dados de conversas. Wang et al. (2016) apresentam o EmotionPush, um sistema que transmite automaticamente a emoção do texto recebido em dispositivos móveis, implantado no aplicativo de mensagens do Facebook. Do mesmo domínio, mas em um tópico diferente, está o estudo da dinâmica dos estados emocionais dos pacientes expressa por suas postagens no Facebook (Lombardo et al., 2017).

Motivado pelo trabalho de estudiosos da literatura, está a criação de conjuntos de dados para estudar a emoção na literatura. Um dos primeiros conjuntos de dados é o corpus de contos de Alm et al. (2005). Kim et al. (2017) investigam a relação entre gêneros literários e emoções.

### 2.4 Métodos usados ​​na identificação de emoções

A classificação das emoções é comumente expressa como classificação de texto. Como classificação de texto em geral, o conjunto de métodos visto para classificação de emoção pode ser dividido em métodos baseados em regras e aprendizado de máquina, que discutiremos a seguir.

### 2.4.1 Algoritmos baseados em regras

A classificação de texto baseada em regras normalmente se baseia em recursos lexicais de palavras carregadas de emoção. Esses dicionários podem ser originados de crowdsourcing ou curadoria de especialistas. Os exemplos incluem WordNetAffect (Strapparava et al., 2004) e SentiWordNet (Esuli e Sebastiani, 2007), ambos originados de anotações de especialistas. Parcialmente construído sobre eles está o NRC Word-Emotion Association Lexicon (Mohammad et al., 2013), que usa as oito emoções básicas (Plutchik, 1980). Warriner et al. (2013) usam crowdsourcing para atribuir valores de valência, excitação e dominância (Russell, 1980).

Outra categoria relacionada de recursos lexicais que tem sido usada para análise de emoção é concretude e abstração (Koper et al., 2017). Brysbaert et al. (2014) publicam um léxico baseado em crowdsourcing, ¨ onde a tarefa era atribuir uma nota de 1 a 5 da concretude de 40.000 palavras. Da mesma forma, Koper ¨ e Schulte im Walde (2016) geram automaticamente normas afetivas de abstração, excitação, imageabilidade e valência para 350.000 lemas em alemão. Por fim, o Linguistic Inquiry and Word Count (LIWC) é um conjunto de 73 léxicos (Pennebaker et al., 2001), construído para reunir aspectos de significado lexical relativos a tarefas psicológicas. As abordagens baseadas em dicionário e regras são particularmente comuns no campo das humanidades digitais devido à sua transparência e uso direto.

### 2.4.2 Aprendizado de Máquina

Uma melhoria de desempenho em relação à consulta de dicionário foi observada com o aprendizado supervisionado baseado em recursos. Os recursos comuns incluem palavras n-gramas, caracteres n-gramas, embeddings de palavras, léxicos de afeto, negação, pontuação, emoticons ou hashtags (Mohammad, 2012). Essa representação de característica é então geralmente usada como entrada para alimentar classificadores como ingênuo Bayes, SVM (Mohammad, 2012), MaxEnt e outros para prever a categoria de emoção relevante (Aman e Szpakowicz, 2007; Alm et al., 2005). Da mesma forma que a mudança de paradigma na análise de sentimento, da modelagem baseada em recursos para o aprendizado profundo, os modelos de última geração para classificação de emoções são frequentemente baseados em redes neurais (Barnes et al., 2017). Schuff et al. (2017) aplicaram modelos das classes CNN, BiLSTM (Schuster e Paliwal, 1997) e LSTM (Hochreiter e Schmidhuber, 1997) e os compararam com classificadores lineares (SVM e MaxEnt), onde o BiLSTM apresenta melhores resultados com a maioria precisão e recall equilibrados. Abdul-Mageed e Ungar (2017) afirmam o F1 mais alto seguindo o modelo de emoção de Plutchik com redes de unidades recorrentes bloqueadas (Chung et al., 2015).

Uma abordagem para lidar com a dispersão de conjuntos de dados é a aprendizagem por transferência; para fazer uso de recursos semelhantes e, em seguida, transferir o modelo para a tarefa real. Um exemplo recente de sucesso para este procedimento é Felbo et al. (2017) que apresentam um modelo de rede neural treinado em emoticons que é então transferido para diferentes tarefas posteriores, ou seja, a previsão de sentimento, sarcasmo e emoções.

(Adicionar Table 1)

## 3 Conjunto de dados unificado de anotações de emoção

Nesta seção, descrevemos cada conjunto de dados que agregamos em nosso corpus unificado. Fornecemos uma breve descrição e mostramos como os diferentes esquemas são mesclados. Observe que nossa interpretação pode ser diferente da descrição original do autor (embora tenhamos o objetivo de evitar isso).

### 3.1 Visão geral dos conjuntos de dados

- AffectiveText

    O conjunto de dados AffectiveText publicado por Strapparava e Mihalcea (2007) é construído em manchetes de notícias e consiste em 1.250 instâncias. O objetivo principal deste recurso é a classificação das emoções e valência nas manchetes dos jornais. O esquema de anotação segue as emoções básicas de Ekman, complementadas por valência. É multi-rótulo anotado por meio de anotação de especialista e pode ser baixado gratuitamente, a licença não é especificada. As categorias de emoção são atribuídas a uma pontuação de 0 a 100. Os dados de treinamento / desenvolvimento chegam a 250 títulos anotados, enquanto os sistemas são avaliados em outras 1.000 instâncias.

- Blogs

    Este conjunto de dados, publicado por Aman e Szpakowicz (2007) consiste em 5.205 frases de 173 blogs. As instâncias são anotadas com um rótulo de emoção cada, intensidade da emoção e indicadores de emoção. O esquema de anotação para a categoria de emoção corresponde às seis emoções Ekman fundamentais às quais nenhuma emoção é adicionada. Este recurso pode ser obtido entrando em contato com os autores.

- CrowdFlower
    
    O conjunto de dados “The Emotion in Text, publicado pela CrowdFlower” consiste em 39.740 tweets. Parte desses dados foi usada na Galeria Cortana Intelligence da Microsoft. O conjunto de etiquetas não é padrão (consulte a Tabela 4). É anotado via crowdsourcing com um rótulo por tweet e pode ser baixado gratuitamente, a licença não é especificada. Os dados são comparativamente ruidosos.

- DailyDialogs
    
    O conjunto de dados, publicado por Li et al. (2017), é construído em conversas e consiste em 13.118 frases. O esquema de anotação segue Ekman, complementado por “sem emoção”. É um rótulo único anotado por meio de anotação de especialista e pode ser baixado gratuitamente para fins de pesquisa. Este conjunto de dados possui anotações adicionais para intenção e tópico de comunicação.

- Electoral-Tweets

    O conjunto de dados, publicado por Mohammad et al. (2015), visa o domínio das eleições. Ele consiste em mais de 100.000 respostas a dois questionários on-line detalhados (as perguntas direcionadas a emoções, propósito e estilo em tweets eleitorais). Os tweets são anotados por meio de crowdsourcing. O conjunto de rótulos para emoção não é padronizado (consulte a Tabela 1). Além das anotações em nível de documento, os tweets são anotados com palavras de emoção. Ele pode ser baixado gratuitamente para fins de pesquisa.

- EmoBank

    O conjunto de dados publicado por Buechel e Hahn (2017a) baseia-se em vários gêneros e domínios. É composto por 10.548 frases em que cada frase foi anotada manualmente de acordo com a emoção que é expressa pelo escritor e a emoção que é percebida pelos leitores. As anotações estão de acordo com o modelo valência-excitação-dominância. Um subconjunto do corpus é AffectiveText, o que torna este conjunto de dados um bom recurso para projetar modelos que mapeiam entre representações discretas ou dimensionais.

- EmoInt

    O EmoInt publicado por Mohammad e Bravo-Marquez (2017a) baseia-se no conteúdo de mídia social que soma 7.097 tweets. O objetivo principal deste recurso é associar o texto a várias intensidades de emoção. Os tweets são anotados via crowdsourcing com intensidades de raiva, alegria, tristeza e medo, enquanto a maioria dos tweets são anotados com apenas uma emoção. Ele pode ser baixado gratuitamente para fins de pesquisa.

- Emotion-Stimulus

    O conjunto de dados Emotion-Stimulus publicado por Ghazi et al. (2015) consiste em 820 frases que são anotadas tanto com as emoções e suas causas, e 1.549 frases que são marcadas apenas com suas emoções. O conjunto de rótulos usados ​​para anotações consiste nas emoções básicas de Ekman às quais a vergonha é adicionada. O objetivo principal deste recurso é prever a causa de uma emoção no texto. É anotado usando o quadro direcionado às emoções do FrameNet com um rótulo de emoção por frase. Ele está disponível para download para fins de pesquisa.

- fb-valence-arousal

    O conjunto de dados fb-valence-arousal publicado por Preot¸iuc-Pietro et al. (2016) é baseado em postagens do Facebook. É composto por 2.895 postagens estratificadas por idade e sexo. O objetivo principal deste recurso é treinar modelos de previsão para valência e excitação. Cada mensagem é gravada por um usuário distinto e todas as mensagens são do mesmo intervalo de tempo. As postagens são anotadas com valência e excitação em uma escala de nove pontos por meio de anotação de especialista. Ele está disponível para download.

- Grounded-Emotions

    O conjunto de dados publicado por Liu et al. (2017) é construído na mídia social e consiste em 2.557 instâncias rotuladas únicas publicadas por 1.369 usuários únicos. O objetivo principal deste recurso é colocar as emoções no contexto de outros fatores, incluindo clima, eventos de notícias, rede social, predisposição do usuário e tempo. O conjunto de rótulos está alegre e triste. Os tweets são anotados pelos autores. As informações são utilizadas em experimentos com o objetivo de mostrar o papel desempenhado pelo contexto externo na previsão de emoções.

- ISEAR

    O conjunto de dados “Pesquisa Internacional sobre Antecedentes e Reações Emoções” publicado por Scherer e Wallbott (1994) é construído a partir da coleta de questionários respondidos por pessoas com diferentes origens culturais. Essas pessoas relatam seus próprios eventos emocionais. O conjunto de dados final contém relatórios de aproximadamente 3.000 entrevistados, para um total de 7.665 frases marcadas com emoções únicas. Os rótulos são alegria, medo, raiva, tristeza, nojo, vergonha e culpa. Ele está disponível para download.

- SSEC

    The Stance Sentiment Emotion Corpus publicado por Schuff et al. (2017) é uma anotação do conjunto de dados de opinião e sentimento do SemEval 2016 no Twitter (Mohammad et al., 2017). É composto por 4.868 tweets. O objetivo principal deste recurso é possibilitar novas pesquisas sobre as relações entre as emoções e outros fatores. É anotado por meio de anotação de especialista com vários rótulos de emoção por tweet seguindo as emoções fundamentais de Plutchik. Um recurso adicional deste recurso é que eles não apenas fornecem uma anotação majoritária, mas publicam as informações individuais para todos os anotadores.

- Tales

    The Tales corpus publicado por Alm et al. (2005) é baseado na literatura e consiste em 15.302 frases de 185 contos de fadas de B. Potter, H.C. Andersen e os irmãos Grimm. Destas 15.302 frases, todos os anotadores concordam apenas em 1.280. O objetivo principal deste recurso é construir classificadores de emoções para a literatura. O esquema de anotação consiste nas seis emoções básicas de Ekman. Nos dados finais, os rótulos de raiva e nojo são mesclados. Ele pode ser baixado gratuitamente para fins de pesquisa.

- TEC

    O Twitter Emotion Corpus publicado por Mohammad (2012) é construído nas redes sociais. É composto por 21.051 tweets. O objetivo principal deste recurso é responder à pergunta se hashtags de palavras emocionais podem ser usadas com sucesso como rótulos de emoção. O esquema de anotação corresponde ao modelo de emoções básicas de Ekman. Eles coletaram tweets com hashtags correspondentes às seis emoções do Ekman: #anger, #disgust, #fear, #happy, #sadness e #surprise, portanto, é anotado distantemente em um único rótulo. Ele pode ser baixado gratuitamente para fins de pesquisa.

(Adicionar Table 2)

### 3.2 Análise

A maioria dos recursos consiste em dados gerados pelo usuário. O maior conjunto de dados é CrowdFlower com 39.740 tweets anotados, seguido por TEC com 21.051 tweets e Blogs com 15.000 sentenças anotadas de posts de blog. De todos os 14 recursos, 9 são anotados com as seis emoções fundamentais definidas por Ekman, com pequenas variações. SSEC e tweets eleitorais seguem o modelo de Plutchik. Eleitoral-Tweets é anotado com 19 emoções e os autores fornecem um mapeamento para o conjunto de Plutchik (que seguimos na agregação). Emoções não fundamentais são anotadas em CrowdFlower (diversão, preocupação, entusiasmo e amor).

Não apenas o tamanho, também a distribuição dos rótulos é diferente nos corpora. A Tabela 2 mostra a distribuição das emoções de Ekman antes e depois de ter aplicado o mapeamento a um conjunto único de rótulos de emoção (ver Tabela 4 no Apêndice A). Em muitos corpora, a alegria é a emoção dominante, seguida pela tristeza, surpresa e raiva. As exceções são SSEC, Eleitoral-Tweets e EmoInt, em que as emoções negativas são mais frequentes. No SSEC, isso se deve à sua origem como um conjunto de dados de posição. Da mesma forma, os Tweets Eleitorais mostram uma natureza polarizadora dos debates políticos, com repulsa e raiva sendo mais comuns.

A Figura 1 mostra uma comparação de similaridade quantitativa dos dados. Representamos cada conjunto de dados por sua distribuição de termos, pegando as 5.000 palavras mais comuns de cada conjunto de dados e calculando a similaridade de cosseno entre os corpora (inspirado por Ruder e Plank (2017) e Plank e Van Noord (2011)). Os corpora do Twitter são mais semelhantes entre si (EmoInt e CrowdFlower são os mais semelhantes ao TEC) do que outros domínios, com exceção do SSEC, que é o mais diferente dos outros conjuntos de dados de tweet. DailyDialogs é mais semelhante aos tweets do que ISEAR e Blogs.

A coluna All representa a união de todos os conjuntos de dados, exceto aquele que está sendo comparado. Nesse contexto, o mais diferente em relação ao respectivo conjunto agregado é AffectiveText. A razão é que este é um pequeno conjunto de dados em comparação com os corpora baseados em tweet e que cobre um tópico específico, as manchetes. O Grounded-Emotions também é notavelmente diferente. Mais semelhante a All é EmoInt, seguido de perto por TEC e Blogs, que cobre postagens de blog e não tweets.

(Adicionar Figure 1)

### 3.3 Agregação

Para fornecer um acesso padronizado aos conjuntos de dados, definimos alegria, raiva, tristeza, repulsa, medo, confiança, surpresa, amor, confusão, antecipação e noemo como nosso conjunto de rótulos comum. Os recursos originais incluem, adicionalmente, outros 44 rótulos provenientes de Electoral-Tweets e CrowdFlower. Sempre que disponível a partir da publicação original, seguimos os mapeamentos propostos (por exemplo, Eleitorais-Tweets com 19 emoções e um mapeamento para o modelo de Plutchik). A Tabela 4 do Apêndice A resume o mapeamento. Incluímos valência, excitação e dominância onde anotados, no entanto, atualmente não mapeamos os modelos categóricos de emoção nos dimensionais.

Cada instância no conjunto de dados unificado contém, além disso, um id único, o nome do corpus de origem, o texto e uma atribuição de um número real para cada uma das 11 variáveis ​​de emoção. Na maioria dos conjuntos de dados, cada instância é apenas anotada discretamente com rótulos únicos (as exceções são SSEC e AffectiveText). Portanto, a maioria das instâncias tem exatamente um rótulo marcado com 1.0. Para os conjuntos de dados multi-rotulados, várias emoções podem ser marcadas. Para conjuntos de dados com intensidade de emoção anotada, os valores podem variar entre 0 e 1. Para conjuntos de dados com várias informações do anotador, seguimos as recomendações dos autores originais. Para SSEC, isso significa aceitar um rótulo se pelo menos um anotador o atribuiu. Para Tales, os autores fornecem uma anotação dourada, na qual raiva e desgosto são mesclados. Nós os tratamos separadamente e aceitamos uma etiqueta se e somente se todos os anotadores concordarem. Para Blogs, pegamos os exemplos do conjunto de dados com as anotações de alta concordância.

Ao lado desses atributos fundamentais, fornecemos as informações de domínio e estilo de anotação, bem como atributos adicionais específicos do conjunto de dados (por exemplo, a história da qual uma instância se origina, no corpus do Tales). Nossos dados unificados incluem todos os conjuntos de dados para os quais as licenças estão disponíveis; como alguns conjuntos de dados não são redistribuíveis, mas são gratuitos para download, fornecemos um script que baixa e converte interativamente os recursos existentes em nosso formato unificado. Um trecho dos dados é descrito no Apêndice B.

## 4 experimentos

Realizamos experimentos nas seguintes configurações: Em primeiro lugar, realizamos uma classificação de emoções dentro do corpus (treinamento em um corpus e teste no mesmo, usando validação cruzada5). Em segundo lugar, fazemos avaliação de corpus em pares: treinando todos os dados de um corpus e avaliando todos os dados de um corpus diferente, para todos os pares de corpus. Isso inclui o uso do corpus agregado, mas para este experimento excluindo o corpus de teste - isso corresponde a uma configuração de validação cruzada em que os subconjuntos correspondem aos corpora.

### 4.1 Configurações experimentais

Métodos anteriores mostraram que os classificadores lineares são quase iguais aos métodos neurais (Schuff et al., 2017). Portanto, usamos classificadores de entropia máxima conforme implementados no scikit-learn (Pedregosa et al., 2011) com recursos de saco de palavras (BOW) para esses experimentos para simplicidade e fácil reprodutibilidade. Usamos a regularização L2 e equilibramos as classes nos dados de treinamento com pesos de instância. As divisões de treinamento / teste são 80% / 20%. A validação cruzada é estratificada em 10 vezes.

Para conjuntos de dados nos quais os rótulos não se alinham de acordo com nosso mapeamento, usamos a interseção dos rótulos no trem e nos dados de teste. Não descartamos nenhuma instância. Para conjuntos de dados projetados para outras tarefas além da classificação de emoção, como EmoInt e Emotion-Stimulus, não alteramos a configuração de nossa tarefa de classificação.

Para experimentos em que passamos de uma configuração de vários rótulos (mais de uma emoção pode conter por instância) para uma configuração de rótulo único (apenas uma emoção é mantida), treinamos vários classificadores binários dos quais apenas aceitamos, na fase de previsão , a emoção de maior pontuação. Para experimentos nos quais passamos de uma configuração de rótulo único para uma configuração de rótulo múltiplo, também treinamos classificadores binários separados e aceitamos todas as emoções para as quais os classificadores binários geram um. O case multi-rótulo para multi-rótulo funciona de forma análoga. De rótulo único para rótulo único, usamos um modelo MaxEnt multiclasse.

### 4.2 Resultados

Classificação de emoções dentro do corpus. Os resultados de nosso primeiro experimento são mostrados por emoção na Tabela 3 (onde restringimos os resultados às seis emoções fundamentais definidas por Ekman) e na diagonal da Figura 2. Elas devem ser interpretadas no contexto da análise de similaridade na Figura 1. Vemos que alguns conjuntos de dados e domínios são mais difíceis de modelar do que outros. O conjunto de dados “mais fácil” parece ser Emotion-Stimulus, seguido por EmoInt. A razão para as pontuações altas reside no fato de que ambos os conjuntos de dados são construídos para tarefas diferentes (estímulo e previsão de intensidade). Como tal, nossa tarefa não se ajusta muito bem a esses dois.

Os próximos conjuntos de dados com medidas de desempenho comparativamente altas são Blogs e DailyDialogs. Em contraste, CrowdFlower e Electoral-Tweets parecem ser os mais desafiadores no cenário dentro do corpus. Para CrowdFlower, os resultados se devem ao conjunto maior de rótulos, o que torna a tarefa mais difícil. Na maioria das vezes, as emoções que ocorrem com menos frequência (como surpresa) apresentam resultados mais baixos do que as que ocorrem com frequência (como alegria e tristeza). Além disso, a inspeção manual mostra que esses dados são comparativamente ruidosos. Isso também é apoiado pela observação geral de que nosso modelo tem um desempenho geralmente pior nos dados do Twitter do que na maioria dos outros domínios.

Em termos de procedimentos de anotação, esses experimentos quase não permitem julgamento, uma vez que a maioria dos conjuntos de dados usa anotação de especialista e temos apenas alguns exemplos para as outras duas formas de anotação (crowdsourcing e supervisão distante) sendo usadas. No entanto, pudemos observar que os conjuntos de dados crowdsourced são mais difíceis, o que pode ser devido a uma anotação mais ruidosa.

Classificação de emoção entre corpus Os resultados dentro do corpus (a diagonal na Figura 2) mostram pontuações F1 mais altas do que os resultados entre corpus. A exceção são os Tweets Eleitorais, onde o mesmo desempenho é observado por meio de treinamento em um corpus diferente, os Blogs. Os modelos treinados nos dados do Twitter têm um desempenho um pouco melhor em outros conjuntos do Twitter, com exceção dos Tweets Eleitorais, para os quais a distribuição de rótulos é diferente, com repulsa dominando o conjunto.

(Adicionar Table 3)

É notável que EmoInt, Emotion-Stimulus, Grounded-Emotions ISEAR e SSEC são mais fáceis de classificar (alto desempenho quando usado para teste), enquanto DailyDialogs, Blogs, CrowdFlower e Tales são mais informativos: treinando neles e classificando outros leads de conjuntos de dados para melhores resultados. Os modelos treinados em ISEAR e SSEC têm um desempenho comparativamente bom. DailyDialogs é melhor classificado não por um classificador treinado em si mesmo, mas por um classificador treinado em blogs.

Não podemos recomendar o treinamento de Emotion-Stimulus e Grounded-Emotions, desde que as propriedades específicas desses conjuntos de dados não sejam necessárias. Os modelos estimados nesses dados não têm um bom desempenho em outros conjuntos. Observe que este não é um julgamento de qualidade, o Grounded-Emotions tem rótulos diferentes e o Emotion-Stimulus foi projetado para um propósito diferente.

Observe que a medida de similaridade é uma aproximação da qualidade medíocre para o desempenho do modelo, com uma correlação de Pearson de r = 0,32.

Classificação de emoção cruzada de corpus tudo contra um. Os resultados dessa configuração podem ser vistos na coluna All da Figura 2. Esses resultados mostram quais conjuntos de dados são mais fáceis de classificar, ou seja, DailyDialogs, Blogs e EmoInt. Pode parecer intuitivo que adicionar mais dados de treinamento diversificados possa ser útil na classificação de quase todos os conjuntos de dados. No entanto, podemos ver nos resultados que este não é o caso. Especialmente os conjuntos de dados multi-rotulados AffectiveText e SSEC junto com os conjuntos de dados que foram mais transformados (ou seja, muitos rótulos unificados) durante o processo de agregação, como Electoral-Tweets e CrowdFlower são mais difíceis de classificar durante o treinamento em todos os outros conjuntos de dados.

## 5 Conclusão e Trabalho Futuro

Conjuntos de dados anotados para classificação de emoção são importantes na pesquisa de análise de emoção, pois também são usados ​​em muitas tarefas posteriores; ter esses conjuntos de dados instalados reduz drasticamente a quantidade de trabalho necessária no pré-processamento e na transferência dos dados necessários. No entanto, é uma coleção diversa de conjuntos de dados conduzidos por diferentes modelos de emoções psicológicas, em diferentes domínios e abordagens usadas na anotação. Com este artigo, apresentamos a primeira pesquisa sobre conjuntos de dados de emoção em texto.

Além desta revisão da literatura, fornecemos um corpus unificado e agregado para apoiar pesquisas futuras sobre dados padronizados. A existência de tal benchmark abre a possibilidade de outros experimentos, como aprendizagem por transferência e trabalho de adaptação de domínio, com foco em diferentes domínios e em diferentes conjuntos de rótulos. A partir dos conjuntos de dados unificados coletados, pode-se aprender como selecionar o conjunto de dados mais adequado para um determinado novo domínio e avaliá-lo em diferentes modelos de classificação, domínios e procedimentos de anotação, mais fácil do que era possível até agora. Também ter isso aberto ajudará a tarefa de detecção de emoções a se tornar uma tarefa padrão na qual os métodos mais modernos usados ​​na classificação geral são testados, da mesma forma que outras tarefas como a classificação de sentimento, que já desempenha esse papel.

(Adicionar Figure 2)

Além disso, este trabalho pode ser usado por qualquer pessoa que queira explorar o estado atual do campo da análise de emoções. Como trabalho futuro, planejamos lançar outra versão do conjunto de dados em que a conversão entre os diferentes modelos de emoção são adicionados e realizar experimentos de aprendizagem de transferência entre conjuntos de dados, domínios e procedimentos de anotação. Além disso, propomos usar o recurso para analisar qualitativamente as diferentes realizações de emoções em esquemas e domínios de anotação.

## Reconhecimentos

Esta pesquisa foi financiada pelo German Research Council (DFG), projeto SEAT (Structured MultiDomain Emotion Analysis from Text, KL 2869 / 1-1). Agradecemos a Evgeny Kim e Jeremy Barnes pelas discussões frutíferas.