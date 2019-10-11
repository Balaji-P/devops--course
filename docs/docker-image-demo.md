<!-- .slide: class="center dark" -->
<!-- .slide: data-background="../img/background/hands-on.jpg" -->
# Docker Images

<div class="label">Hands-on Time</div>


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Requirements

* Docker For Mac/Windows, or Docker installed on Linux
* A Docker Hub user


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Building Images

```bash
git clone https://github.com/vfarcic/go-demo-2.git

cd go-demo-2

cat Dockerfile

docker image build -t go-demo-2 .

docker image ls
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Building Images

* Docker's Multi-Stage Builds
* Allows execution of multiple stages while maintaining small size of the final image
* We run unit tests and we built the application binary
* The result of the first stage (the binary) was used in the final stage to assemble the image


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Pushing Images

```bash
DOCKER_HUB_USER=[...]

docker login -u $DOCKER_HUB_USER

docker image tag go-demo-2 $DOCKER_HUB_USER/go-demo-2:beta

docker image push $DOCKER_HUB_USER/go-demo-2:beta

cd ..
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Pushing Images

* We logged in to Docker Hub (it could be any other registry)
* We tagged the image using a convention that clearly indicates that it is not fully tested
* We pushed the image to the registry (Docker Hub)