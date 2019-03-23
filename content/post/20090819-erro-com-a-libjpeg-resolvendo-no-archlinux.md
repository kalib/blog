---
title: "Erro Com a Libjpeg - Resolvendo No Archlinux"
date: 2009-08-19T12:18:36-04:00
draft: false
image: "/img/archlibjpeg.png"
tags: [
"Arch Linux",
"Linux",
"Software Livre",
]
description: "Quem está utilizando o Archlinux devidamente atualizado, pode ter percebido alguns erros relacionados à libjpeg.so ao tentar executar alguma aplicação, como foi o meu caso com o floola, um gerenciador para meu ipod com o qual transfiro e manipulo minhas músicas pelo linux."
---
**Q**uem está utilizando o [Archlinux](https://archlinux.org) devidamente atualizado, pode ter percebido alguns erros relacionados à libjpeg.so ao tentar executar alguma aplicação, como foi o meu caso com o [floola](https://www.floola.com), um gerenciador para meu ipod com o qual transfiro e manipulo minhas músicas pelo linux.

**O** problema se deu pelo fato de o Archlinux já ter adotado a nova libjpeg. Esta versão 7 possui algumas vantagens em relação a versão anterior, porém muitos aplicativos ainda não foram atualizados para esta nova versão. Este foi o caso do floola por exemplo, que só funciona com a libjpeg6.

**S**egue uma dica simples e rápida para resolver este pequeno problema.

**1-** Baixe o pacote da libjpeg6 diretamente do [AUR](https://aur.archlinux.org) através do seguinte link:


> https://aur.archlinux.org/packages/libjpeg6/libjpeg6.tar.gz


**2-** Descompacte o arquivo:


> [kalib@tuxcaverna downloads]$ tar -xvzf libjpeg6.tar.gz


**3-** Entre no diretório libjpeg6, crie e instale o pacote a partir do PKGBUILD extraído:


> [kalib@tuxcaverna downloads]$ cd libjpeg6/

[kalib@tuxcaverna libjpeg6]$ makepkg

[kalib@tuxcaverna libjpeg6]$ pacman -U libjpeg6-6b-6-i686.pkg.tar.gz


**F**eito isto, pode executar novamente o software que estava apontando o erro. No meu caso foi o floola. Isto resolverá o seu problema. ;]

**A**braços
