---
title: "Introdução ao Terraform"
date: 2018-10-29T22:34:06-05:00
draft: false
image: "img/terraform-integrations.png"
tags: [
"Devops",
"Terraform",
"Cloud",
"GCP",
"Docker",
]
description: "Uma introdução ao Terraform, da Hashicorp, como robusta e eficiente ferramenta para infraestrutura como código."
---
## Terraform - Uma robusta opção para Infraestrutura como Código ##

**A** Hashicorp é uma empresa de bastante destaque no meio DevOps por ter criado várias soluções de automação, que englobam uma série de funcionalidades, como o <a href="https://packer.io" target="_blank">Packer</a> para criação de imagens de forma automatizada, conforme apresentado nestes dois posts  do blog (<a href="/automatizando-a-criacao-de-imagens-com-packer/" target="_blank">1</a>, <a href="http://localhost:1313/criando-uma-imagem-aws-ec2-com-packer-e-puppet/" target="_blank">2</a>), <a href="https://vagrant.io" target="_blank">Vagrant</a>, para provisionamento simples e rápido de máquinas, <a href="https://vaultproject.io" target="_blank">Vault</a>, para gerenciamento de senhas/segredos (*secrets*), <a href="https://consul.io" target="_blank">Consul</a>, para descoberta de serviços, <a href="https://nomadproject.io" target="_blank">Nomad</a>, para agendamento e automação de deployments, e o <a href="https://terraform.io" target="_blank">Terraform</a>, foco principal deste post, uma robusta ferramenta para criação de infraestrutura como código, ou *infrastructure as cdode*.

**C**aso você não possua uma ideia muito clara de qual a idea por trás do conceito de infraestrutura como código, ou mesmo quais as vantagens de se utilizar esta metodologia de gerenciamento/criação de infraestrutura, sugiro que leia meu <a href="/infraestrutura-como-c%C3%B3digo-com-terraform/" target="_blank">post anterior</a>, no qual explico alguns dos principais benefícios desta prática, bem como uma breve apresentação do Terraform.

**D**e forma resumida, o Terraform é uma ferramenta disponível em formatos Open Source ou Enterprise, cujo intuito é permitir a criação de infraestrutura como código, possibilitando o controle de versões. Suporta diversos provedores tais como AWS, OpenStack, Azure, GCP, etc.

**U**ma de suas principais características é a idempotência, termo muito utilizado na matemática ou em ciência da computação para indicar a propriedade que algumas operações têm de poderem ser aplicadas várias vezes sem que o valor do resultado se altere após a aplicação inicial. Ou seja, uma vez aplicado o seu código terraform, você poderá aplicá-lo quantas vezes desejar e nenhuma alteração será feita em sua infraestrutura, a menos que você tenha de fato alterado algo em seu código.

**O** Terraform utiliza uma linguagem de alto nível e fácil de se reutilizar, uma vez que podemos criar módulos e utilizar estes módulos em diversos projetos distintos, mesmo que tenhamos módulos em repositórios também distintos.

**A** ideia de possuir um "plano" de execução nos ajuda a identificar falhas em nosso código mais rapidamente, bem como prevenir problemas em nossa infraestrutura, visto que podemos ter uma visão geral de tudo o que será aplicado em nossa infra antes mesmo da execução real de nosso código, nos permitindo ter a certeza de que todas as alterações serão de fato intencionais.

**S**empre digo que a melhor forma de se aprender uma nova tecnologia é colocando a mão na massa, portanto vamos escrever algumas linhas de código para entendermos as funcionalidades básicas bem como a sintaxe de código utilizada pelo Terraform.

**A**ntes de pensarmos em cenários mais complexos devemos entender o básico, no entanto eu sempre gostei de ver algum resultado como forma de ter uma motivação real para meus estudos. Nunca gostei de apenas ler e escrever códigos que não resultam em nada, e acredito que todos devam sentir a mesma insatisfação ao não ter um uso real e prático para o que quer que esteja estudando.

**S**eguindo esta ideia, antes de pensarmos em cenários mais complexos, vamos iniciar pelo básico, porém com algum resultado prático. A ideia para este post é termos um jogo clássico do Mario rodando em um container Docker que seja acessível através de nosso browser.

## Instalação ##

**P**ara este post precisaremos ter três aplicativos instalados:

1.  Um navegador ou browser qualquer; (Imagino que você já tenha algum...)
2.  Docker;
3.  Terraform

### Docker ###

**O** processo de instalação do Docker varia de acordo com o seu sistema operacional. Caso queira maiores detalhes sobre sua instalação, bem como uma explicação introdutória de como ele funciona, você pode visitar <a href="http://localhost:1313/docker-uma-alternativa-elegante-para-containers-no-linux/" target="_blank">este outro post</a>, embora você não precise ter nenhum conhecimento sobre Docker para seguir as instruções deste tutorial, visto que utilizaremos o Terraform para criar nosso container.

**N**o *Archlinux* a instalação pode ser feita através do pacman:

```
# pacman install docker
```

**N**o *Windows* a instalação pode ser feita através do binário disponível no site oficial: <a href="https://store.docker.com/editions/community/docker-ce-desktop-windows" target="_blank">Binário Windows</a>.

**N**o *OS X*, você também pode baixar o binário diretamente no site oficial, <a href="https://store.docker.com/editions/community/docker-ce-desktop-mac" target="_blank">Binário OS X</a> ou através do brew:

```
brew install docker
```

### Terraform ###

**A** instalação do Terraform é tão simples quanto a do Docker.

**N**o *Archlinux* a instalação pode ser feita através do pacman:

```
# pacman -S terraform
```

**N**o *Windows* e no *OS X* a instalação pode ser feita através do binário disponível no site oficial do Terraform: <a href="https://www.terraform.io/downloads.html" target="_blank">Binários Terraform</a>.

**O**utra opção para OS X é através do brew:

```
brew install terraform
```

## Verificando a instalação ##

**U**ma vez que você tenha instalado ambos, certifique-se de que a instalação foi bem sucedida e de que o serviço Docker esteja rodando em seu sistema. Para isto, abra algum terminal, console ou prompt do CMD (para usuários Windows) e digite o seguinte:

1- Para termos certeza de que o Terraform está instalado e funcionando:
```
terraform -version
```

Você deverá receber algum resultado com a versão do seu Terraform, similar a este:

```
Terraform v0.11.10
```

2- Para termos certeza de que o Docker está devidamente instalado e rodando, digite:
```
docker ps
```

Você deverá receber algum resultado parecido com o seguinte:

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

**N**ão se assuste ou dê importância para este resultado do comando Docker, ele apenas indica o status atual de seu Docker, informando se você possui algum container rodando ou não. Caso você tenha acabado de instalar o mesmo ou iniciado o serviço do Docker, você provavelmente não terá nenhum container rodando.

**C**aso o retorno de seu Docker ou Terraform não seja similar aos que apresentei acima, verifique se a instalação foi realmente bem sucedida ou, no caso do Docker, verifique se o mesmo está rodando, afinal ele não apenas precisa ser instalado, mas precisa estar rodando, diferentemente do Terraform que apenas precisa ser instalado.

## Iniciando nosso projeto ##
![Super Mario](/img/mariodocker.png)

### Escopo ###

**O** primeiro passo de qualquer projeto é identificar alguma espécie de esboço ou escopo para o mesmo.

**O** que queremos para o nosso projeto é:

1.  Ter um jogo web do Mario;
2.  Queremos que ele rode em um container, pois queremos uma aplicação em uma infraestrutura moderna e que possa ser capaz de rodar em qualquer local, seja em um servidor físico local, uma VM ou mesmo um provedor na nuvem, como AWS, GCP, Azure, etc.;
3.  Como não sabemos onde ou como iremos fazer o deployment deste container, queremos fazer com que seja algo automatizado e portável para facilitar futuros planos, portanto queremos criar este container com o Terraform, para que possamos gerenciar nosso código, versionar, etc.
4.  Não escreveremos a aplicação em si. O jogo do Mario já existe e uma imagem para Docker já está disponível para o mesmo através do seguinte link: <a href="https://hub.docker.com/r/pengbai/docker-supermario/" target="_blank">Imagem Mario</a> (Mas nós não precisamos nos precoupar com isto agora, pois o Terraform vai cuidar de baixar a imagem para nós. ;])
5.  A aplicação deverá estar acessível via browser local para que possamos ao menos garantir que o jogo está de fato funcionando.

**E**ste será o nosso escopo básico, portanto vamos começar nosso código.

### Projeto ###

**O** recurso mais básico em um código ou módulo Terraform é o *resource*, ou recurso. O Terraform suporta centenas de recursos diferentes, dentre eles o *docker_image*, que será o recurso de que precisaremos inicialmente.

**A** partir deste momento não mais utilizarei a palavra recurso. Uma vez que o Terraform chama os recursos de *resources*, devemos nos acostumar com sua nomenclatura.

**A**ntes de mais nada, vamos criar um diretório para nosso projeto. Chamaremos nosso projeto de Marioweb, visto que se trata de uma versao open source do jogo Mario.

**A**qui estarei criando o diretório via linha de comando, mas sinta-se livre para criar um diretório da forma que você preferir. Após criar o diretório, com algum terminal ou console aberto (ou prompt do CMD para usuários do Windows), navegue até este diretório recém criado:

```
$ mkdir marioweb

$ cd marioweb
```

**D**entro do diretório *marioweb* crie um novo arquivo chamado *main.tf*.

**P**ara manter um padrão, os arquivos de código do Terraform costumam utilizar o sufixo/extensão .tf e o principal arquivo em módulos ou projetos Terraform costuma se chamar *main.tf*, por se tratar do arquivo principal do módulo ou projeto.

**P**ara criar este arquivo você poderá utilizar qualquer editor de textos de sua escolha: vim, emacs, vi, notepad, notepad++, sublime, atom, etc.

**E**m nosso arquivo *main.tf* insira o seguinte conteúdo por enquanto:

**main.tf**

``` py main.tf
# Baixar a imagem do Projeto Docker-SuperMario
resource "docker_image" "image_id" {
  name = "pengbai/docker-supermario:latest"
}
```

**O** que temos no bloco de código acima:

1.  A primeira linha é apenas um comentário. Como boa prática, é importante termos comentários ao longo de nosso código para descrever o que pretendemos com aquele determinado trecho de código. E por hora, o que pretendemos é exatamente apenas isso: *Baixar a imagem do Projeto Docker-SuperMario* para nosso ambiente.
2.  Na linha 2 estamos especificando que queremos utilizar um *resource*. Cada *resource* no Terraform leva dois parâmetros, sendo um deles o *tipo de resource* e o outro um *nome qualquer* para este resource. Como dito antes, este trecho de código pretende baixar a imagem do projeto SuperMario, portanto precisamos do tipo de *resource* chamado *docker_image*. Este é apenas um dos milhares de *resources* existentes para o Terraform. Em seguida estamos dando o nome *image_id* para nosso *resource* de tipo *docker_image*. O nome poderia ser qualquer um, até mesmo *minha_imagem_do_coracao*, mas para ficar mais descritiva e mantendo boas práticas, utilizarei *image_id*. Uma vez que identificamos o tipo de *resource* e o nome que queremos dar para ele, devemos encerrar a linha abrindo o bloco de código no qual listaremos os atributos deste *resource*. Para isto, utilizaremos um *{* para abrir este bloco.
3.  Na linha 3 começamos a definir os atributos do nosso *resource*. A documentação do Terraform é excelente e lista todos os *resources* suportados, bem como todos os *atributos* suportados por cada *resource*. Neste caso, o único atributo que precisamos no momento é o *name*, ou nome da imagem que desejamos baixar. Em seguida, indicamos qual o nome da imagem desejada. Por padrão, o Docker adota a nomenclatura *<REPOSITÓRIO/IMAGEM:TAG>* para indicar a imagem desejada. Em nosso caso, o repositório onde a imagem se encontra se chama *pengbai* e a imagem em si é chamada de *docker-supermario*, portanto teremos: *name = "pengbai/super-mario"*. A *tag* não é obrigatória. Mas como desejo garantir que utilizaremos sempre a imagem mais recente, utilizarei a tag *latest* (última).
4.  Uma vez que concluímos a definição de nosso *resource*, podemos fechar o nosso bloco de código para o mesmo utilizando um *}* na linha 4.

**A**gora que temos o início de nosso código, já podemos começar a testá-lo.

**D**e volta ao nosso terminal/console, vamos iniciar o nosso ambiente Terraform para este projeto utilizando o comando *terraform init*. Este comando inicia nosso ambiente e baixa os plugins necessários para nosso projeto. No nosso caso, o Terraform baixará os plugins necessários para que nosso código possa lidar com o Docker.

``` yaml
$ terraform init

Initializing provider plugins...
- Checking for available provider plugins on https://releases.hashicorp.com...
- Downloading plugin for provider "docker" (1.1.0)...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.docker: version = "~> 1.1"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

**S**e você recebeu um retorno parecido com o meu, significa que tudo está como deveria e que seu projeto foi iniciado com sucesso. Caso você liste os arquivos e diretórios ocultos de seu diretório, perceberá que ao rodar o comando *terraform init*, um diretório oculto chamado *terraform* foi criado. É nele que ficarão as informações que o Terraform precisa para executar corretamente o seu código, incluindo os plugins que ele necessita. No nosso caso, o plugin para o Docker estará lá.

**N**osso próximo passo será executar o *planejamento* de nosso código. Ao rodar o *planejamento* o terraform listará exatamente tudo o que fará caso nosso código seja de fato executado. Novamente em nosso console/terminal, execute *terraform plan*:

``` yaml
$ terraform plan

Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

ATENÇÃO AQUI:
------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + docker_image.image_id
      id:     <computed>
      latest: <computed>
      name:   "pengbai/docker-supermario:latest"


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**C**aso você tenha recebido um retorno similar a este, significa que tudo parece correto em seu código e que apenas uma ação será executada, conforme descrito no resumo do plano ao final:

*Plan: 1 to add, 0 to change, 0 to destroy.*

**O**u seja: Plano: 1 a adicionar, 0 a alterar, 0 a destruir.

**E**xatamente o que queremos.

**C**aso você tenha recebido uma mensagem de erro, significa que algo em seu código está errado. Por exemplo, se ao invés de utilizarmos *docker_image* como tipo de resource, utilizarmos *docker_images*, o resultado de meu *terraform plan* seria o seguinte:

``` yaml
$ terraform plan

Error: docker_images.image_id: Provider doesn't support resource: docker_images
```

**A** mensagem geralmente é clara e nos indica onde está o erro. No caso acima, o Terraform nos diz que o resource *docker_images* não é suportado. Se checarmos a documentação do Terraform, veremos que o nome correto do *resource* é *docker_image* (no singular).

**U**ma vez que nosso plano foi executado sem erros, chegou a hora de aplicarmos nosso projeto.

**E**xecute o seu código através do comando *terraform apply*:

``` yaml
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + docker_image.image_id
      id:     <computed>
      latest: <computed>
      name:   "pengbai/docker-supermario:latest"


Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
```

**R**epare que mesmo ao utilizar *apply* ao invés de *plan*, uma espécie de planejamento também foi realizado antes da aplicação propriamente dita. O Terraform avaliou o código e nos indicou o que será realizado, perguntando-nos ao final se queremos ou não seguir com a execução. Caso tudo nos pareça correto, basta digitarmos *yes* e pressionar Enter novamente para que ele siga com a execução de fato.

``` yaml

  Enter a value: yes

docker_image.image_id: Creating...
  latest: "" => "<computed>"
  name:   "" => "pengbai/docker-supermario:latest"
docker_image.image_id: Still creating... (10s elapsed)
docker_image.image_id: Still creating... (20s elapsed)
docker_image.image_id: Still creating... (30s elapsed)
docker_image.image_id: Still creating... (40s elapsed)
docker_image.image_id: Creation complete after 47s (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

**A**ssim como no *plan*, o Terraform ao final nos deu um breve relatório do que foi feito:

*Apply complete! Resources: 1 added, 0 changed, 0 destroyed.*

**O**u seja: Aplicação completa! Recursos: 1 adicionado, 0 alterados, 0 destruídos.

**S**e quisermos ter certeza de que de fato o Terraform baixou a imagem Docker de que precisamos, basta digitarmos o comando do Docker que lista as imagens que possuímos em nosso ambiente. A nossa nova imagem do supermario deverá estar lá. Digite *docker images*:

```
$ docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
pengbai/docker-supermario   latest              49beaba1c5cc        4 months ago        686MB
```

**Ó**timo, nossa imagem está presente em nosso ambiente.

**O** Terraform também nos permite saber o que estamos utilizando em termos de *resources* através do comando *terraform show*:

``` yaml
$ terraform show

docker_image.image_id:
  id = sha256:49beaba1c5cc49d2fa424ac03a15b0e761f637e835c1ed4d8108cc247a9c3d62pengbai/docker-supermario:latest
  latest = sha256:49beaba1c5cc49d2fa424ac03a15b0e761f637e835c1ed4d8108cc247a9c3d62
  name = pengbai/docker-supermario:latest
```

**V**oltemos ao nosso código. Agora que já conseguimos fazer com que nosso código baixe a imagem que utilizaremos via Docker, chegou a hora de fazer algo com ela. Precisamos realizar o *deployment* da mesma em um container, certo?!

**V**amos adicionar mais um *resource* em nosso código, desta vez um *resource* de tipo *docker_container*. Como o nome já diz, este *resource* lida com o container em si, e não mais apenas com a imagem.

**S**eu código agora deverá estar da seguinte forma:

``` py main.tf
# Baixar a imagem do Projeto Docker-SuperMario
resource "docker_image" "image_id" {
  name = "pengbai/docker-supermario:latest"
}

# Inicia o Container
resource "docker_container" "container_id" {
  name  = "supermario"
  image = "${docker_image.image_id.latest}"
  ports {
    internal = "8080"
    external = "80"
  }
}
```

**I**gnorando as linhas já descritas anteriormente, vamos descrever as novas linhas de nosso código:

1.  Na linha 6 inserimos apenas mais um comentário, indicando que ali começaremos a descrever o código que criará nosso Container.
2.  Na linha 7 indicamos que queremos mais um *resource*. Desta vez o tipo de *resource* que queremos é o *docker_container*, indicando também que queremos dar o nome *container_id* a este *resource*. Novamente, ao fim da linha, abriremos o bloco de código para este *resource* com uma *{*.
3.  Dentro de nosso bloco, na linha 8, começaremos a listar os atributos deste *resource*. O primeiro atributo que listaremos é o *name*, e para ele daremos o nome *supermario*.
4.  Na linha 9 indicaremos o atributo *image* e utilizaremos nossa primeira interpolação, onde reutilizaremos valores de outra parte de nosso código como se fossem variáveis. Em nosso *resource* anterior, *docker_image*, demos um nome *image_id* que será utilizado agora. Incluiremos também a tag *latest*, pois, conforme pudemos ver na saída de nosso comando *terraform show*, esta foi a tag utilizada pelo terraform para identificar o último status daquele *resource*. Portanto, aqui utilizaremos a interpolação inserindo o que queremos entre *{}* seguidas do símbolo *$*, conforme prega a sintaxe do Terraform para interpolação de valores, ficando o seguinte: *${docker_image.image_id.latest}*, onde *docker_image* é o tipo de resource de onde queremos o valor, *image_id* é o nome deste resource e *latest* é a tag para indicar que queremos o último valor daquele *resource*. O único motivo pelo qual temos um tipo de *resource* e um nome de *resource* é facilitar a identificação quando possuímos diversos *resources* do mesmo tipo. Imagine um projeto em que utilizaremos 5 imagens diferentes do Docker. Teríamos 5 *resources* do tipo *docker_image*, porém cada um deles teria um nome diferente, certo?!
5.  Na linha 10 de nosso código iniciamos o bloco de portas, afinal, toda aplicação roda em uma porta específica e com containers não seria diferente.
6.  Nas linhas 11 e 12 indicamos os valores para portas *intern* e *externß*, onde a porta *intern* será a porta utilizada pela aplicação internamente no container, e *extern* será a porta que o Docker irá mapear em nosso sistema local para que possamos acessar a nossa aplicação. Portanto, em nosso exemplo, a aplicação supermario irá rodar na porta *8080* internamente no container, e a porta 80 será mapeada para que possamos acessá-la de nosso navegador local.
7.  Nas linhas 13 e 14 apenas fecharemos os dois blocos de código que criamos, sendo estes o bloco *ports* e o bloco *resource* do *docker_container*.

**N**ovamente, vamos planejar nosso projeto com *terraform plan*:

``` yaml
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

docker_image.image_id: Refreshing state... (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + docker_container.container_id
      id:               <computed>
      attach:           "false"
      bridge:           <computed>
      container_logs:   <computed>
      exit_code:        <computed>
      gateway:          <computed>
      image:            "sha256:49beaba1c5cc49d2fa424ac03a15b0e761f637e835c1ed4d8108cc247a9c3d62"
      ip_address:       <computed>
      ip_prefix_length: <computed>
      log_driver:       "json-file"
      logs:             "false"
      must_run:         "true"
      name:             "supermario"
      network_data.#:   <computed>
      ports.#:          "1"
      ports.0.external: "80"
      ports.0.internal: "8080"
      ports.0.ip:       "0.0.0.0"
      ports.0.protocol: "tcp"
      restart:          "no"
      rm:               "false"
      start:            "true"


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**R**epare que desta vez apenas 1 ação será executada: a criação do container. Não estamos mais recebendo as informações referentes à ação de baixar a imagem. O motivo para isto é a propriedade de idempotência que citei anteriormente. O Terraform sabe que a imagem já foi baixada, portanto a mesma não precisa ser baixada novamente, a menos que tivéssemos mudado a versão da mesma, nome, repositório, etc.

**U**ma vez que o plano esteja de acordo com o que queremos, podemos aplicar nosso código com *terraform apply*:

``` yaml
$ terraform apply
docker_image.image_id: Refreshing state... (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + docker_container.container_id
      id:               <computed>
      attach:           "false"
      bridge:           <computed>
      container_logs:   <computed>
      exit_code:        <computed>
      gateway:          <computed>
      image:            "sha256:49beaba1c5cc49d2fa424ac03a15b0e761f637e835c1ed4d8108cc247a9c3d62"
      ip_address:       <computed>
      ip_prefix_length: <computed>
      log_driver:       "json-file"
      logs:             "false"
      must_run:         "true"
      name:             "supermario"
      network_data.#:   <computed>
      ports.#:          "1"
      ports.0.external: "80"
      ports.0.internal: "8080"
      ports.0.ip:       "0.0.0.0"
      ports.0.protocol: "tcp"
      restart:          "no"
      rm:               "false"
      start:            "true"


Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
```

**M**ais uma vez ele nos dá uma visão geral do que será feito e nos perguntará se queremos prosseguir. Digite *yes* e pressione Enter novamente.

``` yaml
Enter a value: yes

docker_container.container_id: Creating...
attach:           "" => "false"
bridge:           "" => "<computed>"
container_logs:   "" => "<computed>"
exit_code:        "" => "<computed>"
gateway:          "" => "<computed>"
image:            "" => "sha256:49beaba1c5cc49d2fa424ac03a15b0e761f637e835c1ed4d8108cc247a9c3d62"
ip_address:       "" => "<computed>"
ip_prefix_length: "" => "<computed>"
log_driver:       "" => "json-file"
logs:             "" => "false"
must_run:         "" => "true"
name:             "" => "supermario"
network_data.#:   "" => "<computed>"
ports.#:          "" => "1"
ports.0.external: "" => "80"
ports.0.internal: "" => "8080"
ports.0.ip:       "" => "0.0.0.0"
ports.0.protocol: "" => "tcp"
restart:          "" => "no"
rm:               "" => "false"
start:            "" => "true"
docker_container.container_id: Creation complete after 1s (ID: 8c9d35eac2fcdc0c7e530567323167b82e72f33d4645abf20685b99d802e2359)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

**C**onforme o esperado: Aplicaçao completa! *Resources*: 1 adicionado, 0 alterados, 0 destruídos.

**M**ais uma vez podemos verificar se de fato tudo funcionou como o esperado através do Docker. Desta vez, não queremos apenas baixar uma imagem Docker, mas sim criar um container com a mesma abrindo portas específicas que serão mapeadas entre nosso sistema local e nosso container. Execute agora *docker ps* para ver os containers que estão rodando neste momento:

```
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
8c9d35eac2fc        49beaba1c5cc        "catalina.sh run"   2 minutes ago       Up 2 minutes        0.0.0.0:80->8080/tcp   supermario
```

**C**omo podemos ver, temos um container rodando. Podemos até ver que existe um mapeamento de portas: *80->8080*

**N**ão está convencido ainda?

**A**bra seu navegador e acesse o seguinte endereço: *localhost:80*

**C**aso o seu resultado seja algo parecido com a imagem, significa que seu código funcionou conforme o esperado.
![Super Mario](/img/mariodocker.png)

**O** terraform também nos permite destruir a nossa infraestrutura com o comando *terraform destroy*. Da mesma forma que o *apply*, o comando *destroy* também lhe dará uma prévia de o que será destruído e lhe pedirá par aconfirmar com um *yes* ou *no*:

``` yaml
$ terraform destroy
docker_image.image_id: Refreshing state... (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)
docker_container.container_id: Refreshing state... (ID: 8c9d35eac2fcdc0c7e530567323167b82e72f33d4645abf20685b99d802e2359)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - docker_container.container_id

  - docker_image.image_id


Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.container_id: Destroying... (ID: 8c9d35eac2fcdc0c7e530567323167b82e72f33d4645abf20685b99d802e2359)
docker_container.container_id: Destruction complete after 0s
docker_image.image_id: Destroying... (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)
docker_image.image_id: Destruction complete after 2s

Destroy complete! Resources: 2 destroyed.
```

**C**omo podemos ver, o terraform destruiu dois *resources*, nosso container e nossa imagem. Você poderá confirmar isto tentando acessar novamente o jogo pelo seu navegador ou mesmo através dos comandos *docker images* e *docker ls* para ver que tanto o container quanto a imagem foram removidos de nosso sistema:

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

**D**e volta ao nosso código, vamos incrementá-lo apenas um pouco mais.

**O** terraform nos permite especificar também *outputs*, ou saídas que nos serão apresentadas ao executarmos nosso código. Tratam-se de informações que podem nos ser úteis.

**P**or exemplo, supomos que ao executar nosso código, desejamos que o terraform nos informe o IP do container que foi criado e o nome do mesmo, nosso código agora ficaria assim:

``` py main.tf
# Baixar a imagem do Projeto Docker-SuperMario
resource "docker_image" "image_id" {
  name = "pengbai/docker-supermario:latest"
}

# Inicia o Container
resource "docker_container" "container_id" {
  name  = "supermario"
  image = "${docker_image.image_id.latest}"
  ports {
    internal = "8080"
    external = "80"
  }
}

# Nos informa o ip e nome do container criado
output "Endereco IP" {
  value = "${docker_container.container_id.ip_address}"
}

output "Nome do Container" {
  value = "${docker_container.container_id.name}"
}
```

**N**ovamente, ignorando o código que já descrevemos anteriormente, teremos:

1.  Na linha 16 inserimos mais um comentário.
2.  Na linha 17 especificamos que desta vez queremos um *output*, e não mais um *resource*. Da mesma forma que fizemos com *resources*, vamos dar um nome a este *output*, de forma que possamos facilmente identificá-lo posteriormente. No caso, vamos chamar nosso *output* de *Endereco IP*. Em seguida abriremos o bloco de código para este *output* novamente com *{*.
3.  Na linha 18 daremos o *value* ou valor deste output. Novamente utilizaremos interpolação de valores para pegar este valor de nossos *resources*. Para conseguirmos o endereço ip do container, utilizaremos o atributo *ip_address*, que é um atributo descrito na documentação do Terraform como parte integrante do resource de tipo *docker_container*. Portanto, nosso *value* será: ${docker_container.container_id.ip_address}.
4.  Na linha 19, apenas fechamos o bloco deste *output*.
5.  Na linha 21 iniciamos nosso segundo *output*, com o nome *Nome do Container*. Em seguida, abriremos o bloco de código para este *output* com um *{*.
6.  Na linha 22, faremos algo similar ao que fizemos com o *value* do output anterior. Utilizaremos interpolação para buscar o valor do atributo *name*, que faz parte do *resource* *docker_container*. Nosso *value* será: ${docker_container.container_id.name}
7.  Na linha 23, apenas fechamos nosso *output*.

**V**amos então rodar nosso *plan* para ver o que aconteceria desta vez:

``` yaml
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + docker_container.container_id
      id:               <computed>
      attach:           "false"
      bridge:           <computed>
      container_logs:   <computed>
      exit_code:        <computed>
      gateway:          <computed>
      image:            "${docker_image.image_id.latest}"
      ip_address:       <computed>
      ip_prefix_length: <computed>
      log_driver:       "json-file"
      logs:             "false"
      must_run:         "true"
      name:             "supermario"
      network_data.#:   <computed>
      ports.#:          "1"
      ports.0.external: "80"
      ports.0.internal: "8080"
      ports.0.ip:       "0.0.0.0"
      ports.0.protocol: "tcp"
      restart:          "no"
      rm:               "false"
      start:            "true"

  + docker_image.image_id
      id:               <computed>
      latest:           <computed>
      name:             "pengbai/docker-supermario:latest"


Plan: 2 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**D**esta vez podemos ver que ambas as ações serão executadas: A imagem será baixada e o container será criado, afinal tínhamos removido tudo com *terraform destroy* anteriormente.

**V**amos aplicar nosso código e confirmar com *yes*:

``` yaml
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + docker_container.container_id
      id:               <computed>
      attach:           "false"
      bridge:           <computed>
      container_logs:   <computed>
      exit_code:        <computed>
      gateway:          <computed>
      image:            "${docker_image.image_id.latest}"
      ip_address:       <computed>
      ip_prefix_length: <computed>
      log_driver:       "json-file"
      logs:             "false"
      must_run:         "true"
      name:             "supermario"
      network_data.#:   <computed>
      ports.#:          "1"
      ports.0.external: "80"
      ports.0.internal: "8080"
      ports.0.ip:       "0.0.0.0"
      ports.0.protocol: "tcp"
      restart:          "no"
      rm:               "false"
      start:            "true"

  + docker_image.image_id
      id:               <computed>
      latest:           <computed>
      name:             "pengbai/docker-supermario:latest"


Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.image_id: Creating...
  latest: "" => "<computed>"
  name:   "" => "pengbai/docker-supermario:latest"
docker_image.image_id: Still creating... (10s elapsed)
docker_image.image_id: Still creating... (20s elapsed)
docker_image.image_id: Still creating... (30s elapsed)
docker_image.image_id: Still creating... (40s elapsed)
docker_image.image_id: Still creating... (50s elapsed)
docker_image.image_id: Creation complete after 59s (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)
docker_container.container_id: Creating...
  attach:           "" => "false"
  bridge:           "" => "<computed>"
  container_logs:   "" => "<computed>"
  exit_code:        "" => "<computed>"
  gateway:          "" => "<computed>"
  image:            "" => "sha256:49beaba1c5cc49d2fa424ac03a15b0e761f637e835c1ed4d8108cc247a9c3d62"
  ip_address:       "" => "<computed>"
  ip_prefix_length: "" => "<computed>"
  log_driver:       "" => "json-file"
  logs:             "" => "false"
  must_run:         "" => "true"
  name:             "" => "supermario"
  network_data.#:   "" => "<computed>"
  ports.#:          "" => "1"
  ports.0.external: "" => "80"
  ports.0.internal: "" => "8080"
  ports.0.ip:       "" => "0.0.0.0"
  ports.0.protocol: "" => "tcp"
  restart:          "" => "no"
  rm:               "" => "false"
  start:            "" => "true"
docker_container.container_id: Creation complete after 0s (ID: 655604d672af8ff76c10aca4cd169a6aa284dcca17f0e0215374fb18c86660fd)

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

Endereco IP = 172.17.0.2
Nome do Container = supermario
```

**R**epare que tudo o que queríamos foi executado e que, ao final, recebemos duas saídas ou *outputs*:

``` yaml
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

Endereco IP = 172.17.0.2
Nome do Container = supermario
```

**N**ovamente, se você acessar em seu navegador o endereço *localhost:80*, ou utilizar os comandos *docker ps*, perceberá que sua aplicação está novamente rodando.

**N**ão é complicado, certo?!

**O**bviamente, isto é apenas um exemplo extremamente simplista de uso do Terraform para criar uma pequena infraestrutura como código, o que em nosso caso é apenas um container.

**N**o próximo post pretendo alterar um pouco este nosso código para utilizar algumas melhores práticas propostas pelo Terraform, como a utilização de variáveis e outputs em arquivos distintos, já que não utilizamos variáveis neste post.

**U**m passo de cada vez, certo?!

**H**appy Hacking!
