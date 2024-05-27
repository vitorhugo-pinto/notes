Docker Compose is a tool for defining and running multi-container applications. It is the key to unlocking a streamlined and efficient development and deployment experience.

`docker-compose up -d --build
- -d: detached from terminal
- --build will rebuild images (in case there was changes in `Dockerfile` used to build the services)
`docker-compose down`
`docker-compose stope <name>`

[Example](https://docs.docker.com/compose/compose-application-model/)

[wait-for-it](https://github.com/codeedu/docker-wait-for-it)
Tool for services to wait for another service to be ready.

