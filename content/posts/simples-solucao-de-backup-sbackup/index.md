---
title: "Simples Solucao De Backup Sbackup"
date: 2010-07-15T14:44:33-05:00
draft: false
tags: [
"Arch Linux",
"Cultura Hacker",
"Impressões",
"Linux",
"Segurança",
]
categories: ["Tutoriais"]
description: "Quantas vezes você já foi surpreendido por alguma falha grave em seu sistema de arquivos, disco rígido ou mesmo por vírus (no caso de quem utiliza Sistemas Operacionais Genéricos) e depois de perder centenas de arquivos e informações se perguntou: Porque eu não fiz backup disso antes?"
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**Q**uantas vezes você já foi surpreendido por alguma falha grave em seu sistema de arquivos, disco rígido ou mesmo por vírus (no caso de quem utiliza Sistemas Operacionais Genéricos) e depois de perder centenas de arquivos e informações se perguntou: Porque eu não fiz backup disso antes?

<!--more-->

**É** natural do ser humano ignorar o ditado "Não deixe para amanhã o que pode ser feito hoje" dando espaço ao mais utilizado e adotado (quase que enraizado na cultura brasileira) "Não faça agora o que pode esperar para amanhã", e, por consequência desta atitude, milhares de dados são perdidos diariamente no mundo inteiro pela falta de um simples backup.

**E**xistem milhares de ferramentas de backup super eficientes para meios corporativos, portanto não abordarei este tópico. Empresas já possuem (ou ao menos deveriam possuir) seus especialistas em TI que conhecem boas soluções e estratégias de backup para garantir a continuidade dos serviços da empresa em casos de necessidade ou perda.

**H**oje vou apresentar uma ferramenta bem simples e interessante aos usuários domésticos. Sim, backup não é apenas para empresas. Sempre é bom que se tenha backup mesmo em sua máquina doméstica. Não perca sua monografia que levou tanto tempo para ser trabalhada, ou aquele projeto importante, quem sabe até aquelas fotos de 10 anos atrás que você tanto preza.

**É** comum ver amigos fazendo tais backups em dvds, pendrives, email, etc. Porém, isso me parece um tanto quanto amador.

**S**e você realmente preza pelos seus dados, é hora de planejar um backup sério.

**L**hes apresento o [SimpleBackup](https://sourceforge.net/projects/sbackup/develop), ou simplesmente [SBackup](https://sourceforge.net/projects/sbackup/develop).

**U**ma apresentação informal rápida...

**O** Simple Backup é uma solução de backup simples para atender as necessidades de usuários desktop. O projeto foi patrocinado pelo Google durante o Google Summer of Code 2005.

**I**nstalação? Se você utiliza [Arch Linux](https://www.archlinux.org), basta pedir ao nosso amigo pacman:

_# pacman -S sbackup_

**OBS:** Ele lhe dará duas ferramentas depois de instalado:

_* Configuração do Backup_

_* Restauração de Bakcup_

**V**amos começar pela Configuração do Backup.

**E** agora, a tão esperada Cara do Bixo.


![SimpleBackup](sb1.png)

**E**ssa é a tela inicial. Simples e modesta. Sem propagandas ou logomarcas coloridas.

**O** Simple Backup lhe permite fazer backups simples, completos ou incrementais.

**N**esta primeira tela, você decide a forma desejada. Sugiro que para o primeiro teste, selecione a opção _Somente backup manual_. Desta forma iremos configurar um backup simples e rápido para testar a ferramenta.

**S**elecionada a opção de _"Somente backup manual"_, vamos à próxima aba chamada _"Inclusões"_. Nos será apresentada a tela a seguir:


![SimpleBackup](sb2.png)


**P**or padrão ele vai trazer vários diretórios, pode Remover os diretórios que ele lhe trás e adicionar alguns poucos diretórios (de preferência com poucos arquivos, apenas para agilizar nosso teste. ;] Um total de 20 ou 30 MBs já resolve para nosso teste.

**N**o meu exemplo, eu desejo backup apenas dos diretórios "/home/kalib/imgs/" e "/home/kalib/amsn_received/", conforme pode ser visto na imagem anterior.

**U**ma vez que você tenha finalizado a configuração dos diretórios desejados, vamos para a próxima aba: _"Exceções"_

**E**sta aba servirá para informarmos o que NÃO deverá entrar no backup. Esta sessão se divide em 4 categorias, como pode ser visto na imagem abaixo:


![SimpleBackup](sb3.png)


**1- Pastas -** **A**qui você lista quais pastas ou arquivos não deseja incluir no backup. Repare que ele já trás vários diretórios por padrão. Pode remover todos. Por exemplo:

**S**upondo que eu tenha marcado o diretório /home/kalib/imgs/ para backup, porém dentro deste diretório existem os diretórios /imgs1 /imgs2 e /imgs3. Eu não quero o /imgs3 em meu backup, então posso incluir nestas Exceções de Pastas o caminho "/home/kalib/imgs/imgs3/". Deverá ficar algo como ilustrado a seguir:


![SimpleBackup](sb4.png)


**2- Tipos de arquivos - A**qui devemos descriminar quais tipos de arquivos iremos deixar fora fora do backup. Novamente, ele já trás vários diretórios por padrão. Pode remover todos. Esta função é útil por exemplo para quem não deseja levar no backup algum tipo de arquivos em específico.

**P**or exemplo, supomos que nos diretórios que eu marquei para fazer backup, eu não deseje que sejam levados também os arquivos do tipo .mp3.

**P**ara isto, eu clico na opção Adicionar e na janela que será apresentada escolho a opção de formato mp3.

**D**everá ficar da seguinte forma:


![SimpleBackup](sb5.png)


**3- Expressões Regulares - E**sta é para quem possui conhecimentos em regex. Caso você não entenda o que são expressões regulares, sugiro que remova os que ele trás por padrão e deixe este campo em branco.

**4- Tamanho Máximo - P**or último, definiremos o tamanho máximo de cada arquivo que estará no backup. Vamos supor que eu não queira que ele leve arquivos maiores que 10 MB. Basta indicar neste campo e seguir em diante.

**A**gora vamos conhecer a próxima aba, _"Destino"_.


![SimpleBackup](sb6.png)


**N**esta aba, como o próprio nome já diz, deveremos escolher o local para onde será enviado o nosso backup. Aqui temos três opções:

**1- U**sar a pasta padrão backups que se localiza em /var/backup

**2- U**sar uma pasta personalizada: Neste caso você vai escolher em qual diretório você deseja que o backup seja feito. Lembrando que este destino pode ser também um disco externo que possa estar plugado via usb, por exemplo.

**3- U**tilizar uma pasta remota: Neste caso você envia o backup para uma outra máquina pela rede através de ssh ou ftp.

**N**o meu caso, optei por enviar via ssh para uma outra máquina, como pode ser visto na imagem anterior. Mas, qualquer que seja a opção escolhida por você, servirá para o nosso teste. Apenas a nível de curiosidade, segue comando que digitei como meu destino:

_ssh://MEU_USUARIO:MINHA_SENHA@192.168.0.83/home/marcelo.cavalcante/backups/_

**O**nde:

MEU_USUARIO = nome do usuário na máquina que receberá este meu backup

MINHA_SENHA = senha deste usuário

192.168.0.83 = ip da máquina para onde o backup será enviado

/home/marcelo.cavalcante/backups/ = diretório onde eu desejo que o backup seja guardado na outra máquina

**A** próxima aba é a de _"Agendamento"_. Nela você irá definir a frequência com que seu backup será realizado. Diário, semanal, mensal, a cada 2 dias, a cada 10 dias, etc... Qual dia? Qual hora? Qual minuto?

**E**stas configurações, caso habilitadas, serão para um backup incremental, ou seja, apenas o que foi adicionado desde o último backup.  Repare na opção "Faça um backup completo uma vez a cada XX dias". Caso você faça os agendamentos anteriores e determine neste campo o valor de 10 dias, estará significando que o seu agendamento anterior será para backups incrementais e apenas depois de 10 dias ele será completo.

**E**sta é a cara da criança:


![SimpleBackup](sb7.png)


**C**omo nada disso importa para este nosso teste, deixe como na imagem acima mesmo... sem agendamentos e datas.

**V**amos à última aba: _"Limpeza"_

**É** aqui que você configura sua política de limpeza de backups. por exemplo. Se eu faço backups diários incrmentais e um completo a cada semana, rapidamente eu terei uma quantidade de arquivos de backups enormes e desnecessários, então posso especificar nesta aba que desejo que arquivos de backup de 30 dias atrás, por exemplo, sejam removidos automaticamente.

**N**ovamente, deixemos isto para lá por enquanto. Não utilizaremos em nosso teste. Segue imagem:


![SimpleBackup](sb8.png)


**F**eitas estas configurações, clique em _"Salvar"_. Isto irá gravar as configurações feitas por você. Após isto, basta clicar em _"Backup Agora!"_

**O** backup será iniciado em background, portanto não espere por uma barra de progresso. Caso deseje acompanhar se o processo realmente iniciou, utilize ferramentas como o top ou o ps em seu linux. ;]

**C**omo colocamos poucos diretórios e arquivos, esta tarefa deve levar poucos minutos.

**P**assados alguns minutos, pode reparar no local que você especificou como destino. Lá estarão seus arquivos de backup, com data e hora deste momento em que foi executado.

**E** caso, eu tenha escondido um vírus neste blog e os arquivos de seu hd sejam removidos em 2 minutos? >]

**N**ão se desespere. Uma vez que você já realizou backup de seus dados, tudo o que você precisa fazer é restaurar o mesmo.

**I**nicie agora a outra ferramenta: **Restauração de backup**

**E**sta é a aparência, também simples, da mesma:


![SimpleBackup](sb9.png)


**T**udo o que tem a fazer é escolher o arquivo de backup que deseja restaurar e clicar em restaurar. ;]

**E**la lhe permite inclusive escolher o que exatamente você deseja restaurar e onde deseja restaurar. Tudo isso com uma interface bastante simples e intuitiva.

**Q**ue bom ter uma cópia de seus dados em casos de catástrofes, certo?!

**A**braços!
