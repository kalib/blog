---
title: "Brute Force: O Dirb... Dirbuster Lhe Ajuda a Identificar Vulnerabilidades Em Seu Site"
date: 2011-02-15T21:09:38-05:00
draft: false
image: "/img/tech2.jpeg"
tags: [
"Arch Linux",
"Cultura Hacker",
"Impressões",
"Java",
"Kubuntu",
"Linux",
"Redes",
"Segurança",
"Software Livre",
]
description: "Bom, para quem não conhece nada sobre Brute-Force ou apenas ouviu falar vagamente mas não tem certeza sobre o que realmente é um ataque deste tipo, sugiro dar uma lida em meu último post, onde apresentei algumas informações sobre brute-force bem como uma ferramenta que podemos utilizar para testar nossos sites e verificar o quão vulneráveis eles estão a este tipo de ataque."
---
**B**om, para quem não conhece nada sobre Brute-Force ou apenas ouviu falar vagamente mas não tem certeza sobre o que realmente é um ataque deste tipo, sugiro dar uma lida em meu último post, onde apresentei algumas informações sobre brute-force bem como uma ferramenta que podemos utilizar para testar nossos sites e verificar o quão vulneráveis eles estão a este tipo de ataque. Segue link para o mesmo:

[Brute-Force? O Dirb lhe ajuda a identificar vulnerabilidade em seu site!](https://marcelocavalcante.net/portal/2011/01/31/brute-force-o-dirb-lhe-ajuda-a-identificar-vulnerabilidades-em-seu-site/)

**A**gora que já conhecemos um pouco sobre as técnicas de brute-force e a ferramenta Dirb, que tal conhecer outra mais apresentável?

**M**e refiro ao [DirBuster](https://www.owasp.org/index.php/DirBuster). Uma aplicação multithread desenvolvida pelo [OWASP](https://www.owasp.org/) (The Open Web Application Security Project) que tem como objetivo realizar brute-force em diretórios e nomes de arquivos na web. Assim como o Dirb, o DirBuster trabalha em cima de dicionários ou listas. Ao todo ele trás 9 listas que podem ser utilizadas para nossas varreduras e testes. Como todas as suas listas foram criadas na unha, após exaustívas pesquisas e buscas na web por nomes mais utilizados para arquivos e diretórios em servidores web, o DirBuster mostra-se extremamente efetivo para este tipo de atividade conseguindo encontrar até mesmo os arquivos mais ocultos e escondidos.

**M**as se você não ficar satisfeito, <del>nós garantimos a devolução do seu dinheiro</del>, com apenas utilizar o modelo de dicionários, o DirBuster ainda lhe possibilita o ataque de brute-force mais purista. Tentativa e erro com combinações possíveis de caracteres. Claro, se você tem muito poder de processamento e tempo sobrando, boa sorte. ;]

**L**egal Kalib, quer dizer que agora posso sair invadindo sites?

<del>o.O Como diria o Chaves: Ai que burrrrroooo.. dá zero pra ele...</del>

**N**ão amigo. O meu intuito não é incentivar ou apresentar qualquer mecanismo ou técnica para este tipo de atividade. O DirBuster não foi feito para este propósito e nem conseguirá efetuar isto. Ele não fará nenhum tipo de exploit em arquivos ou diretórios que ele encontre. O DirBuster serve exatamente para identificar possíveis alvos para estes tipos de ataque, o que nos permite agir de forma corretiva ou preventiva em nossos servidores web. ;]

**C**hega de lenga lenga.. vamos ao que interessa!

**C**omo instalar?

![Arch Linux](/img/archlinux.png)
Bom, se você utiliza [Arch Linux](https://archlinux.org), pode pegar o pacote no AUR através do seguinte link(Lembrem-se de votar nele):

[https://aur.archlinux.org/packages.php?ID=20809](https://aur.archlinux.org/packages.php?ID=20809)

**F**eito isto, basta seguir o procedimento padrão para pacotes do AUR.


> 1- Descompactar: [kalib@tuxcaverna downloads]$ tar -xvzf dirbuster.tar.gz

2- Entrar no diretório dirbuster e gerar o pacote: [kalib@tuxcaverna dirbuster]$ makepkg

3- Instalar o pacote: [kalib@tuxcaverna dirbuster]$ sudo pacman -U dirbuster-0.12-1-x86_64.pkg.tar.xz


**F**eito. ;]

**O**utras distribuições? Não tenho certeza se o pessoal de outras distribuições empacotou o DirBuster, portanto provavelmente você vai precisar baixar o arquivo compactado do site oficial do projeto. Busquei tanto no Debian quanto no (K)Ubuntu 10.10 e em nenhum destes encontrei o DirBuster nos repositórios através do aptitude.

**P**ortanto se você não utiliza o Arch, segue link para download da ferramenta:

[https://www.owasp.org/index.php/DirBuster#tab=Download](https://www.owasp.org/index.php/DirBuster#tab=Download)

**M**as qual a cara do bicho?

![DirBuster](/img/dirbuster1.png)


**C**omo explicado antes, ele funciona parecido com o Dirb, porém em uma interface GUI (gráfica).

**C**omo podem ver, é uma interface simples e de fácil manuseio.

**V**amos aos pontos:

1- No campo Target, você deverá inserir a URL que será alvo do seu brute-force de diretórios e arquivos web.

2- Logo abaixo, vocẽ escolhe se deseja utilizar apenas GET ou HEAD & GET.

3- Agora é a vez de escolher quantas threads deseja utilizar. Quanto maior o número de threads, mais requisições simultâneas você estará utilizando. ;]

4- Abaixo você deverá escolher o tipo de brute-force.

* Baseado em dicionários, neste caso você deverá escolher um dos dicionários que a ferramenta já lhe disponibiliza através do seguinte caminho: /opt/DirBuster/

* Baseado em brute-force puro. Neste caso você deverá escolher qual conjunto de caracteres deseja que sejam utilizados, quantidade mínima e máxima de caracteres por tentativa.

5- Em seguida é a hora de escolher os filtros: Deseja fazer brute-force apenas em diretórios? Arquivos também? Modo recursivo? Extensões em branco? Diretório inicial para o scan? Que tipo de extensão?...

**E** como é o retorno que ele me dá?

**B**om, vou escolher meu alvo e vou utilizar 20 threads em simultâneo. Não vou deixar a ferramenta rodando por muito tempo pois estou apenas fazendo uma demonstração.


![DirBuster](/img/dirbuster2.png)


**C**omo podem ver, filtrei para que o brute-force comece no diretório raiz (/) e optei pela maior lista que o DirBuster possui como dicionário. Deixei rodando por cerca de 30 segundos e já tive o seguinte resultado.


![DirBuster](/img/dirbuster3.png)


**A** parte borrada no início da imagem não é um defeito. Vocês não pensaram que eu iria deixar exposto o alvo no qual fiz o teste, certo? No caso, optei pelo site de uma de nossas instituições de ensino. Bom, a imagem acima é um exemplo do retorno que consigo com a ferramenta. É a visão em Lista. Além dela você pode optar pela visão em árvore, que, como o nome informa, lhe trará a árvore de diretórios, sub-diretórios e arquivos que foram encontrados. Abaixo um exemplo da visão em árvore:


![DirBuster](/img/dirbuster4.png)


**O** resultado, mesmo deixando a ferramenta rodando por apenas 30 segundos, foi o esperado. Um espantoso caso de descuido com a segurança do site.

**A**lguns leves exemplos que me chamaram a atenção na lista que consegui:


![DirBuster](/img/dirbuster5.png)


**U**m diretório de administração exposto desta forma indicando que existe uma sessão do site com acesso permitido apenas para administradores. O que isso me leva a crer? Que quem conseguir acesso a esta sessão consegue manipular o sistema do site? O que nos leva a uma posterior análise da página index contida nele que possibilita uma tentativa de brute-force de login e senha? psssiiiuuuu... o.O

![DirBuster](/img/dirbuster6.png)
**U**m diretório para uploads de Arquivos? Que tal testar ele? Apenas usuários administradores possuem permissão de upload? Ou alunos conseguem fazer upload de fotos, trabalhos, por exemplo? De qualquer forma, isso me indica que existe a possibilidade de eu subir arquivos para o servidor deles. Que tal um script em php que me permitiria ter um console shell no servidor deles para inclusão, exclusão, edição, etc.. ? pssiiiiuuuu... o.O

![DirBuster](/img/dirbuster7.png)

**P**ode não ser nada.. mas também pode ser muita coisa.. O que um diretório chamado "gerencia" faz ali tão exposto e com um nome tão... tão... discreto? o.O psssiiiuuuuu

**A**cho que já conseguiram entender um pouco do que o DirBuster faz.

**A**braços!

**H**appy Hacking...

**PS: Wrong developers! Never play security by obscurity!**
