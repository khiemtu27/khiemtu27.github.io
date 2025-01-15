---
title: Lọc ánh sáng xanh trên Xorg
tags:
    - tech
date: 2025-01-15
slug: loc-anh-sang-xanh-tren-xorg
---
Ban đầu tôi dự định sẽ dùng [Redshift](https://github.com/jonls/redshift) nhưng phần mềm này có nhiều tính năng tôi không dùng đến.
Sau một hồi tìm kiếm, tôi thấy một chiếc tool cực kỳ nhỏ gọn được viết với ngôn ngữ C mang tên [sct](https://github.com/faf0/sct)

## Cài đặt

> **Ghi chú**
>
> Lạ cái là repo tên `sct` nhưng file thực thi lại lên `xsct`, khá khó hiểu.


Để compile tool này bạn cần chạy lệnh cài các gói sau (nếu dùng Void Linux):

```bash
sudo xbps-install libXinerama-devel base-devel gcc
```

Sau đó clone repo này về:

```bash
git clone https://github.com/faf0/sct
cd sct
```

Và tiến hành compile thôi!
Lệnh dưới đây sẽ chuyển file mã nguồn `xsct.c` thành file thực thi (execute) `xsct`.

```bash
make
```

Bạn có thể dùng ngay tại folder này nếu muốn bằng lệnh sau:

```bash
./xsct 3000 # Lệnh này khiến màn hình bạn vàng khè, chạy `xsct -t` để khôi phục.
```

Riêng tôi sẽ chuyển `xsct` đến một địa chỉ nằm trong `$PATH` để có thể dùng từ mọi nơi.
Thay vì chạy lệnh `make` tôi sẽ chạy:

```bash
sudo make clean install
```

## Cách dùng

Khi theo sau là một số từ `1000-10000`, nhiệt độ màng hình sẽ chuyển thành giá trị đó.

Để bật tắt chế độ ban đêm nhanh gọn mà không cần nghĩ đến nhiệt độ màu, hãy dùng lệnh `xsct -t`.

Để thuận tiện, tôi sẽ gán `xsct -t` vào `sxhkd`.
Tôi dùng cụm phím {{< kbd "Super Alt n" >}}.

```sh
super + alt + n
	xsct -t
```
