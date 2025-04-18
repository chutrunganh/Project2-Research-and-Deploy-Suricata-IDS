# Các lệnh cơ bản với Cisco Switch, Router

## Các chế độ hoạt động của Cisco Router/Switch


Trong Cisco Router/Switch, "mode" nghĩa là chế độ hoạt động của thiết bị, xác định bạn có thể làm gì và truy cập gì. Các chế độ (mode) chính trong Cisco

| STT | Tên chế độ                 | Lệnh truy cập                   | Dấu nhắc (Prompt)      | Mục đích sử dụng |
|-----|----------------------------|----------------------------------|-------------------------|------------------|
| 1   | **User EXEC Mode**         | [Đăng nhập mặc định]            | `>`                     | Chạy lệnh xem thông tin cơ bản (`ping`, `show version`) |
| 2   | **Privileged EXEC Mode**   | `enable`                        | `#`                     | Truy cập lệnh nâng cao, sao lưu, cấu hình |
| 3   | **Global Configuration**   | `configure terminal`            | `(config)#`             | Cấu hình toàn bộ hệ thống |
| 4   | **Interface Configuration**| `interface <tên_interface>`     | `(config-if)#`          | Cấu hình từng cổng mạng (IP, shutdown, v.v.) |
| 5   | **Line Configuration**     | `line vty 0 4` / `line console 0` | `(config-line)#`     | Cấu hình truy cập từ xa (SSH/Telnet) hoặc local (console) |
| 6   | **Router Configuration**   | `router <protocol>`             | `(config-router)#`      | Cấu hình định tuyến (OSPF, RIP...) |

---

 Mẹo học nhanh
- `>` là bạn chỉ có quyền xem (user)
- `#` là bạn có quyền cao (admin/root)
- `(config-xxx)#` là bạn đang cấu hình thành phần cụ thể



## 🔄 Sơ đồ luồng chuyển đổi giữa các chế độ

```text
User EXEC (>)
   |
   |-- enable
   v
Privileged EXEC (#)
   |
   |-- configure terminal
   v
Global Configuration (config)#
   |
   |-- interface g0/0
   |       v
   |     Interface Mode (config-if)#
   |
   |-- line vty 0 4
   |       v
   |     Line Mode (config-line)#
   |
   |-- router ospf 1
           v
       Router Mode (config-router)#
```

Command thực tế: 

```shell
----User-----
Router> exit    # thoát khỏi Router
Router> enable   # chuyển sang Privileged EXEC mode  
-----Privileged-----
Router# exit     # thoát khỏi Router
Router# disable  chuyển về User EXEC mode
Router# configure terminal # chuyển sang Global Config mode
----Global Config-----
Router(config)# end/exit #chuyển về Privileged EXEC mode
Router(config)# line vty 0 15  # chuyển sang Line configuration mode
Router(config)# interface g0/0/0  # chuyển sang Interface configuration mode
```

## Một vài command hay dùng ở các chế độ

Các command dùng được ở bất cứ chế độ nào:

- `?` : hiển thị danh sách các lệnh có thể sử dụng tại chế độ hiện tại, bấm phím `space` để xem tiếp trang. Ngoài ra có thể dùng để xem gợi ý lệnh, ví dụ `config ?` sẽ hiển thị các lệnh có thể sử dụng sau `config`.


Extra helpful commands:

```shell
Router(conf)# no ip domain-lookup       # prevents miss-typed commands from being "translated..." 
Router(conf-line)# logging synchronous  # prevents logging output from interrupting your command input
