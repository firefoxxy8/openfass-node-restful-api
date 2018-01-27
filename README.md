# OpenFaaS RESTful API w/ Node and Postgres

Simple example of an [OpenFaaS](https://www.openfaas.com/) RESTful API.

## Setup

Spin up [OpenFaas](https://www.openfaas.com/):

```sh
$ docker swarm init
$ git clone https://github.com/openfaas/faas && \
  cd faas && \
  ./deploy_stack.sh
```

Assuming you have an instance of Postgres running, create a new database along with a new table:

```sh
CREATE TABLE movie(id SERIAL, name varchar);
```

Rename *env-sample.yml* to *env.yml* and update the variables:

```yaml
environment:
  POSTGRES_USER: update
  POSTGRES_PASS: update
  POSTGRES_DB: update
  POSTGRES_HOST: update
```

Run a build with the [faas-cli](https://github.com/openfaas/faas-cli):

```sh
$ faas-cli build -f template.yml
```

Deploy:

```sh
$ faas-cli deploy -f template.yml
```

Manually test:

```sh
$ curl -X POST http://localhost:8080/function/create -d \
  '{"name":"NeverEnding Story"}'

$ curl http://localhost:8080/function/read

$ curl -X POST http://localhost:8080/function/update -d \
  '{"name":"NeverEnding Story 2", "id": "1"}'

$ curl -X POST http://localhost:8080/function/delete -d \
  '{"id":"1"}'
```
