---
categories: [tech-blog]
layout: post
title: Webpacker hot-reload (HRM) in Docker
tags: [rails, docker]
---

I was trying to get the HRM working in the docker environment today. What I forgot is how the webpacker try to resolv the webpacker server, which is not obvious in non-docker environment. Few of the points that I learned:

1. Webpacker host and port configurations are important in both `docker-compose.yml` as well as in `webpacker.yml` . Websocket is initialised with these values.
2. It is better to have a separate image that runs the `bin/webpack-dev-server`
3. Compile on demand is way too slow for development if your files are huge, or even medium sized.
4. `public` and `host` configuration in `dev_server` in `webpacker.yml` are two different things.
5. There is some issue with the docker already containing the compiled files in the output directory (typically in `public/packs`).

This was the final configuration that I had to get it up and running:

`docker-compse.yml`
-----------
```
services:
  webpacker:
    build: .
    command: bash -c "rm -rf /myapp/public/packs; /myapp/bin/webpack-dev-server"
    volumes:
      - .:/myapp
    ports:
      - "3035:3035"
  db:
    image: postgres:9.6
    ports:
      - "5432"
  app:
    build: .
    command: bash -c "rm /myapp/tmp/pids/server.pid; bundle exec rails server -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - webpacker
      - postgres
```
`dev_server` section in `webpacker.yml`
---
```
  dev_server:
    https: false
    host: webpacker # <-- this is they key
    port: 3035
    public: 0.0.0.0:3035
    hmr: true
    # Inline should be set to true if using HMR
    inline: true
    overlay: true
    disable_host_check: true
    use_local_ip: false
```

A `docker-compose up` will start the web server along with the webpacker sever that will watch for change and pass it via a websocket to the frontend which will get automatically reloaded after the diff got compiled.

Very fast!

[This comment helped to fix this configuration](https://github.com/rails/webpacker/issues/1019#issuecomment-351066969)
