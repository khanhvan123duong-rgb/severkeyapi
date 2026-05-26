# Key Server - Văn Khánh VIP

Server quản lý key & thiết bị, deploy trên Render.com.

## Deploy lên Render

1. Push repo này lên GitHub
2. Vào [render.com](https://render.com) → New → Web Service → kết nối repo
3. Render tự đọc file `render.yaml` và cấu hình tự động
4. Deploy xong sẽ có link dạng: `https://key-server.onrender.com`

> **Lưu ý:** Trên Render Free tier, dữ liệu key (`database_keys.json`) giữ nguyên giữa các lần **restart** nhưng sẽ bị reset khi bạn **deploy lại** (push code mới). Để giữ dữ liệu vĩnh viễn, nâng cấp lên Render Starter hoặc dùng plan có Persistent Disk.

## Đường link check key/device (tích hợp vào app/tool)

```
GET/POST  https://your-server.onrender.com/api/check_key
```

**Tham số:**
| Tham số | Bắt buộc | Mô tả |
|---------|----------|-------|
| `key` | ✅ | Mã key cần kiểm tra |
| `device_id` | ✅ | ID thiết bị (Machine ID) |

**Ví dụ GET:**
```
https://your-server.onrender.com/api/check_key?key=VIP-ABC-123&device_id=DEVICE-XYZ-456
```

**Ví dụ POST (form-data hoặc x-www-form-urlencoded):**
```
POST https://your-server.onrender.com/api/check_key
key=VIP-ABC-123&device_id=DEVICE-XYZ-456
```

**Kết quả trả về (JSON):**
```json
{ "status": "success", "message": "Key hop le", "time_left": "7 ngày" }
{ "status": "success", "message": "Them thiet bi thanh cong", "time_left": "7 ngày" }
{ "status": "expired", "message": "Key tren thiet bi nay da het han!" }
{ "status": "device_limit", "message": "Qua so thiet bi cho phep (1)" }
{ "status": "invalid", "message": "Key khong ton tai" }
{ "status": "error", "message": "Thieu tham so" }
```

> ✅ API hỗ trợ **CORS** — ứng dụng bên ngoài có thể gọi trực tiếp mà không bị chặn.

## Trang quản trị Admin

- **Trang chủ / Check key:** `/`
- **Admin panel:** Đăng nhập từ trang chủ, tài khoản mặc định: `vkhanh` / `1`
- **URL API** hiển thị ngay trong trang chủ Admin — copy 1 click để dán vào app
- **Nhận key free:** `/nhan-key-free`
- **Check IP key:** `/check-ip-key`
- **Đăng ký thiết bị:** `/dang-ky-thiet-bi`

## Stack

- Python 3.11 + Flask 3.1.1 + Flask-CORS
- Gunicorn (production server)
- Database: JSON file (`database_keys.json`) — tự động tạo, tự động lưu

## Nhạc nền

File `nhac.mp3`, `nhac2.mp3`, `nhac3.mp3` đã có sẵn trong repo.
