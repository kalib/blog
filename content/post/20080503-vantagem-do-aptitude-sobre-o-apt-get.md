---
title: "Vantagem Do Aptitude Sobre O Apt Get"
date: 2008-05-03T12:53:01-04:00
draft: false
image: "/img/aptget.jpg"
tags: [
"Aptitude",
"Debian",
"Linux",
"Software Livre",
]
description: "Novamente estou aqui para lhes passar uma pequena dica que para muitos pode nem ser novidade, porém para alguns a dúvida pode existir."
---
Novamente estou aqui para lhes passar uma pequena dica que para muitos pode nem ser novidade, porém para alguns a dúvida pode existir.

Bom, para aqueles que ainda não sabem, o apt é uma ferramenta da Debian para gerenciamento de pacotes de forma simples, amigável e rápida contando inclusive com a instalação automática de dependências necessárias para a finalização do processo. O que muitas pessoas ainda não sabem é que utilizando-se do comando "apt-get install NOME_PACOTE" serão instalados pacotes que o mesmo não removerá automaticamente posteriormente, fazendo assim um acúmulo de "lixo" em nosso sistema. Como assim? Suponhamos que eu queira instalar um aplicativo de instant messenger como por exemplo o amsn. Esta ferramenta possui dependências necessárias para seu funcionamento, sendo elas o TCL e o TK.

O seguinte comando fará a instalação do amsn juntamente com suas dependências, sem que eu precise me preocupar em buscar por elas desesperadamente na internet:

#apt-get install amsn

Ótimo! Agora tenho o meu messenger devidamente instalado, sem nenhuma dificuldade e funcionando perfeitamente. Porém, um certo dia resolvi remover essa ferramenta que com o tempo parei de usar, então para isso utilizo o seguinte comando:

#apt-get remove amsn

Perfeito! Meu amsn está desinstalado sem dificuldade alguma. ;]

Agora, e o que acontece com os dois pacotes que foram instalados juntamente com ele anteriormente? TCL e TK? Bom, eles continuam instalados, fazendo um certo acúmulo de  "lixo" em seu sistema. O mesmo ocorre com todos os pacotes que forem instalados em seu sistema e futuramente removidos com o apt-get.

Onde entra o Aptitude nessa história?

Bom, o aptitude tem um funcionamento bem semelhante para a instalação de pacotes. Passaremos a adotar o mesmo cenário aqui, instalando portanto o amsn:

#aptitude install amsn

Assim como o apt-get, o aptitude irá instalar automaticamente as dependências do amsn, TCL e TK. Passado algum tempo, resolvo remover o amsn usando o seguinte comando:

#aptitude remove amsn

Aparentemente ele terá o mesmo efeito do apt-get, com o grande diferencial de  excluir juntamente com o amsn, as suas dependências que outrora foram instaladas, TCL e TK.

Imagine a quantidade de pacotes desnecessários que deve existir em sua máquina...provavelmente vários. O aptitude é uma solução para que isto não ocorra mais.

Para os fans de distribuições como o Fedora que utilizam-se da ferramenta Yum para instalar seus pacotes, caso tenha surgido a curiosidade, fica a informação de que, infelizmente, o yum ainda não possui este mecanismo. A mesma curiosidade surgiu em mim e resolvi testar, porém o yum, assim como o apt-get, apenas me removeu o amsn, deixando para trás as dependências que foram instaladas.

Essa foi uma simples dica para aqueles que desconheciam este fato diferencial dentre os dois. Espero ter ajudado com esta pequena contribuição para com a comunidade. abraços e até a próxima.
