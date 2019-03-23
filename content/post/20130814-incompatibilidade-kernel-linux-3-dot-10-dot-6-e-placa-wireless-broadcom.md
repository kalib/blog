---
title: "Incompatibilidade Kernel Linux 3.10.6 E Placa Wireless Broadcom"
date: 2013-08-14T09:04:10-05:00
draft: false
image: "/img/kernel_panic.gif"
tags: [
"Linux",
"Redes",
"Software Livre",
"Arch Linux",
]
description: "Incompatibilidade causada entre kernel 3.10.6 e placas wireless broadcom resultam em kernel panic."
---
**K**ernel Panic!

**F**oi com isto que me deparei logo pela manhã ao chegar no trabalho ao ligar meu notebook.

**O**ntem rodei o habitual *# pacman -Syu* para atualizar meu Arch Linux. Dentre as atualizações estava a nova versão estável do kernel linux: 3.10.6

**H**oje descobri que esta versão estava resultando em kernel panic para os usuários de placas wireless *Broadcom*. Sim, sou um deles.

**A**pós algumas buscas em logs, percebi que o problema estava se dando com minha placa wireless. Logo ao iniciar o notebook, após alguns segundos, tudo travava e eu recebia o aviso de **kernel panic**.

**D**esabilitando minha placa wireless consegui manter o notebook funcionando normalmente. Ao pesquisar, percebi que tratava-se de bug nesta versão do kernel.

**S**olução? Downgrade no kernel linux.

**P**ara quem ainda possui a versão anterior, *3.10.5* no diretório de cache, basta executar a instalação local com o pacman.

**C**om o seguinte comando você pode descobrir se o pacote ainda se encontra em seu cache:

```
 $ ls /var/cache/pacman/pkg/ | grep linux-3.10.5
```

**C**aso você ainda possua o pacote, basta digitar:

```
 # pacman -U /var/cache/pacman/pkg/linux-3.10.5-1-x86_64.pkg.tar.xz
```

**O** nome do pacote poderá ser diferente para você. Estou utilizando a versão x64 do Arch.

**P**or enquanto é melhor manter a versão anterior do kernel, ao menos até o lançamento da versão 3.11, na qual o problema já foi resolvido.
