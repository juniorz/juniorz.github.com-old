---
layout: post
title: "Migrando o blog para Octopress"
date: 2012-01-05 10:34
comments: true
categories: 
---

O Ano novo sempre traz a intenção (promessa) de fazer melhor. E para manter a tradição: ano novo, blog novo.

## Histórico

Já usei o [WordPress](http://wordpress.org/), tanto hospedado pessoalmente quanto compartilhado no wordpress.com. O problema era: atualizar a versão do Wordpress, atualizar os plugins, acessar o painel pra postar e usar o editor WYSIWYG pra formatar. Muito trabalho.

Em 2011, usei o [Blogger](http://www.blogger.com). Poucas opções pra customizar, e o mesmo workflow: acessar painel, usar editor WYSIWYG, alternar entre o código-fonte e o visual pra formatar. Muito trabalho.

## Os problemas

1. **HTML sux**  
   Usar uma linguagem de marcação tão verbosa (`<strong>` e `</strong>` pra negrito, p.e.) e com tanto ruido atrapalha a escrita. As tags começam a quebrar a leitura (e as idéias) e no final você tem que ligar o seu interpretador de HTML cerebral pra expressar as idéias.  
   **Muito esforço.** Por isso é **muito ruim** escrever tudo em HTML

2. **O workflow envolve muitos passos online/offline**  
   Por causa do problema (1) eu era forçado a usar o seginte workflow:
   * Rascunhar os posts "offline", escrevendo num arquivo .txt. Essa parte demora.
   * Acessar o painel do blog "online", criar o post
   * Formatar o post "online", olhar como ficou no editor WYSIWYG, repetir.
   * Colocar color highlight nos códigos, disponibilizar os códigos pra download, etc.

   A questão não é a parte dos "muitos passos" e sim "online/offline".

O problema real é que as plataformas de blogging se baseiam no HTML (afinal de contas, se você pode publicar em HTML pode publicar qualquer coisa). **Eu não quero fazer qualquer coisa que eu quiser. Quero fazer apenas o que eu preciso.**

## Solução

A solução seria uma plataforma de blogging que me fornecesse (por padrão) algo mais legível (mesmo que mais restrito) que HTML e que oferecesse "suporte" a HTML. O que eu queria era uma plataforma que suportasse [Markdown](http://daringfireball.net/projects/markdown/syntax).

No final de 2011, ainda tentei usar o [Posterous](https://posterous.com/) pela promessa de [suporte a Markdown](http://blog.posterous.com/announcing-markdown-support) mas ainda assim era um grande `#fail`: precisava de um comentário HTML no topo do post pra indicar que eu tava escrevendo em Markdown e depois pra editar, era no editor visual deles, arghhhh... **Gambi**.

## Octopress

No final do ano, por acaso, conheci o [Octopress](http://octopress.org/) - um framework de blog baseado no [Jekyll](http://github.com/mojombo/jekyll) (um gerador estático de website).

### Instalando

A instalação foi muito simples, é só seguir a [documentação](http://octopress.org/docs/). Em seguida, configurei o deploy para ser feito no [Github Pages](http://octopress.org/docs/deploying/github/). Isso é MUITO maneiro.

### Importando do Blogger

Pra importar os posts do Blogger eu [modifiquei um script](https://gist.github.com/1564581) que eu achei.  
**Cuidado:** ele vai limpar sua pasta `_posts`. O melhor é criar um diretorio `import`, jogar o script e o .xml do seu blogger nele, executar a importação e depois copiar de `import/_posts` para `_posts` do seu Octopress.

### Workflow

Agora meu workflow é simples:

1. Criar o rascunho em .txt (na verdade em .markdown)
2. Escrever e formatar ao mesmo tempo
3. Dar uma conferida na formatação:
   * Quando tô no Mac, uso o app [Marked](http://itunes.apple.com/us/app/marked/id448925439?mt=12)
   * Independente de onde estou, dá pra usar `rake generate && rake preview` e visualizar o blog em `localhost:4000`
4. `rake generate`
5. `rake deploy`
