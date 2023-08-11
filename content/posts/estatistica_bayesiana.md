---
author: "Pedro Falcão"
title: "O prognóstico por representatividade"
date: "2023-08-09"
description: "As armadilhas anti-intuitivas da matemática"
tags: ["Estatística", "Matemática", "Economia"]
categories: ["Estatística"]
aliases: ["fast-slow-bayes"]
--draft: true

cover:
    image: "https://github.com/Pfalcao97/blog/blob/main/static/cover_bayes.png?raw=true"
    alt: "A estatística bayesiana"
---

## Introdução

A matemática é lógica, por definição. Mas isso não significa que ela seja óbvia e, muito menos, intuitiva. Inclusive, muitas vezes, ela é tão pouco óbvia e intuitiva, que nos parece não-lógica. E isso, por si só, é nada intuitivo!

Tanto é, que existe um campo de estudo inteiro dedicado a tratar o ser humano como ente plenamente lógico e racional - a economia - que foi virado de cabeça pra baixo quando dois psicólogos chegaram e falaram _"Ei, galera, não é bem assim..."_. A revolução foi tão profunda que mereceu até um prêmio Nobel.

Hoje eu quero falar um pouco de algumas falácias lógicas que nos levam a conclusões erradas, a partir de premissas enviesadas, mesmo sem perceber, e uma ferramenta que pode nos ajudar a interpretar o mundo nessas condições, a estatística bayesiana.

## Lógica

Definição do minidicionário da língua portuguesa, Silveira Bueno:

> **Lógica:** _(Substantivo feminino)_
>
>  Ciência que estuda as leis do raciocínio; coerência; raciocínio.


Existem muitas palavras que têm uma definição clara, porém o uso no dia-a-dia acaba raptando e transformando seu significado. Lógica é uma dessas. A expressão _"É lógico"_, ao invés de significar _"tem coerência com as premissas básicas adotadas no seu raciocínio"_, é normalmente utilizada para definir algo que é óbvio, elementar.

Mas essas duas coisas (o óbvio e o coerente) não têm, necessariamente, algo em comum. Não me leve a mal: **deveriam ter**, mas, quantas vezes você já teve certeza de algo? Pode ser algo que compõe fundamentalmente sua maneira de ver o mundo, como uma ideologia política, ou algo trivial, como a certeza de que seu amigo está te evitando.

E quantas dessas vezes a certeza se desintegrou depois de uma explicação? Seu amigo não estava te evitando, ele só estava ocupado no trabalho e, portanto, não conseguiu responder os 20 _tik toks_ que você mandou para ele no período de 2 horas, no meio da tarde. 

A própria prática da psicoterapia está muito calcada nisso: às vezes, só de colocar seus pensamentos (que parecem óbvios e corretos) em palavras, você consegue identificar premissas erradas e saltos lógicos. Mas nem sempre esse reconhecimento é fácil.


Imagine a seguinte descrição de uma mulher, Linda:

> Linda tem 31 anos de idade, é solteira, franca e muito inteligente. É formada em filosofia. Quando era estudante, preocupava-se profundamente com questões de discriminação e justiça social, e também participava de manifestações antinucleares.


Após ler essa descrição, você deve inferir qual sobre a sua situação atual: Será que ela trabalha como professora? Será que é banqueira, ou trabalha numa livraria? Ela participa do movimento feminista? Ela faz ioga?

Diante de várias opções, muitas pessoas atribuem uma probabilidade maior a Linda ser uma banqueira feminista do que apenas uma banqueira. Ou seja, o estereótipo criado na sua descrição, uma pessoa mais alinhada com ideais progressistas, é tão forte que faz com que muitos ignorem a regra lógica de que **para ser feminista e banqueira, ela deve, necessariamente, ser banqueira**, por mais que te pareça muito mais provável que ela seja feminista, do que banqueira.

Assim surge a **falácia da conjunção**: o ato de considerar eventos conjugados como mais prováveis do que qualquer um dos eventos individuais que o compõe, em uma comparação direta. Ninguém diz que é mais provável tirar cara e depois coroa, ao lançar uma moeda duas vezes, do que tirar só cara, ao lançar uma única vez, mas quando damos uma contextualização maior ao problema, a estatística se confunde com os nossos vieses.

Essa foi uma das descobertas da dupla de psicólogos, Daniel Kahneman e Amos Tversky. 

Ao longo de uma frutífera carreira científica, eles desenvolveram inúmeros experimentos e, com eles, artigos, que iriam quebrar muitos dos paradigmas das ciências comportamentais. Seus trabalhos são extremamente influentes até hoje e foram premiados com o Nobel de Economia em 2002 - Por conta do falecimento de Amos em 1996, Daniel recebeu o prêmio sozinho.


## Rápido e Devagar :pencil2:

Mais recentemente, Daniel escreveu o livro Rápido e Devagar, que virou um _best-seller_, contando sobre o seu entendimento do funcionamento da mente à partir de todo arcabouço que a psicologia cognitiva desenvolveu desde então - sempre muito bem ilustrado com exemplos tirados diretamente dos seus experimentos, como é o caso da Linda!

Também é o caso do experimento que me motivou a escrever o texto: **A escolha profissional de Tom W.**.

Em alguns lugares no exterior, como nos Estado Unidos, ao final do Ensino Médio, é comum que professores escrevam cartas de recomendação para universidades, descrevendo as qualidades de um aluno particular - Esperançosamente motivando sua entrada em tal universidade. Eis um trecho de uma dessas cartas que foi escrita para um aluno em particular, Tom W.:

> Tom é dotado de grande inteligência, embora careça de criatividade genuína. Tem necessidade de ordem e clareza e de sistemas claros e ordenados em que cada detalhe encontre seu lugar apropriado. Seu texto está mais para maçante e mecânico, animado ocasionalmente por alguns trocadilhos batidos e lampejos de imaginação do tipo ficção científica. Exibe forte compulsão por competência. Parece apresentar pouca compreensão e pouca simpatia pelas outras pessoas,e não aprecia a interação com os outros. Autocentrado, exibe no entanto um profundo senso moral.

Não há nada de especial em Tom, ele foi um aluno escolhido aleatoriamente em uma universidade, para exemplificar o experimento. A universidade que ele estuda oferece os seguintes cursos:

- Administração
- Ciência da computação
- Engenharia
- Humanidades e educação
- Direito
- Medicina
- Biblioteconomia
- Ciências físicas e biológicas
- Ciência social e assistência social

Se você tivesse que ordenar os cursos acima, do mais provável para o menos provável de ser o curso de Tom, como ficaria sua lista?

Supondo que você tenha realmente listado (e eu espero que sim, pois é uma boa forma de entender o que será explicado a seguir), pode ter acontecido duas coisas:

1. Você ordenou com base no estereótipo do curso e no texto do professor.
2. Você não ordenou, porque está faltando uma informação.

Rápido e devagar é como Daniel descreve os dois sistemas que agem no nosso cérebro, o sistema 1 é rápido e automático, o sistema 2 é lento e deliberado. Se você respondeu da primeira forma, você usou o sistema 1 para _substituir_ a pergunta, ao invés de responder a pergunta (difícil) da probabilidade por curso, o seu cérebro escolheu responder a pergunta (fácil) de _"qual desses cursos tem o estereótipo que mais corresponde com a personalidade descrita?"_. Enquanto que, para responder a pergunta corretamente, seria necessário saber as taxas de incidência de cada curso na universidade, que é o que você deve ter pensado, caso tenha respondido da segunda forma - usando seu sistema 2!

E antes de você considerar a conclusão acima injusta, veja a seguinte questão, bastante similar:

> Eu tenho um pote com 100 bolinhas, na seguinte distribuição: 10 bolinhas vermelhas, 40 bolinhas verdes e 50 bolinhas azuis. Ordene as cores da mais provável para a menos provável de ser a cor de uma bolinha tirada ao acaso.

Se você tiver um conhecimento básico de probabilidade, deve ter feito a seguinte lista: Azul (50% de probabilidade), Verde (40%) e Vermelho (10%). Por que com Tom W. seria diferente? Você nem sabe se a descrição do professor é certeira!

O **prognóstico por representatividade** é a forma como nós focamos nas representações individuais, em detrimento da distribuição de ocorrência do fenômeno. É claro que a descrição individual pode fornecer informações importantes, mas ainda assim, ao invés de **substituir** a probabilidade basal, o ideal é que ela a **atualize**.

## A estatística Bayesiana

Vamos voltar ao exemplo das bolinhas. A distribuição das cores no pote é a mesma, mas, dentre as bolinhas vermelhas, 80% têm uma linha preta desenhada, dentre as verdes, 40% têm a mesma linha, e dentre as azuis, apenas 20%. 

Se eu adicionar que a bolinha tirada também possui o mesmo desenho, como isso muda a ordem de cores mais prováveis? Bom, vamos fazer uma contagem:

1. Existem 8 bolinhas vermelhas com listras (80% de 10).
2. Existem 16 bolinhas verdes com listras (40% de 40).
3. Existem 10 bolinhas azuis com listras (20% de 50).

Então, agora nosso espaço amostral não é mais 100, mas 34 (8+16+10), e a ordem deve ser: Verde (47% de probabilidade), Azul (29,5%) e Vermelho (23,5%). A presença da listra não descartou completamente a distribuição de cores anterior - **apenas atualizou as probabilidades**. O mesmo deveria ter acontecido com Tom.

Digamos que no ano de entrada de Tom na faculdade existam 200 alunos de administração e 20 de Engenharia, por mais que a descrição de Tom seja compatível com 100% dos estudantes de engenharia (_nota do autor: posso confirmar_) e apenas 10% dos estudantes de administração - **a probabilidade ainda é 50/50**, porque representam 20 estudantes de engenharia e 20 de administração!

É justamente esse princípio básico que norteia toda a **estatística bayesiana**.

Thomas Bayes foi um pastor presbiteriano inglês que viveu no século XVIII. Entre rezas, o professo desenvolveu uma teoria para a probabilidade condicional, ou seja, como saber a probabilidade de que algo aconteça, sabendo que uma outra coisa já aconteceu. Formulando matematicamente o **Teorema de Bayes**, ficaria assim:

$$ P(A|B) = \frac{P(B|A) P(A)}{P(B)}$$

Ou, em português: A probabilidade de que aconteça o evento A, sabendo que já aconteceu o evento B, P(A|B), é igual à probabilidade de que aconteça A, P(A), vezes a probabilidade de que aconteça B, dado que A já aconteceu, P(B|A), dividido pela probabilidade de que aconteça B, P(B). Podemos pensar na fórmula como uma proporção: De todas as vezes que B acontece, quantas acontecem depois de A? Pegue esse número e multiplique pela probabilidade de A, chegando assim ao valor atualizado!


Pode parecer confuso, explicando assim, mas a gente acabou de fazer esse exato exemplo, com as bolinhas coloridas.

Vamos reorganizar o enunciado, para ficar mais claro:

> Qual a probabilidade de que uma bolinha com listras, escolhida aleatoriamente do pote, seja azul?

Aplicamos Bayes!

1. **P(azul|listra):** esse é o valor que queremos encontrar, a probabilidade de que uma bolinha listrada seja azul.
2. **P(azul):** anteriormente vimos que, das 100 bolinhas no pote, 50 são azuis. Portanto essa probabilidade é 50%.
3. **P(listra|azul):** aqui, queremos saber qual a probabilidade de que uma bolinha azul seja listrada. Isso também foi dado pelo problema, como apenas 20% das bolinhas azuis são listradas, essa probabilidade é, também, 20%.
4. **P(listra):** As bolinhas listradas são 8 vermelhas, 16 verdes e 10 azuis - 34 no total. Portanto, dividimos pelo número total de bolinhas, 100, e encontramos que essa probabilidade é de 34%.

Basta colocar tudo na fórmula acima e _voilà_!

$$ P(azul|listra) = \frac{P(listra|azul) P(azul)}{P(listra)} = \frac{0,2 \times 0,5}{0,34} = 0,294 $$

Parabéns, você acabou de _reencontrar_ o mesmo número, só que de forma muito mais difícil. Brincadeiras a parte, não é a toa que o teorema de Bayes é chamado de **"bom senso aplicado à matemática"**, é uma boa forma de garantir que seu pensamento é coerente - particularmente nos casos mais complicados de fazer uma verificação simples!

{{< figure align="center" src="https://imgs.xkcd.com/comics/frequentists_vs_bayesians_2x.png" width="auto" >}}

[**XKCD Nº 1132: Frequentistas vs. Bayesianos.**](https://xkcd.com/1132/) _Verdadeiramente, bom senso aplicado._

Um bom caso de estudo para esse ramo da estatística está relacionado aos **testes médicos**, como os testes de COVID, que tanto nos acostumamos a fazer nos últimos anos. Mas não é só nisso que o bom pastor pode te ajudar, ele também pode aumentar suas chances de conseguir um carro novo*!

\* _Essa situação é bastante específica, qualquer não obtenção de um veículo novo não é responsabilidade do autor ou do pastor._

### Como interpretar um teste positivo?

Temos o costume de ver testes médicos como sentenças: Se o resultado deu positivo, nós temos a doença ou condição que o exame estava testando. Mas isso não é tão preto no branco - tanto é que médicos normalmente pedem para você repetir exames com resultados inesperados, em especial para condições que tenham consequências mais graves.

Normalmente, nós só pensamos em um único número, a acurácia do teste, mas, no fundo, durante os testes clínicos são computados quatro números relevantes:


|          | **Verdadeiro** | **Falso** |
|:--------:|:----------:|:-----:|
| **Positivo** |     O teste identificou a doença corretamente    |   O teste identificou a doença erroneamente  |
| **Negativo** |     O teste não identificou a doença quando não deveria    |  O teste não identificou a doença quando deveria   |

Em especial, focamos em testes sensíveis: **É melhor um alarme falso, resolvido facilmente com um segundo teste, do que não identificar casos em que a doença está presente, agravando o quadro**. Isso gera um número de **Falsos Positivos** comumente maior do que o número de **Falsos Negativos** e, ambos, se tudo estiver certo, bastante menores do que os resultados verdadeiros.

Pense em uma doença que afete 1 a cada 1000 pessoas no seu grupo - Doenças raramente podem ser tratadas como homogêneas na população. Poderíamos, de forma simplificada, assumir que você tem 1/1000 chances de pegar a doença, portanto a probabilidade de você estar com a doença é de 0,1%. Mas a médica que está te atendendo é uma pessoa cautelosa, e te pede para fazer um teste mesmo assim.

Lendo o manual de uso, você vê que os testes clínicos foram feitos em 2000 pessoas, das quais 50 estavam doentes. Nesses testes, a sensibilidade foi de 96% e a especificidade foi de 90%. Após uma rápida pesquisa, você vê que **sensibilidade** é uma outra palavra para o **Verdadeiro Positivo** e **especificidade**, outra palavra para **Verdadeiro Negativo**, então, você reconstrói a tabela com os números do teste:

|          | **Verdadeiro** | **Falso** |
|:--------:|:----------:|:-----:|
| **Positivo** |     0,96 (Sensibilidade)    |  0,1  (1 - Especificidade) |
| **Negativo** |     0,90 (Especificidade)    |   0,04 (1 - Sensibilidade)  |

Note que a probabilidade de ser verdadeiro ou falso deve ser 100%, por isso que a taxa de falsos negativos deve ser 100% menos a sensibilidade. Isso pode ficar mais claro usando números: Se o teste tem uma taxa de verdadeiros positivos de 96%, significa que das 50 pessoas que estão doentes, o teste irá identificar corretamente 48 e deixará de identificar 2, fazendo com que essas sejam as falsas negativas, conta similar ocorre para a especificidade.


|          | **Verdadeiro** | **Falso** |
|:--------:|:----------:|:-----:|
| **Positivo** |     48  pessoas  |  195 pessoas |
| **Negativo** |     1.755  pessoas  |   2 pessoas |

Depois de fazer o _mise en place_, você faz o teste e verifica que ele dá positivo. Em qual grupo você está? No das 48 pessoas que realmente têm a doença, ou na das 195 que não têm? Bom, isso quem irá dizer é a profissional da saúde, mas você pode aquietar um pouco a ansiedade, atualizando aquela probabilidade de 0,1% usando a fórmula de Bayes: Qual a probabilidade de que você esteja doente, ao receber um teste positivo, P(doente|positivo)?

1. **P(doente):** essa é a probabilidade que chamamos _à priori_, ou seja, antes do teste. Nesse caso era de 0,1%.
2. **P(positivo|doente):** Aqui tratamos da probabilidade de, estando doente, seu teste ser positivo - Em outras palavras, a sensibilidade do teste: 96%.
3. **P(positivo):** Por fim, a probabilidade do teste dar positivo. Aqui é útil a construção da tabelinha: Dos 2.000 voluntários, 48 foram verdadeiros positivos e 195, falsos positivos, portanto a probabilidade é (48 + 195)/2000, 12,15%.

Assim, aplicando os números à fórmula:


$$ P(doente|positivo) = \frac{P(positivo|doente) P(doente)}{P(positivo)} = \frac{0,96 \times 0,001}{0,1215} = 0,0079 $$

Ou seja, embora o teste aumente em quase 8 vezes sua chance de ter a doença, a grande proporção de falsos positivos para verdadeiros positivos significa que, ao positivar o teste, a probabilidade de ser um resultado falso é alta. Significando que não é uma sentença, mas uma **atualização de probabilidade**. Claro, se você continuar fazendo testes e o resultado continuar sendo positivo, sua chance de ter a doença aumenta bastante: No segundo positivo, 6,24% e no terceiro, 49,3%. Isso motiva a realização de mais testes!

### O problema de Monty Hall

Agora, vamos falar do tal carro novo. Mas, antes, eu preciso te preparar: acontece que a situação a seguir é polêmica - causou acaloradas cartas e reações de professores de estatística, quando foi enunciada pela primeira vez, e causou uma acalorada discussão entre eu e minha namorada, quando eu contei pra ela sobre o problema. Prossiga com cuidado :warning:.

Tudo começa com um quadro de perguntas e respostas em uma revista americana chamado _Ask Marilyn_. Marilyn vos Savant, reverenciada pelo seu QI altíssimo, respondia dúvidas gerais dos leitores, pense em algo como um _google-antes-do-google_. Em certo momento chega a seguinte pergunta, inspirada em um famoso programa de TV, _Let's make a Deal_, apresentado por um carismático _host_, Monty Hall:

> Suponha que os participantes de um programa de auditório recebam a opção de escolher uma dentre três portas: atrás de uma delas há um carro; atrás das outras, há cabras. Depois que um dos participantes escolhe uma porta, o apresentador, que sabe o que há atrás de cada porta, abre uma das portas não escolhidas, revelando uma cabra. Ele diz ao participante: "Você gostaria de mudar sua escolha para a outra porta fechada?". Para o participante, é vantajoso trocar sua escolha?

Tudo que _intuitivamente_ sabemos sobre probabilidades, nos diz o seguinte: 

1. Há chances iguais do prêmio estar nas três portas, portanto, a probabilidade de prêmio é de 33,3% para cada porta.
2. Uma vez que uma das portas é eliminada, sobram duas portas com iguais probabilidades, portanto, cada uma delas tem uma probabilidade de 50% de conter o prêmio.
3. Como há 50% de chance de acertar a porta, não é vantajoso trocar, nem se manter - ambas opções apresentam o mesmo benefício.

Mas - lembrando que a matemática nem sempre é intuitiva - Marylin causou muita confusão ao afirmar que é mais vantajoso trocar a escolha. E não é por pouco, não: **você tem 2 vezes mais chance de ganhar o carro, se trocar a porta escolhida**. Antes que você, como muitos outros, comece a xingá-la, dizendo que ela não sabe de nada, saiba que sua afirmação está, na verdade, correta. E a estatística Bayesiana pode te ajudar a entender o porquê.

Tudo começa com uma premissa básica: O apresentador, Monty Hall, não está abrindo portas ao acaso. Se fosse o caso, ele correria o risco de abrir a porta do carro, sem querer, o que tira toda a graça do jogo. Então nós precisamos nos atentar ao fato de que ele sabe onde está o carro. Embora isso não mude a percepção intuitiva do problema, pois ainda existem duas portas e um único prêmio, quando colocamos na ponta do lápis, esse detalhe fica bastante relevante.

Suponha que o participante escolheu a porta 2, ao passo que o apresentador abriu a porta 3, mostrando uma cabra pacientemente comendo um pouco de palha. Como a probabilidade do carro estar atrás da porta de número 1 se altera, diante da nova evidência da porta 3- P(p1|p3)?

1. **P(p1):** Começamos com chances iguais para as três portas, então mantemos esse valor - a probabilidade de que o carro esteja atrás da porta 1 é de 33,3%.
2. **P(p3|p1):** Aqui está o pulo do gato, qual a probabilidade de que a porta 3 seja aberta, caso o prêmio esteja atrás da porta 1? 

    - O apresentador não pode abrir a porta 2, que você escolheu, porque é contra as regras.
    - O apresentador não pode abrir a porta 1, pois o prêmio está nela e ele não quer revelá-lo.

    Nesse caso, a única porta que pode ser aberta é a 3, ou seja, essa probabilidade é 100%!
3. **P(p3):** Ignorando agora a localização do prêmio, qualquer uma das duas portas não escolhidas poderiam ser abertas, fazendo com que a probabilidade da porta 3 ser aberta seja 50%.

Compilando isso tudo na nossa querida formuleta:

$$ P(p1|p3) = \frac{P(p3|p1) P(p1)}{P(p3)} = \frac{0,333 \times 1}{0,5} = 0,666 $$

Como vemos, a probabilidade de você ter errado, e o prêmio estar atrás da porta de número 1, é de 66,6%, portanto é **mais vantajoso aceitar a troca, ao ser perguntado pelo apresentador**. Fazendo as contas para o caso contrário (P(p2|p3)), a probabilidade da porta 3 ser aberta é de 50%, pois o apresentador pode abrir qualquer uma das duas livremente, fazendo assim com que sua chance seja de 1 em 3 - 33,3%.

Note que esse é um caso particular do teorema. Quando enunciei a fórmula pela primeira vez, a descrevi como uma proporção do evento B, nesse caso específico em que o evento A não influencia o evento B, o evento B também não influencia o evento A - fazendo que que P(B|A) e P(B) se cancelem e P(A|B) seja igual à P(A). Até nisso Bayes foi bem sucedido em formular o bom senso matemáticamente, afinal, não é qualquer coisa que influencia as probabilidades. Não é só porque o [Nicolas Cage lançou um filme novo, que mais pessoas vão morrer afogadas](https://www.nationalgeographic.com/science/article/nick-cage-movies-vs-drownings-and-more-strange-but-spurious-correlations) (o que não siginifica que não haja uma correlação entre as duas coisas, mas isso é um papo para outro momento).


## Conclusão

Viver como um ser lógico e estóico não é muito simples, nossa própria mente prega truques ao tentar poupar energia usando o Sistema 1 para responder rapidamente os dilemas do dia-a-dia. Uma boa forma de ativar o Sistema 2 é tentando adicionar a lógica formal e a matemática no seu raciocínio, e o teorema de Bayes é particularmente bom nisso.

Hoje, inclusive, existe quase que um culto à filosofia bayesiana de vida, algumas pessoas realmente norteiam suas vidas usando esse conceito da estatística.

Não é necessário, no entanto, se inscrever numa seita para tomar decisões melhores, ou interpretar resultados de forma mais correta, usando o teorema. Uma compreensão de que **novas evidências atualizam probabilidades antigas, ao invés de substituí-las** pode nos levar muito longe, ajudando a desmistificar os viéses cognitivos da nossa mente!

## Fontes

[Bayes theorem, the geometry of changing beliefs](https://www.youtube.com/watch?v=HZGCoVF3YvM), **3Blue1Brown**, 2019-12-22 (Acesso em Agosto de 2023).

Rápido e devagar: duas formas de pensar, **Daniel Kahneman**, 2012.

O andar do bêbado, **Leonard Mlodinow**, 2009.