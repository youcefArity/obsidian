api
```
 "name": "1food1me-api",

 "version": "1.0.0",

 "main": "server/server.js",

 "engines": {

 "node": "\>=6"

 },

 "scripts": {

 "lint": "eslint .",

 "start": "env DEBUG=loopback:connector:postgresql,loopback:relations,loopback:security:\* SENDGRID\_API\_KEY='SG.9nQwoWTkShWGTNEuirItJA.wPJD-I721a8om--\_2MzgbPqAYb-CQrjNto909lBgCBM' nodemon .",

 "test": "mocha test/\*\*/\*.js --reporter nyan --timeout 5000",

 "watchtest": "npm run test -- --watch --growl",

 "posttest": "npm run lint && npm audit"

 },

 "dependencies": {

 "@google/maps": "^0.5.5",

 "@sendgrid/mail": "^6.4.0",

 "awesome-phonenumber": "^2.20.0",

 "compression": "^1.0.3",

 "cors": "^2.5.2",

 "ejs": "^2.7.1",

 "express-xml-bodyparser": "^0.3.0",

 "helmet": "^3.21.2",

 "loopback": "^3.22.0",

 "loopback-boot": "^2.6.5",

 "loopback-component-explorer": "^6.2.0",

 "loopback-connector-postgresql": "^3.8.0",

 "loopback-connector-sendgrid": "^2.2.4",

 "loopback-ds-timestamp-mixin": "^3.4.1",

 "node-schedule": "^1.3.2",

 "nodemon": "^1.19.4",

 "ovh": "^2.0.2",

 "sequelize": "^5.21.1",

 "sequelize-cli": "^5.5.1",

 "serve-favicon": "^2.0.1",

 "stripe": "^7.10.0",

 "strong-error-handler": "^3.4.0"

 },

 "devDependencies": {

 "chai": "\*",

 "chai-change": "^2.1.2",

 "eslint": "^5.16.0",

 "eslint-config-loopback": "^8.0.0",

 "mocha": "^6.2.2",

 "supertest": "\*"

 },

 "repository": {

 "type": "",

 "url": ""

 },

 "license": "UNLICENSED",

 "description": "1food1me-api"

}
```

```
FROM node:12.13.1-alpine3.10

  

USER root

  

RUN apk add --update \\

 python \\

 python-dev \\

 py-pip \\

 build-base \\

 git \\

 openssh-client \\

&& pip install virtualenv \\

&& rm -rf /var/cache/apk/\*

  

RUN mkdir /home/node/.npm-global ; \\

 mkdir -p /home/node/backend ; \\

 chown -R node:node /home/node/backend ; \\

 chown -R node:node /home/node/.npm-global

ENV PATH=/home/node/.npm-global/bin:$PATH

ENV NPM\_CONFIG\_PREFIX=/home/node/.npm-global

  

USER node

  

WORKDIR /home/node/backend

  

COPY package\*.json ./

RUN npm install --prefer-offline --no-audit
```

backoffice
```
FROM ruby:2.5.0

  

ENV BUNDLER\_VERSION=2.0.2

  

ARG DEBIAN\_FRONTEND=noninteractive

RUN apt-get update -qq && apt-get install -y postgresql-client imagemagick apt-transport-https ca-certificates

RUN apt-get install -y --no-install-recommends apt-utils

RUN curl -sL https://deb.nodesource.com/setup\_12.x | bash

RUN apt-get install -y nodejs

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -

RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update -qq && apt-get install -yq yarn

RUN mkdir -p /usr/src/back

WORKDIR /usr/src/back

COPY Gemfile /usr/src/back/Gemfile

COPY Gemfile.lock /usr/src/back/Gemfile.lock

COPY package.json /usr/src/back/package.json

COPY yarn.lock /usr/src/back/yarn.lock

RUN yarn install --check-files

RUN gem install bundler -v 2.0.2

RUN bundle config unset deployment

RUN bundle config unset frozen

RUN bundle install

COPY . /usr/src/back

  

# Add a script to be executed every time the container starts.

COPY entrypoint.sh /usr/bin/

RUN chmod +x /usr/bin/entrypoint.sh

ENTRYPOINT \["entrypoint.sh"\]```


```

Gemfile
```
ource 'https://rubygems.org'

git\_source(:github) { |repo| "https://github.com/#{repo}.git" }

  

ruby '2.5.0'

  

# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'

gem 'rails', '~> 6.0.0'

# Use postgresql as the database for Active Record

gem 'pg', '\>= 0.18', '< 2.0'

# Use Puma as the app server

gem 'puma', '~> 3.11'

# Use SCSS for stylesheets

gem 'sass-rails', '~> 5'

# Transpile app-like JavaScript. Read more: https://github.com/rails/webpacker

gem 'webpacker', '~> 4.0'

# Turbolinks makes navigating your web application faster. Read more: https://github.com/turbolinks/turbolinks

gem 'turbolinks', '~> 5'

# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder

gem 'jbuilder', '~> 2.5'

# Use Redis adapter to run Action Cable in production

# gem 'redis', '~> 4.0'

# Use Active Model has\_secure\_password

# gem 'bcrypt', '~> 3.1.7'

  

# Use Active Storage variant

# gem 'image\_processing', '~> 1.2'

  

# Reduces boot times through caching; required in config/boot.rb

gem 'bootsnap', '\>= 1.4.2', require: false

  

group :development, :test do

 # Call 'byebug' anywhere in the code to stop execution and get a debugger console

 gem 'byebug', platforms: \[:mri, :mingw, :x64\_mingw\]

end

  

group :development do

 # Access an interactive console on exception pages or by calling 'console' anywhere in the code.

 gem 'web-console', '\>= 3.3.0'

 gem 'listen', '\>= 3.0.5', '< 3.2'

 # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring

 gem 'spring'

 gem 'spring-watcher-listen', '~> 2.0.0'

end

  
  

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem

gem 'tzinfo-data', platforms: \[:mingw, :mswin, :x64\_mingw, :jruby\]

  

gem 'devise'

  

gem "slim", "~> 4.0"

  

gem "mini\_magick", "~> 4.9"

gem 'image\_processing', '~> 1.2'

  

gem "inherited\_resources"

  

gem "cocoon"

  

gem "excon"

  

gem 'rack-cors', require: 'rack/cors'

  

gem "to\_regexp"

  

gem 'figaro'

  

gem "aws-sdk-s3", require: false
```

package.json

```
{

 "name": "back\_office",

 "private": true,

 "dependencies": {

 "@rails/actioncable": "^6.0.0-alpha",

 "@rails/actiontext": "^6.0.0-rc1",

 "@rails/activestorage": "^6.0.0-alpha",

 "@rails/ujs": "^6.0.0-alpha",

 "@rails/webpacker": "^4.0.7",

 "trix": "^1.0.0",

 "turbolinks": "^5.2.0"

 },

 "version": "0.1.0",

 "devDependencies": {

 "webpack-dev-server": "^3.6.0"

 }

}
```

front
```
version: "3.3"

  

services:

 db:

 image: postgres

 volumes:

 \- ./tmp/db:/var/lib/postgresql/data

 environment:

 POSTGRES\_USER: 1food1me

 POSTGRES\_PASSWORD: 1food1me

 ports:

 \- "5432:5432"

  

 api:

 image: node:12.13.1-alpine3.10

 build:

 context: api

 dockerfile: Dockerfile

 command: npm run start

 depends\_on:

 \- db

 ports:

 \- "3002:3002"

 working\_dir: /home/node/backend

 volumes:

 \- ./api:/home/node/backend

 \- /home/node/backend/node\_modules

  

 backoffice:

 build:

 context: ./back-office

 dockerfile: Dockerfile

 command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3001 -b '0.0.0.0'"

 volumes:

 \- ./back-office:/usr/src/back

 ports:

 \- "3001:3001"

 environment:

 RAILS\_ENV: development

 depends\_on:

 \- db

  

 front:

 command: npm run dev

 ports:

 \- 3000:3000

 build:

 context: front-end

 dockerfile: Dockerfile

 volumes:

 \- ./front-end:/usr/src/app

 \- /usr/src/app/node\_modules

 \- /usr/src/app/.next

 depends\_on:

 \- api
 ```
 
 ```
 FROM node:12

  

RUN mkdir -p /usr/src/app

WORKDIR /usr/src/app

  

COPY package\*.json ./

RUN npm install

  

COPY . /
```


root
```
version: "3.3"

  

services:

 db:

 image: postgres

 volumes:

 \- ./tmp/db:/var/lib/postgresql/data

 environment:

 POSTGRES\_USER: 1food1me

 POSTGRES\_PASSWORD: 1food1me

 ports:

 \- "5432:5432"

  

 api:

 image: node:12.13.1-alpine3.10

 build:

 context: api

 dockerfile: Dockerfile

 command: npm run start

 depends\_on:

 \- db

 ports:

 \- "3002:3002"

 working\_dir: /home/node/backend

 volumes:

 \- ./api:/home/node/backend

 \- /home/node/backend/node\_modules

  

 backoffice:

 build:

 context: ./back-office

 dockerfile: Dockerfile

 command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3001 -b '0.0.0.0'"

 volumes:

 \- ./back-office:/usr/src/back

 ports:

 \- "3001:3001"

 environment:

 RAILS\_ENV: development

 depends\_on:

 \- db

  

 front:

 command: npm run dev

 ports:

 \- 3000:3000

 build:

 context: front-end

 dockerfile: Dockerfile

 volumes:

 \- ./front-end:/usr/src/app

 \- /usr/src/app/node\_modules

 \- /usr/src/app/.next

 depends\_on:

 \- api
 ```
