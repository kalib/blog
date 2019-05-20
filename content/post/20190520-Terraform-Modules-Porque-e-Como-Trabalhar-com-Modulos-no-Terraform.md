---
title: "20190520 Terraform Modules Porque E Como Trabalhar Com Modulos No Terraform - PARTE 2"
date: 2019-05-20T14:09:21-04:00
draft: true
image: "/img/"
tags: []
description: ""
---
**O** AWS possui um recurso chamado *User Data*, o qual nos permite passar um script (bash) que deverá ser executado no momento de criação de uma instância. Utilizaremos este recurso do AWS tmabém através de nosso código Terraform para:

-   Atualizar nossa base de repositórios do APT;
-   Instalar e iniciar os serviços necessários;
-   Liberar o serviço Apache no firewall do Ubuntu.

**C**aso suas instâncias ainda estejam rodando, você perceberá que o Terraform tentará destruir a inst6ancia e criá-la novamente após fazermos esta alteração. Isto se dá porque uma das alterações que faremos de fato exigirá que uma nova instância seja criada, pois incluiremos um script que deve rodar durante a criação da instância e, obviamente, a única forma de este script ser executado é justamente criando a instância novamente. Mas, isso não é problema, certo? Somos engenheiros devops e em um mundo devops o objetivo é ter uma infraestrutura automatizada e extremamente descartável, certo?! É por isso que codificamos tudo.

**C**omeçaremos com nosso servidor *webserver*.

**V**amos incluir uma nova tag *(Projeto)* bem como um bash script que será utilizado como *user_data*. Este bash script atualizará nossa base de repositórios e instalará o servidor web apache2, bem como habilitará o webserver no firewall padrão do Ubuntu.

**N**osso arquivo *variables.tf* do projeto *webserver* agora deverá estar assim:

{{< highlight py "linenos=table,hl_lines=30-42,linenostart=0" >}}
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

variable "user_data" {
  type        = "string"
  description = "Script a ser executado durante a criação da instância."

  default = <<-EOF
              #!/bin/bash
              sudo apt update
              sudo apt install apache2 -y
              sudo systemctl enable apache2
              sudo systemctl start apache2
              sudo ufw allow 'Apache'
              EOF
}
{{< / highlight >}}

**C**om estas alterações já conseguiríamos criar um servidor web com Apache2, no entanto por padrão o AWS mantém as portas de novas instâncias fechadas para o mundo externo, portanto precisamos também criar alguns *resources* para que nossa instância seja de fato utilizável:

-   VPC: Virtual Private Cloud é o recurso utilizado pelo AWS para definições de redes dentro de sua conta AWS;
-   Subnet: Definiremos uma subnet ou sub-rede para nossa infraestrutura;
-   Security Groups: Security Groups ou Grupos de Segurança é como o AWS chama suas definições e regras de firewall.

**V**amos inserir este *resource* em nosso arquivo *main.tf* do projeto *webserver*:

{{< highlight py "linenos=table,hl_lines=5-14,linenostart=0" >}}
provider "aws" {
  region = "${var.region}"
}

resource "aws_security_group" "webserver" {
  name = "firewall_webserver"

  ingress {
    from_port   = "${var.from_port}"
    to_port     = "${var.to_port}"
    protocol    = "tcp"
    cidr_blocks = "${var.ip_range}"
  }
}

resource "aws_instance" "server1" {
  ami           = "${var.ami}"
  instance_type = "${var.instance_type}"

  user_data = "${var.user_data}"

  tags = "${var.tags}"
}
{{< / highlight >}}

**A**qui apenas dissemos que desejamos criar uma regra de firewall para nosso servidor web e informamos que os valores para portas de entrada, portas de destino e IPs de origem serão definidos em nosso arquivo de variáveis.

**A**gora vamos incluir nossas novas variáveis no arquivo *variables.tf* de nosso webserver:

{{< highlight py "linenos=table,hl_lines=44-60,linenostart=0" >}}
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

variable "user_data" {
  type        = "string"
  description = "Script a ser executado durante a criação da instância."

  default = <<-EOF
              #!/bin/bash
              sudo apt update
              sudo apt install apache2 -y
              sudo systemctl enable apache2
              sudo systemctl start apache2
              sudo ufw allow 'Apache'
              EOF
}

variable "from_port" {
  type        = "string"
  description = "Origin port to receive connections from."
  default     = 80
}

variable "to_port" {
  type        = "string"
  description = "Destination port to pass connections to."
  default     = 80
}

variable "ip_range" {
  type        = "list"
  description = "Ip range that will be able to connect to this server."
  default     = ["0.0.0.0/0"]
}
{{< / highlight >}}

**A**qui indicamos que estaremos aceitando conexões vindas pela porta 80, para a porta também 80. Estamos também informando que conexões de qualquer endereço IP serão aceitas.

**I**sto já é o suficiente para criarmos novamente nosso servidor web, desta vez funcional. Execute *terraform plan*.

**S**e suas instâncias ainda estavam rodando, o Terraform irá apenas destruir a instância de nosso *webserver* para criar uma nova aplicando todas as nossas mudanças. Caso você tenha destruído a mesma com *terraform destroy* antes, uma nova será criada da mesma forma. Lembre-se de executar o comando *terraform plan* a partir do diretório onde se encontra nosso projeto *webserver*:

``` bash
$ terraform plan

Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

aws_instance.server1: Refreshing state... (ID: i-01f4d3d13209ac4bf)

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create
-/+ destroy and then create replacement

Terraform will perform the following actions:

-/+ aws_instance.server1 (new resource required)
      id:                                    "i-01f4d3d13209ac4bf" => <computed> (forces new resource)
      ami:                                   "ami-0a313d6098716f372" => "ami-0a313d6098716f372"
      arn:                                   "arn:aws:ec2:us-east-1:562554051836:instance/i-01f4d3d13209ac4bf" => <computed>
      associate_public_ip_address:           "true" => <computed>
      availability_zone:                     "us-east-1b" => <computed>
      cpu_core_count:                        "1" => <computed>
      cpu_threads_per_core:                  "1" => <computed>
      ebs_block_device.#:                    "0" => <computed>
      ephemeral_block_device.#:              "0" => <computed>
      get_password_data:                     "false" => "false"
      host_id:                               "" => <computed>
      instance_state:                        "running" => <computed>
      instance_type:                         "t2.micro" => "t2.micro"
      ipv6_address_count:                    "0" => <computed>
      ipv6_addresses.#:                      "0" => <computed>
      key_name:                              "" => <computed>
      network_interface.#:                   "0" => <computed>
      network_interface_id:                  "" => <computed>
      password_data:                         "" => <computed>
      placement_group:                       "" => <computed>
      primary_network_interface_id:          "eni-026097374ffdfc20c" => <computed>
      private_dns:                           "ip-172-31-93-122.ec2.internal" => <computed>
      private_ip:                            "172.31.93.122" => <computed>
      public_dns:                            "ec2-18-212-168-172.compute-1.amazonaws.com" => <computed>
      public_ip:                             "18.212.168.172" => <computed>
      root_block_device.#:                   "1" => <computed>
      security_groups.#:                     "1" => <computed>
      source_dest_check:                     "true" => "true"
      subnet_id:                             "subnet-18430a36" => <computed>
      tags.%:                                "3" => "3"
      tags.Ambiente:                         "Desenvolvimento" => "Desenvolvimento"
      tags.Name:                             "Ubuntu-webserver" => "Ubuntu-webserver"
      tags.Projeto:                          "Webserver" => "Webserver"
      tenancy:                               "default" => <computed>
      user_data:                             "26030426c9b448ddfdaba9ab3af63e211a0f5d65" => "667f17a65f03728b9c8f8c29166f86915a180f0b" (forces new resource)
      volume_tags.%:                         "0" => <computed>
      vpc_security_group_ids.#:              "1" => <computed>

  + aws_security_group.webserver
      id:                                    <computed>
      arn:                                   <computed>
      description:                           "Managed by Terraform"
      egress.#:                              <computed>
      ingress.#:                             "1"
      ingress.2214680975.cidr_blocks.#:      "1"
      ingress.2214680975.cidr_blocks.0:      "0.0.0.0/0"
      ingress.2214680975.description:        ""
      ingress.2214680975.from_port:          "80"
      ingress.2214680975.ipv6_cidr_blocks.#: "0"
      ingress.2214680975.prefix_list_ids.#:  "0"
      ingress.2214680975.protocol:           "tcp"
      ingress.2214680975.security_groups.#:  "0"
      ingress.2214680975.self:               "false"
      ingress.2214680975.to_port:            "80"
      name:                                  "firewall_webserver"
      owner_id:                              <computed>
      revoke_rules_on_delete:                "false"
      vpc_id:                                <computed>


Plan: 2 to add, 0 to change, 1 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**T**udo parece correto. Vejo a instância que será criada novamente, bem como meu novo *security_group*, abrindo a porta 80 para qualquer range de IP (0.0.0.0/0). Agora vamos executar *terraform apply*:

``` bash
$ terraform apply
(...)

-/+ aws_instance.server1 (new resource required)
      id:                                    "i-01f4d3d13209ac4bf" => <computed> (forces new resource)
      ami:                                   "ami-0a313d6098716f372" => "ami-0a313d6098716f372"
      arn:                                   "arn:aws:ec2:us-east-1:562554051836:instance/i-01f4d3d13209ac4bf" => <computed>
      associate_public_ip_address:           "true" => <computed>
      availability_zone:                     "us-east-1b" => <computed>
      cpu_core_count:                        "1" => <computed>
      cpu_threads_per_core:                  "1" => <computed>
      ebs_block_device.#:                    "0" => <computed>
      ephemeral_block_device.#:              "0" => <computed>
      get_password_data:                     "false" => "false"
      host_id:                               "" => <computed>
      instance_state:                        "running" => <computed>
      instance_type:                         "t2.micro" => "t2.micro"
      ipv6_address_count:                    "0" => <computed>
      ipv6_addresses.#:                      "0" => <computed>
      key_name:                              "" => <computed>
      network_interface.#:                   "0" => <computed>
      network_interface_id:                  "" => <computed>
      password_data:                         "" => <computed>
      placement_group:                       "" => <computed>
      primary_network_interface_id:          "eni-026097374ffdfc20c" => <computed>
      private_dns:                           "ip-172-31-93-122.ec2.internal" => <computed>
      private_ip:                            "172.31.93.122" => <computed>
      public_dns:                            "ec2-18-212-168-172.compute-1.amazonaws.com" => <computed>
      public_ip:                             "18.212.168.172" => <computed>
      root_block_device.#:                   "1" => <computed>
      security_groups.#:                     "1" => <computed>
      source_dest_check:                     "true" => "true"
      subnet_id:                             "subnet-18430a36" => <computed>
      tags.%:                                "3" => "3"
      tags.Ambiente:                         "Desenvolvimento" => "Desenvolvimento"
      tags.Name:                             "Ubuntu-webserver" => "Ubuntu-webserver"
      tags.Projeto:                          "Webserver" => "Webserver"
      tenancy:                               "default" => <computed>
      user_data:                             "26030426c9b448ddfdaba9ab3af63e211a0f5d65" => "667f17a65f03728b9c8f8c29166f86915a180f0b" (forces new resource)
      volume_tags.%:                         "0" => <computed>
      vpc_security_group_ids.#:              "1" => <computed>

  + aws_security_group.webserver
      id:                                    <computed>
      arn:                                   <computed>
      description:                           "Managed by Terraform"
      egress.#:                              <computed>
      ingress.#:                             "1"
      ingress.2214680975.cidr_blocks.#:      "1"
      ingress.2214680975.cidr_blocks.0:      "0.0.0.0/0"
      ingress.2214680975.description:        ""
      ingress.2214680975.from_port:          "80"
      ingress.2214680975.ipv6_cidr_blocks.#: "0"
      ingress.2214680975.prefix_list_ids.#:  "0"
      ingress.2214680975.protocol:           "tcp"
      ingress.2214680975.security_groups.#:  "0"
      ingress.2214680975.self:               "false"
      ingress.2214680975.to_port:            "80"
      name:                                  "firewall_webserver"
      owner_id:                              <computed>
      revoke_rules_on_delete:                "false"
      vpc_id:                                <computed>


Plan: 2 to add, 0 to change, 1 to destroy.
(...)
aws_instance.server1: Still creating... (10s elapsed)
aws_instance.server1: Still creating... (20s elapsed)
aws_instance.server1: Still creating... (30s elapsed)
aws_instance.server1: Creation complete after 33s (ID: i-0cad576c63c98349c)

Apply complete! Resources: 2 added, 0 changed, 1 destroyed.
```

**A**parentemente tudo está correto.

**P**rimeiramente, verificarei se meu *Security Group* foi devidamente criado para permitir as conexões na porta 80. PAra isto, clique em Grupos de Segurança *(Security Groups)* no meno lateral esquerdo do menu EC2 no AWS:

![AWS IAM](/img/terraform_aws12.jpg)

**N**osso *Security Group* foi criado corretamente conforme previsto. Agora podemos verificar se nossa instância foi criada, voltando para o menu de Instâncias. Ao identificar sua nova instância, selecione a caixa no canto esquerdo da mesma para que possamos ver as informações detalhadas desta instância, visto que precisaremos do DNS público para testarmos nosso webserver:

![AWS IAM](/img/terraform_aws13.jpg)

**C**opie este endereço de DNS Público e teste seu novo servidor web. Você pode fazê-lo através de um terminal ou console executando o comando *curl <seu endereço DNS público>* ou através de um navegador de internet qualquer.
