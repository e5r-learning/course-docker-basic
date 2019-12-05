Curso sobre o básico do Docker
==============================

Neste curso você aprenderá o que é o Docker e como usar ele no seu dia a dia para aumentar
sua produtividade como desenvolvedor.

Não te tornará um expert em Docker, mas você saberá o básico para se aprofundar depois.

# NOTAS

> https://docs.docker.com/docker-for-windows

1) Instalação
```
$ docker --version
```

2) Hello World + Modo interativo
```
$ docker run hello-world
```
- Aqui um comando é executado e pronto!

```
$ docker run --interactive --tty ubuntu bash

root@8aea0acb7423:/# hostname
8aea0acb7423
root@8aea0acb7423:/# exit

# ou
$ docker run -it ubuntu bash
```
- Aqui é como usar uma máquina virtual
- Mas, basicamente, não é pra isso que usamos docker

```
$ docker run hello-world --name hello
$ docker run --interactive --tty --name myubuntu ubuntu bash
```
- Antes um `docker container ls -all` revela muitos containers.
  - Mais precisamente, um por execução. Lixo
- `docker container prune` Remove containers não usados
- O --name é mais fácil de você encontrar seu container
  > E permite usar um só container por imagem. Polui menos
- `docker start {CONTAINER ID}` para reexecutar o containar
- `docker start -i {CONTAINER ID}` para reexecutar o containar em modo interativo

3) Diferença de imagens para containers
```
$ docker image ls
$ docker container ls --all
```
- Imagens são templates
- Containers são objetos dos templates em execução (ou parados)

4) Usando docker para executar projetos locais

Rodar scripts em pasta local no docker
```
$ docker run -it --rm --name my-project -v "$PWD":/usr/src/myapp -w /usr/src/myapp perl:latest perl my-local-script.pl
```
- O `--rm` é pra remover automaticamente o container quando existir com o `--name` informado
  > Serve para evitar poluções de containers não utilizados

Rodando um container pra servir algo localmente
```
$ docker run --publish 80:80 --name webserver nginx
$ docker run --detach --publish 80:80 --name webserver nginx
```
- Diferença de --detach
- O --publish
- Apagar imagens ?
- Apagar containers?
```
$ docker container rm webserver laughing_kowalevski relaxed_sammet
```

- Parar containers
```
$ docker container stop webserver
```

5) Interface de configuração
- Start on login
- Automatic check for update
- Send usage statistics
- Exposeaemon without TLS
- Shared drivers
- Advanced
- Network
- Proxies
- Daemon
- Kubernets ?
- Reset
```
$ docker run --rm -v c:/Users:/data alpine ls /data
```
- SHARED DRIVES ON DEMAND

6) Interconexão entre os containers
- ???
- Basicamente subir uma imagem que dependa de serviços e,
  > Ver que os containers são criados automaticamente
  > Ex: Um serviço que usa banco de dados

7) Docker para dev, fluxos básicos
- Listar alguns fluxos do dia a dia
  > Rodar um comando em um container
  > Entrar em um container para usar como máquina
  > Subir um container como serviço
  > Subir vários containers como serviços interconectados
  > Compartilhar dados em uma pasta de projeto
    - Rodar em um container e ver os arquivos locais
    - Modificar arquivos localmente e ver eles em execução no container
    - Rodar máquina PostgreSQL em vários containers temporários e ver
	  > 1 - Sem compartilhar, os dados se perdem
	  > 2 - Com compartilhamento, os dados permanecem
	  > 3 - Veja que os dados ficam em sua máquina
	- Volumes ???

8) Dev - A anatomia do Dockerfile
- Arquivo Dockerfile
```Dockerfile
FROM perl:5.20
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
CMD [ "perl", "./your-daemon-or-script.pl" ]
```
- Criar imagens
- Subir imagens
- Build pipeline
```Dockerfile
FROM perl:5.26

RUN cpanm Carton \
    && mkdir -p /usr/src/app
WORKDIR /usr/src/app

ONBUILD COPY cpanfile* /usr/src/myapp
ONBUILD RUN carton install

ONBUILD COPY . /usr/src/app
```

9) Visual Studio Code + Docker
- Executar tudo em um container
