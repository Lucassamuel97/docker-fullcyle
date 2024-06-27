# Docker Comandos

## Gerenciamento de Contêineres

- `docker run [opções] imagem`: Cria e executa um contêiner baseado em uma imagem.
  - Exemplo: `docker run -d -p 80:80 nginx`
  - Exemplo: `docker run --name nginx -d -p 80:80 nginx`
- `docker ps`: Lista os contêineres em execução.
- `docker ps -a`: Lista todos os contêineres, incluindo os que não estão em execução.
- `docker stop [ID ou nome do contêiner]`: Para um contêiner em execução.
- `docker start [ID ou nome do contêiner]`: Inicia um contêiner parado.
- `docker restart [ID ou nome do contêiner]`: Reinicia um contêiner.
- `docker rm [ID ou nome do contêiner]`: Remove um contêiner.
  - `docker rm [ID ou nome do contêiner] -f`: Força a remoção de um contêiner.
- `docker logs [ID ou nome do contêiner]`: Mostra os logs de um contêiner.
- `docker exec -it [ID ou nome do contêiner] [comando]`: Executa um comando em um contêiner em execução.
  - Exemplo: `docker exec -it meu_contêiner /bin/bash`

## Gerenciamento de Imagens

- `docker build -t [nome:tag] [caminho ou URL do Dockerfile]`: Cria uma imagem a partir de um Dockerfile.
  - Exemplo: `docker build -t minha_imagem:latest .`
- `docker pull [imagem]`: Baixa uma imagem do Docker Hub ou de outro registro.
  - Exemplo: `docker pull ubuntu`
- `docker push [nome:tag]`: Envia uma imagem para um registro.
  - Exemplo: `docker push meu_repositorio/minha_imagem:latest`
- `docker images`: Lista todas as imagens locais.
- `docker rmi [imagem]`: Remove uma ou mais imagens.
  - Exemplo: `docker rmi minha_imagem:latest`

## Volumes

- `docker volume create [nome]`: Cria um volume.
  - Exemplo: `docker volume create meu_volume`
- `docker volume ls`: Lista todos os volumes.
- `docker volume rm [nome]`: Remove um volume.
  - Exemplo: `docker volume rm meu_volume`
- `docker volume prune`: Remove todos os volumes (limpa todos os volumes do ambiente Docker).

## Redes

- `docker network create [nome]`: Cria uma rede.
  - Exemplo: `docker network create minha_rede`
- `docker network ls`: Lista todas as redes.
- `docker network rm [nome]`: Remove uma rede.
  - Exemplo: `docker network rm minha_rede`
- `docker network connect [rede] [contêiner]`: Conecta um contêiner a uma rede.
- `docker network disconnect [rede] [contêiner]`: Desconecta um contêiner de uma rede.

## Outras Utilidades

- `docker-compose up`: Cria e inicia contêineres definidos em um arquivo `docker-compose.yml`.
- `docker-compose down`: Para e remove contêineres, redes e volumes definidos em um arquivo `docker-compose.yml`.
- `docker inspect [ID ou nome do contêiner/imagem]`: Retorna informações detalhadas sobre um contêiner ou uma imagem.
- `docker stats`: Exibe estatísticas de uso de recursos para contêineres em execução.

## Exemplos de Comandos

- `docker run -d --name nginx -p 8080:80 -v C:/docker/htm/:/user/share/nginx/html nginx`
- `docker run --name nginx2 -d --mount type=volume,source=meuvolume,target=/app nginx`

## Gerando Build da Pasta Atual

- `docker build -t lucassamuel/laravel:latest .`

## Executando Node pela Pasta

- `docker run --rm -it -v "C:/docker/fullcycle/node:/usr/src/app" -p 3000:3000 node:15 bash`

## Subindo o Laravel e o Docker na Mesma Rede [laranet]

```shell
C:\docker\fullcycle\laravel> docker build -t lucassamuel/laravel:prod . -f Dockerfile.prod
C:\docker\fullcycle\nginx> docker build -t lucassamuel/nginx:prod . -f Dockerfile.prod

docker run -d --network laranet --name laravel lucassamuel/nginx:prod
docker run -d --network laranet --name nginx -p 8080:80 lucassamuel/nginx:prod
```

## Diferença de Entrypoint vs CMD
dockerfile
```shell
Copiar código
FROM ubuntu:latest

ENTRYPOINT [ "echo", "hello" ]
CMD ["World!"]
```

- `ENTRYPOINT`: sempre é executado. O CMD é alterado quando é passado um parâmetro.
     - Exemplo: `docker run --rm lucassamuel/hello "teste"`


## Docker-Compose
- `docker-compose up`: Sobe os contêineres e fica no terminal.
- `docker-compose up -d`: Sobe os contêineres e libera o terminal.
- `docker-compose up -d --build`: Refaz os builds dos contêineres e sobe.

## Dockerize
```shell
entrypoint: ["dockerize", "-wait", "tcp://db:3306", "-timeout", "20s", "docker-entrypoint.sh"]
```

- `Dockerize`: É uma ferramenta que auxilia na espera por serviços (como bancos de dados) antes de iniciar a aplicação.
- `-wait "tcp://db:3306"`: Instrução para o Dockerize aguardar a disponibilidade do serviço de banco de dados (presumivelmente um MySQL ou MariaDB) rodando no host db na porta 3306.
- `-timeout "20s"`: Define um tempo limite de 20 segundos para aguardar a disponibilidade do serviço especificado. Se o serviço não estiver disponível dentro desse tempo, o processo falhará.
- `docker-entrypoint.sh`: Após a espera bem-sucedida, este script será executado como ponto de entrada do contêiner.