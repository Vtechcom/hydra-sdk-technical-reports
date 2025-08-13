# HydraWallet SDK – Tổng quan kiến trúc

## 1. Giới thiệu
HydraWallet SDK được thiết kế như một bộ công cụ mô-đun, hiệu năng cao để tương tác với blockchain Cardano, tối ưu cho cả môi trường trình duyệt và server.  
SDK cung cấp API, tiện ích, và các thành phần chạy trên WASM để đảm bảo bảo mật và hiệu suất, đồng thời loại bỏ sự phụ thuộc nặng nề vào polyfill của NodeJS.

---

## 2. Mục tiêu thiết kế
- **Tương thích đa môi trường** – Hoạt động tốt trên browser, Node.js và serverless.
- **WASM First** – Toàn bộ logic mã hóa, giải mã và xử lý giao dịch chạy trên WebAssembly.
- **Nhẹ và mô-đun** – Chỉ import phần cần thiết, giảm kích thước bundle.
- **Thân thiện với lập trình viên** – API rõ ràng, có hỗ trợ TypeScript typings.
- **Tương lai bền vững** – Không phụ thuộc vào các package yêu cầu polyfill NodeJS (ví dụ `@cardano-sdk`).

---

## 3. Kiến trúc tổng quan

### 3.1 Các tầng
1. **Core Layer (WASM)**
   - Xử lý serialize/deserialize giao dịch Cardano.
   - Cung cấp primitive mã hóa/giải mã bằng `cardano-serialization-lib` hoặc binding tối ưu.
   - Không phụ thuộc vào Node.js.

2. **API Layer**
   - Trừu tượng hóa các thao tác blockchain (lấy UTXO, submit giao dịch).
   - Hỗ trợ nhiều backend (Ogmios, Blockfrost, API tùy chỉnh).
   - Cho phép cấu hình linh hoạt qua dependency injection.

3. **Utility Layer**
   - Xử lý địa chỉ, build transaction, mã hóa/giải mã CBOR.
   - Hoạt động tốt trên cả browser và Node.

4. **Integration Layer**
   - Tích hợp sẵn với các ví như Nami, Eternl, Gero, Lace.
   - Giúp dApp developers dễ dàng khởi tạo kết nối.

---

## 4. Công nghệ sử dụng
- **Ngôn ngữ**: TypeScript
- **Core Runtime**: WebAssembly (`wasm-bindgen`, `cardano-serialization-lib`)
- **Công cụ build**: Vite (dev) / Rollup (bundle)
- **Testing**: Vitest
- **Linting**: `@antfu/eslint-config`
- **Type Checking**: `tsconfig` (strict mode)

---

## 5. Mục tiêu triển khai
- **Browser** – Build ESM tối ưu cho trình duyệt hiện đại.
- **Node.js** – Build CJS cho môi trường server.
- **WASM Standalone** – Có thể embed vào Rust, Go hoặc thiết bị nhúng.

---

## 6. Ưu điểm so với @cardano-sdk
- Không phụ thuộc module core của Node.js → không cần polyfill trong build Vite/Rollup.
- Tốc độ xử lý nhanh hơn nhờ tối ưu WASM.
- Bundle nhỏ hơn, tree-shaking hiệu quả hơn.
- Dễ dàng tích hợp với các framework frontend hiện đại.

---

## 7. Kế hoạch phát triển
- Hỗ trợ tương tác smart contract (Plutus, Aiken binding).
- Hỗ trợ multi-chain (sidechain Cardano, Hydra).
- Tích hợp sẵn transaction simulation và fee estimation.

---

**Xem thêm**:  
- [Polyfill and bundling issues with @cardano-sdk and MeshJS (EN)](./Polyfill_and_bundling_issues_with_@cardano-sdk_and_MeshJS.md)  
- [Polyfill and bundling issues with @cardano-sdk and MeshJS (VI)](./Polyfill_and_bundling_issues_with_@cardano-sdk_and_MeshJS.vi.md)
