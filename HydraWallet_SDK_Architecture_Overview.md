# HydraWallet SDK – Architecture Overview

## 1. Introduction
The HydraWallet SDK is designed as a modular, high-performance toolkit for interacting with the Cardano blockchain, optimized for both browser and server environments.  
The SDK provides developers with APIs, utilities, and WASM-based components for secure and efficient blockchain operations, removing heavy dependencies on NodeJS polyfills.

---

## 2. Design Goals
- **Cross-Environment Compatibility** – Fully functional in browsers, Node.js, and serverless environments.
- **WASM First** – Core cryptographic and serialization logic implemented in WebAssembly for performance and compatibility.
- **Lightweight & Modular** – Only import what you need, minimizing bundle size.
- **Developer Friendly** – Clear API design with TypeScript typings.
- **Future-Proof** – Avoid dependencies on packages that require NodeJS polyfills (e.g., `@cardano-sdk`).

---

## 3. Architecture Overview

### 3.1 Layers
1. **Core Layer (WASM)**
   - Handles serialization/deserialization of Cardano transactions.
   - Cryptographic primitives using `cardano-serialization-lib` or custom optimized bindings.
   - No Node.js dependencies.

2. **API Layer**
   - Abstracts blockchain interactions (e.g., querying UTXOs, submitting transactions).
   - Supports multiple backends (Ogmios, Blockfrost, custom APIs).
   - Flexible configuration via dependency injection.

3. **Utility Layer**
   - Address manipulation, transaction building, CBOR encoding/decoding.
   - Compatible with both browser and Node environments.

4. **Integration Layer**
   - Prebuilt connectors for wallets like Nami, Eternl, Gero, Lace.
   - Simplified onboarding for dApp developers.

---

## 4. Technology Stack
- **Language**: TypeScript
- **Core Runtime**: WebAssembly (`wasm-bindgen`, `cardano-serialization-lib`)
- **Build Tool**: Vite (for development) / Rollup (for library bundling)
- **Testing**: Vitest
- **Linting**: `@antfu/eslint-config`
- **Type Checking**: `tsconfig` (TypeScript strict mode)

---

## 5. Deployment Targets
- **Browser** – ESM build optimized for modern browsers.
- **Node.js** – CJS build for server-side integration.
- **WASM Standalone** – For embedding into environments like Rust, Go, or embedded systems.

---

## 6. Key Advantages Over @cardano-sdk
- No Node.js core module dependencies → no need for polyfills in Vite/Rollup builds.
- Faster execution due to WASM optimizations.
- Smaller bundle size and better tree-shaking.
- Easier integration into modern frontend frameworks.

---

## 7. Future Roadmap
- Support for smart contract interactions (Plutus, Aiken bindings).
- Multi-chain support (Cardano sidechains, Hydra).
- Built-in transaction simulation & fee estimation.

---

**See also**:  
- [Polyfill and bundling issues with @cardano-sdk and MeshJS (EN)](./Polyfill_and_bundling_issues_with_@cardano-sdk_and_MeshJS.md)  
- [Polyfill and bundling issues with @cardano-sdk and MeshJS (VI)](./Polyfill_and_bundling_issues_with_@cardano-sdk_and_MeshJS.vi.md)
