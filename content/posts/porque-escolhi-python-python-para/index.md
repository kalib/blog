---
title: "Porquê Escolhi Python: Python Para Sysadmins"
date: 2009-11-21T10:19:56-04:00
draft: false
tags: [
"Impressões",
"Python",
"Software Livre",
]
categories: ["Impressões"]
description: "Novamente vou dedicar um post para responder uma pergunta que me fizeram. Não é a primeira vez que alguém me pergunta algo e eu resolvo responder em forma de post. Algumas pessoas já me perguntaram o que me leva a preferir Python à outras linguagens de programação. Mas como, coinscidentemente, de ontem para hoje 3 pessoas me fizeram a mesma pergunta, resolvi responder em forma de post e poupar um pouco de saliva (¿dedo no teclado?) e não responder de forma mais completa para um ou outro."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
**S**audações pessoal...

**N**ovamente vou dedicar um post para responder uma pergunta que me fizeram. Não é a primeira vez que alguém me pergunta algo e eu resolvo responder em forma de post. Algumas pessoas já me perguntaram o que me leva a preferir Python à outras linguagens de programação. Mas como, coinscidentemente, de ontem para hoje 3 pessoas me fizeram a mesma pergunta, resolvi responder em forma de post e poupar um pouco de saliva (¿dedo no teclado?) e não responder de forma mais completa para um ou outro.

<!--more-->

**A**ntes de mais nada gostaria de informar que não sou programador. Trabalho na área de administração de sistemas, redes, segurança, etc, etc, etc, vulgo SysAdmin. Prazer. ;]

**S**e você é SysAdmin com certeza já esbarrou com algumas linguagens de script como Bash, Perl, dentre outras. Provavelmente até já trabalhou com algumas delas. Estas linguagens podem nos ajudar a resolver pequenos problemas bem como automatizar e agilizar tarefas cotidianas de forma a ganhar produtividade e perder menos tempo com aquilo, bem como evitar stress, tédio e fadiga fazendo tarefas mecânicas e repetitivas.

**E**stas linguagens são apenas ferramentas que podem ser utilizadas no dia-a-dia. Mas o que faz uma linguagem ser eficiente? Ela só pode ser considerada eficiente se puder lhe ajudar a ter o seu trabalho feito de uma forma mais produtiva e simples, certo?! Que tal Python dentro deste cenário?

**A** inevitável pergunta acaba aparecendo: O Python é melhor que Perl, Bash, Ruby ou qualquer outra linguagem?

**E**ste é o tipo de pergunta que eu não conseguiria responder. A complexidade empregada nesta pergunta impede uma resposta em poucas palavras, visto que a definição de "melhor" varia de acordo com o cenário, bem como os protagonistas envolvidos. Por conta disto, não direi que o Python é melhor ou pior do que outras linguagens, mas vou abordar alguns exemplos que demonstram que a mesma pode ser uma excelente escolha. (¿Escapei bem?)

**C**reio que o **primeiro motivo** que me leva a adotar Python é a simplicidade de seu código. Se uma linguagem não lhe permite aprender rapidamente e começar a escrever códigos para resolver seus problemas, a mesma acaba perdendo um pouco de credibilidade, certo? Lembrem-se que, como SysAdmin, não tenho tempo para ficar estudando livros e mais livros sobre uma ou outra linguagem. Apenas desejo resolver meu problema atual. Porque perder semanas ou meses estudando uma linguagem para somente então conseguir escrever algum código que realmente produza algo? Bem, o Python nos permite escrever scripts em horas, literalmente falando, ao invés de dias ou semanas. Se, como um SysAdmin, uma linguagem não lhe permite começar os estudos e escrever scripts imediatamente, você deveria realmente se questionar porque você deveria estudar ela.

**M**as de que me vale uma linguagem que me permite um rápido aprendizado, se a mesma não possui "poder"? Se a mesma não me permite realizar tarefas complexas e robustas? Bem, na verdade este é o **segundo motivo** pelo qual eu escolhi Python. Esta linguagem nos permite resolver problemas simples como analisar várias linhas de log e nos retirar apenas informações que nos sejam interessantes ou pertinentes gerando um relatório mais limpo e "legível" ao olho humano. Além de tarefas simples assim, o Python também vem sendo bastante utilizado para tarefas com maior grau de complexidade como análises de sequências genômicas, cálculos complexos de física, mecânica, mecatrônica, etc, sistemas web multithread ou mesmo pesadas análises estatísticas. Bom, se você é um SysAdmin, muito provavelmente você nunca precisará de nada disso, mas eu me sinto confortável em saber que estou estudando uma linguagem que me permitirá realizar tarefas mais complexas quando eu precisar. ;]

![Python Cartoon](pythoncartoon.jpg)
**O**k, Python me permite realizar até as tarefas complexas. Mas e a manutenção deste código. Como SysAdmin, não fico editando e revendo meus códigos todo dia. As vezes não entendemos nossos próprios códigos depois de alguns meses sem olhar para eles. O que eu queria dizer com aquela linha de código mesmo? O.o O Python foi criado com o intuito de possuir uma sintaxe simples e intuitiva, de forma que a manutenção de código se torna extremamente eficaz, mesmo por aqueles que não são os autores originais do código.  Mesmo depois de meses eu vou conseguir interpretar meu código e trabalhar nele. E esta foi a terceira razão pela qual eu escolhi Python, e que por sua vez está muito ligada à quarta razão.

**O** quarto motivo pelo qual escolhi Python é a legibilidade do código e a forma como apenas batendo o olho podemos nos encontrar facilmente no código. O Python utiliza-se de espaços em branco para determinar onde começa ou termina um bloco de código. Esta forma de identação facilita muito a identificação de partes do código bem como o entendimento do mesmo.

**É** o bastante? Não para mim.

**O quinto ponto** na maestria do Python é o seu excelente suporte à Programação Orientada a Objetos (POO). Quando digo suporte à orientação a objetos, não me refiro à obrigação de utilizá-la. Você não precisa utilizar caso não deseje ou caso não se aplique em algum caso específico, mas é sempre bom ter em mãos este "poder" caso seja necessário, e é nestes casos que o Python mostra novamente uma enorme eficiência por conta da simplicidade com a qual a POO é tratada pelo Python.

**A**cho que não preciso citar que outro grande motivo pelo qual adotei o Python é o fato de ser uma linguagem 100% livre que conta com uma comunidade altamente atuante e participativa, certo?!

**M**as, creio que já falei demais por um post. Que tal um simples exemplo prático de simplicidade no código escrito em Python?

**Q**ue tal um simples script para servir de comparação entre, por exemplo, Bash, Perl e Python?

**O** seguinte script tem a função de apresentar todas as combinações possíveis de 1,2 e a,b.

**E**m **Bash** seria mais ou menos o seguinte:


```bash
#!/bin/bash

for a in 1; do
for b in a b; do
echo "$a" "$b"
done
done
```

**E** como ficaria o mesmo em **Perl**?

```perl
#!/usr/bin/perl

foreach $a ('1', '2') {
foreach $b ('a', 'b') {

print "$a $bn";
}
}
```

**C**omo podem ver, é apenas um loop comum. Vejamos agora o mesmo exemplo de loop utilizando-se um for escrito em **Python**:

```python
#!/usr/bin/env python

for a in [1, 2]:
for b in ['a', 'b']:

print a,b
```

**S**imples, certo? Qual pareceu mais simples? (Se você não programa e não entende nada do que estou falando até aqui, segue uma dica: Vote no que utilizou uma quantidade menor de linhas de código! :p A propósito, porque você leu até aqui mesmo? o.O)

**V**ocê já deve estar cansad@ de ler, e eu de digitar, portanto vamos finalizar por aqui ok?!

**E**spero que experimente Python, caso ainda não o tenha feito, e descubra uma nova excelente ferramenta para o seu dia-a-dia.

**C**aso deseje mais informações sobre Python bem como suas vantagens, confira outros posts que fiz listados abaixo:

[1- Porque Python?](/posts/porque-python/)

[2- Pydev: Preparando o Eclipse para o Python](/posts/pydev-preparando-o-eclipse-para/)

[3- The Zen of Python](/posts/the-zen-of-python/)

**A**braços
