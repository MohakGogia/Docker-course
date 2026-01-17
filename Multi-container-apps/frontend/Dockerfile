FROM node 
# FROM node:14.1-alpine (For running via docker-compose use latest version and for separately running it, uncomment this line)
# Compatibility issues with later versions of Node.js

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]