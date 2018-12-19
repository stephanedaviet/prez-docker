<i class="fab fa-docker fa-3x"></i>
# Docker

## Rapide & efficace

par Julien Bourgoin & Stéphane Daviet

---

# Pourquoi Docker ?

<img src="https://media.giphy.com/media/dXICCcws9oxxK/giphy.gif" />

!!!

Stéphane

--- ---

## Un plébiscite

> 37 milliards de pull d'images fin 2018

<img src="img/adoption.png" width="50%" />

--- ---

## Quelques éléments clés

<ul>
    <li class="fragment" data-fragment-index="1">Open-source,</li>
    <li class="fragment" data-fragment-index="2">Facilite la mise en œuvre d'architecture Cloud (twelve factors),</li>
    <li class="fragment" data-fragment-index="3">Écosystème riche,</li>
    <li class="fragment" data-fragment-index="4">Basé sur des technos éprouvées (HTTP, cgroup & namespace, layer FS).</li>
</ul>

--- ---

## Pourquoi le succès Docker ?

Facilité d'utilisation & conception ingénieuse :
<!-- Nécessité de passer par des ul/li pour avoir l'affichage étape par étapr (fragments) sans que les puces soient préaffichées. -->
<ul>
    <li class="fragment" data-fragment-index="1">le **registre** pour stocker & récupérer des images prêtes à l'emploi,</li>
    <li class="fragment" data-fragment-index="2">**Dockerfile** pour décrire la construction d'une image,</li>
    <li class="fragment" data-fragment-index="3">**Docker client** pour lancer simplement les actions usuelles (en commandant un démon),</li>
    <li class="fragment" data-fragment-index="4">images basées sur des **système de fichiers en couches** très efficace (optimisation de la taille).</li>
</ul>

--- ---

## Le registre

> 1,8 million d'images disponibles

<img src="img/dockerHub.png" width="70%" />

!!!

* Repository public & privé d'images Docker (par défaut à l'install), à la npm,
* Système de notation communautaire,
* Images officielles & library,
* Vulnerability security scanning,
* Recherche possible avec la commande `docker search`.

---

# Le client

<img src="https://media.giphy.com/media/l0MYJ3p6Eg2tUHq6s/giphy.gif" />

!!!

Julien

--- ---

### Connexion avec le démon

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li>Démon = service système</li>
            <li>Protocole HTTP & API REST</li>
        </ul>
    </div>
</div>

!!!

JULIEN
1. `service docker status`
2. `curl --unix-socket /var/run/docker.sock http://localhost/images/json | jq .`
3. `docker version`

--- ---

## Manipulation des images

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li>Récupération : <code>docker pull &lt;image-name&gt;<sup>[1]</sup></code></li>
            <li>Listing : <code>docker images -a</code></li>
            <li>Suppression : <code>docker rmi &lt;image-name&gt;</code></li>
        </ul>
    </div>
</div>
<div class="footnote"><sup>[1]</sup> Nom complet : [registry-url/]image-name[:version]</div>

!!!

JULIEN
1. `docker pull alpine`
2. `docker images -a`
3. `docker rmi alpine`

--- ---

## Les conteneurs

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li>Exécution : <code>docker run -t &lt;container-name&gt; &lt;image-name&gt;</code></li>
            <li>Listing : <code>docker ps -a</code></li>
            <li>Suppression : <code>docker rm &lt;container-name&gt;</code></li>
            <li>Exécution intra conteneur : <code>docker exec -it &lt;container-name&gt; &lt;command&gt;</code></li>
    </div>
</div>

!!!

JULIEN
* `docker run hello-world`
* `docker ps -a`
* `docker run -it alpine`

---

# Le Dockerfile

!!!

Stéphane

--- ---

## Anatomie

Une grammaire simple :
```dockerfile
FROM openjdk:8u111-alpine

RUN addgroup -g 10064 dck && adduser -S -u 10064 -G dck admdck
RUN mkdir -p app

VOLUME /etc/localtime:/etc/localtime:ro
ENV _JAVA_OPTIONS="-Duser.timezone=Europe/Paris"

WORKDIR /app
COPY target/app.jar .

EXPOSE 8080

USER admdck
ENTRYPOINT java
CMD ["-jar", "app.jar"]
```

!!!

* Explication de chacune des instructions :
  * `FROM` : image de base,
  * `RUN` : exécution d'une commande,
  * `VOLUME` : partage d'un volume avec l'hôte,
  * `ENV` : déclaration de variable d'environnement,
  * `COPY` : copie d'un élément de l'hôte dans l'image,
  * `WORKDIR` : définition du répertoir de travail,
  * `EXPOSE` : port exposé,
  * `USER` : utilisateur exécutant les prochaines commandes,
  * `ENTRYPOINT` : commande executée lors du démarrage d'un container basé sur cette image
  * `CMD` : paramètres par défaut de l'ENTRYPOINT
  * et quelques autres…

--- ---

## Création & publication d'une image

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>
            <li>Construction : <code>docker build -t &lt;image-name&gt; .</code></li>
            <li>Authentification au registre : <code>docker login &lt;registry-url&gt;</code></li>
            <li>Nommage : <code>docker tag &lt;source-image-name&gt; &lt;target-image-name&gt;</code></li>
            <li>Récupération : <code>docker push &lt;image-name&gt;</code></li>
        </ul>
    </div>
</div>

!!!

* `cd builds/webpage`
* `docker build -t webpage .`
* `docker run webpage`
* `docker tag webpage stephanedaviet/webpage:latest`
* `docker push stephanedaviet/webpage:latest`

--- ---

## Build multi-stages

<div class="cols">
    <div class="shell left">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <!-- &nbsp; = astuce pour éviter une ligne vide, bug renderer marked -->
        <pre><code class="dockerfile">FROM golang:alpine AS build-env
RUN mkdir /src
ADD hello.go /src
RUN cd /src && go build -o goapp
&nbsp;
FROM alpine
WORKDIR /app
COPY --from=build-env /src/goapp /app/
ENTRYPOINT ./goapp</code></pre>
    </div>
</div>

!!!

* `cd builds/multistage`,
* `docker build -f Dockerfile -t multistage/hello .`,
* `docker run --rm multistage/hello`,
* `docker history multistage/hello`,
* `docker images`.

L'image golang:alpine fait 287Mo, quand l'image finale de l'application fait 5,71Mo

--- ---

# TODO

!!!

* `docker build -t webpage .`,
* `docker run goapp`.

---

# Layers

<img src="https://media.giphy.com/media/jHTrcBVhfCGNq/giphy-downsized.gif" />

!!!

JULIEN

--- ---

## Docker pull

<div class="rows">
    <div class="shell up">
        <iframe data-src="http://localhost:8080"></iframe>
    </div>
    <div>
        <ul>Une image est constitué de plusieurs couches :
            <li>celles des images sur lesquelles elle se base directement & transitivement,</li>
            <li>celles introduites par les instructions de son Dockerfile.</li>
        </ul>
    </div>
</div>

!!!

JULIEN
* Téléchargement image de base : `docker pull debian:jessie-slim`
* Téléchargement image Node basé sur la première : `docker pull node:carbon-jessie-slim`

--- ---

## Dockerfile

<div class="rows">
    <div>
        <ul>
            <li>Chaque ligne d'un Dockerfile produit un layer,</li>
            <li>La modification d'une ligne entraine la reconstruction du layer associé et tous les suivants,</li>
            <li>Certaines instructions en dépendances avec un contenu externe comme COPY entrainent la reconstruction du layer si ce contenu change,</li>
            <li>L'ordre des instructions doit être optimisé pour éviter la reconstruction systématique des layers.</li>
			<li>Le regroupement des instructions RUN est conseillée si possible</li>
			<li>Attention au cache !</li>
        </ul>
    </div>
    <div class="cols" style="width: 100%">
        <div class="shell right" style="flex: 1 1 auto">
            <iframe data-src="http://localhost:8080"></iframe>
        </div>
        <div class="shell left" style="flex: 1 1 auto">
            <iframe data-src="http://localhost:8080"></iframe>
        </div>
    </div>
</div>

!!!

JULIEN
* `cd builds/layers`
* `docker build -f Dockerfile.dumb -t testlayer-dumb .`
* `docker build -f Dockerfile.better -t testlayer-better .`

--- ---

## Layered filesystems

<img src="img/container-unionfs-rw-layers.png" width="40%" />

UnionFS, AuFS, Btrfs, zfs, overlay, overlay2, devicemapper


---

# Liens avec l'hôte

!!!

Stéphane

--- ---

## Exposition de ports

--- ---

## Montage de volumes

---

# Sécurité & common flaws

!!!

Julien

--- ---

## Images cacas

--- ---

## Élévation de privilèges

---

# DockerInDocker

!!! Julien

--- ---

## Le faux Docker in Docker

--- ---

## Le vrai

---

# Quelques liens

---

# Vos questions