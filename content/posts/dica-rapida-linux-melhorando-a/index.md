---
title: "Dica Rápida Linux: Melhorando a Política De Senhas"
date: 2013-08-12T09:14:55-05:00
draft: false
tags: [
"Linux",
"Software Livre",
"Segurança",
"Impressões",
]
categories: ["Tutoriais"]
description: "Dica rápida sobre como melhorar a política e a segurança de senhas de usuários no Linux."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**N**a dica anterior mencionei o uso do comando *chage* para **forçar** a alteração de senha por parte do usuário em sistemas Linux, mas e se além disso fosse possível melhorar esta prática, tornando a política de senhas de usuários um pouco mais segura?

<!--more-->

**O** chage também nos permite determinar quando exatamente desejamos que a senha daquele usuário expire, de forma que ele será forçado a criar uma nova senha após aquele determinado tempo. Isto é útil para evitar que uma mesma senha seja utilizada por um longo período de tempo. Particularmente, gosto de forçar a alteração de senhas a cada mês, mas isto dependerá dos sistemas que são acessados e de o quão críticas são as informações e sistemas em jogo.

**O** comando chage altera o número de dias entre a alteração de uma senha e sua alteração anterior. Esta informação fica registrada no sistema e será utilizada para determinar quando um usuário precisará alterar sua senha. Esta e outras configurações ficam armazenadas no arquivo de configuração */etc/login.defs*, o qual determina configurações que englobem todos os usuários do sistema, inclusive um período máxima antes de uma senha ser expirada.

**P**ara verificar as informações sobre quando uma senha expira, digite:

```bash
 # chage -l NomeDoUsuário
```

**O** comando acima lhe retornará informações gerais sobre aquela conta, tais como: data de última mudança de senha, data em que a senha irá expirar, status atual da senha, número mínimo entre troca de senhas, número máximo entre troca de senhas, número de dias de alerta sobre a mudança de senha, etc.

**P**ara efetivar a política de mudança de senha após um determinado tempo, você pode editar diretamente o arquivo */etc/shadow* ou utilizar o comando *chage*, o qual recomendo ao invés do /etc/shadow.

**N**o arquivo */etc/shadow*, a ordem dos campos é a seguinte:

```bash
 {NomeDeUsuário}:{Senha}:{ÚltimaAlteraçãoDeSenha}:{Minimum_days}:{Maximum_days}:{Warn}:{Inactive}:{Expire}:
```

Onde,

**Minimum_days:** *Quantididade mínima de dias entre alterações de senhas. Ex: Uma quantia mínima de dias antes que o usuário possa alterar sua senha.*

**Maximum_days:** *Quantidade máxima de dias pelos quais uma senha será válida (após os quais os o usuário é forçado a alterar sua senha).*

**Warn:** *O número de dias antes de a senha expirar nos quais o usuário será alertado sobre a sua senha expirar em breve.*

**Expire:** *Quantidade de dias desde 1 de Janeiro de 1970 em que a conta expirará. Ex: uma data específica pode ser informada para que, a partir de tal data a senha não possa mais ser utilizada.*

**C**onforme informei anteriormente, costumo recomendar a utilização do comando *chage* ao invás da edição do arquivo */etc/shadow*, o que minimiza as chances de erros.

```bash
 # chage -M 60 -m 7 -W 7 NomeDeUsuário
```

**O**nde, da mesma forma, **M** = Maximum_days, **m** = Minimum_days e **W** = warn.

**B**om proveito...
