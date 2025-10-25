---
title: "Terraform: Variáveis e Outputs"
date: 2018-11-06T08:40:27-05:00
draft: false
tags: [
"Devops",
"Terraform",
"Cloud",
"GCP",
"Docker",
]
categories: ["Tutoriais"]
description: "Uma introdução ao Terraform, da Hashicorp, como robusta e eficiente ferramenta para infraestrutura como código. Separando variáveis e Outputs."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
Uma introdução ao Terraform, da Hashicorp, como robusta e eficiente ferramenta para infraestrutura como código. Separando variáveis e Outputs.

<!--more-->

## Onde paramos

**A**ntes de seguir em frente com esta leitura gostaria de dizer que este post é continuação do anterior, onde dei uma breve introdução ao Terraform, com um exemplo prático em que fizemos o deployment de um jogo web do Mario em um container Docker.

**E**ste post na verdade utilizará o mesmo código que escrevemos no post anterior, portanto se você não o leu, recomendo fortemente que o faça [clicando aqui](/posts/introducao-ao-terraform/).

## Variáveis

**E**mbora nosso código tenha funcionado corretamente, ele não estava limpo. Existem algumas boas práticas que devemos sempre tentar seguir, Não apenas para deixar o código limpo, mas também para facilitar a manutenção do mesmo.

**I**magine o seguinte código em Ruby:

```ruby
puts "Meu nome é Marcelo."
puts "O Marcelo gosta de escrever códigos."
puts "Mas o Marcelo também gosta de surfar."
puts "Não sendo bom o suficiente a ponto de se tornar um profissional do surf, Marcelo decidiu seguir com a carreira de TI."
puts "Este é o Marcelo."
```

**U**m código extremamente simples que apenas imprime diversas strings na tela. Imagine que você precisa fazer manutenção deste código pois sua empresa agora decidiu que o personagem da história seria Pedro e não mais Marcelo. Claro, você pode ir lendo linha a linha e alterando em cada linha, mas isso leva muito mais tempo do que deveria. Imagine agora que este sistema possua algumas centenas de linhas de código. Ou múltiplos arquivos. Começa a ficar mais complexo e demorado alterar tudo, sem falar que fica fácil cometer o erro de esquecer algum. Por outro lado, se o nosso código utilizasse variáveis, apenas trocaríamos o valor em um local, tendo assim certeza absoluta de que o mesmo estaria correto em todo o código. Por exemplo:

```ruby
nome = "Marcelo"

puts ("Meu nome é " + nome + ".")
puts ("O " + nome + " gosta de escrever códigos.")
puts ("Mas o " + nome + " também gosta de surfar.")
puts ("Não sendo bom o suficiente a ponto de se tornar um profissional do surf, " + nome + " decidiu seguir com a carreira de TI.")
puts ("Este é o " + nome + ".")
```

**N**este código, quando precisarmos trocar o nome da pessoa e utilizar Pedro ao invés de Marcelo, precisaríamos alterar apenas o valor da variável na linha 1. muito mais simples, certo?!

**D**a mesma forma que em programação básica utilizamos variáveis, quando pensamos em infraestrutura como código deveríamos pensar da mesma forma, afinal estamos programando, certo?! Nao é um sistema, mas ainda assim estamos programando nossa infraestrutura.

**E**ste é o nosso arquivo main.tf completo do post anterior:

```yaml
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

**N**este código não estamos utilizando variáveis, embora tenhamos um pouco de interpolação de valores. Vamos então começar a criar algumas variáveis, mas, seguindo as boas práticas do Terraform, criaremos um arquivo separado para nossas variáveis.

**C**rie um arquivo chamado *variables.tf*. O motivo pelo qual utilizaremos o nome em inglês aqui é por ser este o padrão adotado pelo Terraform. Ao chamarmos uma variável em nosso código, o Terraform saberá onde buscar o valor daquela variável.

**P**ara cada variável daremos um nome, uma descrição e um valor *default*. Nosso arquivo *variables.tf* ficará assim:

```yaml
variable "nome_container" {
  description = "Nome do container"
  default = "supermario"
}

variable "imagem" {
  description = "Imagem do container"
  default = "pengbai/docker-supermario:latest"
}

variable "porta_interna" {
  description = "Porta interna do container"
  default = "8080"
}

variable "porta_externa" {
  description = "Porta externa do container"
  default = "80"
}
```

**O** que definimos:

1.  Criamos 4 variáveis aqui: *nome_container*, *imagem*, *porta_interna* e *porta_externa*;
2.  Para cada variável nós demos 2 atributos: *description* (descrição) e *default* (Valor padrão);
3.  Variáveis não precisam ser sempre declaradas. Existem ocasiões em que podemos criar uma variável sem qualquer valor atribuído à mesma, de forma que o valor será passado durante a execução do código, por exemplo. Por padrão, quando queremos que a variável possua um valor inicial padrão, o terraform utiliza o atributo *default*;
4.  A *description*, ou descrição, é um atributo também opcional, mas ajuda a identificar melhor o que se pretende com aquela variável e costuma ser uma boa prática, dando maior legibilidade ao seu código.

**A**gora que temos um arquivo com estas 4 variáveis, devemos voltar ao nosso arquivo *main.tf* e alterar um pouco nosso código para que possamos fazer uso destas variáveis. Iremso alterar nosso código bloco a bloco para ficar mais fácil identificarmos as diferenças.

**C**omecemos com o resource *docker_image*, que agora ficará da seguinte forma em nosso arquivo *main.tf*:

```yaml
# Baixar a imagem do Projeto Docker-SuperMario
resource "docker_image" "image_id" {
  name = "${var.imagem}"
}
```

**O** que alteramos:

1.  Nas linhas 1 e 2 não alteramos nada, pois são apenas comentários e a abertura de nosso *resource*;
2.  Na linha 3 tínhamos *name = "pengbai/docker-supermario:latest"* e agora temos *name = "${var.imagem}"*. Basicamente indicamos que o valor para *name* agora deverá ser pego a partir de nossa variável *imagem* em nosso arquivo *variables.tf*. Sim, para pegarmos o valor de uma variável, no Terraform, utilizamos sempre esta sintaxe: *"${}"*. Dentro das chaves iremos indicar onde se encontra a nossa variável. quando utilizamos o prefixo *var*, o Terraform busca automaticamente o valor em um arquivo *variables.tf*. Existem outras formas de declarar variáveis, mas não nos preocuparemos com isso por enquanto. Se checarmos novamente nosso arquivo *variables.tf* veremos a variável *imagem* que criamos, cujo valor é exatamente *pengbai/docker-supermario:latest*;
3.  Novamente, na linha 4, nenhuma alteração foi feita. Estamos apenas fechando nosso bloco de *resource*.

**V**amos ao nosso próximo bloco de código, nosso *resource* *docker_container*. Alteremos o código para que fique da seguinte forma:

```yaml
# Baixar a imagem do Projeto Docker-SuperMario
resource "docker_image" "image_id" {
  name = "${var.imagem}"
}

# Inicia o Container
resource "docker_container" "container_id" {
  name  = "${var.nome_container}"
  image = "${docker_image.image_id.latest}"
  ports {
    internal = "${var.porta_interna}"
    external = "${var.porta_externa}"
  }
}
```

**D**a mesma forma que fizemos antes, apenas trouxemos nossas variáveis:

1.  Na linha 8 passamos a utilizar a variável *nome_container* que criamos para dar o nome ao nosso container. Novamente, em nosso arquivo *variables.tf* você será capaz de encontrar a variável *nome_container*, cujo valor default é *supermario*;
2.  Na linha 11 apenas trocamos o valor *8080* pela variável *porta_interna*, assim como específicamos em nosso arquivo *variables.tf*;
3.  Na linha 12, assim como na linha 11, apenas trocamos o valor *80* pela variável *porta_externa*.

**N**osso arquivo *main.tf* agora deverá estar assim:

```yaml
# Baixar a imagem do Projeto Docker-SuperMario
resource "docker_image" "image_id" {
  name = "${var.imagem}"
}

# Inicia o Container
resource "docker_container" "container_id" {
  name  = "${var.nome_container}"
  image = "${docker_image.image_id.latest}"
  ports {
    internal = "${var.porta_interna}"
    external = "${var.porta_externa}"
  }
}

# Nos informa o ip e nome do container criado
output "Endereco IP" {
  value = "${docker_container.container_id.ip_address}"
}

output "Nome do Container" {
  value = "${docker_container.container_id.name}"
```

**A**cho sempre interessante fazer testes constantes em nosso código para ter certeza de que tudo está funcionando conforme o esperado. Como alteramos um pouco nosso código, criando variáveis em um arquivo *variables.tf* e removemos de nosso *main.tf* valores absolutos para fazermos uso de variáveis, é bom termos certeza de que não cometemos nenhum erro. Assumindo que temos o serviço do Docker rodando, vamos ao nosso terminal e, dentro de nosso diretório *marioweb* criado no posto anterior, vamos nos certificar de que destruímos a aplicação do post anterior para que não tenhamos nenhum container rodando:

```bash
$ terraform destroy
```

**A**gora vamos executar nosso *plan*:

```bash
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

**A**parentemente está tudo correto. Na saída de nosso *plan* podemos ver que os valores estão de acordo com o esperado. Apliquemos então nosso código com *terraform apply*:

```bash
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

  Enter a value:
```

**A**ssim como vimos no post anterior, o *terraform apply* sempre nos apresenta uma prévia das tarefas que serão executadas e em seguida nos pede uma confirmação de execução. Como tudo parece correto, vamos confirmar com um *yes*:

```bash

  Enter a value: yes

docker_image.image_id: Creating...
  latest: "" => "<computed>"
  name:   "" => "pengbai/docker-supermario:latest"
docker_image.image_id: Still creating... (10s elapsed)
docker_image.image_id: Still creating... (20s elapsed)
docker_image.image_id: Still creating... (30s elapsed)
docker_image.image_id: Creation complete after 37s (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)
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
docker_container.container_id: Creation complete after 1s (ID: bbe9e8e7b5428532b882e7fbd304fc2b3d71e0bcb29fa099e15162397731e15e)

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

Endereco IP = 172.17.0.2
Nome do Container = supermario
```

**A**parentemente tudo saiu conforme o esperado, com 2 *resources* adicionados, sendo eles nossa imagem e nosso container.

**N**ovamente, podemos verificar que tudo está correto através do comando *docker ps*, onde deveremos ver que nosso container está rodando:

```bash
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS                  NAMES
bbe9e8e7b542        49beaba1c5cc        "catalina.sh run"   About a minute ago   Up About a minute   0.0.0.0:80->8080/tcp   supermario
```

**T**ambém podemos tentar acessar em nosso browser ou navegador o seguinte endereço: *localhost:80*. Nosso jogo do mario deverá estar funcionando.

**A**gora que entendemos o básico sobre o uso de variáveis e vimos que nosso código, embora um pouco diferente, continua funcionando, chegou a hora de corrigirmos nossos *outputs*. Eles continuam funcionando, porém para seguirmos os padrões e melhores práticas, vamos também retirá-los de nosso *main.tf* e criar um arquivo dedicado para isto.

## Outputs

**C**omecemos criando um arquivo chamado *outputs.tf* com o seguinte conteúdo:

```yaml
# Nos informa o ip e nome do container criado
output "Endereco IP" {
  value = "${docker_container.container_id.ip_address}"
}

output "Nome do Container" {
  value = "${docker_container.container_id.name}"
}
```

**E**ste foi fácil, certo?! Se prestarmos atenção, não alteramos praticamente nada. Apenas copiamos os dois blocos *outputs* do arquivo *main.tf* sem qualquer alteração.

**A**pós salvar nosso arquivo *outputs.tf*, removeremos estes dois *outputs* do arquivo *main.tf*. Nosso *main.tf* ficará assim:

```yaml
# Baixar a imagem do Projeto Docker-SuperMario
resource "docker_image" "image_id" {
  name = "${var.imagem}"
}

# Inicia o Container
resource "docker_container" "container_id" {
  name  = "${var.nome_container}"
  image = "${docker_image.image_id.latest}"
  ports {
    internal = "${var.porta_interna}"
    external = "${var.porta_externa}"
  }
}
```

**S**imples, não? Nosso código está mais limpo e organizado. Vamos destuir novamente nosso projeto com *terraform destroy* para que possamos testar estas últimas alterações:

```bash
$ terraform destroy

docker_image.image_id: Refreshing state... (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)
docker_container.container_id: Refreshing state... (ID: bbe9e8e7b5428532b882e7fbd304fc2b3d71e0bcb29fa099e15162397731e15e)

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

docker_container.container_id: Destroying... (ID: bbe9e8e7b5428532b882e7fbd304fc2b3d71e0bcb29fa099e15162397731e15e)
docker_container.container_id: Destruction complete after 1s
docker_image.image_id: Destroying... (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)
docker_image.image_id: Destruction complete after 1s

Destroy complete! Resources: 2 destroyed.
```

**A**gora vamos aplicar nosso *plan* e em seguida, caso tudo esteja correto, vamos executar *terraform plan*:

```bash
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

```bash
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
docker_image.image_id: Creation complete after 42s (ID: sha256:49beaba1c5cc49d2fa424ac03a15b0e7...9c3d62pengbai/docker-supermario:latest)
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
docker_container.container_id: Creation complete after 1s (ID: 5619b9c45b2509ca1a67cb1d43ea8e91f156f44245539604ad3dd060793900a4)

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

Endereco IP = 172.17.0.2
Nome do Container = supermario
```

**S**ucesso. Tudo saiu como o esperado e nossa aplicação está novamente no ar. Sinta-se livre para executar *docker ps* ou mesmo acessar *localhost:80* em seu navegador para ter certeza de que tudo está funcionando e de que seu jogo Mario está no ar.

## Atenção: Outputs como variáveis de saída

**D**a mesma forma que eu citei que existem outras formas de se declarar e utilizar variáveis, existem outras funções também para os *outputs*. Não, o Terraform não utiliza *outputs* apenas para apresentar informações na tela. A principal função dos *outputs* é na verdade a de variáveis de saída. Ou seja, pegar valores que poderão ser utilizados posteriormente. Este recurso é muito utilizado em projetos maiores com infraestruturas mais complexas mas, novamente, não é o foco deste post abordar isto.

**S**e você é um pouco atencioso e curioso, deve ter notado que não definimos os valores dos *outputs* em nenhum momento, certo? Por exemplo: *value = "${docker_container.container_id.ip_address}"*

**O**u seja, estamos criando um *output* cujo valor será na verdade uma saída após o processamento de nosso código Terraform. Desta forma, nossos *outputs* são na verdade variáveis de saída.. mas isso já é uma outra história.

**A** propósito, se você além de curioso é também meticuloso, deve ter ficado confuso e questionado: *Se outputs são na verdade uma espécie de variáveis, como podemos ter espaços em seus nomes? Como por exemplo "Nome do Container"?*

**A** resposta é: Você me pegou. As melhores práticas pregam que não devemos criar *outputs* com espaços. Porque? Porque variáveis não podem conter espaços. Mas, como desde o início nosso objetivo era utilizar os *outputs* aqui apenas para nos retornar algum valor na tela, resolvi utilizar palavras em portugês e com espaços para facilitar a compreensão.

**O** ideal seria termos utilizado *nome_do_container* ao invés de *Nome do Container*, ou *endereco_ip* ao invés de *Endereco IP* mas, novamente.. isto é uma outra história.

**L**embre-se de destuir o seu projeto para não deixar um container rodando desnecessariamente: *$ terraform destroy*

**E**m meu próximo post pretendo elevar um pouco o nível e utilizar o Terraform para criarmos uma infraestrutura básica na nuvem.

**H**appy Hacking!
