---
title: "Dica Rápida Linux: Como Não Deixar Rastros No History"
date: 2013-11-07T23:58:55-05:00
draft: false
image: "/img/shhh.jpg"
tags: [
"Linux",
"Software Livre",
"Segurança",
]
description: "Saiba como digitar comandos no Linux sem que eles fiquem registrados no history ou histórico de comandos do sistema."
---
**S**hhhhhhh....

**Q**ue tal digitar seus comandos em uma máquina Linux sem que os mesmos sejam registrados no *history* do sistema?

**O** history é responsável por armazenar um histórico dos últimos comandos digitados no sistema. Por exemplo:

```
 [kalib@tuxcaverna ~]$ history
    1  ls
    2  mkdir pc
    3  cd pc/
    4  ls
    5  mkdir pc2
    6  ls
    7  touch ps2/teste
    8  ls
    9  touch /
    ...
    ...
    496  route
    497  ifconfig
    498  cat teste
    499  ls
    500  history
```

**C**omo podemos ver, o history me retornou um histórico dos meus últimos 500 comandos, incluindo o próprio comando *history*, que acabei de digitar.

**V**ejamos o registro de novos comandos:

```
 [kalib@tuxcaverna ~]$ echo "Teste"
 Teste
 [kalib@tuxcaverna ~]$ echo "Registra isso history"
 Registra isso history
 [kalib@tuxcaverna ~]$ history
 ...
 ...
 501  echo "Teste"
 502  echo "Registra isso history"
 503  history
```

**E**ntão, como não deixar registros no history? Utilizaremos os comandos *cat* e *bash* para isto:

```
 [kalib@tuxcaverna ~]$ cat | bash
 pwd
 /home/kalib

 df -h
 Sist. Arq.      Tam. Usado Disp. Uso% Montado em
 /dev/sda3        30G   19G  9,3G  67% /
 dev             2,9G     0  2,9G   0% /dev
 run             2,9G  860K  2,9G   1% /run
 tmpfs           2,9G     0  2,9G   0% /dev/shm
 tmpfs           2,9G     0  2,9G   0% /sys/fs/cgroup
 tmpfs           2,9G  108K  2,9G   1% /tmp
 /dev/sda1        99M   23M   69M  25% /boot
 /dev/sda4       427G  305G  101G  76% /home

 cd testes

 pwd
 /home/kalib/testes
```

**S**imples, não?

**H**appy hacking...
