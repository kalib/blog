---
title: "Infraestrutura como Código com Terraform"
date: 2018-10-22T08:40:21-05:00
draft: false
image: "/img/terraform.png"
tags: [
"Devops",
"Terraform",
"Cloud",
"GCP",
"AWS",
]
description: "Uma introdução aos conceitos e benefícios de IAC, ou Infraestrutura como Código bem como uma breve apresentação da ferramenta Terraform, da Hashicorp."
---

## Infraestrutura como Código (IAC - Infrastructure as Code)

**I**nfraestrutura como código - IaC (ou infrastructure as code em inglês) - é o processo de gerenciamento e provisionamento de recursos de infraestrutura através de códigos ou arquivos de configuração que descrevem o estado desejado para tal infraestrutura ou recursos de infraestrutura. A principal característica de IaC é o uso de scripts ou definições declarativas ao invés de processos manuais, mas o termo é utilizado com mais frequência para promover abordagens declarativas. Como se tratam de arquivos de código, as definições podem ser armazenadas em um sistema de controle de versões, tal como o Git.

**A**bordagens IaC são comumente promovidas para computação em nuvem e, às vezes, são comercializadas como infraestrutura como serviço (infrastructure as a service, IaaS). IaC suporta IaaS, mas os dois conceitos não devem ser confundidos.

## IaC e DevOps

**I**aC, ou Infraestrutura como Código, é um conceito bastante ligado à filosofia DevOps, visto que com práticas de implementação de uma infraestrutura baseada em códigos declarativos podemos aproximar as equipes de Operações e Desenvolvimento, fazendo com que os desenvolvedores tornem-se mais envolvidos nas configurações de máquinas ou recursos de infraestrutura como um todo, enquanto que os profissionais de Operações se envolvem mais cedo no processo de desenvolvimento. Além disso, agora ambas as equipes podem armazenar seu código em um mesmo ambiente, como por exemplo repositórios Git.

**I**nfraestrutura como código mostrou-se uma excelente solução para livrar equipes de tarefas enfadonhas do cotidiano realizadas manualmente. Além de tomarem muito tempo e serem tarefas extremamente repetitivas, os corriqueiros processos manuais estão sujeitos a erros e podem colocar as operações em risco.

#### Algumas vantagens da utilização de IaC

- **Elimina tarefas repetitivas** - Se você precisa criar 3 clusters Kubernetes em seu provedor de cloud (GCP ou AWS, por exemplo), você não precisa repetir os mesmos passos 3 vezes. Escreve um bloco de código que define a criação de um cluster e poderá aplicar este mesmo código quantas vezes forem necessárias;

- **Documentação simplificada** - Não há necessidade de logar-se em um servidor ou provedor de cloud para tentar vasculhar tudo o que foi configurado (está tudo no código);

- **Reaproveitamento** - Uma vez que tudo está codificado e separado em módulos, fica fácil reaproveitar módulos e código para futuras implementações;

- **Simples manutenção** - Mudanças na configuração, versões, regras e demais definições podem ser implementadas e aplicadas rapidamente com pequenas alterações no código;

- **Versionamento** - Ao abordar nossa infraestrutura como código passamos a ter diversos benefícios já rotineiros para desenvolvedores, como por exemplo a possibilidade de gerenciar nosso código em sistemas de versionamento como o Git, de forma a facilitar o trabalho em equipes, controle de versões, mudanças, etc;

- **Agilidade** - Se preciso trocar a faixa de endereços IP de uma VPC ou subnet, alterar uma linha de código é muito mais rápido do que logar em uma dashboard, procurar tal recurso e alterar manualmente os valores desejados;

- **Possibilidade de soluções agnósticas** - Em um mundo tecnológico que muda constantemente não são raras as ocasiões em que temos de mudar completamente nossa infraestrutura, seja deixando de usar servidores físicos para passar a utilizar VMs, ou migrando de VMs locais para a nuvem, ou de VMs na nuvem para containers, etc. Independente de qual seja o cenário de mudança, uma vez que sua infraestrutura está definida em código, dependendo das ferramentas escolhidas para codificar sua infra, o mesmo código poderia ser utilizado para um ambiente VMWare, AWS, Azure, GCP, etc, com poucas modificações. (Já citei agilidade e Simples manutenção, certo?!);

- **Fácil replicar** - Em um ambiente não codificado ou automatizado, geralmente repetimos as mesmas configurações para criarmos ambientes distintos como Produção, Teste, Desenvolvimento, etc. Uma vez que sua infra está codificada, você aplica o mesmo código para criar quantos ambientes desejar;

- **Recuperação de Desastres ou Disaster Recovery** - Desastres acontecem. Imagine um problema grande em sua infraestrutura. Em um cenário de virtualização, imagine que seu host VMWare simplesmente parou de funcionar pois seu disco queimou. Ou que o storage onde se encontravam as suas VMs simplesmente foi destruído. Ou, em um ambiente de cloud, imagine que sua senha de administrador da nuvem vazou e seu ambiente foi completamente excluído. Ou mesmo que o próprio data center ou região na qual se encontra a sua infra estrutura teve algum problema sério e toda a sua infra caiu. Claro, as boas práticas já pregam há muito tempo que sempre devemos ter backups de todos os servidores e sistemas, mas backups nada mais são do que arquivos. E a infraestrutura de fato? Você precisa ter uma infraestrutura ativa antes de conseguir restaurar backups, certo? Rede, firewall, VPCs, Clusters, Servidores, etc.. Se você possui toda a sua infraestrutura em código, recuperar tudo isso é tão simples quanto executar um único comando.

- **Planejamento (plan) e Testes** - Práticas como planejamento e testes fazem parte (ou ao menos deveríam fazer) da rotina de praticamente qualquer desenvolvedor. Profissionais de infraestrutura sempre tiveram uma desvantagem em relação a isso, pois era complicado fazer testes de infraestrutura. Teste basicamente significava instalar exatamente a mesma infraestrutura em um ambiente isolado para testes. Ainda possível, porém pouco confiável e com altas chances de falhas, pois, por ser um processo manual e lento, não há qualquer garantia de que todos os mesmos passos serão executados no ambiente de produção tal como foram executados no ambiente de teste. Com infraestrutura como código, fica fácil utilizar-se das mesmas técnicas de planejamento e testes automatizados há muito utilizadas por desenvolvedores. Agora você consegue executar testes no código de sua infraestrutura que, literalmente, irão avaliar cada bloco de seu código e simular a execução de cada expressão ou descrição, dando-lhe assim uma visão geral sobre o que acontecerá, o que funcionará e o que falhará, garantindo uma integradade fiel entre teste e implantação em produção ou demais ambientes.

**L**indo, não? Lembro de meus tempos de faculdade, quando costumava dizer a meus colegas que todos os profissionais de TI deveríam saber programar, independente de desejarem ou não trabalhar com programação. Na ocasião, em meados de 2006, a maioria deles dizia que isso era loucura. "Porque aprender a programar se vou trabalhar com infraestrutura?" Bom, tudo o que posso dizer hoje é: Bem vindos à era DevOps.

**Q**uando falamos em infraestrutura como código, existem diversas ferramentas que trabalham em cima deste conceito, e muitas delas atuam juntas para englobar soluções mais completas, mas uma de minhas favoritas é o <a href="https://www.terraform.io" target="_blank">Terraform</a>, da <a href="https://www.hashicorp.com/" target="_blank">Hashicorp</a>.

## Escolha da ferramenta ideal para IAC

**C**om uma simples busca no Google pelo termo "ïnfraestrutura como código" ou "IAC", será extremamente fácil encontrar diversas ferramentas, dentre as mais populares estão Chef, Puppet, Ansible, SaltStack, Terraform, CloudFormation, etc.

**U**m problema comum para quem inicia no mundo DevOps ou simplesmente deseja começar a utilizar infraestrutura como código é justamente a escolha. Qual a melhor ferramenta? Qual devo utilizar?

**E**sta é uma dificuldade comum e inerante ao fato de se ter muitas opções. Se em uma sorveteria você só possui os sabores chocolate e baunilha, é extremamente simples optar por um, outro ou nenhum dos dois. No entanto, em uma sorveteria com 50 sabores, você provavelmente perderá alguns minutos apenas lendo todas as opções, do contrário irá apostar na sorte e escolher o primeiro que lhe parecer apetitoso, correndo o risco de descartar algum que não chegou a ver, mas que poderia ser muito melhor e refrescante para um dia quente como os do nordeste cearense.

**O** mais importante é sempre realizar uma pesquisa sobre pontos fortes e fracos de cada uma delas antes de tomar uma decisão, tendo em mente alguns aspectos:

- A escolha não deve (ou não deveria) ser puramente pessoal. A melhor ferramenta dificilmente será a que você gostou mais de utilizar. A melhor ferramenta será a que melhor atende as necessidades do seu projeto ou negócio;

- A análise deve ser feita em diversos aspectos, e não apenas em um ou dois. Supondo que você pesquise por exemplo quais possuem mais módulos gratuitos e quais delas possuem uma comunidade mais ativa na internet para dúvidas, mas esqueceu de ponderar o preço para ter acesso a suporte corporativo, você pode ter feito uma boa ou uma má escolha. Em caso de ser uma pequena empresa, com prazos relativamente longos para entregas de projetos e maior flexibilidade em termos de tempo fora do ar (downtime), o suporte corporativo pode não ser um fator decisivo. No entanto, em uma empresa de ambiente mais crítico, como um banco, demais sistemas financeiros, governamentais, etc., o suporte corporativo se torna um fator mais importante, portanto escolher uma ferramenta sem ponderar o valor de seu suporte corporativo acaba sendo um tiro no pé, o que reforça a ideia de que sempre devemos avaliar o máximo de aspectos possíveis e relevantes ao nosso projeto ou ambiente;

- Outro fator fundamental e, em meu ver o mais importante, é entender que não necessariamente a escolha será exclusiva. Imagine que você precisa montar uma mesa que veio toda desmontada: tábuas, parafusos, gavetas, etc. Você tem algumas ferramentas a disposição, como chave de fendas, martelo, régua, serra, furadeira, etc. Você pode ser uma espécie de rambo e gostar de resolver as coisas com uma ferramenta só e, sinceramente, você pode até ser capaz de conseguir montar a mesa inteira apenas utilizando o martelo, parabéns por isso. Mas será que essa é a forma mais eficiente de resolver o problema? Será que não seria mais rápido e organizado utilizando um martelo e uma chave de fendas? Afinal, temos parafusos também, certo?!

**C**onforme descrito acima, o ideal é sempre avaliar o projeto ou ambiente no qual se irá trabalhar, sem a necessidade de escolher apenas uma ferramenta.

**S**im, Chef, Puppet e Ansible são ferramentas de Infraestrutura como Código, no entanto elas possuem tarefas mais específicas, nas quais possuem mais desempenho, como por exemplo o gerenciamento de configurações, para não listar todas as suas funções.

**E**, claro, o CloudFormation é uma excelente ferramenta da Amazon para criação de infraestrutura como código, no entanto ela fica restrita ao ambiente de cloud da Amazon, o AWS. E se meu projeto estiver utilizando VMs em um ambiente VMWare? Ou se eu utilizar Google Cloud? Ou mesmo um ambiente mais heterogênio, com VMWare, Google Cloud e Amazon AWS? O CloudCloudFormation não seria a melhor opção, por ser restrito ao ambiente AWS.

**S**empre fui a favor de utilizar as ferramentas corretas para cada tarefa em específico, portanto porque utilizar apenas uma se tenho outras disponíveis?

**O** <a href="https://www.terraform.io" target="_blank">Terraform</a>, por outro lado, é específico para a criação da infraestrutura base, se saindo muito melhor do que Chef, Ansible ou Puppet nesta tarefa, mas nada impede (e eu encorajo e o faço) que você utilize Chef, Puppet ou Ansible, para gerenciar configurações, bootstraping ou deployments na infraestrutura criada pelo Terraform.

**A**lém do mais, diferente do CloudFormation, o Terraform é uma solução agnóstica, permitindo-lhe criar infraestrutura em praticamente qualquer ambiente, seja ele em Cloud (Amazon AWS, Microsoft Azure, Google GCP, IBM Cloud, Digital Ocean, etc.), ambiente virtualizado, local ou em Data Centers (VMWare, Xen, Virtual Box, etc.), Docker, Kubernetes, além de recursos diversos de infraestrutura e softwares, tais como Redes (CloudFlare, DNS, DNSimple, F5 BIG-IP, Palo Alto Networks,etc.), Bancos de Dados (InfluxDB, MySQl ou PostgreSQL), dentre muitas outras coisas.

## Terraform

**O** Terraform, da Hashicorp, lhe permite criar, alterar e melhorar sua infraestrutura de forma segura e previsível. É uma ferramenta Open Source que codifica APIs em arquivos de configuração declarativos que podem ser compartilhados entre membros de um time, tratados como código, editados, revisados e versionados.

![Terraform Flow](/img/terraform-flow.png)

**A** imagem acima descreve bem o fluxo básico da utilização de Terraform para codificar sua infraestrutura, na qual o fluxo mais simplista é:

- Escrever o código de sua infraestrutura;
- Planejar a execução do seu código, de forma que você receba informações antecipadamente de tudo o que acontecerá quando você aplicar o seu código;
- Crie uma infraestrutura reproduzível ao aplicar seu código.

**A**pesar de este ser o fluxo mais simplista, com a utilização de infraestrutura como código você pode melhorar seu fluxo inserindo colaboração e compartilhamento, armazenando e gereciando seu código em um repositório git, por exemplo, além de ter assim um registro completo das mudanças e evoluções de sua infraestrutura, facilitando a automação em fluxos mais complexos, como por exemplo em pipelines de Integração Contínua.

**O** Terraform funciona basicamente através de recursos, ou resources, que definem o tipo de infraestrutura você estará criando bem como seus atributos. Além disso, conforme dito anteriormente, o Terraform também pode ser utilizado em paralelo com diversas outras ferramentas de automação em forma de provedores, ou providers, como Puppet, Chef, Ansible, etc.

**P**or estarmos tentando aplicar para a infraestrutura um conceito que seja mais próximo do que já era utilizado por desenvolvedores há muito tempo, o Terraform possui também uma abordagem que lhe permite reaproveitamento de código, através de módulos. Existem diversos módulos criados e disponibilizados gratuitamente, mas você pode também criar seus próprios módulos de forma a melhor organizar e reaproveitar seu próprio código em diversos projetos.

**A**rquivos de configuração descrevem ao Terraform os componentes necessários para rodar uma única aplicação que representa todo o seu datacenter. O Terraform gera um plano de execução descrevendo o que fará para alcançar o estado desejado, e em seguida, caso aprovado, o executará para criar a infraestrutura desejada. Conforme a configuração muda, o Terraform será capaz de determinar o que mudou e criará planos de execução incrementais que podem ser aplicados.

**V**ejamos o seguinte diagrama que descreve uma simples infraesturura. Imaginemos que esta é a infraestrutura que queremos rodar em nossa conta no Google Cloud para termos um site de e-commerce:

![Kalib Ecommerce](/img/diagrama-ecommerce.png)

O diagrama acima possui diversos elementos:

1.  Uma organização no Google Cloud chamada Kalib Avante;
2.  Um diretório ou Folder (como chamado no Google Cloud) chamado Projetos;
3.  Um projeto chamado E-Commerce;
4.  Um projeto chamado Zebra Feliz;
5.  Dentro do projeto E-Commerce temos dois ambientes: Produção e Teste
6.  Repare que o projeto Zebra Feliz está incompleto e sem ambientes distintos, como Produção, teste, etc. Bom, trata-se de um projeto piloto ainda em desenvolvimento e planejamento, portanto os recursos não foram ainda criados por completo. Mas o Terraform nos permite incrementar recursos quando necessário, certo? Portanto, sem problemas com isto por enquanto.

**E**sta é a estrutura básica, já em termos de recursos temos Firewalls, clusters kubernetes com Nodes, buckets de storage para conservar o status ou state do Terraform e por consequência de sua infraestrutura, Discos persistentes, Container Registry (repositório de imagens Docker), VPCs, Load Balancer, VMs, etc.

**C**aso esteja se perguntando, sim o Terraform lhe permite criar esta infraestrutura inteira, bem como outras bem mais complexas, com mais projetos, mais ambientes, mais recursos, etc.

**D**esta forma podemos ter um código terraform dividido em alguns módulos, armazenado em um repositório Git, por exemplo, e criar toda essa infraestrutura, desde a Organização vazia, ao diretório de projetos, aos 2 projetos em si, buckets, clusters kubernetes, DNS, IAM, load balancer, VMs por trás do Load Balancer, etc. Tudo isto com um único comando:

```
terraform apply
```

**L**embrando um pouco do que falamos lá em cima, sobre ser simples reproduzir, ou se recuperar de desastres... imagine que uma região inteira caiu no Google, onde temos nossa infraestrutura. Sim, eu sei que isso é extremamente raro, mas vamos imaginar os cenários mais absurdos e raros também. Imagine que perdi toda a minha infraestrutura. Imagine ter que recriar tudo isso (projetos, diretórios, storages, DNS, IAM, clusters, Load Balancer, VMs, etc, etc..) manualmente..? Demoraria bastante, certo?! Mas, como fomos espertos e criamos tudo via terraform, um simples **terraform apply** irá criar tudo novamente para nós, exatamente como era antes.

**O** mesmo se dá caso precisemos recriar toda essa mesma infraestrutura em uma nova região do Google, ou caso queiramos destruir nossa infra e recriá-la em outra zona, por algum motivo.

**A**qui estamos lidando apenas com a Infraestrutura pois, conforme dito antes, o Terraform é excelente para criar a infraestrutura, mas o ideal ainda é utilizar outros softwares para provisionamento e deployment. Por exemplo, uma vez que temos nossa infraestrutura inteira criada, podemos começar a provisionar os sistemas e softwares através de outras ferrmentas, como Helm (para deployment dentro dos clusters Kubernetes), Chef, Puppet, Ansible, etc.

**A** ideia deste post é apenas dar uma introdução teórica, uma ideia de como infraestrutura como código funciona, e de como o Terraform é capaz de fazer tudo isso de forma segura e robusta. (Embora eu já veja uma enorme barra de rolagem aqui ao lado e sei que lhe fiz ler bastante, supondo que leu até aqui. :p)

**E**m meu próximo post pretendo fazer uma abordagem mais prática e com mão na massa, utilizando de fato o Terraform para criar uma simples infraestrutura via código.

**H**appy Hacking!
