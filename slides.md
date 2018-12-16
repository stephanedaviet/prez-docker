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

## Interaction avec le registre

<div class="rows">
    <div>
        <ul>
            <li><code>docker pull &lt;image-name&gt;</code></li>
            <li><code>docker run &lt;image-name&gt;</code></li>
            <li><code>docker push &lt;image-name</code></li>
        </ul>
    </div>
    <div style="padding-top: 1em; width: 100%">
        <iframe data-src="http://localhost:8080" width="100%" />
    </div>
</div>

--- ---

## Manipulation des images

<div class="rows">
    <div>
        <ul>
            <li><code>docker images</code></li>
            <li><code>docker rmi &lt;image-name&gt;</code></li>
        </ul>
    </div>
    <div style="padding-top: 1em; width: 100%">
        <iframe data-src="http://localhost:8080" width="100%" />
    </div>
</div>

---

# Le Dockerfile

---

# Images & conteneurs

---

# LayerFS

---

# DockerInDocker

---

# Sécurité & common flaws