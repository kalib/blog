---
title: "Brute Force: O Dirb Lhe Ajuda a Identificar Vulnerabilidades Em Seu Site"
draft: false
tags: [
"Cultura Hacker",
"Impressões",
"Linux",
"Redes",
"Segurança",
"Software Livre",
]
categories: ["Tutoriais"]
description: "Para quem não conhece, brute-force é um ataque bastante utilizado em serviços web tais como smtp, pop, ssh, ftp, iax dentre outros. Se você possui um serviço web ele já sofreu tentativa de ataques brute-force."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpeg"
lightgallery: true
---

Para quem não conhece, brute-force é um ataque bastante utilizado em serviços web tais como smtp, pop, ssh, ftp, iax dentre outros. Se você possui um serviço web ele já sofreu tentativa de ataques brute-force.

<!--more-->

## Brute-Force

![Sherlock Tux](sherlocktux.jpg)

**P**ara quem não conhece, brute-force é um ataque bastante utilizado em serviços web tais como smtp, pop, ssh, ftp, iax dentre outros. O ataque consiste em basicamente forçar o login em um determinado serviço que esteja disponível online sem que seja necessário um ataque mais sofisticado ou mesmo grandes conhecimentos sobre a tecnologia em questão. Por ser demasiado simples de se utilizar, visto que existem milhares de ferramentas disponíveis na web para este fim, ele acaba se tornando um dos pesadelos mais constantes dos administradores de redes, SysAdmins, etc.. Pois é um fato! Se você possui um serviço web ele já sofreu tentativa de ataques brute-force. Não sofreu? <del>Não se preocupe, sofrerá.</del> ;]

**B**asicamente existem duas formas de brute-force, porém eu prefiro acrescentar uma terceira forma...rude e grosseira, mas infelizmente já vi casos de pessoas que se utilizam dela, então vamos lá.

**1- Brute-force puro ou força bruta**, como prefiram chamar. É a metodologia mais lenta por testar uma quantidade absurdamente grande de combinações de caracteres de forma aleatória a ponto de tentar descobrir uma senha. Utilizam-se classes como "alfanuméricos", "numéricos", "caracteres especiais", etc. Por exemplo. Supondo que nosso alfabeto fosse composto apenas das letras A, B e C. E alguém possui uma senha para um determinado serviço web. Com uma tentativa de brute-force, eu utilizaria uma ferramenta que geraria todas as combinações possíveis a partir destas 3 letras, o que, ocasionalmente, me daria acesso ao serviço uma vez que, com certeza, eu descobriria a senha, uma hora ou outra. A ferramenta começaria a tentar logar com senhas da seguite forma:

_A
AA
AB
AC
B
C
BA
BB
BC
C
CA
CB
CC
AAB
AAC
...
ACABCAACB
..._

**E**nfim.. Faria todas as combinações possíveis com estas 3 letras e cedo ou tarde teria sucesso no login, mas... Nosso alfabeto não é tão pequeno assim. E os números? E os caracteres especiais como %, $, #, @, !, ", &, *, (, ), _, -, +, =, etc...

**J**á imaginaram quantas tentativas seriam necessárias para se descobrir uma senha, por exemplo, de 8 caracteres contendo letras, números e caracteres especiais? o.O

**Q**uanto mais demorado for o ataque, mais fácil será de detectar o mesmo.

**P**or estas questões ele acaba não sendo a primeira opção para estes ataques. O que nos leva para a outra forma que é a mais utilizada hoje em dia e que tem se provado a mais eficaz, graças a triste realidade global de que as pessoas não possuem uma política eficiente para escolha de suas senhas, acabando por utilizar senhas com palavras reais como: deus, amor, sexo, felicidade, vida, segredo, secreto, etc.

**2- Dicionários.** Lembra daquela sua senha que faz sentido e você gosta porque é fácil de lembrar? Aquela "deus", "amor", "sexo", "cerveja", etc. Bom, ela corre grandes riscos se passar por um ataque que se utiliza de dicionários. Ataques com dicionários se resumem ao mesmo ataque de tentativa e erro com o intuito de logar em algum serviço web porém de forma mais coerente e orientada. O dicionário consiste em classes ou arquivos texto com uma sequência de palavras pré-formatadas pelo atacante que serão utilizadas como banco de possíveis senhas, ao invés de simplesmente sair tentando todas as combinações possíveis.

**S**upondo que sua senha seja "cachorro". Eu lanço o ataque de brute-force utilizando-me de um dicionário de strings, ou palavras, que possui as seguintes:

_amor
deus
ceu
inferno
gato
sexo
divino
futebol
cachorro
galinha
absurdo
segredo
naodigo
...
..._

**C**omo percebem, é uma simples lista de palavras. Porém, notaram que cachorro está na lista? Bom, o meu ataque faria de forma automática tentativas de login com todas estas possíveis senhas até chegar na vez do "cachorro", que me resultaria em acesso à sua conta, serviço, site, aplicação ou o que quer que estivesse <del>"protegido"</del> com a senha "cachorro".

**I**magine a mesma senha cachorro. Já imaginou quantas tentativas seriam necessárias com a outra técnica? A de combinações? Imaginem quantas milhares de combinações seriam necessárias utilizando o nosso alfabeto completo até chegarmos à palavra cachorro. o.O

**C**laro, quem utiliza-se de dicionários não possui um arquivo de strings tão curto como este exemplo que dei. Sequer apenas em português. Geralmente pegam arquivos prontos na internet que são feitos para este fim que possuem uma infinidade de palavras incluindo nomes próprios, cidades, países, objetos, frutas, etc, etc...

**O**u seja, se você possui uma senha bobinha e que é uma palavra de fato e faz sentido, pense seriamente em trocar por algo um pouco mais elaborado. ;]

**S**obre a terceira metodologia, se é que pode-se chamar assim, não possui um nome definido, sequer é reconhecida, porém vou citar pois ela é utilizada em casos mais amadores e específicos. O que seriam esses casos amadores e específicos? Algo como:

_* mulher/namorada tentando descobrir senha do orkut/email/etc do marido/namorado ou o oposto;
* jovens tentando descobrir senha de orkut/email/msn/etc de outros jovens por qualquer motivo que o seja (briguinha na escola? o.O)..._

**3- Lammer ou seja o que deus quizer**. Independente da causa e das pessoas envolvidas, por mais incrível que possa parecer, esta modalidade ainda é bastante utilizada por aí. E o que é pior? Muitas vezes funciona. Não tirando os "méritos" o.O da pessoa que consegue a senha de alguém através dessa "metodologia", mas em 100% 200% dos casos a culpa é apenas da vítima que insiste em colocar uma senha mais do que absurda como por exemplo: telefone de casa, seu número de celular, sua data de nascimento, seu sobre-nome, nome da namorada, nome da sogra <del>(que o Diabo carregue)</del>, etc.

**A**cho que já ficou claro como essa metodologia funciona, certo? Caso não, é o seguinte. É um "ataque" (será que possa chamar assim?) completamente direcionado, como informei anteriormente, onde se conhece o alvo e sabe algumas informações sobre essa pessoa. Neste caso a pessoa, supondo a esposa, não vai se utilizar de tecnologia alguma ou ferramenta específica. Vai apenas abusar do bom senso, torcer pela inocência do marido e tentar a sorte.

**E**la abre algum navegador de internet, acessa a página do orkut do cidadão e começa a tentar senhas como: telefone de casa, data de nascimento dele(a), etc.. Sim, ainda hoje vejo senhas simples como estas citadas e que acabam por ser descobertas. E o que é pior? As pessoas dizem: "Hackearam minhas senha!" hehehe... Parece exagero? o.O

**A**credito que já falei demais sobre isso. Resumi as 2 técnicas utlizadas para ataques de brute-force. Mas este não é o objetivo principal do post, mas sim como inspecionar seu site ou sistema web para saber se o mesmo está livre de tentativas deste tipo de ataque.

**P**ara isso utilizaremos a ferramenta [Dirb](https://sourceforge.net/projects/dirb/).

## Dirb

**D**irb é um URL Bruteforcer. Um WCS (Web Content Scanner) que tem a função de buscar por objetos Web Ocultos. Basicamente funciona com o lançamento um ataque baseado em dicionários contra o servidor Web analizando as respostas do mesmo. O principal propósito do Dirb é auditoria em aplicações web.

**D**entre as funcionalidades avançadas do Dirb destacam-se:

_* setar diferentes cookies;
* adicionar qualquer tipo de HTTP header desejado;
* utilizar proxys para mascarar a conexão;
* utilizar catalogos ou arquivos utilizando dicionários definidos ou templates fazendo uma varredura direcionada._

**M**as, chega de falatório.. vamos ao teclado...

**A** ferramenta pode ser baixada através do site do projeto: [Dirb](https://sourceforge.net/projects/dirb/.

**A** compilação é simples e sem grandes mistérios.

**D**escompacte o arquivo e compile seguindo os seguintes comandos:

```bash
tar -xvzf dirb203.tar.gz

cd dirb/

./configure

 make
```

**P**ronto. O Dirb está pronto para funcionar.

**S**e você apenas executar o dirb sem nenhum parâmetros lhe serão apresentados os parâmetros deisponíveis para utilização bem como uma descrição dos mesmos:

```bash
./dirb
-----------------

DIRB v2.03

By The Dark Raver

-----------------
```

```bash
./dirb <url_base> [<wordlist_file(s)>] [options]
```

```bash
./dirb
-----------------DIRB v2.03    By The Dark Raver-----------------
```

```bash
./dirb <url_base> [<wordlist_file(s)>] [options]
....
....
....
```

**M**as vamos ao uso mais básico.

**A** forma mais simples é utilizando apenas a URL que deseja testar, por exemplo _https://www.meulaboratorio.com.br_

```bash
./dirb https://www.meulaboratorio.com.br

-----------------
DIRB v2.03
By The Dark Raver
-----------------

START_TIME: Mon Jan 31 10:05:16 2011
URL_BASE: https://www.meulaboratorio.com.br
WORDLIST_FILES: wordlists/common.txt
-----------------

GENERATED WORDS: 1942

---- Scanning URL: https://www.meulaboratorio.com.br/ ----

+ https://www.meulaboratorio.com.br/A

(FOUND: 301 [Moved Permanently] - Size: 241)

+ https://www.meulaboratorio.com.br/a

(FOUND: 200 [Ok] - Size: 419)

+ https://www.meulaboratorio.com.br/about

(FOUND: 301 [Moved Permanently] - Size: 237)

+ https://www.meulaboratorio.com.br/accessibility/

==> DIRECTORY

+ https://www.meulaboratorio.com.br/account

(FOUND: 302 [Moved Temporarily] - Size: 227)

+ https://www.meulaboratorio.com.br/accounts

(FOUND: 302 [Moved Temporarily] - Size: 192)

+ https://www.meulaboratorio.com.br/ad

(FOUND: 301 [Moved Permanently] - Size: 223)

+ https://www.meulaboratorio.com.br/ads/

==> DIRECTORY
...
...
...
```

**C**ortei pois o resultado seria muito grande. Mas, como podem ver, ele escaneia a URL por diretórios, arquivos, etc., que podem ser alvos de tentativas de brute-force.

**N**o nosso simples caso, vi que foi identificado um diretório chamado accounts. Este diretório já é um alvo que merece ser inspecionado com mais atenção pois as chances de ele conter algo que interesse ao invasor são grandes.

**C**omo eu havia dito, o Dirb funciona com dicionários. Como devem ter reparado, eu não setei nenhum dicionário, portanto ele utilizou o dicionário padrão "common". Mas você pode especificar manualmente qual dicionário deseja utilizar.

**N**o diretório dirb, você encontrará um diretório chamado wordlists que conterá os dicionários disponíveis.

```bash
cd wordlists/

ls

big.txt  catala.txt  common.txt  euskera.txt  extensions_common.txt  indexes.txt  mutations_common.txt  others  small.txt  spanish.txt  stress  vulns
```

**B**om, e sobre parâmetros?

**V**ou apresentar apenas algumas possibilidades.

**P**ara utilização de um dicionário em específico, basta adicionar o nome do dicionário desejado ao final do comando:

```bash
./dirb https://www.meulaboratorio.com.br euskera.txt
```

**P**ara utilização de SSL, apenas inclua o HTTPS na url desejada:

```bash
./dirb https://www.meulaboratorio.com.br euskera.tx -i
```

**V**ocê também pode utilizar múltiplos dicionários separando-os com vírgulas:

```bash
./dirb https://www.meulaboratorio.com.br euskera.txt,common.txt,spanish.txt,big.txt
```

**A**lém disto você pode filtrar sua busca por uma extensão em específico através do parâmetro -X:

```bash
./dirb https://www.meulaboratorio.com.br euskera.txt -X .asp,.php,.jsp
```

**A**gora é sair experimentando combinações e testando os resultados. Seja criativo em seus testes. ;]

**A**braços!