# Generate gRPC Client and Server Stubs From gRPC Specification

This best practice describes the generation of gRPC client and server stubs based on a protobuf specification. 

To generate the respective stubs, protobuf requires the protocol buffer compiler `protoc`. A guide for the installation of the compiler can be found at the official documentation [\[gRPC-PBC\]](https://grpc.io/docs/protoc-installation/). 

## Go gRPC Server

To generate Go stubs based on a protobuf definition, a plugin must be installed for the protocol compiler [\[gRPC-QG\]](https://grpc.io/docs/languages/go/quickstart/). 

1. Install the compiler plugin: 
    ```
    $ go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
    $ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
    ```
    Note: There might be a newer version of the library installed by the command `go install ..`. Thus, please check for a newer version. 
2. Ensure that the binaries of the Go environment (`GOPATH/bin`) are in the `PATH` environment
3. Enter the root folder of the respective microservice
4. Generate the gRPC Server using the protoc compiler:
    ```
    $ protoc --go_out=src/api/controller  --go-grpc_out=src/api/controller src/api/specification/<am-...>.proto
    ```
    Note: The option `--go-grpc_out` defines the interfaces and handlers of the API controller. The option `--go_out` defines the Go data structures used by the interfaces.  
5. The generated stubs are available in the folder `src/api/controller/pb` and can be used for the implementation of the API controller. 

## gRPC-Web Client

Since the Web still relies on HTTP1.1, a frontend can not communicate with a gRPC server directly. Therefore, gRPC-Web is used, which allows  web clients to access a gRPC servers through a proxy [\[gRPC-Web\]](https://grpc.io/docs/platforms/web/basics/).

1. To generate a gRPC-Web client, the protoc compiler is used (see [\[gRPC-Web\]](https://grpc.io/docs/platforms/web/basics/) [\[Git-grpc\]](https://github.com/grpc/grpc-web)).
    ```
    $ protoc --grpc-web_out=import_style=typescript,mode=grpcweb:<output_folder> <path_to_specification>/<specification_name>.proto
    ```

Note: To use gRPC-Web without an API proxy (e.g., Envoy), a wrapper for the Go server is required. A library is available at [\[Git-grp\]](https://github.com/improbable-eng/grpc-web/tree/master/go/grpcweb). 

## References

[Git-grp] GitHub.com: gprc/grpcweb. https://github.com/grpc/grpc-web  
[Git-imp-grp] GitHub.com: improbable-eng/grpc-web. https://github.com/improbable-eng/grpc-web/tree/master/go/grpcweb  
[gRPC-PBC] gRPC.io: Protocol Buffer Compiler Installation. https://grpc.io/docs/protoc-installation/  
[gRPC-QG] gRPC.io: Quickstart | Go | gRPC. https://grpc.io/docs/languages/go/quickstart/  
[gRPC-Web] grpc.io: Basics tutorial | Web | gRPC. https://grpc.io/docs/platforms/web/basics/
