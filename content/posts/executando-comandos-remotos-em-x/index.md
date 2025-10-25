---
title: "Executando Comandos Remotos Em X Servidores Com O Konsole"
date: 2009-03-06T21:27:50-04:00
draft: false
tags: [
"Arch Linux",
"Cultura Hacker",
"Impressões",
"KDE",
"Linux",
"Redes",
"Software Livre",
]
description: "Algum dia precisou digitar o mesmo comando em mais de um servidor ou realizar a mesma configuração? Não gostou de ter que executar essa tarefa várias vezes, sendo uma em cada máquina?"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
Algum dia precisou digitar o mesmo comando em mais de um servidor ou realizar a mesma configuração? Não gostou de ter que executar essa tarefa várias vezes, sendo uma em cada máquina?

<!--more-->

Porque não usar a tecnologia a seu favor?

Sem muitos truques e malabarismos, o próprio konsole do kde pode resolver isso. Sim, o konsole padrão do kde possui uma função interessante para execução de comandos em múltiplos terminais/servidores.

O procedimento é bem simples como vocês podem perceber abaixo.

Abra o seu konsole e em seguida inicie uma nova aba no mesmo. Para isso clique em Arquivo e selecione "Nova Aba". Sua aba será aberta e ficará indicada na barra de baixo. Porém, de nada vale abrirmos mútliplas abas se não pudermos ver as mesmas, certo?! Então clique no botão Exibir e seleciona a opção "Dividir a exibição". Lhe serão indicadas as possibilidades disponíveis e você pode escolher a que lhe for de melhor agrado. Eu optei pela divisão "Esquerda/Direita".

Agora sim, estamos vendo ambas as abas, certo!?

Repare que mesmo vendo ambas, você tem que digitar o comando em uma e em outra.

Vamos agora acessar os servidores/terminais remotos nos quais precisamos fazer a operação. Para isso na primeira aba acesse o servidor/estação de sua preferência, repetindo o mesmo na segunda aba.  

(Não convém especificar aqui como você se logará em outros servidores/terminais. Eu utilizei conexão via ssh em meu exemplo que poderá ser visto no vídeo após o post.)

Após estar logado à um servidor/terminal diferente em cada aba, partiremos para o truque de espelhamento/repetição do comando.

Clique na opção Editar do konsole e acesse a aba "Copiar a entrada para..".  Lhe será exibida uma janela com suas abas abertas. Uma delas já vem marcada, pois é a que está ativa no momento. Marque a segunda e confirme clicando no botão Ok.

Feito isto, repare que os comandos que você executar na Aba primária serão repetidos na seguinte.

Simples, certo?! ;]

Abaixo disponibilizo um vídeo de demonstração de minha máquina executando a tarefa.  

(Segue em três formatos para que você escolha o que achar melhor.)

[Formato ogg](https://www.marcelocavalcante.net/repositorio/multikonsole.ogg)  
[Formato avi](https://www.marcelocavalcante.net/repositorio/multikonsole.avi)
[Formato flv](https://www.marcelocavalcante.net/repositorio/multikonsole.flv)
