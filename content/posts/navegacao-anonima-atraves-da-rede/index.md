---
title: "Navegação Anônima Através Da Rede Tor"
date: 2013-12-30T19:35:09-05:00
draft: false
tags: [
"Segurança",
"Software Livre",
"Redes",
"Impressões",
]
categories: ["Tutoriais"]
description: "Entenda um pouco sobre a rede Tor e veja como utilizá-la"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
**O** ano de 2013 foi marcado por diversos acontecimentos de grande repercussão e, sem sombra de dúvidas, um deles foi o escândalo causado pelo vazamento de dados que provaram que algumas instituições, dentre elas a *NSA* (Agência de Segurança dos Estados Unidos), estavam espionando diversas entidades, empresas e até mesmo pessoas físicas nas mais diversas regiões do globo. Escândalos a parte, estas denúncias acabaram despertando um pouco mais (uma pena que não tenha sido muito mais) nas pessoas o interesse em preservar sua privacidade.

<!--more-->

## Tor

**S**endo assim, nada mais justo que dedicar o último post do ano ao *Tor*.

**A**inda vivemos em um mundo (principalmente nosso querido Brasil) onde as pessoas parecem simplesmente não se importar com o quesito privacidade. Não é difícil encontrar praticamente todas as informações sobre a maioria das pessoas pois elas mesmas fazem questão de expor isto aos sete ventos através das redes e mídias sociais. Divulgam dezenas de fotos de sua pessoa, bem como de seus parentes, informam dados pessoais, rotinas, locais que frequentam, horários, local de trabalho, amizades, áreas de interesse, etc. A lista é longa...

**F**elizmente algumas pessoas já começaram a atentar para a importância de manter sua privacidade e, para tal, passaram a buscar maneiras alternativas para *driblar* estas *espionagens* e outras formas de golpes tão comuns hoje em dia, mantendo assim o seu anonimato durante sua navegação. A rede **Tor**, ou *The Onion Router*, surgiu basicamente com este propósito.

**T**or é uma rede de computadores distribuída cuja finalidade primária é manter o anonimato na *internet*. Seu objetivo básico é garantir a privacidade e anonimato do usuário que está navegando através desta rede. Apesar de o uso do Tor ser simplificado em sistemas GNU/Linux, visto que a maioria das distribuições disponibilizam o pacote do Tor, também existem versões para sistemas Windows e Mac OS. Trata-se de uma rede de túneis criptografados, onde os roteadores da rede são computadores de usuários comuns que estão rodando um programa e possuem acesso à internet.

**B**asicamente, o usuário instala um programa, *tor-cliente*, em seu computador e este funcionará como um *proxy* para o mesmo. Os demais programas que o usuário utiliza para navegar na internet *(navegador, emule, etc.)* deverão ser configurados para navegar através deste proxy.

**A** partir daí, quando o usuário digitar em seu navegador o endereço destino, *https://www.google.com* por exemplo, ao invés de a sua requisição passar por roteadores convencionais para atingir o destino, ele passará por túneis criptografados da rede Tor, para então chegar ao seu destino. Nestes túneis a sua requisição e informações passam por vários *nós* da rede Tor. Um exemplo prático do anonimato provido pelo Tor é a verificação do seu próprio IP. Caso deseje realizar o teste, experimente acessar o endereço *https://www.meuip.com.br* antes de instalar e configurar o Tor em sua máquina. Este site lhe informará o seu atual endereço IP, no caso o endereço será o do seu roteador de acesso à rede pública. Em seguida, tente acessar novamente o mesmo endereço após ter instalado e configurado o Tor. Você perceberá que ele não retornará o seu endereço IP, mas sim um endereço IP qualquer de um nó da rede Tor. Ou seja, isto atrapalha a vida de quem quer que deseje rastrear a sua máquina a partir de um endereço que você tenha acessado ou mesmo de um email que você tenha enviado.

## Instalação do Tor no Arch Linux

**C**onforme informei no início, o Tor está disponível para praticamente todos os sistemas operacionais, mas vou focar a explicação de instalação para a minha distribuição, Arch Linux, embora o processo seja bastante similar para as demais distribuições GNU/Linux.

1. Instale o Tor:

```bash
 [kalib@tuxcaverna ~]$ sudo pacman -S tor
```

2. Como um opcional, instale também o frontend ou GUI, o qual é desenvolvido em QT, *vidalia*. O vidalia, além de controlar o processo Tor, permite-lhe ver e configurar o status do Tor, monitorar o uso de banda e ver, filtrar ou realizar pesquisas em mensagens de log, etc.

```bash
 [kalib@tuxcaverna ~]$ sudo pacman -S vidalia
```

3. Inicie/Habilite o serviço utilizando o *systemd*.

**A** configuração padrão do Tor deverá funcionar para a maioria dos usuários, mas caso deseje alterar algo, verifique a documentação do Tor e altere as configurações em */etc/tor/torrc*.

4. Para utilizar um programa através da rede Tor, configure-o para utilizar o endereço *127.0.0.1* ou *localhost* como endereço proxy *SOCKS5* através da porta *9050* (porta padrão do Tor) ou *9051* (porta padrão utilizada quando se configura com o vidalia).

**A** rede Tor possui uma espécie de domínio próprio com terminação *.onion*, acessível apenas através da própria rede Tor. Páginas com este domínio são parte da chamada Deep Web, mas isto será assunto para um outro post por ser algo polêmico e extenso.

**D**esejo a todos um feliz ano novo com liberdade, privacidade e anonimato, quando necessário!

**H**appy Hacking!
