# Derivation of a gRPC API Specification From an Application Microservice API Diagram

To derive the gRPC API specification for an application microservice API diagram a systematic approach can be taken. 
This approach allows each modeling element used in the API diagram to be transferred into the API specification for gRPC.
The API specification consists of a protocol buffer (.proto) file which contains all generated code [Pro-Ove]. To familiarize with built-in types, refer to the documentation [Pro-Sca]. gRPC services and messages are written according to the style guides [\[Pro-Sty\]](https://protobuf.dev/programming-guides/style/#services). The approach consists of the following main steps:

Note: 


### Step 1: Analyze the API Diagram
1. Identify entities and collections that provide gRPC functions, represented as methods.
2. Entities and collections that house these functions will translate into gRPC services.
3. Mark only methods in entities or collections meant for interaction as it will be needed in the second step.

### Step 2: Initiate gRPC Services
1. Construct gRPC services reflecting the previously identified entities/collections.
2. When naming the gRPC service, use the name of the entity and append "Service", or use the name of collection and append "CollectionService" to avoid naming collisions.
```protobuf
service CustomerService {}
service CustomersCollectionService{}
```
3. Translate each interactive method from the initial step into an RPC function in the service.
4. For each method, introduce messages named `<MethodName>Request` for RPC input and `<MethodName>Response` for RPC output.
```protobuf
service CustomerService {
    rpc RentCar(RentCarRequest) returns (RentCarResponse) {}
    ...
}
message RentCarRequest {}
message RentCarResponse {}
...
```

**Note**: 
- If a method does not have parameters, the corresponding request or response message remains empty. In such cases, use `google.protobuf.Empty`.
- Include [status codes](https://grpc.io/docs/guides/status-codes/) if necessary.  

### Step 3: Map Enumerations
1. Convert enumerations from the API diagram to gRPC using the `enum` type.
2. Translate each value within the enumeration to an equivalent entry in the gRPC `enum`, starting with the value `0`.

**Note**: 
- Always set a default value, such as `UNSPECIFIED`, to differentiate between the default and specified values.
```protobuf
enum LicenseCategory {
    UNSPECIFIED = 0;
    A = 1;
    B = 2;
    C = 3;
}
```

### Step 4: Translate Every Entity and Value Object
1. Turn each entity and value object from the API diagram into a gRPC message.
2. Ensure that the name of the gRPC message matches the object's name from the API diagram.
```protobuf
message Customer{}
message Rental {}
...
```
3. For entities that possess complex identifiers, it's recommended to utilize nested messages. This pertains to situations where the identifying attribute of an entity isn't merely a straightforward scalar type but rather a composite of several attributes.
```protobuf
message Car {
    Identifier id = 1;
    // other attributes
    message Identifier {
        string vin = 1;
        string registrationNumber = 2;
    }
}
```
4. Map each attribute within the entity/value object to the gRPC message using built-in primitive types when possible. If not, define a new message and use it as the type.
```protobuf
message Car {
    Vin vin = 1; // new message used as a type.
    string brand = 2; // built-in primitive type.
    // other attributes
}
message Vin {
    string vin = 1;
}
```

5. Every application entity derived from a domain entity should encompass all the attributes of the domain entity in addition to the unique attributes specified within the application entity.
```protobuf
message Car {
    // attributes inherited from domain entity.
    Vin vin = 2; 
    string brand = 3;
    string model = 4;
    // followed by application specific attributes.
    float pricePerDay = 1; // application specific attribute.
}
```

**Note**: For attributes that are of type list or array, prefix the type with `repeated`.

### Step 5: Transfer Input and Output Parameters for gRPC function
1. Transfer each input parameter field in method to a field within the message titled `<MethodName>Request`.
Note: The parameters (attributes) are added in the same order as in the API diagram.
```protobuf
message RentCarRequest {
    google.protobuf.Timestamp startDate = 1;
    google.protobuf.Timestamp endDate = 2;
    Vin vin = 3;
    string customerID = 4;
}
...
```

2. Transfer output parameter field in method to a field within this message titled `<MethodName>Response`. The reason behind defining response message, however the output is only one field,
is the api response could have another technical fields like error field.
```protobuf
message RentCarResponse {
    Rental rental = 1;
    ErrorDetail error = 2;
}
...
```

**Note**:
- Each response should include an `ErrorDetail` attribute to communicate error specifics. The `ErrorDetail` has two attributes:
    - `message`: A short description of the error.
    - `details`: More elaborate details or context about the error.

```protobuf
message ErrorDetail {
    string message = 1;
    string details = 2;
}
```

---

**References**:
- [Pro-Sca] [Protocol Buffers Documentation: Scalar Types](https://protobuf.dev/programming-guides/proto3/#scalar)
- [Pro-Sty] [Protocol Buffers Documentation: Style Guide](https://protobuf.dev/programming-guides/style/#services)
- [Pro-Enu] [Protocol Buffers Documentation: Enumerations](https://protobuf.dev/programming-guides/enum/)
