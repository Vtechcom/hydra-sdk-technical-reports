# ESM/CJS Compatibility Issues in Browser Environments

## Introduction
The transition from CommonJS (CJS) to ECMAScript Modules (ESM) has improved the JavaScript ecosystem's compatibility with modern tooling and tree-shaking. However, in browser environments, mixing ESM and CJS modules can introduce significant challenges, especially for packages designed for Node.js.

This report analyzes the ESM/CJS compatibility issues encountered while working with packages in the Hydra SDK.

## Background
The Hydra SDK is built primarily for web environments (Nuxt 3, Vue 3, TypeScript). It depends on multiple Cardano-related packages, some of which are written for Node.js and rely on CommonJS, Node polyfills, or require-based imports.

While tools like Vite or Webpack 5 offer partial solutions via polyfills or build-time transformations, these are often insufficient when dealing with cryptographic libraries and low-level Cardano SDK dependencies.

## Key Issues
1. **Dual module format confusion**  
   Some packages ship both ESM and CJS builds but have misconfigured `package.json` fields (`main`, `module`, `exports`), leading bundlers to pick the wrong version.

2. **Node.js built-in module dependencies**  
   Many Cardano SDK-related libraries import built-ins such as `crypto`, `stream`, `fs`, `path`, which require polyfills in browsers. These polyfills can be incomplete or fail under certain bundlers.

3. **Transitive dependency pitfalls**  
   Even if a top-level package is ESM-compatible, its dependencies may import CJS modules that trigger compatibility issues.

4. **Mixed import/export syntax**  
   Modules mixing `require()` and `import` can break under strict ESM environments like Vite with `type: "module"`.

## Case Study: Packages depending on @cardano-sdk/crypto and @cardano-sdk/core
The Hydra SDK integrates several packages that indirectly depend on `@cardano-sdk/crypto` and `@cardano-sdk/core`. Examples include:

- `libcardano-wallet`
- `test-abc-sdk@apexfusionfoundation/vector-blaze-core`
- `@apexfusionfoundation/vector-sdk-core`
- `@meshsdk/core-cst`
- `@nufi/cardano-metamask-snap`
- `@blaze-cardano/corelibcardano`
- `@okxweb3/coin-cardano`
- `@cardano-sdk/hardware-trezor`
- `@cardano-sdk/hardware-ledger`
- `@tatumio/cardano`
- `@cardano-sdk/projection-typeorm`
- `@cardano-sdk/tx-construction`
- `@cardano-sdk/projection`
- `@cardano-sdk/dapp-connector`
- `@cardano-sdk/governance`
- `@cardano-sdk/key-management`
- `@cardano-sdk/e2e`
- `@cardano-sdk/ogmios`
- `@cardano-sdk/web-extension`
- `@cardano-sdk/cardano-services-client`
- `@cardano-sdk/cardano-services`
- `@cardano-sdk/util-dev`
- `@cardano-sdk/wallet`
- `@cardano-sdk/core`

These dependencies often:
- Use Node.js built-ins without browser fallbacks.
- Bundle CJS-only code.
- Have deep transitive dependencies that assume a Node.js environment.

## Workarounds
1. **Vite polyfill plugin**  
   Use `vite-plugin-node-polyfills` or similar to emulate Node built-ins.
   
2. **Force ESM resolution**  
   Configure `resolve.alias` in Vite to point to ESM builds when available.

3. **Custom shims**  
   Manually mock modules like `fs` in browser contexts.

4. **Pre-bundling**  
   Use `optimizeDeps` in Vite or a pre-build step to convert CJS to ESM.

## Recommendations
- Prefer packages that publish true ESM builds.
- Audit transitive dependencies for Node.js-specific imports.
- Advocate upstream maintainers to improve browser support.

