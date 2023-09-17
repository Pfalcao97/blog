---
author: "Pedro Falcão"
title: "Correlação - do fascismo ao Nicolas Cage"
date: "2023-09-12"
description: "A ferramenta que te permite dizer muita coisa, sem explicar nada"
tags: ["Estatística", "Matemática"]
categories: ["Estatística"]
aliases: ["correlation"]
draft: true

cover:
    image: "https://github.com/Pfalcao97/blog/blob/main/static/cover_bayes.png?raw=true"
    alt: "A estatística bayesiana"
---

## Introdução

Quando eu era mais novo, um dos passatempos populares para crianças eram livros de **ligar os pontos**. Em uma página os editores colocavam vários pontos enumerados que deveriam ser ligados na ordem correta, formando assim uma figura. Eu acho que a popularidade desses livretos era que eles rendiam mais do que livros de colorir simples - Em uma época pré-tablets, manter as crianças entretidas em casa era um pouco mais difícil.

{{< figure align="center" src="https://boracolorir.com.br/wp-content/uploads/2022/04/desenhos-para-ligar-os-pontos-5-1024x751.jpg" width="auto" >}}

Agora, sendo adulto, percebo como essas lições foram valiosas para mim, afinal, uma boa parte do meu trabalho com dados é olhar pontinhos na tela e tentar traçar a linha correta que conecta todos os pontos e forma a figura real. Sem todas as estrelinhas sorridentes e gatinhos manhosos que desenhei quando criança, certamente não seria tão bom no meu trabalho!

Brincadeiras à parte, o analista deve estar sempre atento a muitos aspectos estatísticos dos dados de trabalho, desde a distribuição deles até suas tendências. Foi, inclusive, com essa atenção e curiosidade que Carl Gauss descobriu essa propriedade que dados às vezes têm: a correlação.

## Ligando os pontos

Eu não comecei o texto falando sobre ligar pontos só para fazer uma piadinha sem graça. O fato é que existem muitas formas de representar aquelas informações que estamos trabalhando: enquanto que, antigamente, o trabalho de estatísticos, matemáticos e cientistas era conseguir os dados, passar por horas cálculos manuais e maçantes e ter uma veia artística suficientemente latente para conseguir desenhar um gráfico, hoje todos esses processos são feitos em alguns minutos (quando muito) e o trabalho do analista se está mais na seleção do que na criação. Saber quais são as estatísticas relevantes e quais são os gráficos que melhor representam o conjunto de trabalho, separa o operador cuidadoso, que faz escolhas deliberadas e precisas para extrair informação, do sistema SISO (_Shit in, shit out_, quando a pessoa não entende o que está acontecendo, ela pode gerar qualquer baboseira sem barreiras).

Tem alguns gráficos que são mais diretos: o gráfico do valor arrecadado por dia ao longo de um certo período se encaixa bem em um **gráfico de linha**, enquanto que um **gráfico de barras** é mais adequado para representar o valor vendido no mês, por categoria. Outras vezes, a informação à ser passada é mais complexa e precisa de muitos gráficos ou muitos elementos para ser compreendido. Existe toda uma ciência (e, por que não, uma arte) em fazer gráficos completos, bonitos e que sintetizam os conjuntos de dados - Os jornalistas, com seus infográficos criativos e cheios de desenhos, dominam essa área. Nós, da ciência e das empresas, muitas vezes precisamos colocar o pé no chão e focar no _arroz-com-feijão_, por assim dizer.

Mas nem por isso o trabalho é menos difícil. Muitas vezes nós realmente temos que ligar os pontos (ou preencher as lacunas) para que os gráficos traduzam aquilo que queremos mostrar: uma série de pontinhos em posições diferentes não traduz tão bem a mensagem de que **"A EMPRESA ESTÁ FALINDO!!!"**, sem uma linha grossa e vermelha acentuando a queda.

Um dos gráficos que eu, particularmente, mais gosto é o **gráfico de dispersão**. É com esse método que nós conseguimos comparar coisas, quando você escuta no jornal a ancora falando _"Estudos mostram que pessoas que fumam mais tendem a ter mais câncer"_, você deve estar estar pensando em um gráfico de dispersão, com as pessoas sendo pontinhos, um eixo sendo a medida de "fumar mais" e o outro a medida de "ter mais câncer".

## A bioestatística (e a eugenia)

- bioestatistica 
- karl person
- eugenia 
- digital (dedo)
- altura dos pais e filhos

## Gauss e os "montes"

- altura dos pais e filhos
- os montinhos do gauss
- regressão à média
- surgimento da correlação

## A correlação

- person de novo (lembra dele?)
- formulação
- o que significa?

## Correlação != causalidade

- correlações espurias
- exemplos divertidos
- como interpretar

## fim!