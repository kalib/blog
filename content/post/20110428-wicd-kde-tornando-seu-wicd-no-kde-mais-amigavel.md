---
title: "Wicd Kde - Tornando Seu Wicd No Kde Mais Amigavel"
date: 2011-04-28T15:11:52-05:00
draft: false
image: "/img/wifi.jpeg"
tags: [
"Arch Linux",
"Impressões",
"KDE",
"Linux",
"Software Livre",
]
description: "Para aqueles que também não curtem de forma alguma a aparência padrão GTK do Wicd e não conhecem ainda a interface que foi lançada posteriormente em QT, fica a dica: Troquem com urgência!"
---
**S**audações pessoal,

**P**ara aqueles que também não curtem de forma alguma a aparência padrão GTK do Wicd e não conhecem ainda a interface que foi lançada posteriormente em QT, fica a dica: Troquem com urgência!

**E** o network-manager?

**B**om, o network-manager é um excelente(sendo modesto) gerenciador de redes para o Linux, porém infelizmente ele peca muito em sua interface para o KDE. Compare o network-manager em suas interfaces gtk (Gnome) e qt (KDE) e entenderá o que eu digo. A versão para KDE é extremamente confusa e bagunçada em comparação com a interface para o Gnome.

**E**is que muitas distribuições começaram a sugerir a utilização do Wicd como gerenciador de redes para quem utiliza o KDE.

**N**o Arch, por exemplo, encorajamos fortemente o uso do Wicd com o KDE ao invés do network-manager. Sim, sempre utilizei ele, porém...


![Wicd GTK](/img/wicdgtk.png)


**E** então? Meio bizarro, certo?! Alguém havia pensado em dizer "Mas ele não é tão feio.."?

**B**om, que tal apresentar uma alternativa a isto? O wicd-kde!

**C**omo uma imagem vale mais que mil palavras:


![Wicd QT](/img/wicdqt.png)


**M**ais amigável certo? Uma interface mais limpa, organizada e bonita. Bem ao estilo "clean", como alguns preferem dizer.

**M**as, deixando um pouco a frescura de lado. Não recomendo a utilização da interface em qt apenas por questões de estética.

**A** verdade é que o seu ambiente fica mais limpo com esta interface. Limpo? Sim.

**S**e você utiliza o KDE mas possui aplicações em gtk, por consequência você possuirá uma série de bibliotecas instaladas em seu ambiente para fazer com que esta aplicação em gtk funcione da maneira correta. Porém, se você instala uma aplicação em qt, pouca coisa adicional será necessária, visto que seu sistema já está preparado para rodar este tipo de aplicação.

**D**igamos que é uma união: Útil ao Agradável!

E a parte que interessa? Instalar, instalar, instalar!

**I**nfelizmente, nem todas as distribuições empacotaram o wicd-kde até então, mas se você é usuário **[Arch Linux](https://archlinux.org)**, a sua vida será mais fácil.

**O** pacote wicd-kde encontra-se pronto no [AUR](https://aur.archlinux.org/packages.php?ID=40735).

**O** processo é o já manjado:

**1-** Baixar o arquivo compactado:


$ wget https://aur.archlinux.org/packages/wicd-client-kde-git/wicd-client-kde-git.tar.gz


**2-** descompactar:


$ tar -xvzf wicd-client-kde-git.tar.gz


**3-** entrar no diretório e em seguida executar o PKGBUILD para geração do pacote em si:


$ cd wicd-client-kde-git

$ makepkg


**4-** Instalar o pacote gerado:


# pacman -U wicd-client-kde-git-20110426-1-x86_64.pkg.tar.xz


**F**eito. ;]

**H**ave Fun!

**A**braços!
