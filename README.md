# Socket Network Monorepo

This repository is a **monorepo** containing both the Rust WebSocket server and client for secure communication with RSA authentication.

## Repository Structure

```

/
├── client/   # Rust WebSocket client with RSA authentication
├── server/   # Rust WebSocket server with RSA client auth
└── README.md

````

- `client/`: Connects to a WebSocket server, authenticates via RSA private key, and broadcasts messages.
- `server/`: Accepts authenticated clients, verifies RSA signatures, and relays messages.

## Quick Start

### 1. Requirements

- [Rust (stable toolchain)](https://www.rust-lang.org/tools/install)
- [OpenSSL](https://www.openssl.org/)

### 2. Generate Keys

Both projects rely on PKCS#1-format RSA keypairs for authentication.

Generate a private key for each client:
```sh
mkdir -p keys
openssl genrsa -traditional -out keys/device123.pem 2048
````

Extract the matching public key (for the server):

```sh
openssl rsa -in keys/device123.pem -pubout -RSAPublicKey_out -out keys/device123.pub.pem
```

### 3. Project-Specific Setup

#### Server

1. Copy all allowed client public keys (`*.pub.pem`) into `server/keys/`.
2. From the monorepo root:

   ```sh
   cd server
   cargo build --release
   cargo run
   ```

   The server listens on `ws://0.0.0.0:8081`.

#### Client

1. Place the private key (`device123.pem`) into `client/keys/`.
2. From the monorepo root:

   ```sh
   cd client
   cargo build --release
   cargo run
   ```

   The client will connect to the server at `ws://localhost:8081` by default.

## Project Documentation

For full details, see the individual READMEs in each project:

* [`client/README.md`](./client/readme.md)
* [`server/README.md`](./server/readme.md)

## Development Workflow

* You can make changes to both projects in a single commit.
* Each project can be built and tested independently from its own subdirectory.
* Key generation instructions are duplicated for clarity—ensure both server and client use matching PKCS#1 key formats.
* **No submodules:** All code is directly present in the monorepo.

## Security Notes

* Communication is unencrypted unless you run the server behind a TLS reverse proxy.
* Anyone with access to the client’s private key can impersonate that client.
* Only public keys present on the server can authenticate.

## License

MIT
