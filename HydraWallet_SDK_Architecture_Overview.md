# HydraWallet SDK Architecture Overview

## Introduction
The HydraWallet SDK is designed to provide a robust, modular, and scalable interface for interacting with the Cardano blockchain and Hydra Head protocol. It enables developers to integrate wallet functionalities and Hydra Head management into their applications without deep knowledge of Cardano internals.

## Core Design Principles
1. **Modularity** – Each functional domain (wallet, transaction, Hydra Head management, blockchain queries) is isolated into separate modules.
2. **Abstraction** – The SDK abstracts low-level Cardano serialization and Hydra protocol details.
3. **Cross-Platform Compatibility** – Designed for browser, Node.js, and serverless environments.
4. **WASM-first Approach** – Uses WebAssembly to avoid polyfill issues caused by Node.js-specific packages.
5. **Developer Experience** – TypeScript-first design with detailed typings and ESLint + Prettier integration.

## Architecture Layers
### 1. **Core Layer**
   - Written in Rust and compiled to WebAssembly for maximum performance and compatibility.
   - Handles:
     - Cardano transaction building and signing.
     - Cryptographic primitives.
     - Hydra protocol message serialization/deserialization.

### 2. **API Layer**
   - JavaScript/TypeScript bindings for the WASM core.
   - Provides high-level APIs for:
     - Wallet operations.
     - Hydra Head lifecycle (init, commit, close, fan-out).
     - Cardano node queries (UTxO, slot, block data).

### 3. **Integration Layer**
   - Manages:
     - Network communication (WebSocket, HTTP).
     - Persistent storage (IndexedDB, LocalStorage for browser; file-based for Node.js).
     - Logging and telemetry.

### 4. **Plugin System**
   - Allows extension with custom transaction builders, Hydra Head strategies, or storage engines.

## Key Advantages
- **No Polyfill Issues** – Avoids common Vite/Rollup bundling problems with Node.js modules.
- **Better Performance** – Rust/WASM core reduces transaction building time.
- **Future-proof** – Can integrate with other blockchains or sidechains by swapping protocol modules.

## Related Reports
- [Polyfill and Bundling Issues with @cardano-sdk and MeshJS](./Polyfill_and_bundling_issues_with_@cardano-sdk_and_MeshJS.md)
