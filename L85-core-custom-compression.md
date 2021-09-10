Core: support for custom message compression algorithms
----
* Author(s): [Matt Brown](https://github.com/mattnworb)
* Approver: a11r
* Status: Draft
* Implemented in: none
* Last updated: September 10, 2021
* Discussion at: <google group thread> (filled after thread exists)

## Abstract

[A short summary of the proposal.]

## Background

For languages that wrap the gRPC core library (Python, Ruby, Node, C#, etc),
message compression/decompression is handled in gRPC's C core. The only
algorithms currently supported by gRPC core (defined in the
`src/core/lib/compression` directory) are:

- none
- Deflate
- GZIP

The Java and Go gRPC implementations allow users to register additional
compression types besides the built-in GZIP support by implementing an interface
(named `Compressor` in Go and `Codec` in Java) and adding it to a channel or
server's compression registry.

Support for additional compression types in gRPC core and the wrapped languages
has often been requested (TODO add links to issues) but is not possible today
due to the lack of a similar mechanism in gRPC core for registering additional
types at runtime.

Since gRPC core is distributed in binary form, adding additional compression
types directly in core is not desired as it would require either statically
linking the libraries into the core's binary artifact, thus increasing the
binary size, or placing a burden on users who install the core library to also
install additional libraries even if they will not be used. The addition of new
dependencies to gRPC core has an associated maintenance cost as well, and there
is also a need to consider that different compression libraries (zstd, brotli,
Snappy, etc.) may not support all of the platforms that gRPC core is built and
distributed for.

### Related Proposals:

n/a

## Proposal

[A precise statement of the proposed change.]

gRPC core should allow for custom compression types that can be registered at runtime. These custom compression types will not be directly implemented in the core, but rather via user-supplied code that presumably calls into code supplied by other libraries (zstd, Brotli, Snappy, etc).

This support will extend to all of the wrapped languages (Python, Ruby, Node, etc.) so that users can register custom compressors on a channel or server.

TODO: talk about how the behavior of channels/servers in regards to which compression types are supported by them (defined in <https://github.com/grpc/grpc/blob/master/doc/compression.md>) will not change.

TODO: name the library's packages

TODO: does this require the need to distribute additional libraries, e.g. `grpc-brotli`, `grpc-snappy`, which provides the implementation of this interface?

### Temporary environment variable protection

[Name the environment variable(s) used to enable/disable the feature(s) this proposal introduces and their default(s).  Generally, features that are enabled by I/O should include this type of control until they have passed some testing criteria, which should also be detailed here.  This section may be omitted if there are none.]

## Rationale

[A discussion of alternate approaches and the trade offs, advantages, and disadvantages of the specified approach.]


## Implementation

[A description of the steps in the implementation, who will do them, and when.  If a particular language is going to get the implementation first, this section should list the proposed order.]

## Open issues (if applicable)

[A discussion of issues relating to this proposal for which the author does not know the solution. This section may be omitted if there are none.]
