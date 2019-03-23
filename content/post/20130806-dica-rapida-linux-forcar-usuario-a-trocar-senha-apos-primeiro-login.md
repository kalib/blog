---
title: "Dica Rápida Linux: Forçar Usuário a Trocar Senha Após Primeiro Login"
date: 2013-08-06T09:23:58-05:00
draft: false
image: "/img/password.jpg"
tags: [
"Linux",
"Software Livre",
"Servidor",
"SysAdmin",
"Senha",
"Segurança",
]
description: "Como forçar usuário a alterar senha após primeiro login."
---
**É** natural encontrar pessoas que utilizam linux a pouco tempo (ou que nunca utilizaram) questionando aspectos simples que seguem o jargão: *"Mas no Windows eu consigo fazer isso, enquanto que no Linux...."*

**I**sso não é novidade alguma e, de fato, é um comportamento esperado, embora demonstre apenas uma mera falta de boa vontade para pesquisar um pouco. O fato é que por vezes encontro pessoas me questionando sobre alguns destes *recursos em falta* no mundo Linux, quando na verdade eles, na grande maioria das vezes, não estão em falta.

**U**m exemplo recente disto se deu quando um colega (não se preocupe, seu nome está a salvo) me perguntou:

> *Porque o Linux não tem um recurso para forçar um usuário a alterar sua senha após o primeiro login? Isso me ajuda muito como SysAdmin em servidores Windows.*

**D**evo concordar com ele no sentido de que este recurso ajuda muito a vida de um SysAdmin. Sejamos justos, as pessoas vivem esquecendo suas senhas. Segunda-feira? Dia mundial do *"esqueci minha senha, você pode gerar outra por favor?"*

**P**orém, devo acrescentar que o Linux possui sim este recurso (e não é uma novidade). O chage é apenas uma das formas de o fazer.

**S**upondo que seu usuário esqueceu sua senha e você deseja possibilitar que ele crie uma nova. Você pode mudar a senha dele para qualquer coisa que venha em sua mente, por exemplo: *123mudar456*

**E**m seguida, digite o seguinte comando:

```
 # chage -d 0 {nome-do-usuário}
```

**S**upondo que eu tenha alterado a senha do usuário *linus.torvalds* para *123mudar456*, o comando seria:

```
 # chage -d 0 linus.torvalds
```

**O** parâmetro -d determina quando a senha do mesmo expira, ou deverá ser trocada. Você pode determinar uma data específica, a qual deve ser indicada no formato AAAA-MM-DD, porém, ao invés da data, o 0 indica que a senha deverá ser trocada após o primeiro login daquele usuário.

**S**imples, certo?!
