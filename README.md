# Sử dụng tool:

## Chạy file update:

> tmux a -t update

> python update.py

## Chạy file crawl all:

> tmux a -t all

> python all.py

## Trong session tmux:

**Khi muốn dừng tool**: ấn tổ hợp phím: Ctrl+C

**Khi muốn ẩn session tmux hiện tại (để thực hiện thao tác khác hoặc vào tmux session khác)**: ấn tổ hợp phím: Ctrl+B D (Bấm và giữ Ctrl sau đó ấn B, sau đó nhả 2 phím và ấn D)

# Cài đặt cần lưu ý ở file settings.py (trong trường hợp cần sửa lại hoặc chạy tool ở vps khác):

- **user, password, host, port, database**: Kết nối tới database
- **TABLE_PREFIX:** Bắt đầu tên table trong database (Hiện tại là wp\_)

- **UPLOAD_FOLDER:** Folder uploads của theme

- **IMAGE_SAVE_PATH:** Bỏ qua
- **THUMB_SAVE_PATH:** Folder lưu ảnh covers

- **TELEGRAM_BOT_TOKEN:** Token của bot telegram (tạo trong chat với BotFather). Điền vào giữa dấu ""
- **TELEGRAM_CHAT_ID:** Telegram ID của user hoặc group nhận thông báo khi domain trumtruyen có thể die. Điền vào giữa dấu "" (Yêu cầu /start với bot trước khi nhận thông báo)

- **WAIT_BETWEEN_LATEST:** Thời gian đợi giữa 2 lần cào page 1 trumtruyen để update: 5 \* 60 là 5 phút
- **WAIT_BETWEEN_ALL**: Thời gian đợi giữa 2 lần cào từ page 2 tới page cuối của trumtruyen để cào các truyện còn lại: 1 \* 20 là 20 giây

- **TRUMTRUYEN_UPDATE_PAGE**: Domain của trumtruyen (Đổi trong trường hợp trumtruyen đổi)

# Trong trường hợp restart VPS cần chạy các lệnh sau sau khi ssh vào VPS

> cd leechtruyen

> tmux new -s update

> source venv/bin/activate

> Bấm và giữ Ctrl sau đó ấn B, sau đó nhả 2 phím và ấn D (dùng để ẩn session tmux - vẫn chạy bình thường)

> tmux new -s all

> source venv/bin/activate

> Bấm và giữ Ctrl sau đó ấn B, sau đó nhả 2 phím và ấn D

Sau đó sử dụng theo hướng dẫn trên

# Sync hình ảnh giữa VPS chạy crawl với hosting

## Tạo key ssh lần đầu trên vps chạy crawl (chỉ cần 1 lần đầu)

> ssh-keygen -t ed25519

> Enter tới hết là được (dể default toàn bộ)

## Copy ssh key của vps crawl lên hosting (chỉ cần 1 lần đầu)

> ssh-copy-id user@ip

> Nhập mật khẩu ssh

- user: ssh user trên hosting (có quyền truy cập vào folder lưu ảnh - uploads của web) - ví dụ trường hợp hosting hiện tại thì user là `root`
- ip: ip của hosting - ví dụ trường hợp hosting hiện tại thì ip là `123.168.251.199`

## Thêm cronjob (trên vps crawl) để copy ảnh covers lên hosting 30 giây 1 lần

> crontab -e

> Thêm 2 dòng sau

* * * * * scp -r [đường dẫn folder lưu ảnh covers trên vps crawl] user@ip:[đường dẫn folder uploads trên hosting]
* * * * * (sleep 30; scp -r [đường dẫn folder lưu ảnh covers trên vps crawl] user@ip:[đường dẫn folder uploads trên hosting])

Ví dụ trường hợp hosting hiện tại
```
* * * * * scp -r /www/wwwroot/nuitruyen.vn/wp-content/uploads/covers root@123.168.251.199:/www/wwwroot/nuitruyen.vn/wp-content/uploads
* * * * * (sleep 30; scp -r /www/wwwroot/nuitruyen.vn/wp-content/uploads/covers root@123.168.251.199:/www/wwwroot/nuitruyen.vn/wp-content/uploads)
```

> Lưu và thoát

# Cài đặt python và thư việ để crawl
**Cài đặt python mới nhất.**
> sudo apt update

> sudo apt install python3 python3-venv python3-pip


**Chạy lệnh cài đặt thư viện**
> pip install -r requirements.txt
