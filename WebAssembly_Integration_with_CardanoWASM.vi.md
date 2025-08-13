# Tích Hợp WebAssembly với CardanoWASM

## Tổng Quan
WebAssembly (WASM) cung cấp một cách thức di động và hiệu quả để chạy mã đã biên dịch trong nhiều môi trường khác nhau, bao gồm trình duyệt web và Node.js. CardanoWASM là bản build WebAssembly của Cardano Serialization Library, cho phép lập trình viên tương tác với dữ liệu blockchain Cardano, giao dịch và các tác vụ mật mã trực tiếp từ môi trường JavaScript hoặc TypeScript.

## Lợi Ích Chính
- **Đa Nền Tảng**: Chạy trên trình duyệt, Node.js và các môi trường serverless.
- **Hiệu Suất Cao**: Tốc độ gần như native cho các thao tác mật mã.
- **Tương Tác Tốt**: Dễ dàng tích hợp với ứng dụng JavaScript frontend và backend.

## Các Trường Hợp Sử Dụng
1. Xây dựng ví Cardano gọn nhẹ trên trình duyệt.
2. Xác minh giao dịch phía client mà không lộ khóa riêng.
3. Tích hợp chức năng Cardano vào ứng dụng phi tập trung (dApp).
4. Thực hiện xác thực mật mã trong các hàm serverless.

## Ví Dụ Tích Hợp
```javascript
import init, { Address } from '@emurgo/cardano-serialization-lib-browser';

async function loadWasm() {
    await init();
    const address = Address.from_bech32("addr1...");
    console.log(address.to_bytes());
}

loadWasm();
```

## Thực Hành Tốt
- Sử dụng bản build dành riêng cho trình duyệt (`-browser`) để giảm kích thước bundle.
- Tránh chặn luồng UI bằng cách chạy các tác vụ nặng trong Web Workers.
- Luôn xác minh việc khởi tạo WASM trước khi gọi các phương thức.
