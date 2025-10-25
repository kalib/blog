---
title: "Minhas Primeiras Impressões Com O Arch Linux"
date: 2008-06-03T16:47:37-04:00
draft: false
tags: [
"Arch Linux",
"Impressões",
"Linux",
]
categories: ["Impressões"]
description: "Quem me conhece sabe que sou um aventureiro quando se trata de distribuições Linux. Sempre utilizei várias distros dos mais variados tipos passando pelo Conectiva, Slackware, Suse, Kurumin, Ubuntu, Kalango, Kubuntu, Debian, Fluxbuntu, Goblinx, Fedora, Big, dentre outros. Agora é a vez do Arch."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---

Quem me conhece sabe que sou um aventureiro quando se trata de distribuições Linux. Sempre utilizei várias distros dos mais variados tipos passando pelo Conectiva, Slackware, Suse, Kurumin, Ubuntu, Kalango, Kubuntu, Debian, Fluxbuntu, Goblinx, Fedora, Big, dentre outros. Agora é a vez do Arch.

<!--more-->

A simplicidade do Slackware sempre me fascinou, porém seu gerenciamento de pacotes me desanimou bastante há alguns anos atrás quando vi outras distribuições evoluindo tanto neste ponto como o Debian por exemplo. Foi quando fui fascinado pelo poder do apt-get/aptitude. Por outro lado, sempre gostei de ter os aplicativos mais atualizados e funcionando em minha máquina, e como esse não é o forte do Debian resolvi partir para distros baseadas nele, onde o Kubuntu se destacou ficando por mais tempo. Independente de minha máquina, também utilizo diariamente o Fedora, distribuição que utilizo no trabalho em minha estação. Diante de tantas mudanças, porque não tentar o Arch Linux agora?

Há algum tempo ouço amigos falando sobre suas vantagens mas queria confirmar se era tudo verdade, ou apenas mais uma visão de usuário fanático por sua distro. E foi aí que tudo começou...

Primeiramente parti para a filosofia e história do mesmo. Para quem não sabe, o Arch Linux foi criado em 2001 por **Judd Vinet** e, desde, então vem crescendo bastante. É uma distribuição rápida, leve, elegante e bastante flexível. No Brasil ainda é uma distro pouco conhecida e usada. Por ser otimizado para processadores i686, o Arch roda mais rápido que a maioria das distribuições que, por sua vez, utilizam pacotes para i386, porém por outro lado só roda em máquinas com processadores superiores ao pentium II. O Arch possui uma filosofia que se assemelha a do Slackware no estilo KISS, Keep It Simple Stupid, mantendo o sistema simples e bastante customizável de modo que pode-se configurar tudo possuindo um pouco de intimidade com o shell do Linux. Porém encontrei nele uma grande evolução com relação ao Slackware que resolveu se manter mais tradicional, enquanto que o Arch trouxe um gerenciador de pacotes chamado pacman. O pacman, de certa forma, lembra o apt-get/aptitude, com o qual é possível facilmente atualizar seu sistema, buscar, instalar ou remover pacotes de forma rápida e segura sem precisar se preocupar com problemas de dependências que o mesmo resolve por você corretamente.

Sempre adorei ler, mas não ia ser uma simples leitura que me convenceria sobre o fato de o Arch ser uma boa distribuição Linux, mesmo porque em cinco minutos eu sou capaz de "criar" uma distribuição Kalib e escrever uma bela história sobre como ela funciona e torná-la assim a melhor distribuição Unix Like ou mesmo BSD, concorda? Sendo assim, parti para os primeiros testes reais.

Como não queria particionar meu disco, pelo mesmo já ter muitas fatias, resolvi criar uma máquina virtual com o Arch Linux, utilizando para isto um aplicativo chamado Virtual Box.

A instalação foi bem simples, sem maiores dificuldades, apesar de ser toda em modo texto sem as facilidades da interface gráfica, o que acaba por assustar um pouco os usuários iniciantes no mundo Linux. Sem maiores traumas passei pelo poucos passos de particionamento do disco, escolha dos pacotes e instalação dos mesmos.

Após a instalação pude, ou melhor, TIVE que testar as habilidades do pacman no gerenciamento de pacotes. Porque TIVE? Bom, digamos que o Arch não traga muitos pacotes instalados por default. Tive de instalar as fontes, o xorg, interface gráfica (kdemod), driver de minha placa de áudio, driver de vídeo, etc... Mas nada que o pacman não tenha resolvido com bastante eficiência. ;] Fiquei surpreso com sua eficiência nesse momento.

Após instalado, resolvi checar a eficiência de seus repositórios. Mandei o pacman instalar o amsn, apenas a nível de curiosidade para saber qual versão ele traria, e para minha surpresa ele me instalou a versão 0.97 que é a mais recente de todas, e que no kubuntu havia conseguido instalar apenas via svn. Realmente me surpreendeu muito o pacman.

Outra coisa que me chamou bastante atenção foi a leveza com que o sistema se mostrou funcionar.

Em resumo, esta foi a primeira impressão que tive do Arch Linux. Uma distribuição bastante leve, eficiente, estável e extremamente maleável. Não é o tipo de distribuição que eu recomendaria para usuários que apenas desejam ter um sistema operacional com tudo funcionando, mas com certeza eu o indicaria para alguém que quer ter um excelente sistema e não tem medo de aprender ou ter que resolver problemas de configuração na unha. Esperemos para ver no que essa relação Kalib/Arch irá resultar. ;]

Links:  

[1- Maiores detalhes sobre o Arch Linux e sua história](https://www.archlinux-br.org/about/)  

[2- Download do Arch](https://www.archlinux-br.org/download/)  

[3- Tutorial de Instalação](https://www.vivaolinux.com.br/artigos/verArtigo.php?codigo=8256)
