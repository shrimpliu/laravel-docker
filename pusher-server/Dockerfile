FROM node:6.11.2-alpine

MAINTAINER Shrimp Liu <liu.shrimp@gmail.com>

# Create app directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
 
# Install app dependencies
COPY package.json /usr/src/app/
RUN npm install --registry=https://registry.npm.taobao.org
 
# Bundle app source
COPY laravel-echo-server.json /usr/src/app/laravel-echo-server.json
 
EXPOSE 3000
CMD [ "npm", "start" ]