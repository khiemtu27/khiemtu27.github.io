---
title: Cài đặt phần mềm suckless không cần root
tags:
- tech
date: 2024-06-12
slug: suckless-no-root
---
> **Cảnh báo**
>
> Đây là một bước tôi thường làm nhưng gần đây khi hiểu hơn về nguyên lý hoạt động của các phần mềm Suckless, tôi không còn làm điều này nữa. Tôi khuyến cáo bạn chỉ đọc tham khảo cho vui thôi nhé!

Mặc định khi cài đặt các files sẽ được chuyển đến `/usr/local`, điều này đòi hỏi ta phải thực hiện các thao tác bằng người dùng `root`. Để né tránh việc sử dụng `root`, ta có thể chuyển thư mục cài đặt thành `$HOME/.local`.

> **Lưu ý**
>
> Để đảm bảo bước này thành công, bạn phải chắc chắn rằng trong `$PATH` của mình có địa chỉ `$HOME/.local/bin`. Nếu không sau khi cài đặt st sẽ không xuất hiện cho chúng ta khởi chạy. Để kiểm tra, hãy thử nhập lệnh này:

```bash
echo $PATH
```

Nếu trong chuỗi kết quả không có đoạn `/home/<tên người dùng>/.local/bin` thì hãy thêm dòng sau vào `~/.bash_profile` (nếu bạn dùng shell khác thì chắc đã không cần tôi giải thích bước này).

```bash
export PATH=$HOME/.local/bin:$PATH
```

Một lưu ý nhỏ là việc thay đổi này sẽ khiến chỉ có người dùng bạn dùng để compile các phần mềm mới có thể dùng chúng, những người dùng khác trên hệ thống nếu muốn dùng sẽ phải tự compile một bản cho riêng mình.

Trong file config.mk có một dòng như sau:

```makefile
PREFIX = /usr/local
```

Để thay đổi vị trí cài vào `$HOME/.local`, bạn có thể dùng lệnh:

```bash
$ sed -i 's/^PREFIX =.*$/PREFIX = \/home\/$(shell whoami)\/.local/' config.mk
```

Lệnh `sed` này sẽ tìm trong file config.mk dòng bắt đầu bằng `PREFIX =` và thay cả dòng đó thành `PREFIX = /home/$(shell whoami)/.local`. Khi bạn chạy lệnh `make install`. Biến `PREFIX` sẽ được khởi tạo, bên trong biến này có cụm `$(shell whoami)` sẽ dùng kết quả của lệnh `whoami` lấp vào chỗ đó.

> **Ghi chú**
>
> `whoami` là lệnh để tra cứu tên người dùng hiện tại, bạn có thể nhập vào terminal của mình lệnh này để thử.

Nghĩa là biến `PREFIX` lúc này sẽ là `/home/<tên người dùng>/.local`. Thế là xong, sau này mỗi lần cài đặt không cần phải dùng `root` nữa.
