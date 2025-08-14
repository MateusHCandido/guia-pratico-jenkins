FROM node:21.6.0-bullseye
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . . 
EXPOSE 8081
CMD ["node", "server.js"]