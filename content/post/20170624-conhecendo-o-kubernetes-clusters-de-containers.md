---
title: "Conhecendo O Kubernetes - Clusters De Containers"
date: 2017-06-24T16:27:16-05:00
draft: false
image: "/img/kubernetes_logo.png"
tags: [
"Devops",
"Docker",
"Kubernetes",
]
description: "Sistema Open Source desenvolvido pelo Google para o gerenciamento de cluster de containers tendo como características auto-scaling de serviços e containers, auto-monitoração de containers, permite o deploy de containers e serviços, load balancer, orquestração de containers, e orquestração de volumes de armazenamento."
---
## O que é Kubernetes

**K**ubernetes é uma solução Open Source desenvolvida pelo Google, originalmente chamada de K8s, como uma ferramenta para gerenciar clusters de containers (ou containeres, como prefira). Em 2005, quando a ferramenta foi desenvolvida, originalmente para uso interno, o Google doou o código à recém fundada <a href="https://www.cncf.io/" target="_blank">Cloud Native Computing Foundation</a>, em parceria com a <a href="https://www.linuxfoundation.org/" target="_blank">The Linux Foundation</a>.

**O** motivo do leme em sua logomarca é devido à origem grega da palavra, que vem de Kuvernetes, que representa a pessoa que pilota o navio, timoneiro.

**C**omo objetivo primário o Kubernetes provê uma plataforma de automação para deployments, escalonamento e operações de containers de aplicações em um cluster de hosts ou nodes.

**A**ntes de seguir com a explicação, instalação e configuração do Kubernetes, estou supondo que você já possui algum conhecimento básico sobre o que sejam containers e tenha alguma familiaridade com o <a href="https://www.docker.com" target="_blank">Docker</a>. Caso não possua um entendimento básico sobre containers e Docker, sugiro que leia algo antes de seguir com este artigo. Possuo um post introdutório sobre containers com um exemplo básico e prático sobre como criar containers com Docker, bem como iniciar uma simples aplicação web - <a href="docker-uma-alternativa-elegante-para-containers-no-linux/" target="_blank">aqui</a>.

**O** Kubernetes é formado por uma série de componentes ou blocos que, quando utilizados coletivamente, fornecem um método de deployment, manutenção e escalonamento de clusters de aplicações baseadas em containers. Estes componentes, ou primitives como o Kubernetes os chama, foram desenvolvidos com o intuito de serem independentes, de forma que quase não se faz necessário ter conhecimento entre si para que possam funcionar e trabalhar juntos, visto que todos se comunicam e interligam através de uma API, sejam componentes internos do Kubernetes ou mesmo extensões e containers.

**E**mbora tenha sido inicialmente desenvolvido para o deployment e utilização de **bilhões de containers** internamente no Google, desde que seu código passou a ser distribuído abertamente com a licença <a href="https://commons.apache.org/proper/commons-daemon/license.html" target="_blank">Apache Commons</a> o Kubernetes tem sido adotado formalmente por praticamente todos os grandes provedores de serviços em nuvem.

## Arquitetura do Kubernetes

![Kubernetes Architecture](/img/kubernetes_architecture.png)

**D**entre os principais componentes do Kubernetes, vamos destacar os seguintes:

* **Master ou Master Controller** - Host que será o gerenciador principal do Kubernetes, responsável por gerenciar os Minions ou Nodes do Cluster;

* **Nodes ou Minions** - Embora normalmente a nomenclatura em diversos serviços de tecnolocia seja Node, o Kubernetes prefere chamar de Minions os hosts que fazem parte de um Cluster gerenciado pelo próprio Kubernetes. Este minion pode ser um servidor físico ou virtual, necessitando possuir um serviço de gerenciamento de containers, como o Docker, por exemplo;

* **ETCD** - Embora este seja um serviço independente, estou listando-o aqui pois este será fundamental em seu ciclo de desenvolvimento com o Kubernetes. Cada Minion deverá rodar o <a href="https://coreos.com/etcd/docs/latest/" target="_blank">ETCD</a> (serviço de comunicação e gerenciamento de configurações no formato par de Chave/Valor). O ETCD é utilizado para troca e armazenamento de informações sobre os containers, pods, minions, etc.

* **Pods** - São grupos de containers (um ou mais) rodando em um único minion do cluster. Cada Pod receberá um endereço IP único no Cluster como forma de possibilitar a utilização de portas sem a necessidade de se preocupar com conflitos;

* **Labels** - São informações de identificação na configuração e gerenciamento dos objetos (como Pods ou Minions) formados de pares "chave:valor";

* **Controllers** - Além do Master Controller, dependendo do tamanho de sua infraestrutura e quantidade de Pods e Minions, você pode optar por ter mais de um Controller para dividir a carga e tarefas de gerenciamento. Os Controllers gerenciam um grupo de pods e, dependendo do estado de configuração desejada, podem acionar outros Controllers para lidar com as replicações e escalonamento. Os Controllers também são responsáveis pela substituiçao de Pods, caso um entre em estado de falha.

## Instalação

**V**amos ao que interessa...

**N**ovamente estou supondo que você já possui alguma familiaridade com Containers, Docker e, por consequência, com GNU/Linux.

**E**estarei utilizando 4 servidores virtuais rodando CentOS 7 nos exemplos a seguir, mas fica a seu critério decidir quantos utilizar.

*Certamente você optar por utilizar outra distribuição, seja Debian, Ubuntu, etc.. Uma vez que optei pelo CentOS 7, estarei utilizando comandos voltados para esta distro, mas sinta-se livre para adaptar seus comandos, como substituir o "yum" pelo "apt-get", "pacman", etc..*

**E**m minha configuração chamarei os servidores da seguinte forma:

* centos-master
* centos-minion1
* centos-minion2
* centos-minion3

**A** primeira coisa que se deve fazer sempre que se pensa em trabalhar com clusters, independente de ser um cluster de containers ou não, é ter a certeza de que os servidores terão uma correta sincronização de relógios entre si. A forma mais simples e eficiente no nosso contexto é com a utilização do NTP, portanto comece instalando o NTP nos 4 servidores, bem como habilitando o serviço e iniciando-o:

```
# yum install -y ntp
```

```
# systemctl enable ntpd && systemctl start ntpd
```

**C**aso queira certificar-se de que o serviço está realmente rodando:

```
# systemctl status ntpd

● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2017-06-24 17:46:02 UTC; 3s ago
  Process: 1586 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 1587 (ntpd)
   Memory: 2.1M
   CGroup: /system.slice/ntpd.service
           └─1587 /usr/sbin/ntpd -u ntp:ntp -g
```

**P**inga?

**É** importante nos certificarmos de que os servidores conseguem se comunicar e de que conseguem resolver nomes corretamente.

**N**este exemplo, conforme informado mais acima, estamos utilizando 4 servidores com os seguintes nomes: *centos-master*, *centos-minion1*, *centos-minion2* e *centos-minion3*, portanto vamos editar o arquivo **/etc/hosts** de cada um deles para que possam se comunicar pelos nomes que desejamos:

**Insira as seguintes linhas no arquivo /etc/hosts dos 4 servidores:**

*Lembre-se de substituir os IPs pelos IPs dos servidores em seu ambiente*

```
# Ip local do servidor master
172.31.22.126   centos-master

# Ip local do minion1
172.31.120.16   centos-minion1

# Ip local do minion2
172.31.25.6     centos-minion2

# Ip local do minion3
172.31.123.22   centos-minion3
```

Feito isto, tente pingar do master para os 3 minions utilizando os nomes especificados no /etc/hosts:

```
[root@kalib1 ~]# ping centos-minion1
PING centos-minion1 (172.31.120.16) 56(84) bytes of data.
64 bytes from centos-minion1 (172.31.120.16): icmp_seq=1 ttl=64 time=1.06 ms

[root@kalib1 ~]# ping centos-minion2
PING centos-minion2 (172.31.25.6) 56(84) bytes of data.
64 bytes from centos-minion2 (172.31.25.6): icmp_seq=1 ttl=64 time=0.588 ms

[root@kalib1 ~]# ping centos-minion3
PING centos-minion3 (172.31.123.22) 56(84) bytes of data.
64 bytes from centos-minion3 (172.31.123.22): icmp_seq=1 ttl=64 time=1.24 ms
```

**V**ocê pode realizar o mesmo teste a partir dos minions, pingando entre si e também para o centos-master.

**U**ma vez que tenhamos certeza de que todos os hosts se comunicam, é hora de instalar mais alguns pacotes necessários.

**P**rimeiramente, vamos configurar o repositório do Docker para o CentOS 7:

- Configurações de repositório retiradas dos repositórios CBS do Centos: https://cbs.centos.org/repos/virt7-docker-common-release/

**V**amos criar o seguinte arquivo de repositório:

```
# vim /etc/yum.repos.d/virt7-docker-common-release.repos
```

**O** conteúdo deste arquivo será o seguinte:

```
[virt7-docker-common-release]
name=virt7-docker-common-release
baseurl=https://cbs.centos.org/repos/virt7-docker-common-release/x86_64/os/
gpgcheck=0
```

*Este arquivo deverá ser criado nos 4 servidores.*

**E**m seguida, vamos atualizar a nossa base de repositórios e pacotes, também **nos 4 servidores**, bem como habilitar o novo repositório para instalar os pacotes docker e kubernetes:

```
# yum update
```

```
# yum install -y --enablerepo=virt7-docker-common-release docker kubernetes
```

**C**omo dito na introdução, também precisaremos do etcd para o armazenamento e troca de configurações, portanto vamos instalá-lo também nos 4 hosts:

```
# yum instal -y etcd
```

## Configuração

**V**amos começar com a configuração básica dos serviços envolvidos. Primeiramente, vamos abrir o arquivo de configuração do kubernetes e fazer algumas alterações:

*/etc/kubernetes/config*

**N**o arquivo config altere as seguintes linhas:

*Edite o valor do parâmetro KUBE_MASTER, de forma que nosso master possa ser encontrado pelo nome que definimos no hosts file. O valor original é "--master=https://127.0.0.1:8080", portanto mudaremos para o seguinte:*

```
KUBE_MASTER="--master=https://centos-master:8080"
```

**A**inda neste arquivo de configuração, vamos inserir a configuração do serviço ETCD, portanto inclua a seguinte linha ao final do arquivo:

```
KUBE_ETCD_SERVERS="--etcd-servers=https://centos-master:2379"
```

**S**eu arquivo de configuração deverá estar similar a este:

```
###
# kubernetes system config
#
# The following values are used to configure various aspects of all
# kubernetes services, including
#
#   kube-apiserver.service
#   kube-controller-manager.service
#   kube-scheduler.service
#   kubelet.service
#   kube-proxy.service
# logging to stderr means we get it in the systemd journal
KUBE_LOGTOSTDERR="--logtostderr=true"

# journal message level, 0 is debug
KUBE_LOG_LEVEL="--v=0"

# Should this cluster be allowed to run privileged docker containers
KUBE_ALLOW_PRIV="--allow-privileged=false"

# How the controller-manager, scheduler, and proxy find the apiserver
KUBE_MASTER="--master=https://centos-master:8080"

KUBE_ETCD_SERVERS="--etcd-servers=https://centos-master:2379"
```

**R**epita esta mesma configuração nos 4 hosts. Todos eles devem utilizar exatamente os mesmos valores utilizados aqui, apontando KUBE_MASTER e KUBE_ETCD_SERVERS para centos-master, visto que este será o responsável por gerenciar todos os nossos minions.

**U**ma vez que o arquivo de configuração do kubernetes esteja pronto nos 4 hosts, vamos configurar o serviço de API do kubernetes:

*/etc/kubernetes/apiserver*

**Esta configuração abaixo será apenas para o Master.**

*Edite o valor do parâmetro KUBE_API_ADDRESS, que originalmente é "--insecure-bind-address=127.0.0.1", de forma que possamos novamente receber comunicação dos demais hosts.*

```
KUBE_API_ADDRESS="--address=0.0.0.0"
```

*Descomente as linhas KUBE_API_PORT e KUBELET_PORT, para que possamos estabelecer as portas de comunicação com a API:*

```
# The port on the local server to listen on.
KUBE_API_PORT="--port=8080"

# Port minions listen on
KUBELET_PORT="--kubelet-port=10250"
```

*Em nosso exemplo não utilizaremos o parâmetro KUBE_ADMISSION_CONTROL, o qual nos permite ter mais controles e restrições sobre quais nodes ou minios podem entrar em nosso ambiente, portanto vamos apenas comentar esta linha por enquanto:*

```
# default admission control policies
# KUBE_ADMISSION_CONTROL="--admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ServiceAccount,ResourceQuota"
```

*Nosso arquivo /etc/kubernetes/apiserver deverá estar assim:*

```
##
# kubernetes system config
#
# The following values are used to configure the kube-apiserver
#

# The address on the local server to listen to.
KUBE_API_ADDRESS="--address=0.0.0.0"

# The port on the local server to listen on.
KUBE_API_PORT="--port=8080"

# Port minions listen on
KUBELET_PORT="--kubelet-port=10250"

# Comma separated list of nodes in the etcd cluster
KUBE_ETCD_SERVERS="--etcd-servers=https://127.0.0.1:2379"

# Address range to use for services
KUBE_SERVICE_ADDRESSES="--service-cluster-ip-range=10.254.0.0/16"

# default admission control policies
# KUBE_ADMISSION_CONTROL="--admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ServiceAccount,ResourceQuota"

# Add your own!
KUBE_API_ARGS=""
```

**S**alve e feche o arquivo. Novamente, esta configuração deve ser feita apenas para o Master.

**A**gora vamos configurar o serviço ETCD:

*/etc/etcd/etcd.conf*

**Esta configuração abaixo será apenas para o Master.**

*Edite os valores dos parâmetros ETCD_LISTEN_CLIENT_URLS e ETCD_ADVERTISE_CLIENT_URLS, que originalmente apontam para localhost. Como desejamos que nosso etcd escute requisições dos demais hosts, altere para o seguinte:*

```
ETCD_LISTEN_CLIENT_URLS="https://0.0.0.0:2379"
...
...
ETCD_ADVERTISE_CLIENT_URLS="https://0.0.0.0:2379"
```

**N**ovamente, não é necessário alterar a configuração do etcd nos demais hosts, apenas no Master.

**U**ma vez que as configurações iniciais foram feitas, vamos habilitar e iniciar os serviços necessários **no Master**, sendo eles:

* etcd
* kube-apiserver
* kube-controller-manager
* kube-scheduler

```
# systemctl enable etcd kube-apiserver kube-controller-manager kube-scheduler
```

```
# systemctl start etcd kube-apiserver kube-controller-manager kube-scheduler
```

**O**s 4 serviços devem estar rodando. Para termos certeza, vamos checar o status dos mesmos:

```
# systemctl status etcd kube-apiserver kube-controller-manager kube-scheduler | grep "(running)"
   Active: active (running) since Sat 2017-06-24 21:45:37 UTC; 1min ago
   Active: active (running) since Sat 2017-06-24 21:46:13 UTC; 1min ago
   Active: active (running) since Sat 2017-06-24 21:44:25 UTC; 1min ago
   Active: active (running) since Sat 2017-06-24 21:44:25 UTC; 1min ago
```

**N**ovamente, estes serviços serão iniciados no Master, e não nos nodes/minions, visto que estes utilizarão outros serviços.

**A**gora vamos configurar o seguinte arquivo nos nodes/minions:

*/etc/kubernetes/kubelet*

**Este arquivo apenas deverá ser editado nos nodes/minions, não no Master.**

*Vamos alterar o valor do parâmetro KUBELET_ADDRESS para que aceite comunicação não apenas do localhost:*

```
KUBELET_ADDRESS="--address=0.0.0.0"
```

*Descomentaremos também a linha KUBELET_PORT, para que possamos ter uma porta definida para a comunicação do kubelet:*

```
# The port for the info server to serve on
KUBELET_PORT="--port=10250"
```

*Vamos alterar o valor do parâmetro KUBELET_HOSTNAME para o nome que definimos **para cada um dos minions**, portanto em cada um deles este será um valor diferente. Supondo que este seja o minion1, utilizaremos:*

```
KUBELET_HOSTNAME="--hostname-override=centos-minion1"
```

*Vamos também alterar o valor para KUBELET_API_SERVER, apontando para o nosso Master:*

```
KUBELET_API_SERVER="--api-servers=https://centos-master:8080"
```

*Vamos comentar a linha KUBELET_POD_INFRA_CONTAINER, visto que não utilizaremos uma infraestrutura de containers externa, pois estaremos utilizando nossos próprios PODs e containers:*

```
# pod infrastructure container
#KUBELET_POD_INFRA_CONTAINER="--pod-infra-container-image=registry.access.redhat.com/rhel7/pod-infrastructure:latest"
```

*Nosso arquivo deverá estar assim: (Lembrando que o parâmetro KUBELET_HOSTNAME deverá ser diferente para cada um dos 3 minions, respectivamente: centos-minion1, centos-minion2 e centos-minion3)*

```
###
# kubernetes kubelet (minion) config

# The address for the info server to serve on (set to 0.0.0.0 or "" for all interfaces)
KUBELET_ADDRESS="--address=0.0.0.0"

# The port for the info server to serve on
KUBELET_PORT="--port=10250"

# You may leave this blank to use the actual hostname
KUBELET_HOSTNAME="--hostname-override=centos-minion1"

# location of the api-server
KUBELET_API_SERVER="--api-servers=https://centos-master:8080"

# pod infrastructure container
#KUBELET_POD_INFRA_CONTAINER="--pod-infra-container-image=registry.access.redhat.com/rhel7/pod-infrastructure:latest"

# Add your own!
KUBELET_ARGS=""
```

**U**ma vez que estas configurações também estão feitas nos 3 minions, vamos habilitar e iniciar os serviços necessários nos minions:

* kube-proxy
* kube-kubelet
* docker

```
# systemctl enable kube-proxy kubelet docker
```

```
# systemctl start kube-proxy kubelet docker
```

**N**ovamente, vamos ter certeza de que os 3 serviços estão rodando:

```
# systemctl status kube-proxy kubelet docker | grep "(running)"
   Active: active (running) since Sat 2017-06-24 21:44:23 UTC; 1h 16min ago
   Active: active (running) since Sat 2017-06-24 21:44:27 UTC; 1h 16min ago
   Active: active (running) since Sat 2017-06-24 21:44:27 UTC; 1h 16min ago
```

**N**ovamente, estes 3 serviços devem ser habilitados e iniciados nos 3 minions.

**N**este momento já temos nosso cluster rodando, com um master e 3 minions. :D

## Testando o Cluster com o Kubernetes

**A**gora que temos a configuração básica de nosso Master Controller e de 3 minions, vamos testar nosso cluster.

**U**tilizaremos o utilitário kubectl (KubeControl) disponível com o kubernetes. Caso tenha interesse em ver os parâmetros e funções do mesmo... *$ man kubectl*

**V**amos verificar a lista dos nodes ou minions que temos neste momento registrados em nosso Cluster. Vamos digitar alguns comandos em nosso Master Controller (centos-master):

```
[root@kalib1 ~]# kubectl get nodes
NAME             STATUS    AGE
centos-minion1   Ready     17m
centos-minion2   Ready     15m
centos-minion3   Ready     10m
```

**O**s três nodes criados e configurados anteriormente já são reconhecidos pelo nosso Kubernetes através do Master Controller. Além de registrados, estão com o status Ready, o que indica que estão prontos para funcionar e executar o que precisarmos.

*Caso deseje conhecer mais parâmetros que a função **get** do kubectl possui, podemos invocar o manual desta função: $ man kubectl-get*

**A**lém do status, podemos conseguir diversas outras informações dos nodes através do *kubectl*: *(Ex: kubectl describe nodes)* Isto lhe daria informações sobre todos os nodes. Vamos experimentar com um node em específico.

```
[root@kalib1 ~]# kubectl describe node centos-minion1
Name:                   centos-minion1
Role:
Labels:                 beta.kubernetes.io/arch=amd64
                        beta.kubernetes.io/os=linux
                        kubernetes.io/hostname=centos-minion1
Taints:                 <none>
CreationTimestamp:      Tue, 20 Jun 2017 19:27:31 +0000
Phase:
Conditions:
  Type                  Status  LastHeartbeatTime                       LastTransitionTime                      Reason     Message
  ----                  ------  -----------------                       ------------------                      ------     -------
  OutOfDisk             False   Sun, 25 Jun 2017 01:39:38 +0000         Fri, 23 Jun 2017 17:31:44 +0000         KubeletHasSufficientDisk    kubelet has sufficient disk space available
  MemoryPressure        False   Sun, 25 Jun 2017 01:39:38 +0000         Tue, 20 Jun 2017 19:27:31 +0000         KubeletHasSufficientMemory  kubelet has sufficient memory available
  DiskPressure          False   Sun, 25 Jun 2017 01:39:38 +0000         Tue, 20 Jun 2017 19:27:31 +0000         KubeletHasNoDiskPressure    kubelet has no disk pressure
  Ready                 True    Sun, 25 Jun 2017 01:39:38 +0000         Fri, 23 Jun 2017 17:31:54 +0000         KubeletReady                        kubelet is posting ready status
Addresses:              172.31.120.16,172.31.120.16,centos-minion1
Capacity:
 alpha.kubernetes.io/nvidia-gpu:        0
 cpu:                                   1
 memory:                                1015348Ki
 pods:                                  110
Allocatable:
 alpha.kubernetes.io/nvidia-gpu:        0
 cpu:                                   1
 memory:                                1015348Ki
 pods:                                  110
System Info:
 Machine ID:                    f9afeb75a5a382dce8269887a67fbf58
 System UUID:                   EC2C8A0E-91D6-F54E-5A49-534A6A903FDA
 Boot ID:                       20961efd-c946-481a-97cb-7788209551ae
 Kernel Version:                3.10.0-327.28.2.el7.x86_64
 OS Image:                      CentOS Linux 7 (Core)
 Operating System:              linux
 Architecture:                  amd64
 Container Runtime Version:     docker://1.12.6
 Kubelet Version:               v1.5.2
 Kube-Proxy Version:            v1.5.2
ExternalID:                     centos-minion1
Non-terminated Pods:            (0 in total)
  Namespace                     Name            CPU Requests    CPU Limits      Memory Requests Memory Limits
  ---------                     ----            ------------    ----------      --------------- -------------
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.
  CPU Requests  CPU Limits      Memory Requests Memory Limits
  ------------  ----------      --------------- -------------
  0 (0%)        0 (0%)          0 (0%)          0 (0%)
Events:
  FirstSeen     LastSeen        Count   From                            SubObjectPath   Type            Reason             Message
  ---------     --------        -----   ----                            -------------   --------        ------             -------
  15m           15m             1       {kubelet centos-minion1}                        Normal          Starting           Starting kubelet.
  15m           15m             1       {kubelet centos-minion1}                        Warning         ImageGCFailed      unable to find data for container /
  15m           15m             2       {kubelet centos-minion1}                        Normal          NodeHasSufficientDisk       Node centos-minion1 status is now: NodeHasSufficientDisk
  15m           15m             2       {kubelet centos-minion1}                        Normal          NodeHasSufficientMemory     Node centos-minion1 status is now: NodeHasSufficientMemory
  15m           15m             2       {kubelet centos-minion1}                        Normal          NodeHasNoDiskPressure       Node centos-minion1 status is now: NodeHasNoDiskPressure
  15m           15m             1       {kubelet centos-minion1}                        Warning         Rebooted           Node centos-minion1 has been rebooted, boot id: 20961efd-c946-481a-97cb-7788209551ae
```

**O**bviamente recebemos um retorno com muitas informações em formato Json, o que nem sempre é como esperamos. Existem formas de filtrar os resultados e conseguir informações mais precisas, como o bom e velho grep:

```
[root@kalib1 ~]# kubectl describe node centos-minion1 | grep Addresses
Addresses:              172.31.120.16,172.31.120.16,centos-minion1
```

**V**ocê também pode utilizar expressões regulares e a sintaxe do próprio Kubernetes para consultas mais complexas, como por exemplo, formatar a minha saída Json de forma a pegar apenas a listagem de status dos meus nodes que estão com Ready = True:

```
[root@kalib1 ~]# kubectl get nodes -o jsonpath='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}'| tr ';' "\n" | grep "Ready=True"

Ready=True
Ready=True
Ready=True
```

**A** sua criatividade é o limite. ;]

**N**ão temos nenhum pod configurado, mas também poderíamos utilizar *kubectl get* para conseguir a listagem de nossos pods:

```
[root@kalib1 ~]# kubectl get pods

No resources found.
```

## Criando pods

**A**ssim como com o Docker, Ansible e algumas outras ferramentas, utilizaremos a linguagem <a href="ttps://pt.wikipedia.org/wiki/YAML" target="_blank">YAML]</a> para criar nossos arquivos de configuração.

**C**riaremos um diretório chamado *Builds* em nosso Master Controller apenas para melhor organizar nossos arquivos de configuração e ficar mais fácil encontrá-los no futuro:

```
# mkdir Builds

# cd Builds
```

**P**ara criarmos Pods, o que fazemos na verdade é criar arquivos de configuração que vão dizer ao Kubernetes qual o estado em que desejamos nossa infraestrutura. O papel do Kubernetes é ler esta configuração e assegurar que o estado de nossa infraestrutura reflita o estado desejado.

**P**ara facilitar, vamos utilizar exemplos encontrados na própria documentação do Kubernetes. Comecemos com a criação de um Pod para um servidor web Nginx.

**V**amos criar um arquivo chamado nginx.yaml dentro do diretório Builds que criamos anteriormente:

```
# vim nginx.yaml
```

**N**o arquivo indicaremos alguns atributos ou variáveis, bem como seus respectivos valores:

* apiVersion - Indica a versão da API do kubernetes utilizada
* kind - o tipo de recurso que desejamos
* metadata - dados referentes ao recurso desejado
* spec - especificações sobre o que este recurso irá conter

**V**amos criar um Pod contendo um único container rodando a versão 1.7.9 do nginx bem como disponibilizando a porta 80 para receber conexões. Este deverá ser o conteúdo do arquivo *nginx.yaml*:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.7.9
      ports:
      - containerPort: 80
```

**A**ntes de executarmos, vamos nos certificar novamente de duas coisas:

* Que realmente não temos nenhum Pod criado e ativo;
* Que não temos nenhum container rodando em nossos nodes.

*No centos-master:*

```
[root@kalib1 Builds]# kubectl get pods

No resources found.
```

*No centos-minion1: (Execute o mesmo comando nos demais nodes (centos-minion2 e centos-minion3))*

```
[root@kalib2 ~]# docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

*Novamente: Se você não faz ideia do que acabei de digitar, (docker ps) volte e leia um pouco sobre [Docker](/blog/2015/08/20/docker-uma-alternativa-elegante-para-containers-no-linux/) antes de seguir com este artigo.*

**C**omo podemos ver, não temos nenhum Pod, bem como nenhum container rodando em nossos nodes.

**V**amos utilizar *kubectl create* para criar o Pod utilizando o arquivo que criamos *nginx.yaml*: *Executaremos este comando no Master Controller - centos-master*

```
[root@kalib1 Builds]# kubectl create -f nginx.yaml

pod "nginx" created
```

**O** Kubernetes está dizendo que nosso Pod "nginx" foi criado. Vamos verificar:

```
[root@kalib1 Builds]# kubectl get pods

NAME      READY     STATUS    RESTARTS   AGE
nginx     1/1       Running   0          1m
```

**O** pod está criado e rodando. Agora, execute novamente *docker ps* nos 3 nodes para identificar em qual deles o container foi criado. Sim, como não especificamos nada, o Kubernetes vai verificar os recursos disponíveis no momento e vai lançar onde ele achar mais adequado.

```
[root@kalib4 ~]# docker ps

CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS              PORTS               NAMES
6de8e22e1536        nginx:1.7.9                                "nginx -g 'daemon off"   2 minutes ago       Up 2 minutes                            k8s_nginx.b0df00ef_nginx_default_d4debd3a-594c-11e7-b587-06827a5b32d4_583881e0
ae49b36ae11b        gcr.io/google_containers/pause-amd64:3.0   "/pause"                 2 minutes ago       Up 2 minutes                            k8s_POD.b2390301_nginx_default_d4debd3a-594c-11e7-b587-06827a5b32d4_fb5c834f
```

**S**im, existem dois containers rodando. Um deles é o nosso "nginx", enquanto que o outro é um container padrão do google chamado "/pause", o qual será responsável pela manutenção de alguns recursos de nosso cluster.

**P**odemos novamente pedir a descrição deste pod que acabamos de criar:

```
[root@kalib1 Builds]# kubectl describe pod nginx

Name:           nginx
Namespace:      default
Node:           centos-minion3/172.31.123.22
Start Time:     Sun, 25 Jun 2017 02:20:18 +0000
Labels:         <none>
Status:         Running
IP:             172.17.0.2
Controllers:    <none>
Containers:
  nginx:
    Container ID:               docker://6de8e22e153618271bb6e8095c68070126541331c8acfc3f5d1a654f4b978454
    Image:                      nginx:1.7.9
    Image ID:                   docker-pullable://docker.io/nginx@sha256:e3456c851a152494c3e4ff5fcc26f240206abac0c9d794affb40e0714846c451
    Port:                       80/TCP
    State:                      Running
      Started:                  Sun, 25 Jun 2017 02:20:27 +0000
    Ready:                      True
    Restart Count:              0
    Volume Mounts:              <none>
    Environment Variables:      <none>
Conditions:
  Type          Status
  Initialized   True
  Ready         True
  PodScheduled  True
No volumes.
QoS Class:      BestEffort
Tolerations:    <none>
Events:
  FirstSeen     LastSeen        Count   From                            SubObjectPath           Type            Reason     Message
  ---------     --------        -----   ----                            -------------           --------        ------     -------
  6m            6m              1       {default-scheduler }                                    Normal          Scheduled  Successfully assigned nginx to centos-minion3
  6m            6m              1       {kubelet centos-minion3}        spec.containers{nginx}  Normal          Pulling    pulling image "nginx:1.7.9"
  6m            6m              2       {kubelet centos-minion3}                                Warning         MissingClusterDNS   kubelet does not have ClusterDNS IP configured and cannot create Pod using "ClusterFirst" policy. Falling back to DNSDefault policy.
  6m            6m              1       {kubelet centos-minion3}        spec.containers{nginx}  Normal          Pulled     Successfully pulled image "nginx:1.7.9"
  6m            6m              1       {kubelet centos-minion3}        spec.containers{nginx}  Normal          Created    Created container with docker id 6de8e22e1536; Security:[seccomp=unconfined]
  6m            6m              1       {kubelet centos-minion3}        spec.containers{nginx}  Normal          Started    Started container with docker id 6de8e22e1536
```

**O**bviamente que isto é apenas a configuração mais básica que se possa imaginar, sem storage, mapeamentos de portas, redirecionamentos, rotas, etc. A ideia é apenas uma apresentação inicial..o que é o Kubernetes.

**H**appy Hacking
