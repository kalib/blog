---
title: "Resolvendo Alguns Problemas Causados Pelo Novo Xorg No Arch"
date: 2008-12-02T11:48:33-04:00
draft: false
image: "/img/archlibjpeg.png"
tags: [
"Arch Linux",
"Linux",
"Software Livre",
]
description: "Não é de hoje que vejo o nível de alguns pacotes essenciais como o Kernel e o Xorg caírem cada vez mais. É triste porém uma realidade. A maneira como tais pacotes vem sendo lançados sem maiores preocupações está prejudicando a vida de muita gente. Eu fui um deles recentemente ao atualizar o meu Xorg. Para aqueles que também fizeram upgrade recentemente, podem ter aparecido vários problemas."
---
Não é de hoje que vejo o nível de alguns pacotes essenciais como o Kernel e o Xorg caírem cada vez mais. É triste porém uma realidade. A maneira como tais pacotes vem sendo lançados sem maiores preocupações está prejudicando a vida de muita gente. Eu fui um deles recentemente ao atualizar o meu Xorg. Para aqueles que também fizeram upgrade recentemente, podem ter aparecido vários problemas.




Aqui apresentarei as soluções que utilizei no ArchLinux para sanar os problemas que me apareceram, sendo eles:




1- O X não subia mais. Simplesmente morria e ficava tudo preto.  

2- As teclas especiais do teclado não respondiam mais, como por exemplo as setas direcionais.




Vamos às soluções encontradas...




1- O X não sobe mais




1.1- Para este problema, precisei fazer uma pequena busca pelo erro que acontecia, para isso busquei em primeira mão os logs do Xorg. Você pode fazer isso com o seguinte comando:




# tail -f /var/log/Xorg.0.log




1.2- Em meu caso, apontou o erro na sintax RgbPath, a qual informava ser incompatível. Muita gente está passando por este problema.




A solução neste caso é simples, basta editar o arquivo /etc/X11/xorg.conf e comentar a linha:  

RgbPath = "/usr/share/X11/rgb"




1.3- Ao comentar esta linha, tudo o que fiz foi reinstalar o driver de minha placa de vídeo, para que meu xorg voltasse a funcionar. Como eu utilizo um driver da ATI Radeon que costumo baixar diretamente no site da ATI, segui o procedimento padrão para os drivers baixados do mesmo:




1.3.1- Dar permissão de execução:  

# chmod +x ati-driver-installer-8-11-x86.x86_64.run




1.3.2- Executar o instalador:  

# ./ati-driver-installer-8-11-x86.x86_64.run




Pronto. Feitos estes passos, eu já conseguia subir meu servidor X numa boa.




Com relação ao segundo problema. As teclas...




2- As teclas especiais do teclado não respondem.  

2.1- A solução foi simples, bastou uma pequena busca na wiki do ArchLinux com uma dica de como corrigir isto no meu KDE4. Indo para as configurações do sistema e partindo para a configuração de "Regional & Idioma". Lá busque a aba "Layout de Teclado".




2.2- Nesta você precisará habilitar a opção "Habilitar Layouts de Teclado" e selecionar como Modelo de Teclado a opção "Evdev-managed keyboard".




Feito isto, as teclas voltaram a funcionar no meu caso bem como outras pessoas que também testaram.
