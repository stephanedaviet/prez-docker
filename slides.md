<i class="fab fa-docker fa-3x"></i>
# Docker

## Rapide & efficace

par Julien Bourgoin & Stéphane Daviet

---

## Un plébiscite

> 37 milliards de pull d'images fin 2018

<img src="img/adoption.png" width="60%" />

---

## Pourquoi le succès Docker ?

Facilité d'utilisation & conception ingénieuse :
<!-- Nécessité de passer par des ul/li pour avoir l'affichage étape par étapr (fragments) sans que les puces soient préaffichées. -->
<ul>
    <li class="fragment" data-fragment-index="1">le **registre** pour stocker & récupérer des images prêtes à l'emploi,</li>
    <li class="fragment" data-fragment-index="2">**Dockerfile** pour décrire la construction d'une image,</li>
    <li class="fragment" data-fragment-index="3">**Docker client** pour lancer simplement les actions usuelles (en commandant un démon),</li>
    <li class="fragment" data-fragment-index="4">images basées sur des **système de fichiers en couches** très efficace.</li>
</ul>

---

## Le registre

> 1,8 million d'images disponibles

<img src="img/dockerHub.png" width="80%" />

---

# Le client

--- ---

## Pour commencer

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li><code>docker pull &lt;image-name&gt;<sup>[1]</sup></code></li>
            <li><code>docker run &lt;image-name&gt;</code></li>
        </ul>
    </div>
</div>
<div class="footnote"><sup>[1]</sup> Nom complet : [registry-url/]image-name[:version]</div>

--- ---

## Manipulation des images

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li><code>docker images -a</code></li>
            <li><code>docker rmi &lt;image-name&gt;</code></li>
        </ul>
    </div>
</div>

--- ---

## Les conteneurs

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li><code>docker run -t &lt;container-name&gt; &lt;image-name&gt;</code></li>
            <li><code>docker ps -a</code></li>
            <li><code>docker rm &lt;container-name&gt;</code></li>
            <li><code>docker exec -it &lt;container-name&gt; &lt;command&gt;</code></li>
    </div>
</div>

---

# Le Dockerfile

--- ---

## Anatomie

<pre><code class="dockerfile">FROM openjdk

RUN mkdir app

WORKDIR /app

VOLUME /certs

ENV _JAVA_OPTS -XmX 256M

COPY target/app.jar .

ENTRYPOINT java

EXPOSE 8080

CMD ["-jar" "app.jar]</code></pre>

--- ---

## Création & publication d'une image

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li><code>docker build - &lt;image-name&gt; .</code></li>
            <li><code>docker push &lt;image-name&gt;</code></li>
            <li><code>docker tag &lt;source-image-name&gt; &lt;target-image-name&gt;</code></li>
        </ul>
    </div>
</div>

--- ---

## Build multi-stages

<div class="cols">
    <div class="shell right">
        <iframe data-src="https://heeeeeeeey.com/"></iframe>
    </div>
    <div>
        <pre><code class="dockerfile"># build stage
FROM golang:alpine AS build-env
RUN mkdir /src
ADD hello.go /src
RUN cd /src && go build -o goapp

# final stage
FROM alpine
WORKDIR /app
COPY --from=build-env /src/goapp /app/
ENTRYPOINT ./goapp</code></pre>
    </div>
</div>

---

# LayerFS

---

# DockerInDocker

---

# Sécurité & common flaws