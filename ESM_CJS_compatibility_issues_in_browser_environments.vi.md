# Các vấn đề tương thích ESM/CJS trong môi trường trình duyệt

## Giới thiệu
Việc chuyển đổi từ CommonJS (CJS) sang ECMAScript Modules (ESM) đã giúp hệ sinh thái JavaScript tương thích tốt hơn với các công cụ hiện đại và hỗ trợ tree-shaking. Tuy nhiên, trong môi trường trình duyệt, việc trộn lẫn ESM và CJS có thể gây ra nhiều vấn đề, đặc biệt với các package thiết kế cho Node.js.

Báo cáo này phân tích các vấn đề tương thích ESM/CJS gặp phải khi làm việc với các package trong HydraWallet SDK.

## Bối cảnh
HydraWallet SDK được xây dựng chủ yếu cho môi trường web (Nuxt 3, Vue 3, TypeScript). Nó phụ thuộc vào nhiều package liên quan đến Cardano, trong đó có những package được viết cho Node.js và dựa vào CommonJS, Node polyfills hoặc `require()`.

Mặc dù các công cụ như Vite hoặc Webpack 5 có thể cung cấp giải pháp polyfill hoặc chuyển đổi build-time, nhưng những giải pháp này thường không đủ khi xử lý các thư viện mật mã hoặc các thư viện Cardano SDK cấp thấp.

## Các vấn đề chính
1. **Nhầm lẫn định dạng dual module**  
   Một số package cung cấp cả build ESM và CJS nhưng cấu hình `package.json` (`main`, `module`, `exports`) sai, khiến bundler chọn sai phiên bản.

2. **Phụ thuộc vào module built-in của Node.js**  
   Nhiều thư viện Cardano SDK import các module như `crypto`, `stream`, `fs`, `path` — các module này cần polyfill khi chạy trên trình duyệt.

3. **Phụ thuộc bắc cầu**  
   Ngay cả khi package ở mức cao tương thích ESM, các dependency của nó vẫn có thể import CJS và gây lỗi.

4. **Trộn cú pháp import/export**  
   Module trộn `require()` và `import` có thể bị lỗi trong môi trường ESM nghiêm ngặt như Vite với `type: "module"`.

## Nghiên cứu tình huống: Các package phụ thuộc vào @cardano-sdk/crypto và @cardano-sdk/core
HydraWallet SDK tích hợp nhiều package phụ thuộc gián tiếp vào `@cardano-sdk/crypto` và `@cardano-sdk/core`, ví dụ:

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

Các package này thường:
- Sử dụng built-in của Node.js mà không có fallback cho trình duyệt.
- Chỉ cung cấp mã CJS.
- Có dependency bắc cầu giả định đang chạy trong môi trường Node.js.

## Giải pháp tạm thời
1. **Plugin polyfill cho Vite**  
   Dùng `vite-plugin-node-polyfills` hoặc tương tự để giả lập Node built-ins.
   
2. **Buộc bundler chọn ESM**  
   Cấu hình `resolve.alias` trong Vite để trỏ đến build ESM nếu có.

3. **Tạo shim thủ công**  
   Tự mock các module như `fs` khi chạy trên trình duyệt.

4. **Pre-bundling**  
   Dùng `optimizeDeps` trong Vite hoặc bước build trước để chuyển CJS sang ESM.

## Khuyến nghị
- Ưu tiên package có build ESM chuẩn.
- Kiểm tra dependency bắc cầu để tìm import đặc thù Node.js.
- Đóng góp cải thiện hỗ trợ trình duyệt cho các thư viện upstream.

