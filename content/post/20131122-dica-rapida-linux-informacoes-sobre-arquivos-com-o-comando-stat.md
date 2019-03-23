---
title: "Dica Rápida Linux: Informações Sobre Arquivos Com O Comando Stat"
date: 2013-11-22T23:54:53-05:00
draft: false
image: "/img/information.png"
tags: [
"Linux",
"Software Livre",
]
description: "Consiga informações mais detalhadas sobre um arquivo pela linha de comando através da ferramenta stat"
---
**Q**ue os sistemas GNU/Linux possuem uma infinidade de comandos todo mundo sabe, o que nem todos conhecem, na verdade, são alguns comandos simples porém eficientes e importantes. Um deles é justamente o *stat*.

**Q**uando se está em frente ao terminal de um servidor que não possui interface gráfica, tudo o que está a nossa disposição são as ferramentas de linha de comando, portanto é bom conhecer uma boa variedade das mesmas, desde ferramentas para tarefas complexas até ferramentas para as atividades mais simples e banais.

**A** dica que deixo hoje é uma ferramenta que muitas pessoas desconhecem: *stat*

**O** stat serve para apresentar as informações de status de um arquivo ou sistema de arquivos. Ele apresenta uma série de informações sobre o arquivo que você informar como argumento. Dentre as informações estão o *Tamanho*, *Blocos*, *Permissões de Acesso*, *Data e Hora de último acesso*, *Data e Hora de última modificação*, etc.

**O** uso é simples, bastando digitar: *stat <caminho_do_arquivo_ou_sistema_de_arquivos>*.

```
 [kalib@tuxcaverna ~]$ stat testdisk.log
  File: “testdisk.log”
  Size: 102478          Blocks: 208        IO Block: 4096   arquivo comum
 Device: 804h/2052d      Inode: 27001862    Links: 1
 Access: (0644/-rw-r--r--)  Uid: ( 1000/   kalib)   Gid: (  100/   users)
 Access: 2013-11-11 11:31:34.360892496 -0300
 Modify: 2011-05-10 09:48:46.000000000 -0300
 Change: 2011-05-11 15:01:29.019407886 -0300
 Birth: -
```

ou

```
 [kalib@tuxcaverna ~]$ stat /dev/sda1
  File: “/dev/sda1”
  Size: 0               Blocks: 0          IO Block: 4096   arquivo especial de bloco
 Device: 5h/5d   Inode: 7253        Links: 1     Device type: 8,1
 Access: (0660/brw-rw----)  Uid: (    0/    root)   Gid: (    6/    disk)
 Access: 2013-11-22 08:01:39.246618958 -0300
 Modify: 2013-11-22 08:01:39.246618958 -0300
 Change: 2013-11-22 08:01:39.246618958 -0300
 Birth: -
```

**H**ave fun!
