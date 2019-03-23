---
title: "Chromium No Testing Do Arch Linux"
date: 2009-12-14T10:13:33-04:00
draft: false
image: "/img/chromium.jpg"
tags: [
"Arch Linux",
"Chromium",
"Cultura Hacker",
"Impressões",
"Software Livre",
]
description: "Para quem já vem acompanhando este projeto desde seu início, tenho o prazer de anunciar esta novidade nos repositórios do Arch."
---
**S**audações pessoal...

**J**á estava na hora eim?! Chromium passando para o repositório [testing] no Arch.

**P**ara quem já vem acompanhando este projeto desde seu início, tenho o prazer de anunciar esta novidade nos repositórios do Arch.

**P**or questões de segurança e estabilidade para os usuários como um todo, resolvemos manter também o pacote em nosso repositório nacional [archlinux-br] para evitar que os usuários do chromium tenham que habilitar o [testing] em seus sistemas correndo alguns riscos por pacotes pouco testados.

**A**ssim que o mesmo passar para repositórios mais confiáveis como o [extra], estaremos removendo do repositório nacional.

**C**om algumas mudanças nesta versão do pacote, não estamos mais utilizando o snapshot, mas sim a versão já considerada beta do próprio projeto chromium.

**P**ara os que já utilizam o repositório [archlinux-br] e já possuem o chromium instalado, basta mandar atualizar seu sistema normalmente com:

_# pacman -Syu_

**P**erceberá que o pacman informará que o chromium-snapshot precisará ser removido para a instalação do chromium. Podem confirmar sem medo algum. Ontem mesmo eu e [thotypous](https://matias.archlinux-br.org/) realizamos os últimos testes neste sentido e tudo correrá bem.

**P**ara aqueles que ainda não utilizam o repositório [archlinux-br] e ainda não possuem o chromium instalado, a dica é a seguinte:

**1-** Habilite o repositório inserindo as seguintes linhas no arquivo /etc/pacman.conf:

_[archlinux-br]
Server = https://repo.archlinux-br.org/i686_

**2-** Atualize seus mirros e instale o chromium:

_# pacman -Sy chromium_

**U**ma terceira opção, indicada para os mais aventureiros, seria habilitar o repositório [testing]. Esta opção eu apenas recomendo para os que estão dispostos a ajudar mais diretamente no desenvolvimento do Arch e que desejem assumir alguns riscos de estabilidade, visto que muitos pacotes ali ainda se encontram em estado de teste. Neste caso, basta descomentar as linhas referente a ele no arquivo /etc/pacman.conf:

_[testing]
Include = /etc/pacman.d/mirrorlist_

**F**açam bom proveito pessoal. ;]

**U**sem com moderação.. :p
**A**braços
