---
title: "Criando Uma Lambda Function Com Golang Que Interage Com S3 Buckets"
date: 2021-06-27T14:38:00-04:00
draft: false
image: "/img/golang-lambda.png"
tags: [
"Devops",
"AWS",
"Lambda",
"Golang",
"Go",
"Microservices",
"Cloud"
]
description: "Criando uma lambda function com golang que interage com s3 buckets do AWS"
---
## Go ou Golang

**G**o, ou <a href="https://go.dev" target="_blank">Golang</a>, é uma linguagem de programação estaticamente tipificada, estrutural e com suporte nativo à concorrência. É uma linguagem compilada e desenvolvida pelo Google, mais especificamente por Robert Griesemer, Rob Pike e Ken Thompson. Go é uma linguagem sintaticamente similar ao C, com recursos de segurança para a memória, coleta de lixo, etc.

**E**xistem diversas razões pelas quais considero Golang a minha linguagem favorita, mas este não é o foco deste post. Caso tenha interesse, escrevi um outro post sobre o assunto:

<a href="/go-ou-golang-porquê-adotei-go-como-minha-linguagem-favorita/" target="_blank">Go Ou Golang: Porquê Adotei Go Como Minha Linguagem Favorita</a>

**E**ste post também não visa ensinar o básico sobre a linguagem Go, portanto assumo que você já possua familiaridade com a linguagem. A ideia é apenas apresentar um simples caso de uso no qual Go pode ser uma excelente escolha. Implementação de microserviços (*microservices*) ou soluções *serverless*, como no caso de Lambda Functions, no AWS.

## Lambda Functions

**A**ws Lambda é uma plataforma movida a eventos e *serverless* oferecida pelo AWS. É um serviço de computação em nuvem que executa códigos em resposta a eventos específicos e automaticamente gerencia o uso de recursos computacionais necessários para aquele código.

**E**mbora eu vá utilizar *AWS Lambda* neste exemplo, uma plataforma similar existe em quase todos os provedores de computação em nuvem (*Cloud*). No GCP (*Google Cloud Platform*), por exemplo, o serviço se chama *Google Functions*. No Microsoft Azure, se chama *Microsoft Azure Functions*, etc.

**T**odos estes serviços possuem algo em comum: Suportam múltiplas linguagens de programação. No caso do AWS as linguagens hoje (Junho, 2021) suportadas são: *Golang*, *Node.js*, *Python*, *Ruby*, *Java* e *.NET Core*.

**A** plataforma, por se tratar de uma solução *serverless*, não possui qualquer servidor ou sistema operacional que precise ser gerenciado por você. Toda a infraestrutura e camadas de SO são administradas e gerenciadas pelo seu provedor de *Cloud*. É a solução perfeita para desenvolvimento rápido quando se quer focar apenas em seu código e deixar que a plataforma de *Cloud* cuide das demais camadas.

## Criando a nossa função

**O** primeiro passo é criarmos a nossa função no AWS e para isso devemos ir ao menu de serviços do AWS e buscar por Lambda:

![Golang Lambda](/img/golang-lambda-1.png)

**A**o clicar em Lambda você será levado a uma janela similar à seguinte. Clique em **Criar função** (*Create function*).

![Golang Lambda](/img/golang-lambda-2.png)

**N**a janela de criação, daremos um nome à nossa função e selecionaremos o *Runtime* ou executor de linguagem que queremos utilizar. Utilizarei o nome *minhafuncaogolang* e escolherei o *Runtime* Golang 1.x.

![Golang Lambda](/img/golang-lambda-3.png)

**N**o topo da janela você também terá opções de criação de sua função, mas como vamos criar o nosso código completamente do zero, vamos utilizar a primeira opção, **Criar do Zero** (*Author from scratch*). Caso você esteja utilizando o AWS em português, o título poderá ser um pouco diferente, mas a ideia será a mesma.

**O**utro aspecto que gosto de selecionar é a região. Repare que no canto superior direito estou utilizando a região Canada Central 1 (*Central*). Por residir no Canada, esta é a região mais próxima de mim, portanto possuirá melhor performance. Embora para o exercício não vá fazer muita diferença, sinta-se livre para escolher qualquer região ou a que seja mais próxima de você.

**N**as configurações abaixo podemos optar por uma Role, caso já saibamos que Role utilizaremos, mas podemos ignorar isto e deixar o padrão, para que o AWS crie uma automaticamente para nós.

**A**gora apenas nos resta clicar em **Criar função** (*Create function*), no botão laranja no canto inferior direito.

**A**pós alguns segundos veremos a tela de configuração e administração de nossa função (*function*).

![Golang Lambda](/img/golang-lambda-4.png)

**A**gora que já possuímos nossa função criada, chegou a hora divertida. Vamos escrever o código de nossa função.

**J**á criei meu diretório *minhafuncaogolang* e agora criarei meu arquivo *main.go*.

**C**omo em qualquer código em Go, criaremos o nosso pacote com uma função main básica.

{{< highlight go "linenos=table, linenostart=0" >}}
package main

func main() {
	
}
{{< / highlight >}}

**A**té aqui temos apenas o início básico de nosso pacote. Por padrão, o AWS lambda espera que nosso pacote possua um *handler*, o qual será um método no código de nossa função que processará eventos. Quando nossa função é invocada ou executada, o Lambda rodará o método handler. Quando o handler finaliza sua execução ou retorna uma resposta, a função volta a ficar disponível para atender a outro evento ou requisição.

**U**ma função lambda em Go requer o pacote *github.com/aws/aws-lambda-go/lambda*, portanto teremos de importá-lo também.

**A** função *main* também é necessária, e será utilizada para chamar o nosso handler. Vamos incrementar um pouco mais nosso código:

{{< highlight go "linenos=table,hl_lines=3-5 8 11-13, linenostart=0" >}}
package main

import (
	"github.com/aws/aws-lambda-go/lambda"
)

func main() {
	lambda.Start(handler)
}

func handler(bucket MeuBucket) (*s3.CreateBucketOutput, error) {

}
{{< / highlight >}}

**A**gora que importamos *github.com/aws/aws-lambda-go/lambda* e criamos nosso handler que recebe um *bucket* e retorna um *Output* e um *error*, chamamos este handler a partir de nossa função main utilizando o pacote lambda fornecido pelo AWS.

**I**ndicamos que nosso handler retorna um CreateBucketOutput, o qual é padrão fornecido pela sdk Go do AWS. Mais detalhes podem ser encontrados <a href="https://docs.aws.amazon.com/sdk-for-go/api/service/s3/" target="_blank">aqui</a>.

**J**á que pretendemos utilizar o pacote *S3* fornecido pelo AWs, devemos também importá-lo em nosso código. Além disso, para trabalharmos com o AWS devemos iniciar uma sessão, portanto criaremos uma variável para nossa sessão também.

{{< highlight go "linenos=table,hl_lines=5-7 10-12, linenostart=0" >}}
package main

import (
	"github.com/aws/aws-lambda-go/lambda"
	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

var (
	sessaos3 *s3.S3
)

func main() {
	lambda.Start(handler)
}

func handler(bucket MeuBucket) (*s3.CreateBucketOutput, error) {

}
{{< / highlight >}}

**C**omo pretendemos criar um bucket no s3, criaremos um struct para nosso bucket, no qual passaremos um nome para o bucket que desejamos criar e uma região.

{{< highlight go "linenos=table,hl_lines=14-17, linenostart=0" >}}
package main

import (
	"github.com/aws/aws-lambda-go/lambda"
	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

var (
	sessaos3 *s3.S3
)

type MeuBucket struct {
	NOME_BUCKET string `json:"bucket"`
	REGIAO      string `json:"regiao"`
}

func main() {
	lambda.Start(handler)
}

func handler(bucket MeuBucket) (*s3.CreateBucketOutput, error) {

}
{{< / highlight >}}

**R**epare que nosso struct MeuBucket está em formato padrão json, tal como invocaríamos em uma API qualquer.

**C**om nosso struc devidamente criado é hora de darmos corpo ao nosso código e inciarmos a nossa lógica. A ideia é criar um bucket no s3, portanto vamos criar uma função chamada *criarBucket*. Nossa função receberá nosso struct bucket e iniciará a sessão aws para o s3.

**A** nossa função *criarBucket* deverá receber os dados fornecidos em nosso struct a criar o bucket com o nome que foi passado, obviamente que validando a existência do bucket pois o AWS não permite que dois buckets existam com o mesmo nome globalmente.

**P**ara que possamos fazer estes tratamentos de erros, importaremos também os pacotes *errors* e *awserr*, que também faz parte da SDK do AWS.

{{< highlight go "linenos=table,hl_lines=4 8 22-47, linenostart=0" >}}
package main

import (
    "errors"

	"github.com/aws/aws-lambda-go/lambda"
	"github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

var (
	sessaos3 *s3.S3
)

type MeuBucket struct {
	NOME_BUCKET string `json:"bucket"`
	REGIAO      string `json:"regiao"`
}

func criarBucket(bucket MeuBucket) (*s3.CreateBucketOutput, error) {
	sessaos3 = s3.New(session.Must(session.NewSession(&aws.Config{
		Region: aws.String(bucket.REGIAO),
	})))

	resp, err := sessaos3.CreateBucket(&s3.CreateBucketInput{
		ACL:    aws.String(s3.BucketCannedACLPublicRead),
		Bucket: aws.String(bucket.NOME_BUCKET),
		CreateBucketConfiguration: &s3.CreateBucketConfiguration{
			LocationConstraint: aws.String(bucket.REGIAO),
		},
	})
	if err != nil {
		if aerr, ok := err.(awserr.Error); ok {
			switch aerr.Code() {
			case s3.ErrCodeBucketAlreadyExists:
				return nil, errors.New("Bucket com este nome já existe!")
			case s3.ErrCodeBucketAlreadyOwnedByYou:
				return nil, errors.New("Bucket com este nome já existe, e é seu!")
			default:
				return nil, err
			}
		}
	}
	return resp, nil
}

func main() {
	lambda.Start(handler)
}

func handler(bucket MeuBucket) (*s3.CreateBucketOutput, error) {

}
{{< / highlight >}}

**E**ste bloco de código foi um pouco maior. Aqui, além de iniciarmos nossa sessão s3 também tratamos possíveis erros caso já exista um bucket com o nome que escolhemos.

**N**osso código está se utilizando da própria SDK do aws para tratar os erros, de forma que receberemos a mensagem *Bucket com este nome já existe!* caso já exista globalmente (em alguma outra conta do AWS que não seja a sua) um bucket utilizando o nome fornecido, ou *Bucket com este nome já existe, e é seu!* caso o bucket já exista e tenha sido criado em sua conta.

**A** única coisa que nos falta é tratar nosso handler para que ele traga os devidos retornos ou resultados gerados pela nossão função *criarBucket*.

{{< highlight go "linenos=table,hl_lines=54, linenostart=0" >}}
package main

import (
    "errors"

	"github.com/aws/aws-lambda-go/lambda"
	"github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

var (
	sessaos3 *s3.S3
)

type MeuBucket struct {
	NOME_BUCKET string `json:"bucket"`
	REGIAO      string `json:"regiao"`
}

func criarBucket(bucket MeuBucket) (*s3.CreateBucketOutput, error) {
	sessaos3 = s3.New(session.Must(session.NewSession(&aws.Config{
		Region: aws.String(bucket.REGIAO),
	})))

	resp, err := sessaos3.CreateBucket(&s3.CreateBucketInput{
		ACL:    aws.String(s3.BucketCannedACLPublicRead),
		Bucket: aws.String(bucket.NOME_BUCKET),
		CreateBucketConfiguration: &s3.CreateBucketConfiguration{
			LocationConstraint: aws.String(bucket.REGIAO),
		},
	})
	if err != nil {
		if aerr, ok := err.(awserr.Error); ok {
			switch aerr.Code() {
			case s3.ErrCodeBucketAlreadyExists:
				return nil, errors.New("Bucket com este nome já existe!")
			case s3.ErrCodeBucketAlreadyOwnedByYou:
				return nil, errors.New("Bucket com este nome já existe, e é seu!")
			default:
				return nil, err
			}
		}
	}
	return resp, nil
}

func main() {
	lambda.Start(handler)
}

func handler(bucket MeuBucket) (*s3.CreateBucketOutput, error) {
    return criarBucket(bucket)
}
{{< / highlight >}}

**I**sto é tudo o que precisamos retornar em nosso handler.

**A**qui já temos um código funcional e que pode ser executado com sucesso em nossa função lambda.

**P**odemos agora inicializar nosso projeto:

```yaml
➜ go mod init minhafuncaogolang
go: creating new go.mod: module minhafuncaogolang
go: to add module requirements and sums:
	go mod tidy
➜ go get .
```

**C**om isto os arquivos go.mod e go.sum foram criados. O próximo passo é fazermos o build/compilação do nosso código para que possámos gerar um binário e enviá-lo para o AWS.

**C**aso você esteja em uma máquina **Linux** ou **Mac/OS X**, utilize:

```yaml
➜ GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o main -ldflags '-w' main.go && zip archive.zip main
```

**E**ste comando irá fazer o build, utilizando as flags exigidas pelo AWS e compactará o seu binário em um arquivo *archive.zip*.

**U**m arquivo binário *main* será criado, juntamente com seu arquivo compactado *archive.zip*.

**WINDOWS:** Se você utiliza Windows, obviamente as coisas são um pouco mais complicadas. Você primeiramente precisará baixar uma ferramenta chamada build-lambda-zip: *(Eu imagino que as instruções abaixo funcionem, já que foram fornecidas pela própria Amazon, mas não possuo máquinas com Windows portanto não testei este procedimento. - https://docs.aws.amazon.com/lambda/latest/dg/golang-package.html)*

```yaml
go get -u github.com/aws/aws-lambda-go/cmd/build-lambda-zip
```

**E**m seguida utilize os seguintes comandos:

```yaml
➜ set GOOS=linux
➜ go build -o main main.go
➜ %USERPROFILE%\Go\bin\build-lambda-zip.exe -output archive.zip main
```

**V**amos agora enviar nosso código para o AWS para que possamos testar nossa função.

**D**e volta ao AWS, na página de nossa função, clique no botão de Upload, localizado na sessão de Código Fonte (*Source Code*). Selecione a opção *.zip file*.

![Golang Lambda](/img/golang-lambda-5.png)

**S**elecione seu arquivo *archive.zip* e envie-o clicando em **Salvar** (*Save*).

![Golang Lambda](/img/golang-lambda-6.png)

**A**gora nossa função no AWS já possui um código funcional e podemos fazer o nosso primeiro teste.

**A** primeira coisa que faremos é mudar o nome de nossa função *Handler*. Por padrão o AWS trás o nome *hello*, portanto mudaremos para *main*, que é o nome que utilizamos. Para isso, ainda na tela de sua função, clique em **Editar** (*Edit*) na sessão *Runtime*.

![Golang Lambda](/img/golang-lambda-7.png)
![Golang Lambda](/img/golang-lambda-8.png)

**A**ntes de executarmos um teste em si, vamos alterar a *role* que é utilizada pela função para que a mesma possua permissão de criar buckets no s3.

**C**lique na aba de **Configurações** (*Configuration*) e em seguida em **Permissões** (*Permissions*).

![Golang Lambda](/img/golang-lambda-11b.png)

**C**lique no nome da *Role*. A página de permissões da role será apresentada, clique em *Attach policies*.

![Golang Lambda](/img/golang-lambda-12.png)

**E**m um ambiente de produção daríamos apenas as permissões que realmente precisamos para nossa *Role*, mas para simplificar esta demonstração, busque pela policy *AmazonS3FullAccess*. Selecione-a e clique em *Attach policy* no canto inferior direito da tela.

![Golang Lambda](/img/golang-lambda-13.png)

**A**gora vamos criar um evento de teste. Voltando para a página de nossa função, clique em **Teste** (*Test*) e apague os campos de Input, já que não passamos ainda nenhum input para nosso código.

**O** conteúdo de nosso teste será o seguinte:

{{< highlight go "linenos=table,hl_lines=4-6 17-26 33-45, linenostart=0" >}}
{
    "bucket": bucket-minhafuncaogolanglambda,
    "regiao": "ca-central-1"
}
{{< / highlight >}}

**S**inta-se livre para utilizar o nome que desejar para seu bucket, bem como a região que preferir.

**E**m seguida clique no botão laranja **Teste** (*Test*).

![Golang Lambda](/img/golang-lambda-9.png)

**S**e o seu código estava exatamente igual ao meu, você provavelmente receberá um resultado com status *200*, o que significa que sua função executou com sucesso. (Caso você tenha recebido um **erro** significa que o seu código contém algo errado OU um bucket com o nome que você indicou já existe.)

![Golang Lambda](/img/golang-lambda-10.png)

**S**e eu executar novamente o mesmo teste, com o mesmo nome para o bucket, desta vez teremos um erro:

![Golang Lambda](/img/golang-lambda-11.png)

**C**omo era esperado, o erro indica que o bucket já existe e é meu, pois este foi o bucket que criei em minha execução anterior. Experimente alterar o nome do bucket para algo bem óbvio e que certamente alguém já usou, por exemplo *teste*.

![Golang Lambda](/img/golang-lambda-14.png)

**E**xatamente o erro esperado. O bucket já existe, porém não é nosso.

**O**bviamente que se formos até nossa página do s3 veremos que o bucket que criamos na primeira execução estará lá:

![Golang Lambda](/img/golang-lambda-15.png)