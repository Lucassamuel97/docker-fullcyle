FROM node:16

WORKDIR /usr/src/app

# Copie os arquivos de package.json e package-lock.json primeiro
COPY package*.json ./

# Instale as dependências do projeto
RUN npm install

# Copie todos os arquivos do projeto
COPY . .

# Instale Dockerize
ENV DOCKERIZE_VERSION v0.6.1
RUN apt-get update && apt-get install -y wget && \
    wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

EXPOSE 3000

# Comando para iniciar a aplicação
CMD ["node", "index.js"]