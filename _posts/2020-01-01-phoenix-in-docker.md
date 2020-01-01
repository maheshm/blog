---
categories: [tech-blog]
layout: post
title: Bootstrapping Phoenix App in docker
---

I mostly code in Ruby. And I tend to bootstrap lots of project to test out my ideas. I create lots of projects, and experiment different languages and frameworks and docker helps to keep them segragated and help in managing the dependencies easily. Though it has limitations on mac I still tend to use it.

I was trying to get started with a Phoenix app and thougth this might help others as well. It has been taken from multiple places and helps me get started quickly.

I have the following Dockerfile and docker-compose.yml

#### Dockerfile
```
# ./Dockerfile

# Extend from the official Elixir image
FROM elixir:latest

RUN apt-get update && \
  apt-get install -y postgresql-client

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt-get install -y nodejs

# Create app directory and copy the Elixir projects into it
ADD . /app
WORKDIR /app

RUN mix local.hex --force
RUN mix archive.install --force https://github.com/phoenixframework/archives/raw/master/phx_new.ez

EXPOSE 4000
CMD ["./entrypoint.sh"]
```

#### docker-compose.yml
```yml
version: '3'

services:
  web:
    build: .
    ports:
      - "4000:4000"
    volumes:
      - .:/app
    depends_on:
      - db
  db:
    image: postgres:9.6
```

You will notice that I have used a file called `entrypoint.sh`. It has the following content:

#### entrypoint.sh
```bash
#!/bin/bash

# Wait for Postgres to become available.
until psql -h db -U "postgres" -c '\q' 2>/dev/null; do
  >&2 echo "Postgres is unavailable - sleeping"
  sleep 1
done

mix ecto.migrate
mix phx.server
```

Let us assume that I want to create a project called `mvp-app`. Create a directory called the same and put these three files inside it.

To init the app run the following commands:

```bash
$ docker-compose up
$ docker-compose run --rm web mix phx.new mvp-app
```

The first one should exit (may be with some error, I dont remember). The second one will create another directory called mvp-app inside the current directory. Move all the contents inside the new directory to its parent and delete the new one.

You can continue with what Phoenix suggests to continue setting up the project:

```bash
$ docker-compose run --rm web mix ecto.create
$ docker-compose run --rm web mix ecto.migrate
```

Once you are done with this the initial landing page will be visible with simple

```bash
$ docker-compose up
```

Once you have the containers running you can use `exec` instead of `run` in docker-compose.

You can generate models like this

```bash
$ docker-compose exec web mix ecto.gen.migration add_information
```

and run migration as mention before, may be with `exec` this time!

To access you can visit ```localhost:4000``` on you browser. Dont forget to ```git init``` :)
