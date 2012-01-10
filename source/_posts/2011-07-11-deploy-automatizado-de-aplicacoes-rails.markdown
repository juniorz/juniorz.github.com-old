---
layout: post
title: "Deploy automatizado de aplicações Rails com Capistrano e Github"
date: 2011-07-11
categories:
 - deploy
 - rails
 - github
 - tutorial
 - capistrano
---

Nesse tutorial irei abordar a realização de deploy automatizado de uma aplicação Rails (3.0.X) utilizando o Github como repositório. A idéia é utilizar o capistrano para automatizar (e organizar) as tarefas de deployment. Para isso, irei criar um usuário no servidor que utilizará minha chave privada (cadastrada no Github) para fazer o deployment da aplicação.

## Configurando o servidor

No servidor, crie um usuário que será utilizado para realizar o deploy. Eu utilizei uma senha gerada aleatóriamente (um MD5 de um arquivo, por exemplo). Em seguida, adicionei minha chave privada (a que eu uso para acessar meus repositórios no Github) ao arquivo `authorized_keys` do usuário de `deploy`.

**Na máquina local**  

```console
scp ~/.ssh/id_rsa.pub reinaldo@servidor:/home/reinaldo/my_key.pub
ssh reinaldo@servidor
```

**No servidor**

```console
sudo adduser deploy
MY_PUBLIC_KEY=/home/reinaldo/my_key.pub
sudo mkdir -p /home/deploy/.ssh
sudo cat $MY_PUBLIC_KEY >> /home/deploy/.ssh/authorized_keys && rm $MY_PUBLIC_KEY
sudo chmod 600 /home/deploy/.ssh/authorized_keys
sudo chmod 700 /home/deploy/.ssh
sudo chown deploy:deploy -R /home/deploy
```

## Configurando a aplicação

Adicione a gem capistrano no seu *Gemfile*

```ruby
gem 'capistrano'
```

Em seguida, no diretório da aplicação

{% codeblock "Capistrando" a aplicação lang:console %}
bundle install
capify .
echo ".bundle\n" >> .gitignore
{% endcodeblock %}


Isso vai criar o arquivo *config/deploy.rb* onde serão armazenadas as configurações de deploy, bem como as tasks.

Meu arquivo ficou assim:
<script src="https://gist.github.com/1076956.js"> </script>

Pra finalizar, você precisa criar os diretórios no servidor. Para isso simplesmente execute (no diretorio da aplicação) `cap deploy:setup`.

## Realizando o Deploy

Depois disso, você poderá realizar seu deploy automático executando o comando: `cap deploy`.
