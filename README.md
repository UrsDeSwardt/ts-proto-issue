# ts-proto-issue

This repo serves as a minimal reproduction of an issue with [ts-proto](https://github.com/stephenh/ts-proto).

## Problem

When using the option `--ts_proto_opt=noDefaultsForOptionals=true`, the generated TypeScript code incorrectly marks all fields as possibly undefined, including non-optional ones.

## Example

Given the following `.proto` file:

```proto
syntax = "proto3";

message Message {
  string foo = 1;
  optional string bar = 2;
}
```

Running:

```bash
protoc --plugin=./node_modules/.bin/protoc-gen-ts_proto --ts_proto_out=. ./example.proto  --ts_proto_opt=noDefaultsForOptionals=true
```

Expected output:

```ts
export interface Message {
    foo: string;
    bar?: string | undefined;
}
```

Actual output:

```ts
export interface Message {
    foo?: string | undefined;
    bar?: string | undefined;
}
```
