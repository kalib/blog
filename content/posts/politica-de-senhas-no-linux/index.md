---
title: "Política De Senhas No Linux: Senhas Com Data Para Expirar"
date: 2011-09-27T11:36:19-05:00
draft: false
tags: [
"Arch Linux",
"Cultura Hacker",
"Debian",
"Impressões",
"Linux",
"Segurança",
"Software Livre",
]
categories: ["Tutoriais"]
description: "Já é sabido que a maior vulnerabilidade dentro das empresas ou mesmo computadores pessoais são justamente os próprios funcionários ou usuários."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**J**á é sabido que a maior vulnerabilidade dentro das empresas ou mesmo computadores pessoais são justamente os próprios funcionários ou usuários.

<!--more-->
p
![Securepassword](securepasswords.jpg)
**M**anter uma boa **política de senhas** é fundamental para garantir a segurança **básica** de suas informações e tentar minimizar as chances de alguém ter acesso indevido a elas.

**V**ou apresentar dicas simples para uma boa política de senhas que vão além das dicas comuns como "mantenha uma senha que contenha mais de 8 caracteres, sendo eles números, letras minúsculas, maiúsculas e caracteres especiais...".

**E**m escritórios é importante haver também uma política de troca de senha regular. Mas, como forçar os usuários a seguirem essa política de troca de senha regular? Evitando que o mesmo se logue caso não troque a senha de forma periódica, claro.

**V**amos começar pelo básico.

## 1 Definindo a complexidade das senhas de usuários:

**P**ara definir a complexidade, precisaremos alterar a configuração de definição de senhas de usuários. O processo pode ser diferente, de acordo com a sua distribuição.

[_**A**rch Linux:_](https://www.archlinux.org/)

**E**dite o arquivo _passwd_ que se encontra em _/etc/pam.d/passwd_, da seguinte forma:

> # vim /etc/pam.d/passwd_

_[**D**ebian](https://www.debian.org), [Ubuntu](https://www.ubuntu.com), [RedHat](https://www.redhat.com), [CentOS](https://www.centos.org)_ e outras:

> # vim /etc/pam.d/system-auth_

**C**onfigure sua linha de senha de forma que fique algo parecido com o seguinte:

_password       required        pam_cracklib.so difok=3 minlen=10 ucredit=3 ocredit=2 retry=3_

**O**nde:

_difok=3_ -> Informa a quantidade de caracteres que podem se repetir em relação à última senha. Por exemplo: Se minha antiga senha era "kalib" e eu tento usar "kalamba" como nova senha, receberei uma informação de erro, pois eu repeti 3 letras que já existem na senha anterior "kal".

_minlen=10_ -> Informa qual a quantidade mínima de caracteres aceitos para a senha do usuário. No![](https://securepasswordgeneratortips.com/wp-content/uploads/2011/05/securepasswordgeneratortips08.jpg) exemplo, o mínimo de caracteres aceitos serão 10, caso contrário será apresentada uma mensagem de erro solicitando que o usuário tente uma nova senha.

_ucredit=3_ ->Informa a quantidade de letras maiúsculas que deverão compor minha senha. No exemplo, eu precisarei utilizar no mínimo 3 letras em maiúsculo, ou "Uper Characters" em minha nova senha.

_ocredit=2_ -> Informa a quantidade de "outros caracteres" ou caracteres especiais, como por exemplo *, &, %, $, _, etc.

_retry=3_ -> Informa quantas vezes o usuário vai poder tentar, em caso de senha indevida, antes de receber a mensagem de erro.

**O**utros parâmetros que podem ser utilizados são os seguintes:

_dcredit=x_ -> Informa a quantidade de dígitos que deverão ser utilizados como números na senha, onde x é o número mínimo desejado.

_lcredit=x_ -> Informa a quantidade de caracteres minúsculos, ou "Lower Characters", mínimos em sua senha.

## 2 Definindo que a nova senha não poderá ser igual às anteriores:

**N**o **mesmo arquivo** do ponto anterior, iremos inserir o parâmetro remember na linha conforme exemplo:

_password sufficient pam_unix.so use_authtok md5 shadow remember=10_

_remember=10_ -> Informa que a nova senha não poderá ser igual às últimas 10 senhas utilizadas por este usuário.

**A**gora que já sabemos como definir uma política para criação de senhas seguras, vamos conhecer o "**chage**", que nos ajudará a definir a política de datas ou validade das senhas.

## 3 Checando as políticas de validade de senhas de um determinado usuário:

_# chage -l usuário_

**O** comando acima verifica os atributos de validade daquela senha, e lhe retornará algo similar ao seguinte:

    <em>Last password change : May 11, 2011</em>
    <em>Password expires : never</em>
    <em>Password inactive : never</em>
    <em>Account expires : never</em>
    <em>Minimum number of days between password change : 0</em>
    <em>Maximum number of days between password change : 99999</em>
    <em>Number of days of warning before password expires : 7</em>

**A**s informações acima apresentam dados como data de última mudança de senha, data de expiração, tempo de inatividade, quantidade mínima de dias para se trocar a senha antes de a mesma expirar, etc.

## 4 Criando uma "validade" ou período de expiração para a senha de um determinado usuário:

    <em># chage -M 99999 linustorvalds</em>

**E**ste comando vai **desabilitar** o período de validade da senha, informando que a mesma não expira nunca.

    <em># chage -M 50 -m 7 -W 7 linustorvalds</em>

**E**ste comando **aplica** a política de senha para o usuário "linustorvalds" onde:
_-M 50_ -> Define que a senha será válida por um máximo de 50 dias, quando a mesma deverá ser trocada.
_-m 7_ -> Define o número mínimo de dias em que o usuário poderá trocar sua senha antes do período especificado para expiração. Caso o usuário possa trocar a qualquer momento ou dia, o valor deverá ser 0.
_-W 7_ -> Número de dias antes de expirar nos quais o usuário vai receber alertas informando que sua senha irá expirar.

**C**aso você deseje especificar um **dia exato** no qual a senha de determinado usuário vai expirar, você pode utilizar o parâmetro _-E_ seguido da data desejada no formato _AAAA-MM-DD_.

**A**lém disto, você pode desejar que a senha do usuário "linustorvalds" seja trocada no **próximo login** do mesmo. Neste caso você poderá utilizar o parâmetro _-d_, conforme exemplo abaixo:

    <em># chage -d 0 linuxtorvalds</em>

**O** _-d_ significa quantidade de dias também.

    <strong>É</strong> isso pessoal.

    <strong>H</strong>ave fun!
