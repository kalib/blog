---
title: "Tornando Seu Ssh Mais Seguro"
date: 2013-08-28T08:40:41-05:00
draft: false
image: "/img/openssh.png"
tags: [
"Linux",
"Software Livre",
"Segurança",
"Redes",
"SSH",
]
description: "Dicas para tornar seu SSH ainda mais seguro."
---
**O** *SSH* (Secure Shell) é um dos protocolos mais utilizados atualmente para transferência de arquivos ou mesmo conexão remota, principalmente em ambientes Linux/Unix e, por si só, já tem se provado ser confiável e seguro. O mesmo passou a ser o principal substituto ao *TELNET* por possuir a vantagem da criptografia na conexão entre o cliente e o servidor, além da possibilidade de criação de túneis, o que chamamos de *tunneling*, oferecendo assim a capacidade de redirecionar pacotes de dados. Mas não é por isso que devemos nos descuidar...

**S**empre existe espaço para melhorias.

**A**s configurações do servidor SSH, geralmente, ficam localizadas em */etc/ssh/sshd_config*. Vale lembrar que a cada alteração feita neste arquivo será necessário reiniciar o serviço SSH para que suas alterações possam ser efetivadas.

**I**ndo ao que interessa:

**1- Altere a porta de escuta do SSH**

**P**or padrão o SSH aceita conexões na porta 22, portanto é importante alterar esta porta para dificultar as tentativas de ataques aleatórios de brute-force que buscam máquinas que disponibilizam o SSH através da porta padrão. Preferivelmente, é interessante optar por uma porta posterior a 1024, visto que a maioria dos softwares de escaneamento de portas e serviços não fazem uma varredura por portas muito altas. Esta é geralmente uma das primeiras configurações no arquivo */etc/ssh/sshd_config*, conforme pode ser visto abaixo:

```
 Port 22
```


**2- Permita apenas o protocolo SSH 2**

**D**ependendo de sua distribuição Linux, o padrão poderá ser a utilização de ambos os protocolos, 1 e 2. A versão 1 do protocolo SSH possui vulnerabilidades bastante conhecidas, portanto é preferível que utilize apenas o protocolo de versão 2, diminuindo assim suas chances de um ataque de inserção, por exemplo, conforme possível com a versão 1 do protocolo. Procure pela seguinte linha de configuração e altere-a mantendo apenas a opção *2*:

```
 Protocol 2,1
```


**3- Decida quais usuários poderão logar-se via SSH**

**P**ermitir que qualquer usuário possa logar-se via SSH não é aconselhável, da mesma forma que permitir que o usuário root efetue login via SSH também é extremamante desencorajado devido às vulnerabilidades envolvidas. Por possuírem determinados (ou todos, no caso do root) privilégios no sistema, alguns usuários simplesmente não devem ter permissão de login via SSH, evitando assim que alguém utilize ferramentas de brute-force e consiga, por quebra de senha, logar-se em seu servidor com um usuário privilegiado. Para proibir o login do usuário root via SSH, altere a seguinte linha para *no*:

```
 PermitRootLogin yes
```

**P**ara definir quais usuários poderão ter acesso via SSH, insira o nome dos mesmos na seguinte linha, conforme exemplo abaixo:

```
 AllowUsers usuario1 usuario2 usuario3
```


**4- Utilize uma chave pública DSA como mecanismo de autenticação**

**A**o invés de utilizar nomes de usuários e senhas para autenticar-se via SSH, você pode utilizar chaves públicas DSA. Outra opção é utilizar ambos os mecanismos, usuário e senha + chave. Uma das maiores vantagens de se utilizar chaves como mecanismo de autenticação é que você impossibilitará o acesso indevido através de ataques de força bruta ou brute force, pois você não precisará de um login e senha para logar-se ao servidor ou estação. Ao invés disso, você utilizará um par de chaves - uma pública e uma privada. No cenário de chaves DSA, a chave privada fica em sua máquina e uma cópia da chave pública fica no servidor que você deseja acessar via SSH.

**A**o tentar logar-se em um servidor configurado para autenticação via chave DSA, o mesmo verifica as chaves de ambos os lados e, se elas combinarem, autoriza seu login, caso contrário nega sua conexão.

**N**o arquivo de configuração do servidor SSH você irá procurar os seguintes parâmetros e deixá-los da seguinte forma:

```
 RSAAuthentication yes
 PubkeyAuthentication yes
 AuthorizedKeysFile %h/.ssh/authorized_keys
```

**C**aso você deseje utilizar *apenas* a autenticação por chaves DSA, você precisará desabilitar a autenticação por senha, deixando como *no* o seguinte parâmetro:

```
 PasswordAuthentication no
```

**A**gora que as regras de configuração para a autenticação via chaves foram criadas, vamos criar as chaves em si.

**E**m sua máquina, vamos digitar o seguinte comando para a criação de suas chaves pública e privada:

```
 $ ssh-keygen -t dsa -C "$(whoami)@$(hostname)-$(date -I)"
```

**O** sistema lhe perguntará se você deseja escolher um diretório no qual salvar suas chaves. Caso você não especifique nenhum, ele salvará no diretório .ssh que se encontra no home de seu usuário *(~/.ssh/id_dsa)*. Em seguida lhe perguntará se você deseja criar uma senha para sua chave (não é obrigatório).

**A**o finalizar este procedimento você receberá a confirmação de que o procedimento foi realizado. Algo similar a isto:

```
 Your identification has been saved in /home/username/.ssh/id_dsa.
 Your public key has been saved in /home/username/.ssh/id_dsa.pub.
 The key fingerprint is:
 dd:15:ee:24:20:14:11:01:b8:72:a2:0f:99:4c:79:7f username@localhost-2013-08-28
 The key's randomart image is:
 +--[DSA  1024]---+
 |     ..oB=.   .  |
 |    .    . . . . |
 |  .  .      . +  |
 | oo.o    . . =   |
 |o+.+.   S . . .  |
 |=.   . E         |
 | o    .          |
 |  .              |
 |                 |
 +-----------------+
```

**O** próximo passo é copiar a sua chave pública para o servidor que você deseja acessar via SSH. Você pode copiar de algumas formas diferentes:

**4.1 - Copiando manualmente**

**C**opie o conteúdo retornado através do seguinte comando:

```
 $ cat ~/.ssh/id_dsa.pub
```

**O** retorno será algo similar a isto:

```
 ssh-dss AAAAB3NzaC1kc3MAAACBAKWJtlUANTuqKy1NN4bb5qBJLZVMnR+nr84hk7EWnHmpJgipRZ8Y/tUDJTpCab
 GwoMNYmPZaz62Nm5/F5Nl9yzgMmxfGja8OYt3bwir6A379NOWFM2NBq7Q107uH4L+MFszXKoCVn6rM9SddkAJL1V66
 eGX+Y+r1o/8wG773h9P5AAAAFQDh09qovWFzKJk0E58c0oRz9S4UAQAAAIBuVxw+LZDrWVVCLr4WMubRPJuFzXqqHN
 re9RUZ2kMrUENnAenYyJgjcBg7uJJA/wMfn7Oe32e4hvpU7SsXEUf7xT1sKb/UFyX9qTr5G0kkU6t8IYPjIQaqRIOQ
 mnRiqm7JCDd1GTAFU9n/ocK4sNPZDL8KeEEquZmk+ihKcu9FLwAAAIBlz+K7fcZSlA5hKewHOF7fA9IdwdMsbc2Ri7
 bSw4Fv70rQeMnYYUDeHQQJHeJJyScmuHswordGXYKe2tsf7fYn11CW46P9WxCRcargojxJNrRHlF5WiK15fhNMLtfx
 7/+CSmoXnEUCw8uCCBVduGLyevZ3WC+FEx7/aqJjuUCNJg== username@localhost-2013-08-28
```

**É** exatamente este conteúdo que precisa ser copiado.

**E**m seguida você precisará colar este conteúdo no servidor desejado, porém incrementando dentro do seguinte arquivo (no servidor): *~/.ssh/authorized_keys*

**P**ara facilitar, você poderá já ter copiado o arquivo da chave completo para o servidor. Veja nos próximos métodos algumas formas de o fazer.

**4.2 - Método Simples - Deve ser usado antes de configurar o servidor para permitir autenticação por chaves**

**S**e o seu arquivo de chave é *~/.ssh/id_dsa*, você poderá simplesmente digitar o seguinte comando para enviar a chave para o servidor:

```
 $ ssh-copy-id servidor-remoto.com.br
```

**C**aso o nome de usuário em sua máquina seja diferente do nome de usuário no servidor, lembre-se de adicionar o nome do usuário do servidor antes do endereço do mesmo:

```
 $ ssh-copy-id nome-de-usuario@servidor-remoto.com.br
```

**C**aso o servidor esteja recebendo as conexões SSH em uma porta diferente da padrão (22), utilize (supondo que estamos utilizando a 221):

```
 $ ssh-copy-id -p 221 nome-de-usuario@servidor-remoto.com.br
```

**4.3 - Método Tradicional - Deve ser usado antes de configurar o servidor para permitir autenticação por chaves**

**P**or padrão a chave pública precisa ser concatenada no arquivo *~/.ssh/authorized_keys*. Comece copiando a chave pública para o servidor remoto:

```
 $ scp ~/.ssh/id_dsa.pub nome-do-usuario@servidor-remoto.com.br:
```

**O** comando acima copiará a chave pública para o diretório home do usuário citado para o servidor via scp. Não esqueça de inluir o *:* ao final do endereço do servidor.

**C**aso o diretório *~/.ssh* ainda não exista no servidor, você precisará criá-lo. O mesmo com o arquivo *authorized_keys*:

```
 $ ssh nome-do-usuario@servidor-remoto.com.br
 nome-do-usuario@servidor-remoto's password:
 $ mkdir ~/.ssh
 $ cat ~/id_dsa.pub >> ~/.ssh/authorized_keys
 $ rm ~/id_dsa.pub
 $ chmod 600 ~/.ssh/authorized_keys
```

**O**s últimos dois comandos removem a chave pública do diretório do servidor e definem as permissões para o arquivo *authorized_keys* de forma que ele apenas possa ser lido ou escrito por você, o dono.

**P**ronto, sua chave já está no servidor após seguir uma das dicas acima.

**L**embre-se sempre de reiniciar o serviço ssh após efetuar qualquer alteração de configuração.

**D**icas simples de serem implementadas mas que irão melhorar bastante a segurança de seu servidor SSH. ;]
