---
title: "Chef: Uso De Condicionais not_if E only_if"
date: 2018-04-14T15:03:10-05:00
draft: false
tags: [
"Devops",
"Chef",
]
categories: ["Tutoriais"]
description: "Chef é uma popular ferramenta de Gerenciamento de Configurações criado pela empresa de mesmo nome, Chef. Neste post veremos a utilização de condicionais em recipes com only_if e not_if."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---

Chef é uma popular ferramenta de Gerenciamento de Configurações criado pela empresa de mesmo nome, Chef. Neste post veremos a utilização de condicionais em recipes com only_if e not_if.

<!--more-->

## Pare e volte uma casa
![Volte 1 Casa](volte1casa.jpeg)

**S**e você nunca utilizou ou sabe pouco sobre o *Chef* é importante que você pare aqui mesmo e volte uma casa. Sugiro que leia o post [Chef: Automação e Gerenciamento de Configuração](posts/chef-aautomacao-e-gerenciamento-de) antes de seguir este, uma vez que através dele você entenderá o que é, e como funciona o Chef, bem como sua instalação básica.

## Resources

**C**onforme dito no post anterior, resource é uma descrição de estado que:

* Descreve o estado desejado para um item de configuração
* Declara os passos necessários para levar o item especificado ao estado desejado
* Especifica o tipo de resource - por exemplo, `package`, `template` ou `service`
* Lita detalhes adicionais (também conhecidos como propriedades de resources), conforme necessário
* São agrupados em *recipes*, que descrevem configurações em geral

## Utilizando (Guardas) not_if e only_if

**T**odos os resources (incluindo os personalizados) no Chef compartilham um conjunto de opções comuns: ações, propriedades, condicionais, notificações e paths relativos.

**Guards**

**P**ropriedades de *guarda*, ou `guards`, como são chamadas no Chef, podem ser utilizadas para avaliar o estado de um node durante a fase de execução do chef-client. Esta avaliação funciona como uma condicional e, baseando-se nos resultados da mesma, a propriedade guard é então utilizada para indicar ao chef-client se ele deve ou não continuar a execução de um resource. Uma propriedade guard aceita tanto um valor string quanto um bloco de código Ruby.

* A string é executada como um comando shell. Caso o comando retorne `0` (true), a propriedade guard é aplicada. Caso o comando retorne qualquer outro valor a propriedade guard não será aplicada.
* Um bloco é executado como um código Ruby que deve retornar `true` ou `false`. Da mesma forma, caso o comando retorne `true`, a propriedade guard é aplicada. Caso o comando retorne `false` a propriedade guard não será aplicada.

**U**ma **guard** é importante para garantir que um resource seja idempotente ao permitir um teste no próprio resource certificando-se de que o mesmo se encontra no estado desejado de forma que o chef-client não faça nada.

**Atributos**

**O**s seguintes atributos podem ser utilizados para definir uma guard que é avaliada durante a fase de execução do chef-client:

`not_if` - **Impede** a execução de um resource quando a condição retornar `true`.

`only_if` - Permite a execução de um resource **apenas** quando a condição retornar `true`.

## Mãos à obra

**P**ara simplificar seguirei utilizando a recipe utilizada no post anterior. Não sabe o que é uma recipe? `->` Novamente, caso não entenda o que estou dizendo, volte uma casa e leia o [post anterior](posts/chef-aautomacao-e-gerenciamento-de) no qual explico o que é uma recipe, bem como cada elemento da mesma, visto que a utilizaremos aqui.

**N**ossa recipe era a seguinte:

```ruby
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

**C**omo primeira alteração vamos editar o arquivo `/etc/motd`. Este arquivo nada mais é do que a definição de um baner que será apresentado sempre que alguém se logar em seu servidor.

**P**or padrão, este arquivo costuma vir vazio. Este é o caso da máquina utilizada para este exemplo:

```bash
# cat /etc/motd
```

**C**omeçaremos definindo o conteúdo que deverá existir em nosso arquivo `/etc/motd` incluindo um resource do tipo `file` no final de nossa recipe `exemplo.rb`:

```ruby
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

file '/etc/motd' do
        content 'Bem Vindo!'
end
```

**A**té aqui nenhuma novidade (caso você tenha lido de fato o post anterior ou já tenha utilizado Chef anteriormente), apenas incluímos mais um resource em nossa recipe `exemplo.rb` informando que o arquivo `/etc/motd` deve existir e que seu conteúdo deverá ser *Bem Vindo!*.

**V**alidando e executando localmente nossa recipe conforme feito anteriormente veremos que todos os demais passos ou resources serão ignorados por já estarem estarem no estado desejado (idempotência), restando apenas a inclusão do conteúdo no `/etc/motd`:

**Validando**

```bash
# ruby -c exemplo.rb && foodcritic exemplo.rb
Syntax OK
Checking 1 files
x
FC011: Missing README in markdown format: ../README.md:1
FC031: Cookbook without metadata.rb file: ../metadata.rb:1
FC071: Missing LICENSE file: ../LICENSE:1
```

**Executando**

```bash
[root@kalib6 ~]# chef-client --local-mode exemplo.rb
[2018-04-15T22:03:26+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-15T22:03:26+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-15T22:03:29+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 4 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install (up to date)
  * service[httpd] action enable (up to date)
  * service[httpd] action start (up to date)
  * file[/var/www/html/index.html] action create (up to date)
  * file[/etc/motd] action create
    - update content in file /etc/motd from e3b0c4 to f13843
    --- /etc/motd       2018-04-15 20:51:00.411479476 +0000
    +++ /etc/.chef-motd20180415-1681-1p1fm7m    2018-04-15 22:03:42.142091791 +0000
    @@ -1 +1,2 @@
    +Bem Vindo!
    - restore selinux security context

Running handlers:
Running handlers complete
Chef Client finished, 1/5 resources updated in 15 seconds
```

**P**ara termos certeza de que o nosso arquivo foi corretamente alterado...

```bash
[root@kalib6 ~]# cat /etc/motd
Bem Vindo!
```

**N**ovamente, nenhuma novidade até aqui.

**V**amos agora utilizar o tipo de resource `execute`, o qual nos permite executar um comando a cada execução do chef-client. Vale lembrar que uma das características do Chef é a idempotência, portanto o `execute` pode ser considerado uma exceção, já que o comando será executado sempre, mesmo que já tenha sido executado anteriormente. E é neste tipo de situação que os `guards` se mostram importantes.

**C**omecemos inserindo um resource do tipo `execute` em nossa recipe:

```ruby
package 'apache' do
        package_name 'httpd'
        action :install
end
https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
service 'httpd' do
        action [:enable, :start]
end

file '/var/www/html/index.html' do
        content 'Hello World!'
        mode '0755'
        owner 'root'
        group 'apache'
end

file '/etc/motd' do
        content 'Bem Vindo!'
end

execute 'meu-comando' do
        command 'echo " Obrigado!" >> /etc/motd'
        only_if 'test -r /etc/motd'
end
```

**O** que adicionamos aqui?

* Resource Type: execute (Para que possamos executar um comando)
* Resource Name: meu-comando (Poderíamos ter utilizado qualquer nome)
* Command: 'echo " Obrigado!" >> /etc/motd' (O comando que desejamos executar. Estou utilizando `echo` para inserir * Obrigado!* ao meu arquivo `/etc/motd`)
* Guard: only_if 'test -r /etc/motd' (`only_if` implica que meu resource *meu-comando* será executado **apenas** caso o resultado de `test -r /etc/motd` seja positivo. *test -r* irá verificar se o arquivo `/etc/motd` existe no sistema. Caso sim, meu comando `echo " Obrigado!" >> /etc/motd` será executado conforme planejado, do contrário será ignorado)

**N**ós já sabemos que o arquivo existe, portanto esperamos que * Obrigado!* seja adicionada ao mesmo após execução do chef-client.

**Verificando nosso código**

```bash
[root@kalib6 ~]# ruby -c exemplo.rb && foodcritic exemplo.rb
Syntax OK
Checking 1 files
x
FC011: Missing README in markdown format: ../README.md:1
FC031: Cookbook without metadata.rb file: ../metadata.rb:1
FC071: Missing LICENSE file: ../LICENSE:1
```

**Tudo ok. Executando...**

```bash
[root@kalib6 ~]# chef-client --local-mode exemplo.rb
[2018-04-15T23:08:54+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-15T23:08:54+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-15T23:09:03+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 5 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install (up to date)
  * service[httpd] action enable (up to date)
  * service[httpd] action start (up to date)
  * file[/var/www/html/index.html] action create (up to date)
  * file[/etc/motd] action create (up to date)
  * execute[meu-comando] action run
    - execute echo " Obrigado!" >> /etc/motd

Running handlers:
Running handlers complete
Chef Client finished, 1/6 resources updated in 31 seconds
```

**R**epare que o Chef executou o comando para inserir ` Obrigado!`.

```bash
[root@kalib6 ~]# cat /etc/motd
Bem Vindo! Obrigado!
```

**C**onforme dito anteriormente, o resource `execute` irá executar o meu comando SEMPRE que o chef-client rodar. Vejamos o que acontece ao executar novamente o chef-client sem alterar a recipe.

```bash
[root@kalib6 ~]# chef-client --local-mode exemplo.rb
[2018-04-15T23:18:10+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-15T23:18:10+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-15T23:18:20+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 5 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install (up to date)
  * service[httpd] action enable (up to date)
  * service[httpd] action start (up to date)
  * file[/var/www/html/index.html] action create (up to date)
  * file[/etc/motd] action create
    - update content in file /etc/motd from 201ddf to f13843
    --- /etc/motd       2018-04-15 23:16:13.705005377 +0000
    +++ /etc/.chef-motd20180415-2432-zhtjnq     2018-04-15 23:18:42.138379629 +0000
    @@ -1,2 +1,2 @@
    -Bem Vindo! Obrigado!
    +Bem Vindo!
    - restore selinux security context
  * execute[meu-comando] action run
    - execute echo " Obrigado!" >> /etc/motd

Running handlers:
Running handlers complete
Chef Client finished, 2/6 resources updated in 31 seconds
```

**C**omo em nossa recipe temos o resource `file` que determina o conteúdo do arquivo `/etc/motd` como sendo `Bem Vindo!`, o chef percebeu que o arquivo se encontrava diferente, pois acrescentamos o ` Obrigado!` no passo anterior. O arquivo foi corrigido, voltando a ter seu conteúdo original, em seguida o Chef inseriu novamente ` Obrigado!`, pois assim está determinado em nossa recipe.

**É** fácil perceber isto nas linhas a seguir, retiradas do resultado de nossa última execução do chef-client:

```bash
* file[/etc/motd] action create
  - update content in file /etc/motd from 201ddf to f13843
  --- /etc/motd       2018-04-15 23:16:13.705005377 +0000
  +++ /etc/.chef-motd20180415-2432-zhtjnq     2018-04-15 23:18:42.138379629 +0000
  @@ -1,2 +1,2 @@
  -Bem Vindo! Obrigado!
  +Bem Vindo!
  - restore selinux security context
* execute[meu-comando] action run
  - execute echo " Obrigado!" >> /etc/motd
```

**S**e verificarmos nosso arquivo, veremos que ele está da mesma forma:

```bash
[root@kalib6 ~]# cat /etc/motd
Bem Vindo! Obrigado!
```

**P**ara percebermos a diferença, vamos comentar as linhas do resource file `/etc/motd`:

```bash
...
#file '/etc/motd' do
#       content 'Bem Vindo!'
#end
...
```

**A**o comentar estas linhas, nossa recipe não mais indicará que o conteúdo do arquivo `/etc/motd` deve ser `Bem Vindo!`, portanto vejamos o que acontece quando executamos novamente o chef-client.

```bash
[root@kalib6 ~]# chef-client --local-mode exemplo.rb
[2018-04-15T23:28:32+00:00] WARN: No config file found or specified on command line, using command line options.
[2018-04-15T23:28:33+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /root.
Starting Chef Client, version 13.8.5
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Installing Cookbook Gems:
Compiling Cookbooks...
[2018-04-15T23:28:42+00:00] WARN: Node kalib6.test.com has an empty run list.
Converging 4 resources
Recipe: @recipe_files::/root/exemplo.rb
  * yum_package[apache] action install (up to date)
  * service[httpd] action enable (up to date)
  * service[httpd] action start (up to date)
  * file[/var/www/html/index.html] action create (up to date)
  * execute[meu-comando] action run
    - execute echo " Obrigado!" >> /etc/motd

Running handlers:
Running handlers complete
Chef Client finished, 1/5 resources updated in 32 seconds
```

**U**ma vez que nosso arquivo `/etc/motd` já possuía o conteúdo `Bem Vindo! Obrigado!` e desta vez não tentou garantir que o conteúdo do mesmo fosse apenas `Bem Vindo!`, vejamos como ele se encontra:

```bash
[root@kalib6 ~]# cat /etc/motd
Bem Vindo! Obrigado!
Obrigado!
```

**C**omo esperado, inserimos mais um ` Obrigado!` ao arquivo. Se executarmos novamente o chef-client, estaremos acrescentando novamente um ` Obrigado!` ao arquivo, pois o mesmo existe e não estamos mais tentando validar seu conteúdo.
**E** se removermos manualmente o arquivo `/etc/motd`?

```bash
[root@kalib6 ~]# rm /etc/motd
rm: remove regular file ‘/etc/motd’? y
[root@kalib6 ~]# cat /etc/motd
cat: /etc/motd: No such file or directory
```

**V**ejamos o que o chef-client fará:

```bash
... (Ignorando linhas desnecessárias)
  * execute[meu-comando] action run (skipped due to only_if)
...
```

**C**omo já tínhamos comentado as linhas que garantem que o `/etc/motd` existe e possui o conteúdo `Bem Vindo!`, o resource que incluiría ` Obrigado!` foi ignorado, pois em nossa guard temos a condição `only_if`, que indica que o comando só será executado SE o arquivo /etc/motd existir. Para ter certeza disto, vamos verificar:

```bash
[root@kalib6 ~]# cat /etc/motd
cat: /etc/motd: No such file or directory
```

**A**proveitando a situação, vamos alterar nossa recipe e utilizar `not_if` ao invés de `only_if`:

```ruby
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

#file '/etc/motd' do
#       content 'Bem Vindo!'
#end

execute 'meu-comando' do
        command 'echo " Obrigado!" >> /etc/motd'
        not_if 'test -r /etc/motd'
end
```

**J**á sabemos que o arquivo não existe, portanto a nossa regra agora será satisfeita, visto que queremos adicionar o conteúdo ` Obrigado!` APENAS caso o arquivo NÃO exista.

**V**ale lembrar que, por padrão, o comando `echo ' Obrigado!' >> /etc/motd` irá adicionar o conteúdo ao arquivo e, caso o mesmo não exista, ele será criado com o determinado conteúdo. Isto não é um recurso do Chef, mas sim do próprio linux/echo. Executando nosso chef-client:

```bash
... (Ignorando linhas desnecessárias)
* execute[meu-comando] action run
  - execute echo " Obrigado!" >> /etc/motd

Running handlers:
Running handlers complete
```

**R**epare que o arquivo foi criado apenas com o conteúdo ` Obrigado!`, conforme descrito em nossa recipe.

```bash
[root@kalib6 ~]# cat /etc/motd
 Obrigado!
```

**C**aso o chef-client seja executado novamente, nada acontecerá, uma vez que o resource atual indica que o comando APENAS deverá ser executado caso o arquivo NÃO exista.

```bash
... (Ignorando linhas desnecessárias)
  * execute[meu-comando] action run (skipped due to not_if)
...
```

**S**imples, certo?! É importante lembrar que o Chef executará os resources na devida ordem em que forem listados na recipe, portanto é importante alinhar todas as instruções e resources de acordo com o resultado desejado.

**H**appy hacking!
