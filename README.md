# Bài 3: Cấu hình tường lửa UFW và chuẩn đoán cổng mạng

## Mục tiêu
- Cấu hình tường lửa UFW chặn và mở cổng dịch vụ chính xác.
- Sử dụng các công cụ chẩn đoán mạng CLI (`netstat`, `ss`, `curl`, `nc`/`nmap`) để kiểm tra trạng thái cổng kết nối.

---

## 1. Các Bước Cấu hình UFW

```bash
# Đặt chính sách mặc định
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Mở cổng SSH và HTTP
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp

# Bật tường lửa UFW
sudo ufw enable
```

---

## 2. Kiểm tra Cấu hình UFW (`sudo ufw status verbose`)

```bash
$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
```

---

## 3. Chẩn đoán Cổng Mạng Lắng nghe (`ss -tuln`)

Chạy lệnh tra cứu socket lắng nghe trên server:
```bash
$ sudo ss -tuln
Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port  Process
tcp    LISTEN  0       511            0.0.0.0:80          0.0.0.0:*      
tcp    LISTEN  0       4096           0.0.0.0:22          0.0.0.0:*      
```

### Thử nghiệm kết nối HTTP bằng `curl -I`:
```bash
$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Wed, 07 Oct 2026 11:20:00 GMT
Content-Type: text/html
Content-Length: 612
```

---

## 4. Kết luận
- Tường lửa UFW đã bảo vệ an toàn cho máy chủ, chỉ mở đúng 2 cổng 22 (SSH) và 80 (HTTP).
- Kiểm tra bằng `ss -tuln` và `curl` xác nhận dịch vụ web đang hoạt động bình thường.
