# Technical Report: Polyfill and Bundling Issues with @cardano-sdk and MeshJS

## 1. Overview

During the development of the HydraWallet SDK, significant technical challenges were encountered when attempting to integrate the `@cardano-sdk` library within browser-based environments using modern bundlers such as **Vite** and **Rollup**.

These issues directly influenced the decision to **replace @cardano-sdk with a WebAssembly (WASM) implementation** for core cryptographic and blockchain interaction logic.

## 2. Problem Statement

When importing `@cardano-sdk` and its dependencies in a browser context, the following errors were observed:

```bash
Uncaught ReferenceError: exports is not defined
Uncaught TypeError: import_serialize_error.default is not a function
```

Additionally, related polyfill errors occurred due to the lack of Node.js core module support in browsers:

- Missing modules such as `util`, `stream`, `crypto`, etc.
- Errors when attempting to bundle with Vite/Rollup without manual polyfills.

## 3. Root Cause Analysis

### 3.1 Node.js Core Module Dependencies
`@cardano-sdk` internally relies on Node.js core modules and APIs. These are **not available in browsers** by default. Older bundlers (e.g., Webpack 4/5) sometimes inject polyfills automatically, but **Vite** and **Rollup** do not do this by default for performance and compatibility reasons.

### 3.2 Lack of ESM-Friendly Build
Some dependencies within `@cardano-sdk` are CommonJS (CJS) packages, leading to interoperability issues when used in ESM-only browser environments. For example:

- The `serialize-error` package may export as CJS but is imported as if it were an ESM default export, causing `import_serialize_error.default is not a function`.

### 3.3 Polyfill Handling in Vite and Rollup
Both Vite and Rollup require **manual configuration** to polyfill Node core modules (via `node-polyfills` or similar). Without these configurations:

- `exports is not defined` occurs because CJS-style exports are used in an ESM environment.
- `require is not defined` errors appear when Node-specific calls are made.
- Crypto-related modules fail to load.

## 4. Related Issues with MeshJS SDK

`MeshJS` is another SDK in the Cardano ecosystem that internally uses `@cardano-sdk` as its **core package**. Consequently:

- The same bundling errors occur when attempting to use MeshJS in browser builds with Vite/Rollup.
- Any project relying on MeshJS inherits the same polyfill and compatibility problems.

## 5. Impact on HydraWallet SDK Development

Due to these bundling issues:
- Browser compatibility became unreliable.
- Additional complexity was introduced by the need for manual polyfills.
- Performance was negatively impacted by large polyfill bundles.

As a result, the decision was made to **transition to a WASM-based implementation** for cryptographic and Cardano-related functions.

## 6. WASM as a Solution

By replacing `@cardano-sdk` with a WASM-based implementation:

- **No Node.js core module dependencies** are required.
- **Full browser compatibility** is achieved without polyfills.
- **Better performance** for cryptographic operations.
- Smaller and cleaner bundle output.

The WASM approach leverages `cardano-serialization-lib` compiled to WebAssembly, ensuring compatibility with modern frontend frameworks.

## 7. Conclusion

The incompatibility of `@cardano-sdk` and MeshJS with modern ESM-first browser bundlers like Vite and Rollup — due to Node.js-specific dependencies and polyfill requirements — was a key blocker for HydraWallet SDK development.

Switching to WASM provides a **future-proof, browser-friendly, and high-performance** alternative.

---

**Author:** HydraWallet SDK Development Team  
**Date:** 2025-08-13
