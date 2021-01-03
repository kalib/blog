---
title: "Criando Um Cluster Kubernetes Local Com O Kind"
date: 2021-01-03T13:29:27-05:00
draft: false
image: "/img/kindlogo.png"
tags: [
"Devops",
"Containers",
"Kubernetes",
"Docker",
"Kind"
]
description: "Rodar um cluster kubernetes local muitas vezes facilita e acelera o desenvolvimento de aplicações, testes ou mesmo criação de pipelines de CI/CD. Dentre as várias opções disponívels para tal, abordarei o Kind, que roda clusters kubernetes locais utilizando-se do Docker."
---

## O que é o Kind - Seja gentil

**D**esenvolver uma aplicação para rodar em um cluster Kubernetes, embora não seja uma tarefa tão difícil, requer muitos testes e deployments, como em qualquer outro tipo de desenvolvimento mas o desafio se torna um pouco maior e caro quando falamos de Kubernetes.

**N**em sempre podemos ter uma série de máquinas disponíveis para criarmos um ou mais clusters para testes de forma simples e rápida, certo?! É claro, hoje temos diversos provedores na nuvem (cloud) que podem nos permitir criar um novo cluster Kubernetes em alguns minutos, mas nem sempre esta opção é viável, pois caso você mantenha o cluster rodando por algumas horas o custo começa a pesar.

**A**lém disso, dificilmente você irá precisar de apenas algumas horas, certo?! Sua aplicação vai receber atualizações, melhorias, correções de bugs, etc. Tudo isto requer mais desenvolvimento, mais testes, mais deployments, o que significa novamente clusters disponíveis para tais tarefas. Uma forma de reduzir os custos na nuvem para este tipo de rotina é você criar e destruir seus clusters cada vez que for parar de desenvolver/testar. Mas se você faz isso diariamente ou algumas vezes por semana, este trabalho se torna complicado e demandará muito de seu tempo, além de ainda assim lhe custar alguns dólares por cada hora que mantém seu cluster funcionando.

**D**iversas alternativas foram criadas com foco nesta necessidade, visando justamente uma forma eficiente, prática e barata para que possamos rodar clusters Kubernetes localmente sem a necessidade de termos múltiplas máquinas, múltiplas VMs ou mesmo clusters na nuvem, como por exemplo no Google Cloud ou AWS.

**D**entre estas alternativas, algumas das mais comuns são o Docker em si, que agora trás uma versão simplificada do Kubernetes com apenas um node (master), o kubeadm, minikube, kind, etc.

**N**este post falaremos um pouco mais sobre o <a href="https://kind.sigs.k8s.io/">Kind</a>. Kind, que na verdade é uma palavra em inglês que significa gentil, também é uma abreviação para *Kubernetes in Docker* (Kubernetes em Docker).

**D**e acordo com sua página oficial, o Kind é uma ferramenta que lhe permite rodar clusters Kubernetes utilizando-se de Docker containers como nodes. Isso mesmo, cada node do seu cluster, será na verdade um container Docker em sua máquina, o que resultará na verdade em um cluster de containers rodando dentro de outros containers. Parece engraçado e estranho, rodar containers dentro de outros containers, mas lembre-se, o objetivo aqui não é um cluster de produção, e sim uma forma simples e eficiente para testes, desenvolvimento, CI, etc.

**D**e fato, o Kind é uma ferramenta tão eficiente neste sentido que é utilizada oficialmente no desenvolvimento do Kubernetes em si, como plataforma de testes.

**N**ão seria fantástico poder subir ou deletar um novo cluster em segundos? Todos sabemos o quão rápido é criar um novo container no Docker, certo?! Muito mais rápido que criar uma nova máquina virtual. Da mesma forma, se cada node de nosso cluster será um container, podemos apenas esperar que a criação de um novo cluster seja também rápida.

**O** kind utiliza o Containerd como seu Container Runtime ao invés do Dockershim. Apenas estou citando isto devido a todo o barulho criado com a notícia de o Kubernetes estar abandonando o uso do Docker como seu container runtime. Ou seja, o kind não será afetado em nada com isto.

**D**iferentemente do Kubernetes que vem em sua instalação Docker, o Kind lhe permitirá facilmente criar múltiplos clusters, com imagens diferentes para os nodes bem como com múltiplos nodes, o que tornará seus testes ainda mais realistas.

## Instalação

**A**qui assumirei que você já possui o **Docker** instalado e funcionando, afinal o Kind criará seus nodes com containers Docker.

**A**lém do Docker, assumo que você já possua alguma familiaridade com o Kubernetes e que já possua o **kubectl** instalado.

### OS X

**N**o mac podemos utilizar o gerenciador de pacotes homebrew que cuidará de todas as dependências e configurações de ambiente:

```
$ brew install kind
```

### Linux

**P**ara diferentes distribuições Linux o pacote poderá ter um nome diferente em seus repositórios ou mesmo não estar presente nos repositórios, portanto utilizaremos o binário oficial para a instalação aqui:

```
$ curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64

$ chmod +x ./kind

$ mv ./kind /algum-diretório-no-seu-PATH/kind
```

### Windows

**C**aso você possua o choco ou chocolatey como gerenciador de pacotes, pode instalar o kind apenas utilizando o seguinte comando:

```
$ choco install kind
```

**C**aso não possua este gerenciador de pacotes, pode fazer o download manualmente através do seguinte link: https://kind.sigs.k8s.io/dl/v0.9.0/kind-windows-amd64

**S**alve o arquivo e lembre-se de **renomeá-lo** para kind.exe. É importante que ele seja salvo em um diretório que faça parte de seu Path, do contrário você precisará incluir o diretório no seu PATH em suas variáveis de ambiente.

## Como utilizar

**A**ntes de mais nada, vejamos se temos algum container rodando em nosso Docker. (Embora isto não seja necessário, queremos ver o que realmente será criado pelo Kind). Para isto utilizaremos o comando *docker ps* que listará os containers rodando no momento:

```
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

**Ó**timo, nenhum container foi listado. Não temos nada rodando em nosso Docker.

**P**ara criarmos um novo cluster Kubernetes precisamos apenas utilizar o comando *kind create cluster --name <algum nome para nosso cluster>*: (Aqui criarei um cluster chamado teste)

```
$ kind create cluster --name teste
Creating cluster "teste" ...
 ✓ Ensuring node image (kindest/node:v1.19.1) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-teste"
You can now use your cluster with:

kubectl cluster-info --context kind-teste

Have a nice day! 👋
```

**O** kind nos informou que tudo ocorreu bem e que nosso cluster foi criado com sucesso. Além disso, ele nos informou que o nome do contexto criado foi *kind-teste*.

**R**epare que ele aparentemente criou apenas o Master node (control-plane), conforme podemos ver na linha *✓ Preparing nodes 📦*. (Apenas uma caixa).

**V**amos novmaente rodar *docker ps* para ver o que foi criado:

```
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                       NAMES
0c468b4d1574   kindest/node:v1.19.1   "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes   127.0.0.1:65165->6443/tcp   teste-control-plane
```

**P**odemos ver que de fato apenas um container foi criado, e que se trata de nosso master node ou control-plane.

**C**omo podemos ver se nosso cluster está funcional?

**S**e você está utilizando o Docker para Desktop que possui Kubernetes nativo e, caso o mesmo esteja com o Kubernetes inicializado, o seu novo cluster provavelmente estará visível na lista de contextos disponíveis em seu docker:

![Docker](/img/kind-1.png)

**A**o selecionar este contexto *kind-teste* poderemos a seguir utilizar o kubectl para gerenciar nosso cluster.

**O**pcionalmente, você também pode exportar o contexto manualmente com o comando *export KUBECONFIG="$(kind get kubeconfig-path --name="teste")"*.

**C**om o cluster criado, podemos listar os clusters existentes no kind com o comando *kind get clusters*:

```
$ kind get clusters
teste
```

**E** também podemos pegar informações importante sobre o nosso cluster com o comando *kubectl cluster-info --context kind-teste*:

```
$ kubectl cluster-info --context kind-teste
Kubernetes master is running at https://127.0.0.1:51908
KubeDNS is running at https://127.0.0.1:51908/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

**P**odemos listar nossos nodes com *kubectl get nodes* e listar todos os pods (inclusive os padrões do sistema kubernetes) com *kubectl get pods --all-namespaces*:

```
$ kubectl get nodes
NAME                  STATUS   ROLES    AGE   VERSION
teste-control-plane   Ready    master   12m   v1.19.1

$ kubectl get pods --all-namespaces
NAMESPACE            NAME                                          READY   STATUS    RESTARTS   AGE
kube-system          coredns-f9fd979d6-254xj                       1/1     Running   0          11m
kube-system          coredns-f9fd979d6-s6jlc                       1/1     Running   0          11m
kube-system          etcd-teste-control-plane                      1/1     Running   0          12m
kube-system          kindnet-6qnl5                                 1/1     Running   0          11m
kube-system          kube-apiserver-teste-control-plane            1/1     Running   0          12m
kube-system          kube-controller-manager-teste-control-plane   1/1     Running   0          12m
kube-system          kube-proxy-fbhnf                              1/1     Running   0          11m
kube-system          kube-scheduler-teste-control-plane            1/1     Running   0          12m
local-path-storage   local-path-provisioner-78776bfc44-dfcc8       1/1     Running   0          11m
```

**C**omo podemos ver, temos todos os pods padrões do Kubernetes, mas nenhum que nós mesmos tenhamos criado.

**P**ara deletar o cluster podemos utilizar o comando *kind delete cluster --name teste**:

```
$ kind delete cluster --name teste
Deleting cluster "teste" ...
```

**S**imples assim o seu cluster foi deletado.

**O** kind permite diversas configurações e cenários, como por exemplo a utilização de arquivos de configuração para as definições do seu cluster.

**N**o exemplo a seguir criaremos um arquivo chamado *cluster-teste.yml* com o seguinte conteúdo:

{{< highlight yml "linenos=table,linenostart=0" >}}
# Configuração para um cluster com 3 nodes (1 master e 2 worker nodes)
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
{{< / highlight >}}

**E**m seguida podemos iniciar o nosso cluster com o comando *kind create cluster --name teste --config cluster-teste.yml*:

```
$ kind create cluster --name teste --config cluster-teste.yml
Creating cluster "teste" ...
 ✓ Ensuring node image (kindest/node:v1.19.1) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-teste"
You can now use your cluster with:

kubectl cluster-info --context kind-teste

Have a nice day! 👋
```

**R**epare que desta vez a saída do comando nos indicou a criação de 3 nodes. Podemos ver que ele nos apresentou 3 caixas na linha: ✓ Preparing nodes 📦 📦 📦

**S**e executarmos o comando *docker ps* poderemos ver que agora 3 containers foram criados, um para cada node:

```
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                       NAMES
41f91c9a0e30   kindest/node:v1.19.1   "/usr/local/bin/entr…"   2 minutes ago   Up 2 minutes   127.0.0.1:49543->6443/tcp   teste-control-plane
5830cff7f4e8   kindest/node:v1.19.1   "/usr/local/bin/entr…"   2 minutes ago   Up 2 minutes                               teste-worker
094a5c181c00   kindest/node:v1.19.1   "/usr/local/bin/entr…"   2 minutes ago   Up 2 minutes                               teste-worker2
```

**D**a mesma forma podemos ver nossos nodes no cluster:

```
$ kubectl get nodes
NAME                  STATUS   ROLES    AGE     VERSION
prod1-control-plane   Ready    master   3m29s   v1.19.1
prod1-worker          Ready    <none>   2m58s   v1.19.1
prod1-worker2         Ready    <none>   2m58s   v1.19.1
```

**O** Kind lhe possibilitará uma série de configurações como mapeamento de portas, proxy, ou mesmo versões específicas do Kubernetes, o que é muito útil para fazer testes em sua aplicação antes de realizar uma atualização real do Kubernetes em seu cluster de produção. Todas as configurações podem ser encontradas em detalhes na documentação do projeto.

**P**ara fazermos um teste simples e termos certeza de que nosso cluster está de fato funcional e que poderia receber deployments de aplicações vamos rodar um servidor web em nosso cluster local.

**C**om o nosso cluster já funcionando com 1 master (control-plane) e 2 workers, vamos criar um deployment e um service para o nosso nginx web server.

**P**rimeiramente criaremos um arquivo chamado *nginx-deployment.yml* com o seguinte conteúdo:

{{< highlight yml "linenos=table,linenostart=0" >}}
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16.1
        ports:
        - containerPort: 80
{{< / highlight >}}

**E**m seguida criaremos o arquivo *nginx-service.yml*:

{{< highlight yml "linenos=table,linenostart=0" >}}
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
  labels:
    app: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30225
  selector:
    app: nginx
{{< / highlight >}}

(Não entraremos em detalhes sobre o conteúdo destes arquivos pois o foco do post é o Kind e assumo que você já possui certa familiaridade com o Kubernetes)

**C**om nossos dois arquivos criados vamos aplicá-los ao nosso cluster, criando assim um deployment do nginx com duas réplicas e um serviço que irá expôr o nosso deployment:

```yml
$ kubectl create -f nginx-deployment.yml
deployment.apps/nginx-deployment created

$ kubectl create -f nginx-service.yml
service/nginx-svc created
```

**P**odemos confirmar a criação de nosso deployment e de nosso service com os comandos *kubectl get deployments* e *kubectl get services*, ou mesmo checar tudo de uma vez com *kubectl get all*:

```yml
$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-559d658b74-5klfx   1/1     Running   0          18m
pod/nginx-deployment-559d658b74-zclb2   1/1     Running   0          18m

NAME                 TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.96.0.1     <none>        443/TCP        22m
service/nginx-svc    NodePort    10.96.88.59   <none>        80:30225/TCP   17m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     2            2           18m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-559d658b74   2         2         2       18m
```

**A**li podemos ver também que dois pods foram criados rodando o nginx. Isto se dá por termos dito em nosso deployment que queremos 2 replicas do nosso webserver.

**P**ara que possamos de fato testar nosso webserver nos resta agora apenas fazer um redirecionamento de portas para que possamos testar localmente. Como nosso serviço está escutando na porta 80, vamos mapear isso na porta 8000 de nossa máquina local (assumindo que nada em sua máquina está utilizando esta porta): *kubectl port-forward service/nginx-svc 8000:80*

```yml
$ kubectl port-forward service/nginx-svc 8000:80
Forwarding from 127.0.0.1:8000 -> 80
Forwarding from [::1]:8000 -> 80
Handling connection for 8000
Handling connection for 8000
```

**A**gora podemos abrir nosso browser e acessar nosso endereço local, na porta 8000: 127.0.0.1:8000

![Docker](/img/kind-2.png)

**D**a mesma forma que fizemos para inserir worker nodes, podemos inserir também múltiplos Master ou control-planes, para um cluster de alta disponibilidade. As possibilidades com o kind são inúmeras e vale muito a pena ler a documentação do projeto e brincar um pouco com ele.