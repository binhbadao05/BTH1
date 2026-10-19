# Bài Thực Hành 1: Gửi và Nhận Message Cơ Bản Qua Queue trong AMQP

> **Sinh viên thực hiện**: Nguyễn Phú Bình | **MSV**: B23DCCN083 | **Lớp**: D23CNPM06

## 1. Mục Tiêu
- Làm quen với mô hình cơ bản của giao thức AMQP: **Producer → Queue → Consumer**.
- Kết nối ứng dụng Python với RabbitMQ broker thông qua thư viện `pika`.
- Thực hiện gửi thông tin sinh viên từ Producer và hiển thị đúng định dạng trên Consumer kèm mốc thời gian nhận.

---

## 2. Cấu Trúc Thư Mục
```text
BTH1/
├── producer_bai1.py    # Chương trình gửi message (Producer)
├── consumer_bai1.py    # Chương trình nhận và hiển thị message (Consumer)
└── README.md           # Hướng dẫn chi tiết Bài 1
```

---

## 3. Cấu Hình Broker
- **Broker**: RabbitMQ
- **Host**: `localhost` (hoặc IP broker dùng chung trên lớp / CloudAMQP)
- **Port**: `5672` (hoặc `5671` đối với SSL/TLS)
- **Tài khoản mặc định**: `guest` / `guest`
- **Queue sử dụng**: `iot_lab_queue`

---

## 4. Hướng Dẫn Cài Đặt & Chạy Chương Trình

### Bước 1: Cài đặt thư viện phụ thuộc
Đảm bảo đã cài đặt thư viện `pika`:
```bash
pip install pika
```

### Bước 2: Chạy Consumer trước để đón tin nhắn
Mở một cửa sổ dòng lệnh (Terminal 1) và chạy:
```bash
python consumer_bai1.py
```
> Consumer sẽ kết nối tới RabbitMQ và bắt đầu lắng nghe tại queue `iot_lab_queue`.

### Bước 3: Chạy Producer để gửi tin nhắn
Mở một cửa sổ dòng lệnh khác (Terminal 2) và chạy:
```bash
python producer_bai1.py --name "Nguyen Phu Binh" --id "B23DCCN083"
```
*(Nếu chạy `python producer_bai1.py` không tham số, chương trình cũng đã cài đặt mặc định sẵn thông tin sinh viên **Nguyễn Phú Bình - B23DCCN083**)*.

#### Các tùy chọn mở rộng khi chạy Producer:
- Gửi nhiều message liên tiếp (ví dụ 5 messages, cách nhau 1 giây):
  ```bash
  python producer_bai1.py --name "Nguyen Phu Binh" --id "B23DCCN083" --count 5 --interval 1.0
  ```
- Kết nối tới broker từ xa (IP phòng lab hoặc CloudAMQP):
  ```bash
  python producer_bai1.py --host 192.168.1.100 --user admin --password secret
  # hoặc dùng URL:
  python producer_bai1.py --url "amqp://user:password@hostname:5672/"
  ```

---

## 5. Kết Quả Đạt Được

### Tại màn hình Producer (Gửi):
```text
============================================================
      BTH1 - AMQP PRODUCER (GỬI MESSAGE VÀO QUEUE)
============================================================
[*] Đang kết nối tới broker: localhost:5672...
[+] Đã kết nối thành công. Queue: 'iot_lab_queue'
[x] Đã gửi [1/1]: "Xin chao tu ung dung Python AMQP - B23DCCN083 - Nguyen Phu Binh"
[*] Đã đóng kết nối RabbitMQ an toàn.
```

### Tại màn hình Consumer (Nhận):
```text
============================================================
      BTH1 - AMQP CONSUMER (LẮNG NGHE QUEUE)
============================================================
[*] Đang kết nối tới broker: localhost:5672...
[*] Đang lắng nghe trên queue 'iot_lab_queue'. Nhấn Ctrl+C để thoát.
------------------------------------------------------------
Da nhan message: Xin chao tu ung dung Python AMQP - B23DCCN083 - Nguyen Phu Binh
Thoi gian nhan: 14:05:21
------------------------------------------------------------
```
