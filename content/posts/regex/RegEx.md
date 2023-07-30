---
author: "Pedro Falcão"
title: "Expressões Regulares"
date: "2023-07-08"
description: "Como e quando usar RegEX?"
tags: ["RegEx", "Python", "SQL"]
categories: ["Programação"]
aliases: ["regex-deepdive"]
---

## Introdução

Quase todo mundo que está inserido no mundo da programação conhece sobre Expressões Regulares, mas, tão frequente quanto, o conhecimento fica só no "sobre", mesmo: O modo de escrita, que beira a criptografia, e a falta de repertório para os casos de uso geram um sentimento generalizado de que RegEx é ~basicamente~ mágica. Mas, diferente de mágica "de verdade", conhecer o segredo e estragar a ilusão pode ser muito mais vantajoso.

Claro, existem necessidades diferentes, que vão desde escrever uma expressão simples para _debugar_ o código no seu editor, até fazer a extração de informações em dados não estruturados, passando por melhorar a legibilidade de uma query no SQL. O que importa é que, com um pouco de [_sitzfleisch_](https://www.bbc.com/worklife/article/20180903-to-have-sitzfleisch---its-a-professional-compliment), você pode adicionar mais uma ferramenta no seu arcabouço e se tornar mais produtivo.

A ideia do texto é apenas molhar seus pés nesse universo, desmistificando o dialeto místico que separa os juniors dos sêniors, a partir de uma compreensão mais palatável.

## Puxando a cortina

Para continuar a análogia da mágica, vamos puxar a cortina no meio do espetáculo, tal qual o Mr. M. De forma simples, aquelas instruções escritas em caractéres aparentemente aleatórios são instruções para definir uma máquina de estados finita, basicamente um circuito lógico com estados. Uma boa forma de entender esse conceito é com o exemplo disponível na [página da Wikipédia](https://en.wikipedia.org/wiki/Finite-state_machine): Uma catraca.

O **estado** "natural", ou inicial, da catraca é travada, você pode tentar empurrar, mas ela continua travada. A partir do momento que você passa o seu cartão com créditos suficiente, a catraca é destravada. Ela fica nesse **estado** até que você empurre ela, fazendo com que ela retorne ao **estado** natural.

{{< figure align="center" src="https://github.com/Pfalcao97/blog/blob/main/static/images/finitestatemachine.png?raw=true" >}}

Tá, mas, **como nós transportamos essa ideia para identificar um texto?**

Vou responder isso usando um exemplo: suponha que você deseje criar uma dessas máquinas de estados finita que reconheça palavras entre aspas. Nesse caso, nós teremos três estados:

1. **Estado inicial:** Vamos percorrer nosso texto, caracter a caracter, começando aqui. Esse estado indica, efetivamente, que estamos começando uma nova busca. Só iremos passar pro próximo, caso encontremos o caracter " (aspas duplas).
2. **Estado intermediário:** Encontramos o início, agora precisamos achar o fim. Tudo que for letra encontrada pelo caminho não altera o estado, a única coisa capaz de fazer com que nós passemos de estado é, novamente, o caracter ".
3. **Estado final:** Se chegamos até aqui, significa que tivemos um _match_, ou seja, encontramos a combinação de caracteres capaz de percorrer todo o nosso algoritmo e podemos retornar esse texto.

Para exercitar a imaginação, eu gosto de pensar em um setinha em cima das letras, conforme percorremos o texto. A seta indica qual estado estamos, dizendo algo do tipo _"Isso é um A, não é um caracter que muda o meu estado"_ ou _"Epa, encontrei uma aspa dupla, muda de estado e vamos esperar para encontrar a próxima"_, como na ferramenta [Regexp::Debugger](https://metacpan.org/pod/Regexp::Debugger). **\[1\]** 


Programaticamente, poderíamos fazer algo similar da seguinte forma:

```python
estado = 1 # Estado inicial
match = '' # Nosso match

for caracter in texto: # Faz um loop, letra a letra, pelo texto

    if estado == 2:       # Caso estejamos no estado intermediário,
        match += caracter # adiciona o caracter à variável de match

    if caracter == '"': # Se encontrar uma aspa dupla,                       
        estado += 1     # muda de estado

        if estado != 3:       # A menos que o estado seja o final,
            match += caracter # adiciona o caracter à variável de match
```

Embora essas representações deixem a intenção mais clara, a dificuldade de desenhar diagramas de máquinas de estados finitas para cada caso (Particularmente os mais complexos) se tornou um problema. Mas o criador das expressões regulares, Stephen Kleene, deu um jeito de abreviar isso, gerando a forma como escrevemos expressões regulares hoje. Todas as linhas de código poderiam ser substituído por uma única linha:

```python
import re 

match = re.search('".*"', texto).group()
```

Bem mais compacto, né? 

Pois então, se definirmos o seguinte texto:

```python
texto = """Este é um exemplo de texto que contém "aspas duplas"!"""
```

Os dois códigos resultam no mesmo valor para a variável ```match```:

> "aspas duplas"

_Nota do autor: re é a biblioteca nativa de expressões regulares do Python. A documentação está nas fontes, no final da página._ :wink:

## O bêaba  do RegEx

Agora você pode dizer: _"Tá, muito legal, muito interessante, mas isso não me ajuda em nada a escrever regex"_. Sim, verdade, mas é importante construir essa intuição antes de começar a estudar de fato, porque assim tem menos _decoreba_ e mais **compreensão**. 

Se eu tiver feito bem o meu trabalho, quando eu escrevi o regex ali em cima (```".*"```), você achou legal como ele resume tanta coisa. E a ideia é continuar nesse mote, ao invés de você olhar um asterisco e falar _"Que merda é essa?!"_, eu espero que você, no mínimo, reconheça seu _poder sintetizador_.

Pois bem, nessa linha, existem duas classes que podem definir um caracter numa expressão regular:

- **Literal:** É quando o caracter representa ele próprio. **A é A**. Se você quer saber se a palavra "Paçoca" aparece no texto, basta usar a própria palavra.
- **Reservado:** Um caracter é reservado, ou especial, quando o seu significa surpassa o seu simbolo, um bom exemplo é o já mencionado asterisco, que representa a repetição de um caracter "zero ou mais vezes". Ou seja, um regex como ```b*```dá match com ' ', 'b', 'bb', 'bbb' e assim por diante.

Às vezes, você precisa usar um caracter reservado como literal, para isso usamos a barra invertida ```\```. Ou seja, para usar o asterisco como asterisco, e não como "zero ou mais vezes", basta colocar ```\*```. 

Mas o que acontece se a gente usar a barra invertida em um literal? 

Bom, na maioria das vezes, nada. Mas algumas combinações são, pasmem, boas sintetizadoras. Por exemplo, caso você queira encontrar um número de um digito, ou seja _0 ou 1 ou 2 ou... ou 9_. Você pode fazer um regex assim:

```python
re.search('0|1|2|3|4|5|6|7|8|9', texto).group()
# A barra vertical, |, indica "ou".
```

Ou você pode usar ```\d``` - Qualquer digito numérico. Claro, quem conhece um pouquinho de regex deve estar me xingando pela forma acima, que nunca seria utilizada, sendo preferível a forma mais compacta: ```[0-9]```. Mas eu prometo que não vou deixar ponto sem nó, tudo tem um motivo (Sim, até o ```".*"```, pros mais avançados).

E com isso a gente já tem os pilares para começar a construir algumas expressões progressivamente mais difíceis.

#
#
### Velocidade um na dança do créu

Essa primeira parte é a que todo usuário de computador conhece: quando você aperta ctrl+f (Ou cmd+f :apple:) para pesquisar uma palavra em uma página da web. Você digita a palavra que você está procurando e ela é grifada na página. É pura e simplesmente o uso de uma expressão literal.

Embora isso possa parecer tão óbvio que você talvez nem perceba, esse é um dos casos mais utilizados, na minha experiência. Por exemplo, você está analisando um banco de dados de produtos de moda e quer ver todos os produtos com cor rosa, para fazer um marketing de oportunidade pro filme da Barbie, basta rodar uma query assim:

```sql
SELECT
    *
FROM
    tabela_produtos
WHERE 
    LOWER(nome_produto) LIKE 'rosa'
```

Assim nós encontramos aqueles produtos como "Camiseta M Básica Rosa" ou "Salto Alto Rosa". O problema é que muitas vezes o analista não conhece muito de regex (Ou aprendeu esse "truque" do ```LIKE``` e replica, sem pensar muito no que está fazendo) e a query rapidamente vira uma macarronada com dezenas de linhas fazendo vários filtros de ```LIKE``` na mesma coluna.

Para resolver isso...

### Velocidade dois na dança do créu

Aqui a gente começa a adicionar alguns daqueles caracteres reservados. Seja usando a já mencionada barra vertical ou os colchetes. Por exemplo, em uma lista de cidades com o nome da seguinte forma:

> Cidade, Estado (Ex: São Paulo, SP)

Como podemos contar quantas cidades são da região Norte?

```python
import re

qtd_cidades_norte = len(re.findall("AC|AM|AP|PA|RO|RR|TO", lista_cidades))

```

Agora, se você quisesse saber _quais_ são essas cidades, fica um pouco mais difícil. No caso do regex que eu escrevi ali, ele vai procurar as siglas dos estados no texto e adicionar essas siglas à lista. Por isso ela é útil só pra fazer a contagem. Ou em casos específicos, como quando você é mais liberal com a sua definição da cor rosa:

```sql
SELECT
    *
FROM
    tabela_produtos
WHERE 
    REGEXP_LIKE(nome_produto, 'rosa|rox[ao]', 'i')
```

Parece que eu adicionei muita coisa de uma vez, mas vamos por partes:

1. Além da cor rosa, também vamos procurar as peças com a cor roxa, portanto ```rosa|roxa```.
2. Roxo, porém, é um adjetivo que possui flexões diferentes, conforme o gênero do substantivo ("Camiseta Roxa" ou "Lenço Roxo"), diferente do rosa. Assim, precisamos levar em consideração as duas versões. Mas, ao invés de escrever ```rosa|roxa|roxo```, como a raíz "rox" é igual para as duas palavras, podemos fazer o seguinte: ```rox[ao]```, ou seja, _"Encontre uma palavra que começa com 'rox' e termina com 'a' ou 'o'"_. Portanto: ```rosa|rox[ao]```.
3. Ao invés de usar o ```LIKE```, eu estou utilizando ```REGEXP_LIKE```, para poder usar a flag ```i```, que indica que não vou fazer diferenciação entre letras maiusculas e minusculas.

### Tá ficando difícil, ein?! 

Até agora, não escrevemos nada que salte os olhos como especialmente complexo. Mas nesse próximo nível, a gente precisa começar a incluir alguns daqueles símbolos diferentes. 

Imagine, por exemplo, que você esteja auxiliando na digitalização de documentos em uma empresa. Alguém já transformou os arquivos físicos em arquivos digitais, porém ainda é preciso colocar alguns desses dados em bases de dados. Uma dessas informações é a data de admissão de todos os colaboradores. A sua chefa acabou de te pedir para fazer isso. 

É evidente que esse trabalho não seria tão compartimentado assim, mas, para fim de exemplo, exercite sua suspensão da descrença: Você só precisa salvar o nome do arquivo e a data de admissão, depois alguém junta todos os dados do funcionários em uma tabela, usando o nome de arquivo como chave primária.

O documento é um ```.txt```que toma a seguinte forma:

> Nome: Fulano de Tal
>
> Cargo: Engenheiro de Dados
>
> Data de Nascimento: 01/01/1990
>
> Data de Admissão: 01/01/2020


Uma boa forma de começar é pegar só a parte que você quer e garantir que você está conseguindo corresponder com ela. No nosso caso, queremos uma data com o formato DIA/MÊS/ANO, ou seja: _dois digitos numéricos + / + dois digitos numéricos + / + quatro digitos numéricos_:

```python
import re 

re.search('\d\d/\d\d/\d\d\d\d', '01/01/2020').group()
```

Claro, se você já percebeu o modus operandi da ferramenta, há uma forma de simplificar a construção de caracteres iguais repetindo N vezes (Quando N é conhecido e fixo, já vimos que para vezes indeterminadas pode se usar o asterisco), usando as chaves, ```{N}```:

```python
import re 

re.search('\d{2}/\d{2}/\d{4}', '01/01/2020').group()
```

Só tem um problema... Da mesma forma que ela funciona para a data desejada, também funciona para "99/99/9999", que, obviamente, não é uma data. Se você tiver certeza que tudo está corretamente digitalizado, você poderia apenas usar o regex acima, mas é possível encontrar erros de digitalização se você restringir um pouco o código: Sabemos que os dias só podem começar com 0, 1, 2 ou 3, que os meses só podem começar com 0 ou 1 e que o ano só pode começar com 1 ou 2 (Ou só 2, caso sua empresa tenha sido fundada depois do ano 2000), portanto podemos deixar nosso regex mais robusto (e confiável) da seguinte forma:

```python
import re 

re.search('[0123]\d/[01]\d/[12]\d{3}', '01/01/2020').group()
```

Agora tá começando a ficar com uma carinha legal, né?

Depois disso fica fácil, a gente só precisa garantir que o regex pega a data de admissão e não a de nascimento, mas para isso basta usar o texto literal, que é estático:

```python
import re 

re.search('Data de Admissão: [0123]\d/[01]\d/[12]\d{3}', 
          'Data de Admissão: 01/01/2020').group()
```

Bom, na verdade, **quase** basta, porque se usarmos esse código, ele vai trazer inclusive o texto "Data de Admissão: ", que nós não queremos. Para separar o joio do trigo, nós podemos utilizar **grupos**. Nesse caso, é possível adicionar um **grupo de captura**, usando o parênteses em torno da data. Aí sim, temos o código completo para automatizar esse processo:

```python
from os import listdir
from re import search

lista_de_datas = list()

diretorio = "/caminho/" # Caminho da pasta com os arquivos
for documento in listdir(diretorio):
    if '.txt' in documento:
        
        # Lê os documentos
        with open(diretorio + documento, 'r') as lupa:
            texto = lupa.read() 

        # Salva o nome do documento e a data de admissão em
        # uma tupla, e essa tupla em uma lista
        lista_de_datas.append(
            (documento, 
            search('Data de Admissão: ([0123]\d/[01]\d/[12]\d{3})', 
                    texto).group(1))
        )
```

### créu créu créu créu créu... 
#### ```(créu )*``` :satisfied:

Tocamos brevemente no último ponto que quero fazer, na seção anterior: O poder do RegEx como validador. Qualquer um que já teve a oportunidade de cuidar de um banco de dados com dados digitados por usuário (Como dados de cadastro), teve a experiência única de descobrir que pessoas que nasceram em 1822, ou que vão nascer em 3034, são compradoras frequentes da sua loja (_Nota do Autor: Esta é uma obra de ficção. Os personagens, acontecimentos e nomes retratados são inventados e qualquer semalhança com a realidade é mera coincidência_). 

O ponto é: Essas informações não são sempre confiáveis. 

Mas nós podemos melhorar isso com uma camada de validação no cadastro: Por que aceitar um dado que nós já sabemos que é errado? Validadores de CPF, por exemplo, são tão comuns que viraram exercício para estudantes de programação. Por que não fazer o mesmo com validadores de outras informações, como emails e números de telefone, para estudantes de regex?

Na verdade, até tem um porquê... Eu preciso ser sincero: **a validação de email usando regex é incompleta e extremamente confusa**, por alguns motivos:

1. Da mesma forma que um CPF estar no formato válido, não garante a existência dele, um email válido não necessariamente existe. Para checar isso, é necessário o uso de outras ferramentas.
2. Existem muitas formas de emails válidos, principalmente formas que nós não temos contato no dia-a-dia, que são raras e diferentes, mas...
3. MAS você entrar o seu email corretamente e o site não aceitar, porque um programador teve preguiça na hora de fazer um RegEx, é uma das piores experiências possíveis para um usuário. Recomendo evitar, se você quiser mantê-lo como cliente.

Para um RegEx que obedeça às normas oficiais (Pelo menos na época em que foi escrita - Não, eu não chequei), dê uma olhada na referência **\[2\]**. Mas, aqui, vamos prezar pelo simples:

> O usuário pode ter letras maiusculas ou minusculas, números, ".", "-" e "_". 
> 
> O servidor será apenas de alto nível e contendo apenas letras (Isso significa que vamos ignorar o IP do servidor).
>
> Igualmente, vamos manter só domínios ".com".

Antes de prosseguir, quero só enfatizar mais uma vez: **Estou mais preocupado com o didatismo, use o código abaixo com parcimônia**. Se você quer algo um pouco mais completo, [esse site](https://emailregex.com/) clama ter códigos que funcionam 99,99% das vezes _(Mas, ainda assim, a palavra de ordem é :sparkles: **parcimônia** :sparkles:. Experiência do usuário é importante)_.

O fim é mais fácil, então comecemos por ele:

```python
import re

re.search("@[a-z]+.com$", '@servidor.com').group()
```

Os principais servidores comerciais são contemplados por essa regra, que diz, basicamente: _Encontre um texto que começa com o "@", depois tem uma ou mais (Uma ou mais está representado pelo ```+```) letras minúsculas até encontrar o termo ".com", que é a última parte do texto (Representado pelo cifrão)_.

Em cima disso, precisamos adicionar a parte do usuário, que é um pouco menos restritiva, pois ela pode conter letras maiusculas ```[A-Z]``` ou minusculas ```[a-z]```, números ```[0-9]```, o ponto, o hífen e a barra inferior.

```python
import re

re.search("^[a-zA-Z0-9_\.-]+@[a-z]+.com$", 
          'fulano.detal90@servidor.com').group()
```

Fizemos algo bem parecido, mas, ao invés de considerar o fim do texto, ```$```, estamos considerando o início, ```^```, e ao invés de considerarmos só as letras minúsculas, temos uma ou mais (```+```) de qualquer dos caracteres citados. Esse RegEx é muito bom para enxugar uma lista de emails que vai ser usada em CRM, apesar de não ser tão boa para validação.

E talvez você esteja achando estranho o tom tão preocupado que estou tendo em relação à validação.Não é impressão sua, o tom é propostial e eu vou te contar o porquê...

## Tamanho é documento, SIM!

Lembra que comentei que eu não ia deixar ponto sem nó? Então, se você é um bom aluno (:nerd:) e estiver testando junto com o texto, pode ter percebido que o ```".*"```funciona, _pero no mucho_. Porque, no exemplo cuidadosamente selecionado que dei, é uma maravilha, mas se você coloca duas frases com aspas duplas, dá tudo errado. Por que? Eu menti pra você?!

Existe mais de uma forma para chegar ao seu objetivo com RegEx e, a bem da verdade, existem formas melhores e piores. Essa que eu sugeri, é uma das piores, porque o asterisco é um modificador **ganancioso**, isso significa que ele pega **o máximo de caracteres possíveis**. Para o momento, ele estava mais do que suficiente, mas, depois de tudo que passamos juntos, acho que eu posso ter uma conversa mais séria com você.

Escrever um bom RegEx é sinônimo de muitos testes e pensar fora da caixa, porque é preciso ver se ele funciona em todas as situações em que você precisa que ele funcione e, tão importante quanto, não funcione quando você precise que ele não funcione. Há, ainda, que se considerar a performace. Por isso, no geral, **Expressões regulares mais completas e mais longas, são melhores**. Se você tomar atalhos, sua performace, sua segurança ou os dois, podem sofrer.

Foi exatamente isso que aconteceu com a Cloudflare, em 2019. Vale a pena a leitura da descrição completa do problema no blog deles (**\[3\]**), especialmente se você se interessa por segurança, mas o resumo é:

1. Um engenheiro escreveu um código RegEx para o WAF (_Web Application Firewall_) do serviço. WAF é um firewall específico para chamadas HTTP, que visa proteger os serviços de ataques maliciosos como SQL Injection ou outras formas de interagir com o servidor usando comandos "escondidos" na query. O código era esse:

> (?:(?:\"|'|\]|\}|\\|\d|(?:nan|infinity|true|false|null|undefined|symbol|math)|\`|\-|\+)+[)]*;?((?:\s|-|~|!|{}|\|\||\+)*.*(?:.*=.*)))

2. Pela natureza desse tipo de ação, que deve ser rápida para impedir que **O Mais Novo Cyber Ataque**:tm: cause estragos, alguém escreve um código, há uma validação entre o time de engenharia, que aceita o _Pull Request_ e o _commit_ é feito, sendo implementado em todos os servidores ao redor do globo em segundos, sem, naquele momento, todas as precauções necessárias.

3. Não haviam travas ou testes de consumo de CPU para barrar o RegEx, apenas travas de detecção, isso fez que o código mal otimizado, particularmente ```.*.*=.*```, exigisse muito do CPU do servidor, para toda nova requisição. Em poucos minutos essa regra fritou os servidores globais da CloudFlare, deixando vários serviços fora do ar.

O que aconteceu foi, basicamente, uma regra perigosa (_Procure qualquer coisa de qualquer tamanho, seguida de qualquer coisa de qualquer tamanho, seguida de um igual, seguida de qualquer coisa de qualquer tamanho_) e a falta de testes suficientes gerando um problema de escala global. 

O perigo nesse tipo de regra é conhecido, damos a ele o nome _Catastrophic Backtracking_ (Em tradução livre, seria algo como Marcha-ré Catastrófica): A regra é escrita de uma maneira que o motor regex precisa checar multiplas vezes uma mesma palavra, para verificar a existência de correspondências.

{{< figure align="center" src="https://blog.cloudflare.com/content/images/2019/07/555-steps.gif" >}}

Por isso a ênfase que estou tendo, em particular em códigos de validação, é preciso testes e travas que protejam o ambiente contra essas situações catastróficas.

## Conclusão

As expressões regulares são uma das ferramentas mais poderosas na programação, ela pode, literalmente, transformar sua vida em um inferno ou agilizar seu trabalho em muitas vezes. Embora eu não espere que você saia desse texto com a capacidade de escrever um código RegEx capaz de encontrar números primos (Recomendo demais olhar isso, é uma das coisas mais legais que já vi feitas com RegEx - **\[4\]**), eu espero que ele tenha sido útil para, ao menos, desmistificar os códigos e te incentivar a finalmente estudar.



## Citações

**\[1\] -** [Watch regexes with Regexp::Debugger](https://www.learning-perl.com/2016/06/watch-regexes-with-regexpdebugger/)

**\[2\] -** [Mail::RFC822::Address: regexp-based address validation ](https://www.ex-parrot.com/%7Epdw/Mail-RFC822-Address.html)

**\[3\] -** [How Regular Expressions and a WAF DoS-ed Cloudflare](https://www.acunetix.com/blog/web-security-zone/regular-expressions-waf-cloudflare/)

**\[4\] -** [A regular expression to check for prime numbers](https://www.noulakaz.net/2007/03/18/a-regular-expression-to-check-for-prime-numbers/)

## Fontes 

[Regular Expressions](https://www.youtube.com/watch?v=528Jc3q86F8), Computerphile, 2020-01-09. (Acesso em Julho de 2023)

[Tutorial](https://www.regular-expressions.info/tutorial.html), RegExp.info (Acesso em Julho de 2023)

[Documentação re](https://docs.python.org/3/library/re.html), Python Software Foundation (Acesso em Julho de 2023)

[Documentação do PostgreSQL](https://www.postgresql.org/docs/), PostgreSQL Global Developmente Group (Acesso em Julho de 2023)

[Under the Hood: Regular Expressions](https://reindeereffect.github.io/2018/06/24/index.html), Reindeer Effect, 2018-06-24 (Acesso em Julho de 2023)

[Regexes: The Bad, the Better, and the Best](https://www.loggly.com/blog/regexes-the-bad-better-best/), Liz Bennett, 2015-06-18 (Acesso em Julho de 2023)
