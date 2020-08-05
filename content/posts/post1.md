---
title: "Como postar no nosso blog"
date: 2020-08-05T20:21:42+01:00
draft: false
toc: false
images:
tags: 
  - posts
  - hugo
---

Esse primeiro post é para explicar para as gurias que estão juntas no making off do #chamaasminas como criar posts no nosso blog **ˆ-ˆ**

**Antes** de tudo instalar o [HUGO](https://gohugo.io/getting-started/quick-start/)

1- Clonar o nosso repo do [Blog](https://github.com/chamaasminas/chamaasminas.hugo.io) no diretorio que vc desejar


2- Entrar no diretorio **do projeto**

3- Você pode rodar no terminal ```hudo server -D ``` e vai conseguir visualizar todas suas modificações no localhost:/1313

4- Para criar outro post no terminal digite ```hugo new post/<nome-do-post-curto>.md ```
No diretório será criado seu post
![post](/postex.png)


5- Quando clicar no seu post terá uma estrutura +- assim
![structure](/structure-post.png)

Em **"Title"** você pode renomear pro título que quiser isso não influenciará em nada

O draft está como **false** pois assim sempre que eu pedir pro o post ser publicado ele não ficará em draft

Abaixo do **"---"** você poderá escrever seu post

6- Vamos Publicar, para isso é só rodar no seu terminal ```hugo -d chamaasminas.github.io```