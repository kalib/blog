---
title: "Criando Infraestrutura No Aws Com Sceptre"
date: 2022-03-12T20:12:54-05:00
draft: false
image: "/img/python-aws.png"
tags: [
"Devops",
"AWS",
"Sceptre",
"Python",
"Cloud",
"CloudFormation"
]
description: "O que é o Sceptre e como utilizar esta tecnologia para melhor gerenciamento de infraestruturas na nuvem AWS com CloudFormation."
---
## Sceptre? O que é isso?

**S**ceptre é uma ferramenta de IaC ou Infraestrutura como Código (*Infrastructure as Code*). Se este termo ainda é novidade para você, sugiro uma lida em um post no qual explico o que é e quais as vantagens de se utilizar Infraestrutura como Código: <a href="/infraestrutura-como-código-com-terraform/" target="_blank">Infraestrutura como Código com Terraform</a>

**A**ssim como o *Terraform*, o Sceptre nos ajuda a gerenciar infraestruturas no AWS de forma mais eficiente. Criado pela empresa <a href="https://www.cloudreach.com/" target="_blank">CloudReach</a>, o Sceptre possui algumas diferenças marcantes quando comparado ao Terraform. A primeira delas é que não se trata de uma ferramenta para gerenciamento de infraestrutura como código de forma generalista, ou seja, não pode ser utilizado para qualquer provedor de nuvem por exemplo. O Sceptre foi criado especificamente com o intuito de gerenciar infraestrutura no AWS. Além disso, o Sceptre não funciona de forma 100% autônoma, como no caso do Terraform que gerencia e manipula recursos na nuvem mantendo um arquivo (ou vários) para seu status. O Sceptre na verdade utiliza-se do já existente *CloudFormation* do AWS.

**S**e você não conhece o CloudFormation, trata-se da solução nativa da Amazon para gerenciamento de infraestrutura na nuvem AWS utilizando-se de infraestrutura como código. Mas.. espera aí... algo está esquisito...

![Estranho](/img/estranho.png)

**S**e o AWS já possui sua própria ferramenta nativa de infraestrutura como código, que é o CloudFormation... É compreensível o uso de Terraform como uma alternativa por ser um padrão mais genérico e que pode ser utilizado para todas as nuvens, não apenas o AWS. Mas o que justificaria utilizar o Sceptre ao invés do CloudFormation se por baixo ele acabará utilizando o CloudFormation?

![calma](/img/calma.png)

**O** Sceptre foi criado para gerenciar o CloudFormation, sim. Ninguém criaria algo apenas por criar, portanto o Sceptre veio com a proposta de lidar com algumas carências já existentes no CloudFormation. O Sceptre é capaz de criar, atualizar e deletar stacks ou conjuntos de recursos no AWS, além de fornecer informações mais detalhadas, metadata bem como recuperar facilmente tais dados.

**C**riado em *Python*, o Sceptre pode ser utilizado como um comando CLI ou como um módulo Python, pronto para ser utilizado em ambientes de produção e enterprise e desenhado para ser executado como parte de pipelines de CI/CD.

**A** principal motivação para a criação do Sceptre foi o fato de o CloudFormation não possuir recursos mais robustos para gerenciamento de stacks ou conjuntos de recursos, enquanto que a CLI padrão do AWS bem como a biblioteca Boto3 para Python oferecem alguns destes recursos, mas ainda assim nenhuma delas disponibiliza:

- Links entre a saída de uma stack com parâmetros de outra stack;
- Facilidade para se trabalhar com roles ou mesmo com múltiplas contas do AWS;

**S**em mais lenga lenga, vamos ao que interessa.

## Instalação

**A**s duas formas mais simples de se utilizar o Sceptre são via docker ou instalando via Pip. Sinta-se livre para utilizar a forma que mais lhe agradar.

**A**o longo do post estarei utilizando o sceptre instalado localmente através do pip, mas os passos deverão funcionar via docker também caso prefira este método.

### Utilizando Docker

**E**sta opção assume que você já possua o Docker instalado.

**B**aixe a imagem Docker do Sceptre da seguinte forma:

```bash
➜ docker pull cloudreach/sceptre
```

**C**aso queira baixar alguma versão em específico, inclua a versão desejada ao seu comando:

```bash
➜ docker pull cloudreach/sceptre:2.1.3
```

**D**eixando em branco ele buscará a última versão disponível (*latest*).

**P**odemos testar o Sceptre via Docker utilizando o comando a seguir para vermos a versão do Sceptre utilizada.

```bash
➜ docker run cloudreach/sceptre:latest --version

Sceptre, version 3.0.0
```

**S**im, o container Docker é iniciado, executa o que pedimos e se encerra em seguida. Oi, simples assim.

**V**ale lembrar que será necessário sempre montar o seu diretório local em seu container docker para que o sceptre executado no container tenha acesso ao seu código, portanto para cada comando que executaremos a seguir você utilizaria algo similar a isto:

```bash
docker run -v $(pwd):/project -v ~/.aws/:/root/.aws/:ro cloudreach/sceptre:latest
```

### Utilizando Pip

**E**sta opção assume que você já possua o Python e o Pip instalados.

**S**endo uma ferramenta Python, podemos intalar o sceptre com o pip:

```bash
➜ pip install sceptre
```

**E**m seguida podemos confirmar a versão do sceptre instalada:

```bash
➜ sceptre --version
```

## Utilizando o Sceptre

### Autenticação com o AWS

**O** Sceptre utilizará os mesmos arquivos de configuração utilizados pela CLI do AWS para autenticar-se, portanto este post assume que você já possui também o AWS CLI instalado e configurado em sua máquina.

**C**aso você ainda não possua, siga os passos descritos aqui para instalar e configurar o seu AWS CLI: 

- <a href="https://docs.aws.amazon.com/pt_br/cli/latest/userguide/getting-started-install.html" target="_blank">Instalação do AWS CLI</a>
- <a href="https://docs.aws.amazon.com/pt_br/cli/latest/userguide/cli-configure-quickstart.html" target="_blank">Configuração do AWS CLI</a>

**V**ocê pode testar seu AWS CLI através do seguinte comando:

```bash
➜ aws sts get-caller-identity

{
    "UserId": "ABLABLABLABLABLABLADH",
    "Account": "6777777777775",
    "Arn": "arn:aws:iam::6777777777775:user/marcelo"
}
```

**S**e você recebeu um retorno similar a este, informando o ID do seu usuário AWS, o ID de sua conta AWS e o ARN de seu uruário, significa que seu AWS CLI está devidamente configurado e conseguindo autenticar e se comunicar com o AWS.

### Iniciando com o Sceptre

**A**gora que sabemos que o Sceptre já conseguirá autenticar-se com o AWS vamos iniciar a configuração básica de nossa infraestrutura.

**O** Sceptre utiliza uma árvore ou estrutura de diretórios seguindo o padrão a seguir:

```bash
meu-projeto-sceptre
    ├── config
    │   └── config.yaml
    └── templates
```

**B**asicamente a estrutura mais simples é com o diretório principal de seu projeto e dentro dele dois diretórios, sendo um deles para as configurações e o outro para os templates.

**O** Sceptre pode criar esta árvore de diretórios automaticamente ao criarmos um novo projeto. Para isto utilizaremos o comando *sceptre new project*. Perceba que ele lhe pedirá que indique a região na qual deseja o seu projeto. Esta seria a região a ser utilizada no AWS para a sua infraestrutura. No meu caso estou utilizando ca-central-1, pois é onde moro. Sinta-se livre para usar a região de sua preferência, como por exemplo sa-east-1 (para São Paulo, Brasil). A lista de regiões pode ser encontrada aqui: <a href="https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/using-regions-availability-zones.html" target="_blank">Lista de Regiões AWS</a>. Em seguida o Sceptre lhe pedirá um código para o projeto, pode deixar em branco:

```bash
➜ sceptre new project meu-projeto-sceptre

Please enter a region []: ca-central-1
Please enter a project_code [meu-projeto-sceptre]:
```

**P**odemos confirmar a criação do diretório de nosso projeto, bem como os dois sub-diretórios:

```bash
➜ ls meu-projeto-sceptre

config    templates
```

**A**gora que temos nosso projeto criado, podemos começar a inserir nosso código de fato.

**V**erifcando o que foi criado pelo Sceptre automaticamente veremos que o diretório de templates está vazio, mas o diretório de config possui um arquivo padrão *config.yaml* com o seguinte conteúdo:

{{< highlight yaml "linenos=table, linenostart=0" >}}
project_code: meu-projeto-sceptre
region: ca-central-1
{{< / highlight >}}

**O** diretório *config* é onde manteremos as configurações de nossas stacks e o diretório *templates* é onde manteremos os arquivos de template do CloudFormation.

**V**amos iniciar criando um stack chamado *test* e iniciaremos criando uma VPC nesta stack, visto que VPCs são geralmente o nosso ponto de partida em uma nuvem já que todo e qualquer recurso deve ser criado em uma VPC.

**C**riaremos um diretório chamado *test* dentro do diretório *config*. Sinta-se livre para criá-lo da forma que preferir. Em Linux ou OS X(mac) podemos fazer isto através do comando *mkdir* executado dentro do diretório de nosso projeto, *meu-projeto-sceptre*:

```bash
➜ mkdir config/test

➜ ls config
config.yaml test
```

**E**m seguida vamos criar o arquivo de configuração de nossa stack *test*, o arquivo de configuração de nossa VPC e também o template de nossa vpc:

```bash
➜ touch config/test/config.yaml config/test/vpc.yaml templates/vpc.yaml
```

**A** nossa árvore de diretórios e arquivos deverá estar da seguinte forma:

```bash
➜ tree .
.
├── config
│   ├── config.yaml
│   └── test
│       ├── config.yaml
│       └── vpc.yaml
└── templates
    └── vpc.yaml

3 directories, 4 files
```

**N**estes arquivos teremos:

- templates/vpc.yaml: o template CloudFormation de nossa VPC
- config/test/vpc.yaml: as configurações referentes a este template de VPC
- config/test/config.yaml: a configuração do ambiente

**V**amos iniciar pela configuração do nosso ambiente. Para isto daremos um código de projeto e a região na qual desejamos nossa infraestrutura. Vamos inserir o seguinte conteúdo no arquivo *config/test/config.yaml*:

{{< highlight yaml "linenos=table, linenostart=0" >}}
project_code: projeto-sceptre
region: ca-central-1
{{< / highlight >}}

**N**ovamente, lembre-se de usar a região que melhor lhe sirva, não precisa utilizar a região canadense como estou utilizando aqui.

**E**m seguida vamos inserir a configuração do template de nossa VPC no arquivo *config/test/vpc.yaml*:

{{< highlight yaml "linenos=table, linenostart=0" >}}
template:
  path: vpc.yaml
  type: file
parameters:
  CidrBlock: 10.0.0.0/16
{{< / highlight >}}

**R**epare que aqui temos dois blocos de código:

- template: definimos que template será usado com esta configuração. Indicamos que o path é *vpc.yaml*, pois este é o caminho referente ao nosso template de vpc que fica dentro do diretório de templates.
- parameters: aqui listamos parâmetros, que podem ser utilizados como variáveis para nosso template. Neste caso estamos utilizando apenas um parâmetro, que é o bloco de endereços IP de nossa VPC, o *CidrBlock*.

**A**gora vamos ao nosso template CloudFormation em si. Insira o seguinte código no arquivo *templates/vpc.yaml*:

{{< highlight yaml "linenos=table, linenostart=0" >}}
Parameters:
  CidrBlock:
    Type: String
Resources:
  VPC:
    Type: 'AWS::EC2::VPC'
    Properties:
      CidrBlock:
        Ref: CidrBlock
Outputs:
  VpcId:
    Value:
      Ref: VPC
{{< / highlight >}}

**P**erceba que este código yaml é bem parecido com o yaml já utilizado pelo CloudFormation padrão. O tipo do recurso (*type*) também é exatamente como definido e utilizado pelo CloudFormation, *AWS::EC2::VPC*. O único detalhe aqui é o parâmetro que estamos utilizando nas propriedades (*Properties*), onde estamos referenciando (*Ref*) o parâmetro *CidrBlock*. Apenas lembrando, nós definimos este parâmetro em nosso arquivo de configuração */config/test/vpc.yaml*, portanto aqui o valor *10.0.0.0/16* será utilizado para a nossa VPC.

**Agora que nosso código está utilizável podemos começar a testar o que fizemos até agora.

**A**ntes disto, podemos ver que não possuo nenhuma stack já criada em minha conta do AWS, bem como ainda não temos a VPC que será criada com este CidrBlock que definimos.

![AWS](/img/sceptre-1.png)

![AWS](/img/sceptre-2.png)

**A**gora podemos executar nosso código. Com o comando *sceptre create test/vpc.yaml* poderemos aplicar e criar nosso stack. O comando nos pedirá uma confirmação, bastando digitar *y* para confirmar a execução:

```bash
➜ sceptre create test/vpc.yaml

Do you want to create 'test/vpc.yaml' [y/N]: y
[2022-03-13 13:41:37] - test/vpc - Creating Stack
[2022-03-13 13:41:38] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack CREATE_IN_PROGRESS User Initiated
[2022-03-13 13:41:42] - test/vpc VPC AWS::EC2::VPC CREATE_IN_PROGRESS
[2022-03-13 13:41:42] - test/vpc VPC AWS::EC2::VPC CREATE_IN_PROGRESS Resource creation Initiated
[2022-03-13 13:41:59] - test/vpc VPC AWS::EC2::VPC CREATE_COMPLETE
[2022-03-13 13:42:03] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack CREATE_COMPLETE
```

**R**epare que a execução não reportou nenhum erro. Se durante esta execução checarmos novamente a página de stacks do CLoudFormation, poderemos ver que a nossa stack está sendo criada:

![AWS](/img/sceptre-3.png)

**A**pós a finalização da execução, deveremos ver a stack já criada bem como a VPC:

![AWS](/img/sceptre-4.png)

![AWS](/img/sceptre-5.png)

**T**ambém podemos utilizar o próprio Sceptre para verificar a lista de nossos recursos no ambiente test com o comando *sceptre list resources*:

```bash
➜ sceptre list resources test

StackLogicalResourceIdPhysicalResourceId

test/vpcVPCvpc-027cb3d73fcc7aa64
```

**A**lgo que devemos sempre ter em nossa lista de prioridades para todo e qualquer recurso criado em nossa infraestrutura é a identificação. No caso do AWS, utilizamos Tags. As tags são essenciais para organização e identificação de nossos recursos. Como esquecemos de inserir tags em nossa VPC, vamos alterar nosso arquivo *templates/vpc.yaml*:

{{< highlight yaml "linenos=table,hl_lines=10-12, linenostart=0" >}}
Parameters:
  CidrBlock:
    Type: String
Resources:
  VPC:
    Type: 'AWS::EC2::VPC'
    Properties:
      CidrBlock:
        Ref: CidrBlock
      Tags:
      - Key: Ambiente
        Value: test
Outputs:
  VpcId:
    Value:
      Ref: VPC
{{< / highlight >}}

**A**qui inserimos apenas uma tag para este exemplo. Para efetuarmos esta atualização podemos usar o comando *sceptre update*:

```bash
➜ sceptre update test/vpc.yaml

Do you want to update 'test/vpc.yaml' [y/N]: y
[2022-03-13 13:56:45] - test/vpc - Updating Stack
[2022-03-13 13:56:45] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack UPDATE_IN_PROGRESS User Initiated
[2022-03-13 13:56:49] - test/vpc VPC AWS::EC2::VPC UPDATE_IN_PROGRESS
[2022-03-13 13:56:49] - test/vpc VPC AWS::EC2::VPC UPDATE_COMPLETE
[2022-03-13 13:56:53] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack UPDATE_COMPLETE_CLEANUP_IN_PROGRESS
[2022-03-13 13:56:53] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack UPDATE_COMPLETE
```

Podemos voltar à nossa página de VPCs do AWS e veremos que nossa VPC agora possui a nossa tag:

![AWS](/img/sceptre-6.png)

**V**amos criar então um recurso mais tangível, como por exemplo uma Instância EC2 (*VM*). Primeiramente, podemos ver que não possuo nenhuma instância já existente:

![AWS](/img/sceptre-7.png)

**C**riaremos dois novos arquivos, sendo um de configuração para nosso ec2 e um de template para o ec2:

```bash
➜ touch config/test/ec2.yaml templates/ec2.yaml
```

**P**ara simplificar nosso exemplo, definiremos 2 parâmetros em nosso arquivo *test/config/ec2.yaml*, sendo eles *ImageId* e *InstanceType*:

{{< highlight yaml "linenos=table, linenostart=0" >}}
template:
  path: ec2.yaml
  type: file
parameters:
  ImageId: ami-041d49677629acc40 # AMI para Amazon Linux distro
  InstanceType: t2.micro
{{< / highlight >}}

**A**gora inserimos o código de nosso arquivo *templates/ec2.yaml* onde referenciaremos os dois parâmetros já definidos no arquivo de config:

{{< highlight yaml "linenos=table, linenostart=0" >}}
Parameters:
  ImageId:
    Type: String
  InstanceType:
    Type: String
Resources:
  Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId:
        Ref: ImageId
      InstanceType:
        Ref: InstanceType
      Tags:
      - Key: Ambiente
        Value: test
      - Key: Name
        Value: Instancia-test
Outputs:
  InstanceId:
    Value:
      Ref: Instance
{{< / highlight >}}

**A**gora podemos criar esta nova stack com o comando *sceptre create test/ec2.yaml*:

```bash
➜ sceptre create test/ec2.yaml

Do you want to create 'test/ec2.yaml' [y/N]: y
[2022-03-13 14:32:16] - test/ec2 - Creating Stack
[2022-03-13 14:32:17] - test/ec2 projeto-sceptre-test-ec2 AWS::CloudFormation::Stack CREATE_IN_PROGRESS User Initiated
[2022-03-13 14:32:21] - test/ec2 Instance AWS::EC2::Instance CREATE_IN_PROGRESS
[2022-03-13 14:32:25] - test/ec2 Instance AWS::EC2::Instance CREATE_IN_PROGRESS Resource creation Initiated
[2022-03-13 14:32:37] - test/ec2 Instance AWS::EC2::Instance CREATE_COMPLETE
[2022-03-13 14:32:41] - test/ec2 projeto-sceptre-test-ec2 AWS::CloudFormation::Stack CREATE_COMPLETE
```

**R**epare que tanto a nova stack quanto a instância EC2 foram devidamente criadas no AWS:

![AWS](/img/sceptre-8.png)

![AWS](/img/sceptre-9.png)

**A**través do sceptre podemos ainda listar os outputs que definimos em nossos templates com o comando *sceptre list outputs*:

```bash
➜ sceptre list outputs test

StackOutputKeyOutputValue

test/ec2InstanceIdi-05031a4a80244fd17
test/vpcVpcIdvpc-027cb3d73fcc7aa64
```

**O**bviamente que não precisamos lidar com cada stack individualmente. Aqui criamos com o sceptre a stack de vpc e em seguida a stack de ec2. Mas e se tivéssemos 20 stacks? 50 stacks? Não queremos ter que executar um comando para cada uma, certo?!

**V**amos testar isso limpando nosso ambiente com o comando *sceptre delete*:

```bash
➜ sceptre delete test/ec2.yaml

The following stacks, in the following order, will be deleted:
test/ec2

Do you want to delete 'test/ec2.yaml' [y/N]: y
[2022-03-13 14:45:21] - test/ec2 - Deleting stack
[2022-03-13 14:45:21] - test/ec2 projeto-sceptre-test-ec2 AWS::CloudFormation::Stack DELETE_IN_PROGRESS User Initiated
[2022-03-13 14:45:25] - test/ec2 Instance AWS::EC2::Instance DELETE_IN_PROGRESS
[2022-03-13 14:45:54] - test/ec2 - delete complete
```

e...

```bash
➜ sceptre delete test/vpc.yaml

The following stacks, in the following order, will be deleted:
test/vpc

Do you want to delete 'test/vpc.yaml' [y/N]: y
[2022-03-13 14:46:07] - test/vpc - Deleting stack
[2022-03-13 14:46:08] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack DELETE_IN_PROGRESS User Initiated
[2022-03-13 14:46:12] - test/vpc VPC AWS::EC2::VPC DELETE_IN_PROGRESS
[2022-03-13 14:46:29] - test/vpc - delete complete
```

**A**gora que nossas stacks e recursos foram excluídos com sucesso, podemos criar tudo de uma vez com o comando *sceptre create test*, sem especificar uma stack em específico. Desta forma o sceptre entenderá que precisa executar todas as stacks que se encontram dentro de *test*:

```bash
➜ sceptre create test

Do you want to create 'test' [y/N]: y
[2022-03-13 14:49:20] - test/ec2 - Creating Stack
[2022-03-13 14:49:20] - test/vpc - Creating Stack
[2022-03-13 14:49:21] - test/ec2 projeto-sceptre-test-ec2 AWS::CloudFormation::Stack CREATE_IN_PROGRESS User Initiated
[2022-03-13 14:49:21] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack CREATE_IN_PROGRESS User Initiated
[2022-03-13 14:49:25] - test/vpc VPC AWS::EC2::VPC CREATE_IN_PROGRESS
[2022-03-13 14:49:25] - test/ec2 Instance AWS::EC2::Instance CREATE_IN_PROGRESS
[2022-03-13 14:49:25] - test/vpc VPC AWS::EC2::VPC CREATE_IN_PROGRESS Resource creation Initiated
[2022-03-13 14:49:29] - test/ec2 Instance AWS::EC2::Instance CREATE_IN_PROGRESS Resource creation Initiated
[2022-03-13 14:49:41] - test/ec2 Instance AWS::EC2::Instance CREATE_COMPLETE
[2022-03-13 14:49:41] - test/vpc VPC AWS::EC2::VPC CREATE_COMPLETE
[2022-03-13 14:49:46] - test/vpc projeto-sceptre-test-vpc AWS::CloudFormation::Stack CREATE_COMPLETE
[2022-03-13 14:49:46] - test/ec2 projeto-sceptre-test-ec2 AWS::CloudFormation::Stack CREATE_COMPLETE
```

**R**epare que desta vez tanto a stack de vpc quanto a de ec2 foram criadas.

**O**bviamente que este foi apenas um exemplo simplista do uso do Sceptre, mas a ideia era de fato apresentar esta alternativa.

**A**braço!