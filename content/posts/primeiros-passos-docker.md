---
title: "Primeiros Passos com Docker"
date: 2020-08-13T15:57:39-03:00
draft: false
toc: false
images:
tags: 
  - posts
  - docker
---


Você já deve ter passado pelos vídeos de [Como Instalar o Docker](https://www.youtube.com/watch?v=Hg4i7f5SdWI&list=PLASqcNvBI-ScSDL7FZtGyxt1WnXBItQoP&index=4), e talvez tenha se perguntado "O que exatamente é Docker?" 

Primeiramente podemos definir Docker como uma ferramenta de containerização, desenvolvida pela Docker Inc, mas que é opensource. Basicamente o docker permite que a gente divida as aplicações em serviços e rode esses serviços separadamente, aumentando segurança, escalabilidade e várias coisas legais.


## Do início:

Antes de explicar o que é um container e entrar sobre a forma que utilizamos no curso de Terraform, vamos entender a organização de projetos em Docker.

## Imagens

Os containers administrados pelo Docker são gerados através de imagens, que são mantidas pela comunidade ou pelas empresas proprietárias das ferramentas.

Uma imagem é formada por camadas, que juntas têm os recursos, códigos, bibliotecas e configurações necessários para rodar o serviço. Dessa forma os containers que usarem essa imagem específica serão todos iguais, evitando conflitos de configuração de ambientes durante a implementação de aplicações.

Essa imagem é disponibilizada no [Docker Hub](https://hub.docker.com/), e isso permite que outras aplicações usem essas imagens de base, economizando o tempo que se gastaria para deixar um ambiente 'igual' ao outro.

## O que é container

Certo, entendemos que o Docker serve pra administrar containers e que as imagens são a base desses containers, mas o que é **realmente** um container?

Segundo o [site do Docker](https://www.docker.com/resources/what-container), "_Um container é uma unidade padrão de software que empacota o código e todas as suas dependências para que a aplicação seja executada de maneira rápida e confiável de um ambiente de computação para outro_"

Em resumo, o container é uma pequena máquina pré-configurada através da imagem de origem. 

Usando uma analogia simples, podemos imaginar que uma imagem é como uma forma de bolo em uma padaria:
- Há apenas uma forma, mas é possível fazer diversos bolos nela. Todos eles terão o mesmo formato, mas serão independentes entre si - como os containers.

Dentro do curso nós utilizamos um container baseado na imagem terraform da Hashicorp - empresa mantenedora. Isso garantiu que todas as pessoas teriam as mesmas ferramentas, recursos e configurações para rodar o terraform.

## Rodando o Hello World

Agora que você já tem o Docker instalado e já sabe os conceitos iniciais, vamos fazer um teste pra saber se tudo está rodando legal: Vamos o rodar o Hello World

Vamos criar um container a partir de uma imagem chamada hello-world, que quando estiver rodando imprimirá no terminal o famoso "Hello World"

Antes de começar vamos a como funciona o nosso primeiro comando:
#### Comandos Docker - Rodando um container <_docker run_>
```$ docker run [imagem]```

Isso criará um container a partir da imagem passada. Caso você não tenha ela no seu computador, o docker a buscará no _Docker Hub_ e então criará o container.


#### Comandos Docker - listando imagens locais <_docker images_>

Para verificar as imagens que você tem na sua máquina basta rodar o comando

```$ docker images```

![docker images](/primerios-passos-com-docker/imagens/dockerimages.jpeg)

Neste caso eu tenho algumas imagens locais e a hello-world não está entre elas. Isso significa que quando eu rodar 
```
$ docker run hello-world
```

O Docker irá até o docker hub e disponibilizará essa imagem localmente:

![hello-from-docker](/primerios-passos-com-docker/imagens/hello-world.jpeg)

Na imagem acima podemos ver o que aconteceu:

Eu pedi pra rodar um container de uma imagem que não tinha localmente, o docker fez o pull do docker hub e rodou o container, imprimindo no terminal a mensagem abaixo:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.(amd64)
 3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
## Entrando num container

Você se lembrará de que no curso nós rodamos um container terraform e fizemos modificações nele, alteramos arquivos, usamos a linha de comando, mudamos coisas de lugar. Como isso é possível se o container é formado a partir de uma imagem?

Pois bem, vamos ver agora uma forma de entrar num container e verificar seu conteúdo e utilizar ferramentas.
Para essa demonstração vamos utilizar [uma imagem da linguagem GO](https://hub.docker.com/_/golang).

Primeiro vamos fazer o pull da imagem:
```
$ docker pull golang
```

![Pull golang](/primerios-passos-com-docker/imagens/golang-image.jpeg)

Agora que temos a imagem local, vamos rodar o seguinte comando:

```
$ docker run -it golang /bin/bash
```

Entendendo o que pedimos:
* -i - é uma flag de docker que cria algo interativo, ou 'mantem o stdin aberto'
* -t - cria um TTY. Uma interface com o terminal de dentro do container.
* /bin/bash - É o modo do terminal que estamos abrindo - um terminal bash.

Resumindo, fizemos o seguinte: '_Docker, por favor, rode um container baseado na imagem golang, com um terminal que seja interativo, do modo bash_'. Como pedimos com educação, deu certo, e agora temos uma mudança no nosso terminal:

![Dentro do container](/primerios-passos-com-docker/imagens/run-golang.jpeg)

Vemos agora que estamos no usuário "root", no container de ID "bea1898936a0", e na pasta /go. 
Se abrirmos outro terminal e verificarmos os containers que estão rodando, poderemos ver nosso container GO lá:

#### Comandos docker - Listando containers em execução - <_docker ps_>

```$ docker ps```

![docker ps](/primerios-passos-com-docker/imagens/container-go-rodando.jpeg)

Dentro do container podemos utilizar comandos bash para transitarmos entre pastas, criarmos arquivos e editá-los da forma que quisermos. Para conhecer melhor esses comandos, acesse o artigo [Comandos Linux](link.post.comandos) (em breve estará disponível).
Por hora vamos fazer algo simples pra entendermos como funcionam as coisas num container:

1. Voltar um nível e criar uma pasta na raiz [/] chamada teste -> /teste
```
> $ cd ..
> $ mkdir teste
> $ cd teste/
```


![passo1](/primerios-passos-com-docker/imagens/dentro-container-1.jpeg)

2. Criar um arquivo na pasta chamado 'hello-go.go', com um hello-world em GO

```
> $ touch hello-go.go
$ echo "package main
import \"fmt\"
func main() {
    fmt.Println(\"hello world\")
}" > hello-go.go
```

![passo2](/primerios-passos-com-docker/imagens/dentro-container-2.jpeg)

3. Compilar esse arquivo e executar dentro do container.

```
> $ go build hello-go.go
> $ ./hello-go
```
![passo3](/primerios-passos-com-docker/imagens/dentro-container-3.jpeg)


## Criando um container com Terraform

Já vimos que aparentemente tudo está funcionando corretamente, jóia!
Agora vamos voltar um pouco mais para o cenário do curso e vamos rodar um container com o terraform.

Vamos até o [Docker Hub](hub.docker.com) e pesquisaremos por _terraform_

![terraform-dockerhub](/primerios-passos-com-docker/imagens/terraform-dockerhub.jpeg)

Como no curso, escolheremos a imagem _hashicorp/terraform_, mantida pela comunidade.

![Página da imagem hashicorp/terraform](/primerios-passos-com-docker/imagens/hashicorp-terraform.jpeg)

Podemos ver as informações sobre a imagem nessa página, as mais importantes são:
* Nome
* Matenedor
* Overview - com exemplos como utilizar a imagem
* Tags - Com a lista de tags que podem ser utilizadas para acessar diferentes versões da imagem
* Dockerfile - Aqui está como a imagem é construida, todas as camadas, instalação de dependências e tudo o que for compor o container
* Docker Pull Command - este é o comando para importar a imagem do docker hub para seu pc sem necessariamente rodar um container.

Dessa vez vamos utilizar o comando _docker pull_ e passaremos a tag 'light', a mesma que utilizamos no curso para facilitar o entendimento.

```
$ docker pull hashicorp/terraform:light
```

![Saída do comando docker pull](/primerios-passos-com-docker/imagens/pull-terraform.jpeg)

Agora que temos a imagem salva localmente, vamos rodar um container do terraform como no curso. 
Eu salvei os arquivos da primeira aula [neste repositório](https://github.com/chamaasminas/codigo-descomplicando-o-terraform/tree/master/Dia%2001/01%20-%20Entendendo%20o%20HCL), você pode baixar de lá e fazer as substituições apropriadas para esse teste. 

Para ficar mais próximo do que foi visto do curso, vamos ter os arquivos numa pasta chamada terraform, da seguinte forma:
``` 
terraform/
├── ec2.tf
├── main.tf
├── output.tf
├── security-groups.tf
└── variable.tf 
```
Para executar da forma ideal lembre-se de:
- Alterar o nome do bucket no arquivo main.tf para o seu bucket
- Ter suas credenciais AWS_ACCESS_KEY_ID e AWS_SECRET_ACCESS_KEY

Caso tenha dúvidas sobre essas questões vá até o post [Noções de AWS](link.post.noçõesaws) (em breve)

Finalmente, vamos rodar o container da forma que foi vista no curso. Dentro da pasta que criamos agora execute:

```
$ docker run -it -v $PWD:/app -w /app --entrypoint "" hashicorp/terraform:light sh
```

Vamos entender o comando:

* -i -> Da mesma forma que no exemplo de GO, é uma flag de docker que cria algo interativo, ou 'mantem o stdin aberto'
* -t -> Também criará um terminal, através do qual poderemos interagir com o container cria um TTY. Uma interface com o terminal de dentro do container.
* -v -> Cria um volume, colocando os arquivos do diretório atual no container (no nosso caso, os arquivos que criamos na pasta)
* $PWD:/app -> Mostra ao docker que a pasta atual <**$PWD**> (no linux o comando pwd retorna a pasta atual no terminal) será mapeada para a pasta /app dentro do container
* -w /app -> Seta o workdir, o diretório de trabalho, em que o docker irá fazer as modificações futuras.
* --entrypoint "" -> Substitui o entrypoint padrão da imagem. Entrypoint é um comando de inicialização do container, que vem definido na criação da imagem (no Dockerfile)
* hashicorp/terraform:light -> Referencia a imagem terraform da hashicorp marcada como light
* sh -> Mostra que o terminal que abriremos será do tipo shell

Ok, bastante informação, vamos traduzir de forma mais amigável. O que pedimos dessa vez foi algo do tipo:

"Ah, olá docker, eu de novo. Dessa vez por favor, rode um container a partir da imagem hashicorp/terraform, aquela que é marcada como light, crie também um volume da minha pasta local atual para a pasta /app dentro do container, faça com que as alterações que eu fizer sejam feitas nessa pasta e deixe o comando de inicialização do container em branco. Por fim você poderia abrir um terminal interativo, que me permita fazer entradas de texto, do tipo sh? Obrigada"


### Espera aí, aqui deu erro!

Algumas pessoas que utilizam Windows tiveram um erro ao tentar executar o comando acima, algo parecido com isso (Obrigada a Naty que nos cedeu a imagem):

![erro windows](/primerios-passos-com-docker/imagens/erro-windows.png)

Nele o docker interpreta o sh do final como a tentativa de importar outra imagem e vira tudo uma bagunça. A Giuliana - que falou sobre a Instalação do Docker -, conseguiu uma solução. Caso você tenha esbarrado nesse erro tente executar da seguinte forma:

```
> docker container run -it -v ${PWD}:/app -w /app --entrypoint='' hashicorp/terraform:light sh
```


Agora nós temos um container rodando e se dermos um _ls_ poderemos ver nossos arquivos dentro do container como pedimos:

![container terraform](/primerios-passos-com-docker/imagens/container-terraform.jpeg)

O mesmo aconteceria se abríssemos o terminal do VSCode:

![terminalvsc](/primerios-passos-com-docker/imagens/terminal-vsc.jpeg)

Agora que estamos no mesmo cenário que o início do curso, exporte suas variáveis AWS_ACCESS_KEY_ID e AWS_SECRET_ACCESS_KEY

```
# export AWS_ACCESS_KEY_ID= 
```
```
# export AWS_SECRET_ACCESS_KEY= 
```
Finalmente, poderemos dar início ao curso com 

```
# terraform init
```

![terraformiit](/primerios-passos-com-docker/imagens/terraform-init.jpeg)

Agora seus problemas com Docker estão resolvidos, aproveite o curso e, se precisar de ajuda, estamos à disposição no grupo do telegram do curso.

## Um Pouco Além

Já estamos no ponto em que devemos para fazer o curso, mas se você gostou do Docker e quer aprender mais, temos umas coisinhas pra falar.

A primeira coisa é que alguns dos comandos que utilizamos têm alternativas, especificamente os comandos _docker run_, para rodar um container, e o _docker ps_, para listar os containers ativos.

1. ``` docker container run ```

É o modo recomendado na [documentação do Docker](https://docs.docker.com/engine/reference/commandline/container_run/) para rodarmos containers. a sintaxe dele é a seguinte:

```
docker container run [opções] imagem [comando] [argumentos]
```
No nosso caso, podemos rodar um container de hello-world como:

```
docker container run hello-world
```
![docker container run](/primerios-passos-com-docker/imagens/docker-container-run.jpeg)

Novamente segundo a [documentação](https://docs.docker.com/engine/reference/commandline/container/), o comando **docker container** é o ideal para gerenciar containers docker. Nesse link acima você pode verificar os vários comandos possíveis e suas sintaxes. Sinta-se a vontade para testar!

2. ``` docker container ls ```

Da mesma forma que o item anterior, é possível utilizar o [_docker container_](https://docs.docker.com/engine/reference/commandline/container_ls/) para listar os containers em execução, em substituição ao _docker ps_ que utilizamos no curso e no começo do material.

No link que inserimos você pode ver todas as flags possíveis a esse comando, cuja sintaxe é:

``` docker container ls [opções] ```

![docker container ls](/primerios-passos-com-docker/imagens/docker-container-ls.jpeg)

Aqui listamos primeiramente os containers ativos e depois todos os containers na máquina.

Por último, caso você queira saber e praticar mais, sugiro que utilize o [Play With Docker](https://www.docker.com/play-with-docker). É uma seção com tutoriais e laboratórios sobre docker, onde é possível testar e implementar algumas coisas rapidamente. Vale muito a pena dar uma olhada!


Boa jornada!

#ChamaAsMina

