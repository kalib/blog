---
title: "Terraform Modules: Porquê E Como Trabalhar Com Módulos No Terraform - PARTE 1"
date: 2019-05-17T20:01:40-04:00
draft: false
image: "/img/terraform-modules.jpg"
tags: [
"Devops",
"Terraform",
"Cloud",
"AWS",
]
description: "Todos sabem que o Terraform é uma solucão fantástica para criar infraestrutura através de código, porém nem todos utilizam as diversas possibilidades do mesmo, como por exemplos módulos. Com módulos podemos fazer a reutilização de código para nossa infraestrutura."
---
**T**odos sabem o quão fantástico o Terraform é quando o assunto é criar infraestrutura de forma automática e através de código, a chamada IaC ou Infraestrutura como Código *(Infrastructure as Code)*. No entanto, nem todos se utilizam de todos os recursos ou funcionalidades que o Terraform nos disponibiliza para tornar nossa vida ainda mais fácil e nosso código mais organizado.

**A** possibilidade de utilizar módulos em nosso código Terraform faz com que possamos ter a reutilização de código, não apenas nosso mas também de terceiros, evitando a repetição de código bem como nos dando flexibilidade para criarmos dezenas de recursos similares porém com suas respectivas particularidades utilizando-se da mesma base de código.

**O** objetivo deste post não é ensinar o básico de Terraform, portanto se você ainda não possui uma certa familiaridade com o Terraform, sugiro que volte um pouco e leia estes posts antes de seguir com esta leitura:

-   <a href="/infraestrutura-como-c%C3%B3digo-com-terraform/" target="_blank">Infraestrutura como Código com Terraform</a>
-   <a href="/introdu%C3%A7%C3%A3o-ao-terraform/" target="_blank">Introdução ao Terraform</a>
-   <a href="/terraform-vari%C3%A1veis-e-outputs/" target="_blank">Terraform: Variáveis e Outputs</a>
-   <a href="/terraform-criando-uma-infraestrutura-no-google-cloud/" target="_blank">Terraform: Criando uma infraestrutura no Google Cloud</a>

# Multi Plataforma - AWS #

![Terraform and AWS](/img/terraform-aws.jpeg)

**J**á que o Terraform é multi plataforma, desta vez utilizarei o AWS como cloud para criar minha infraestrutura, ao contrário dos posts anteriores nos quais utilizei Google Cloud.

**C**omo costumo deixar claro, gosto muito de ambas as opções, GCP (Google Cloud Plataform) e AWS (Amazon Web Services), e não gosto de julgar uma como melhor que a outra. GCP se sai melhor em alguns aspectos, enquanto que AWS se sai melhor em outros, mas isto não é tema para este post.

**C**aso queira acompanhar o passo a passo deste post e praticar os mesmos códigos, você precisará atender a alguns pré-requisitos:

1.  Já possuir uma conta no AWS. O AWS lhe permite criar uma conta para que você utilize alguns recursos gratuitamente durante os primeiros 12 meses para fins de estudos e serão estes os recursos nos quais focarei aqui;
2.  Possuir um usuário não root com sua devida chave. Por padrão, ao criar uma conta no AWS, você sempre logará inicialmente com a conta root ou master. Embora muitas pessoas utilizem esta conta para tudo, é extremamente importante, por questões de segurança, que esta conta seja utilizada única e exclusivamente para criar novos usuários. Portanto, sugiro que você crie um novo usuário qualquer bem como uma chave para o mesmo, de forma que você possa interagir com o AWS programaticamente, via APIs, aws cli ou mesmo via Terraform. Você deverá possuir o *AWS_ACCESS_KEY_ID* deste usuário, bem como sua *AWS_SECRET_ACCESS_KEY*. Estas informações serão utilizadas para que você consiga se comunicar com o AWS através do Terraform;

## Criando uma conta e usuário no AWS ##

**C**aso você já possua uma conta e um usuário pronto para utilização, siga para a próxima sessão: **Criando VMs no AWS com Terraform**

**P**ara criar sua conta no AWS preencha o formulário de cadastro através do seguinte link: <a href="https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start" target="_blank">Cadastro AWS</a>

**A**pós ter sua conta criada, efetue login com seu email para que possamos criar um usuário para nosso exemplo:

**N**o canto superior esquerdo clique em **Serviços** *(Services)*. Busque e clique no serviço **IAM**.
![AWS IAM](/img/terraform_aws1.jpg)
**N**o menu da lateral esquerda clique em **Usuários** *(Users)* e em seguida clique no botão azul **Adicionar Usuário** *(Add User)*.
![AWS IAM](/img/terraform_aws2.jpg)
**I**nsira um nome para seu usuário e, em **Tipo de Acesso** *(Access Type)*, selecione a opcão **Acesso programático** *(Programmatic Access)*.
![AWS IAM](/img/terraform_aws3.jpg)
**C**lique no botão azul do canto inferior direito **Próximo: Permissões** *(Next: Permissions)*.

**N**a tela seguinte clique em **Anexar políticas existentes de forma direta** *(Attach existing policies directly)* e em seguida, no campo de busca, digite **AmazonEC2FullAccess**. Selecione a caixa para atribuir esta *policy* ao seu novo usuário.

![AWS IAM](/img/terraform_aws4.jpg)
**A**gora que demos permissão para criar instâncias EC2 (VMs) ao nosso usuário, clique no botão azul **Próximo: Tags** *(Next: Tags)*.

**E**mbora *Tags* não sejam obrigatórias, por questões de organização e para seguirmos melhores práticas, definiremos uma *Tag* para este usuário: *Uso: Terraform*
![AWS IAM](/img/terraform_aws5.jpg)

**C**lique no botão azul do canto inferior direito, **Próximo: Revisar** *(Next: Review)*.

**C**ertifique-se de que está tudo certo quanto ao seu novo usuário e em seguida clique no botão azul **Criar Usuário** *(Create user)*.

![AWS IAM](/img/terraform_aws6.jpg)

**ATENÇÃO!** Tenha cuidado nesta tela seguinte, pois esta será a única vez em que a senha de sua chave será apresentada. Clique no botão **Fazer download .csv** *(Download .csv)* para realizar o download de um arquivo .csv com sua chave e senha, ou apenas copie o conteúdo exibido na tela para sua chave e senha após clicar em **Exibir** *(Show)* no campo de senha da chave e salve estes dados em algum local seguro, pois você precisará destes dois valores para controlar sua conta AWS programaticamente com este usuário.
![AWS IAM](/img/terraform_aws7.jpg)

**C**om seu usuário criado, clique no botão do canto inferior direito **Fechar** *(Close)*.

## Criando VMs no AWS com Terraform ##

**A**ssumindo que você já possui uma conta no AWS e um usuário com chave para acesso, vamos exportar nossa chave de acesso e nossa senha para esta chave. No Linux ou OS X, execute:

``` bash
$ export AWS_ACCESS_KEY_ID=<SUA_CHAVE_AUI>

$ export AWS_SECRET_ACCESS_KEY=<SUA_SENHA_PARA_A_CHAVE_AQUI>
```

**V**ocê poderá confirmar que seus comandos deram certo digitando:

``` bash
$ echo $AWS_ACCESS_KEY_ID && echo $AWS_SECRET_ACCESS_KEY
```

**C**aso sua chave e senha tenham sido exibidas, podemos partir para o Terraform.

**N**ovamente, estou assumindo que você já possui um conhecimento básico de Terraform, visto que o foco deste post não é explicar detalhadamente o funcionamento do mesmo, mas sim explicar como e porque utilizar módulos no Terraform.

**C**omecemos criando uma instância ou VM Ubuntu Linux simples.

**I**niciemos criando um diretório para nosso projeto e em seguida indo para dentro do mesmo com algum terminal ou console:

``` bash
$ mkdir terraform

$ cd terraform
```

**A** forma mais simplificada de iniciar nosso projeto e criar uma simples instância Ubuntu seria criando um arquivo *main.tf* e inserindo o seguinte conteúdo no mesmo:

{{< highlight py "linenos=table,hl_lines=,linenostart=0" >}}
# Criando uma Instância Ubuntu no AWS
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "server1" {
  ami           = "ami-0a313d6098716f372"
  instance_type = "t2.micro"

  tags = {
    Name     = "Ubuntu-1"
    Ambiente = "Desenvolvimento"
  }
}
{{< / highlight >}}

**O**bviamente este código é extremamente simplista. Sequer estamos definindo variáveis, mas chegaremos lá.

**O** que fizemos:

1.  Indicamos que o nosso *provider* será o **AWS**;
2.  Criamos um *resource* do tipo **aws_instance**;
3.  Indicamos qual o ID exato da imagem que queremos utilizar para nossa instância. A lista completa de AMI ou imagens disponibilizadas pelo AWS pode ser encontrada <a href="https://console.aws.amazon.com/ec2/" target="_balnk">aqui</a>;
4.  Indiquei o tipo de instância como sendo *t2.micro*. Cada tipo de instância representa uma configuração diferente em termos de CPU, memória, etc. Uma lista completa com todos os tipos disponíveis pode ser encontrada <a href="https://aws.amazon.com/sap/instance-types/" target="_blank">aqui</a>;
5.  Inseri algumas tags para seguir melhores práticas e deixar nossa infraestrutura organizada e catalogada;

**P**rimeiramente, vamos testar nosso código e ver se estamos com acesso ao AWS via Terraform.

**V**amos validar que não temos esta instância já criada no AWS clicando em **Serviços** *(Services)* e em seguida buscando o serviço **EC2**:

![AWS IAM](/img/terraform_aws8.jpg)

**E**m seguida clique em **Instâncias** *(Instances)* no menu esquerdo para listar suas instâncias:

![AWS IAM](/img/terraform_aws9.jpg)

**D**e volta ao nosso terminal, no diretório onde criamos nosso arquivo *main.tf*, vamos iniciar o terraform com *terraform init .* :

``` bash
$ terraform init .

Initializing provider plugins...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.aws: version = "~> 2.11"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

**C**aso você não tenha recebido um erro com o *init*, podemos executar o nosso plano ou *plan*:

``` bash
$ terraform plan

Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + aws_instance.server1
      id:                           <computed>
      ami:                          "ami-0a313d6098716f372"
      arn:                          <computed>
      associate_public_ip_address:  <computed>
      availability_zone:            <computed>
      cpu_core_count:               <computed>
      cpu_threads_per_core:         <computed>
      ebs_block_device.#:           <computed>
      ephemeral_block_device.#:     <computed>
      get_password_data:            "false"
      host_id:                      <computed>
      instance_state:               <computed>
      instance_type:                "t2.micro"
      ipv6_address_count:           <computed>
      ipv6_addresses.#:             <computed>
      key_name:                     <computed>
      network_interface.#:          <computed>
      network_interface_id:         <computed>
      password_data:                <computed>
      placement_group:              <computed>
      primary_network_interface_id: <computed>
      private_dns:                  <computed>
      private_ip:                   <computed>
      public_dns:                   <computed>
      public_ip:                    <computed>
      root_block_device.#:          <computed>
      security_groups.#:            <computed>
      source_dest_check:            "true"
      subnet_id:                    <computed>
      tags.%:                       "2"
      tags.Ambiente:                "Desenvolvimento"
      tags.Name:                    "Ubuntu-1"
      tenancy:                      <computed>
      volume_tags.%:                <computed>
      vpc_security_group_ids.#:     <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**T**udo parece certo no plano, portanto o próximo passo será aplicarmos este código:

``` bash
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + aws_instance.server1
      id:                           <computed>
      ami:                          "ami-0a313d6098716f372"
      arn:                          <computed>
      associate_public_ip_address:  <computed>
      availability_zone:            <computed>
      cpu_core_count:               <computed>
      cpu_threads_per_core:         <computed>
      ebs_block_device.#:           <computed>
      ephemeral_block_device.#:     <computed>
      get_password_data:            "false"
      host_id:                      <computed>
      instance_state:               <computed>
      instance_type:                "t2.micro"
      ipv6_address_count:           <computed>
      ipv6_addresses.#:             <computed>
      key_name:                     <computed>
      network_interface.#:          <computed>
      network_interface_id:         <computed>
      password_data:                <computed>
      placement_group:              <computed>
      primary_network_interface_id: <computed>
      private_dns:                  <computed>
      private_ip:                   <computed>
      public_dns:                   <computed>
      public_ip:                    <computed>
      root_block_device.#:          <computed>
      security_groups.#:            <computed>
      source_dest_check:            "true"
      subnet_id:                    <computed>
      tags.%:                       "2"
      tags.Ambiente:                "Desenvolvimento"
      tags.Name:                    "Ubuntu-1"
      tenancy:                      <computed>
      volume_tags.%:                <computed>
      vpc_security_group_ids.#:     <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.server1: Creating...
  ami:                          "" => "ami-0a313d6098716f372"
  arn:                          "" => "<computed>"
  associate_public_ip_address:  "" => "<computed>"
  availability_zone:            "" => "<computed>"
  cpu_core_count:               "" => "<computed>"
  cpu_threads_per_core:         "" => "<computed>"
  ebs_block_device.#:           "" => "<computed>"
  ephemeral_block_device.#:     "" => "<computed>"
  get_password_data:            "" => "false"
  host_id:                      "" => "<computed>"
  instance_state:               "" => "<computed>"
  instance_type:                "" => "t2.micro"
  ipv6_address_count:           "" => "<computed>"
  ipv6_addresses.#:             "" => "<computed>"
  key_name:                     "" => "<computed>"
  network_interface.#:          "" => "<computed>"
  network_interface_id:         "" => "<computed>"
  password_data:                "" => "<computed>"
  placement_group:              "" => "<computed>"
  primary_network_interface_id: "" => "<computed>"
  private_dns:                  "" => "<computed>"
  private_ip:                   "" => "<computed>"
  public_dns:                   "" => "<computed>"
  public_ip:                    "" => "<computed>"
  root_block_device.#:          "" => "<computed>"
  security_groups.#:            "" => "<computed>"
  source_dest_check:            "" => "true"
  subnet_id:                    "" => "<computed>"
  tags.%:                       "" => "2"
  tags.Ambiente:                "" => "Desenvolvimento"
  tags.Name:                    "" => "Ubuntu-1"
  tenancy:                      "" => "<computed>"
  volume_tags.%:                "" => "<computed>"
  vpc_security_group_ids.#:     "" => "<computed>"
aws_instance.server1: Still creating... (10s elapsed)
aws_instance.server1: Still creating... (20s elapsed)
aws_instance.server1: Still creating... (30s elapsed)
aws_instance.server1: Creation complete after 34s (ID: i-0cc1dcdbee6a81644)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

**Ó**timo. Nenhum erro e um *resource* criado. Se atualizarmos a lista de instâncias em nossa interface do AWS, deveremos ver nossa nova instância Ubuntu criada.

![AWS IAM](/img/terraform_aws10.jpg)

**T**udo certo. Nossa instância foi criada e está disponível para acesso.

**M**as tudo ainda está muito simples e desorganizado. Vamos destruir a nossa infraestrutura para melhorarmos um pouco. Utilizaremos *terraform destroy* para destruir tudo o que foi criado:

``` bash
$ terraform destroy

aws_instance.server1: Refreshing state... (ID: i-0cc1dcdbee6a81644)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - aws_instance.server1


Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_instance.server1: Destroying... (ID: i-0cc1dcdbee6a81644)
aws_instance.server1: Still destroying... (ID: i-0cc1dcdbee6a81644, 10s elapsed)
aws_instance.server1: Still destroying... (ID: i-0cc1dcdbee6a81644, 20s elapsed)
aws_instance.server1: Destruction complete after 21s

Destroy complete! Resources: 1 destroyed.
```

**A**ntes de mais nada, vamos organizar o código e criar algumas variáveis.

**C**omecemos criando um arquivo chamado *variables.tf* no mesmo diretório onde se encontra o arquivo main.tf e insira as seguintes variáveis nele:

{{< highlight py "linenos=table,hl_lines=,linenostart=0" >}}
variable "region" {
  type        = "string"
  description = "Região no AWS onde nossos recursos estarão."
  default     = "us-east-1"
}

variable "ami" {
  type        = "string"
  description = "Id da imagem EC2 que desejamos utilizar para nossa instância."
  default     = "ami-0a313d6098716f372"
}

variable "instance_type" {
  type        = "string"
  description = "Tipo de instância a ser utilizado"
  default     = "t2.micro"
}

variable "tags" {
  type        = "map"
  description = "Tags a serem aplicadas à nossa instância."

  default = {
    "Name"     = "Ubuntu-1"
    "Ambiente" = "Desenvolvimento"
  }
}
{{< / highlight >}}

**A**gora que temos nosso arquivo de variáveis, vamos limpar nosso arquivo *main.tf* para que ele faça uso destas variáveis, eliminando assim valores codificados diretamente em nosso arquivo principal. O arquivo *main.tf* deverá ficar assim:

{{< highlight py "linenos=table,hl_lines=2 6 7 9,linenostart=0" >}}
provider "aws" {
  region = "${var.region}"
}

resource "aws_instance" "server1" {
  ami           = "${var.ami}"
  instance_type = "${var.instance_type}"

  tags = "${var.tags}"
}
{{< / highlight >}}

**E**mbora tenhamos organizado melhor nosso código, nada de fato mudou no resultado.

**S**inta-se livre para rodar *terraform plan* e *terraform apply* em seguida para constatar que ele apenas criará a mesma instância novamente, caso você tenha destruído a anterior com *terraform destroy*, claro.

**A**pós testar, lembre-se de limpar sua infraestrutura novamente com *terraform destroy*, pois estamos apenas testando nosso código por enquanto.

**A**té então temos apenas o código que cria uma instância Ubuntu comum e vazia. Agora que temos este código pronto, a gerência da empresa na qual você trabalha lhe pede que crie um projeto chamado *Webserver*. Tudo o que este projeto precisa ter por enquanto é uma instância Ubuntu. Ela não precisa possuir de fato um servidor web instalado neste momento.

**V**ocê é esperto e já tinha o código pronto, apenas não possuia um projeto para utilizá-lo. Neste caso, vamos começar a organizar nossos projetos. O que temos até então é um diretório qualquer, que no meu caso se chama *terraform*, e dentro dele temos dois arquivos principais:

-   main.tf
-   variables.tf

**A**lém deles, o Terraform terá criado alguns arquivos de *state* onde ele armazena as informações sobre o estado de sua infraestrutura, bem como um diretório oculto *.terraform*.

**V**amos criar um diretório para nosso novo projeto. O novo diretório deverá se chamar *webserver*. Este deverá ser criado dentro do seu diretório original. Além disso, agora podemos mover nossos arquivos para dentro de nosso diretório de projeto. Sua estrutura de diretórios e arquivos agora deverá ser similar a esta: (Os arquivos realmente importantes são *main.tf* e *variables.tf*, visto que como destruimos nossa infraestrutura por completo, podemos facilmente ter novos arquivos de state.)

``` bash
terraform
└── webserver
    ├── main.tf
    ├── terraform.tfstate
    ├── terraform.tfstate.backup
    └── variables.tf
```

**I**magine que além do projeto webserver, agora você precisará se preparar para o projeto que rodará um banco de dados em uma instância também Ubuntu. Por enquanto tudo o que será preciso fazer é criar um diretório para o projeto *db* e criar o código que criará uma instância Ubuntu.

**C**omo já temos o código que faz isso, podemos simplesmente copiar os mesmos dois arquivos *main.tf* e *variables.tf* para dentro de um novo diretório chamado *db* e alterar alguns valores neles, certo?!

**S**endo assim, nossa árvore de diretórios agora estará assim:

``` bash
terraform
└── db
    ├── main.tf
    ├── variables.tf
└── webserver
    ├── main.tf
    ├── terraform.tfstate
    ├── terraform.tfstate.backup
    └── variables.tf
```

**C**omo já tínhamos isolado nossas variáveis, não precisaremos tocar nos arquivos *main.tf*. Podemos alterar apenas valores das variáveis desejadas nos respectivos arquivos *variables.tf*.

**C**omeçando pelo *variables.tf* do projeto **webserver**, alteraremos apenas a variável de tags, que agora ficará assim:

{{< highlight py "linenos=table,hl_lines=24-26,linenostart=0" >}}
variable "region" {
  type        = "string"
  description = "Região no AWS onde nossos recursos estarão."
  default     = "us-east-1"
}

variable "ami" {
  type        = "string"
  description = "Id da imagem EC2 que desejamos utilizar para nossa instância."
  default     = "ami-0a313d6098716f372"
}

variable "instance_type" {
  type        = "string"
  description = "Tipo de instância a ser utilizado"
  default     = "t2.micro"
}

variable "tags" {
  type        = "map"
  description = "Tags a serem aplicadas à nossa instância."

  default = {
    "Name"     = "Ubuntu-webserver"
    "Ambiente" = "Desenvolvimento"
    "Projeto"  = "Webserver"
  }
}
{{< / highlight >}}

**A**lteramos apenas o nome da instância para *Ubuntu-webserver* e adicionamos uma tag para o *projeto*.

**A**gora faremos algo similar no arquivo *variables.tf* do projeto **db**, ficando assim:

{{< highlight py "linenos=table,hl_lines=24-26,linenostart=0" >}}
variable "region" {
  type        = "string"
  description = "Região no AWS onde nossos recursos estarão."
  default     = "us-east-1"
}

variable "ami" {
  type        = "string"
  description = "Id da imagem EC2 que desejamos utilizar para nossa instância."
  default     = "ami-0a313d6098716f372"
}

variable "instance_type" {
  type        = "string"
  description = "Tipo de instância a ser utilizado"
  default     = "t2.micro"
}

variable "tags" {
  type        = "map"
  description = "Tags a serem aplicadas à nossa instância."

  default = {
    "Name"     = "Ubuntu-db"
    "Ambiente" = "Desenvolvimento"
    "Projeto"  = "Db"
  }
}
{{< / highlight >}}

**N**este momento não temos nenhuma instância rodando, pois exluímos a que criamos anteriormente com *terraform destroy*.

**V**amos validar nosso código agora. Primeiramente, vamos executar os seguintes comandos de dentro do diretório *db*:

``` bash
$ terraform init .

$ terraform plan

$ terraform apply
```

**E**m seguida, de dentro do diretório *webserver* execute também os comandos:

``` bash
$ terraform plan

$ terraform apply
```

**O** motivo pelo qual não precisamos executar *terraform init .* para o projeto *webserver* é por já termos feito isso anteriormente, portanto já devemos ter os arquivos de configuração e state do Terraform dentro de nosso diretório webserver. Caso contrário, execute *terraform init .* antes do *plan*.

**S**e verificarmos nossas instâncias no AWS agora, devemos ter as duas instâncias: *Ubuntu-webserver* e *Ubuntu-db*.

![AWS IAM](/img/terraform_aws11.jpg)

**J**á vimos que nossas duas instâncias podem ser criadas com nosso código mas até então elas não fazem nada além de simplesmente existir.

**V**amos destruir tudo o que foi criado para não gastarmos nossos créditos AWS de forma desnecessária. Execute *terraform destroy* a partir de ambos os diretórios, *db* e *webserver* para garantir que ambas as instâncias serão destruídas.

**N**a PARTE 2 deste tutorial criaremos então o código que defato definirá e criará um servidor web e um servidor de banco de dados rapidamente.
