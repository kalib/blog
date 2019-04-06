---
title: "Instalação Do Kdemod 4.1 No Arch Linux"
date: 2008-08-03T16:01:08-04:00
draft: false
image: "/img/kdemod.jpg"
tags: [
"Arch Linux",
"KDE",
"Linux",
"Software Livre",
]
description: "Creio que todos os usuários de Arch Linux conhecem ou já ouviram falar do Hugo Doria. O Hugo é um dos maiores representantes do Arch Linux no Brasil e um dos atuais desenvolvedores do mesmo."
---
Creio que todos os usuários de Arch Linux conhecem ou já ouviram falar do [Hugo Doria](https://hugodoria.org/). O Hugo é um dos maiores representantes do Arch Linux no Brasil e um dos atuais desenvolvedores do mesmo.




Este artigo sobre instalação do KDEmod 4.1 foi escrito por ele e resolvi postar aqui para que possamos atingir mais público ainda. Quanto mais a informação correr, melhor. ;]  



Introdução




No _Arch Linux_ o KDE é empacotado de um jeito que eu não curto. Nele, diversas aplicações são "englobadas" em um só pacote (isso é o padrão na maioria das distribuições).




Ao instalar o pacote _kdegraphics_, por exemplo, você acaba instalando diversas aplicações para manipulação gráfica. Mesmo que você queira apenas a "aplicação x" daquele pacote será obrigado a instalar o kdegraphics todo. Isso torna o KDE enorme e com diversas aplicações que você nunca usa.




A solução para isto é usar o [KDEMod](https://www.kdemod.ath.cx/), um projeto que visa fornecer uma instalação modular do KDE e otimizada para o Arch Linux.




NOTA: Não há planos para portar o KDEMod para outras distribuições.




Instalação




O primeiro passo a ser feito é remover toda e qualquer instalação do _KDEMod_ anterior. Faça isso com:




> # pacman -Rscn kdemod-complete
>
>





Se você já está usando as versões betas do kdemod4, também faça:




> # pacman -Rscn kdemod4-complete
>
>





Verifique se tudo relacionado ao kde foi removido com:




> # pacman -Q | grep kde
>
>





Se aparecer algo, pode remover. Uma maneira mais direta de fazer isto é executando:




> # pacman -Q | grep -i "kde|qt" | cut -d " " -f1 | xargs pacman -Rd
>
>





É preciso, também, remover os arquivos de configuração do kdemod 3 e 4 anteriores, já que eles são incompatíveis com a nova versão:




> $ rm -rf ~/.kde* ~/.kdemod*
>
>





NOTA: Ao invés de removê-los eu sugiro que você faça um backup.




Pronto, seu sistema está limpo e agora podemos instalar a nova versão sem problemas. Abra o arquivo _/etc/pacman.conf_ com seu editor preferido e adicione:







> [kdemod-core]  

Server = [https://kdemod.ath.cx/repo/core/i686](https://kdemod.ath.cx/repo/core/i686)
>
>








NOTA:  








  * Remove qualquer entrada referente ao [kdemod] ou ao [kdemod-unstable]




Para instalar o KDEMod faça:




> # pacman -Sy kdemod
>
>





Se quiser o kdemod completo faça:




> # pacman -Sy kdemod-complete
>
>





Seu KDEMod está pronto para uso. :) Para usá-lo basta editar seu ~/.xinitrc ou iniciar o kdm (gerenciador de login do KDE) com:




> # /etc/rc.d/kdm restart
>
>





O primeiro boot do KDE 4 demora um pouco, mas depois melhora. Portanto, não se preocupe.




Caso você queira seu ambiente em português instale o pacote abaixo:




> # pacman -Sy kdemod-kde-l10n-pt_br
>
>





E selecione seu idioma nas configurações do KDE.




Pacotes Adicionais




Agora existe um repositório com alguns pacotes adicionais para o KDE (aplicativos, plasmoids etc). Para usá-lo adicione as seguintes linhas ao _/etc/pacman.conf_:







> [kdemod-extragear]  

Server = https://kdemod.ath.cx/repo/extragear/i686
>
>








E sincronize a base de dados dos pacotes:  






> # pacman -Sy
>
>





Para visualizar o que existe neste repositório faça:




> # pacman -Sl kdemod-extragear
>
>





Por enquanto ainda existem poucos pacotes neste repositório, mas vários outros estão sendo adicionados.




Bem, é isso. Agora é só você aproveitar seu KDEMod 4. No momento que estou escrevendo este artigo ele se encontra na versão 4.1 e já está ficando fantástico.  



Maiores informações:






  * [https://www.kdemod.ath.cx/](https://www.kdemod.ath.cx/)




Deixo aqui os agradecimentos ao Hugo em nome de todos pela contribuição.
