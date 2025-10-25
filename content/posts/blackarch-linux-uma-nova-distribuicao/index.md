---
title: "Blackarch Linux - Uma Nova Distribuição Para Pentesters"
date: 2014-01-30T18:54:14-05:00
draft: false
tags: [
"Linux",
"Seguranca",
"Cultura Hacker",
"Arch Linux",
]
categories: ["Impressões"]
description: "Experimente a nova distribuição baseada no Arch Linux voltada para pentesters e demais profissionais de segurança da informação."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
Experimente a nova distribuição baseada no Arch Linux voltada para pentesters e demais profissionais de segurança da informação.

<!--more-->

## Introdução

**U**ma boa novidade para os profissionais de segurança: <a href="https://www.blackarch.org" target="_blank">BlackArch</a>! Para quem, assim como eu, gosta de como as coisas funcionam no Arch Linux essa é uma notícia particularmente boa, visto que o BlackArch não se trata realmente de uma nova distribuição, mas sim de uma *extensão* para o Arch Linux. Como assim? Bom, você possui duas opções para utilizar o BlackArch, sendo uma delas como uma distribuição completa, através de um Live CD, por exemplo, e a outra como uma extensão (um repositório de pacotes) para o Arch Linux, onde você poderá apenas inserir um repositório em sua já existente distribuição Arch Linux e ter acesso ao conjunto de ferramentas do BlackArch.

**O** BlackArch, atualmente, possui suporte para as arquiteturas i686 e x86_64, com previsão de suporte para ARM em breve (Sim, meu RaspBerry poderá se tornar uma ferramenta para pentests). No mais, o BlackArch hoje possui mais de 600 ferramentas, estando este número crescendo constantemente, e utiliza grupos modulares de pacotes, facilitando a instalação dos mesmos.

**A** ISO Live trás diversos gerenciadores de janelas ou ambientes gráficos, como o dwm, Fluxbox, Openbox, Awesome, Wmii, i3 e Spectrwm. É claro, ele também trás um instalador capaz de instalar a partir do fonte.

**D**entre as ferramentas existentes estão: 3proxy, 42zip, acccheck, aesfix, against, airflood, airoscript, bluepot, blueprint, braces, bss, bully, cisco-ocs, cmospwd, dbd, dc3dd, deblaze, dhcpig, enumiax, fakedns, ... Vocẽ não espera que eu liste todos os mais de 600, certo?

[Openbox](https://blackarch.org/images/screenshots/openbox.png)

[Black OpenBox](openbox.png)

## Configurando como um Repositório Não Oficial

**S**e você já possui o Arch Linux instalado e deseja apenas inserir o BlackArch como um repositório em sua distro, execute os seguintes comandos como root, os quais servirão para assinar os pacotes: *(Se você não possui o Arch Linux instalado e/ou simplesmente deseja rodar o Live CD ou instalar o mesmo por completo, seja em uma máquina física ou virtual, siga para a seção Instalando o BlackArch Linux utilizando a Live-ISO.

```bash
 # wget -q https://blackarch.org/keyring/blackarch-keyring.pkg.tar.xz{,.sig}

 # gpg --keyserver hkp://pgp.mit.edu --recv 4345771566D76038C7FEB43863EC0ADBEA87E4E3

 # gpg --keyserver-o no-auto-key-retrieve --with-f blackarch-keyring.pkg.tar.xz.sig

 # pacman-key --init

 # rm blackarch-keyring.pkg.tar.xz.sig

 # pacman --noc -U blackarch-keyring.pkg.tar.xz
```

**E**m seguida, adicione as seguintes linhas ao seu arquivo */etc/pacman.conf*:

```bash
  [blackarch]
  Server = <mirror_site>/$repo/os/$arch
```

**Substitua** *<mirror_site>* por um mirror de sua escolha, preferencialmente um dos **mirrors oficiais** contidos [neste link](https://blackarch.org/download.html#mirrors).

**U**ma vez que você tenha seguido os passos acima, execute:

```bash
 $ sudo pacman -Syyu
```

## Instalando os pacotes

**A**gora que você já preparou o terreno assinando e configurando o repositório do Black Arch, basta instalar os pacotes em seu Arch Linux.

**P**ara listar todas as ferramentas disponíveis, execute:

```bash
 $ sudo pacman -Sgg | grep blackarch | cut -d' ' -f2 | sort -u
```

**P**ara instalar todas as ferramentas, execute:

```bash
 $ sudo pacman -S blackarch
```

**P**ara instalar uma categoria de ferramentas, execute:

```bash
 $ sudo pacman -S blackarch-<categoria>
```

**P**ara ver as categorias existentes no BlackArch, execute:

```bash
 $ sudo pacman -Sg | grep blackarch
```

## Instalando o BlackArch Linux utilizando a Live-ISO

**A**ntes de mais nada, [baixe a ISO a partir do site oficial](https://blackarch.org/download.html.

**E**m seguida, dê boot na ISO e instale o script de instalação do BlackArch:

```bash
 $ sudo pacman -S blackarch-install-scripts
```

**A**gora, basta instalar:

```bash
 # blackarch-install
```

**H**appy Hacking!
