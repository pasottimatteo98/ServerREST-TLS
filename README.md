# ServerREST

A Java-based REST server implementation for managing tool inventory through JSON files.

## Overview

This project implements a simple REST API server that manages a collection of tool records stored as JSON files. The server supports standard CRUD operations (Create, Read, Update, Delete) through HTTP methods and organizes tools hierarchically in directories.

## Features

- **REST API** supporting GET, POST, PUT, and DELETE operations
- **JSON file storage** for persistent data
- **Versioning support** for data records
- **Hierarchical organization** of tools in directory structure

## Project Structure

```
ServerRest/
├── .classpath           # Eclipse classpath configuration
├── .project             # Eclipse project configuration
├── Ferramenta.java      # Model class for tool objects
├── JsonPage.java        # HTTP handler for REST operations
├── RESTHttpServer.java  # Main server class
└── Root/                # Root directory for data storage
    ├── Bulloni/         # Example directory for bolts
    │   ├── Ferro/       # Metal bolts
    │   │   ├── v15x3.json
    │   │   ├── v15x4.json
    │   │   └── v15x7.json
    │   └── Legno/       # Wood bolts
    │       └── v85x5.json
    └── Viti/            # Screws directory
        └── v15x9.json
```

## Dependencies

- Java JRE/JDK
- Google Gson library (gson-2.6.2.jar)

## Installation

1. Clone the repository
2. Ensure you have Java installed
3. Place the gson-2.6.2.jar in your classpath (currently configured for a specific path in .classpath)

## Usage

### Starting the Server

```bash
java RESTHttpServer [-port PORT_NUMBER]
```

The server runs on port 3000 by default.

### API Endpoints

All endpoints use the base path `/index`.

#### GET

- **List all directories/files**: GET `/index/[path]`
- **Retrieve a specific record**: GET `/index/[path]/?type=[filename]`
- **Retrieve with version**: GET `/index/[path]/?type=[filename]&version=[version]`

#### POST (Create)

- **Create a new record**: POST `/index/[path]/?type=[filename]&id=[id]&N=[quantity]&Usato=[true/false]&version=[version]`

#### PUT (Update)

- **Update an existing record**: PUT `/index/[path]/?type=[filename]&id=[id]&N=[quantity]&version=[version]`

#### DELETE

- **Delete a record**: DELETE `/index/[path]/?type=[filename]`

### Data Model

The `Ferramenta` class represents tools with the following properties:

- `id` (String): Unique identifier for the tool
- `N` (int): Quantity available
- `Usato` (boolean): Whether the tool is used or new (available in version 1.1+)

## Example Requests

### Retrieve All Items in a Directory

```
GET http://localhost:3000/index/Bulloni/Ferro/
```

### Retrieve a Specific Item

```
GET http://localhost:3000/index/Viti/?type=v15x9
```

### Create a New Item

```
POST http://localhost:3000/index/Viti/?type=newScrew&id=A2000&N=100&Usato=false
```

### Update an Item

```
PUT http://localhost:3000/index/Bulloni/Ferro/?type=v15x3&id=A9999&N=200
```

### Delete an Item

```
DELETE http://localhost:3000/index/Viti/?type=v15x9
```

## Notes

- The server stores all data as JSON files in the Root directory
- Version control is implemented using Gson's `@Since` annotation
- Directory structure in Root represents the organization of tools
