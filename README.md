# Yet Another Multi-Format - Handshake

Performs handshakes over a bidirectional communication channel. Each handshake starts out by sending the identifier of the desired handshake protocol, the first response is also prefixed with this identifier. This allows to add new handshakes in a backwards-compatible way.

If the client initiates an unsupported handshake, the server responds with an id of 0, optionally followed by a list of handshakes it supports.

Handshake ids are encoded as [VarU64](https://github.com/AljoschaMeyer/varu64-rs)s. The 0 response is followed by another VarU64 signifying the total length of the list of supported handshake ids, followed that many bytes of ids. If the server does not whish to inform the client of the supported handshakes, it can simply send a list of length zero (the complete failure message thus consists of two zero bytes).

## Supported Handshakes

### 0: Failure
A handshake that never succeedes. The id 0 is followed by a VarU64, followed by that many bytes of arbitrary data.

### 1: Plaintext
A handshake that always succeeds (if the server supports it). The id 1 is followed by a VarU64, followed by that many bytes of arbitrary data. The response consists of the the id (1), also followed by a VarU64 and that many bytes of arbitrary data. The connection can then be used, without applying any sort of encryption.

## Planned Handshakes

- shs2-bs2: [secret-handshake](https://github.com/auditdrivencrypto/secret-handshake) version 2 (doesn't exist yet) , setting up a connection encrypted via [box-stream](https://github.com/dominictarr/pull-box-stream) version 2 (also doesn't exist yet)
- auth-hs: Set up a stream where data is not encrypted, but authenticated
