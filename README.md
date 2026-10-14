# Url Shortener 2

A url shortener using the CRC32 hashing algorithm and the following tech stack:

- Kotlin
- Spring Boot
- MongoDB
- Docker
- Kubernetes

## Disclaimer

This app isn't production ready:

- it only works on localhost (short urls are relative)
- test coverage is low
- CRC32 hash collisions silently overwrite existing urls (`save()` upserts)
- unknown keys return 500 instead of 404
- long urls aren't validated (e.g. urls without a scheme redirect within the app)
- MongoDB in k8s has no volume, so data is lost on restart
- no health checks or resource limits in k8s

## local development

- start database(s) `docker-compose up`
- Interact with docker `docker exec --tty --interactive <container> <program>`
  - MongoDB Shell `mongosh`
- start server: with IntelliJ or `./gradlew bootRun`

### mongosh

- `use urls`
- `db.urls.find()`

## features

- add a url `./scripts/createUrl.sh "8081" "https://www.learnitmyway.com"`
- paste url in response in browser and get redirected

## local deployment with Kubernetes

1. start Kubernetes cluster with Docker Desktop
2. build jar, build docker image and push to docker hub: `./scripts/build-and-push.sh`
3. deploy everything `kubectl apply -f k8s`
  - ip address of url-service is `localhost:30081`
