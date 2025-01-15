---
title: dmenu - Dynamic Menu
tags:
    - tech
date: 2024-06-02
slug: dmenu
---
Dmenu hay gọi đầy đủ là dynamic menu, là một phần mềm nữa đến từ [Suckless](/suckless). Bài viết này tôi sẽ không đi sâu vào công dụng cũng như cách thức hoạt động của dmenu mà chỉ bàn nhiều về các bước cài đặt. Sẽ có một bài viết trong tương lai tôi chia sẻ về những ứng dụng của dmenu, cũng như các shell scripts tôi dùng.

Tương tự như những bài viết trước về phần mềm của Suckless, chúng ta sẽ bắt đầu bằng một bảng kế hoạch:

* [Cài đặt gói cần thiết](/dmenu/#dependencies)
* [Tải mã nguồn](/dmenu/#source-code)
* [Cài đặt patch](/dmenu/#patch)
  1. [Password](/dmenu/#password)
  2. [Reject no match](/dmenu/#reject-no-match)
  3. [Case insensitive](/dmenu/#case-insensitive)
* [Thay đổi chiều cao](/dmenu/#bar-height)
* [Màu sắc](/dmenu/#colors)

---

## Cài đặt gói cần thiết

Dùng lệnh dưới đây tùy vào distro của bạn để cài các gói cần thiết cho dmenu.

```bash
# Debian/Ubuntu/PopOS
sudo apt install build-essential libx11-dev libxft-dev libxinerama-dev

# Arch Linux
sudo pacman -S base-devel libx11 libxft libxinerama

# Void Linux
sudo xbps-install -S base-devel libXft-devel libXinerama-devel
```

Nếu máy của bạn không có nhiều màn hình (multi-monitor), bạn có thể comment hoặc xóa hai dòng sau trong config.mk và không cần cài đặt các gói `Xinerama` tương ứng với từng distro. Hai thư viện này giúp dmenu cũng như dwm hỗ trợ đa màn hình, nếu không cần, xóa đi cũng đỡ được tí mã nguồn đấy!

```Makefile
XINERAMALIBS  = -lXinerama
XINERAMAFLAGS = -DXINERAMA
```

---

## Tải mã nguồn

Dùng lệnh sau để tải mã nguồn dmenu về máy từ git server của Suckless.

```bash
# Clone repo dmenu về máy
$ git clone https://git.suckless.org/dmenu

# Tạo thư mục lát nữa chứa các patches
$ mkdir dmenu/patches

# Vào thư mục gốc của dmenu
$ cd dmenu
```

---

## Cài đặt patch
Tôi sẽ giải thích về khái niệm patch (bản vá) trong [bài viết về Suckless](/suckless), bạn có thể tham khảo cụ thể vấn đề này [tại đây](/suckless/#patch).

### Password
Sau này tôi sẽ dùng dmenu để nhập mật khẩu máy tính mỗi lần muốn mã hóa một tệp nào đó, để mặc định dmenu hiển thị mọi thứ gõ vào là rất nguy hiểm. Patch này sẽ cho đúng ta lựa chọn `-P`, tất cả dữ liệu nhập vào dmenu sẽ được che lại bằng một ký tự được đặt trong `dmenu.c`.

```bash
$ curl https://gitlab.com/khiemtu27/dmenu/-/raw/master/patches/password.diff -o patches/password.diff
$ patch -i patches/password.diff
```

Để thay đổi ký tự `*` mặc định thành một ký tự khác, bạn có thể vào `dmenu.c` và thay đổi dòng sau:

```c
memset(censort, '*', strlen(text));
```

---

### Reject no match

Ví dụ khi chỉ có hai lựa chọn `yes` và `no`, sau khi đưa dmenu lựa chọn `-r` bạn sẽ không thể nhập {{< kbd "m" >}} vì chẳng có kết quả nào cho ký tự đó cả. Dmenu sẽ từ chối các ký tự không trùng với bất kỳ kết quả nào.

Patch này sẽ phát huy tác dụng trong các trường hợp như trên, chỉ được chọn `yes` hoặc `no` hoặc bấm {{< kbd "Esc" >}} để không chọn gì cả.

```bash
$ curl https://gitlab.com/khiemtu27/dmenu/-/raw/master/patches/reject-no-match.diff -o patches/reject-no-match.diff
$ patch -i patches/reject-no-match.diff
```

---

### Case insensitive

Mặc định dmenu sẽ phân biệt viết hoa và thường. Ví dụ có hai lựa chọn `apple` và `Apple`, khi bấm {{< kbd "A" >}}, lựa chọn `apple` sẽ mất đi, chỉ còn lại lựa chọn `Apple`! Muốn tắt phân biệt hoa thường ta phải đưa dmenu lựa chọn `-i`.

Patch này sẽ khiến dmenu mặc định **không phân biệt hoa thường**. Muốn phân biệt hoa thường phải đưa dmenu lựa chọn `-s`.

> **Lưu ý**
>
> Hầu hết các script trên mạng đều phục vụ cho bản mặc định của dmenu, do đó các tác giả thường dùng lựa chọn `-i` để tắt phân biệt hoa thường. Patch này sẽ xóa hoàn toàn lựa chọn đó, vì thế khi chạy các script này sẽ thông báo lỗi.
>
> Muốn khắc phục phải vào file script để xóa đi lựa chọn `-i`. Bạn nên cân nhắc điều này khi cài patch này nhé!

```bash
$ curl https://gitlab.com/khiemtu27/dmenu/-/raw/master/patches/case-insensitive.diff -o patches/case-insensitive.diff
$ patch -i patches/case-insensitive.diff
```

---

## Thay đổi chiều cao

Cũng như với [dwm](/dwm/#bar-height), tôi sẽ không dùng patch để đưa lựa chọn chiều cao vào config.def.h mà sẽ chỉnh sửa trực tiếp trong `dmenu.c`.

Để thay đổi chiều cao dmenu, chúng ta sẽ chỉnh sửa thẳng trên file `dmenu.c`, hãy tìm kiếm dòng sau:

```c
bh = drw->fonts->h + 2;
```

Ở đây ta có thể thấy chiều cao của dmenu bằng chiều cao của font chữ, cộng thêm 2 pixels. Muốn thay đổi chiều cao dmenu, hãy tự tin thay đổi phần `+ 2` thành bất cứ giá trị dương nào bạn muốn. Để trùng lấp vào thanh trạng thái của dwm tôi sẽ dùng chung một giá trị cho cả hai chúng nó.

```c
bh = drw->fonts->h + 6;
```

## Màu sắc

Tương tự như [st](/st/#colors) và [dwm](/dwm/#colors), tôi sẽ tách phần cài đặt màu sắc, giao diện vào những file riêng biệt để dễ dàng thay đổi màu sắc dmenu. Chỉ cần đổi một dòng thay vì từng giá trị một.

Trong patch này tôi sẽ thay gói màu `[SchemeOut]` thành `[SchemeDim]` để có thể dùng chung file màu sắc với dwm của mình. Đồng thời tôi sẽ đính kèm các bảng màu tương tự như dwm và st. Bạn có thể [đến đây](/st/#showcase) để tham khảo các bảng màu.

```sh
$ curl https://gitlab.com/khiemtu27/dmenu/-/raw/master/patches/colors.diff -o patches/colors.diff
$ patch -i patches/colors.diff
```

Mặc định tôi sẽ để bảng màu `colors-dark`, ví dụ muốn đổi sang `nord`, hãy vào config.h thay dòng:

```c
#include "colors-dark.c"
```

thành:

```c
#include "colors-nord.c"
```

## Kết lại

Phía trên là cách chuẩn bị một bộ mã nguồn dmenu đầy đủ chức năng (đối với tôi). Để cài đặt lên máy và sử dụng, nếu chưa biết, bạn có thể tham khảo [bài viết này](/suckless/#install).

Như đã nói ở đầu bài viết, tôi sẽ có một bài viết khác nói sâu vào các ứng dụng của dmenu. Bật mí vài thứ tôi dùng dmenu như: quản lý bookmarks, thư viện ebook, di chuyển giữa các thư mục, truy xuất mật khẩu...

Chính vì quá đơn giản nên dmenu có thể linh hoạt góp công vào mọi hoạt động trên máy tính để khiến các shell script tăng tính tương tác!
