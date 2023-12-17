---
title: "Criando Funções Lambda Facilmente Com O Framework Serverless"
date: 2023-12-17T11:51:33-05:00
draft: false
image: "/img/serverless.png"
tags: [
"Devops",
"AWS",
"Serverless",
"Python",
"Cloud",
"CloudFormation",
"Lambda"
]
description: "Que tal criar a infraesturtura e sua função lambda de forma simples e dinâmica com poucos comandos? O Serverless é um framework que abstrai tudo isto de forma uq epossamos focar apenas em nosso código."
---
## Serverless Framework - O que é e para que serve

**E**ntregue software com bem menos trabalho!

**S**im, é uma afirmação forte. Não, não sou eu quem o está dizendo. É literalmente a primeira frase que podemos encontrar na página de documentação do <a href="https://www.serverless.com/framework/docs/" target="_blank">Framework Serverless</a>.

**E**mbora não seja minha, concordo completamente com esta afirmação. Utilizando o framework Serverless eu realmente consigo entregas mais rápidas, passando por todo o ciclo de desenvolvimento e testes, abstraindo a parte que geralmente não nos interessa tanto: a infraestrutura.

**N**ão estou menosprezando a infraestrutura, obviamente. Sempre trabalhei com infraestrutura e entendo bem a importância de uma infraestrutura bem projetada e configurada, mas quando falamos em um mundo moderno e na nuvem, principalmente em soluções serverless como é o caso de *lambda functions* ou funções *lambda*, está claro que o nosso objetivo principal é o código da aplicação em si, e não a infraestrutura.

**O** Serverless nos ajuda justamente neste propósito, abstraindo e automatizando toda a infraestutrura que é necessária para rodarmos nossa lambda: IAM, cloudformation, Labda em si, etc. Desta forma podemos dedicar a maior parte do nosso tempo de fato em nosso código. E a melhor parte, isto não compromete a segurança ou estabilidade da infraestrutura, já que o framework Serverless estará sempre seguindo as melhores práticas ao abstrair esta camada.

**A**lém de facilitar o processo de desenvolvimento e entrega ou *deployment* de nossa função lambda, o Serverless framework possui suporte à diversas linguagens, assim como o AWS Lambda: *Node.js*, *Python*, *Java*, *Go*, *C#*, *Ruby*, *Swift*, *Kotlin*, *PHP*, *Scala* e *F#*.

## Mas o que é serverless?

**A**qui é preciso diferenciar um pouco o conceito de serverless e o Framework Serverless.

**O** conceito serverless, ou sem servidor quando traduzimos ao pé da letra, indica um modelo de desenvolvimento nativo na núvem (*cloud-native*). Isto quer dizer que o foco do desenvolvimento é 100% na aplicação, de forma que você não gerenciará a infraestrutura necessária para rodar sua aplicação. A ideia é você não precisar se preocupar em gerenciar/criar servidores, por exemplo. Sim, ainda existirão servidores que serão responsáveis por rodar a sua aplicação, obviamente, mas você não lidará com eles diretamente. Estes serão gerenciados por serviços na nuvem de sua escolha, como AWS, GCP, Azure, etc. O provedor de nuvem (*cloud*) cuidará desta parte de forma que você nem sequer saberá onde fisicamente está o seu servidor assim como não precisará ter acesso ao mesmo remotamente ou preocupar-se com as configurações do mesmo. Foco 100% na sua aplicação.

## Instalação do Serverless Framework

**IMPORTANTE:** O meu objetivo com este post não é explicar sobre Lambda functions, mas sim como criá-las com o framework Serverless. Caso esteja buscando informações mais focadas em Lambda Functions em si, sugiro que confira um outro post que publiquei anteriormente onde o foco era a criação de uma função Lambda utilizando a linguagem GO para interagir com S3 Buckets no AWS: <a href="/criando-uma-lambda-function-com-golang-que-interage-com-s3-buckets/" target="_blank">Criando Uma Lambda Function Com Golang Que Interage Com S3 Buckets</a>

**A** forma mais simples de instalar o serverless framework é através do *npm*. Caso não possua ainda Node.js instalado em sua máquina, você precisará instalá-lo antes de instalar o serverless com o *npm*: (<a href="https://nodejs.org/en" target="_blank">Node.js</a>)

```bash
➜ npm install -g serverless
```

**A**lternativamente, caso prefira instalar o Serverless sem o *npm*, você poderá utilizar o binário diretamente: <a href="https://www.serverless.com/framework/docs/install-standalone" target="_blank">Binário Serverless</a>

## Uso básico do Serverless Framework

**U**ma ves que o Serverless já esteja instalado e disponível, a primeira coisa que precisamos fazer é definir as credenciais com as quais trabalharemos com ele para nosso projeto.

**N**o console *IAM* do AWS, crie um novo usuário e, apenas para facilitar seu estudo inicial, inclua Acesso de Administrador ao seu novo usário. (Não entrarei em detalhes aqui sobre como criar um usuário no IAM pois assumo que se você está criando funções lambda, provavelmente já sabe fazer isso. Caso não, siga as instruções da <a href="https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_users_create.html" target="_blank">Amazon aqui</a>.)

**C**om seu usuário já criado e com permissões de administrador, copie a chave de acesso (*access key*) e a senha da chave de acesso (*secret access key*) do seu novo usuário, como na imagem a seguir:
![AWS](/img/serverless-01.png)
**U**tilizaremos estas credenciais para configurar nosso framework Serverless.

**E**m seu terminal digite:

```bash
➜  serverless config credentials --provider aws --key <sua-chave-de-acesso> --secret <sua-senha-da-chave-de-acesso> --profile serverlessUser
```

**P**ara criarmos nosso primeiro projeto podemos utilizar duas formas distintas de interação com o Serverless.

**A** primeira é através do menu interativo do Serverless. Desta forma, apenas responderemos as perguntas que o Serverless nos fará.

**D**igite *serverless* e um meno lhe será apresentado onde definiremos o nosso projeto:

```bash
➜ serverless

Creating a new serverless project

? What do you want to make?
❯ AWS - Node.js - Starter
  AWS - Node.js - HTTP API
  AWS - Node.js - Scheduled Task
  AWS - Node.js - SQS Worker
  AWS - Node.js - Express API
  AWS - Node.js - Express API with DynamoDB
  AWS - Python - Starter
  AWS - Python - HTTP API
  AWS - Python - Scheduled Task
  AWS - Python - SQS Worker
  AWS - Python - Flask API
  AWS - Python - Flask API with DynamoDB
  Other
```

**N**o menu acima escolheremos a opção *AWS - Python - Starter* para esta demonstração, já que Python é uma de minhas linguagens favoritas, mas perceba que temos muitas opções disponíveis e se escolhermos *Other* outras linguagens serão apresentadas também.

**A** selecionarmos *AWS - Python - Starter* o Serverless criará o diretório de nosso projeto já com um template para Python que funcionará de imediato com AWS Lambda Functions. Mas antes de isto tudo acontecer, ele nos perguntará mais alguns detalhes, portanto vamos selecionar Python e seguir em frente.

**A** pergunta seguinte será qual nome você deseja dar para seu projeto. Aqui utilizarei *primeiro-teste-serverless*:

```bash
➜ serverless

Creating a new serverless project

? What do you want to make? AWS - Python - Starter
? What do you want to call this project? primeiro-teste-serverless
```

**R**epare que ao pressionar enter o Serverless começará o download e criação dos templates:

```bash
➜ serverless

Creating a new serverless project

? What do you want to make? AWS - Python - Starter
? What do you want to call this project? primeiro-teste-serverless

✔ Project successfully created in primeiro-teste-serverless folder

? Register or Login to Serverless Framework (Y/n)
```

**E**le lhe perguntará se você deseja se cadastrar ou logar no serviço Serverless. Por hora você não precisar fazer isso, portanto apenas selecionei *n* e segui em frente. A pergunta seguinte será se você deseja efetuar o deployment de seu projeto. Mas vamos selecionar *n* por enquanto para que possamos verificar nosso projeto antes de fazermos o primeiro deployment.

```bash
➜ serverless

Creating a new serverless project

? What do you want to make? AWS - Python - Starter
? What do you want to call this project? primeiro-teste-serverless

✔ Project successfully created in primeiro-teste-serverless folder

? Register or Login to Serverless Framework No

? Do you want to deploy now? No

What next?
Run these commands in the project directory:

serverless deploy    Deploy changes
serverless info      View deployed endpoints and resources
serverless invoke    Invoke deployed functions
serverless --help    Discover more commands
```

**P**erceba que ignoramos o deployment inicial mas o Serverless já nos indicou que comandos podemos utilizar para fazer o deployment, pegar informações, invocar, etc.

**R**epare que um diretório foi criado com o nome do seu projeto e o mesmo agora possui alguns arquivos que foram criados automaticamente pelo Serverless:

```bash
➜ ls primeiro-teste-serverless
README.md      handler.py     serverless.yml
```

**V**amos verificar os arquivos *handelr.py* e *serverless.yml*.

**O** handler.py é o arquivo principal de nossa função lambda em python. De forma resumida, para quem nunca utilizou lambda, o handler é o método no código da nossa função que processa eventos. Quando nossa função é invocada, Lambda executa o método handler. A função continuará rodando até que o handler retorne uma resposta, finalize ou expire (*time out*).

**O** conteúdo padrão que o Serverless adiciona ao nosso *handler.pY* será algo similar ao seguinte:

*handler.py*
{{< highlight python "linenos=table, linenostart=0">}}
import json

def hello(event, context):
    body = {
        "message": "Go Serverless v3.0! Your function executed successfully!",
        "input": event,
    }

    return {"statusCode": 200, "body": json.dumps(body)}
{{< / highlight >}}

**C**omo podevemos ver, nada de absurdo. Um template padrão com uma função que cria um body que retorna uma mensagem dizendo que a função foi executada com sucesso no AWS Lambda.

**E** aqui está o conteúdo de nosso *serverless.yml*:

*serverless.yml*
{{< highlight yml "linenos=table, linenostart=0">}}
service: primeiro-teste-serverless

frameworkVersion: '3'

provider:
  name: aws
  runtime: python3.9

functions:
  hello:
    handler: handler.hello
{{< / highlight >}}

**N**ada de muito complexo aqui. São as definições que serão utilizadas pelo Serverless para definir a nossa função lambda. Aqui estamos usando os valores padrões que ele me trouxe, como versão do *Python* (*3.9*), nome da nossa função (*hello*) e nome do nosso *handler*.

**C**omo optamos por não fazer o deployment inicial, podemos conectar ao nosso console do AWS e confirmar no menu do serviço *Lambda* que não temos nenhuma função lambda com o nome que indicamos:
![AWS](/img/serverless-02.png)
**D**a mesma forma, podemos verificar o console do *CloudFormation* e ver que também Não temos uma stack com o nome de nosso projeto:
![AWS](/img/serverless-03.png)
**V**amos então experimentar o deployment de nossa função lambda. Para isto, digite *sls deploy*:

```bash
➜ sls deploy

Deploying primeiro-teste-serverless to stage dev (us-east-1)

⠙ Creating CloudFormation stack (4s)
⠹ Updating CloudFormation stack (47s)
✔ Service deployed to stack primeiro-teste-serverless-dev (91s)

functions:
  hello: primeiro-teste-serverless-dev-hello (1.7 kB)

Need a faster logging experience than CloudWatch? Try our Dev Mode in Console: run "serverless dev"
```
**C**omo podemos ver, o Serverless cuidou de todo o deployment, criando a stack do CLoudformation, lambda function, etc. E nos informou que tudo foi criado com sucesso.

**R**epare que o Serverless também lhe retornou o nome da sua função: *hello*

**S**e verificarmos novamente em nosso console da AWS, em *Lambda* e em *Cloudformation*, veremos que agora nosso projeto estará lá:
![AWS](/img/serverless-05.png)
![AWS](/img/serverless-04.png)
**P**odemos clicar em nossa função no console *Lambda* para vermos os detalhes de nossa função caso queiramos:
![AWS](/img/serverless-06.png)
**E** simples assim, em questão de 2 ou 3 minutos, temos nossa primeira função Lambda já funcional, bem como primeiro deployment feito com sucesso no AWS, incluindo todos os componentes de infraestrutura, com o quais não precisamos nos preocupar.
![AWS](/img/serverless-07.png)

**A**lém de deployment, o Serverless nos permite também executar nosta função diretamente de nossa linha de comando, bastando apenas executarmos *sls invoke* e informar o nome de nossa função:

```bash
➜ sls invoke -f hello --log
{
    "statusCode": 200,
    "body": "{\"message\": \"Go Serverless v3.0! Your function executed successfully!\", \"input\": {}}"
}
--------------------------------------------------------------------
START
END Duration: 1.49 ms (init: 122.58 ms) Memory Used: 37 MB
```

**C**omo podemos ver, nossa função Lambda funcionou como esperado e nos retornou exatamente a mensagem que esperávamos: *Go Serverless v3.0! Your function executed successfully!* bem como o *statusCode* 200, que significa sucesso.

**S**imples e rápido, certo?!

**O** Serverless também nos permite facilmente remover tudo referente à nossa função, incluindo a stack do *Cloudformation* e a função *lambda* em si. Para isto digite: *sls remove*:

```bash
➜ sls remove
Removing primeiro-teste-serverless from stage dev (us-east-1)

✔ Service primeiro-teste-serverless has been successfully removed (28s)
```

**C**aso verifique novamente no Console do AWS, tanto em *Lambda* quanto em *Cloudformation*, perceberá que todos os componentes foram excluídos com sucesso.

## Fazendo uma pequena customização

**V**amos fazer uma pequena alteração no template padrão que nos foi entregue.

**P**rimeiramente vamos editar nosso arquivo *serverless.yml* da seguinte forma:

*serverless.yml*
{{< highlight yml "linenos=table,hl_lines=7, linenostart=0">}}
service: primeiro-teste-serverless

frameworkVersion: '3'

provider:
  name: aws
  runtime: python3.11

functions:
  hello:
    handler: handler.hello
{{< / highlight >}}

**A** única alteração que fizemos aqui foi na linha 6, onde alteramos a versão do Python. Vamos utilizar uma versão mais atual do Python, 3.11, ao invés da versão 3.9 que estávamos utilizando antes. (Dependendo de quando você estiver lendo isto, a versão padrão que poderá vir em seu template poderá ser diferente).

**A**gora vamos modificar um pouco nosso arquivo *handler.py*:

*handler.py*
{{< highlight python "linenos=table,hl_lines=4-10, linenostart=0">}}
import json

def hello(event, context):

    cumprimento = json.dumps('Olá, diretamente da sua função Lambda!')

    print("Minha primeira função AWS Lambda com Python!")
    return {
            'statusCode': 200,
            'body': cumprimento.encode('utf-8').decode('unicode-escape')
    }
{{< / highlight >}}

**A**s linhas marcadas são nossas alterações. Basicamente recriamos todo o corpo de nossa função *hello* para que possamos desta vez receber uma mensagem um pouco mais customizada em caso de execução com sucesso, ou seja, com status *200*.

**V**amos fazer um novo deployment de nosso projeto com *sls deploy*:

```bash
➜ sls deploy

Deploying primeiro-teste-serverless to stage dev (us-east-1)

⠼ Creating CloudFormation stack (2s)
⠼ Creating CloudFormation stack (0/2) (15s)
⠋ Updating CloudFormation stack (46s)
✔ Service deployed to stack primeiro-teste-serverless-dev (90s)

functions:
  hello: primeiro-teste-serverless-dev-hello (1.7 kB)

Need a faster logging experience than CloudWatch? Try our Dev Mode in Console: run "serverless dev"
```

**N**ovamente, podemos agora testar nossa função e confirmar que nosso deployment foi realizado com sucesso executando *sls invoke -f hello --log*:

```bash
➜ sls invoke -f hello --log
{
    "statusCode": 200,
    "body": "\"Olá, diretamente da sua função Lambda!\""
}
--------------------------------------------------------------------
START
Minha primeira função AWS Lambda com Python!
END Duration: 1.24 ms Memory Used: 41 MB
```

**P**odemos ver claramente que nosso cumprimento foi impresso no body e também nossa frase *Minha primeira função AWS Lambda com Python!* foi retornada ao final.

**O** céu é o limite quanto ao que você pode fazer com isto. Como qualquer código Python, voc6e pode utilizar lambdas para múltiplas tarefas e automações, interações. Realmente as possibilidades são infinitas. 