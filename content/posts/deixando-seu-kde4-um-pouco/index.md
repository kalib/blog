---
title: "Deixando Seu Kde4 Um Pouco Mais Arch - Kdm"
date: 2008-09-26T11:08:57-04:00
draft: false
tags: [
"Arch Linux",
"Impressões",
"KDE",
"Linux",
"Software Livre",
]
categories: ["Tutoriais"]
description: "Sabe aqueles horários em que você não tem o que fazer e acaba, literalmente, procurando qualquer coisa pra mexer? Foi em um horário de almoço como este que fiz logoff em meu KDE4 e observei minha tela do KDM. Reparei em como os designers resposnsáveis fizeram um bom trabalho no KDE4. Foi aí que pensei: Porque não ter um pouquinho do gosto Arch Linux já na hora de me logar ao KDE4?"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
Sabe aqueles horários em que você não tem o que fazer e acaba, literalmente, procurando qualquer coisa pra mexer? Foi em um horário de almoço como este que fiz logoff em meu KDE4 e observei minha tela do KDM. Reparei em como os designers resposnsáveis fizeram um bom trabalho no KDE4. Foi aí que pensei: Porque não ter um pouquinho do gosto Arch Linux já na hora de me logar ao KDE4?

<!--more-->

Alterei então o tema para ter como resultado final um tema de kdm para o meu Arch Linux. Ficou com a elegância que já trazia por padrão do KDE4, porém com uma pequena pitada de Arch Linux nele, de forma a tornar o ambiente mais customizado.

Mãos à obra...

Editei a imagem original com o Inkscape, portanto este pequeno trabalho lhes será poupado. Tudo o que vocês precisam é inserir esta imagem no tema original do KDM. Vamos às formas disponíveis:

Primeiro, faça o download da imagem através do seguinte link: [download](https://www.megaupload.com/?d=TGY8J88O)

1- Você pode simplesmente substituir a imagem original por esta com o comando:

```bash
mv ~/background.svg /usr/share/apps/kdm/themes/nome-do-tema/
```
ps: Isto, levando em consideração que a imagem alterada se encontra dentro do seu home.

2- Manter as duas imagens: Original e Alterada  

Para isso você precisa antes de mais nada renomear a imagem original no tema para um segundo nome:

```bash
mv /usr/share/apps/kdm/themes/nome-do-tema/background.svg /usr/share/apps/kdm/themes/nome-do-tema/background2.svg
```

Em seguida, jogar a imagem alterada lá dentro:

```bash
mv ~/background.svg /usr/share/apps/kdm/themes/nome-do-tema/
```

ps: Isto, levando em consideração que a imagem alterada se encontra dentro do seu home.

É isso pessoal, em breve maiores customizações para nosso KDE4 + Arch Linux. ;]

Abraços!