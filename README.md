# MongoDB REST API
This small tool allows to do some simple querying towards a MongoDB database over HTTP.

## Docker
The tool can be run as a Docker container, using a pre-built container or after creating a Docker image for it locally. The Docker container will keep running until stopped.

To create a Docker image, run the following command:
```bash
docker build --tag vsds/mongodb-rest-api:latest .
```

To run the Docker image, you can run it interactively, e.g.:
```bash
docker run --rm -it vsds/mongodb-rest-api:latest
```

All available environment variables are (see [below](#run) for details):
* `CONNECTION_URI` (optional)
* `SILENT` (optional)

## Build
The tool is implemented as a [Node.js](https://nodejs.org/en/) application.
You need to run the following commands to build it:
```bash
npm install
npm run build
```

## Run
The tool takes the following command line arguments:
* `--connection-uri=<connection-string>` defaults to `mongodb://localhost:27017`
* `--port=<port-number>` allows to set the port, defaults to `9000`
* `--host=<hostname>` allows to set the hostname, defaults to `localhost`
* `--silent=<true|false>` prevents any console debug output if true, defaults to `false` (not silent, logging debug info)

You can run the tool providing one or more optional arguments after building it, e.g.:
```bash
node dist/server.js --silent true
```
