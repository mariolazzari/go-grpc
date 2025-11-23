# gRPC in Go

## gRPC overview

### gRPC in general

[gRPC](https://grpc.io/)
[gRPC in go](https://grpc.io/docs/languages/go/quickstart/)

- scale
- security
- upgrades
- language independent

### Protocol buffers

[Docs](https://protobuf.dev/)

```proto
syntax = "proto3";

import "google/protobuf/timestamp.proto";

option go_package = "github.com/353solutions/rides/pb";

message Location {
  double lat = 1;
  double lng = 2;
}

enum RideType {
  USET = 0;
  REGULAR = 1;
  POOL = 2;
}

message StartRequest {
  string id = 1;
  string driver_id = 2;
  Location location = 3;
  repeated string passenger_ids = 4;
  google.protobuf.Timestamp time = 5;
  RideType type = 6;
}

message StartResponse { string id = 1; }

message UpdateRequest {
  string driver_id = 1;
  Location location = 2;
}

message UpdateResponse {
  string driver_id = 1;
  int64 count = 2;
}

service Rides {
  rpc Start(StartRequest) returns (StartResponse) {}
  rpc Update(stream UpdateRequest) returns (UpdateResponse) {}
}
```

```sh
brew install protobuf
protoc --version
```

### HTTP/2

- header compression
- server push
- request priority
- requst multiplexing over single connection
- Binary frames
- TLS

### gRPC ecosystem

[gRPC architecture](https://grpc.io/docs/what-is-grpc/core-concepts/)
[gRPC curl](https://github.com/fullstorydev/grpcurl)
[postman](https://learning.postman.com/docs/sending-requests/grpc/first-grpc-request/)
[buf build](https://buf.build/)
[gRPC gateway](https://grpc-ecosystem.github.io/grpc-gateway/)

## Protocol buffer

### Writing proto file

```proto
syntax = "proto3";

import "google/protobuf/timestamp.proto";

option go_package = "github.com/353solutions/rides/pb";

message Location {
  double lat = 1;
  double lng = 2;
}

enum RideType {
  UNSET = 0;
  REGULAR = 1;
  POOL = 2;
}

message StartRequest {
  string id = 1;
  string driver_id = 2;
  Location location = 3;
  repeated string passenger_ids = 4;
  google.protobuf.Timestamp time = 5;
  RideType type = 6;
}
```

### Compiling proto in Go

[Quick start](https://grpc.io/docs/languages/go/quickstart/)

```sh
go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
export PATH="$PATH:$(go env GOPATH)/bin"
go:generate mkdir -p pb
go:generate protoc --go_out=pb --go_opt=paths=source_relative ./rides.proto
```

### Using generated code
