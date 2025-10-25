---
title: "Automatizando a Criacao De Imagens Com Packer"
date: 2018-07-30T13:32:42-05:00
draft: false
tags: [
"Devops",
"Packer",
"Docker",
"Cloud",
]
categories: ["Tutoriais"]
description: "Packer é uma ferramenta Open Source, criada e mantida pela HashiCorp, para a criação de imagens de máquinas idênticas para múltiplas plataformas a partir de uma única fonte ou código de configuração. O Packer é leve, roda em praticamente todos os principais Sistemas Operacionais e possui alta performance, permitindo a criação de imagens para múltiplas plataformas em paralelo."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---

Já utilizou Packer para automatizar e facilitar sua criação de recursos e imagens de máquinas? Caso não, está na hora de você conhecer e experimentar esta excelente ferramenta.

<!--more-->

## O que é Packer e qual sua importância

**P**acker é uma ferramenta Open Source, criada e mantida pela [HashiCorp](https://www.hashicorp.com), para a criação de imagens de máquinas idênticas para múltiplas plataformas a partir de uma única fonte ou código de configuração. O [Packer](https://www.packer.io/) é leve, roda em praticamente todos os principais Sistemas Operacionais e possui alta performance, permitindo a criação de imagens para múltiplas plataformas em paralelo.

**U**ma imagem de máquina, nada mais é que um arquivo estático que contém um sistema operacional pré-configurado, com determinados softwares instalados e que pode ser utilizada para criar novas máquinas ou servidores de forma mais rápida, evitando o trabalho repetitivo de instalar o sistema operacional e configurar aplicações padrões.

**E**xistem diversos formatos diferentes de imagens que podem ser criadas através do Packer, como por exemplo Amazon EC2, VirtualBox, VMware, Google Cloud Platform, Microsoft Azure, Docker, QEMU, CloudStack, DigitalOcean, etc.

**O** Packer suporta e é compatível com diversas ferramentas de provisionamento e automação, como shell script, Ansible, Puppet, Chef, etc., fazendo com que a criação de imagens seja ainda mais simples, dinâmica e robusta.

**A** ideia de criar imagens não é nova, visto que Sysadmins já o fazem há muitos anos, porém esta sempre foi uma tarefa tediosa, demorada e muito pouco produtiva. Basicamente a ideia de se construir uma imagem partia de, antes de mais nada, realizar de fato uma instalação completa de um Sistema Operacional e em seguida utilizar algum aplicativo para "salvar" aquele estado em uma imagem, que poderia ser posteriormente aplicada em outras máquinas. Isto por si só já facilitava muito a vida de Sysadmins em geral, visto que eles apenas realizavam a instalação completa do SO uma vez. Caso, além do SO, fossem necessárias outras aplicações, o processo seria basicamente o mesmo, instalando-se uma vez o SO completo e em seguida instalando todas as aplicações desejadas para a imagem.

**A**té então a criação da imagem parecia ser um sucesso, no entanto isto era algo improdutivo por ser absolutamente estático e imutável. Sempre que fosse necessário fazer uma mudança na imagem, atualizar versão de sistema operacional, aplicar patches, atualizar demais aplicações ou mudar configurações, novamente o processo deveria se repetir do início. Não é necessário sequer mencionar que não era nada simples gerenciar e versionar isto.

**E**is que surge o Packer para deixar a criação de imagens menos entediante, flexível e mais gerenciável.

## Instalação

**A** instalação não é complexa e pode ser feita através do binário disponível na página de downloads do Packer: [Download Packer](https://www.packer.io/downloads.html)</a>

**Ubuntu** - Caso não queira instalar através do binário fornecido no link acima, pode instalar via:

```bash
$ sudo apt-get install packer -y
```

**Arch linux** - Caso não queira instalar através do binário fornecido no link acima, pode instalar através do pacote disponível no [AUR](https://aur.archlinux.org/packages/packer/).

**OS X** - Caso não queira instalar através do binário fornecido no link acima, pode instalar através do homebrew:

```bash
$ brew install packer
```

**I**ndependente de sua forma de instalação, confirme que a instalação foi concluída com sucesso:

```bash
$ packer
Usage: packer [--version] [--help] <command> [<args>]

Available commands are:
    build       build image(s) from template
    fix         fixes templates from old versions of packer
    inspect     see components of a template
    push        push a template and supporting files to a Packer build service
    validate    check that a template is valid
    version     Prints the Packer version
```

**S**e você recebeu algo similar, significa que você já pode começar a criar suas imagens. Caso tenha recebido um erro informando que o Packer não foi encontrado, significa que o mesmo não foi inserido corretamente na variável de ambiente de seu PATH. Certifique-se de inserir o diretório no qual o Packer foi instalado em seu PATH.

## Criando Imagens com o Packer

**C**onforme dito anteriormente, o Packer pode criar imagens para diversas extensões e plataformas, portanto o primeiro passo é saber para qual plataforma você deseja criar sua imagem.

**P**ara este tutorial introdutório, utilizaremos o Docker como destino para nossa imagem, ou seja, criaremos uma imagem de container que poderá rodar com o Docker.

**OBS:** A partir deste ponto estou assumindo que você já possui o Docker instalado em seu sistema. Para confirmar, digite:

```bash
$ docker --version
Docker version 18.06.0-ce, build 0ffa825
```

**Vamos ao Packer.**

**P**rimeiramente mostrarei como não tenho nenhuma imagem Docker neste momento em meu sistema:

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

**1-** Escolha um editor de textos de sua preferência e inicie um arquivo chamado *template.json*;

**2-** Digite o seguinte em seu arquivo *template.json*.

```json
{
  "builders": [{
    "type": "docker",
    "image": "ubuntu",
    "commit": true
  }]
}
```

**O** que temos...

*builders:* Abre o bloco builders (construtores), onde iniciamos as instruções que definirão como a nossa imagem será criada;

*type:* Especifica o tipo de construtor que será utilizado. Neste exemplo escolhemos Docker. Aqui poderíamos utilizar AWS EC2, Google Cloud Instance, etc.

*image:* Parâmetro no qual indicaremos qual imagem será utilizada como origem para a nossa imagem final.

*commit:* Indica que queremos realizar o commit da imagem gerada no final.

**OBS:* É importante lembrar que o Packer possui uma infinidade de parâmetros ou propriedades diferentes que podem ser específicos para cada tipo de builder. Lista de parâmetros e opções para o builder [Docker](https://www.packer.io/docs/builders/docker.html).

**P**rimeiramente, o indicado é validarmos a sintaxe de nosso código json: *($ packer validate template.json)*

```bash
$ packer validate template.json
Template validated successfully.
```

**O** código parece estar correto do ponto de vista do Packer. Caso houvesse algo errado com o código, a mensagem daria alguma pista de onde está o erro. Removerei o último **}** do arquivo template.json para forçar um erro: *($ packer validate template.json)*

```bash
$ packer validate template.json
Failed to parse template: Error parsing JSON: unexpected end of JSON input
At line 7, column 1 (offset 88):
    6:   }]
    7:
```

**C**om o arquivo de volta à sua versão correta, o próximo passo seria de fato o build, onde criaremos a imgem desejado de acordo com as intruções que demos: *($ packer build template.json)*

```bash
$ packer build template.json
docker output will be in this color.

==> docker: Creating a temporary directory for sharing data...
==> docker: Pulling Docker image: ubuntu
    docker: Using default tag: latest
    docker: latest: Pulling from library/ubuntu
    docker: c64513b74145: Pulling fs layer
    docker: 01b8b12bad90: Pulling fs layer
    docker: c5d85cf7a05f: Pulling fs layer
    docker: b6b268720157: Pulling fs layer
    docker: e12192999ff1: Pulling fs layer
    docker: b6b268720157: Waiting
    docker: e12192999ff1: Waiting
    docker: c5d85cf7a05f: Verifying Checksum
    docker: c5d85cf7a05f: Download complete
    docker: 01b8b12bad90: Verifying Checksum
    docker: 01b8b12bad90: Download complete
    docker: e12192999ff1: Verifying Checksum
    docker: e12192999ff1: Download complete
    docker: b6b268720157: Verifying Checksum
    docker: b6b268720157: Download complete
    docker: c64513b74145: Verifying Checksum
    docker: c64513b74145: Download complete
    docker: c64513b74145: Pull complete
    docker: 01b8b12bad90: Pull complete
    docker: c5d85cf7a05f: Pull complete
    docker: b6b268720157: Pull complete
    docker: e12192999ff1: Pull complete
    docker: Digest: sha256:3f119dc0737f57f704ebecac8a6d8477b0f6ca1ca0332c7ee1395ed2c6a82be7
    docker: Status: Downloaded newer image for ubuntu:latest
==> docker: Starting docker container...
    docker: Run command: docker run -v /Users/kalib/.packer.d/tmp/packer-docker835877235:/packer-files -d -i -t ubuntu /bin/bash
    docker: Container ID: 50211726119aa045b0bc5eb9da8c9af243bc179fef0aad3cd94156d0f2a7a45a
==> docker: Committing the container
    docker: Image ID: sha256:3a6a21aab4706e2b512f0c1fcbe8265e2527d4794f7c6fbdc5e4faf907d10622
==> docker: Killing the container: 50211726119aa045b0bc5eb9da8c9af243bc179fef0aad3cd94156d0f2a7a45a
Build 'docker' finished.

==> Builds finished. The artifacts of successful builds are:
--> docker: Imported Docker image: sha256:3a6a21aab4706e2b512f0c1fcbe8265e2527d4794f7c6fbdc5e4faf907d10622
```

**N**a saída do comando, ou output, podemos ver todos os passos e instruções que foram realizadas na criação de nossa imagem. Neste exemplo, o Docker baixou a última imagem ubuntu dos repositórios do Docker e em seguida gerou nossa imagem.

**P**ara confirmar, podemos utilizar novamente o *docker images*:

```bash
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              3a6a21aab470        5 minutes ago       83.5MB
ubuntu              latest              735f80812f90        6 days ago          83.5MB
```

**C**omo esperado, agora temos duas imagens: a do ubuntu padrão, que foi baixada para servir de base para a nossa, bem como a nossa, até então sem nome.

**D**e certa forma nada foi feito até então. Nós apenas geramos uma imagem idêntica à que já existia do ubuntu, sem qualquer mudança. Vamos então dar algum sentido à nossa imagem em seguida. Além disso, podemos perceber que nossa imagem não tinha sequer nome, ficando listada como *<none>*. Corrigiremos isso também.

**O** Packer considera o Docker apenas como uma plataforma para gerar/rodar containers e, como tal, para gerar uma imagem o Packer descarta a necessidade da utilização de um Dockerfile, como de costume para quem já criou containers com Docker. Através de blocos de código *changes* e *provisioners*, o Packer nos permite passar praticamente todas as informações de metadata que fariam parte de um Dockerfile.

**P**ara demonstrar, criaremos uma imagem de um ubuntu rodando *nginx*, porém utilizaremos a imagem padrão do ubuntu que já baixamos anteriormente.

**O** código do template.json agora deverá ser o seguinte:

```json
{
  "builders": [{
    "type": "docker",
    "image": "ubuntu",
    "commit": true,
    "changes": [
      "CMD [\"nginx\", \"-g\", \"daemon off;\"]",
      "EXPOSE 80"
   ]
  }],
  "provisioners": [
    {
      "type" : "shell",
      "inline" : ["apt-get update && apt-get install -y nginx"]
    }
  ],
  "post-processors": [
      {
        "type": "docker-tag",
        "repository": "kalib/ubuntunginx",
        "tag": "0.1"
      }
  ]
}
```

**V**ejamos o que foi incluído:

*changes:* Parâmetro que nos permite alterar a meta-data de nossa imagem docker.

  *CMD [\"nginx\", \"-g\", \"daemon off;\"]:* Comando para iniciar o nginx no container, assim como faríamos utilizando Dockerfile.

  *EXPOSE 80:* Da mesma forma, apenas informando que a porta 80 no container deverá estar disponível, assim como faríamos em um Dockerfile.

*provisioners:* É onde indicamos que ferramentas utilizaremos para provisionar as alterações/configurações em nossa imagem. Podemos ter um ou mais provisioners ao mesmo tempo. Neste exemplo estaremos utilizando um simples comando bash, portanto nosso provisioner será *bash*, mas poderia ser chef, ansible, puppet, ou mesmo um script bash mais complexo, mas como o objetivo para este post é uma abordagem simples e introdutória, manteremos o provisioner mais simples possível.

  *type:* especificamos que o tipo de provisioner será *bash*.

  *inline:* Inserimos o comando bash que desejamos que seja executado para personalizar nossa imagem. Neste caso, *apt-get update && apt-get install -y nginx* irá atualizar a base de dados dos repositórios e em seguida instalar o nginx.

*post-processors:* Como o nome já diz, são instruções ou processors que acontecem ao final do build.

  *type:* Indica o tipo de post-processor que utilizaremos. No exemplo, utilizaremos *docker-tag*, para que possamos dar uma tag/nome para nossa imagem. (Lembrando que como uma boa prática, ao criar uma imagem Docker, sempre inserimos o padrão *repositório/imagem*)

  *repository:* É onde indicamos o repositório e o nome da imagem que criaremos.

  *tag:* Onde podemos definir a tag que ajudar a manter o controle de versão de nossa imagem.

**N**ovamente, vamos validar nosso código:

```bash
$ packer validate template.json
Template validated successfully.
```

**D**esta vez o output será muito extenso, portanto colarei apenas alguns trechos aqui para demonstrar o resultado:

```bash
$ packer build template.json
docker output will be in this color.

==> docker: Creating a temporary directory for sharing data...
==> docker: Pulling Docker image: ubuntu
    docker: Using default tag: latest
    docker: latest: Pulling from library/ubuntu
    docker: Digest: sha256:3f119dc0737f57f704ebecac8a6d8477b0f6ca1ca0332c7ee1395ed2c6a82be7
    docker: Status: Image is up to date for ubuntu:latest
==> docker: Starting docker container...
    docker: Run command: docker run -v /Users/kalib/.packer.d/tmp/packer-docker737353515:/packer-files -d -i -t ubuntu /bin/bash
    docker: Container ID: 097a85a796e0e928fbc82cd6618bd60e286b46dfadad20e823027fc2f93d25db
==> docker: Provisioning with shell script: /var/folders/nn/njz4sd054fv_tvq30vh0zygh0000gp/T/packer-shell161880041
    docker: Get:1 https://archive.ubuntu.com/ubuntu bionic InRelease [242 kB]
    docker: Get:2 https://security.ubuntu.com/ubuntu bionic-security InRelease [83.2 kB]
    ...
    ...
    docker: Reading state information...
docker: The following additional packages will be installed:
docker:   fontconfig-config fonts-dejavu-core geoip-database libbsd0 libexpat1
docker:   libfontconfig1 libfreetype6 libgd3 libgeoip1 libicu60 libjbig0
docker:   libjpeg-turbo8 libjpeg8 libnginx-mod-http-geoip
docker:   libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter
docker:   libnginx-mod-mail libnginx-mod-stream libpng16-16 libssl1.1 libtiff5
docker:   libwebp6 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6 libxml2 libxpm4
docker:   libxslt1.1 multiarch-support nginx-common nginx-core ucf
docker: Suggested packages:
docker:   libgd-tools geoip-bin fcgiwrap nginx-doc ssl-cert
docker: The following NEW packages will be installed:
docker:   fontconfig-config fonts-dejavu-core geoip-database libbsd0 libexpat1
docker:   libfontconfig1 libfreetype6 libgd3 libgeoip1 libicu60 libjbig0
docker:   libjpeg-turbo8 libjpeg8 libnginx-mod-http-geoip
docker:   libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter
docker:   libnginx-mod-mail libnginx-mod-stream libpng16-16 libssl1.1 libtiff5
docker:   libwebp6 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6 libxml2 libxpm4
docker:   libxslt1.1 multiarch-support nginx nginx-common nginx-core ucf
docker: 0 upgraded, 35 newly installed, 0 to remove and 0 not upgraded.
docker: Need to get 16.1 MB of archives.
docker: After this operation, 58.8 MB of additional disk space will be used.
docker: Get:1 https://archive.ubuntu.com/ubuntu bionic/main amd64 multiarch-support amd64 2.27-3ubuntu1 [6916 B]
docker: Get:2 https://archive.ubuntu.com/ubuntu bionic/main amd64 libxau6 amd64 1:1.0.8-1 [8376 B]
docker: Get:3 https://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libjpeg-turbo8 amd64 1.5.2
...
...
docker: Setting up nginx (1.14.0-0ubuntu1) ...
    docker: Processing triggers for libc-bin (2.27-3ubuntu1) ...
==> docker: Committing the container
    docker: Image ID: sha256:0cbafebeaf17d8b8e8eccad87e706845993b78265a99e3e0f1ef8bd0bd724aa7
==> docker: Killing the container: 097a85a796e0e928fbc82cd6618bd60e286b46dfadad20e823027fc2f93d25db
==> docker: Running post-processor: docker-tag
    docker (docker-tag): Tagging image: sha256:0cbafebeaf17d8b8e8eccad87e706845993b78265a99e3e0f1ef8bd0bd724aa7
    docker (docker-tag): Repository: kalib/ubuntunginx:0.1
Build 'docker' finished.

==> Builds finished. The artifacts of successful builds are:
--> docker: Imported Docker image: sha256:0cbafebeaf17d8b8e8eccad87e706845993b78265a99e3e0f1ef8bd0bd724aa7
--> docker: Imported Docker image: kalib/ubuntunginx:0.1
```

**C**omo podemos ver, todos os passos foram executados, incluindo a atualização da lista de pacotes dos repositórios, a instalação do nginx e a criação da imagem com o repositório, nome e tag de versão que indicamos.

**V**ejamos nossa imagem na lista de imagens disponíveis no Docker:

```bash
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
kalib/ubuntunginx   0.1                 0cbafebeaf17        5 minutes ago       182MB
<none>              <none>              4d86b1edf015        43 minutes ago      83.5MB
ubuntu              latest              735f80812f90        6 days ago          83.5MB
```

**S**ucesso, aparentemente nossa imagem foi criada conforme o esperado. Hora de testar e ver se tudo realmente funcionou.

**OBS:** Como dito no início do post, o objetivo deste post não é ensinar Docker, portanto estou assumindo que você já possui alguma familiaridade com esta ferramenta.

**V**amos iniciar um container a partir desta nossa imagem em background, mapeando a porta 80 do container na porta 80 de nosso host, para facilitar o teste:

```bash
$ docker run -d -p 80:80 kalib/ubuntunginx:0.1
53de48d3e7708a7ac16dbb5ba6ed71b8672201729264e51945ee3412a02e0deb
```

**N**enhuma mensagem de erro, portanto ao que tudo indica nosso container está rodando corretamente. Vamos confirmar isto com *docker ps*:

```bash
$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                NAMES
53de48d3e770        kalib/ubuntunginx:0.1   "nginx -g 'daemon of…"   5 seconds ago       Up 4 seconds        0.0.0.0:80->80/tcp   clever_keldysh
```

**T**udo certo aqui também. Podemos ver nosso container rodando, o processo do nginx aparentemente rodando, bem como a porta 80 mapeada como esperado. Vamos abrir o browser em nosso host e acessar *localhost:80*...

![Packer](packer_nginx.png)

**P**ronto, validamos a nossa imagem criando um container e confirmando que o nginx está de fato rodando como deveria.

**E**m futuros posts, pretendo dar exemplos mais aprofundados, utilizando combinações com outros provisioners, como Puppet, Ansible e Shell Scripts, bem como outros builders, como por exemplo o AWS.
