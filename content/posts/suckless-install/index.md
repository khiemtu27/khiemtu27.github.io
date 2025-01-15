---
title: Cài và chạy thử phần mềm suckless
tags:
- tech
date: 2024-06-12
slug: suckless-install
---
Đối với các phần mềm suckless, sau khi đã áp các patch thành công vào mã nguồn thì việc tiếp theo là cài đặt lên máy.

## Chuẩn bị

Bước này yêu cầu máy của bạn phải có `make`, một phần mềm dùng để bảo máy tính thực hiện lần lượt các bước trong `Makefile` -- một file kịch bản.
Để cài đặt `make` một cách đơn giản, hãy cài đặt các gói như sau:

```bash
# Debian/Ubuntu/PopOS:
sudo apt install build-essential

# Arch Linux:
sudo pacman -Sy base-devel

# Void Linux
sudo xbps-install -S base-devel
```

## Chạy thử

Trước hết hãy chắc chắn rằng bạn đang trong thư mục chứa mã nguồn (thư mục chứa các files như config.def.h, Makefile, config.mk,...). Sau đó chạy lệnh sau:

```bash
$ make
```

Lệnh này sẽ làm mọi thứ cần thiết để có một file binary bạn có thể chạy và dùng, chỉ có điều nó sẽ đặt mọi thứ vào folder chứa mã nguồn thay vì trên các folder thuộc hệ điều hành (`/usr/local/`). Tại sao không cài thẳng lên máy luôn mà phải qua bước này?

> **Ghi chú**
>
> Ngay lúc này đây, trong thư mục mã nguồn của bạn đã có file binary của các phần mềm, ví dụ như đang làm việc với dmenu bạn có thể dùng lệnh `./dmenu` để chạy dmenu mà không cần làm gì thêm.

Việc cài đặt lên máy yêu cầu bạn phải có quyền root, nghĩa là phải dùng lệnh `sudo`. Khi chạy lệnh `make`, máy tính sẽ copy file config.def.h thành config.h và sử dụng các cài đặt trong config.h để áp dụng cho phần mềm của bạn. Việc chạy `make` bằng `sudo` sẽ khiến file config.h thuộc quyền sở hữu của root, điều này là rất bất tiện khi muốn chỉnh sửa file này về sau.

> **config.def.h**
>
> Đây là file cài đặt gốc _(original configuration file)_, có thể xem như file cài đặt mẫu. Mọi tinh chỉnh của bạn không nên xảy ra trong file này, vì khi có bản cập nhật mới trong mã nguồn, config.def.h sẽ thay đổi gây ra mâu thuẫn với các cài đặt của bạn.
>
> Thay vào đó, hãy làm mọi thứ bạn muốn trong config.h, khi có cập nhật trong config.def.h hãy tự cập nhật config.h theo file mẫu. Đây là một thói sử dụng phần mềm Suckless gần đây mà tôi học được, tin tôi đi, nó sẽ giúp bạn tránh được nhiều phiền phức đấy!

Do đó tôi sẽ chạy lệnh `make` trước hết, để đảm bảo file config.h thuộc quyền sở hữu của tôi, sau này chỉnh sửa sẽ không cần rườm rà `sudoedit` hay `sudo vim`...

## Cài chính thức

Sau khi đã chạy lệnh `make` để chạy thử và chuẩn bị, hãy vào config.h để tinh chỉnh mọi thứ bạn muốn:

* Phím tắt
* Giao diện
* Tính năng
* ...

Một khi đã xong xuôi, để chính thức cài đặt phần mềm lên máy, hãy chạy lệnh sau:

```bash
$ sudo make clean install
```

Để hiểu rõ lần lượt từng thao tác máy tính của bạn sẽ làm trong lệnh này, hãy mở file `Makefile` lên và đọc qua nhé! Tóm lại, lệnh này sẽ làm các bước sau:

* Biên dịch các file mã nguồn (source code) thành file nhị phân (binary)
* Tạo các thư mục cần thiết
* Sao chép các files vào đúng nơi của chúng trong `/usr/local/`
  * File binary sẽ nằm trong `/usr/local/bin/`
  * File manual (hướng dẫn) sẽ nằm trong `/usr/local/share/man/`

Chính vì đã được copy vào `/usr/local/bin/`, một trong những folder thuộc `$PATH`, sau khi chạy lệnh này, bạn sẽ có thể chạy phần mềm của mình bất cứ đâu bạn muốn.
