---
title: "Habilitando Plugins No Chromium"
date: 2009-08-12T12:27:17-04:00
draft: false
tags: [
"Arch Linux",
"Chromium",
"Impressões",
"Linux",
"Software Livre",
]
categories: ["Tutoriais"]
description: "Todos que visitam meu blog já devem estar cansados de saber que estou bastante atento ao projeto chromium e ajudando com o possível para o desenvolvimento e divulgação do mesmo."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**T**odos que visitam meu blog já devem estar cansados de saber que estou bastante atento ao projeto chromium e ajudando com o possível para o desenvolvimento e divulgação do mesmo.

<!--more-->

## Introdução

**B**om, eu já havia notificado aqui o fato de que o chromium passou a ter suporte à plugins como o flash por exemplo. De fato ele já suporta os plugins, mas muitos estão me procurando para perguntar porque o chromium cortou o suporte aos plugins, visto que nas builds mais recentes do chromium o suporte parece ter sido cortado, visto que os plugins não estão funcionando a princípio.

![Shot Chromium](shot_chromium.png)

**M**as, porque o meu funciona e o seu não?

**N**ão é mágica..nem feitiçaria.. Vamos à realidade.

**O** suporte do chromium aos plugins ainda não está 100% homologado, o que resulta em alguns bugs e falhas, podendo acarretar em uso excessivo de processamento ou outros recursos de sua máquina em alguns casos. Sabendo disso, o projeto chromium adotou a posição de não tornar o suporte à plugins uma opção "default".

## Habilitando os plugins

**P**ara utilizar os plugins em seu chromium você precisa iniciar o seu chromium com o seguinte parâmetro:

```bash
$ chromium-browser --enable-plugins
```

**L**embrando que nestas novas builds do chromium não existe mais a necessidade de criar os links simbólicos para os plugins. Ele já detecta automaticamente os plugins que você tem instalados.

**P**ara testar, pode abrir o seguinte endereço ou mesmo acessar algo como o youtube:

about:plugins

**C**aso você ainda não tenha o chromium instalado em sua máquina pode buscar informações na internet sobre como o fazer em sua distro. Caso use Archlinux, pode seguir os passos abaixo:

### Repositórios

Adicione o repositório brasileiro em sua lista de mirrors. Adicione as seguintes linhas em seu arquivo /etc/pacman.conf :

```bash
[archlinuxbr]
Server = https://repo.archlinux-br.org/i686
```

ou
```bash
[archlinuxbr]
Server = https://repo.archlinux-br.org/x86_64
```

### Instalação

Atualize seus mirrors e instale o chromium:

```bash
# pacman -Sy
# pacman -S chromium-snapshot
```

**F**eito!

**B**om proveito...
