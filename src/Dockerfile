FROM node:21.6.0-alpine3.18
WORKDIR /app

# Instala ferramentas de build
RUN apk add --no-cache python3 make g++

COPY package*.json ./

RUN npm install

COPY . . 
EXPOSE 8081
CMD ["node", "server.js"]
