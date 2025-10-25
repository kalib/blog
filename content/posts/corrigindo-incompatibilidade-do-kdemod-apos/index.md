---
title: "Corrigindo Incompatibilidade Do Kdemod Após O Nascimento Do Kde4.1 No Arch"
date: 2008-07-30T16:06:40-04:00
draft: false
tags: [
"Arch Linux",
"KDE",
"Linux",
"Software Livre",
]
categories: ["Tutoriais"]
description: "Os usuários de Arch Linux que utilizam o kdemod como ambiente gráfico e que tentaram fazer uma atualização do sistema (pacman -Syu) nos últimos dias devem ter notado que havia um problema. Como o KDE 4.1 entrou para os repositórios do Arch, passou a haver uma incompatibilidade entre alguns pacotes."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
Os usuários de Arch Linux que utilizam o kdemod como ambiente gráfico e que tentaram fazer uma atualização do sistema (pacman -Syu) nos últimos dias devem ter notado que havia um problema. Como o KDE 4.1 entrou para os repositórios do Arch, passou a haver uma incompatibilidade entre alguns pacotes. E a atualização do sistema pedia que você substituisse alguns pacotes do kdemod pelos do kde.

<!--more-->

Logo que percebi este problema busquei ajuda em fórums mas ninguém tinha a solução, portanto decidi reportar este bug para os desenvolvedores do kdemod. A minha surpresa foi chegar no site de bugreport do kdemod e ver que este bug já havia sido identificado. Achei ótimo ver que o bug já havia sido notificado e que já estavam trabalhando em cima do mesmo, como o próprio desenvolvedor "funkyou" me informou.

Hoje, 30/07, o problema já se encontra solucionado e o bug foi fechado no bugreport deles. É preciso seguir um procedimento para normalizar a situação, já que os pacotes do antigo kdemod foram re-construídos e re-empacotados como kdemod3 para fazê-los funcionar no novo upgrade do Arch.

Mudanças que foram feitas:

- Todos os pacotes foram renomeados para kdemod3-$PACKAGE

- KDEmod 3.5.9 agora conflita com KDE 4.1, então não é possível ter ambos instalados simultaneamente

- Não é uma atualização muito comum, você precisará remover todos os pacotes do kdemod primeiramente para depois reinstalar

- Note que muitas coisas do [community] por exemplo precisam ser adaptados ao novo KDE3 renomeando no Arch

- Não force nada, o melhor é reportar os problemas no link a seguir: [https://www.kdemod.ath.cx/bbs/viewtopic.php?pid=5303#p5303](https://www.kdemod.ath.cx/bbs/viewtopic.php?pid=5303#p5303)

- Este repositório deixará de ser suportado até que alguém passe a lhe dar mais amor como nós acasionalmente fazemos  

INSTALAÇÃO**:**  

**1. **Remover todos os pacotes do KDE4: pacman -Rcs kde  

**2. **Remover todos os pacotes do KDEmod4: pacman -Rcs kdemod4-complete  

**3. **Remover todos os pacotes do KDEmod3: pacman -Rcs kdemod-complete  

**4. **Remover _qualquer_ pacote do KDE que ainda exista em seu sistema  

**5. **Checar novamente se tudo foi removido: pacman -Q | grep kde  

**6. **Remover o antigo repositório [kdemod] do seu pacman.conf  

**7. **Adicionar o novo repositório [kdemod-legacy] ao seu pacman.conf

Para i686

```bash
[kdemod-legacy]
Server = https://kdemod.ath.cx/repo/legacy/
```

Para x86_64

```bash
[kdemod-legacy]
Server = https://kdemod.ath.cx/repo/legacy/x86_64
```

****Em seguida atualize com: pacman -Suy

****Instale com:  

**pacman -S kdemod3** (sistema funcional básico)  

ou  

**pacman -S kdemod3-complete** (instalação completa do KDEmod)  

ou  

**pacman -S kdemod3-vanilla** (instalação completa do KDEmod, sabor vanilla (baunilha))  

ou  

**pacman -S kdemod3-base** (instalação básica do KDE (base))

****Instale sua língua(cheque os disponíveis com **pacman -Ss kdemod3-kde-i18n**)

Feito. ;]
