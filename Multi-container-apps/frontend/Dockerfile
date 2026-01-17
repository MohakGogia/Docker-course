FROM node:14.1-alpine 
# Compatibility issues with later versions of Node.js

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]