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

### `GET /:database/:collection` -- Retrieve Collection Count
You can use a regulator browser, [Postman](https://www.postman.com/), [curl](https://curl.se/) or any other HTTP client to query the MongoDB REST API. This GET request allows to query for the number of documents in collection `:collection` from database `:database`.

To query the document count from `ldesfragment` in `gipod` the (Bash) command line using curl:
```bash
curl http://localhost:9000/gipod/ldesfragment
```
which responds with something like:
```json
{
  "count": 2
}
```

### `GET /:database/:collection?includeIds=true` -- Retrieve Collection Count And Document IDs
In addition to the document count you can request to receive the IDs of the documents in the collection, e.g.:
```bash
curl http://localhost:9000/gipod/ldesfragment?includeIds=true
```
which responds with something like:
```json
{
  "count": 2,
  "ids": [
    "/mobility-hindrances/by-time",
    "/mobility-hindrances/by-time?generatedAtTime=2023-04-17T11:05:45.879Z"
  ]
}
```

### `GET /:database/:collection?includeDocuments=true` -- Retrieve Collection Count And Document Content
In addition to the document count you can request to receive the actual documents in the collection, e.g.:
```bash
curl http://localhost:9000/gipod/ldesfragment?includeDocuments=true
```
which responds with something like:
```json
{
  "count": 2,
  "documents": [
    {
      "_id": "/mobility-hindrances/by-time",
      "root": true,
      "viewName": "mobility-hindrances/by-time",
      "fragmentPairs": [],
      "immutable": false,
      "softDeleted": false,
      "parentId": "root",
      "numberOfMembers": 0,
      "relations": [
        {
          "treePath": "",
          "treeNode": "/mobility-hindrances/by-time?generatedAtTime=2023-04-17T11:05:45.879Z",
          "treeValue": "",
          "treeValueType": "",
          "relation": "https://w3id.org/tree#Relation"
        }
      ],
      "_class": "be.vlaanderen.informatievlaanderen.ldes.server.infra.mongo.fragment.entity.LdesFragmentEntity"
    },
    {
      "_id": "/mobility-hindrances/by-time?generatedAtTime=2023-04-17T11:05:45.879Z",
      "root": false,
      "viewName": "mobility-hindrances/by-time",
      "fragmentPairs": [
        {
          "fragmentKey": "generatedAtTime",
          "fragmentValue": "2023-04-17T11:05:45.879Z"
        }
      ],
      "immutable": false,
      "softDeleted": false,
      "parentId": "/mobility-hindrances/by-time",
      "numberOfMembers": 1016,
      "relations": [],
      "_class": "be.vlaanderen.informatievlaanderen.ldes.server.infra.mongo.fragment.entity.LdesFragmentEntity"
    }
  ]
}
```
> **NOTE**: this can potentionally return a large number of documents or may not work at all if the maximum response length is exceeded. Use wisely!

### `GET /:database/:collection?includeIds=true&includeDocuments=true` -- Retrieve Collection Count, Document IDs and Document Content
Although not very useful, you can request both the ID and document content:
```bash
curl http://localhost:9000/gipod/ldesfragment?includeIds=true&includeDocuments=true
```
