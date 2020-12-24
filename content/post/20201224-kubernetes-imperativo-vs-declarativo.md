---
title: "Kubernetes: Imperativo vs Declarativo"
date: 2020-12-24T10:53:55-05:00
draft: false
image: "/img/kubernetes-ship.jpg"
tags: [
"Devops",
"Containers",
"Kubernetes",
]
description: "Um breve compartivo entre duas formas de gerenciar seus recursos no Kuberntes: Declarativo vs Imperativo. O kubectl é mais poderoso do que você imagina."
---

## O que é o que

**U**ma abordagem muito comum é a de que a capacidade de reproduzirmos coisas de forma simples e rápida é fundamental no sentido de agilidade e estabilidade. Se a ideia é termos uma infraestrutura cada vez mais descartável, a utilização de scripts ou códigos sempre que possível, preferivelmente armazenados em algum local seguro e gerenciável, como por exemplo um repositório no github, bitbucket, gitlab ou afins, é a forma correta de fazer as coisas.

**U**m ótimo exemplo disto é a criação de recursos de infraestrutura com o Terraform, utilizando-se o princípio de Infraestrutura como Código. Ao codificarmos toda a nossa infraestrutura, torna-se fácil a restauração da mesma em casos de incidentes, indisponibilidade ou até mesmo escalonamento quando necessário. Uma vez que tenhamos tudo em código e devidamente armazenado em um repositório, fica fácil também o repasse de informações e continuidade de trabalho por outros membros da equipe em caso de você tirar férias ou deixar a empresa. Neste tipo de cenário costuma-se dizer que o código é a própria documentação da infraestrutura, sempre fiel e atualizada automagicamente.

**C**om o Kubernetes não seria diferente, certo?!

**C**aso não tenha um entendimento básico sobre o que é e como funciona o Kubernetes, sugiro a leitura deste outro post (<a href="/conhecendo-o-kubernetes-clusters-de-containers/" target="_blank">Conhecendo O Kubernetes - Clusters De Containers</a>) antes de dar continuidade neste.

**O** Kubernetes faz utilização da linguagem yaml para a criação e gerenciamento de seus recursos em formato de código. Desta forma, um simples exemplo para a criação de um pod rodando um servidor web nginx poderia ser um único arquivo chamado nginx-pod.yaml, da seguinte forma:

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - image: nginx
      name: nginx
      ports:
        - containerPort: 80
          name: http
```

**E**ste é um típico exemplo de manifesto ou arquivo de configuração para recursos do Kubernetes. Neste caso, temos toda a definição do pod nginx em um arquivo yaml. Isto é o que chamamos de forma **Declarativa**, onde declaramos tudo o que esperamos para nosso pod e, ao aplicarmos este código, o Kubernetes apenas irá ler nossas definições e criar exatamente como apresentado ou _declarado_.

**A** outra forma de se criar/gerenciar recursos no Kubernetes seria a **Imperativa**, na qual ao invés de criarmos um arquivo declarando o que queremos, simplesmente damos uma ordem (_imperativa_) ao Kubernetes, informando o que ele deve fazer. Algo como: _Kubernetes, crie um pod rodando uma imagem nginx com a porta 80 disponível._

## Kubernetes de forma Declarativa

**C**omo apresentado mais acima, a forma declarativa nada mais é do que uma forma instruída com declarações ou passos que devem ser seguidos, através de um arquivo yaml, no caso do Kubernetes.

**S**e pegarmos um cluster Kubernetes limpo, podemos ver que nenhum pod está rodando utilizando o comando _kubectl get pods_:

```yaml
➜ kubectl get pods
No resources found in default namespace.
```

**E**m seguida, criaremos um arquivo chamado _nginx-pod.yaml_ com o seguinte conteúdo:

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - image: nginx
      name: nginx
      ports:
        - containerPort: 80
          name: http
```

**A**gora podemos de fato criar o nosso pod através da _declaração_ que incluímos no arquivo _nginx-pod.yaml_. Para isto utilizaremos o comando _kubectl apply -f nginx-pod.yaml_:

```yml
➜ kubectl apply -f nginx-pod.yaml
pod/nginx created
```

**A**parentemente tudo ocorreu bem e podemos agora confirmar que nosso pod foi criado através do comando _kubectl get pods_ novamente:

```yml
➜ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          44s
```

**D**esta mesma forma podemos interagir e criar todos os recursos necessários para nosso cluster, sejam deployments, replicasets, volumes, etc. Por hora, vamos deletar o pod criado anteriormente utilizando o comando _kubectl delete pod nginx_ e em seguida vamos confirmar o sucesso do delete com _kubectl get pods_:

```yml
➜ kubectl delete pod nginx
pod "nginx" deleted
```

```yml
➜ kubectl get pods
No resources found in default namespace.
```

## Kubernetes de forma Imperativa

**D**a mesma forma que fizemos de forma declarativa, as APIs do Kubernetes nos permitem uma abordagem mais direta e imperativa sem a necessidade de arquivos de configuração ou manifestos em formato yaml. Neste tipo de abordagem tudo o que precisamos fazer é dizer o que queremos que seja feito, e o Kubernetes assumirá a responsabilidade de definir o que deverá ser feito para atingir o resultado esperado.

**S**eguindo o mesmo exemplo de um simples pod rodando nginx, poderíamos simplesmente utilizar o comando _kubectl run --image=nginx nginx_ e em seguida confirmarmos a criação do pod com _kubectl get pods_:

```yml
➜ kubectl run --image=nginx nginx
pod/nginx created
```

```yml
➜ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          15s
```

**O** resultado foi exatamente o mesmo, porém não utilizamos nenhum arquivo yaml para tal, o que faz com que esta abordagem acabe sendo, em muitos casos, mais simples e rápida, certo?!

**A**lém da criação do recurso em si, a abordagem imperativa lhe permite também criar uma forma híbrida de se trabalhar, onde você utilizará a abordagem imperativa como uma forma de facilitar a sua vida com a abordagem declarativa. Sim, é isso mesmo que você acabou de ler, eu não estou louco.

**A** maior desvantagem da abordagem declarativa, em meu ver, é justamente a necessidade de digitar todas aquelas linhas de código yaml. Nem sempre conseguimos memorizar cada linha e sempre temos de ir à documentação para definir o que precisamos em nosso arquivo. Com a abordagem imperativa, temos a capacidade de criar arquivos yaml declarativos de forma rápida e automatizada. Assim, ao invés de precisarmos memorizar todas as linhas, precisamos apenas alterar as linhas que nos interessam. É basicamente uma forma simples de gerar templates rapidamente.

Por exemplo, no caso de nosso pod nginx, vamos iniciar deletando o pod criado com _kubectl delete pod nginx_ novamente:

```yml
➜ kubectl delete pod nginx
pod "nginx" deleted
```

**A**gora vamos criar um novo arquivo yaml, chamado _nginx-pod2.yaml_. Porém, desta vez, não criaremos o código manualmente. Utilizaremos a forma imperativa para gerar este código, através do comando _kubectl run nginx --image=nginx --dry-run=client -o yaml_:

```yml
➜ kubectl run nginx --image=nginx --dry-run=client -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

**P**odemos ver que o comando nos jogou toda a declaração para um pod nginx, porém não era isso que queríamos, certo?! Queríamos que um novo arquivo fosse criado. Mas é importante notar que o comando nos apresentou toda a declaração necessária, o que pode nos servir de referência apenas para validação ou em caso de dúvida sobre algo. Agoras podemos incluir _> nginx-pod2.yaml_ ao final do nosso comando anterior e o mesmo irá de fato jogar este conteúdo para um novo arquivo, assim como desejamos:

```yml
➜ kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod2.yaml
```

**A**gora, com nosso novo arquivo criado, podemos criar nosso pod exatamente como fizemos anteriormente com o método declarativo, porém apontando para o novo arquivo _nginx-pod2.yaml_:

```yml
➜ kubectl apply -f nginx-pod2.yaml
pod/nginx created
```

```yml
➜ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          6s
```

**N**osso pod foi novamente criado, exatamente da mesma forma. Sim, o arquivo yaml que foi criado automaticamente possui algumas linhas a mais, pois foi criado utilizando o template padrão do kubernetes, mas nenhuma delas afetará o funcionamento do seu pod. São linhas não obrigatórios, por isso conseguíamos criar o pod mesmo sem elas.

**D**a mesma forma podemos fazer muitas outras coisas de forma imperativa:

```yml
# (Para criar um deployment)
kubectl create deployment --image=nginx nginx
```

```yml
# (para criar um service para expôr um deployment)
kubectl expose deployment nginx --port 80
```

```yml
# (para escalonar um deployment existente)
kubectl scale deployment nginx --replicas=10
```

```yml
# (para editar um deployment já existente)
kubectl edit deployment nginx
```

```yml
# (para setar uma nova versão de imagem a ser utilizada em um deployment existente)
kubectl set image deployment nginx nginx=nginx:2.10
```

etc.. etc... etc...

## Imperativo vs Declarativo

**S**endo assim, qual das duas é melhor? Qual devo utilizar?

**A** resposta mais simplista seria que ambas são boas e que você deveria aprender e se sentir confortável com ambas as alternativas, visto que elas se complementam.

**T**udo dependerá da situação e do que se deseja fazer. Como sempre costumo dizer, um código em um repositório git é sempre a melhor prática para continuidade de negócio, no entanto existem situações em que um simples comando imperativo lhe trará resultados mais rápidos como em testes simples, automação em pipelines, validação de comandos, geração de templates ou mesmo durante exames de certificação, como o da certificação CKA (Certified Kubernetes Administrator), no qual você possui uma série de questões para resolver e um tempo limitado para tal. Comandos imperativos vão economizar segundos preciosos durante o exame. (Sim, já vi muita gente falhando no exame e dizendo que não teve tempo o suficiente para todas as questões.)

**N**o mais, espero que a diferença entre ambas as abordagens tenha ficado clara. Este post não possui o intuito de ensinar a fundo nenhum dos comandos citados acima, mas sim a diferença entre ambas as abordagens no Kubernetes portanto não entrei em detalhes para cada comando.
