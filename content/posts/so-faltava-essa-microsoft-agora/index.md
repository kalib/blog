---
title: "Só Faltava Essa - Microsoft Agora Tenta Fragmentar O Odf"
date: 2009-05-12T10:48:18-04:00
draft: false
tags: [
"Cultura Hacker",
"Impressões",
"Software Livre",
]
categories: ["Impressões"]
description: "Tal como já havia feito com Java e HTML (só para citar dois casos), a Microsoft agora investiu ao menos 12 meses de trabalho para tentar fragmentar o ODF no mercado de TI: Uma vergonha."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**S**ó nos faltava essa eim!?

**D**epois de várias atitudes "questionáveis" em toda a sua história, me deparo com o seguinte texto publicado pelo [Vitorio Furusho](https://www.softwarelivre.org/news/13422).

<!--more-->

>
> _Tal como já havia feito com Java e HTML (só para citar dois casos), a Microsoft agora investiu ao menos 12 meses de trabalho para tentar fragmentar o ODF no mercado de TI: Uma vergonha._
>

Juro que tinha me preparado para publicar nesta semana um post elogiando a Microsoft por ter finalmente lançado o SP2 do Office 2007 com suporte nativo ao ODF, mas infelizmente após os testes iniciais de diversos usuários, o que vemos é uma tentativa absurda de enganar os consumidores (que pagaram pelo software) e fragmentar o ODF na indústria de TI.

Quando uso o termo fragmentar, me refiro á tática já conhecida de usar a ‘criatividade’ no momento de implementação de um padrão para tornar a sua implementação compatível apenas com a sua ferramenta (que já viu sites que só funcionam no Internet Explorer sabe bem do que estou falando). Por fora os documentos parecem idênticos e compatíveis mais por dentro são completamente diferentes, fragmentando assim a uniformidade esperada com a utilização de um padrão.

Um dos primeiros artigo publicado sobre o tema e para o qual chamo a atenção de todos é do Rob Weir, coordenador do OASIS ODF TC (grupo que desenvolve o ODF, do qual faço parte). Chega a ser assustador o que o Office 2007 faz com as planilhas existentes de ODF.

Os detalhes técnicos estão todos no blog do Rob, mas em resumo, quando se abre uma planilha ODF (extensão .ods) existente no Office 2007, ele simplesmente elimina todas as fórmulas existentes sem avisar nada ao usuário, deixando nas células apenas os valores do resultado do cálculo das fórmulas (valores estes já previamente armazenados no documento). Se um usuário quiser testar o suporte ao ODF no Office, e sem prestar a devida atenção salvar uma planilha aberta, vai sobrescrever o documento eliminando todas as fórmulas, como se estivesse gravando um documento que foi totalmente digitado. Já vi absurdos na vida, mas nada se compara a isso.

Quando se utiliza o Office 2007 para gerar uma nova planilha, as fórmulas serão armazenadas de tal forma que só o Office 2007 (ou o CleverAge, um plug-in de suporte ao ODF para o Office desenvolvido em Open Source com patrocínio da Microsoft) será capaz de ler o documento, acabando com a possibilidade de que qualquer outra aplicação existente seja capaz de ler o documento.

Enquanto o primeiro problema simplesmente joga fora toda a inteligência de negócios dentro das planilhas (as fórmulas), o segundo prende o usuário ao Office 2007 para sempre (já vimos este filme antes, não é mesmo ?).

A justificativa que a Microsoft poderia usar para isso é a falta de definição de fórmulas em planilhas no ODF 1.0/1.1. Interessante notar que no ODF 1.2 (que é desenvolvido com a participação da Microsoft) este problema já foi resolvido com a criação do OpenFormula.

Na primeira tabela comparativa do post do Rob, que resume um teste sobre o mesmo assunto que ele fez há algumas semanas, fica fácil perceber que mesmo sem existir a especificação de fórmulas para planilhas dentro do ODF 1.1, a interoperabilidade entre as aplicações existentes testadas (KOffice, OpenOffice, Google Docs, Symphony e o plug-in da Sun para o Office) existe de fato (com exceção do CleverAge que apresentou alguns problemas). Isso significa que todos os outros desenvolvedores não se preocuparam apenas em ‘cumprir com um requisito de norma’ (ou seja, conformidade), mas também em desenvolver uma aplicação realmente útil e interoperável para seus usuários. O Rob comenta ainda que o conjunto de fórmulas utilizado por todas estas aplicações (com base no OpenOffice) foi desenvolvido com base nas fórmulas existentes no Excel (no mínimo irônico, heim ?).

Destaco ainda que os problemas apresentados pelo OpenOffice quando o Rob repetiu seus testes com a versão nova da suíte, foram causados por que os desenvolvedores do OpenOffice 3.0 decidiram incorporar como padrão na ferramenta o suporte ao ODF 1.2 que ainda está em desenvolvimento do OASIS. Não questiono o que os levou a tal decisão, mas tenho orientado a todos os usuários do OpenOffice 3.0 que conheço, que alterem a configuração da suíte para utilizar como padrão o ODF 1.0/1.1 (no menu de Opções do OpenOffice existe um grupo chamado “carregar/salvar” onde esta configuração pode ser feita). Admiro o esforço dos desenvolvedores do OpenOffice em incorporar o ODF 1.2 à sua ferramenta, mas acho que isso deveria ter sido colocado como uma funcionalidade adicional, e não como o seu formato padrão (tenho usado o meu OpenOffice 3.0.1 configurado para trabalhar com o ODF 1.0/1.1 e não tenho tido nenhum problema de interoperabilidade).

Gostei também de ver a avaliação que PJ (Groklaw) fez sobre o SP2 do MSOffice. PJ não desenvolve software e por isso criou um documento texto relativamente simples e obteve resultados absurdos também.

PJ escreve ainda algo bem interessante, e concordo 100% com o que escreveu:

“…Querida Microsoft, você poderia por favor fazer alguma coisa sobre isso ? É somente código, o que significa que pode ser corrigido. Como seu código é proprietário, nós não podemos corrigi-lo. Só você pode. Como as canções antigas falam, você poderia por favor acelerar isso ? Outros como o Google Docs parecem ser capazes de trabalhar com planilhas em ODF. Por que vocês não são ? Não tenho dúvidas de que haverão melhorias, mas quando ?…”

Como não tenho nenhuma máquina com Windows e não tenho o Office 2007 para testar, acabei testando algumas coisas no SP2 através de troca de documentos com amigos que possuem o Office 2007 e aqui vão os meus 2 centavos para os testes que todos estão fazendo (e publicando os resultados a cada segundo na rede):

O Microsoft Office 2007 não suporta a criptografia (proteção por senha) nos documentos ODF !!!

Gerei um documento texto (.odt) simples em ODF utilizando o OpenOffice e o gravei com proteção de senha. Enviei o documento (e a senha) a diversos amigos e o resultado foi o mesmo: O MS Office não pode abrir o documento pois ele está protegido por senha (alguns deles tinham em suas máquinas outras ferramentas com suporte a ODF e em 100% dos casos funcionou).

Pedi a eles que gerassem no Office 2007 um documento com senha e me enviassem, e disseram que quando tentam fazer isso, o Office apresenta uma mensagem de aviso dizendo que não é possível utilizar senha para proteger o documento utilizando o formato ODF.

Gostaria de verdade de encontrar uma explicação técnica para isso, uma vez que a criptografia e a proteção por senha estão completamente especificadas dentro do ODF 1.0/1.1 (item 17.3 da especificação), e utilizam algorítimos já existentes e mais do que conhecidos por qualquer desenvolvedor.

Um comentário do Rob no seu post (que não tratou da criptografia) é capaz de comentar com maestria o problema que encontrei (e concordo plenamente com ele):

“…Me ensinaram a nunca presumir malícia onde a incompetência pode ser a explicação mais simples. Mas o grau de incompetência necessária para explicar o suporte pobre ao ODF no Service Pack 2 atormenta a mente e me leva a ter pensamentos cruéis…”

É impressionante ver como a Microsoft demonstra constantemente o desrespeito aos seus clientes (o que eu ouvi de clientes deles ansiosos pelo suporte ao ODF no Office…), desrespeito aos parceiros de mercado e uma completa incapacidade de mudar.

Antes de encerrar o post, quero alertar aos incondicionáveis apoiadores da Microsoft que insistem em postar comentários sem conteúdo técnico algum neste blog que não tenho mais mais paciência (nem tempo) para ser educado ao responder as imbecilidades que vocês costumeiramente escrevem por aqui. Se escreverem bobagem aqui, vão receber resposta a altura. Se querem mesmo defender a Microsoft, enviem um curriculum para a empresa e tentem trabalhar lá dentro para mudar as coisas (falar é muito fácil, já fazer é outro assunto).

Aos demais, todos os comentários serão bem vindos (como sempre) e se encontrarem mais textos por aí comentando os problemas referentes ao ODF no Service Pack 2, por favor coloque os links nos comentários (vamos fazer deste post uma fonte para pesquisas sobre o assunto).

A todos, segue minha recomendação: Não utilizem o Office 2007 com o Service Pack 2 para manipular documentos em ODF. Se a sua decisão for continuar utilizando o MS Office (em qualquer versão), instale o plug-in da Sun, mas recomendo mesmo que vocês procurem uma outra solução que trate nativamente documentos ODF. Não percam mais tempo com quem não lhes respeita.

Quem quiser acompanhar as notícias sobre ODF no mundo, recomendo uma visita diária ao Planet ODF (ele indexa tudo).

Share/Save/Bookmark Filed under: Interoperability, ODF, Português Article tags: ODF suporte MSOffice 2007 SP2 18 Responses to “Microsoft agora tenta fragmentar o ODF”

1. ASF May 5th, 2009 - 11:56 pm

Jomar, parabéns pelo excelente post, pela coerência, coragem e responsabilidade. 2. DalTux May 6th, 2009 - 8:50 am

Infelizmente, Redmond nos decepcionou novamente, o que já suspeitávamos desde o princípio. Agora o perigo é os usuários acharem bonito utilizar o “padrão Redmond ODF” e com a disseminação de documentos este virar o padrão ODF de fato. O que faremos a respeito? Será possível por exemplo à ISO divulgar que o Redmond Office não foi homologado como aderente ao padrão, ou o quê?

Aliás, mais uma vez, ótimo e sólido artigo. 3. DalTux May 6th, 2009 - 10:22 am

Eu realmente estava encafifado com o fato dos documentos que faço com o OOo 3 Writer não ficarem formatados corretamente no Abiword, por exemplo. Achei interessante a dica contida no artigo e fui atrás de mudar o padrão para salvar como ODF 1.0/1.1 ao invés de 1.2.

Para quem desejar proceder, aí vai a captura de tela de onde está a opção no menu Ferramentas/Opções do OpenOffice.org 3.0.1 (Ubuntu 9.04): https://yfrog.com/ejcapturadetelaopescarregp

Só que ainda não consegui fazer um extenso documento que tinha sido criado pelo OOo 3, contendo índices, tabelas, estilos, numeração de páginas, notas de rodapé, etc. ser formatado decentemente pelo Abiword 2.6.6. O problema é o Abiword, creio. Segundo a ODF Fellowship, a implementação do ODF no Abiword ainda é pobre. Os mais recomendados com quatro estrelas parecem ser apenas o OOo e o KOffice e outros derivados do primeiro. Fiquei com vontade de experimentar o KWord no Ubuntu/Gnome, só que depende de uns 48 pacotes, o que desanima. Há algum outro que você recomenda? 4. Luís Guilherme May 6th, 2009 - 8:42 pm

Em geral, eu desprezo os MS-haters e os conspiratórios que acham que a Microsoft faz tudo por vileza e Bill Gates é a reencarnação de Satã. (detalhe, eu uso quase-exclusivamente GNU/Linux, nem por isso preciso comprar discursinho anti-empresarial). Mas essa história do ODF pegou muito mal mesmo… Vamos ver qual a explicação.

O IE está perdendo share, cada dia mais, por causa dessas práticas. O IE 8 tenta ser o mais standards-compliant possível, porque a MS viu a meleca que fez. Espero que façam o mesmo com o Office. E logo. 5. Pablo Iglesias May 8th, 2009 - 12:34 am

Prezado Jomar,

Muito interessante o seu post. É de grande valia este tipo de informação, pois nos faz “abrir o olho” para este tipo de atitute, como a da Microsoft.

Reconheço a qualidade dos produtos da Microsoft. O Windows é prova disso! Mas é por isso e por outros motivos que cada vez mais incentivo as pessoas ao meu redor a utilizarem uma suite office aberta como o OpenOffice.

Enfim, concluo meu comentário com uma tremenda decepção às práticas protecionistas da Microsoft. 6. Fernando Cima [Microsoft] May 8th, 2009 - 1:31 pm

>Interessante notar que no ODF 1.2 (que é desenvolvido com a participação da Microsoft) > este problema já foi resolvido com a criação do OpenFormula.

Jomar, poderia nos apontar onde está a especificação do OpenFormula? No link que você colocou somente existem rascunhos preliminares (”preliminary drafts”), você sabe onde está a versão final? 7. Jomar Silva May 8th, 2009 - 2:12 pm

Caro Fernando,

Já que eu coloquei o aviso no post, preciso cumprir com o que prometi.

Sugiro que você contacte o Doug aí na Microsoft para que ele te explique o processo de aprovação e desenvolvimento de documentos dentro do OASIS ODF TC. Após a explicação do Doug você vai entender o que é um “preliminary draft”, e qual é o status do OpenFormula atualmente.

Para não ser tão grosseiro assim, o OpenFormula (tal como o ODF 1.2) ainda é um draft e deve ser colocado em discussão no ODF TC tão logo tenhamos finalizado as discussões do documento central do ODF 1.2 (pergunte ao Doug o estado disso tudo, e aproveite para perguntar também como a Microsoft tem colaborado).

Se quiser ver o OpenFormula implementado e funcionando, instale o OpenOffice 3 e fique a vontade para testar.

Abraço,

Jomar

PS.: A comunicação aí na Microsoft deve ser ruím mesmo, heim ? Com 3 pessoas no comitê do ODF você precisa fazer uma pergunta dessas para alguém de fora… 8. Fernando Cima [Microsoft] May 8th, 2009 - 6:25 pm

Exatamente Jomar. Pelo que nós sabemos aqui o OpenFormula ainda é um rascunho preliminar, estado aliás em que se encontra há quatro anos, e ainda muito longe de ser aprovado. Muita trabalho ainda precisa ser feito no comitê e muita coisa ainda vai mudar na especificação. Se ele fosse software seria ainda um pré-alpha, vaporware.

Eu já vi a Microsoft ser cobrada por não implementar um determinado padrão OASIS/ISO/W3C/whatever, mas ser cobrada por não implementar algo que nem existe ainda é a primeira vez.

O grande problema é que até hoje o ODF não possui um padrão definido para fórmulas, o que abre espaço para várias implementações conformes com o padrão e ainda assim incompatíveis entre si. Além dos problemas do OpenOffice 3.0 que você citou, ainda existem problemas com o Symphony 1.2 (vide blog do Doug), e provalmente outros mais. Depois alguns ainda dizem que o OpenXML é que não estava pronto…

Sobre a criptografia de documentos, o ODF 1.1 infelizmente especifica somente um único algoritmo possível para uso na encriptação, o Blowfish. O Windows não implementa este algoritmo, e o Office portanto não tem como usá-lo (nenhuma aplicação da Microsoft implementa a sua própria criptografia). É uma pena que a especificação só dê esta opção, porque o Blowfish ao que eu saiba não é adotado por nenhum governo em nenhum lugar do mundo. Mesmo aqui no Brasil ele está fora dos padrões da ICP-Brasil e do e-Ping. Ao que parece o ODF 1.2 - quando ele existir - irá adotar algoritmos padrão NIST (AES), senão fica aí a sugestão. 9. Jomar Silva May 8th, 2009 - 6:57 pm

Fernando,

Em algum lugar do texto você me viu “cobrar” o OpenFormula (eu apenas citei que ele existe no ODF 1.2) ?

Acho leviano da sua parte dizer que o OpenFormula está parado há quatro anos. Dê uma olhada nos arquivos do ODF TC e você vai ver que está falando asneira. Quando você falha no “trabalho que ainda precisa ser feito”, não se esqueça que a Microsoft é parte deste trabalho, ou será que vocês estão lá no comitê com alguma agenda oculta ? (quem sabe eu não posso escrever um outro post abordando “como as pessoas da Microsoft no Brasil falam do ODF como se não fossem parte dele”, para vermos se vocês se comportam assim também em outros países, e com isso conseguimos talvez descobrir se vocês tem uma agenda oculta ou se é falta de organização mesmo.)

Sobre o “…espaço para várias implementações conformes com o padrão e ainda assim incompatíveis entre si…”, não precisa ter um QI do Eistein para perceber que todas as outras empresas/projetos que implementaram com o ODF, decidiram usar uma única coisa, pois afinal o foco deles é RESOLVER problemas e não AUMENTA-LOS. Aliás, recebi muitos e-mails (para não dizer centenas deles) nos últimos dias me perguntando o que é que eu esperava de vocês, e acho que as pessoas estão certas mesmo… eu já devia ter perdido a ponta de esperança que um dia vocês vão se enquadrar faz tempo.

Ainda sobre as fórmulas, será que você entendeu mesmo o que está no blog do Doug, ou será que você entendeu que o post do Doug não é um, mas parte de uma discussão com diversos outros posts publicados ? Vai pesquisar direitinho, pq a coisa pode acabar ficando mais complicada ainda para vocês, OK ? (Ex. nem a nomenclatura correta para referenciar células, esta sim muito bem definida no ODF 1.0/1.1 vocês seguiram… só para citar mais um “detalhe” no meio desse salseiro).

Se você quiser falar sobre OpenXML, vamos fazer o seguinte: Entra numa máquina do tempo, volte no passado e participe do grupo lá da ABNT que ralou durante mais de um ano para analisar com profundidade o OpenXML e constatar que ele não estava (e AINDA NÃO ESTÁ) pronto. Vale a pena ainda você acompanhar os relatórios dos comitês da ISO que tratam do OpenXML, pois os indícios e as demonstrações mais contundentes de que ele não estava e não está pronto estão todos nestes documentos, relatando inclusive os REMENDOS que estão tentando fazer agora, sendo que alguns destes IGNORAM as decisões tomadas no BRM, atropelando tudo o que foi discutido no ano passado. Fique tranquilo que vou compilar uma boa coletânea sobre estes problemas e REMENDOS e devo publicar em breve, afinal você deve ler mais meu blog do que os documentos da ISO, não é mesmo ?

Gostei do seu argumento sobre criptografia, pois ele mostra que a mentalidade de LOCK-IN de vocês é mesmo absurda… prefiro nem comenta-la, mas sugiro que você solicite que na normalização internacional exista uma nova regra: Só pode usar o que está implementado do Windows… tenha dó…

PS.: Minha cota de boa educação com você acabou de acabar, portanto lhe recomendo que não abuse. 10. Fernando Cima [Microsoft] May 8th, 2009 - 7:32 pm

Relaxa, Jomar. Todo mundo sabe que você fica “esquentadinho” toda vez que alguém tenta conversar em termos técnicos contigo. Se lembra quando você deu um “piti” e abandonou o comitê da ABNT, só para voltar logo depois que esfriou a cabeça? Isto somente mostra a sua insegurança.

>Acho leviano da sua parte dizer que o OpenFormula está parado há quatro anos.

Eu não disse que estava parado. Eu disse que continua no mesmo estado - preliminary draft - a quatro anos, o que é um fato. Leviano, por sinal, é colocar palavras na boca de outra pessoa.

>Sobre o “…espaço para várias implementações conformes com o padrão e ainda assim > incompatíveis entre si…”, não precisa ter um QI do Eistein para perceber que todas as > outras empresas/projetos que implementaram com o ODF, decidiram usar uma > única coisa, > pois afinal o foco deles é RESOLVER problemas e não AUMENTA-LOS

A forma de resolver o problema seria o ODF ter um padrão para fórmulas, o que até hoje infelizmente não aconteceu. O resultado é que Symphony faz de um jeito, OpenOffice 2.x de outro, OpenOffice 3.x de outro. E o pior é que todos estão em conformidade com o padrão. Quer melhor atestado de imaturidade de um standard do que este?

Sobre criptografia, a encriptação não é requisito para conformidade com o ODF, como você deve saber muito bem. No seu estado atual a encriptação do ODF é inutilizável e proibido por praticamente qualquer padrão governamental, incluindo o e-Ping. Fica aí a dica para que isto mude no ODF 1.2 quando ele vier a existir. 11. Jomar Silva May 8th, 2009 - 8:00 pm

Fernando (e demais colegas da Microsoft que estão ajudando o Fernando nessa),

Sabe o que me fez perder a cabeça na ABNT aquela vez ? Eu te refresco a memória… foi a Microsoft tentando a todo custo IMPEDIR O COMITÊ BRASILEIRO DE TOMAR UMA DECISÃO com o argumento de que seu representante naquela reunião (o mesmo que tinha ido a todas as outras, e que aliás anda meio sumido), não tinha “condições de decidir” pois “desconhecia aprofundadamente o assunto”… vocês são ou não são uma piada ? Aliás, um dos motivos de eu ter voltado atrás foi uma conversa que tive com o mesmo executivo da vossa empresa, que se desculpou pela postura infantil e imatura…

O documento do OpenFormula está sendo trabalhado e basta uma visita sua ao repositório de documentos que você vai descobrir que a última versão dele foi ali depositada na última sexta feira. Não me venha falar de leviandade, pois lembro muito bem de uma certa discussão que tivemos no seu blog há alguns anos, onde você insistia em me apresentar como sendo um “simples lobista”… aquilo foi leviano, mas acho que meu trabalho nos últimos anos já deve ter feito você ver como estava errado.

Dá uma olhada no blog do Rob Weir pois lá ele coloca um texto bem legal sobre conformidade e interoperabilidade… acho bom você ter bastante certeza de que conhece estes dois temas, pois pelo que escreve, tá difícil.

Ao invés de ficar me dando dica de segurança para o ODF 1.2, e se você entende tanto do assunto, por que não vai lá para o OASIS trabalhar na segurança do ODF ? Nós não mordemos lá não e a colaboração sempre é bem vinda, mesmo que seja de pessoas como você.

Agora, se quiser continuar só falando (e escrevendo em blogs que têm alguma visibilidade) eu entendo, afinal de contas é essa a postura mais cômoda, não é ? 12. Machado May 8th, 2009 - 8:49 pm

Fernando Cima [Microsoft] escreveu: > Quer melhor atestado de imaturidade de um standard do que este?

Olha eu conheço um atestado maior que esse. Há algum tempo trabalhei num GT da ABNT que tratou de um pseudo-padrão que não é coerente nem com ele mesmo. Tem coisas iguais que são feitas de um jeito na planilha, de outro na apresentação e de outro no texto. Sem contar inúmeros defeitos que persistem ainda hoje no texto atual, apesar dos esforços mundiais em apontar esses defeitos, a criadora da especificação não teve capacidade de implementar.

Essa especificação é a MSOOXML que, infelizmente também é conhecida como ISO/IEC 29500. Aliás, esse docuimento ficou quase dois anos com o status de DIS e FDIS(onde o D = DRAFT) 13. Ricardo May 8th, 2009 - 11:05 pm

Brother….concordo e muito com você e seus posts que venho acompanhando ao longo do tempo, sobre o opemxml e odf. Mas se a coisa não se espalhar na mídia, nada vai mudar e o plano (não sejamos ingênuos de achar que não tenha um plano), vai dar certo. Afinal, a suite M$$$$$ corresponde a maior fatia de faturamento deles. A qualquer manifestação eu vou, basta chamar (contanto que seja em sampa). Abs. 14. Não use o Microsoft Office | Cyaneus May 9th, 2009 - 8:59 am

[…] Perda de fórmulas em planilha eletrônica e diversas outras coisas… […] 15. Fernando Cima [Microsoft] May 9th, 2009 - 9:01 am

> Aliás, um dos motivos de eu ter voltado atrás foi uma conversa que tive com o mesmo > executivo da vossa empresa, que se desculpou pela postura infantil e imatura…

Ele pediu desculpas pela tua postura infantil e imatura?? O Raimundo realmente é um dos sujeitos mais educados que eu conheço.

>O documento do OpenFormula está sendo trabalhado e basta uma visita sua ao > repositório de documentos que você vai descobrir que a última versão dele foi ali > depositada na última sexta feira.

Este é exatamente o meu ponto. O OpenFormula é um trabalho em andamento, ainda em estágio inicial e longe de ficar pronto. Por isso é inusitado você querer cobrar que a Microsoft implemente algo que nem existe ainda.

Vamos só por suposição imaginar que a Microsoft tivesse implementado o rascunho preliminar do OpenFormula no Service Pack 2, que foi lançado no dia 28 de Abril. Logo depois como você disse a OASIS divulgou um novo rascunho preliminar, com diversas alterações na proposta de formato. E aí, como ficam os arquivos que os usuários gravaram no formato rascunho anterior? Como é que eles vão ser convertidos para o próximo formato rascunho? Quantos formatos rascunhos a Microsoft vai ter que implementar até a aprovação final?

Ninguém é irresponsável em oferecer para um usuário a opção de gravar os seus arquivos em um formato que não está pronto e pode mudar a qualquer momento. Por isto a Microsoft implementou no Office 2007 SP2 o formato ODF 1.1, que é a versão terminada e aprovada pela OASIS. Se existem problemas com a implementação (e certamente existem, ninguém acerta de primeira) o feedback e as críticas são válidas e bem-vindas. Agora você criticar a Microsoft por não ter implementado algo que nem está na especificação e chamar isso de tentativa de “enganar os usuários” mostra bem o nível do debate que nós tivemos que aguentar na ABNT. Deve ser por isso que o Raimundo pediu desculpas…

>Ao invés de ficar me dando dica de segurança para o ODF 1.2, e se você entende tanto > do assunto, por que não vai lá para o OASIS trabalhar na segurança do ODF ?

Quando o ODF 1.2 chegar na ISO e na ABNT eu terei o maior prazer em apresentar estas sugestões. Na ISO o Brasil finalmente poderá ter voz e voto no formato ODF, algo que nunca teve oportunidade até hoje, e eu certamente participarei.

Além da encriptação, poderemos colaborar com o suporte a XADES e adequá-lo ao perfis utilizados na ICP-Brasil. Hoje a falta de suporte a assinaturas digitais no ODF 1.1 é um passo atrás em todo o processo de certificação no Brasil. 16. Jomar Silva May 9th, 2009 - 9:29 am

Fernando,

Você realmente tem algum problema sério. Em lugar algum do texto eu cobro que a Microsoft implemente o OpenFormula (quer que eu desenhe) ?

“Ninguém é irresponsável em oferecer para um usuário a opção de gravar os seus arquivos em um formato que não está pronto e pode mudar a qualquer momento…” é realmente mediocre ouvir isso de quem desenvolveu o OpenXML, não acha ?

Quando você teve chance de participar da ABNT para trabalhar com o OpenXML, se ausentou. Vou entender seu texto sobre o ODF na ISO como uma ameaça, e agora me parece que a agenda oculta está começando a se revelar. Fique tranquilo que vou blogar sobre isso e te dar os devidos créditos.

Ja dei a voce espaço suficiente neste blog e está claro para qualquer um o seu intuito aqui. Para cumprir o que prometi, qualquer comentário adicional seu aqui será removido.

Cresça, trabalhe e apareça :D

* Acompanhe as discussões no blog de Jomar Silva - ODF Alliance: https://homembit.com


**Q**uanta barbaridade eim!? Faço questão de publicar aqui o texto para que possamos atingir o máximo de leitores!

**A**braços
