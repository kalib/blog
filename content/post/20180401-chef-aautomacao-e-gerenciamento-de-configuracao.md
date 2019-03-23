---
title: "Chef: Aautomação E Gerenciamento De Configuração"
date: 2018-04-01T16:11:20-05:00
draft: false
image: "/img/cheflogo.png"
tags: [
"Devops",
"Chef",
]
description: "Chef é uma popular ferramenta de Gerenciamento de Configurações criado pela empresa de mesmo nome, Chef. O Chef é desenvolvido em Ruby e Erlang, utiliza uma linguagem DSL em Ruby puro para escrever arquivos de configuração de sistemas chamados recipes."
---
## Uma coisa de cada vez
**C**hef é uma popular ferramenta de Gerenciamento de Configurações criado pela empresa de mesmo nome, Chef. O <a href="https://www.chef.io" target="_Blank">Chef</a> é desenvolvido em <a href="https://www.ruby-lang.org" target="_blank">Ruby</a> e <a href="https://www.erlang.org" target="_blank">Erlang</a>, e utiliza uma linguagem DSL (domain-specific language) em Ruby puro para escrever arquivos de configuração de sistemas chamados "recipes" (receitas).

**A**ntes de falarmos sobre o Chef é importante entender primeiramente o conceito e a utilidade de ferramentas de Gerenciamento de Configuração.

## Gerenciamento de Configuração

**G**erenciamento de Configuração, ou CM (Configuration Management), é um processo de engenharia de sistemas que visa garantir a consistência entre ativos físicos e lógicos em um ambiente operacional. O processo de gerenciamento de configuração busca identificar e rastrear itens individuais de configuração, documentando capacidades funcionais e interdependências. Administradores, técnicos e desenvolvedores de sistemas podem utilizar ferramentas de Gerenciamento de Configuração para verificar o efeito que uma mudança em um item de configuração terá em outros sistemas.

**E**m palavras simples, o objetivo de uma ferramenta de gerenciamento de configuração é simplificar a vida de quem administra serviços e sistemas garantindo uma uniformidade no quesito configuração.

**C**omo exemplo prático e simplista, imagine um servidor web que será responsável por hospedar um pequeno site em php. Este servidor possuirá alguns atributos/aplicativos/configurações, tais como:

* Apache instalado;
* Alterações específicas nos arquivos de configuração do Apache;
* Serviço do Apache ativo e iniciado;
* Arquivos referentes ao site em si em um diretório específico;
* Permissões específicas atribuídas ao diretório específico do site;
* etc...

**C**onfigurar tudo isso manualmente em um único servidor é simples. Você poderia conectar-se via SSH no servidor, instalar o apache com o gerenciador de pacotes da distribuição utilizada, configurar o que for necessário no apache, iniciar o serviço, etc, etc, etc. Mas o que fazer quando sua infraestrutura cresce? Quando se quer maior disponibilidade do site, quando agora você roda este site em um cluster com 5 servidores?

**B**asicamente a mesma coisa, certo? Você pode se conectar em cada um dos servidores e repetir os mesmos passos. O problema se dá justamente nessa repetição de passos, onde você pode cometer erros, perder tempo, etc. Além disso, o que acontece se um colega seu modificar algo em um dos servidores e não lhe avisar? E se ele esquecer de replicar esta mudança nos demais?

**É** fácil achar diversas razões pelas quais torna-se difícil administrar e gerenciar configurações em ambientes mais complexos. A ideia por trás de uma ferramenta de Gerenciamento de Configuração é justamente reduzir esta complexidade, eliminando a necessidade de conectar-se manualmente à diversos servidores para aplicar as mesmas rotinas, passos e configurações.

**A**través de arquivos de texto podemos literalmente descrever o estado e as ações desejadas para nossos serviços e sistemas. Por exemplo: Posso dizer que possuo um grupo de servidores chamado WebServer, o qual contém 10 servidores com o Sistema Operacional CentOS 7. Posso incluir a informação de que preciso que todos eles estejam com a versão X do Apache instalada e que o serviço esteja ativo e rodando. Além disso, posso dizer que desejo que exista no diretório /var/www/meusite/ todo o conteúdo que está em um mapeamento de rede específico, ou mesmo em um repositório que possuo no github.

**A**o invés de me conectar em cada um dos 10 servidores para fazer tudo isso, um simples comando será o suficiente. O comando em específico dependerá da solução adotada, visto que existem diversas ferramentas de CM (Gerenciamento de Configuração). Mas, basicamente, ele irá ler o(s) arquivo(s) de "instruções" que nós definimos e saberá em quais servidores ele deverá instalar o Apache e configurar de acordo com o especificado, nos dando apenas o resultado final em forma de relatório simples.

**E** se alguém da equipe alterar um arquivo de configuração diretamente em um dos servidores? A ferramenta, em sua próxima execucação, irá identificar que o estado desejado para aquele meu grupo de servidores está diferente em um dos servidores. Ela então modificará aquele arquivo específico naquele servidor para que ele volte ao seu estado desejado.

**A**lém desta segurança, nós agora passamos a ter um ponto único de modificação. Ao desejarmos mudar algo, o faremos apenas no nosso "código", ao invés de o fazer nos 10 servidores manualmente.

**O** mesmo benefício se dá em caso de erros e falhas: um ponto único de correção.

**C**om este mecanismo de descrever o estado de nossa infraestrutura em arquivos de texto/código, entramos em um novo conceito: Infrastructure as Code, ou Infraestrutura como Código. Uma vez que temos nossa infraestrutura em formato de código, podemos literalmente versionar e gerenciar nossa infraestrutura com repositórios Git, por exemplo.

## O que é Chef?

**C**onforme dito mais acima, Chef é uma das mais populares ferramentas de Gerenciamento de Configuração disponíveis atualmente. É compatível e facilmente integrado à plataformas de computação em nuvem, tais como Internap, Amazon EC2, Google Cloud Platform, OpenStack, SoftLayer, Microsoft Azure e Rackspace, provendo e configurando servidores automaticamente.

**O** usuário escreve "recipes" (receitas) que descrevem como o Chef deve gerenciar aplicações, servidores e utilitários. Estas "recipes", as quais podem ser agrupadas em "cookbooks" (livros de receitas) descrevem uma série de recursos que devem estar em um determinado estado. Este recursos podem ser pacotes, serviços ou mesmo arquivos.

**C**hef pode rodar em um modo cliente/servidor ou standalone, com o chamado "chef-solo". No modo cliente/servidor, o cliente Chef envia uma série de atributos sobre o node ou cliente/host para o Chef server. O Chef server utiliza-se da ferramenta <a href="https://lucene.apache.org/solr" target="_blank">Solr</a> para indexar estes atributos e provê uma API na qual os clientes podem fazer consultas. As recipes podem fazer requisições à esta base de atributos e utilizar os dados resultantes para configurar o cliente ou node.

**E**mbora inicialmente o Chef fosse utilizado para gerenciar exclusivamente máquinas Linux, as versões mais atuais também suportam máquinas Windows.

**A** ideia para este post é dar uma breve introdução ao Chef, portanto não vou entrar em maiores detalhes do funcionamento por hora.

### Resources

**C**omo dito antes, um Resource é uma descrição de estado desejado para um determinado item. Estes resources são gerenciados através de recipes.

**U**m resource possui basicamente 4 componentes fundamentais que são definidos em um bloco de código Ruby:

* Resource Type - Tipo de resource (Pode ser um pacote, serviço, arquivo...)
* Resource Name - Nome do resource
* Resource Properties - Propriedades do resource
* Actions - Ações a serem aplicadas ao resource

**U**m exemplo de recipe para instalar o Apache em um servidor Ubuntu, por exemplo, seria o seguinte:

```
package 'httpd' do
    action :install
end
```

No exemplo acima temos o tipo de resource como sendo "package", o nome do resource como sendo "httpd" e a ação "install".

**O** que vai acontecer aqui? Simples de entender, certo? O pacote httpd (Apache) será instalado. Mas o que acontece caso o pacote httpd já esteja instalado?

**N**ada. Uma das características do Chef é a **idempotência**. Na matemática e ciência da computação, a idempotência é a propriedade que algumas operações têm de poderem ser aplicadas várias vezes sem que o valor do resultado se altere após a aplicação inicial. Ou seja, O Chef primeiramente confere se o estado desejado já está aplicado e, caso sim, ignora aquela instrução.

**N**ovamente... A ideia para este post é dar uma breve introdução ao Chef, portanto não vou entrar em maiores detalhes sobre os tipos de resources e suas possíveis ações. Vamos ao que interessa...

## Instalando o Chef

**C**onforme explicado acima, o Chef pode ser utilizado em modo cliente/servidor ou standalone. Para esta introdução utilizaremos o modo standalone ou local para simplificar as coisas.

**P**ara instalar podemos utilizar o gerenciador de pacotes da distribuição Linux que utilizamos ou baixando o chefdk (Development Kit) através da página de <a href="https://downloads.chef.io/chefdk" target="_blank">downloads do chef</a>.

### Arch Linux

**N**o meu caso, utilizarei o pacote <a href="https://aur.archlinux.org/packages/chef-dk/" target="_blank">chef-dk</a> existente para o Arch Linux, mas sinta-se livre para baixar diretamente no site e executar o pacote de acordo com sua distribuição.

**1- Baixar o pacote do AUR:**

```
$ wget https://aur.archlinux.org/cgit/aur.git/snapshot/chef-dk.tar.gz
```

**2- Descompactar e Compilar:**

```
$ tar -xvzf chef-dk.tar.gz

$ cd chef-dk

$ makepkg
```

**3- Instalar o pacote:**

```
$ sudo pacman -U chef-dk-2.5.3-1-x86_64.pkg.tar.xz
```

**4- Confirmar que deu tudo certo:***

```
$ chef --version

Chef Development Kit Version: 2.5.3
chef-client version: 13.8.5
delivery version: master (73ebb72a6c42b3d2ff5370c476be800fee7e5427)
berks version: 6.3.1
kitchen version: 1.20.0
inspec version: 1.51.21
```

### Centos, Debian, Ubuntu...

**N**o CentOS, Ubuntu, Debian ou outras distribuições, o procedimento será relativamente parecido, portanto vejamos como seria no caso do CentOS baixando o arquivo diretamente do site de downloads:

**1- Baixar o arquivo .rpm para Red Hat:** <a href="https://downloads.chef.io/chefdk" target="_blank">Aqui</a>

```
$ wget https://packages.chef.io/files/stable/chefdk/2.5.3/el/7/chefdk-2.5.3-1.el7.x86_64.rpm
```

**2- Instalar via RPM:**

```
$ sudo rpm -ivh chefdk-2.5.3-1.el7.x86_64.rpm
```

**3- Confirmar que deu tudo certo:**

```
$ chef --version

Chef Development Kit Version: 2.5.3
chef-client version: 13.8.5
delivery version: master (73ebb72a6c42b3d2ff5370c476be800fee7e5427)
berks version: 6.3.1
kitchen version: 1.20.0
inspec version: 1.51.21
```

## Testando o Chef Localmente

**P**ara facilitar o entendimento, vamos criar uma recipe simples para aplicarmos localmente.

**V**amos começar criando um arquivo chamado **exemplo.rb** com o seguinte conteúdo:

```
package 'apache' do
        package_name 'httpd'
        action :install
end
```

**O** que temos aqui?

* Resource Type: package (Pois queremos instalar o pacote httpd)
* Resource Name: apache (Embora o nome do pacote no CentOS seja httpd, o nome do nosso resource aqui é apache, mas poderia ser qualquer coisa que desejarmos)
* Resource Properties: Aqui temos apenas *package_name* como propriedade, no qual damos o nome do pacote desejado. OBS: Caso não utilizemos a propriedade *package_name*, ele buscará por um pacote com mesmo nome do resource. No nosso caso, demos o nome *apache* para nosso resource, portanto ele buscaria por um pacote chamado *apache* e falharia, pois no CentOS este pacote não existe.
* Actions: install (Temos apenas uma ação para esta recipe, que é justamente a de instalar o pacote, caso já não esteja instalado (idempotência))

**S**alve o arquivo e verifique o mesmo com os dois passos a seguir:

**1-** Verifique se a sintaxe ruby está correta:

```
# ruby -c exemplo.rb

Syntax OK
```

**2-** Utilize uma ferramenta do chef para verificar se a recipe está de acordo com o esperado pelo Chef:

```
# foodcritic exemplo.rb
Checking 1 files
x
FC011: Missing README in markdown format: ../README.md:1
FC031: Cookbook without metadata.rb file: ../metadata.rb:1
FC071: Missing LICENSE file: ../LICENSE:1
```

PS: Não se espante por enquanto com estes Warnings. Ele apenas está indicando que não possuímos um metadata, um readme e uma licença, pois não os criamos para este exemplo.

**V**erificado o código e aprovado, vamos executar esta recipe localmente.
*(Repare no retorno que será apresentado, onde ele verifica o tipo de resource e identifica que estamos rodando em uma máquina CentoOS, portanto utiliza por padrão o yum para instalar o pacote desejado.)*

```
# chef-client --local-mode exemplo.rb

[2018-04-01T19:18:00+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-01T19:18:00+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-01T19:18:03+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 1 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install
    - install version 2.4.6-67.el7.centos.6 of package httpd

Running handlers:
Running handlers complete
Chef Client finished, 1/1 resources updated in 16 seconds
[2018-04-01T19:18:17+00:00] WARN: No config file found or specified on command line, using command line options.
```

**S**imples, certo? O pacote httpd (Apache) foi instalado em nosso CentOS. Verificando o status do serviço httpd, veremos que o serviço não está rodando e também não está ativo.

```
# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:httpd(8)
           man:apachectl(8)
```

**I**sto está correto, afinal o Chef vai deixar a máquina no estado que determinamos. O determinado foi apenas instalar o pacote httpd. Mas de nada ele serve sem estar rodando como serviço, portanto vamos editar nossa recipe *exemplo.rb* e incluir nela um novo resource, desta vez um resource do tipo *service*, ou serviço. Sim, podemos ter diversos resources em uma mesma recipe. ;]

**E**dite sua recipe para que ela possua o seguinte conteúdo:

```
package 'apache' do
        package_name 'httpd'
        action :installchef_httpd_default.png
end

service 'httpd' do
        action [:enable, :start]
end
```

**O** que adicionamos aqui?

* Resource Type: service (Pois queremos gerenciar o serviço httpd)
* Resource Name: httpd (Poderíamos ter utilizado qualquer nome, mas para simplificar e não precisarmos utilizar uma propriedade de nome, deixaremos o resource com o nome do serviço, *httpd*)
* Actions: enable e start (Como nosso objetivo é não apenas iniciar o serviço, mas também deixá-lo habilitado para ser iniciado automaticamente após reinicialização, utilizaremos o *enable* e o *start*) -> equivalente aos comandos *systemctl enable httpd* e *systemctl start httpd*

**N**ovamente vamos verificar nosso código via ruby e foodcritic:

```
# ruby -c exemplo.rb && foodcritic exemplo.rb
```

**E** executando nossa recipe. (Novamente, o pacote httpd já está instalado, esta parte da recipe será ignorada automaticamente pelo Chef.)

```
# chef-client --local-mode exemplo.rb
[2018-04-01T19:32:26+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-01T19:32:26+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-01T19:32:28+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 2 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install (up to date)
  * service[httpd] action enable
    - enable service service[httpd]
  * service[httpd] action start
    - start service service[httpd]

Running handlers:
Running handlers complete
Chef Client finished, 2/3 resources updated in 06 seconds
[2018-04-01T19:32:33+00:00] WARN: No config file found or specified on command line, using command line options.
```

**A**gora podemos verificar que nosso serviço httpd está de fato rodando.

```
# systemctl status httpd && ps aux | grep httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Sun 2018-04-01 19:32:33 UTC; 1min 17s ago
     Docs: man:httpd(8)
           man:apachectl(8)
 Main PID: 2409 (httpd)
   Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
   CGroup: /system.slice/httpd.service
           ├─2409 /usr/sbin/httpd -DFOREGROUND
           ├─2410 /usr/sbin/httpd -DFOREGROUND
           ├─2411 /usr/sbin/httpd -DFOREGROUND
           ├─2412 /usr/sbin/httpd -DFOREGROUND
           ├─2413 /usr/sbin/httpd -DFOREGROUND
           └─2414 /usr/sbin/httpd -DFOREGROUND

Apr 01 19:32:33 kalib6.test.com systemd[1]: Starting The Apache HTTP Server...
Apr 01 19:32:33 kalib6.test.com systemd[1]: Started The Apache HTTP Server.
root      2409  0.0  0.2 226040  4948 ?        Ss   19:32   0:00 /usr/sbin/httpd -DFOREGROUND
apache    2410  0.0  0.1 226040  2872 ?        S    19:32   0:00 /usr/sbin/httpd -DFOREGROUND
apache    2411  0.0  0.1 226040  2872 ?        S    19:32   0:00 /usr/sbin/httpd -DFOREGROUND
apache    2412  0.0  0.1 226040  2872 ?        S    19:32   0:00 /usr/sbin/httpd -DFOREGROUND
apache    2413  0.0  0.1 226040  2872 ?        S    19:32   0:00 /usr/sbin/httpd -DFOREGROUND
apache    2414  0.0  0.1 226040  2872 ?        S    19:32   0:00 /usr/sbin/httpd -DFOREGROUND
root      2425  0.0  0.0 112660   976 pts/0    R+   19:33   0:00 grep --color=auto httpd
```

**A**lém disso, você pode testar seu novo servidor web diretamente em seu navegador. Caso esteja executando tudo em localhost, pode utilizar *localhost* como endereço. Caso contrário, pode utilizar o endereço ip da máquina em questão.

![Chef](/img/chef_httpd_default.png)

**A** página padrão do Apache não é algo que queremos, portanto vamos criar nosso próprio site (Hello World) para exemplificar melhor. Para isto editaremos novamente nossa recipe e incluiremos mais um resource, do tipo *file*, ou arquivo. O conteúdo de sua recipe deverá ficar assim:

```
package 'apache' do
        package_name 'httpd'
        action :install
end

service 'httpd' do
        action [:enable, :start]
end

file '/var/www/html/index.html' do
        content 'Hello World!'
        mode '0755'
        owner 'root'
        group 'apache'
end
```

**O** que adicionamos aqui?

* Resource Type: file (Pois queremos gerenciar o nosso arquivo index.html, o qual, caso não exista, será criado)
* Resource Name: /var/www/html/index.html (Poderíamos ter utilizado qualquer nome, mas para simplificar e não precisarmos utilizar uma propriedade de nome, deixaremos o resource com o nome do arquivo que vamos utilizar, */var/www/html/index.html*)
* Content: Hello World! (Para simplificar teremos uma simples string *Hello World!* como conteúdo de nosso site)
* Mode: Permissão que desejamos atribuir ao arquivo index.html
* Owner: Dono que deve ser atribuído ao arquivo index.html
* Group: Grupo que deve ser atribuído ao arquivo index.html

**N**ovamente vamos verificar nosso código via ruby e foodcritic:

```
# ruby -c exemplo.rb && foodcritic exemplo.rb
```

**E** executaremos novamente nossa recipe. (Assim como anteriormente, o Chef ignorará as instruções referentes aos resources que já se encontram no estado desejado)

```
# chef-client --local-mode exemplo.rb          
[2018-04-01T19:48:00+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-01T19:48:00+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-01T19:48:02+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 3 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install (up to date)
  * service[httpd] action enable (up to date)
  * service[httpd] action start (up to date)
  * file[/var/www/html/index.html] action create
    - create new file /var/www/html/index.html
    - update content in file /var/www/html/index.html from none to 7f83b1
    --- /var/www/html/index.html        2018-04-01 19:48:06.829566745 +0000
    +++ /var/www/html/.chef-index20180401-2631-164958p.html     2018-04-01 19:48:06.829566745 +0000
    @@ -1 +1,2 @@
    +Hello World!
    - change mode from '' to '0755'
    - change owner from '' to 'root'
    - change group from '' to 'apache'
    - restore selinux security context

Running handlers:
Running handlers complete
Chef Client finished, 1/4 resources updated in 06 seconds
[2018-04-01T19:48:07+00:00] WARN: No config file found or specified on command line, using command line options.
```

**P**odemos verificar que o Chef criou o nosso arquivo index.html, atribuindo o dono, grupo e permissão que indicamos:

```
# ls -lh /var/www/html/
total 4.0K
-rwxr-xr-x. 1 root apache 12 Apr  1 19:48 index.html
```

**T**ambém podemos voltar em nosso navegador e atualizar a página para que vejamos o nosso Hello World ao invés da página padrão do Apache.

![Chef](/img/chef_httpd_hw.png)

## Vale a pena?

**R**ealizar este processo manualmente na mesma máquina CentoOS seria mais rápido do que utilizando o Chef. Vamos rever:

**O** que precisamos?

* Instalar o pacote httpd
* Habilitar e Iniciar o serviço httpd
* Criar o arquivo index.html com o conteúdo "Hello World!"

**F**azendo manualmente seria apenas uma questão de executarmos 4 comandos:

```
# yum install httpd

# systemctl enable httpd && systemctl start httpd

# echo "Hello World!" > /var/www/html/index.html

# chmod 0755 /var/www/html/index.html && chown root:apache /var/www/html/index.html
```

**S**im, é verdade que fazendo manualmente neste caso seria MUITO mais rápido. O importante é lembrar do que falamos anteriormente: E se não for apenas 1 servidor? E se for um grupo? E se ao invés de apenas 3 resources, tiver 15? ou 40? E se alguém modificar algo em algum dos resources? Como você saberá qual foi? Vai verificar todos um a um para identificar o que precisa ser corrigido?

# Vantagens:

**I**magine que algum membro de sua equipe alterou a permissão do arquivo index.html sem lhe avisar, por exemplo ele foi lá e...

```
# chmod 0666 /var/www/html/index.html
```

**L**embrando que em nossa recipe exemplo.rb, definimos a permissão 0755. Neste caso, sempre que executarmos a recipe, o Chef irá verificar todos os resources e corrigir o que quer que tenha sido alterado.

```
# chef-client --local-mode exemplo.rb
[2018-04-01T20:04:08+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-01T20:04:08+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-01T20:04:10+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 3 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install (up to date)
  * service[httpd] action enable (up to date)
  * service[httpd] action start (up to date)
  * file[/var/www/html/index.html] action create
    - change mode from '0666' to '0755'
    - restore selinux security context

Running handlers:
Running handlers complete
Chef Client finished, 1/4 resources updated in 06 seconds
[2018-04-01T20:04:14+00:00] WARN: No config file found or specified on command line, using command line options.
```

**R**epare na linha *- change mode from '0666' to '0755'*. O Chef acabou de corrigir automaticamente sem que nós tenhamos de vasculhar cada componente e arquivo em nosso servidor para saber o que foi alterado ou o que está diferente do estado desejado. Novamente, aqui tratamos de um servidor único, com apenas um arquivo. Imagine ter que varrer manualmente diversos servidores e diversos arquivos e diretórios?

**N**este exemplo, provavelmente o site poderia continuar funcionando, pois foi alterada apenas a permissão de um único arquivo, certo? Mas imagine que sem querer ele acabou parando o serviço httpd, ou até desinstalou o mesmo? Em uma instalação padrão com Chef Server, existem agendamentos que fazem com que o Chef execute as recipes a cada X minutos, portanto o serviço seria inicializado novamente automaticamente, ou mesmo instalado caso necessário.

**É** fácil imaginar diversos cenários nos quais seria útil ter a sua infraestrutura em formato de código.

**I**magine uma catástrofe em que seu servidor simplesmente parou de funcionar e você precisará criar outro. Novamente você teria que executar aqueles comandos. Se desde o início tivesse utilizado Chef, ou outra ferramenta de Gerenciamento de Configuração, você poderia ter a sua recipe armazenada em um repositório Git, por exemplo, conforme mencionado no início deste post, e bastaria apenas executar o seu chef-client para instalar os pacotes necessários, habilitar e inicializar serviços, criar arquivos, etc.

**A** imaginação é o seu limite. ;]

**E**m posts futuros pretendo explorar mais a fundo o Chef, bem como outras ferramentas.

**H**appy Hacking!
