---
author: "Pedro Falcão"
title: "MLOps - Um projeto introdutório"
date: "2024-01-19"
description: "Aplicando DevOps ao mundo do Aprendizado de Máquina"
tags: ["MLOps", "DevOps", "Machine Learning"]
categories: ["Programação"]
aliases: ["mlops-introductory"]
draft: false

cover:
    image: "https://github.com/Pfalcao97/blog/blob/main/static/cover_mlops.png?raw=true"
    alt: "A estatística bayesiana"
---

# Introdução

É argumentável que 2023 foi o ano da Inteligência Artificial. Pessoas que, como eu, já tinham um contato mais próximo com a estatística, em virtude da sua formação acadêmica, conheciam conceitos que são o "feijão com arroz" de _Machine Learning_ (ML), como regressão e otimização, alguns até já sabiam sobre a existência das IAs generativas, mas poucos realmente possuiam acesso à tecnologia.

Isso mudou, principalmente, graças ao ChatGPT, uma inteligência artificial generativa desenvolvida pela companhia (ou organização? confesso que não sei direito _qual é_ a deles) OpenAI. De repente todo mundo com acesso à internet tinha um robôzinho capaz de responder qualquer coisa em poucos _clicks_. A ferramenta foi lançada no frigir dos ovos de 2022 e, ainda em Janeiro do ano seguinte, já havia chegado aos 100 milhões de usuários, uma explosão de usuários tão intensa, que lhe angariou o título de "Aplicação de mais rápido crescimento da história". \[1\]

Junto desse crescimento aterrador, percebi duas formas com as quais as pessoas tendiam a encarar o fenômeno:

1. Um sentimento análogo ao início da internet, com um otimismo exagerado e, principalmente, uso indiscriminado das novas ferramentas.
2. Em um tom um pouco mais comedido, a compreensão do ônus e o bônus das IAs, tentando ser mais crítico ao uso da nova tecnologia.

Eu tendo a ser um cara mais cético, então fiquei no segundo grupo. Mas isso não me faz um ludista digital, pelo contrário. Por trabalhar na área, eu sou bombardeado com novas tecnologias cujos desenvolvedores juram de pés juntos que vão revolucionar o mundo. Meu trabalho, em partes, é separar o joio do trigo.

Se a tecnologia é passageira e eu pular nesse barco sem olhar pra trás, em alguns meses vou ter uma miríade de soluções desenhadas para uma tecnologia que não possui mais suporte. Essa brincadeira sai caro e é trabalhosa, gerando o tão temido **debito técnico** :ghost:. 

Por outro lado, ao não adotar o novo _estado da arte_ na minha área, eu estou correndo o risco de me tornar um profissional desatualizado e minhas aplicações serem piores do que elas poderiam ser.

Tudo na vida é questão de equilíbrio. No caso do Aprendizado de Máquina, a história não é diferente.

Dito isso, as inteligências artifíciais e ferramentas de aprendizado de máquina vieram para ficar. Quando o otimismo desenfreado acabar, muitas pessoas vão ver que estão usando um canhão para matar formigas, e, espero, adotar ferramentas mais bem dimensionadas para o serviço. Para aqueles problemas que realmente precisam (ou se beneficiam) do Aprendizado de Máquina, surge o MLOps!

{{< figure align="center" src="https://github.com/Pfalcao97/blog/blob/main/static/amazon_chatgpt.jpg?raw=true" width="auto" >}}
_"ai is coming for our j-", @internetofshit ([via Twitter](https://twitter.com/internetofshit/status/1745905483297558752))_ 

# MLOps - DevOps aplicado à Machine Learning

MLOps é algo relativamente recente, até porque o uso de Machine Learning, pelo menos aqui no Brasil, começou a ser difundido recentemente. Uma necessidade maior em criar aplicações que se apoiem nessa nova onda de IA começou a ganhar corpo com as APIs da OpenAI, que poderiam ser usadas para criar um chat bot inteligente ou artes customizadas e instantâneas para uma nova campanha de marketing.

Junto, porém, dessa nova vontade, surge (ou **deveria** surgir) também a precaução. Cada vez mais nós vemos que grandes IAs generativas foram criadas em bases indevidas \[2\]\[3\]\[4\] e não devem ter nossa confiança irrestrita, em especial em relação ao uso de dados sensíveis \[2\]\[5\]. No melhor dos casos, uma IA mal implementada pode gerar alguns _clicks_ a mais no seu site, porque os usuários estão se divertindo, como aconteceu com o site da Chevrolet de Watsonville (California) \[6\].

Diante disso, surge um problema para esses recém chegados: _"ok, se não o chat gpt, o que?"_.

E, pra essa pergunta, surgem milhões de respostas, desde soluções proprietárias de outras empresas até os modelos de código aberto e construidos com auxílio da comunidade. Ao escolher a segunda opção, de repente o uso de ferramentas de Machine Learning não é mais uma chamada de API: agora você precisa escolher um modelo, treinar ele, hospedar (provavelmente na núvem) e garantir sua estabilidade.

**É um trabalho e tanto.**

Para os profissionais das mais diversas áreas que estavam criando os modelos, toda essa parte pós-modelagem era nebulosa, mas os desenvolvedores de software já haviam solucionado esse mesmo problema, embora em outro contexto, ao adotar **a metodologia de DevOps**!

A ideia é simples: automatizar o máximo de processos possíveis, para garantir estabilidade e agilidade na hora de implantar uma solução. Ao invés de versões, passa-se a pensar na prática do desenvolvimento como um ciclo. Você deve ter percebido o movimento dos programas deixando de se tornar aplicativos que você compara, para se tornarem serviços que você contrata (é o tal do _Software as a Service_ - SaaS) - isso é consequência desse novo _modus operandi_ de **DevOps** que, dentre outras coisas, prevê o desenvolvimento contínuo!

Ou seja, ao invés da Adobe lançar uma nova versão do Photoshop todo ano, agora ela lança um serviço de inscrição que você contrata e tem acesso ao código mais atualizado, que é continuamente desenvolvido e melhorado pelos desenvolvedores da empresa (com o "benefício" adicional - para a empresa - de que agora você não paga uma vez e é "dono" de um software, mas sim tem que pagar todo mês uma taxa se não perde acesso à ele)!

Essas esteiras de desenvolvimento, como são chamadas em português, são norteadas por alguns princípios básicos:

- **Integração Contínua:** cada mudança feita por cada desenvolvedor passa por testes automáticos e revisões por pares, garantindo assim que a sugestão só será "mergeada" (ou seja, a mudança só será aplicada) caso os principais problemas do código sejam identificados e corrigidos. Se cada mudança sugerida foi testada e validada, a possibilidade do todo ter problemas é menor.

- **Entrega Contínua:** uma vez que esse código é mergeado no repositório central, um fluxo de entrega (ou implantação) é ativado, de forma que em poucos minutos todos os passos necessários para colocar o código em produção são executados de forma automática.

- **Microsserviços:** o serviço do software é oferecido de forma a ser simples (em termos relativos, não objetivos) e direto, no sentido de fazer melhor, menos tarefas. Esse código é, muitas vezes, implantado na forma de conteiners (Docker) ou aplicações autônomas na núvem, que não requerem a contratação de um servidor específico, podendo ser dinâmicamente escalado conforme a necessidade (_serverless_).

- **Infraestrutura por código:** ainda na temática da núvem, alocar os recursos físicos da companhia aos servidores de um provedor maior te dá maior liberdade para dimensionar os serviços de forma mais conservadora, sem se preocupar (tanto) na expansão e necessidades futuras - Precisa de um pouco mais de memória? dois _clicks_ e você está com uma máquina mais potente (e uma fatura mais salgada no fim do mês). Dessa forma, pode-se gerir a infraestrutura disponível aos serviços justamente como se gere um código, com versionamento e arquivos de texto, permitindo maior flexibilidade na gestão de recursos computacionais.

- **Monitoramento contínuo:** todos esses serviços geram dados e logs continuamente com o uso, então o monitoramente é, também, contínuo. Dashboards, avisos no _Slack_ e painéis de monitoramento viram parte fundamental do engenheiro desenvolvendo as soluções. Isso contribui com o ciclo de desenvolvimento, pois, assim que um erro é alertado, o desenvolvedor pode corrigir o código e colocar a solução em produção, tudo numa fração mínima de tempo.

E, independente do assunto, **software é software**. Não importa se você está desenvolvendo uma aplicação de cálculo de gastos mensais, um aplicativo de reconhecimento facil ou uma _pipeline_ de transformação de dados, tudo isso é código e pode ser gerido usando os princípios de DevOps. Assim surgem os conceitos mais "nichados" de DataOps ou MLOps.

# Mas, pra quem serve o MLOps?

Bom, a resposta simples e direta é: qualquer um que possui aplicações de _Machine Learning_ e quer otimizá-las conforme as boas práticas. (_tl;dr_)

Se você está satisfeito, basta [clicar aqui](http://localhost:1313/blog/posts/mlops/#desenvolvendo-uma-aplica%C3%A7%C3%A3o-de-mlops) pra ir pra próxima seção, caso contrário, vamos por partes.

O livro Practical MLOps, uma das maiores fontes de informação desse texto, faz uma comparação com a pirâmide de Maslow que eu achei bem interessante. Para aqueles que não conhecem, a pirâmide de Maslow define os níveis de necessidade humana: você precisa garantir água e comida para poder perseguir segurança. Uma vez que você já se sente seguro constantemente e tem recursos suficientes pra se manter, você começa a se preocupar com as suas relações sociais, seus vínculos. A ideia é que um nível mais alto do potêncial humano só pode ser atingido à partir do momento em que as suas necessidades mais básicas são garantidas.

Para MLOps, é a mesma coisa, segundo os autores.

{{< figure align="center" src="https://github.com/Pfalcao97/blog/blob/main/static/maslow_mlops.png?raw=true" width="auto" >}}

O Engenheiro precisa ser proficiente em DevOps e ter as ferramentas e dados à disposição. Não faz sentido querer implementar um esteira de aprendizado de máquina, sem ao menos ter os dados da sua companhia à disposição para treinar/usar o modelo implantado!

Primeiro é preciso garantir que todas as aplicações funcionam corretamente e estão seguindo o fluxo correto (**DevOps**). Depois, nos preocupamos com os dados e a qualidade deles, garantindo que as _pipelines_ são estáveis e entregam dados de qualidade conforme a necessidade do negócio (**DataOps**). Só então nos preocupamos com os modelos de aprendizado de máquina (**MLOps**). Claro, as coisas podem ser construídas paralelamente e não precisam, necessariamente, seguir essa ordem, mas, de forma geral, inverter essa lógica seria como construir um prédio em uma base frágil. Santos fez isso e não deu muito certo... \[7\]

Supondo, então, que a sua companhia já tem essas bases necessárias, como fica o time de dados? O Engenheiro de _Machine Learning_ é par do Cientista de Dados? Eles trabalham juntos para fazer o modelo? Como funciona?

Claro, eu não tenho todas as respostas. E, mais importante ainda, não existe **uma** resposta, cada caso é um caso. Companhias diferentes terão estruturas diferentes, pela própria natureza da forma como as coisas são estruturadas.

Mas, na minha visão particular, a relação entre o Cientista de Dados e o Engenheiro de Machine Learning, é paralela àquela entre o Analista de Dados e o Engenheiro de Dados.

Surge um problema de negócio, o analista de dados entende o problema e quais são os recursos necessários para solucioná-lo. O engenheiro de dados cria o ETL e disponibiliza os dados para consumo contínuo. Se for necessário, ele disponibiliza microsserviços para incrementar a solução. O analista pode entregar para a área de negócio a dashboard, com a certeza de que os dados continuarão a ficar disponíveis, por conta do trabalho dos engenheiros.

Analogamente: surge um problema de negócio, o cientista de dados entende o problema e quais são os recursos necessários para solucioná-lo, talvez até desenvolvendo um `jupyter notebook` com uma análise e modelo preliminares. O Engenheiro de _Machine Learning_, a partir dessas específicações, vai transformar o `notebook` em um microsserviço contaînerizado com um fluxo de dados limpos e corretos em uma _feature store_ e uma programação de re-treinamento contínuo, para garantir a qualidade do modelo, mesmo com o fluxo de novos dados. Assim, o cientista tem acesso ao microsserviço do modelo, que pode ser aplicado a novos dados para gerar relatórios e informações estratégicas para a área de negócio, com a seguraça garantida por todo o processo de DevOps.

Ou seja, a ideia é a mesma nos dois casos: um dos profissionais cuida da parte mais humana e de negócio (entender o problema, abstrair a solução necessária, desenvolver os requerimentos, realizar análises) e o outro está mais focado na parte técnica (aplicar boas práticas de DevOps, garantir a estabilidade do processo, monitorar e corrigir erros). 

Isso, claro, num vácuo. Na vida real, pode ser que o Cientista de Dados seja bem versado nas práticas de DevOps e você nem precise de um Engenheiro de _Machine Learning_. Pode ser que um Engenheiro de Dados tenha experiência com aprendizado de máquina e, por conta da baixa demanda, possa cuidar de casos _ad hoc_. A boa e velha máxima prevalece sempre: **na prática, a teoria é outra**.

# Desenvolvendo uma aplicação de MLOps

Agora que você aprendeu as bases do que é o MLOps, vamos ao que importa: como aplicar. E nada melhor do que um projetinho simples para iniciar essa jornada!

[Todo o código do projeto está disponível em um repositório público!](https://github.com/Pfalcao97/mlops-example)

Evidente que o projeto, simples por força de sua proposta, não irá tocar todos os pontos chave de MLOps. Aliás, pela natureza dessa aplicação, talvez nem toque nos pontos principais (ou mais atrativos) da metodologia, afinal, eu estou escrevendo um texto que deve ser concebido, desenvolvido e publicado em um espaço curto e em condições microscópicas, diante do contexto de _Big Data_. O que isso significa é que eu não vou conseguir desenvolver _pipelines_ resilientes de dados (até porque eu nem os tenho) e nem vou ter tempo para notar _bugs_ e implementar correções dinâmicamente.

O que eu **vou** conseguir fazer e, portanto, minha proposta é: **desenvolver uma aplicação de síntetese de textos usando um modelo de linguagem de grande escala** (do inglês _Large Language Model_ - LLM) **com código aberto, implantado na núvem e com um fluxo de CI/CD desenvolvido usando Github Actions**.

Vou, também, me permitir tomar alguns atalhos. Ao invés de treinar uma LLM do zero, vou aproveitar da plataforma _Hugging Face_ que já possui uma grande variedade de modelos prontos, inclusive em português, pra poder focar no processo de implantação de um aplicativo à núvem. A núvem escolhida, inclusive, é a da Google, _Google Cloud Plataform_ (GCP), justamente por conta do _free tier_ ser suficiente para esse projetinho simples.

### O modelo

Tudo começa no hub do [Hugging Face](https://huggingface.co/), uma grande comunidade de compartilhamento de código para aplicações de ML. Para quem vem da área de análise de dados, dá pra pensar como sendo o equivalente do Kaggle. A missão deles, segundo sua própria página, é democratizar o *bom* aprendizado de máquina.

No site existem opções de diferentes modelos, bases de dados e até uma plataforma para hospedar um aplicativo de ML, permitindo a distribuição do seu trabalho. Além da url, é possível acessar os modelos através de uma API!

Na aba de modelos, é possível filtrar entre (pelo menos durante o período em que eu escrevo) quase meio milhão de opções, por função, linguagem, base de dados, licença de uso e outras.

Eu optei por fazer dois filtros: como quero implementar uma LLM, o modelo precisa ter sido treinado em português, então eu seleciono aqueles que são cadastrados nessa lingua. Isso faz com que a seleção de meio milhão de modelos caia para um pouco mais de mil. Em seguida, busco por modelos que façam `text classification`, isto é, que "compreendam" um texto e te informe como o autor estava se sentindo.

Um texto como "Nossa, eu gosto muito de estudar MLOps" deve ser entendido como **Positivo**, outras frases serão negativas ou, até mesmo, neutras (pense como seria difícil distinguir a emoção em "Isto é uma batata"). 

Esse é exatamente o tipo de ferramenta que pode ser muito útil em pesquisas massivas de opinião, como quando pesquisadores analizam tweets para entender como a população geral está se manifestando em relação a um tópico. Imagine, por exemplo, um debate presidencial ao vivo: ao invés de assumir o pesadelo logístico de fazer pesquisas de opinião sérias em tempo real pela internet para ver como os candidatos estão se saindo, basta olhar o que as pessoas estão dizendo por livre e expontânea vontade no twitter. Claro, com um volume massivo de dados, ninguém vai ficar lendo e avaliando caso-a-caso, o algoritmo faz isso!

Exatamente nesse contexto, escolhi o modelo `twitter-xlm-roberta-base-sentiment-finetunned`, baseado no modelo RoBERTa do Facebook, [criado pelo usuário citizenlab](https://huggingface.co/citizenlab/twitter-xlm-roberta-base-sentiment-finetunned). Como disse, a Hugging Face possui uma API que abstrai muitas das complexidades de usar um modelo de código aberto como esse, de forma que, para chamá-lo programaticamente basta baixar as bibliotecas com o `pip` e três linhas de código (seguindo a documentação):

```python
from transformers import pipeline

sentiment_classifier = pipeline(
    "text-classification", 
    model="citizenlab/twitter-xlm-roberta-base-sentiment-finetunned",
    tokenizer="citizenlab/twitter-xlm-roberta-base-sentiment-finetunned"
)
sentiment_classifier("this is a lovely message")
```

Ao que devemos receber de resposta algo similar à:

```
> [{'label': 'Positive', 'score': 0.9918450713157654}]
```

Testando mensagens em português, recebo os sentimentos esperados, por exemplo:

```
sentiment_classifier("Nossa, eu gosto muito de estudar MLOps")
> [{'label': 'Positive', 'score': 0.989}]
```

Ou seja, o modelo também funciona bem em português!

### A entrega

Com o modelo em mãos, podemos começar a pensar em formas de deixá-lo disponível aos usuários. Existem diversas opções, desde dashboards e bots até **webapps**; essa última sendo a mais comum e, portanto, a minha escolha.

Em Python existem várias bibliotecas que auxiliam na criação, mas eu optei pela `FastAPI`, que tem crescido em popularidade e brigando com os pesos pesados da linguagem, `Flask` e `Django`. Algo que eu gosto muito dessa biblioteca, e o motivo que me fez escolher ela, é a geração automática de uma documentação interativa: ao adicionar `/docs` no final da url, [uma página HTML automática é gerada](https://fastapi.tiangolo.com/#interactive-api-docs-upgrade), com a possibilidade de fazer chamadas aos seus endpoints!

Pra quem já usou `Flask`, a sintaxe do `FastAPI` é bem similar:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}
```

A gente começa importando a biblioteca e instânciando a classe `FastAPI`, que será responsável por todo _boilerplate_ do aplicativo. Aí, é só questão de ir definindo os endpoints usando funções com o decorador `@app.get("/")`, sendo a string o caminho do endpoint e `get` o tipo de interação.

Com isso podemos definir uma página inicial:

```python
@app.get("/")
def home():

    return {
        "Apresentação": """Este é um programa básico para 
        apresentar os conceitos introdutório de MLOps."""
    }
```

E um endpoint que ativa o modelo:

```python
from transformers import pipeline

# https://huggingface.co/citizenlab/twitter-xlm-roberta-base-sentiment-finetunned
MODEL_PATH = "citizenlab/twitter-xlm-roberta-base-sentiment-finetunned"
SENTIMENT_DICT = {"Neutral": "Neutro", "Positive": "Positivo", "Negative": "Negativo"}

@app.get("/sintetize/{text}")
def sintetize(text: str):

    sentiment_classifier = pipeline(
        "text-classification", model=MODEL_PATH, tokenizer=MODEL_PATH
    )

    return {
        "Classificação": SENTIMENT_DICT[sentiment_classifier(text)[0]["label"]],
        "Confiança": f"{100*sentiment_classifier(text)[0]['score'] :.3f}%",
    }
```

Aqui eu estou chamando o `pipeline`, como sugerido anteriormente, e criando um dicionário de retorno com a tradução do sentimento e a confiança normalizada para uma porcentagem com três casas decimais.

O programa é bem simples, de novo, por proposta, mas é mais do que o suficiente para entender os conceitos!

Para executar o código e ser capaz de interagir com o nosso aplicativo, precisamos usar uma das dependências do `FastAPI`, o `uvicorn`. Então, em um terminal na pasta do código, executamos:

```bash
uvicorn app:app --reload
```

### Conteinerizando o código

Tudo que foi construido agora deve ser empacotado em algo homogêneo. Muitos programadores já passaram pela experiência de compartilhar um código com outra pessoa e receber a devolutiva de que não está funcionando como deveria, ou algo assim, ao que a cartilha nos manda responder um sonoro: _"Pois na minha máquina estava funcionado..."_.

Em um ambiente de DevOps, isso simplesmente **não** pode acontecer, se o código de produção e o código local têm comportamentos distintos, os testes e correções locais não podem ser considerados como representativos do ambiente de produção e, portanto, não podem ser implantados de maneira ágil - e isso faz com que a metodologia perca todo o sentido.

Pra garantir essa paridade foram desenvolvidos os conteiners: de forma simplificada são como máquinas virtuais super leves que agregam o código e todas suas dependências, **exatamente da forma como elas foram definidas pelo programador**. Esse é o poder do Docker: independente da máquina, a mesma imagem e o mesmo código têm o mesmo comportamento. Para quem já programa, uma comparação que ajuda a entender a funcionalidade são os ambientes virtuais, como o `venv` do Python, em que você cria um ambiente apartado do "resto" da sua máquina e pode, assim, garantir que não haverão interações danosas entre dependências!

A criação do conteiner é feita através de um arquivo especial, o `Dockerfile`, nele são definidas as intruções necessárias para montar o nosso pacote, _tim-tim por tim-tim_. Inclusive fica mais fácil de entender o `Dockefile` ao pensar nele como uma receita, mesmo!

:ramen: A receita mais simples de macarrão possível:

> Do pacote de macarrão, pegue o macarrão e jogue na água fervendo. 
> Adicione sal à gosto. 
> Deixe um prato à postos. 
> Coloque o macarrão no prato após 5 minutos.

Ou, em um `Dockerfile`, com as [instruções disponíveis](https://docs.docker.com/engine/reference/builder/), seria algo assim:

```Dockerfile
# Começamos com o pacote
FROM pacote_macarrão 

# Vamos "trabalhar" na água em ebulição
WORKDIR /panela-com-água-fervendo

# Manda o macarrão pra água fervendo
COPY macarrão ./panela-com-água-fervendo/ 

# Adiciona 4 pitadas de sal
RUN adicionar-sal --pitadas 4 

# Deixa o prato disponível para receber o macarrão
EXPOSE prato

# Aguarda 05 minutos
RUN sleep 300

# Tira o macarrão da água e o "expõe" no prato
RUN tira-macarrão --colocar-em prato
```

Antes que você fique com fome, na programação nós precisamos começar de alguma base, também. Como disse anteriormente, os conteiners são parecidos com uma máquina virtual, isso significa que a gente começa, normalmente, de um sistema operacional. Algumas **imagens**, nome dado à essa base, são simplificadas para reduzir o tamanho do conteiner com apenas aquilo que é necessário - por exemplo, se você vai trabalhar com Python, você pode usar a imagem do ubuntu ou uma versão simplifica de um sistema linux com os principais componentes do Python já instalados - a única diferença é o tamanho.

Eu optei por trabalhar, justamente, com essa [imagem simplificada do Python](https://github.com/docker-library/python/blob/8bc80d1109001365559eded16423ba3692eff1ff/3.11/slim-bullseye/Dockerfile):

```Dockerfile
# Imagem 3.11 bullseye do Python (versão enxuta)
FROM python:3.11-slim-bullseye

# Move todos os arquivos para a pasta "mlops" do sistema operacional do conteiner
WORKDIR /mlops

# Copia os arquivos para dentro do conteiner
COPY . /mlops/

# Instala as dependências do código
RUN pip install --no-cache-dir --upgrade -r /mlops/requirements.txt

# Expõe a porta 8000
EXPOSE 8000

# Roda um comando para iniciar o uvicorn 
CMD ["uvicorn", "main:app", "--host",  "0.0.0.0", "--port", "8000"]
```

Com isso as instruções foram montadas, agora sempre que eu quiser replicar esse aplicativo, onde quer que seja, basta eu usar ela e pronto!

O que eu estou dizendo, em linguagem simples, é:

> Em uma imagem simplificada e leve do Python, crie a pasta "mlops", copie todos os arquivos para ela, instale as dependências e exponha a porta 8000. Em seguida execute o `uvicorn` para ativar o webserver.

### Github (+ actions)

Então, chegamos àquele momento gostoso no fim de todo código:

```bash
git add .
git commit -m "mensagem super significativa"
git push
```

Mas, até o momento, tudo que temos é uma vitrine para as nossas habilidades em Python. Para elevar o nosso projeto às alturas (_risos_), precisamos colocá-lo na núvem. :cloud: 

Existem muitas ferramentas que fazem esse processo de pegar um código de um repositório e enviá-lo para núvem de forma automática, sempre que houverem mudanças, como Jenkins, Gitlab ou o próprio Build, da Google. Eu escolhi a solução própria do Github, o `Actions`, visto que vou hospedar meu código lá. Mas o fluxo de trabalho é bem parecido, independente da solução: mais uma vez nós criamos uma receita, mas dessa vez seria como as instruções para mandar o macarrão para a mesa de um cliente, usando um outro tipo de arquivo, o `.yml`.

```yaml
steps:

    # Deixa o macarrão disponível ao garçom
    - name: Pega o macarrão
      usa: garçom 

    # Usa a comanda para "autentificar" o número da mesa
    - name: Verifica o número da mesa do cliente
      usa: comanda
      executa: |
        NUMERO_MESA=ler-comanda

    # Entrega o macarrão pra mesa correta
    - name: Entrega macarrão ao cliente
      usa: garçom
      executa: |
        entrega-macarrão --mesa $NUMERO_MESA
```

Infelizmente o `.yml` não se serve tão bem à simplicidade do exemplo quanto o `Dockerfile`, então não vou colocar [o fluxo completo](https://github.com/Pfalcao97/mlops-example/blob/main/.github/workflows/google-cloudrun-docker.yml) aqui no texto, mas, a ideia geral é a seguinte:

1. Definimos que essa instrução será executada toda vez que houver uma alteração na `main` - ou seja, no "galho" principal do repositório.
2. Tudo será executado em uma máquina do próprio Github, com o ubuntu instalado.
3. Pegamos o código do repositório.
4. Rodamos alguns testes unitários.
5. Instalamos a ferramenta CLI do Google Cloud (`gcloud`).
6. Subimos o código para o **Artifact**.
7. Criamos uma **Cloud Run** a partir da imagem que está no **Artifact**.

Aqui você vai notar que eu estou citando pela primeira vez os testes unitários. Como eu comentei quando defini a prática de **DevOps**, monitoramento e testes são fundamentais para o desenvolvimento de uma aplicação robusta. Neste projeto eu fiz uso de duas ferramentas que auxiliam o desenvolvedor a criar os testes: `pytest`, para testes unitários, e `pylint`, para testes de síntaxe. 

O `pylint` é um programa que você roda enquanto está codificando, ele vai checar a síntaxe do seu código e prever alguns problemas "óbvios", do tipo: "Você está usando essa função, mas ela não é importada ou definida em lugar nenhum!". Além disso, ela enforça um padrão de qualidade no código, como a criação de `docstrings` junto à definição de uma função.

O `pytest`, por outro lado, é usado para criar testes unitários, que são testes individualizados para o funcionamento de partes específicas do seu código, o que pode complementar muito bem o `pylint` para garantir a robustez do programa. A criação de testes unitários funciona da seguinte forma: imagine que você criou uma função que soma 2 números inteiros:

```python
def soma(a:int, b:int) -> int:
    return a+b
```

Se o seu código está correto, você espera que, por exemplo, ao fazer 2+2, você encontre quatro. Então, você diz ao código: "Se assegure de que a resposta da função `soma`, quando eu coloco os parâmetros 2 e 2, seja 4". A forma de fazer isso no Python é através da palavra chave `assert`:

```python
def test_soma():
    assert soma(2,2) == 4
```

Ao rodar o script nesse novo arquivo criado, se o seu teste passar, você pode se garantir de que o código está correto para os usos previstos. Por isso a criação de um processo de testagem extenso é tão importante, quanto mais casos propensos à falha você testar e passar, mais certeza pode ter na qualidade e robustez do código!

> _Mas, Pedro, se é tão importante assim, por que você está falando disso só agora?!_ 

Bem... é que os testes do meu código foram forçados a dar certo. :disappointed_relieved:

Como eu disse, o meu código é extremamente simples e não tem muito como eu testá-lo de forma significativa sem ativar a API do Hugging Face, que é bastante pesada, então achei melhor [criar um teste bobo](https://github.com/Pfalcao97/mlops-example/blob/main/tests/test.py), só pra mostrar como funciona isso na esteira. Mas, sem dúvidas, se você quer fazer código de qualidade e que será implantado em produção, você deve estudar `pytest` e testes unitários à fundo!

Depois de 7 minutos de ansiedade (costuma ser bem mais rápido, mas, como eu disse, a API do Hugging Face é pesada :grimacing:), nosso código saiu do nosso computador e está na Cloud Run, para que todos possam acessar!

# Conclusão

Ufa, que jornada, ein?

Eu sempre digo que vou fazer um texto mais curto da próxima vez, para tentar postar com mais frequência, mas aí eu decido fazer um projeto de algumas semanas, com conceitos que não tenho tanta familiaridade e muitas palavras. :sweat_smile:

DevOps, porém, é um tema que eu tenho me aprofundado cada vez mais, seja no contexto de Machine Learning, como aqui no texto, ou nos contextos de dados e software, e esse projeto me permitiu estudar um pouco mais de Docker e principalmente a parte de implantação automática na núvem - que eu penei muito para fazer!

Creio que eu não fui capaz de ilustrar todos os principais conceitos necessários para colocar um modelo na núvem de forma segura e robusta, mas, acho que consegui introduzir o tema, afinal, essa aplicação que eu criei já está disponível e pode ser acessada por qualquer pessoa, podendo ser útil em diversos contextos. Claro, não vou disponibilizar o link de acesso, pois eu respeito minha carteira e não quero ninguém enchendo a URL de chamados que serão prontamente cobrados ao fim do mês pela Google, mas, em um contexto empresarial, essa aplicação poderia ser disponibilizada facilmente e o modelo estaria perfeitamente utilizável!

Existe toda uma parte de treinamento, validação cruzada e verificação que, ao usar um modelo pronto, eu não precisei me preocupar, mas na maioria dos casos, quando estamos resolvendo problemas reais, são a parte que mais importa. Apesar disso, existem pilhas e mais pilhas de conteúdo para quem quer aprender a treinar modelos do zero, enquanto que a parte de implantação e MLOps, como um todo, ainda não é tão difundida, principalmente em português. Nesse sentido, acredito ter cumprido com o que me propus, mas, de qualquer forma, fico à disposição caso surja alguma dúvida, vamos continuar conversando [lá no meu LinkedIn](https://www.linkedin.com/in/pfalcao97/)!

# Citações

\[1\] [ChatGPT sets record for fastest-growing user base - analyst note](https://www.reuters.com/technology/chatgpt-sets-record-fastest-growing-user-base-analyst-note-2023-02-01/), Krystal Hu (Reuters), 02/02/2023 (Acesso em 26/12/2023).

\[2\] [A lawsuit claims OpenAI stole 'massive amounts of personal data,' including medical records and information about children, to train ChatGPT](https://www.businessinsider.com/openai-chatgpt-generative-ai-stole-personal-data-lawsuit-children-medical-2023-6?op=1), Grace Dean (Business Insider), 29/07/2023 (Acesso em 26/12/2023).

\[3\] [Child sex abuse images found in dataset training image generators, report says](https://arstechnica.com/tech-policy/2023/12/child-sex-abuse-images-found-in-dataset-training-image-generators-report-says/), Ashley Belanger (Arstechnica), 12/20/2023 (Acesso em 26/12/2023).

\[4\] [Is A.I. Art Stealing from Artists?](https://www.newyorker.com/culture/infinite-scroll/is-ai-art-stealing-from-artists), Kyle Chayka (The New Yorker), 10/02/2023 (Acesso em 26/12/2023).

\[5\] [Sharing sensitive business data with ChatGPT could be risky](https://www.csoonline.com/article/574799/sharing-sensitive-business-data-with-chatgpt-could-be-risky.html), Michael Hill (CSO), 22/03/2023 (Acesso em 26/12/2023).

\[6\] [A Chevrolet dealer offered an AI chatbot on its website. It told customers to buy a Ford](https://www.usatoday.com/story/money/cars/2023/12/19/chevy-of-watsonville-chatgpt-use/71976591007/), Phoebe Wall Howard (USA Today), 19/12/2023 (Acesso em 26/12/2023).

\[7\] [A história dos edifícios tortos de Santos](https://www.archdaily.com.br/br/961668/a-historia-dos-edificios-tortos-de-santos), Giovana Martino (Arch Daily), 18/04/2022 (Acesso em 26/12/2023).

# Fontes

[What is DevOps?](https://azure.microsoft.com/en-us/resources/cloud-computing-dictionary/what-is-devops/), Microsoft (Acesso em 26/12/2023).

[What is DevOps?](https://aws.amazon.com/devops/what-is-devops/), AWS (Acesso em 26/12/2023).

Practical MLOPs, **Noah Gift** e **Alfredo Deza**, 2021.

[Docker Documentation](https://docs.docker.com/reference/), Docker (Acesso em 03/01/2024).

[Google Cloud Documentation - Deploying to Cloud Run](https://cloud.google.com/run/docs/deploying#command-line_1), Google (Acesso em 05//01/2024).

[FastAPI Reference](https://fastapi.tiangolo.com/reference/), FastAPI (Acesso em 03/01/2024).