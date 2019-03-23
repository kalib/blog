---
title: "Recebendo Alarmes Do Aws Diretamente No Slack"
date: 2017-03-11T17:01:49-05:00
draft: false
image: "/img/aws_slack.png"
tags: [
"Seguranca",
"Internet",
"AWS",
"Sysadmin",
"DevOps",
"Slack",
]
description: "Monitoramento de serviços e infraestrutura é uma peça fundamental em qualquer corporação. Com as tendências de times cada vez mais ágeis e interativos, automatizar e receber notificações de status de serviços nos mais distintos meios se torna cada vez mais importante. Vejamos como podemos receber notificações do AWS diretamente em um canal do seu time no Slack."
---
**A**ntes de entrar na configuração dos serviços, talvez seja necessário apresentar o <a href="https://www.slack.com" target="_blank">Slack</a>, visto que muitos ainda não conhecem ou utilizam esta poderosa e versátil ferramenta de comunicação instantânea para times.

**S**lack é uma plataforma para comunicação entre times que desejam um ambiente mais dinâmico e ágil. Diferentemente de muitas plataformas de chat disponíveis, como o Google Hangouts, o Slack nos permite criar canais distintos com membros distintos de um mesmo time fazendo parte daquele canal específico. Não, não estou falando de chat em grupo, mas sim canais específicos que permitem integrações com serviços distintos, como receber notificações sobre commits feitos em um repositório ou branch específico no github, notificações de tickets abertos em ferramentas como o Jira, por exemplo, etc. O Slack é completamente programável e escalável, o que nos permite ter inúmeras funcionalidas.

**P**rovavelmente não seja necessário apresentar o AWS, ou Amazon Web Services, visto que já está no mercado desde 2006, no entanto cabe um resumo para os que não estão familiarizados com o mesmo (embora o público alvo deste post seja quem já possui alguma familiaridade com AWS).

**A**ws ou Amazon Web Services é uma plataforma de serviços em nuvem segura, oferecendo poder computacional, armazentamento de banco de dados, distribuição de conteúdo e outras funcionalidades.

**P**or que eu deveria ter alarmes e notificações do AWS em um serviço de chat como o Slack quando já recebo estas notificações por email?

**É** verdade que o uso mais comum para envio de alarmes e notificações do AWS costuma ser via email, no entanto fica fácil identificar alguns problemas com este método. O principal e mais recorrente que vejo é o caso de as notificações caírem em um email específico visto por poucas pessoas (na maioria das vezes) ou nem visto sequer, pois geralmente as pessoas ficam cansadas de olhar notificações e ter sua caixa de entrada entupida com eles portanto criam filtros que jogam os emails de notificação para um diretório que dificilmente será checado.

**O**utro problema comum com esta prática é a demora até que alguém leia a notificação no meio de tantos outros na pasta ou filtro criado e, muitas vezes, quando se vê a notificação, o problema já está aguardando uma solução há horas.

**D**eixando claro, não estou defendendo a ideia de abolir as notificações por email. Eu mesmo utilizo ambos, afinal o email continua bastante eficiente para fins de armazenamento e checagem histórica, por exemplo.

**U**ma vez que nos dias atuais os times de TI estão cada vez mais unificados e dinâmicos, buscando incorporar uma mentalidade DevOps e Agile, a comunicação rápida e eficiente se torna um fator primordial para o sucesso de qualquer projeto. Ter um local centralizado para conversar com os demais membros do time, trocar arquivos, detalhes de projetos, receber notificações de commits, prazos, tickets, documentação e, por que não, notificações de monitoramento e alarmes, torna-se essencial.

**V**amos então entender como funcionaria uma solução para enviar as notificações e alarmes do AWS para o Slack.

**O** que utilizaremos:

1. No Slack:
    * Um plugin ou Slack App chamado **Incoming WebHooks**
    * O nome de um canal para envio das notificações
2. No AWS:
    * Serviço **SNS Topic**
    * Serviço **CloudWatch**
    * Serviço **Lambda Function**

**V**amos lá...

**Slack**

**V**amos começar escolhendo o canal no Slack no qual desejo receber a minha notificação ou alarme: #devops

*Estou supondo que você já utiliza o Slack e já possui um time criado no mesmo. Caso ainda não, crie um time no Slack seguindo os passos descritos no <a href="https://slack.com/" target="_blank">site oficial</a> antes de seguir em frente... ;]*

**O** próximo passo é configurar a integração instalando o Plugin ou Slack App **Incoming WebHooks**. Para isto, acesse a página de apps de seu time no Slack: https://SEUTIME.slack.com/apps

**P**esquise por Incoming WebHooks e você terá apenas um resultado, portanto clique sem medo.

![Incoming Webhook](/img/slack1.png)

**C**lique no pequeno lápis que se encontrará no canto direito para editar as configurações do Incoming WebHook. Os únicos campos que precisaremos editar neste momento são os seguintes:
  * Post to Channel - Aqui indicarei o meu canal: #devops
  * Customize Name - Aqui indicarei um nome qualquer: AWS-Alerts

**Importante:** Repare que nesta página de configurações ele lhe passará uma entrada ou URL com o código para o seu WebHook. Esta informação estará listada em **Webhook URL** e será algo como: *https://hooks.slack.com/services/T434P71A4/U4G3JUG13/kPjvXY4Kd8wPm4TvrEqhN6Dv*. Copie esta informação em algum local de fácil acesso pois precisaremos desta URL para a configuração que faremos a seguir no AWS.

**S**alve suas configurações e vamos configurar os serviços do AWS para que nosso WebHook possa receber as informações devidamente.

**Amazon Web Services**

**S**e você já possui alguma familiaridade com o AWS, sabe que existem duas formas principais para administração e gerenciamento de nossos serviços: Pela interface web de gerenciamento (GUI) OU pela linha de comandos através da AWS CLI Tool que se comunica com a API do AWS. Este procedimento, assim como praticamente todos os outros, pode ser realizado por ambos os meios.

**S**e você também gosta de automação, provavelmente prefere utilizar a CLI, no entanto irei listar aqui o procedimento em ambos os meios.

**Passo 1: Criando um SNS Topic para receber os alarmes**

**1.1 - Pela Interface Web de Gerenciamento (GUI)**

  * A partir da Dashboard principal, clique ou busque pelo serviço SNS;
  * Crie um novo SNS Topic:
    * No menu da lateral esquerda, clique em **Topics**;
    * Clique em **Create new topic**;
    * Preencha os campos **Name** (obrigatório) e **Display Name** (opcional) para o seu tópico. Para este exemplo utilizarei *aws-slack-alerts* como **Name** e *aws-slack* como **Display Name**; *(O Display Name só é necessário em caso de você também desejar enviar notificações por SMS)*
    * Clique em **Create Topic**
  * Agora você já deve ser capaz de ver seu SNS Topic na lista.

**1.2 - Pela AWS CLI Tool**

*Estou assumindo que se você optou por utilizar este método, é porque já possui sua CLI configurada e autenticando em sua conta do AWS com sua chave. Caso você não saiba do que estou falando, sugiro que siga a <a href="https://aws.amazon.com/pt/cli/?sc_channel=PS&sc_campaign=acquisition_CA&sc_publisher=google&sc_medium=command_line_b&sc_content=aws_cli_bmm&sc_detail=%2Baws%20%2Bcli&sc_category=command_line&sc_segment=161196437429&sc_matchtype=b&sc_country=CA&s_kwcid=AL!4422!3!161196437429!b!!g!!%2Baws%20%2Bcli&ef_id=V8jOHQAABelSRAnr:20170311204146:s" target="_blank">documentação oficial</a> para isto.*

  * Pela CLI tool, digite o seguinte comando, indicando a região na qual você deseja criar seu tópico e o nome desejado:

```python
aws sns create-topic
    --region us-west-1
    --name aws-slack-alerts
```
  * **IMPORTANTE:** Você receberá um identificador (TopicArn) para este alarme. Você precisará dele no passo seguinte.
  * Caso queira ter certeza, você pode listar seus tópicos utilizando:

```python
aws sns list-topics
```

**Passo 2: Criando um Alarme no serviço CloudWatch**

**2.1 - Pela Interface Web de Gerenciamento (GUI)**

  * A partir da Dashboard principal, clique ou busque pelo serviço **CloudWatch**;
  * Crie um novo Alarme:
    * Clique em **Alarms**;
    * Clique no botão **Create Alarm**;
    * Escolha a categoria do alarme desejado. Para este exemplo utilizarei **ELB Metric > Per-LB Metrics** *(Dentre as várias categorias disponíveis, esta se refere à Load Balancers)*;
    * Selecione a métrica exata desejada. No caso deste exemplo, preciso selecionar a métrica e o Load Balancer desejado. Ao escolher a métrica e o alvo (em meu caso um Load Balancer) clique em **Next**. Neste exemplo eu escolhi a métrica **HTTPCode_Backend_5XX** *(para monitorar 500 errors)* e um Load Balancer chamado **LB-GuySpyV3**;
    * O próximo passo é definir um nome e uma descrição para este **Alarme**, bem como definir as triggers e períodos de monitoramento. Neste exemplo utilizei o nome **LB-GuySpyV3-ELB_500** para meu alarme; *(Não entrarei em detalhes quanto ao uso das triggers, visto que para cada tipo ou categoria de métrica, as triggers serão diferentes, bem como o cenário de seu ambiente e nível de criticidade. Em resumo, se você deseja monitorar o uso de CPU de um determinado servidor, a trigger seria o gatilho que ativaria o alarme, por exemplo: Só quero ser alarmado se o uso de CPU neste servidor ou instância for >= 90% e assim permanecer por pelo menos 60 segundos, ou por dois períodos seguidos de 60seg.)*
    * Na seção **Actions** da configuração do Alarme defina o **State** e indique que a notificação deverá ser enviada **(Send notification to)** para o **SNS Topic** que criamos anteriormente. Para este exemplo optei por **State is ALARM** e decidi enviar as notificações para **aws-slack-alerts**, sendo este o SNS Topic que criei no início;
    * Finalize clicando em **Create Alarm**.

  **2.2 - Pela AWS CLI Tool**

  *Novamente... Estou assumindo que se você optou por utilizar este método, é porque já possui sua CLI configurada e autenticando em sua conta do AWS com sua chave. Caso você não saiba do que estou falando, sugiro que siga a <a href="https://aws.amazon.com/pt/cli/?sc_channel=PS&sc_campaign=acquisition_CA&sc_publisher=google&sc_medium=command_line_b&sc_content=aws_cli_bmm&sc_detail=%2Baws%20%2Bcli&sc_category=command_line&sc_segment=161196437429&sc_matchtype=b&sc_country=CA&s_kwcid=AL!4422!3!161196437429!b!!g!!%2Baws%20%2Bcli&ef_id=V8jOHQAABelSRAnr:20170311204146:s" target="_blank">documentação oficial</a> para isto.*

  * Pela CLI tool, digite o seguinte comando, indicando os atributos abaixo:
      * **region** *(Região)*;
      * **alarm-name** *(Nome do alarme)*;
      * **alarm-description** *(Descrição do alarme)*;
      * **alarm-actions** *(Definir a ação do alarme - Apontar para o TopicArn do SNS Topic que criamos anteriormente)*;
      * **metric-name** *(Nome da Métrica desejada)*;
      * **namespace AWS/ELB --statistic** *(Estatística desejada para aquela métrica, neste caso utilizarei Sum (Soma) ao invés de Average (Média))*;
      * **dimensions** *(O alvo desta métrica de monitoramento, no nosso caso um Load Balancer)*;
      * **period** e **evaluation-periods** *(Períodos desejados para a trigger)*;
      * **threshold** *(O valor desejado: Neste exemplo estou colocando o valor como 1, portanto receberei o alarme caso seja >= 1. Sim, eu sei que receberei o alarme a cada minuto, mas estou fazendo isto de propósito para recebermos a notificação a fim de teste. Nunca utilize um threshold desses em produção. :p)*;
      * **comparison-operator** *(Operador de comparação desejado, neste caso >=)*;

```python
aws cloudwatch put-metric-alarm --region us-west-1
    --alarm-name "LB-GuySpyV3-ELB_500"
    --alarm-description "Sends 500-errors to Slack"
    --actions-enabled
    --alarm-actions "TheTopicArn from last step"
    --metric-name "HTTPCode_Backend_5XX"
    --namespace AWS/ELB --statistic "Sum"
    --dimensions "Name=LoadBalancerName,Value=LB-GuySpyV3"
    --period 60
    --evaluation-periods 60
    --threshold 1
    --comparison-operator "GreaterThanOrEqualToThreshold"
```  

**Passo 3: Criando uma Função Lambda como Assinante (Subscriber) do nosso SNS Topic**

**3.1 - Pela Interface Web de Gerenciamento (GUI)**

  * A partir da Dashboard principal, clique ou busque pelo serviço **Lambda**;
  * Crie uma **Nova Função Lambda**:
    * Clique em **Create a Lambda Function**;
    * Na tela **Select Blueprint** clique na opção **cloudwatch-alarm-to-slack**; *(Você poderá precisar buscar por esta opção)*
    * O próximo passo será a tela **Configure Triggers**. Selecione o **SNS Topic** que foi criado anteriormente (aws-slack-alerts neste exemplo) e marque a opção **Enable Trigger** e clique em Next;
    * Em **Configure Function** dê um Nome e uma Descrição para a função e escolha **Node.js.4.3** como **Runtime**;
    * No campo **Lambda Function Code** cole o seguinte código: <a href="https://gist.github.com/tomfa/b33f768908b0a83987d26f269e377e95" target="_blank">Disponível no github</a>
      * (Você deverá setar os valores das variáveis **CHANNEL** e **PATH**, onde CHANNEL é o canal do Slack para o qual você deseja mandar as notificações e PATH é a URL de seu WebHook, recebida quando configuramos o Incoming WebHook no Slack)

```python
var https = require('https');
var util = require('util');

var CHANNEL = "#devops";
var PATH = "/services/T434P71A4/U4G3JUG13/kPjvXY4Kd8wPm4TvrEqhN6Dv";

exports.handler = function(event, context) {
    console.log(JSON.stringify(event, null, 2));
    console.log('From SNS:', event.Records[0].Sns.Message);

    var postData = {
        "channel": CHANNEL,
        "username": "AWS SNS",
        "text": "*" + event.Records[0].Sns.Subject + "*",
        "icon_emoji": ":aws:"
    };

    var message = event.Records[0].Sns.Message;
    var severity = "good";

    var dangerMessages = [
        " but with errors",
        " to RED",
        "During an aborted deployment",
        "Failed to deploy application",
        "Failed to deploy configuration",
        "has a dependent object",
        "is not authorized to perform",
        "Pending to Degraded",
        "Stack deletion failed",
        "Unsuccessful command execution",
        "You do not have permission",
        "Your quota allows for 0 more running instance"];

    var warningMessages = [
        " aborted operation.",
        " to YELLOW",
        "Adding instance ",
        "Degraded to Info",
        "Deleting SNS topic",
        "is currently running under desired capacity",
        "Ok to Info",
        "Ok to Warning",
        "Pending Initialization",
        "Removed instance ",
        "Rollback of environment"       
        ];

    for(var dangerMessagesItem in dangerMessages) {
        if (message.indexOf(dangerMessages[dangerMessagesItem]) != -1) {
            severity = "danger";
            break;
        }
    }

    // Only check for warning messages if necessary
    if (severity == "good") {
        for(var warningMessagesItem in warningMessages) {
            if (message.indexOf(warningMessages[warningMessagesItem]) != -1) {
                severity = "warning";
                break;
            }
        }       
    }

    postData.attachments = [
        {
            "color": severity,
            "text": message
        }
    ];

    var options = {
        method: 'POST',
        hostname: 'hooks.slack.com',
        port: 443,
        path: PATH
    };

    var req = https.request(options, function(res) {
      res.setEncoding('utf8');
      res.on('data', function (chunk) {
        context.done(null, postData);
      });
    });

    req.on('error', function(e) {
      console.log('problem with request: ' + e.message);
    });   

    req.write(util.format("%j", postData));
    req.end();
};
```

  * O **Handler** deverá ser o default `index.handler`;
    * Para **role** selecione **Create a custom role**; *(Isto será necessário apenas para a sua primeira função)*
    * Na tela seguinte selecione **lambda_basic_execution** como **IAM role** e deixe o **Policy Name** com seu valor default. O AWS irá criar uma política de segurança padrão que nos dará os privilégios necessários. Clique em **Allow**;
    * Certifique-se de que o valor para **VPC** na seção **Advanced Settings** seja **No VPC**;
    Clique em **Next**, reveja suas configurações e clique em **Create Function**;
  * Aguarde seu alarme acontecer e receba a notificação no Slack. :D

O resultado em seu Slack será algo assim...

![Notification Slack](/img/slack2.png)

**P**arabéns, você já está recebendo suas notificações via Slack. Basta criar outros alarmes no AWS utilizando a mesma Lambda Function e o mesmo SNS Topic.

**H**appy Hacking!
