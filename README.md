# bnex

Estrutura de Diretórios
project-root/
├── docker-compose.yml
├── Front-End-Server/
│   └── Dockerfile
│   └── (outros arquivos e pastas do Front-End)
├── Back-End-Server/
│   └── Dockerfile
│   └── (outros arquivos e pastas do Back-End)
└── DB-Server/
    └── (sem Dockerfile, será configurado no docker-compose.yml)

docker-compose.yml
version: '3.8'

services:
  front-end:
    build:
      context: ./Front-End-Server
    ports:
      - "3000:3000" # Altere para a porta apropriada do Front-End
    depends_on:
      - back-end

  back-end:
    build:
      context: ./Back-End-Server
    ports:
      - "8080:8080" # Altere para a porta apropriada do Back-End
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/mydatabase
      - SPRING_DATASOURCE_USERNAME=myuser
      - SPRING_DATASOURCE_PASSWORD=mypassword
    depends_on:
      - db

  db:
    image: postgres:latest
    environment:
      POSTGRES_DB: mydatabase
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
    ports:
      - "5432:5432"


Dockerfile para Front-End-Server 
# Use uma imagem base apropriada para o seu Front-End
FROM node:14

# Crie o diretório do aplicativo
WORKDIR /usr/src/app

# Copie os arquivos do Front-End
COPY package*.json ./
RUN npm install
COPY . .

# Exponha a porta que o Front-End vai usar
EXPOSE 3000

# Comando para iniciar o Front-End
CMD ["npm", "start"]

Dockerfile para Back-End-Server
# Use uma imagem base apropriada para o seu Back-End
FROM openjdk:11-jre-slim

# Crie o diretório do aplicativo
WORKDIR /usr/src/app

# Copie o JAR do Back-End para o contêiner
COPY target/back-end.jar ./app.jar

# Exponha a porta que o Back-End vai usar
EXPOSE 8080

# Comando para iniciar o Back-End
CMD ["java", "-jar", "app.jar"]

README.md
# Infraestrutura para o Sistema

## Configuração

1. **Clone o repositório**

   ```bash
   git clone <URL-do-repositório>
   cd <nome-do-repositório>

No diretório raiz do projeto, execute:
docker-compose up --build

Verifique se tudo está funcionando

Front-End: Acesse http://localhost:3000
Back-End: Acesse http://localhost:8080
Banco de Dados: O PostgreSQL estará disponível em localhost:5432.

Para parar e remover os contêineres, execute:
docker-compose down

Para configurar o Back-End com o PostgreSQL, certifique-se de que as variáveis de ambiente estão definidas corretamente no docker-compose.yml:

SPRING_DATASOURCE_URL
SPRING_DATASOURCE_USERNAME
SPRING_DATASOURCE_PASSWORD


Esse guia deve cobrir a configuração básica. Se precisar de ajustes específicos, você pode adaptar os Dockerfiles e o `docker-compose.yml` conforme necessário para suas necessidades.




