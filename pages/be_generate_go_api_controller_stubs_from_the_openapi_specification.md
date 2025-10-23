# Generate Go API Controller From OpenAPI Specification

This best practice describes the steps required to use the plugin for generating API-relevant Go structs and interfaces.

The OpenAPI specification serves as the foundation for developing the Go API controller code artifacts. It allows to generate Go structs representing OpenAPI component schemas and Go interface methods representing OpenAPI endpoints from the API specification. For this, the [oapi-codegen](https://github.com/deepmap/oapi-codegen) plugin can be used. The automatic generation of the controller simplifies the implementation of the API.

The following steps are required to generate the controller:

1. Ensure Go is properly installed via the command
   ```
   go version
   ```
2. Install the oapi-codegen plugin via the command
   ```
   go install github.com/oapi-codegen/oapi-codegen/v2/cmd/oapi-codegen@latest
   ```
   Note: There might be a newever version of the tool oapi-codegen. This can have an effect on the input parameters. To solve the respective M2Go tasks, the command displayed in this step should be used. 
3. Open the microservice repository
4. Create directory `src/api/controller` and navigate to the directory in the terminal
5. Generate structs representing OpenAPI component schemas via the command
   ```
   oapi-codegen -package controller -generate types <relative-path-to-api-specification> > server-model.gen.go
   ```
6. Generate interface methods representing OpenAPI endpoints via the command
   ```
   oapi-codegen -package controller -generate server <relative-path-to-api-specification> > server.gen.go
   ```
7. Now, the directory `src/api/controller` contains the two files `server-models.gen.go` and `server.gen.go` with the generated API relevant Go structs and interfaces.
8. Open each generated file and sync dependencies
9. Implement the interface `ServerInterface` located in the file `server.gen.go` to use the generated API controller. 
10. If there is an import error, enter the `src` folder and update the Go modules via the commands
   ```
      go get github.com/oapi-codegen/runtime
      go mod tidy
   ```
