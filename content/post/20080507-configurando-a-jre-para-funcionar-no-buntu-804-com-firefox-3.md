---
title: "Configurando a Jre Para Funcionar No *Buntu 8.04 Com Firefox 3"
date: 2008-05-07T12:49:34-04:00
draft: false
image: "/img/java-ubuntu.png"
tags: [
"Firefox",
"Java",
"Kubuntu",
"Linux",
"Software Livre",
]
description: "Aqui segue uma rápida dica que poderá ajudar aqueles que, assim como eu, instalaram o *buntu 8.04 e perceberam que o mesmo possui o Firefox 3.0 em seus repositórios como o navegador padrão."
---
Saudações galera..




Aqui segue uma rápida dica que poderá ajudar aqueles que, assim como eu, instalaram o *buntu 8.04 e perceberam que o mesmo possui o Firefox 3.0 em seus repositórios como o navegador padrão.




Bom, ao tentar instalar, desde ontem, o plugin do java para poder acessar minha página do banco, encontrei uma solução bem simples depois de várias tentativas frustradas.




Após tentar tudo que sempre utilizei como por exemplo baixar o pacote jre diretamente do site da sun, descompactar no diretório /usr/java (como sempre fiz por questão de organização), instalar e criar um link simbólico para o mesmo no diretório de plugins do Firefox, e mesmo assim não ter nenhum resultado concreto, pesquisei um pouco e achei uma solução bem mais simples e rápida.




Basta descomentar as linhas referentes aos mirrors multiverse nos seus repositórios (/etc/apt/sources.list), dar um update (# aptitude update) e em seguida instalar os pacotes com o seguinte comando:




# aptitude install sun-java6-jre sun-java6-plugin sun-java6-fonts




Feito isto reinicie o firefox e digite em sua barra de endereços:




about:plugins




Deverá agora constar em sua lista de plugins os referentes ao java. ;]




Outro teste é digitar no seu console: $ java -version




Você deverá ter um retorno similar a este:




java version "1.6.0_06"  

Java(TM) SE Runtime Environment (build 1.6.0_06-b02)  

Java HotSpot(TM) Client VM (build 10.0-b22, mixed mode, sharing)




Ou mesmo acessando alguma página que se utilize de java como o teclado virtual do Banco do Brasil por exemplo.  

;]




Bom, espero ter ajudado com esta simples, porém eficiente dica.




Abraços
