---
title: "Terraform: Criando uma infraestrutura no Google Cloud"
date: 2018-11-24T21:04:44-05:00
draft: false
image: "img/terraform_cloud.jpeg"
tags: [
"Devops",
"Terraform",
"Cloud",
"GCP",
"Docker",
"Kubernetes",
]
description: "Uma introdução ao Terraform, da Hashicorp, como robusta e eficiente ferramenta para infraestrutura como código. Criando infraestrutura na nuvem GCP, do Google."
---
**Q**uando se fala em infraestrutura como código imagina-se algo mais complexo do que simplesmente um container docker rodando uma aplicação, certo?! O propósito deste post é justamente criar uma infraestrutura um pouco mais complexa e completa no GCP, *Google Cloud Platform*. Embora o Terraform possua integração com diversos provedores de computação em nuvem, utilizarei o Google Cloud para este post por estar trabalhando mais com GCP atualmente e estar gostando da experiência.

**U**ma vez que não entrarei em tantos detalhes básicos do Terraform neste post, espero que você já possua algum conhecimento básico sobre o mesmo bem como entenda como funcionam seus *resources*, *variables*, etc. Do contrário, recomendo **fortemente** que você volte um pouco e leia meus posts anteriores nesta respectiva ordem:

-   <a href="/infraestrutura-como-c%C3%B3digo-com-terraform/" target="_blank">Infraestrutura como Código com Terraform</a>
-   <a href="/introdu%C3%A7%C3%A3o-ao-terraform/" target="_blank">Introdução ao Terraform</a>
-   <a href="/introdu%C3%A7%C3%A3o-ao-terraform/" target="_blank">Terraform: Variáveis e Outputs</a>

**A**ssumindo que você já possui algum conhecimento básico sobre Terraform, chegou a hora de inciarmos o nosso pequeno projeto de infraestrutura como código.

## GCP - Google Cloud Platform ##

![Google Cloud Platform](/img/gcloud.png)
**G**oogle Cloud Platform, ou GCP, é uma suíte de computação em nuvem oferecida pelo Google, funcionando na mesma infraestrutura que a empresa utiliza para seus produtos dirigidos aos usuários, dentre eles o buscador Google e o Youtube. Juntamente com um conjunto de ferramentas de gerenciamento modulares, o GCP fornece uma série de serviços incluindo computação, armazenamento de dados, análise de dados, machine learning, containers, etc.

**N**ovamente, o motivo pelo qual optei por utilizar GCP para este pequeno projeto foi simplesmente o fato de eu estar trabalhando mais com GCP em meu dia a dia atual, mas nada impede que você utilize AWS ou Microsoft Azure, por exemplo. Embora a sintaxe e o código Terraform deverá ser ajustado para tais plataformas caso decida utilizá-las.

**O**utro motivo interessante é o plano gratuito oferecido pelo Google, o que facilita nossos estudos e experimentos. O GCP nos oferece 1 ano de utilização grátis OU $300 dólares em créditos, o que ocorrer primeiro. De uma forma ou de outra, isto será muito mais que o suficiente para a execução deste nosso projeto.

**A**lém das duas razões já citadas, a integração e facilidade de criação de uma nova conta no GCP foi levada em conta para esta escolha. O fato de o gmail e demais serviços do Google serem amplamente utilizados, é bem provável que você possua um email do Google (gmail, por exemplo), certo!? Se este é o caso, você já possui uma "pré-conta" no GCP sem ao menos saber.

### Cadastro ###

**S**e você possui uma conta do Google, autentique-se com a mesma. Caso não possua uma, você poderá criar uma através do Gmail, por exemplo, ou criar diretamente na interface do Google Cloud durante o cadastro.

1-   Uma vez logado com sua conta do Google, acesse em seu navegador o seguinte endereço: <a href="https://cloud.google.com" target="_blank">Google Cloud Console</a> ;

2-   Caso esteja logado com sua conta do Google, verá sua foto ou imagem de conta no canto superior direito da página conforme na imagem a seguir. Clique em *Try free* ou Experimente Gratuitamente;
![Google Cloud Platform](/img/gcloud1.png)
3-   Você cairá na primeira página do cadastro. Informe seu País, leia e concorde com os termos de uso caso deseje seguir e em seguida clique em Concordar e Continuar;
![Google Cloud Platform](/img/gcloud2.png)
4-   Após confirmar e validar todas as informações que eles pedem na etapa 2, clique em Iniciar minha avaliação gratuita;
![Google Cloud Platform](/img/gcloud3.png)
**E**m poucos segundos você deverá cair no painel ou *dashboard* principal do GCP, com um popup de boas vindas com alguma mensagem de boas vindas: "Olá, Marcelo, Agradecemos por você se inscrever na avaliação gratuita de 12 meses. Demos US$ 300 de crédito grátis para você gastar. Não se preocupe se o crédito acabar, você só receberá cobranças se tivermos sua permissão."

**E**ste é outro fator interessante do GCP. Durante este período de avaliação, você não corre o risco de ser cobrado caso utilize mais do que deveria por descuido. Uma vez que o período de 12 meses, ou o crédito de $300 tenha se esgotado, você será notificado e deverá encerrar sua conta ou confirmar que deseja continuar utilizando os serviços, autorizando assim o Google a lhe efetuar cobranças a partir deste momento.

**O** painel principal se parecerá com este:
![Google Cloud Platform](/img/gcloud4.png)
**C**onforme a imagem abaixo, através do Menu principal que se encontra na lateral esquerda, clique em **IAM e Admin**, e em seguida em **Gerenciar recursos**.
![Google Cloud Platform](/img/gcloud5.png)
**V**ocê receberá uma listagem inicial de seus recursos. Por padrão, quando se cria uma nova conta no GCP, apenas um projeto inicial é criado, chamado My First Project, sem qualquer recurso vinculado ou inserido no mesmo.

### Criando uma conta de serviço ou service account ###

Embora seja possível e permitido utilizar-se de uma conta pessoal para este tipo de tarefa, não é o mais indicado. O ideal é deixar serviços utilizarem contas específicas, as chamadas service accounts. Uma vez que utilizaremos o Terraform para criar nossos recursos na nuvem, ele não deixa de funcionar como um serviço, não sendo uma pessoa XYZ de um departamento qualquer de uma empresa.

1-   No menu principal da lateral esquerda, clique em **IAM e Admin**, em seguida em **Contas de serviço**, conforme na imagem abaixo:
![Google Cloud Platform](/img/gcloud7.png)
2-   Na tela de Contas de serviço, clique em **Criar conta de serviço**;

3-   Indique o nome **terraform** para esta conta, conforme imagem abaixo, e clique em **CRIAR**;
![Google Cloud Platform](/img/gcloud8.png)
4-   No passo 2, daremos um papel para esta conta de serviço. Aqui indicaremos quais permissões ela terá. Para facilitar nosso exercício, utilizaremos **Projeto** > **Proprietário**, significando que nossa conta de serviço terá todas as permissões em nosso projeto, podendo criar ou destruir qualquer tipo de recurso.
![Google Cloud Platform](/img/gcloud9.png)
5-   Em seguida, clicaremos em **Continuar** para seguir com a criação de nossa conta de serviço;

6-   Na tela seguinte lhe será dada a opção de dar permissões para algum usuário que possa precisar utilizar-se desta conta de serviço para criar recursos. Você pode inserir neste campo o seu usuário principal do google cloud, o qual será o seu email que foi utilizado para criar esta conta no GCP. Insira-o em *Papel de usuários da conta de serviço*. Em seguida, clique no botão de *Criar Chave* que se encontra logo abaixo. Esta será a chave criptografada que utilizaremos para comunicação segura com o GCP;
![Google Cloud Platform](/img/gcloud10.png)
7-   Lhe será perguntado em que formato você deseja slavar a chave. Escolha o formato JSON e clique em **CRIAR**;

8-   Escolha com atenção o local onde salvará esta chave, pois você não poderá baixá-la novamente e precisaremos da mesma posteriormente;

**OBS:** Para facilitar este exemplo, estarei salvando a chave no mesmo diretório no qual criaremos o código de nosso projeto. Caso você decida fazer o mesmo, e decida hospedar este código em alguma espécie de repositório git, por exemplo, lembre-se de **sempre** incluir esta e outras chaves ou dados sigilosos em seu *.gitignore*, para que este tipo de arquivo com informações confidenciais não seja enviado para o repositório juntamente com o código. =) (Sim, já vi pessoas hospedando chaves em repositórios git e tendo sérios problemas. Sim, é você mesmo. :p)

### Google Cloud SDK ###

**U**m dos nossos pré-requisitos será o Google Cloud SDK, que possui um conjunto de ferramentas via linha de comando que nos permitem interagir com o Google Cloud remotamente através de APIs. O Terraform também fará uso desta ferramenta.

**P**ara o funcionamento do gcloud SDK precisaremos também possuir o Python instalado, na versão 2.7. Você poderá confirmar a sua versão do python executando *python -V* em algum console ou terminal.

**I**nformações detalhadas sobre como instalar o Gcloud SDK encontram-se com excelentes detalhes nas páginas oficiais do Google, listadas abaixo:

-   Para Linux (genérico): <a href="https://cloud.google.com/sdk/docs/quickstart-linux" target="_blank">Aqui</a>

-   Para Linux (Debian e Ubuntu): <a href="https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu" target="_blank">Aqui</a>

-   Para Linux (Red Hat e CentOS): <a href="https://cloud.google.com/sdk/docs/quickstart-redhat-centos" target=_blank">Aqui</a>

-   Para Mac OS X: <a href="https://cloud.google.com/sdk/docs/quickstart-macos" target="_blank">Aqui</a>

-   Para Windows: <a href="https://cloud.google.com/sdk/docs/quickstart-windows" target="_blank">Aqui</a>

**É** importante lembrar de reiniciar o seu terminal ou console após a instalação.

**P**ara certificar-se de que a instalação foi realizada com sucesso, execute:

``` yaml
$ gcloud --version

Google Cloud SDK 225.0.0
bq 2.0.37
core 2018.11.09
gsutil 4.34
```

**S**e você recebeu informações referentes à versão do Google Cloud SDK, significa que a instalação foi bem sucedida. O próximo passo é autenticar-se com sua conta do google através do SDK. Execute *gcloud init*. O seu navegador deverá abrir automaticamente lhe pedindo a autenticação de sua conta do Google após você confirmar com um *Y* a solicitação no console ou terminal.

``` yaml
$ gcloud init

Welcome! This command will take you through the configuration of gcloud.

Your current configuration has been set to: [default]

You can skip diagnostics next time by using the following flag:
  gcloud init --skip-diagnostics

Network diagnostic detects and fixes local network connection issues.
Checking network connection...done.
Reachability Check passed.
Network diagnostic passed (1/1 checks passed).

You must log in to continue. Would you like to log in (Y/n)?
```

**C**aso ao inserir um *Y*, o seu navegador não lhe solicite a autenticação do Google, copie e cole a longa URL que lhe será apresentada.

**A** solicitação de autenticação será similar à esta:
![Google Cloud Platform](/img/gcloud6.png)
**A**o finalizar sua autenticação no navegador, você receberá mais uma pergunta em seu terminal ou console. O SDK lhe indicará o seu projeto e lhe perguntará se você quer utilizá-lo ou criar um novo. Vamos escolher a opção *1*, para utilizar o projeto que foi criado automaticamente e em seguida confirmar com um Enter:

``` yaml
Your browser has been opened to visit:

    https://accounts.google.com/o/oauth2/auth?reblablabalblablaba=http%3A%2F%2Flocalhost%3A8085%2F&prompt=select_account&response_typereblablabalblablabauid.apps.googleusercontent.com&scope=https%3A%2F%2Fwww.googleapis.com%reblablabalblablabaemreblablabalblablabaw.gooreblablabalblablaba%2Fauth%2Fclreblablabalblablaba%2Fwwwreblablabalblablabacomreblablabalblablabaappenreblablabalblablabattpsreblablabalblablabaww.reblablabalblablabaleapis.com%2Fauthreblablabalblablabattps%3A%2F%2Fwww.googleapis.com%2Fauthreblablabalblablaba&access_type=offline


You are logged in as: [marcelo@marceloemail.net].

Pick cloud project to use:
 [1] possible-sun-meuid
 [2] Create a new project
Please enter numeric choice or text value (must exactly match list
item):
```

**O** SDK finalizará o setup e lhe informará que o projeto está pronto para ser utilizado.

## Terraform ##
![Terraform](/img/terraform_badge.png)
**V**amos ao que interessa agora. Antes de iniciarmos nosso código, crie um diretório chamado *terraform-gcp*, ou algo de sua preferência. Dentro deste diretório apenas teremos por enquanto o arquivo JSON que baixamos do GCP com as credenciais de nossa conta de serviço.

**C**riaremos agora nosso primeiro arquivo terraform.

**C**omecemos criando nosso arquivo de variáveis, o qual por enquanto conterá apenas 2 variáveis para começarmos nosso código. Crie o arquivo *variables.tf* com o seguinte conteúdo:

``` py variables.tf
variable "project_id" {
  type    = "string"
  default = "possible-sun-83482736"
}

variable "regiao" {
  type = "string"
  default = "northamerica-northeast1"
}
```

**OBS:** Lembre-se de alterar o valor default da variável *project_id*. Eu inseri aqui o id do projeto que foi criado para mim pelo GCP automaticamente. O id do seu projeto deverá ser o mesmo fornecido a você pelo GCP.

**O**utra coisa importante é lembrar que estou assumindo que você já possui algum conhecimento básico sobre como o Terraform funciona, ou que leu meus posts anteriores sobre o assunto, de forma que não estarei aqui entrando em tantos detalhes explicativos sobre cada arquivo, conforme fiz nos anteriores.

**E**m nosso arquivo *variables.tf*, por enquanto, criamos apenas duas variáveis: *project_id* e *regiao*. Para quem já utilizou algum serviço de computação em nuvem, seja GCP, AWS, Azure, etc., isto pode soar familiar. Sempre que se deseja criar recursos na nuvem, devemos optar por alguma região disponível no provedor de escolha. Estou optando por utilizar *northamerica-northeast1* pelo fato de eu morar em Toronto, e esta ser a região mais próxima, mas sinta-se livre para optar por qualquer região disponível no GCP conforme <a href="https://cloud.google.com/compute/docs/regions-zones/" target="_blank">lista fornecida aqui</a>.

**E**m seguida criaremos nosso arquivo *main.tf* que, inicialmente, possuirá apenas o seguinte:

``` py main.tf
# Configura o projeto GCP
provider "google" {
  credentials = "${file("possible-sun-83482736-sabh45jhb2345ghv.json")}"
  project     = "${var.project_id}"
  region      = "${var.regiao}"
}
```

**A**qui estamos apenas informando ao Terraform que utilizaremos o *google* como provedor ou *provider*. Para nos conectarmos com o *provider* precisamos passar nossas credenciais e para tal estamos apontando o arquivo ou *file* que baixamos do GCP com as informações de nossa conta de serviço. Aqui estou passando apenas o nome do arquivo, pois o mesmo se encontra no mesmo diretório onde se encontra meu código.

**A**lém da credencial, estamos também informando qual o nome do projeto que utilizaremos no GCP bem como a região na qual estaremos trabalhando. Ambos os valores estão sendo trazidos do arquivo *variables.tf*. Aqui apenas invocamos as variáveis. (Novamente: Se esta invocação das variáveis lhe parece confusa, significa que não leu o post anterior, onde expliquei o básico sobre uso de variáveis no Terraform. Volte uma casa!)

**C**om ambos os arquivos criados, podemos iniciar a execução de nosso projeto. Neste momento, nosso código não fará nada além de permitir que o Terraform consiga se conectar ao GCP e validar que nossa conta e projeto de fato existem. Executemos *terraform init* para ver se está tudo certo:

``` yaml
$ terraform init

Initializing provider plugins...
- Checking for available provider plugins on https://releases.hashicorp.com...
- Downloading plugin for provider "google" (1.19.1)...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.google: version = "~> 1.19"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

**A**o que parece, o nosso *state* do Terraform foi iniciado com sucesso e tudo parece correto. Vamos tentar criar nosso plano de execução agora com *terraform plan*:

``` yaml
$ terraform plan

Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

No changes. Infrastructure is up-to-date.

This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.
```

**T**udo parece correto e, assim como dito anteriormente, o Terraform também nos informa que tudo está atualizado e que nenhuma ação precisa ser realizada, afinal não indicamos nada a ser criado, nenhum recurso. Apenas indicamos que utilizaremos um determinado projeto em uma determinada região, e tal projeto já existe no GCP.

**V**amos então iniciar a criação de nossa infraestrutura básica. Iniciemos criando uma VM simples.

**Q**uando falamos em VMs na nuvem, existem alguns atributos importantes que devemos levar em conta antes de criar a mesma:

-   Nome: Nossa VM precisa ter um nome;
-   Tipo de máquina: Quando se decide comprar um novo servidor para sua empresa, você terá diversas máquinas disponíveis a venda, algumas com mais memória, outras com menos, CPU, Disco, etc. Da mesma forma funcionam VMs em um ambiente de nuvem como o GCP. Precisamos determinar o tipo de VM que queremos;
-   Zona: Todo provedor na nuvem ou *cloud* possui diversas zonas ou regiões disponíveis espalhadas pelo globo, portanto devemos sempre informar onde queremos rodar nossos recursos;
-   Imagem: Assim como fazemos com um servidor físico, sempre devemos escolher uma imagem ou Sistema Operacional para ser instalado em nossa VM;

**U**ma vez que tenhamos iuma listagem básica de algumas variáveis importantes para nossa VM, vamos inserí-las em nosso arquivo *variables.tf*:

``` py variables.tf
variable "project_id" {
  type    = "string"
  default = "possible-sun-83482736"
}

variable "regiao" {
  type = "string"
  default = "northamerica-northeast1"
}

variable "nome" {
  type = "string"
  default = "vm-webserver"
}

variable "tipo_maquina" {
  type = "string"
  default = "f1-micro"
}

variable "zona" {
  type = "string"
  default = "northamerica-northeast1-a"
}

variable "imagem" {
  type = "string"
  default = "debian-cloud/debian-9"
}
```

**A**qui criamos 4 novas variáveis. Daremos um nome (vm-webserver) para nossa VM, um tipo de máquina (f1-micro), uma zona (em meu caso utilizarei northamerica-northamerica1-a por ser a mais próxima de mim, mas sinta-se livre para utilizar a que melhor lhe servir) e uma imagem (debian-cloud/debian-9, a qual faz parte da enorme lista de imagens disponíveis no Google).

Agora que temos nossas variáveis definidas, vamos editar o arquivo *main.tf* para criar nossos recursos para a VM:

``` py main.tf
# Configura o projeto GCP
provider "google" {
  credentials = "${file("possible-sun-83482736-sabh45jhb2345ghv.json")}"
  project     = "${var.project_id}"
  region      = "${var.regiao}"
}

# Cria a VM com o Google Compute Engine
resource "google_compute_instance" "webserver" {
  name          = "${var.nome}"
  machine_type  = "${var.tipo_maquina}"
  zone          = "${var.zona}"

  boot_disk {
    initialize_params {
      image = "${var.imagem}"
    }
  }

  # Instala o servidor web Apache
  metadata_startup_script = "sudo apt-get update; sudo apt-get install apache2 -y; echo Testando > /var/www/html/index.html"

  # Habilita rede para a VM bem como um IP público
  network_interface {
    network = "default"
    access_config {

    }
  }
}
```

**A**qui inserimos um *resource* de tipo *google_compute_instance* para criar nossa VM e nele passamos os detalhes de nossa VM, tais como nome, tipo de máquina, zona e rede.

**E**stamos também utilizando a propriedade *metadata_startup_script*, na qual o Terraform nos permite utilizar o recurso de script de inicialização do Google Cloud. Este parâmetro nos permite executar um script logo que a VM é criada, portanto podemos automatizar qualquer setup inicial de nossa VM através deste recurso. Neste exemplo estaremos apenas atualizando os repositórios de nosso Debian e instalando um servidor web Apache para nosso teste.

**A**gora que temos nossos arquivos *variables.tf* e *main.tf* atualizados, vamos executar novamente *terraform plan* para ver o que aconteceria com nossa infraestrutura:

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

 + google_compute_instance.webserver
     id:                                                  <computed>
     boot_disk.#:                                         "1"
     boot_disk.0.auto_delete:                             "true"
     boot_disk.0.device_name:                             <computed>
     boot_disk.0.disk_encryption_key_sha256:              <computed>
     boot_disk.0.initialize_params.#:                     "1"
     boot_disk.0.initialize_params.0.image:               "debian-cloud/debian-9"
     boot_disk.0.initialize_params.0.size:                <computed>
     boot_disk.0.initialize_params.0.type:                <computed>
     can_ip_forward:                                      "false"
     cpu_platform:                                        <computed>
     create_timeout:                                      "4"
     deletion_protection:                                 "false"
     guest_accelerator.#:                                 <computed>
     instance_id:                                         <computed>
     label_fingerprint:                                   <computed>
     machine_type:                                        "f1-micro"
     metadata_fingerprint:                                <computed>
     metadata_startup_script:                             "sudo apt-get update; sudo apt-get install apache2 -y; echo Testando > /var/www/html/index.html"
     name:                                                "vm-webserver"
     network_interface.#:                                 "1"
     network_interface.0.access_config.#:                 "1"
     network_interface.0.access_config.0.assigned_nat_ip: <computed>
     network_interface.0.access_config.0.nat_ip:          <computed>
     network_interface.0.access_config.0.network_tier:    <computed>
     network_interface.0.address:                         <computed>
     network_interface.0.name:                            <computed>
     network_interface.0.network:                         "default"
     network_interface.0.network_ip:                      <computed>
     network_interface.0.subnetwork_project:              <computed>
     project:                                             <computed>
     scheduling.#:                                        <computed>
     self_link:                                           <computed>
     tags_fingerprint:                                    <computed>
     zone:                                                "northamerica-northeast1-a"


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**A**parentemente tudo está conforme o esperado. O Terraform criará um recurso para nossa VM.

**A**ntes de aplicarmos nosso plano de fato, volte à sua *Dashboard* do GCP e, no menu principal do canto esquerdo navegue até o painel de Instâncias (VMs):
![Google Cloud Platform](/img/gcloud11.png)
**C**aso seja a sua primeira vez acessando esta seção, você deverá receber a informação de que o Google esta carregando a API de *Compute Engine* para seu uso. Este processo costuma levar cerca de 1 ou 2 minutos, mas apenas acontece na primeira vez que você acessa este painel. Para otimizar sua nuvem o Google não carrega todas as APIs por padrão, habilitando-as aos poucos conforme você as utiliza, bem como lhe permitindo habilitar apenas as que você deseja ou desabilitar as que não precisa.

**V**ocê provavelmente não terá nenhuma VM ou instância criada, portanto nada será listado para você além de uma tela informando que ali ficarão suas VMs, bem como um botão de **Criar** VMs, através do qual você poderia criar suas VMs e passar todas as suas definições pela interface. Mas o nosso objetivo é automatizar tudo isso e codificar nossa infraestrutura, certo?! Portanto, ignoremos isto por enquanto. O nosso objetivo nesta interface era apenas ver que não temos nossa vm webserver criada (ainda).

**D**e volta ao nosso console ou terminal, executemos *terraform apply*:

``` yaml
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + google_compute_instance.webserver
      id:                                                  <computed>
      boot_disk.#:                                         "1"
      boot_disk.0.auto_delete:                             "true"
      boot_disk.0.device_name:                             <computed>
      boot_disk.0.disk_encryption_key_sha256:              <computed>
      boot_disk.0.initialize_params.#:                     "1"
      boot_disk.0.initialize_params.0.image:               "debian-cloud/debian-9"
      boot_disk.0.initialize_params.0.size:                <computed>
      boot_disk.0.initialize_params.0.type:                <computed>
      can_ip_forward:                                      "false"
      cpu_platform:                                        <computed>
      create_timeout:                                      "4"
      deletion_protection:                                 "false"
      guest_accelerator.#:                                 <computed>
      instance_id:                                         <computed>
      label_fingerprint:                                   <computed>
      machine_type:                                        "f1-micro"
      metadata_fingerprint:                                <computed>
      metadata_startup_script:                             "sudo apt-get update; sudo apt-get install apache2 -y; echo Testando > /var/www/html/index.html"
      name:                                                "vm-webserver"
      network_interface.#:                                 "1"
      network_interface.0.access_config.#:                 "1"
      network_interface.0.access_config.0.assigned_nat_ip: <computed>
      network_interface.0.access_config.0.nat_ip:          <computed>
      network_interface.0.access_config.0.network_tier:    <computed>
      network_interface.0.address:                         <computed>
      network_interface.0.name:                            <computed>
      network_interface.0.network:                         "default"
      network_interface.0.network_ip:                      <computed>
      network_interface.0.subnetwork_project:              <computed>
      project:                                             <computed>
      scheduling.#:                                        <computed>
      self_link:                                           <computed>
      tags_fingerprint:                                    <computed>
      zone:                                                "northamerica-northeast1-a"


Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
```

**T**udo parece correto, inclusive podemos ver nosso script de metadata que deverá ser executado durante a criação da VM para instalar nosso servidor WEB Apache. Confirme com um *yes*:

``` yaml
Enter a value: yes

google_compute_instance.webserver: Creating...
boot_disk.#:                                         "" => "1"
boot_disk.0.auto_delete:                             "" => "true"
boot_disk.0.device_name:                             "" => "<computed>"
boot_disk.0.disk_encryption_key_sha256:              "" => "<computed>"
boot_disk.0.initialize_params.#:                     "" => "1"
boot_disk.0.initialize_params.0.image:               "" => "debian-cloud/debian-9"
boot_disk.0.initialize_params.0.size:                "" => "<computed>"
boot_disk.0.initialize_params.0.type:                "" => "<computed>"
can_ip_forward:                                      "" => "false"
cpu_platform:                                        "" => "<computed>"
create_timeout:                                      "" => "4"
deletion_protection:                                 "" => "false"
guest_accelerator.#:                                 "" => "<computed>"
instance_id:                                         "" => "<computed>"
label_fingerprint:                                   "" => "<computed>"
machine_type:                                        "" => "f1-micro"
metadata_fingerprint:                                "" => "<computed>"
metadata_startup_script:                             "" => "sudo apt-get update; sudo apt-get install apache2 -y; echo Testando > /var/www/html/index.html"
name:                                                "" => "vm-webserver"
network_interface.#:                                 "" => "1"
network_interface.0.access_config.#:                 "" => "1"
network_interface.0.access_config.0.assigned_nat_ip: "" => "<computed>"
network_interface.0.access_config.0.nat_ip:          "" => "<computed>"
network_interface.0.access_config.0.network_tier:    "" => "<computed>"
network_interface.0.address:                         "" => "<computed>"
network_interface.0.name:                            "" => "<computed>"
network_interface.0.network:                         "" => "default"
network_interface.0.network_ip:                      "" => "<computed>"
network_interface.0.subnetwork_project:              "" => "<computed>"
project:                                             "" => "<computed>"
scheduling.#:                                        "" => "<computed>"
self_link:                                           "" => "<computed>"
tags_fingerprint:                                    "" => "<computed>"
zone:                                                "" => "northamerica-northeast1-a"
google_compute_instance.webserver: Still creating... (10s elapsed)
google_compute_instance.webserver: Creation complete after 14s (ID: vm-webserver)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

**N**osso código parece ter sido executado com sucesso e o Terraform ao final nos confirma que um *resource* foi adicionado (o qual esperamos ser nossa VM).

Se atualizarmos nossa página de instâncias no GCP, devemos agora ver nossa *vm-webserver* criada e rodando:
![Google Cloud Platform](/img/gcloud12.png)
**A**gora que temos a certeza de que nosso código funciona para a criação de nossa VM, precisamos também fazer com que a mesma seja acessível, afinal a principal função de qualquer servidor web é justamente ser acessível para apresentar alguma aplicação ou site. Para isto precisamos habilitar o firewall de nossa VM ou instância. Além disso, precisamos saber qual o endereço IP desta instância. Embora seja possível e simples conseguir este endereço IP através da interface do GCP ao clicarmos em cima de nossa VM, podemos também automatizar isto e receber este valor diretamente através do Terraform. (Lembre-se: O principal objetivo por trás da ideia de infraestrutura como código é automatizar ao máximo, evitando o uso de interfaces ou  dashboards (odeio cliques) :p).

**V**amos começar criando um arquivo *outputs.tf* que será utilizado para nos informar o IP da VM criada pelo Terraform:

``` py outputs.tf
# Retorna o IP da VM criada
output "ip" {
  value = "${google_compute_instance.webserver.network_interface.0.access_config.0.nat_ip}"
}
```

**N**ão, eu não estou inventando comandos ou trazendo algo do além. Se você é curioso e atencioso deve ter percebido que aqui estamos apenas puxando informações que o GCP criou através do Terraform. Se você voltar e reparar em seu comando *terraform apply*, perceberá que um dos atributos listados/criados em sua VM foi justamente o *network_interface.0.access_config.0.nat_ip*:

``` yaml
...
network_interface.0.access_config.0.assigned_nat_ip: "" => "<computed>"
--> network_interface.0.access_config.0.nat_ip:          "" => "<computed>"
network_interface.0.access_config.0.network_tier:    "" => "<computed>"
network_interface.0.address:                         "" => "<computed>"
...
```

**P**ara validarmos que isto de fato funcionará, vamos executar novamente *terraform apply*:

``` yaml
$ terraform apply
google_compute_instance.webserver: Refreshing state... (ID: vm-webserver)

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

ip = 35.203.117.191
```

**C**onforme o esperado, nenhum *resource* precisou ser criado, pois não alteramos nada em nosso código indicando a necessidade de alguma mudança. A única coisa que aocnteceu de diferente foi que desta vez recebemos o IP de nossa instância ou VM.

**O**utra forma de recebermos este valor a qualquer momento é executando *terraform output ip*:

``` yaml
$ terraform output ip
35.203.117.191
```

**Ó**timo, já temos nossa instância e sabemos que podemos receber o endereço IP externo dela facilmente para acesso. Só nos resta agora abrir as portas de firewall necessárias para que possamos acessar nossa aplicação. Vamos abrir a porta 80 para nossa VM.

**V**amos criar as seguintes variáveis em nosso arquivo *variables.tf*:

-   nome_fw: precisamos dar um nome para nosso firewall;
-   portas: precisamos criar uma lista de portas que serão abertas neste firewall.

``` py variables.tf
variable "project_id" {
  type    = "string"
  default = "possible-sun-83482736"
}

variable "regiao" {
  type = "string"
  default = "northamerica-northeast1"
}

variable "nome" {
  type = "string"
  default = "vm-webserver"
}

variable "tipo_maquina" {
  type = "string"
  default = "f1-micro"
}

variable "zona" {
  type = "string"
  default = "northamerica-northeast1-a"
}

variable "imagem" {
  type = "string"
  default = "debian-cloud/debian-9"
}

variable "nome_fw" {
  type = "string"
  default = "webserver-firewall"
}

variable "portas" {
  type = "list"
  default = ["80"]
}
```

**A** única novidade aqui foi o tipo *list* que utilizamos para a variável *portas*. Uma vez que podemos querer abrir múltiplas portas, utilizaremos o tipo *list*, o qual nos permite ter vários valores, ou uma lista de valores, para esta variável.

**A**gora vamos criar o *resource* para nosso firewall no arquivo *main.tf*:

``` py main.tf
# Configura o projeto GCP
provider "google" {
  credentials = "${file("possible-sun-83482736-sabh45jhb2345ghv.json")}"
  project     = "${var.project_id}"
  region      = "${var.regiao}"
}

# Cria a VM com o Google Compute Engine
resource "google_compute_instance" "webserver" {
  name          = "${var.nome}"
  machine_type  = "${var.tipo_maquina}"
  zone          = "${var.zona}"

  boot_disk {
    initialize_params {
      image = "${var.imagem}"
    }
  }

  # Instala o servidor web Apache
  metadata_startup_script = "sudo apt-get update; sudo apt-get install apache2 -y; echo Testando > /var/www/html/index.html"

  # Habilita rede para a VM bem como um IP público
  network_interface {
    network = "default"
    access_config {

    }
  }
}

# Cria o Firewall para a VM
resource "google_compute_firewall" "webfirewall" {
  name        = "${var.nome_fw}"
  network     = "default"

  allow {
    protocol  = "tcp"
    ports     = "${var.portas}"
  }
}

```

**I**ncluímos apenas um novo *resource* para a criação de nosso firewall abrindo as portas que definimos em nosso arquivo *variables.tf* com o protocolo *tcp*. Execute novamente *terraform plan* para ver o que mudaria:

``` yaml
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

google_compute_instance.webserver: Refreshing state... (ID: vm-webserver)

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + google_compute_firewall.webfirewall
      id:                       <computed>
      allow.#:                  "1"
      allow.272637744.ports.#:  "1"
      allow.272637744.ports.0:  "80"
      allow.272637744.protocol: "tcp"
      creation_timestamp:       <computed>
      destination_ranges.#:     <computed>
      direction:                <computed>
      name:                     "webserver-firewall"
      network:                  "default"
      priority:                 "1000"
      project:                  <computed>
      self_link:                <computed>
      source_ranges.#:          <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**T**udo parece correto. Como a VM já existe e nada foi modificado no código da mesma, o Terraform apenas criará um  novo *resource* para nosso firewall com os valores que definimos. Execute seu plano com *terraform apply* e confirme com *yes*:

``` yaml
$ terraform apply
google_compute_instance.webserver: Refreshing state... (ID: vm-webserver)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + google_compute_firewall.webfirewall
      id:                       <computed>
      allow.#:                  "1"
      allow.272637744.ports.#:  "1"
      allow.272637744.ports.0:  "80"
      allow.272637744.protocol: "tcp"
      creation_timestamp:       <computed>
      destination_ranges.#:     <computed>
      direction:                <computed>
      name:                     "webserver-firewall"
      network:                  "default"
      priority:                 "1000"
      project:                  <computed>
      self_link:                <computed>
      source_ranges.#:          <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

google_compute_firewall.webfirewall: Creating...
  allow.#:                  "" => "1"
  allow.272637744.ports.#:  "" => "1"
  allow.272637744.ports.0:  "" => "80"
  allow.272637744.protocol: "" => "tcp"
  creation_timestamp:       "" => "<computed>"
  destination_ranges.#:     "" => "<computed>"
  direction:                "" => "<computed>"
  name:                     "" => "webserver-firewall"
  network:                  "" => "default"
  priority:                 "" => "1000"
  project:                  "" => "<computed>"
  self_link:                "" => "<computed>"
  source_ranges.#:          "" => "<computed>"
google_compute_firewall.webfirewall: Still creating... (10s elapsed)
google_compute_firewall.webfirewall: Creation complete after 14s (ID: webserver-firewall)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:

ip = 35.203.117.191
```

**T**udo parece ter funcionado. Um novo resource de *firewall* foi criado e, conforme visto anteriormente, também recebemos o nosso IP como output.

**C**onfirme que tudo funcionou como o esperado copiando este IP e colando-o em seu navegador:
![Google Cloud Platform](/img/gcloud13.png)
**S**e você recebeu uma página em branco com a palavra *Testando*, significa que tudo ocorreu conforme o esperado.

**A**té agora nossa infraestrutura possui:

-   1 VM como servidor Web básico rodando Apache e uma página de teste;
-   1 rede default;
-   1 firewall básico aplicado à nossa VM abrindo a porta 80.

**E**mbora esta seja uma infraestrutura extremamente simples, o Terraform lhe permite criar infraestruturas muito mais complexas e robustas.

**N**o próximo post incrementaremos este código para incluírmos a criação de um cluster Kubernetes em nossa *cloud*.

**P**or hora, para não consumir muito de nossos créditos no GCP, vamos destruir nossa infraestrutura com *terraform destroy*. É tão simples e rápido criar tudo novamente agora que temos o código pronto, certo?! Então destruir tudo não nos causará problemas.

``` yaml
$ terraform destroy
google_compute_firewall.webfirewall: Refreshing state... (ID: webserver-firewall)
google_compute_instance.webserver: Refreshing state... (ID: vm-webserver)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - google_compute_firewall.webfirewall

  - google_compute_instance.webserver


Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

google_compute_firewall.webfirewall: Destroying... (ID: webserver-firewall)
google_compute_instance.webserver: Destroying... (ID: vm-webserver)
google_compute_firewall.webfirewall: Still destroying... (ID: webserver-firewall, 10s elapsed)
google_compute_instance.webserver: Still destroying... (ID: vm-webserver, 10s elapsed)
google_compute_firewall.webfirewall: Destruction complete after 12s
google_compute_instance.webserver: Destruction complete after 14s

Destroy complete! Resources: 2 destroyed.
```

**T**udo certo, sua infra foi completamente removida e seus créditos não mais serão utilizados agora.

**H**appy hacking!
