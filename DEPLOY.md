# Hướng dẫn Deploy lên Streamlit Cloud

## Chuẩn bị trước khi deploy

### 1. Đảm bảo các file quan trọng đã được .gitignore

Các file sau sẽ KHÔNG được commit lên GitHub:
- `config.py` - File chứa API key (nếu có)
- `*.xlsx`, `*.xls` - File Excel sẽ được tạo tự động khi chạy ứng dụng
- `.streamlit/secrets.toml` - File secrets local (không cần thiết)

### 2. Cấu trúc project

Đảm bảo project có cấu trúc như sau:
```
.
├── app.py
├── pages/
│   ├── Hoa_don_ban_ra.py
│   ├── Hoa_don_mua_vao.py
│   ├── Ket_qua_kinh_doanh.py
│   ├── Lay_thong_tin_CCCD.py
│   └── Tao_moi_HDLD_CN.py
├── requirements.txt
├── README.md
├── HDLD_Mau.txt
└── .gitignore
```

## Bước 1: Đẩy code lên GitHub

1. Khởi tạo git repository (nếu chưa có):
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

2. Tạo repository mới trên GitHub

3. Đẩy code lên GitHub:
   ```bash
   git remote add origin <your-github-repo-url>
   git branch -M main
   git push -u origin main
   ```

## Bước 2: Deploy lên Streamlit Cloud

1. Truy cập: https://share.streamlit.io/
2. Đăng nhập bằng tài khoản GitHub
3. Click "New app"
4. Chọn:
   - **Repository**: Repository GitHub của bạn
   - **Branch**: `main` (hoặc branch chính của bạn)
   - **Main file path**: `app.py`
5. Click "Deploy"

## Bước 3: Cấu hình API Key trong Streamlit Secrets

### Cách 1: Qua Streamlit Cloud Dashboard (Khuyến nghị)

1. Sau khi deploy, vào trang quản lý app trên Streamlit Cloud
2. Click vào menu (3 chấm) → "Settings" → "Secrets"
3. Thêm secret sau:

```toml
OPENAI_API_KEY = "sk-proj-your-api-key-here"
```

4. Lưu và app sẽ tự động restart

### Cách 2: Tạo file `.streamlit/secrets.toml` (chỉ cho local)

⚠️ **Lưu ý:** File này đã được thêm vào `.gitignore`, sẽ không được commit.

Tạo file `.streamlit/secrets.toml` trong project root:
```toml
OPENAI_API_KEY = "sk-proj-your-api-key-here"
```

## Kiểm tra sau khi deploy

### ✅ Kiểm tra API Key

1. Mở app trên Streamlit Cloud
2. Vào bất kỳ trang nào có sử dụng OpenAI (ví dụ: "Lấy thông tin CCCD")
3. Mở phần "Cấu hình nâng cao (OpenAI API)"
4. API key sẽ tự động được lấy từ Streamlit Secrets (không cần nhập thủ công)

### ✅ Kiểm tra File Excel

- Ứng dụng sẽ tự động tạo file Excel khi cần
- File Excel sẽ được lưu trong filesystem của Streamlit Cloud (tạm thời)
- **Lưu ý:** File Excel trên Streamlit Cloud sẽ bị xóa khi app restart hoặc sau một thời gian không dùng

## Xử lý sự cố

### Lỗi "API key not found"

- Kiểm tra đã thêm `OPENAI_API_KEY` vào Streamlit Secrets chưa
- Đảm bảo format đúng: `OPENAI_API_KEY = "sk-proj-..."` (có dấu ngoặc kép)
- Restart app sau khi thêm secret

### Lỗi "File Excel not found"

- Đây là bình thường khi lần đầu chạy
- File Excel sẽ được tạo tự động khi bạn lưu dữ liệu đầu tiên
- Code đã xử lý trường hợp này tự động

### Lỗi import module

- Kiểm tra `requirements.txt` đã có đầy đủ dependencies chưa
- Streamlit Cloud sẽ tự động cài đặt từ `requirements.txt`

## Lưu ý quan trọng

1. **KHÔNG commit `config.py` có API key** - File này đã được thêm vào `.gitignore`
2. **Sử dụng Streamlit Secrets** - Cách an toàn nhất để lưu API key trên cloud
3. **File Excel trên cloud** - Chỉ tồn tại tạm thời, nên export/backup định kỳ nếu cần
4. **Restart app** - Luôn restart app sau khi thay đổi secrets

## Cấu trúc secrets.toml trên Streamlit Cloud

```toml
# File này chỉ tồn tại trên Streamlit Cloud, không được commit vào git
OPENAI_API_KEY = "sk-proj-xxxxx..."
```

---

**Tài liệu tham khảo:**
- Streamlit Secrets: https://docs.streamlit.io/streamlit-community-cloud/deploy-your-app/secrets-management
- Deploy guide: https://docs.streamlit.io/streamlit-community-cloud/deploy-your-app
