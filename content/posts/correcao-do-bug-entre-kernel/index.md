---
title: "Correção Do Bug Entre Kernel Linux E Placas Broadcom"
date: 2013-09-03T08:35:42-05:00
draft: false
tags: [
"Linux",
"Software Livre",
"Redes",
]
categories: ["Impressões"]
description: "Bug entre kernel 3.10.x e placas wifi broadcom resolvido para o kernel 3.11.x"
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**B**ug capturado e tratado!

**D**iversas pessoas se depararam com intermináveis erros de *kernel panic* após atualizar seu kernel para a versão *3.10.6* (e posteriores) quando utilizavam placas wireless da *Broadcom*. O erro reportado foi tratado e, apesar de ainda não estar resolvido no último kernel lançado, *3.10.10*, já se encontra na árvore de *Linus Torvalds*, conforme código e correção podem ser vistos através do seguinte link:

<!--more-->

[mac80211: add a flag to indicate CCK support for HT clients](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=2dfca312a91631311c1cf7c090246cc8103de038)

**O** problema se dava com o módulo brcm80211 que não conseguia lidar adequadamente com o envio de frames com taxas CCK como parte de uma sessão A-MPDU. Apesar de outros drivers/módulos da broadcom conseguirem conectar-se sem o kernel panic logo na inicialização do sistema, estes ainda podem enfrentar problemas em outros momentos sem esta correção. O patch corrige o erro reportado com módulos brcmsmac bem como outros.

**A**tualmente estou utilizando o último kernel disponibilizado, o *3.10.10*. Apesar de a correção ainda não ter saído nesta versão, estou utilizando uma versão do kernel com o patch já aplicado.

**E**speremos pelo kernel *3.11* que deverá ser lançado nos próximos dias e que, por sua vez, deverá resolver este problema definitivamente.
