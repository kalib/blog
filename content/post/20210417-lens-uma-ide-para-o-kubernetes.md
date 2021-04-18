---
title: "Lens - Uma IDE para o Kubernetes"
date: 2021-04-17T15:26:43-04:00
draft: false
image: "/img/lens-logo.png"
tags: [
"Devops",
"Containers",
"Kubernetes",
"Docker",
"Prometheus",
"Lens"
]
description: "Cansado de digitar tantos comandos com o kubectl para saber como anda o status do seu cluster? Cansado de alternar seu contexto o tempo todo para gerenciar múltiplos clusters Kubernetes? Cansado de não ter uma forma simples e interativa de gerenciar e manipular os diversos recursos em seu cluster? Apresento-lhe o Lens, uma magnífica IDE para o Kubernetes."
---

## O problema

**E**m meu dia a dia gerencio múltiplos clusters, seja no trabalho onde temos diversos clusters para distintos projetos ou mesmo em casa onde costumo criar clusters para estudos ou pequenos projetos pessoais, e por vezes sinto que a linha de comando, embora muito eficiente, simples e robusta, acaba me limitando ou ao menos tomando muito tempo em alguns aspectos. Se você também gerencia múltiplos clusters certamente deve ter passado pelos mesmos problemas...

**E**xecuto um `kubectl get pods` e de repente percebo que executei no cluster errado pois esqueci de trocar meu contexto. Preciso verificar como anda o uso de recursos de meu cluster e preciso abrir uma outra aplicação, seja uma suíte completa de monitoramente, como o Datadog, Prometheus, entre outros, ou simplesmente utilizando outras ferramentas de linha de comando tais como o *top*.

**E** se eu pudesse ter tudo isto em uma única interface, que reunisse todas as informações sobre o meu cluster, monitoramento e gráficos que me indiquem a performance, além de total controle sobre cada recurso rodando em meu cluster, seja um simples pod, um deployment, um replicaset ou mesmo um chart Helm?

**M**as sabe o que seria legal mesmo além de tudo isso? Se eu pudesse a partir desta mesma interface me conectar diretamente dentro de um container rodando em meus pods e executar comandos, checar logs, ou mesmo buscar e instalar novos charts Helm em meu cluster. Eu sei, eu sei.. estou querendo demais... ou será que não?

## Lens - A magnífica IDE para Kubernetes

**Q**uer uma solução open source e gratuita que entrega tudo o que mencionei acima e muito mais? Lhes apresento o Lens, uma IDE para Kubernetes originalmente desenvolvida pela Mirantis e disponibilizada como um projeto Open Source.

**O** Lens é hoje a IDE mais robusta para quem precisa gerenciar clusters Kubernetes diariamente. É uma aplicação única, disponível para Linux, MacOS e Windows, e não possui quaisquer requisitos específicos quanto ao cluster, podendo funcionar perfeitamente bem em qualquer cluster que você possua, seja em um provedor de cloud como o Google Cloud ou AWS, clusters locais ou mesmo soluções simples e de desenvolvimento como o kind, minikube ou o Docker para Desktop.

**O** gerenciamento multi cluster é simples e apenas precisamos identificar o contexto de cada cluster, o que nos permitirá gerenciar clusters no Google Cloud, no AWS, locais em sua máquina com o kind, etc.. Todos em uma única interface, de forma que você apenas precisará clicar no cluster que deseja gerenciar e tudo o que verá em sua frente será referente ao cluster escolhido.

**C**omo forma de melhorar a organização de seus clusters você pode também agrupá-los de forma que você tenha clusters de produção em um grupo, clusters de desenvolvimento em outro, e até mesmo clusters de estudos em um terceiro grupo. O Lens chama estes grupos de workspaces.

**P**ara simplificar o monitoramento o Lens nos entrega uma integração com o Prometheus que lhe permite habilitar e realizar o deployment do Prometheus em seu cluster com um único clique, e assim passará a ter gráficos e métricas de seu cluster.

## Instalação

O processo de instalação será um pouco diferente em cada plataforma.

### OS X

**P**or sorte temos o pacote disponível via homebrew:

```
$ brew cask install lens
```

### Linux e Windows

**O**s instaladores específicos para cada sistema operacional podem ser baixados diretamente aqui: https://github.com/lensapp/lens/releases/tag/v4.2.2

## Configuração

**A**gora que temos o Lens instalado o próximo passo é sabermos que cluster desejamos administrar com ele. Para este exemplo estarei criando um cluster de testes com o kind rapidamente.

**V**ocê poderá utilizar qualquer cluster Kubernetes que deseje, seja em algum provedor cloud, com docker, minikube, etc. Caso queira seguir exatamente o mesmo exemplo que farei aqui com o kind, aqui vai o arquivo yaml de configuração que estarei utilizando para criar o meu cluster:

{{< highlight yml "linenos=table,linenostart=0" >}}
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
{{< / highlight >}}

**A** propósito, caso nunca tenha utilizado o kind ou não entenda bem como ele funciona, sugiro a leitura de meu post anterior: <a href="/criando-um-cluster-kubernetes-local-com-o-kind/" target="_blank">Criando um cluster kubernetes local com o Kind</a>

**C**om meu arquivo yaml criado criarei o cluster utilizando o seguinte comando: `kind create cluster --name kalibk8s --config cluster-teste.yml`

```
➜ kind create cluster --name kalibk8s --config cluster-teste.yml

Creating cluster "kalibk8s" ...
 ✓ Ensuring node image (kindest/node:v1.19.1) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-kalibk8s"
You can now use your cluster with:

kubectl cluster-info --context kind-kalibk8s
```

**C**om o meu cluster pronto, é hora de configurar o Lens. Repare que ao finalizar a criação do cluster o Kind me informa o nome do contexto para o meu novo cluster: *kind-kalibk8s*. Precisarei desta informação para configurar o Lens.

**A**o iniciar o Lens recebo uma tela similar a esta:

![Lens](/img/lens2.png)

**O** primeiro passo é clicar no botão de novo cluster ou +, conforme apresentado no imagem acima.

**E**m seguida clique no botão Select Contexts (*Selecionar Contextos*).

![Lens](/img/lens3.png)

**U**m menu em drop down será apresentado com todos os contextos existentes para você neste momento. Ele levará em conta as configurações existentes no seu arquivo de configuração do Kubernetes. Aqui eu selecionarei o contexto do meu cluster criado com o kind: *kind-kalibk8s*

![Lens](/img/lens4.png)

**C**om o contexto selecionado, basta clicar em **Add Cluster** (*Adicionar Cluster*).

![Lens](/img/lens5.png)

**C**ada um destes ícones no lado esquerdo representa um dos clusters que eu tenho configurado neste momento no Lens. No seu caso, caso tenha acabado de instalar, só deverá listar um.

**N**o menu lateral esquerdo podemos ver que temos as opções de Cluster, Nodes, Workloads, Configuration, Network, etc. Estes são os recursos do meu cluster. Já na janela central, temos a parte de monitoramento ou métricas. Por padrão repare que nada aparece, pois não tenho Prometheus instalado ou configurado para este cluster.

**P**ara vermos o que acontecerá, posso executar em meu terminal o seguinte comando: `kubectl get all -A`

{{< highlight yml "linenos=table,linenostart=0" >}}
➜ kubectl get all -A

NAMESPACE            NAME                                                 READY   STATUS    RESTARTS   AGE
kube-system          pod/coredns-f9fd979d6-5nf47                          1/1     Running   0          39m
kube-system          pod/coredns-f9fd979d6-5v6gc                          1/1     Running   0          39m
kube-system          pod/etcd-kalibk8s-control-plane                      1/1     Running   0          39m
kube-system          pod/kindnet-4s9pl                                    1/1     Running   1          39m
kube-system          pod/kindnet-d4qfp                                    1/1     Running   0          39m
kube-system          pod/kindnet-ppwm7                                    1/1     Running   0          39m
kube-system          pod/kube-apiserver-kalibk8s-control-plane            1/1     Running   0          39m
kube-system          pod/kube-controller-manager-kalibk8s-control-plane   1/1     Running   0          39m
kube-system          pod/kube-proxy-c2j8p                                 1/1     Running   0          39m
kube-system          pod/kube-proxy-r2574                                 1/1     Running   0          39m
kube-system          pod/kube-proxy-s7ssz                                 1/1     Running   0          39m
kube-system          pod/kube-scheduler-kalibk8s-control-plane            1/1     Running   0          39m
local-path-storage   pod/local-path-provisioner-78776bfc44-69b2l          1/1     Running   0          39m

NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  40m
kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   40m

NAMESPACE     NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   daemonset.apps/kindnet      3         3         3       3            3           <none>                   40m
kube-system   daemonset.apps/kube-proxy   3         3         3       3            3           kubernetes.io/os=linux   40m

NAMESPACE            NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system          deployment.apps/coredns                  2/2     2            2           40m
local-path-storage   deployment.apps/local-path-provisioner   1/1     1            1           40m

NAMESPACE            NAME                                                DESIRED   CURRENT   READY   AGE
kube-system          replicaset.apps/coredns-f9fd979d6                   2         2         2       39m
local-path-storage   replicaset.apps/local-path-provisioner-78776bfc44   1         1         1       39m
{{< / highlight >}}

**R**epare que não há nada em nosso cluster referente à métricas. Nenhum pod, nenhum deployment, nenhum replicaset...

**V**amos começar habilitando o Prometheus. Clique com o botão direito no ícone de seu cluster e em seguida clique em **Settings** (*Configurações*):

![Lens](/img/lens6.png)

**N**esta janela de configurações, navegue para o final e encontrará a opção de instalar os recursos de Métricas. Clique no botão azul **Install** (*Instalar*):

![Lens](/img/lens7.png)

**A**gora que instalamos o pacote de métricas, podemos executar novamente o comando anterior para vermos a diferença. A seguir destaquei todas as linhas referentes aos recursos que foram instalados pelo Lens em meu cluster: pods, services, daemonset, deployment, replicaset, etc.

{{< highlight yml "linenos=table,hl_lines=15-19 25-27 32 36 41 45, linenostart=0" >}}
➜ kubectl get all -A
NAMESPACE            NAME                                                 READY   STATUS    RESTARTS   AGE
kube-system          pod/coredns-f9fd979d6-5nf47                          1/1     Running   0          43m
kube-system          pod/coredns-f9fd979d6-5v6gc                          1/1     Running   0          43m
kube-system          pod/etcd-kalibk8s-control-plane                      1/1     Running   0          43m
kube-system          pod/kindnet-4s9pl                                    1/1     Running   1          43m
kube-system          pod/kindnet-d4qfp                                    1/1     Running   0          42m
kube-system          pod/kindnet-ppwm7                                    1/1     Running   0          42m
kube-system          pod/kube-apiserver-kalibk8s-control-plane            1/1     Running   0          43m
kube-system          pod/kube-controller-manager-kalibk8s-control-plane   1/1     Running   0          43m
kube-system          pod/kube-proxy-c2j8p                                 1/1     Running   0          42m
kube-system          pod/kube-proxy-r2574                                 1/1     Running   0          43m
kube-system          pod/kube-proxy-s7ssz                                 1/1     Running   0          42m
kube-system          pod/kube-scheduler-kalibk8s-control-plane            1/1     Running   0          43m
lens-metrics         pod/kube-state-metrics-565db9f7c-8sn7p               1/1     Running   0          53s
lens-metrics         pod/node-exporter-6hqxj                              1/1     Running   0          52s
lens-metrics         pod/node-exporter-bhzqt                              1/1     Running   0          52s
lens-metrics         pod/node-exporter-fkstm                              1/1     Running   0          52s
lens-metrics         pod/prometheus-0                                     1/1     Running   0          53s
local-path-storage   pod/local-path-provisioner-78776bfc44-69b2l          1/1     Running   0          43m

NAMESPACE      NAME                         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)                  AGE
default        service/kubernetes           ClusterIP   10.96.0.1     <none>        443/TCP                  43m
kube-system    service/kube-dns             ClusterIP   10.96.0.10    <none>        53/UDP,53/TCP,9153/TCP   43m
lens-metrics   service/kube-state-metrics   ClusterIP   10.96.67.28   <none>        8080/TCP                 53s
lens-metrics   service/node-exporter        ClusterIP   None          <none>        80/TCP                   52s
lens-metrics   service/prometheus           ClusterIP   10.96.59.93   <none>        80/TCP                   53s

NAMESPACE      NAME                           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system    daemonset.apps/kindnet         3         3         3       3            3           <none>                   43m
kube-system    daemonset.apps/kube-proxy      3         3         3       3            3           kubernetes.io/os=linux   43m
lens-metrics   daemonset.apps/node-exporter   3         3         3       3            3           <none>                   53s

NAMESPACE            NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system          deployment.apps/coredns                  2/2     2            2           43m
lens-metrics         deployment.apps/kube-state-metrics       1/1     1            1           53s
local-path-storage   deployment.apps/local-path-provisioner   1/1     1            1           43m

NAMESPACE            NAME                                                DESIRED   CURRENT   READY   AGE
kube-system          replicaset.apps/coredns-f9fd979d6                   2         2         2       43m
lens-metrics         replicaset.apps/kube-state-metrics-565db9f7c        1         1         1       53s
local-path-storage   replicaset.apps/local-path-provisioner-78776bfc44   1         1         1       43m

NAMESPACE      NAME                          READY   AGE
lens-metrics   statefulset.apps/prometheus   1/1     53s
{{< / highlight >}}

**A**gora que temos suporte à métricas em nosso cluster, podemos fechar essa janela de configurações clicando no **X** do canto superior direito:

![Lens](/img/lens8.png)

**A**o clicar em **Cluster** novamente poderei agora ver alguns gráficos sobre os recursos do meu cluster:

![Lens](/img/lens9.png)

**A**o clicar em **Nodes** posso ver que possuo 3 nodes em meu cluster, bem como uma breve visão de como andam os recursos destes nodes:

![Lens](/img/lens10.png)

**A**penas para ilustrar um pouco melhor, farei o deployment de alguns recursos em meu cluster, simplesmente para facilitar os exemplos a seguir.

**R**epare que na região inferior temos um botão para **Terminal**:

![Lens](/img/lens11.png)

**A**ssim podemos ter um terminal dentro do Lens. Utilizarei este terminal para criar um arquivo yaml chamado recursos-lens-demo.yml com o seguinte conteúdo:

{{< highlight yml "linenos=table, linenostart=0" >}}
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: web-replicaset
  labels:
    app: web
spec:
  replicas: 6
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web-replicaset
        image: nginx
---
apiVersion: v1
kind: Pod
metadata:
  name: outropod
  labels:
    app: multiplos
spec:
  containers:
  - name: redis
    image: redis
  - name: nginx
    image: nginx
  - name: fluentd
    image: fluentd
{{< / highlight >}}

**E**m seguida utilizando o mesmo terminal de dentro do Lens executarei: `kubectl create -f recursos-lens-demo.yml`

![Lens](/img/lens12.png)

**A**gora podemos clicar em **Workloads** e em seguida em **Overview** para termos uma visão geral dos recursos em nosso cluster no namespace **default**. No meu caso, o arquivo yaml utilizado acima cria 1 replicaset com 6 pods, e 1 pod extra rodando 3 containers. Portanto poderemos ver na imagem abaixo que temos 7 Pods ao todo e 1 Replica Set:

![Lens](/img/lens13.png)

**R**epare que também temos os eventos de nosso cluster no canto inferior da janela quando fechamos ou minimizamos o terminal.

**N**a visão de Pods podemos ver mais detalhes sobre nossos pods, inclusive a quantidade de containers rodando em cada pod, o status, o node no qual estão rodando, etc.

![Lens](/img/lens14.png)

**A**o clicarmos em um pod em específico podemos pegar mais detalhes do mesmo bem como uso de recursos total do pod e individual por cada container do mesmo, caso seja um pod com múltiplos containers:

![Lens](/img/lens15.png)

**R**epare também que ao clicar em um pod qualquer, uma das opções que será apresentada será a de terminal:

![Lens](/img/lens16.png)

**A**o clicarmos nesse ícone de terminal um terminal será aberto já logado dentro do pod que selecionamos. Isso facilita nossa vida quando precisamos checar logs, debugar ou executar comandos diretamente dentro do pod:

![Lens](/img/lens17.png)

**A**o clicarmos em **Apps** no menu e em seguida em **Charts**, podemos gerenciar os charts que tivermos instalados em nosso cluster, bem como buscar e instalar novos charts:

![Lens](/img/lens18.png)

**A**penas a nível de teste, vamos instalar um chart qualquer. No campo de busca de helm charts, vamos buscar por *Jenkins*. Vamos instalar o Jenkins para que tenhamos uma ferramenta de CI/CD em nosso cluster:

![Lens](/img/lens19.png)

**A**o clicar em Jenkins, uma janela com maiores informações será apresentada. Esta janela terá o botão de instalação. Clique em **Install**.

![Lens](/img/lens20.png)

**U**m terminal será apresentado no qual você poderá alterar as configurações deste helm chart antes de instalá-lo. Para este exemplo básico, não alteraremos nada, apenas confirmarei a instalação clicando em **Install** novamente:

![Lens](/img/lens21.png)

**U**ma vez que a instalação esteja finalizada, você receberá a confirmação e poderá clicar em **View Helm Release**.

![Lens](/img/lens22.png)

**V**ocê receberá uma janela com informações sobre o seu pacote helm que foi instalado. Pode ignorar isto por enquanto. Vou clicar novamente em **Workloads** e em **Overview**. Desta vez podemos ver que temos um Deployment que não tínhamos antes, 1 novo pod e 1 Replica Set também novo. Estes são justamente por conta da instalação do chart do Jenkins em nosso cluster.

![Lens](/img/lens23.png)

**E**stes são apenas alguns dos recursos e possibilidades que o Lens oferece. Brinque um pouco e explore todas as possibilidades que esta ótima ferramenta tem a oferecer.

**E**espero que, assim como aconteceu comigo, o Lens lhe traga maior performance e economize tempo para que você possa otimizar seus resultados e dedicar mais tempo à novas implementações.

**H**ave fun!