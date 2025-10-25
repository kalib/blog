---
title: "Docker - Uma Alternativa Elegante Para Containers No Linux"
date: 2015-08-20T17:14:12-05:00
draft: false
tags: [
"DevOps",
"Software Livre",
"Segurança",
"Linux",
"Ubuntu",
"Internet",
"Docker",
"Containers",
]
categories: ["Tutoriais"]
description: "Cada vez mais precisamos de soluções ágeis para deploy de aplicações e serviços e as soluções de containers surgiram com este propósito. Conheça o Docker, uma solução open source simples e prática, embora robusta, para utilização de containers no Linux."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
Cada vez mais precisamos de soluções ágeis para deploy de aplicações e serviços e as soluções de containers surgiram com este propósito. Conheça o Docker, uma solução open source simples e prática, embora robusta, para utilização de containers no Linux.

<!--more-->

## O que são containers?

**A**ntes de falar sobre o Docker, é importante que se entenda o conceito de um container, portanto vamos começar do básico. A imagem acima, logomarca do Docker, deixa claro o que é um container. A baleia, representando um navio, carregando diversos caixotes ou containers ilustra o conceito físico de um container. Nada mais do que um enorme caixote que possui o intuito de isolar algo. Quando um grande navio transporta mercadorias de um porto para outro, ele costuma trazer diversos containers separando estas mercadorias, de forma que as coisas não fiquem misturadas e bagunçadas. A forma de separação vai depender dos critérios de organização utilizados pela embarcação, seja por proprietário, seja por categoria de produtos, etc. De qualquer forma, embora cada container possua seus elementos próprios, todos os containers compartilham alguns recursos básicos, como por exemplo a embarcação, que é o meio de transporte para todos os containers ali contidos.

**D**a mesma forma se dá no mundo dos computadores, onde o conceito de containers surgiu para separar e isolar alguns recursos e aplicações, otimizando os recursos que servem como base e que podem ser utilizados de forma compartilhada, como por exemplo o kernel do Sistema Operacional. De certa forma isto nos faz lembrar um pouco da virtualização, onde cada máquina virtual compartilha os recursos da máquina física, no entanto existe uma diferença clara no contexto de containers, visto que em um cenário de virtualização você precisará possuir um SO instalado na máquina física, com seu kernel e todos os seus recursos, e um SO instalado em sua máquina virtual, também com seu kernel e todos os seus recursos. Quando falamos em containers, imagine que você só precisará do kernel, bem como vários outros recursos, na máquina que será a hospedeira do container (a embarcação).

**C**ontainers Linux surgiram como uma tecnologia chave para empacotamento e entrega de aplicativos, combinando a leveza do isolamento de aplicativos com a flexibilidade de métodos de deploy baseados em imagens.

**U**ma das formas mais simples de se imaginar a vantagem da utilização de containers é imaginar que você possui uma empresa que hospeda servidores de aplicações para seus clientes. Se um novo cliente surge querendo hospedar a aplicação dele, você subirá uma nova máquina virtual, o que inclui todo um novo sistema operacional, enquanto que em uma solução baseada em containers você poderá ter apenas a sua máquina com um único kernel Linux provendo as priorizações de recursos (CPU, memória, I/O, rede, etc.) sem a necessidade de dar boot em um novo sistema operacional (máquinia virtual) na qual rodará a aplicação deste cliente.

**D**izem que uma imagem vale mais que mil palavras...

![Virtual Machine Diagram](docker_vmdiagram.png)

**N**a imagem acima temos o cenário convencional com a utilização de **Máquinas Virtuais**. Em suma, temos um host físico, com seu respectivo SO e kernel. Acima deles temos a camada de virtualização ou HyperVisor, enquanto que acima desta teremos as máquinas virtuais, com seus respectivos SOs (cada um com seu kernel) instalados. No caso temos 3 VMs, com 3 SOs (cada um com seu kernel). Na camada acima encontramos o que realmente é necessário para o app do cliente funcionar, que são as bibliotecas e os binários. Por fim, o App do cliente em si.

**V**ejamos como fica o cenário com a utilização de containers, docker neste caso...

![Docker Diagram](docker_diagram.png)

**N**o cenário com o Docker percebemos que a camada de SO das VMs sumiu, visto que ela não é mais necessária. Ao invés de Máquinas Virtuais, agora nós temos 3 containers, onde cada container roda os binários e bibliotecas de um SO, porém se aproveitando do kernel já existente no Host.

**C**om este grau de modularização nós ganhamos maior flexibilidade e agilidade no deploy de ambientes e aplicações.

## Docker

**U**ma das vantagens da utilização do Docker é a existência de um repositório de imagens prontas que ficam disponibilizadas livremente para quem desejar utilizar. Seja uma imagem pronta de um container com CentOS, Ubuntu, etc.. Já existem centenas e centenas de imagens prontas para uso, sendo esta uma base de compartilhamento comunitário, mas...

**V**amos ao que interessa...

**N**os exemplos a seguir, estou utilizando o Ubuntu Server 15.04, visto que estou atualmente realizando uma POC de VPS com um novo host, portanto aproveitarei para fazer disto uma parte de meus testes nesta VPS. Sinta-se livre para utilizar sua máquina física com Ubuntu, com Debian, ou mesmo uma máquina virtual, caso não goste de realizar testes em sua máquina física, o resultado será o mesmo. Para que tudo funcione como esperamos, só existem 2 pré-requisitos a serem atendidos:

## Mão na massa

1- O kernel do Linux que será utilizado deve ser igual ou superior ao 3.8;

2- Caso você esteja realizando os testes em uma VM, seria interessante que sua máquina física tivesse comunicação com a VM. Isso pode ser testado com um ping da máquina física para a VM. No caso de sua máquina virtual ser instalada com interface gráfica, esta comunicação não será necessária, pois o único momento em que utilizaremos isto será para abrir um navegador e fazer um teste de acesso ao endereço da máquina virtual.

**V**amos lá. Para ter a certeza de que você atende o pré-requisito de kernel, utilize o comando "uname -r":

```bash
 kalib@cloudcaverna:~$ uname -r
 3.19.0-25-generic
```

**E**stou com o kernel 3.19, portanto superior ao kernel 3.8 que é o pré-requisito mínimo. Vamos em frente.

**P**rimeiramente, vamos instalar o Docker. Seja lá qual for sua distribuição Linux, digite o comando: **(O comando deve ser executado com o usuário root ou com o comando sudo!)**

```bash
 # curl -sSL https://get.docker.com | sh
```

**E**le baixará e executará um script de instalação, no meu caso do Ubuntu ele irá instalar um repositório e em seguida instalará o docker. O próximo passo será iniciar o serviço do docker:

```bash
 # /etc/init.d/docker start
 [ ok ] Starting docker (via systemctl): docker.service.
```

**O** docker possui uma série de scripts/comandos próprios para facilitar a sua administração, como por exemplo um script de **ps**, para que possamos ter a certeza de que ele está rodando e, além disso, saber se existem containers em execução, da mesma forma que faríamos com o ps do linux para ver os processos em andamento.

```bash
 # docker ps

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

**P**odemos ver que o docker está rodando, no entanto nenhum container está em execução. Na verdade, não temos nenhum container criado, portanto obviamente não poderia estar em execução.

**A**lém do ps, podemos utilizar o script **images** para ver quais imagens de containers já possuímos para uso:

```bash
 # docker images

 REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
```

**D**a mesma forma, não temos ainda nenhuma imagem baixada para uso.

**U**ma vez que estamos falando de containers, conforme dito anteriormente, a ideia é isolar ao máximo e otimizar o que precisamos para este container, portanto precisamos informar o processo que desejamos iniciar no container em questão.

**V**amos criar um container do Ubuntu, por exemplo, na versão 15.04, lançada em Abril deste ano, e vamos iniciar juntamente com ele o processo /bin/bash. O comando utilizado será: docker run -i -t ubuntu:15.04 /bin/bash

```bash
 # docker run -i -t ubuntu:15.04 /bin/bash

 Unable to find image 'ubuntu:15.04' locally
 15.04: Pulling from library/ubuntu

 6e6a100fa147: Pull complete
 13c0c663a321: Pull complete
 2bd276ed39d5: Pull complete
 013f3d01d247: Already exists
 library/ubuntu:15.04: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.

 Digest: sha256:b2d4940544e515d4bc62b2a9ad3e6137b3e1e0937a41fdc1f0f30d12935e5b09
 Status: Downloaded newer image for ubuntu:15.04

 root@d70562e7533c:/#
```

**É** importante reparar que na primeira linha de execução ele me trouxe um alerta informando que não foi possível encontrar a imagem "ubuntu:15.04" localmente. Como disse acima, não temos ainda nenhuma imagem baixada, portanto ele não encontrou localmente e foi baixar diretamente no repositório de imagens do docker.

**O** procedimento foi extremamente rápido, certo? Acredite ou não, você já possui um container Ubuntu rodando em sua máquina. ;] Ainda não acredita? Repare novamente no seu prompt de comandos, veja que logo que ele finalizou o processo ele lhe deixou em um prompt "estranho". No caso do meu exemplo acima, perceba que ao concluir o processo ele me deixou com o prompt assim:

```bash
 root@d70562e7533c:/#
```

**N**ão, minha máquina não se chama d70562e7533c. Tenho certeza de que a sua também não se chama.. seja lá qual for a combinação de caracteres que lhe foi apresentada no prompt. Na verdade, sempre que iniciamos um container, o comportamento default é que você já é logado nele. Em suma, seu container com ubuntu 15.04 já foi criado e você já está logado nele, e esta combinação de caracteres estranha é o ID que foi dado ao seu container.

**A**inda não acredita? Bom, você pode, por exemplo, dar um **cat /etc/issue**, para ver que você está de fato rodando um ubuntu 15.04. Claro, no meu caso não haverá diferença, pois minha máquina que está rodando o docker também é ubuntu 15.04.

```bash
 root@d70562e7533c:/# cat /etc/issue

 Ubuntu 15.04 \n \l

 root@d70562e7533c:/#
```

**O*utro teste, seria rodar **ps -ef** no container. Você verá que não existe nenhum processo rodando. Aliás, haverá apenas 1 processo (além do próprio ps), que foi o processo indicado na criação: /bin/bash. Desta forma você terá a certeza de que não está no prompt de sua máquina mesmo, visto que na sua certamente existem dezenas ou centenas de processos rodando, do kernel, do usuário, etc.

```bash
 root@d70562e7533c:/# ps -ef

 UID        PID  PPID  C STIME TTY          TIME CMD
 root         1     0  0 13:36 ?        00:00:00 /bin/bash
 root         9     1  0 13:44 ?        00:00:00 ps -ef

 root@d70562e7533c:/#
```

**D**a mesma forma você poderá experimentar outros comandos para testar (caso ainda não esteja acreditando que de forma tão "oi, simples assim" você já está com seu container pronto): ls, apt-get update, etc.. Tudo funcionando como se fosse uma máquina real, ou virtual, no entanto sem um kernel, visto que ela está utilizando o kernel da máquina host.

**A**gora que temos a certeza de que estamos em nosso container, você pode sair do container e voltar para sua máquina host. Para isso você precisará pressionar as teclas **Ctrl + P + Q**. Desta forma, você verá que seu prompt voltará para seu host enquanto que seu container continuará rodando, você apenas saiu do prompt do mesmo.

```bash
 root@d70562e7533c:/# root@cloudcaverna:~#

 root@cloudcaverna:~#
```

**V**amos verificar novamente o ps do docker, visto que da última vez ele estava vazio. Desta vez ele nos mostrará um processo em execução, que no caso é o container que criamos.

```bash
 root@cloudcaverna:~# docker ps

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
 d70562e7533c        ubuntu:15.04        "/bin/bash"         15 minutes ago      Up 15 minutes                           modest_khorana
```

**N**o retorno podemos ver o ID do nosso container, o nome da imagem que ele utiliza, o comando em execução, o tempo desde sua criação, o seu status, portas e nome.

**O** container ID poderá ser utilizado para voltar para nosso container através do comando **docker attach <container-id>**: (Após digitar o comando, pressione novamente Enter para liberar o prompt)

```bash
 root@cloudcaverna:~# docker attach d70562e7533c

 root@d70562e7533c:/#
```

**N**o exemplo anterior nós utilizamos a combinação **Ctrl + P + Q** para sair do container mantendo-o rodando. Vamos experimentar utilizar desta vez **Ctrl + D**. Desta forma você não apenas está saindo mas também desligando o container. Execute novamente a lista de processos/containers do docker para ver:

```bash
 root@cloudcaverna:~# docker ps

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

**N**o entanto, é importante lembrar que a imagem do Ubuntu 15.04 que baixamos, continua disponível para o caso de precisarmos criar novos containers Ubuntu 15.04:

```bash
 root@cloudcaverna:~# docker images

 REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
 ubuntu              15.04               013f3d01d247        17 hours ago        131.4 MB
```

**S**imples... Mas vamos fazer algo mais próximo do mundo real, afinal um container apenas com o Ubuntu rodando, ou qualquer outra que seja a distribuição escolhida, não tem muita utilidade, portanto vamos criar um container rodando um servidor web, para o caso de querermos hospedar um site ou aplicalção web neste container. Por ser mais leve e simples, vou utilizar o nginx no exemplo. Vamos utilizar o comando **docker run -i -t -p 8080:80 ubuntu:15.04 /bin/bash**

**N**o comando acima estamos dizendo que queremos criar um novo container ubuntu 15.04 rodando o /bin/bash. Desta vez temos um parâmetro que não utilizamos anteriormente. O **p** serve para indicar a utilização de portas. Quando utilizamos **p 8080:80** estamos dizendo que vamos utilizar a porta 80 no container e que ela estará mapeada na porta 8080 do nosso host ou hospedeiro. Ou seja, quando instalarmos o Nginx no ubuntu do container, você poderá através de seu host abrir o navegador e acessar o seu endereço ip ou nome de host (caso você possua resolução de nomes funcionando) na porta 8080.

```bash
 root@cloudcaverna:~# docker run -i -t -p 8080:80 ubuntu:15.04 /bin/bash

 root@08abb8611700:/#
```

**O** processo foi bem mais rápido desta vez, visto que já tínhamos a imagem do container ubuntu 15.04, portanto não tivemos a necessidade de baixar outra imagem. O seu container já está criado e você já está logado no prompt do mesmo, conforme pode ver através do ID do container que apareceu em seu prompt.

**V**amos agora instalar o servidor web nginx para realizarmos o nosso teste. Para isso vamos atualizar os repositórios e em seguida instalar o pacote: **apt-get update && apt-get install -y nginx**

```bash
 root@08abb8611700:/# apt-get update && apt-get install -y nginx

 Ign https://archive.ubuntu.com vivid InRelease
 Ign https://archive.ubuntu.com vivid-updates InRelease
 Get:1 https://archive.ubuntu.com vivid/main Sources [1358 kB]
 Get:2 https://archive.ubuntu.com vivid/restricted Sources [7100 B]
 ...
 Building dependency tree       
 Reading state information... Done
 The following extra packages will be installed:
  fontconfig-config fonts-dejavu-core geoip-database init-system-helpers libexpat1 libfontconfig1 libfreetype6 libgd3 libgeoip1 libicu52
  libjbig0 libjpeg-turbo8 libjpeg8 libpng12-0 libssl1.0.0 libtiff5 libvpx1 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6 libxml2 libxpm4
  libxslt1.1 nginx-common nginx-core sgml-base ucf xml-core
 Suggested packages:
  libgd-tools geoip-bin fcgiwrap nginx-doc ssl-cert sgml-base-doc debhelper
 The following NEW packages will be installed:
  fontconfig-config fonts-dejavu-core geoip-database init-system-helpers libexpat1 libfontconfig1 libfreetype6 libgd3 libgeoip1 libicu52
  libjbig0 libjpeg-turbo8 libjpeg8 libpng12-0 libssl1.0.0 libtiff5 libvpx1 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6 libxml2 libxpm4
  libxslt1.1 nginx nginx-common nginx-core sgml-base ucf xml-core
 0 upgraded, 31 newly installed, 0 to remove and 0 not upgraded.
 Need to get 14.0 MB of archives.
 After this operation, 53.3 MB of additional disk space will be used.
 ...
 Get:1 https://archive.ubuntu.com/ubuntu/ vivid/main libexpat1 amd64 2.1.0-6ubuntu1 [70.6 kB]
 Processing triggers for systemd (219-7ubuntu6) ...

 root@08abb8611700:/#
```

**C**ortei bastante a saída visto ser desnecessária, mas uma vez que o nginx esteja instalado, vamos iniciá-lo: **/etc/init.d/nginx start**. Em seguida, vamos utilizar o ps para ver os processos que estão rodando no nosso container:

```bash
 root@08abb8611700:/# /etc/init.d/nginx start

 root@08abb8611700:/# ps -ef

 UID        PID  PPID  C STIME TTY          TIME CMD
 root         1     0  0 14:10 ?        00:00:00 /bin/bash
 root       609     1  0 14:19 ?        00:00:00 nginx: master process /usr/sbin/nginx
 www-data   610   609  0 14:19 ?        00:00:00 nginx: worker process
 www-data   611   609  0 14:19 ?        00:00:00 nginx: worker process
 www-data   612   609  0 14:19 ?        00:00:00 nginx: worker process
 www-data   613   609  0 14:19 ?        00:00:00 nginx: worker process
 root       614     1  0 14:19 ?        00:00:00 ps -ef

 root@08abb8611700:/#
```

**F**eito isto, o serviço está rodando e já pode ser testado. Em seu host você poderá abrir o navegador e acessar o endereço local de seu host, com a porta 8080, visto que foi esta que definimos inicialmente para mapear a porta 80 do container: **localhost:8080**

**C**aso esteja utilizando uma máquina virtual para fazer seus testes, você terá duas possibilidades:
1- Caso sua máquina virtual possua alguma interface gráfica instalada, você poderá abrir o navegador da própria VM e acessar o mesmo endereço de localhost com a porta 8080;
2- Caso sua VM não esteja com nenhum ambiente gráfico instalado, você poderá utilizar aplicações de CLI para testar (ex: lynx, curl, etc.) ou usar o navegador da máquina que serve de host para sua VM, levando em conta que você fez o teste descrito no início para ter certeza de que sua máquina física consegue se comunicar com sua VM. Neste caso, em sua máqunina física você acessará o endereço de sua vm no navegador, com a porta 8080.

**R**esultado? O Nginx em nosso container rodando perfeitamente.

![Nginx on Docker](docker_nginx8080.png)

**D**a mesma forma feita anteriormente, podemos sair de nosso container com a combinação de teclas **Ctrl + P + Q** e verificar os processos/containers do docker em execução:

```bash
 root@08abb8611700:/# root@cloudcaverna:~#

 root@cloudcaverna:~# docker ps

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
 08abb8611700        ubuntu:15.04        "/bin/bash"         19 minutes ago      Up 19 minutes       0.0.0.0:8080->80/tcp   jolly_hawking

 root@cloudcaverna:~#
```

**É** importante salientar que quando saímos do container com a combinação **Ctrl + P + Q** nós não estamos fechando o container, pois ele continua rodando, conforme pode ser visto com **docker ps**. No entanto, quando saímos do nosso antigo container com a combinação **Ctrl + D**, nós percebemos que ele finalizou de vez o container. Além de finalizar, ele excluiu o nosso container, visto que o mesmo não foi salvo, ou "comittado". Se sairmos deste container no qual instalamos o nginx utilizando **Ctrl + D**, nós estaremos descartando tudo o que foi feito nele. Para poder finalizar o seu container sem perdê-lo, ou seja, mantendo o container salvo e tudo o que foi instalado/configurado nele, nós precisamos sair do container com a combinação **Ctrl + P + Q**, conforme fizemos acima, e em seguida realizar um commit deste container. Estaremos então criando uma imagem com o atual estado do container. Desta forma, se posteriormente precisarmos subir um novo container que rode o SO ubuntu 15.04 e que também possua o nginx, poderemos criar um novo container a partir desta imagem em poucos segundos. O commit se dá da seguinte forma: **docker commit <container-id> <nome-que-vc-desejar>**

**L**embrando que o id do container pode ser conseguido através do comando **docker ps** e que o **nome-que-vc-desejar** será o nome utilizado para identificar esta sua máquina/imagem.

```bash
 root@cloudcaverna:~# docker commit 08abb8611700 cloudcaverna/ubuntu-nginx:1.0

 b0922bc8f41295cadadbd131c075e29288b52e8bb2d9546cb7c0327eb95fe7dc

 root@cloudcaverna:~#
```

**U**tilizei 1.0 ao final para ter uma ideia de versionamento, visto que o docker nos permite trabalhar desta forma. É uma questão de organização.

**E**m seguida você pode verificar sua imagem criada através do script **images** do docker, bem como o seu container ainda rodando através do script **ps**:

```bash
 root@cloudcaverna:~# docker images

 REPOSITORY                  TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
 cloudcaverna/ubuntu-nginx   1.0                 b0922bc8f412        About a minute ago   204.6 MB
 ubuntu                      15.04               013f3d01d247        18 hours ago         131.4 MB

 root@cloudcaverna:~# docker ps

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
 08abb8611700        ubuntu:15.04        "/bin/bash"         49 minutes ago      Up 49 minutes       0.0.0.0:8080->80/tcp   jolly_hawking

 root@cloudcaverna:~#
```

**V**amos agora criar um novo container. Aqui você pode escolher como prosseguir. Eu criarei um container com a distribuição Arch Linux rodando, mas você pode seguir e criar outra com o Ubuntu caso deseje, então vejamos as opções:

**Opção 1 - Caso escolha criar este segundo container como Arch Linux**

```bash
 root@cloudcaverna:~# docker run -i -t -p 6660:80 base/archlinux /bin/bash

 Unable to find image 'base/archlinux:latest' locally
 latest: Pulling from base/archlinux

 b31c6c1462e6: Pull complete
 b97e110c94d9: Already exists
 Digest: sha256:7905fad7578b9852999935fb0ba9c32fe16cece9e4d1d742a34f55ce9cebdfd1
 Status: Downloaded newer image for base/archlinux:latest

 [root@be266bf7e5a3 /]#

```

**Opção 2 - Caso deseje criar um novo container Ubuntu aproveitando a imagem que já está pronta e com nginx já instalado**

```bash
 docker run -i -t -p 6660:80 <nome-que-vc-deu-para-sua-imagem>
```

Como eu dei o nome de *cloudcaverna/ubuntu-nginx*, eu utilizaria *docker run -i -t -p 6660:80 cloudcaverna/ubuntu-nginx*.

Desta forma você não precisará sequer instalar o nginx novamente neste container, visto que você utilizou como base uma imagem que já possuía o nginx instalado, restando apenas a você testar no navegador novamente o endereço porém trocando a porta para 6660.

**C**omo eu resolvi seguir em frente com um container totalmente novo, baseado no Arch Linux, vou dar um **cat /etc/issue** para ver que realmente estou em um ambiente com a distribuição Arch Linux e vou instalar o nginx nele com o pacman, visto que este é o gerenciador de pacotes do Arch:

```bash
 [root@be266bf7e5a3 /]# cat /etc/issue

 Arch Linux \r (\l)

 [root@be266bf7e5a3 /]# pacman -Sy nginx

 :: Synchronizing package databases...
 core                                                        121.2 KiB   203K/s 00:01 [#################################################] 100%
 extra                                                      1773.8 KiB   644K/s 00:03 [#################################################] 100%
 community                                                     2.7 MiB   936K/s 00:03 [#################################################] 100%
 resolving dependencies...
 looking for inter-conflicts...

 Packages (1): nginx-1.8.0-1

 Total Download Size:    0.34 MiB
 Total Installed Size:   0.98 MiB

 :: Proceed with installation? [Y/n]
 :: Retrieving packages ...
 nginx-1.8.0-1-x86_64                                        349.5 KiB   266K/s 00:01  [#################################################] 100%
 (1/1) installing nginx                                                                [ #################################################] 100%

 [root@be266bf7e5a3 /]#
```

**A**gora que estou com o nginx rodando também neste container do Arch Linux, vou sair do container pressionando a combinação **Ctrl + P + Q** e em seguida rodar o script de **ps** do docker:

```bash
 [root@be266bf7e5a3 /]# root@cloudcaverna:~#

 root@cloudcaverna:~# docker ps

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
 be266bf7e5a3        base/archlinux      "/bin/bash"         10 minutes ago      Up 10 minutes       0.0.0.0:6660->80/tcp   cocky_hoover
 08abb8611700        ubuntu:15.04        "/bin/bash"         About an hour ago   Up About an hour    0.0.0.0:8080->80/tcp   jolly_hawking

 root@cloudcaverna:~#
 ```

 **D**esta vez eu possuo dois containers criados, sendo um com o ubuntu 15:04 e outro com o Arch Linux. Agora vou realizar o commit do meu container Arch Linux com nginx, para não perder esta imagem. Em seguida, executarei o script **images** para ver as imagens que já possuo:

 ```bash
  root@cloudcaverna:~# docker commit be266bf7e5a3 cloudcaverna/archlinux-nginx:1.0

 f4ea14bf23c47466fb256fff9e3ab32ca85fb0256a05007ef4972ad7ff5f2aa9

 root@cloudcaverna:~# docker images

 REPOSITORY                     TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
 cloudcaverna/archlinux-nginx   1.0                 f4ea14bf23c4        5 seconds ago       285.9 MB
 cloudcaverna/ubuntu-nginx      1.0                 b0922bc8f412        20 minutes ago      204.6 MB
 ubuntu                         15.04               013f3d01d247        18 hours ago        131.4 MB
 base/archlinux                 latest              b97e110c94d9        8 weeks ago         278.8 MB

 root@cloudcaverna:~#
```
**A**gora vou testar no navegador o nginx deste meu segundo container, o qual defini que utilizaria a porta 6660 do meu host:

![Nginx on Docker](docker_nginx6660_8080.png)

**R**epare que acessei ambos os endereços, tanto o da porta 8080, o qual está apresentando o nginx do meu primeiro container, quanto o da porta 6660, que apresenta o nginx do meu segundo container. Ambos funcionando em paralelo, com ambientes distintos, sendo um Ubuntu e o outro Arch Linux, porém ambos compartilham o mesmo kernel, que é o do meu host.

**O** próprio script **ps** do docker lhe informa as portas que estão sendo utilizadas para cada container, caso você esteja em dúvida:

```bash
 root@cloudcaverna:~# docker ps

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
 be266bf7e5a3        base/archlinux      "/bin/bash"         26 minutes ago      Up 26 minutes       0.0.0.0:6660->80/tcp   cocky_hoover
 08abb8611700        ubuntu:15.04        "/bin/bash"         About an hour ago   Up About an hour    0.0.0.0:8080->80/tcp   jolly_hawking

 root@cloudcaverna:~#
```

**D**a mesma forma, você poderá fazer toda e qualquer operação que você faria em uma máquina qualquer, como por exemplo monitorar os logs de acesso do nginx. Basta se conectar em algum dos dois containers com **docker attach <container-id>** e em seguida abrir o arquivo de log do nginx com o tail: **tail -f /var/log/nginx/access.log**. Com o log rodando, pode acessar novamente no navegador o endereço com a porta que está mapeada para este container e você verá os logs do seu acesso.

**A**nteriormente nós vimos que com a combinação **Ctrl + P + Q** eu consigo sair do container porém ele permanecerá rodando. Com **Ctrl + D** eu finalizava o container de vez. Mas, supondo que eu queira parar temporariamente o container, posso utilizar o script **stop** do container inserindo o id do container que desejo parar:

```bash
 root@cloudcaverna:~# docker stop be266bf7e5a3
 be266bf7e5a3

 root@cloudcaverna:~#
```

**E**xiste ainda uma forma de ver o que foi alterado no container desde sua criação. É literalmente uma espécie de "diff":

```bash
 root@cloudcaverna:~# docker diff 08abb8611700
 C /.wh..wh.plnk
 A /.wh..wh.plnk/93.1190648
 C /bin
 A /bin/running-in-container
 A /core
 C /etc
 C /etc/default
 A /etc/default/nginx
 A /etc/fonts
 A /etc/fonts/conf.avail
 A /etc/fonts/conf.avail/10-antialias.conf
 A /etc/fonts/conf.avail/10-autohint.conf
 A /etc/fonts/conf.avail/10-hinting-full.conf
 A /etc/fonts/conf.avail/10-hinting-medium.conf
 A /etc/fonts/conf.avail/10-hinting-slight.conf
 A /etc/fonts/conf.avail/10-hinting.conf
 A /etc/fonts/conf.avail/10-no-sub-pixel.conf
 A /etc/fonts/conf.avail/10-scale-bitmap-fonts.conf
 A /etc/fonts/conf.avail/10-sub-pixel-bgr.conf
 A /etc/fonts/conf.avail/10-sub-pixel-rgb.conf
 A /etc/fonts/conf.avail/10-sub-pixel-vbgr.conf
 A /etc/fonts/conf.avail/10-sub-pixel-vrgb.conf
 A /etc/fonts/conf.avail/10-unhinted.conf
 A /etc/fonts/conf.avail/11-lcdfilter-default.conf
 A /etc/fonts/conf.avail/11-lcdfilter-legacy.conf
 A /etc/fonts/conf.avail/11-lcdfilter-light.conf
 ...
 ...
```

**A** saída é extensa, portanto cortei aqui mesmo, mas você terá basicamente todo o diff do que foi alterado desde a criação do container.

**R**esumidamente, esta é a função do docker. Com criatividade e disposição se faz muita coisa com ele. Não é a toa que os big players de mercado já estão utilizando bastante esta ferramenta para soluções de container, como por exemplo: Amazon, Apcera, Cisco, CoreOS, Datera, VMWare, Verizon Labs, Red Hat, Google, RackSpace, Oracle, IBM, Intel, Microsoft, HP, etc.

**L**embrando que o endereço de repositórios com as imagens já existentes e disponibilizadas gratuitamente é https://hub.docker.com

**A**té a próxima...

**H**appy Hacking!
