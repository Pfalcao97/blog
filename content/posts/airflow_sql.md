---
author: "Pedro Falcão"
title: "Consultando DAGs por horário no SQL"
date: "2023-11-29"
description: "Consulta de metadados de DAGs."
tags: ["SQL", "Engenharia de Dados", "Airflow"]
categories: ["Programação"]
aliases: ["airflow_cron_query"]
draft: true

cover:
    image: "https://github.com/Pfalcao97/blog/blob/main/static/cover_bayes.png?raw=true"
    alt: "A estatística bayesiana"
---

# Introdução

Hoje gostaria de propor fazer algo um pouco diferente por aqui, ao invés de explicar algum conceito, eu gostaria de compartilhar como resolvi uma dor no meu trabalho, pois acredito que essa solução possa ajudar iniciantes no SQL com conceitos mais avançados, e pode até auxiliar usuários mais experientes que nunca tenham pensado em fazer algo assim.

O Airflow é constituído de quatro elementos principais: `Worker`, `Scheduler`, `Webserver` e `Database`. Este último é um banco de metadados do Airflow, inicialmente construído em SQLite, mas normalmente portado para MySQL ou PostgreSQL, que é, inclusive, o motor em que me basearei para a construção da query. Nesse banco, estão as principais informações de metadados do Airflow, ou seja, dados sobre a própria ferramenta.

O schema dessas tabelas de metadados [está disponível na documentação](https://airflow.apache.org/docs/apache-airflow/stable/database-erd-ref.html), mas, uma tabela é especialmente importante para a nossa tarefa: `Dag`. Nela, estão contidos os dados como o CRON da DAG e se ela está ativa e pausada. 

Mas aí que está, nós, humanos, conseguimos interpretar um código CRON com facilidade (depois de um pouco de prática), mas consultar na tabela de metadados quais são as DAGs que são executadas em um horário específico é um desafio. Nisso surge a query que quero desenvolver hoje.

# Expressões CRON

Pra quem nunca teve contato, o código CRON (ou apenas CRON) pode ser um pouco intimidador, mas é, na verdade, bem simples: é um código que sintetiza a informação de **frequência de atualização de uma tarefa** em uma string com 5 informações, em ordem:

1. O **Minuto** de execução.
2. A **Hora** de execução.
3. O **Dia (do mês)** de execução.
4. O **Mês** de execução.
5. O **Dia (da semana)** de execução.

Sendo que qualquer um desses pode ser ocupado por uma das três opções:

> A. Valores específicos (separados por vírgula)
> 
> B. Faixas de valores (separadas por hífen)
> 
> C. Qualquer valor (representado pelo asterisco)
> 
> D. Frequências (representado por um número (ou o asterisco) dividido por outro número)

A melhor forma de explicar, nesse caso, é com exemplos. 

Vamos criar um CRON que execute todos os dias, às 09:20h e às 18:20: Como a ativação acontece todos os dias, qualquer valor para o dia do mês, dia da semana e mês são considerados, portanto preenchemos eles com **o asterisco**. A hora e o minuto, por outro lado, são valores específicos:

1. **Minuto:** 20 
2. **Hora:** 9,18
3. **Dia (do mês):** *
4. **Mês:** *
5. **Dia (da semana):** *

E, portanto, escrevemos o CRON da seguinte forma:

```
20 9,18 * * *
```

Agora um exemplo um pouco mais completo: a ação deve ser executada todas as segundas-feiras, no início de cada hora par à partir das 06 da manhã. Nesse caso, precisamos colocar segunda-feira no último espaço (os dias da semana começam no domingo, cujo valor é 0, e terminam no sábado, valor 6), porém o dia do mês e o mês podem ser mantidos como "qualquer valor". Para fazer essa execução periódica, usamos a barra, portanto escrever `*/2` equivale à dizer "começando no zero, execute à cada duas horas", mas desejamos começar às 06h, portanto, basta substituir o asterisco por 6:

1. **Minuto:** 0 
2. **Hora:** 6/2
3. **Dia (do mês):** *
4. **Mês:** *
5. **Dia (da semana):** 1

Ou, no formato de CRON:

```
0 6/2 * * 1
```

Esse básico é suficiente pra gente começar a desenvolver a query, mas existem mais exemplos complexos e outras possibilidades. Para testar o seu entendimento, é possível utilizar [o site Crontab Guru](https://crontab.guru/) para consultar o significado de diferentes códigos.

# A query

Como disse, o CRON é uma síntese, ele transmite uma informação em um formato denso, o que, nesse caso, facilita a interpretação humana, mas dificulta a consulta. Imagine, por exemplo, a dor de cabeça que é pra ordenar uma lista de tarefas em ordem de execução. 

De forma manual você precisa identificar todas as tarefas, traduzir as expressões para frequência de atualização e organizar. Mas, principalmente em uma operação madura, são centenas de DAGs, muito além do que qualquer engenheiro consegue manter na caixola. Pois então **surge a necessidade de ser capaz de criar um código capaz de organizar a tal tabela de de metadados do Airflow, ordenando as DAGs em relação à ordem de execução delas!**

Começando do começo: precisamos garantir que todos as colunas sejam, de fato, códigos CRON. Como a tabela de metadados é gerada automáticamente pelo Airflow, não precisamos nos preocupar com valores nulos ou erros de digitação (a não ser que esteja errado na DAG também, mas isso acho que é um problema maior), apesar disso o Airflow possui algumas expressões simplificadas, escritas em inglês. O primeiro passo é traduzi-las:

| Expressão | CRON correspondente | Interpretação |
|:-----------:|:------:|:---:|
|_@once_ | - | Uma única vez |
|_@hourly_ | 0 * * * * | De hora em hora |
|_@daily_ | 0 0 * * * | Todo dia |
|_@weekly_ | 0 0 * * 0| Toda semana |
|_@monthly_ | 0 0 1 * * | Todo mês |
|_@yearly_ | 0 0 1 1 * | Uma vez ao ano |


Para fazer isso, eu uso um `CASE-WHEN` simples:

```
SELECT 
    dag_id,
    CASE WHEN schedule_interval = '@once' THEN null
         WHEN schedule_interval = '@hourly' THEN '0 * * * *'
         WHEN schedule_interval = '@daily' THEN '0 0 * * *'
         WHEN schedule_interval = '@weekly' THEN '0 0 * * 0'
         WHEN schedule_interval = '@monthly' THEN '0 0 1 * *'
         WHEN schedule_interval = '@yearly' THEN '0 0 1 1 *'
         ELSE schedule_interval END AS schedule_interval
FROM 
    dag
```

Note que o código é uma `string`!

Como a query vai ficar bem extensa, vou focar apenas nas duas colunas imediatamente relevantes: `dag_id`, que é o nome da DAG, e `schedule_interval`, que é o CRON, mas posteriormente, quando formos discutir o uso real dessa query, algumas outras colunas serão importantes, como a informação sobre se a DAG está ativa ou despausada.

Agora que garantimos que a coluna está homogênea, podemos transformá-la!

Felizmente, as expressões são separadas por espaços sempre, então nós podemos usar isso à nosso favor: a função `split_part`, do PostgreSQL, recebe três parâmetros:

```
split_part(
    coluna, -- Nome da coluna a ser separada
    separador, -- Caracter utilizado para fazer a separação
    posição -- Posição do fragmento que você quer recuperar
) 
```

Como já disse antes, as expressões CRON são formadas por cinco espaços. então repetimos essa função cinco vezes, para recuperar cada uma das informações. Outra informação que comentei é que, além do asterisco, também podemos encontrar a barra, `\`, a vírgula, `,` e o hífen `-`. 

Lidaremos com a barra e o hífen depois, mas a vírgula será relevante imediatamente.

Embora, sim, estamos representando uma frequência no tempo e ordenaremos a lista de DAGs conforme a sua próxima execução, optei por não tratar as unidades de tempo como unidades de tempo, mas sim como números inteiros, `INTEGER`. Mais do que isso: **listas de números inteiros**. E aí que vem o pulo do gato, para fazer a query funcionar, a gente precisa decompor cada uma dessas cinco informações nas listas de números que elas estão tentando emular!

No caso em que a informação é um conjunto de valores específicos, fica mais fácil, já que o elefantinho azul nos fornece uma função relevante pra isso: `string_to_array`. Como o nome sugere, uma string é transformada em uma lista, usando um separador, caso não exista nenhuma instância desse separador na string, o retorno é uma lista unitária.

```
string_to_array(
    coluna, -- Coluna à ser dividida, de tipo textual
    separador -- Caracter utilizado para fazer a separação
)
```

Note que a resposta dessa função é sempre uma lista **textual**, e nem adianta tentar transformar ele numa lista de inteiros ainda, porque a maioria dos elementos serão listas unitárias com asteriscos, `{*}`. Pois então, pra segunda fase da transformação, temos:

```
WITH base AS (
SELECT
    dag_id,
    schedule_interval,
    string_to_array(split_part(schedule_interval, ' ', 1), ',') as minuto,
    string_to_array(split_part(schedule_interval, ' ', 2), ',') as hora,
    string_to_array(split_part(schedule_interval, ' ', 3), ',') as dia_mes,
    string_to_array(split_part(schedule_interval, ' ', 4), ',') as mes,
    string_to_array(split_part(schedule_interval, ' ', 5), ',') as dia_semana,
FROM
    (SELECT 
        dag_id,
        CASE WHEN schedule_interval = '@once' THEN null
            WHEN schedule_interval = '@hourly' THEN '0 * * * *'
            WHEN schedule_interval = '@daily' THEN '0 0 * * *'
            WHEN schedule_interval = '@weekly' THEN '0 0 * * 0'
            WHEN schedule_interval = '@monthly' THEN '0 0 1 * *'
            WHEN schedule_interval = '@yearly' THEN '0 0 1 1 *'
            ELSE schedule_interval END AS schedule_interval
    FROM 
        dag) AS tradutor_cron)
```

Para fechar o pacotinho eu peguei aquele primeiro segmento do código e coloquei dentro de uma subquery, pra poder fazer a consulta em cima dele. Notem que, além disso, adicionei todo o código à uma CTE, para reduzir o tamanho do código. Isso significa que todo esse segmento de código agora é "funcionalmente" uma tabela, que será chamada usando o nome `base`, me permitindo omitir esse pedaço.

Agora, não tem mais pra onde correr, precisamos falar dos outros casos, e isso inclui dois tópicos mais avançados: `RegEx` e `Geradores`. Caso você não conheça muito de `RegEx`, recomendo [a leitura de um texto que escrevi aqui no blog](https://pfalcao97.github.io/blog/posts/regex/), que faz uma introdução bem suave ao tópico. Sobre os geradores, vou fazer uma breve introdução:

O PostgreSQL (bem como outros motores de SQL) é capaz de gerar listas de alguns tipos de variáveis, como datas e, mais importante no nosso caso, números, usando os `geradores`, ou [funções que retornam listas](https://www.postgresql.org/docs/current/functions-srf.html), a anatomia dessas funções é simples:

```
generate_series(
    start, -- Início da lista
    stop, -- Fim da lista
    step -- Passo da lista
)
```

Setando, por exemplo, `start = 1`, `stop = 5` e `step = 1` (ou vazio, visto que 1 é o valor padrão), iríamos gerar a lista `{1,2,3,4,5}`. Eu já passei o mandrake do código, iremos decodificar as informações em **listas de inteiros**, pois bem, essas listas são (ou, pelo menos, começam sendo) todos os valores possíveis que aquela informação pode tomar: dentre uma hora, existem 60 minutos, isso significa que a informação "Minuto" pode tomar os valores 0, 1, 2,..., 58, 59. Isso pode ser desenvolvido muito facilmente usando a função `generate_series`: 

```
WITH lista_auxiliar_minutos AS (
SELECT 
    generate_series(0, 59) as auxilair
)
```

Para as demais unidades de tempo, a simplicidade se mantém: basta entender quais são os valores possíveis que aquela unidade de tempo pode assumir e gerar uma série com isso. Aqui, porém, surge o maior problema em usar números, ao invés de datas: no dia do mês, a lista gerada é de 1 a 31, porém, obviamente, nem todos os meses têm 31 dias. **Isso significa que essa solução é apenas aproximada.** A depender de como você vai usar esse código, isso não vai te impactar, mas, é uma limitação importante de se ter em mente!

Continuando as demais listas:

```
lista_auxiliar_horas AS (
SELECT 
    generate_series(0,23) as aux
),

lista_auxiliar_dias_mes AS (
SELECT 
    generate_series(1,31) as aux
),                              

lista_auxiliar_meses AS (
SELECT 
    generate_series(1,12) as aux
),

lista_auxiliar_dias_semana AS (
SELECT 
    generate_series(0,6) as aux  -- 0 é domingo.
), 
```

Note que eu gerei, mais uma vez, essas listas em CTEs. Fiz isso por um motivo bastante pessoal: a aplicação que eu usei pra fazer essa consulta não tinha acesso de administrador ao banco, isso significa que ela não poderia criar funções, que seriam a forma como eu preferiria fazer todas essas transformações (ficaria bem mais organizado). O código seria o mesmo, apenas a estrutura que seria mais organizada!

