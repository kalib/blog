---
title: "Dica: Escondendo Seu Endereço De Email Ao Divulgar Seu Endereço De Email"
date: 2014-01-23T19:02:43-05:00
draft: false
tags: [
"Linux",
"Seguranca",
"Cultura Hacker",
]
categories: ["Tutoriais"]
description: "Deseja divulgar seu endereço de email em algum local mas tem medo de ser vítima de pishing e passar a receber trocentos emails indesejados por conta disso? Experimente esconder seu endereço de email ao mesmo tempo que o divulga..."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**D**a mesma forma que alguns animais só conseguem sobreviver utilizando-se de sua capacidade e/ou técnica de camuflar-se, misturando-se assim ao ambiente, para enganar seus predadores naturais, as pessoas do mundo atual praticamente precisam adotar as mesmas técnicas para manter seus endereços de email livres de um turbilhão de emails indesejados com propagandas de viagra, dicas para crescimento peniano, irregularidades em seu CPF, problemas em sua conta bancária, etc, etc.

<!--more-->

**U**m dos métodos mais simples e mais utilizado é substituir a *@* por alguma outra coisa, como por exemplo:

*fulanoEMgmail.com*
*fulanoAThotmail.com*
...

**M**as existem aqueles que podem preferir levar isto à um nível mais elevado. Que tal fazer de uma forma mais geek?

**O** Linux pode lhe ajudar a fazer isto de forma inusitada, porém eficiente e confiável. Para nosso exemplo, vamos assumir que utilizaremos o email *fulano@marcelocavalcante.net*.

**E**xperimente o seguinte comando:

```bash
 $ echo fulano@marcelocavalcante.net | tr a-z@. n-za-m.@
```

**O** retorno será algo como:

```bash
 shynab.zneprybpninypnagr@arg
```

**I**legível, certo? Parabéns, você acaba de "camuflar" ou "esconder" o seu email. Este é o seu endereço de email. Como divulgar ele para que alguém possa entendê-lo? Bom, ao invés de divulgar um endereço de email, você vai divulgar um comando, da seguinte forma:

**echo shynab.zneprybpninypnagr@arg | tr a-z@. n-za-m.@**

**D**esta forma, quem copiar este comando e o executar em um terminal Linux, terá o seguinte resultado:

```bash
 $ echo shynab.zneprybpninypnagr@arg | tr a-z@. n-za-m.@
 fulano@marcelocavalcante.net
```

**D**esta forma você evita que robôs ou scanners saiam vasculhando seu endereço de email indexando conteúdos aleatórios pela internet e assim, reduzindo as chances de o seu endereço de email cair em listas de SPAM.

**L**egal, certo?!

**H**appy Hacking!
