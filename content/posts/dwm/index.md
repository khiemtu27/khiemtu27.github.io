---
title: dwm - Dynamic Window Manager
tags:
  - tech
date: 2024-05-05
slug: dwm
---
**dwm** là [trình quản lý cửa sổ](https://en.wikipedia.org/wiki/Window_manager) của [Suckless](/suckless). Tính đến nay tôi dùng dwm được tầm một năm rưỡi, cũng có bỏ túi được tí kinh nghiệm. Bài viết này sẽ ghi lại quá trình tôi setup dwm của mình, dưới đây là một danh sách những thứ tôi sẽ làm:

* [Cài đặt gói cần thiết](/dwm/#dependencies)
* [Tải mã nguồn](/dwm/#source-code)
* [Cài đặt patch](/dwm/#patch)
  1. [Pertag](/dwm/#pertag)
  2. [Sticky](/dwm/#sticky)
  3. [Save floats](/dwm/#save-floats)
  4. [Hide vacant tags](/dwm/#hide-vacant-tags)
  5. [Remove border](/dwm/#remove-border)
  6. [Always center](/dwm/#always-center)
  7. [Shift tools](/dwm/#shift-tools)
  8. [Monocle symbol](/dwm/#monocle-symbol)
* [Gỡ bỏ thanh tiêu đề](/dwm/#no-title)
* [Gỡ ràng buộc với dmenu](/dwm/#no-dmenu)
* [Chiều cao thanh trạng thái](/dwm/#bar-height)
* [Màu sắc](/dwm/#colors)

---

## Cài đặt gói cần thiết

Trước hết ta cần một môi trường linux đủ các "gói cần thiết" _(dependencies)_ để dwm hoạt động. Lệnh cài đặt các gói này tùy vào distro và package manager bạn dùng:

```bash
# Debian/Ubuntu/PopOS
sudo apt install git curl build-essential libx11-dev libxft-dev libxinerama-dev

# Arch Linux
sudo pacman -Sy git curl base-devel libx11 libxft libxinerama

# Void Linux
sudo xbps-install -S git curl base-devel libX11-devel libXft-devel libXinerama-devel
```

Nếu máy của bạn không có nhiều màn hình (multi-monitor), bạn có thể comment hoặc xóa hai dòng sau trong config.mk và không cần cài đặt các gói Xinerama tương ứng với từng distro. Hai thư viện này giúp dwm hỗ trợ đa màn hình, nếu không cần, xóa đi cũng đỡ được tí mã nguồn đấy!

```Makefile
XINERAMALIBS  = -lXinerama
XINERAMAFLAGS = -DXINERAMA
```

---

## Tải mã nguồn

Vào lúc tôi viết bài viết này thì bản cập nhật gần đây nhất của dwm là 6.5, công bố ngày 19/03/2024. Để tải mã nguồn dwm về máy, bạn có thể dùng lệnh sau:

```bash
git clone https://git.suckless.org/dwm
```

---

## Cài đặt patch

Tôi sẽ giải thích về khái niệm patch (bản vá) trong [bài viết về Suckless](/suckless), bạn có thể tham khảo cụ thể vấn đề này [tại đây](/suckless/#patch).

### Pertag

Bạn có thể hiểu tag trong dwm gần giống như workspace hay màn hình ảo (virtual desktop). Patch này giúp những tùy chỉnh về số lượng master, layout... sẽ được lưu ở mỗi tag thay vì đồng bộ chúng như mặc định dwm làm. Nghĩa là sau khi cài patch này, ta có thể để tag 1 ở layout tile và tag 2 ở layout monocle.

Đầu tiên chúng ta sẽ tải file diff của `pertag` về máy, ở đây tôi sẽ dùng lệnh curl. Link của các files diff tôi sẽ để ở ghi chú số nhỏ cạnh tên các patch. Để thư mục gọn gàng hơn tôi sẽ tạo một thư mục con chỉ để chứa các files diff.

```bash
# tạo thư mục patches để chứa các files diff
$ mkdir dwm/patches

# cd vào folder mã nguồn của dwm
$ cd dwm

# tải file diff của patch pertag vào thư mục patches
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/pertag.diff -o patches/pertag.diff
```

Để tiến hành apply patch tự động, ta dùng lệnh patch, lưu ý bạn phải đang ở trong thư mục gốc của dwm nhé:

```bash
$ patch -i ./patches/pertag.diff
patching file dwm.c
Hunk #3 succeeded at 273 (offset 1 line).
Hunk #4 succeeded at 644 (offset -6 lines).
Hunk #5 succeeded at 655 (offset -6 lines).
Hunk #6 succeeded at 1006 (offset -1 lines).
Hunk #7 succeeded at 1536 (offset -7 lines).
Hunk #8 succeeded at 1561 with fuzz 2 (offset -7 lines).
Hunk #9 succeeded at 1783 (offset 23 lines).
Hunk #10 succeeded at 2100 (offset 22 lines).
```

Như bạn thấy, lệnh tôi nhập là `patch -i ./patches/pertag.diff` và bên dưới trả kết quả mọi thứ đều "succeeded" nghĩa là okela hết!

---

### Sticky

Patch này cho phép ta gán phím tắt để chuyển cửa sổ bình thường thành cửa sổ "dính" _(sticky)_, điều này khiến cửa sổ đó sẽ theo ta cho dù ta ở tag nào đi nữa. Thích hợp dùng chung với chức năng "Picture-in-picture" của các trình duyệt web để việc xem video và đa nhiệm được tiện lợi hơn!

![Cửa sổ dính sẽ đi theo bạn ở mọi tags](sticky.gif)

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/sticky.diff -o patches/sticky.diff
$ patch -i patches/sticky.diff
```

---

### Save floats

Mặc định khi chuyển đổi nhanh (toggle) giữa floating mode và tiling mode, cửa sổ sẽ nằm ở vị trí tương tự khi nó đang tiling và các cửa sổ khác sẽ lấp đầy vị trí cũ của nó. Patch này sẽ lưu vị trí
khi đang float của cửa sổ và khôi phục lại vị trí đó khi bạn chuyển đổi chế độ của nó. Ví dụ một cửa sổ đang float ở chính giữa màn hình, khi tôi đưa nó vào tiling mode nó sẽ nằm ngay ngắn vào chung các cửa sổ khác, khi tôi chuyển nó thành floating mode lại, nó sẽ nằm ngay giữa màn hình như lúc đầu.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/save-floats.diff -o patches/save-floats.diff
$ patch -i patches/save-floats.diff
```

---

### Hide vacant tags

Patch này đơn giản sẽ ẩn các tags trống (không chứa cửa sổ nào), làm gọn gàng thanh trạng thái!

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/hide-vacant-tags.diff -o patches/hide-vacant-tags.diff
$ patch -i patches/hide-vacant-tags.diff
```

---

### Remove border

Đơn giản là ẩn khung cửa sổ (border) khi chỉ có một cửa sổ trên màn hình.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/remove-border.diff -o patches/remove-border.diff
$ patch -i patches/remove-border.diff
```

---

### Always center

Cửa sổ floating **khi bật** sẽ luôn nằm ngay giữa màn hình thay vì góc trên cùng bên trái màn hình. Tuyệt vời nhất khi phối hợp với `save floats`.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/always-center.diff -o patches/always-center.diff
$ patch -i patches/always-center.diff
```

---

### Shift tools

Ở patch này sẽ có tí khác biệt, vì tôi sẽ không dùng tất cả các hàm (functions) mà nó mang lại, do đó sẽ có một số tinh chỉnh gỡ bỏ bớt các chức năng không dùng tới. Nhưng đầu tiên ta vẫn sẽ làm các bước như trước, chỉ là bạn sẽ tải file diff tôi viết tại repo GitLab chứ không phải trên trang chủ của Suckless.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/shift-tools.diff -o patches/shift-tools.diff
$ patch -i patches/shift-tools.diff
```

---

### Monocle symbol

Mặc định biểu tượng layout (bên phải dãy số các tags trên thanh trạng thái) sẽ hiển thị số cửa sổ đang mở trong tag hiện tại. Tôi không thích chức năng này và chỉ muốn nó hiện biểu tượng của monocle layout thôi.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/monocle-symbol.diff -o patches/monocle-symbol.diff
$ patch -i patches/monocle-symbol.diff
```

---

## Gỡ bỏ thanh tiêu đề

Mặc định phần tên cửa sổ trên thanh trạng thái của dwm sẽ dùng màu accent để làm background, đây có thể là điểm nhấn thẩm mỹ, nhưng tôi lại không thích điều này. Do đó tôi viết một patch để gỡ bỏ hoàn toàn hiển thị tên cửa sổ trên thanh trạng thái.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/notitle.diff -o patches/notitle.diff
$ patch -i patches/notitle.diff
```

---

## Gỡ ràng buộc với dmenu

Mặc định trong file cài đặt của dwm (config.def.h) có các biến và cài đặt như `dmenufont`, `dmenumon`, `dmenucmd`. Tôi thích các script gọi dmenu của mình nằm riêng biệt với mã nguồn của dwm nên thường xóa các dòng này trong config.h. Một điều bạn có thể thấy ngay khi xóa các dòng này là dwm sẽ lỗi khi build. Vì trong dwm.c cũng có một chỗ phụ thuộc vào những biến này. Thế nên tôi đã viết một patch để gỡ bỏ hoàn toàn dmenu khỏi mã nguồn dwm. Lưu ý là tôi sẽ giữ lại chức năng gọi dmenu bằng tố hợp phím {{< kbd "Mod p" >}}.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/nodmenu.diff -o patches/nodmenu.diff
$ patch -i patches/nodmenu.diff
```

---

## Chiều cao thanh trạng thái

Trước đây tôi dùng patch [statuspadding](https://dwm.suckless.org/patches/statuspadding) để thay đổi chiều cao cũng như căn ngang hai bên lề của thanh trạng thái. Nhưng suy đi nghĩ lại, việc vào config.h để tùy chỉnh cũng không khác gì vào `dwm.c`. Cài thêm một patch chỉ làm tăng khả năng mâu thuẫn với các patches khác.

Do đó tôi quyết định nếu có muốn tùy chỉnh chiều cao thanh trạng thái, tôi sẽ vào `dwm.c` và thay đổi dòng này:

```c
bh = drw->fonts->h + 2;
```

Dòng này quyết định biến `bh` viết tắt của barheight, nói chung nó sẽ lấy chiều cao của font chữ, cộng thêm 2 pixels. Từ đây, để điều chỉnh chiều cao thanh trạng thái, hãy tự tin thay đổi phần `+ 2` thành bất cứ số gì bạn muốn. Đối với tôi, `+ 6` là ổn nhất. Do đó, biến `bh` của tôi sẽ trông như sau.

```c
bh = drw->fonts->h + 6;
```

---

## Màu sắc

Để thuận tiện cho việc thay đổi giao diện của dwm, cụ thể là màu sắc thanh trạng thái và viền cửa sổ. Tôi thường không thay đổi từng màu trong config.h mà sẽ tạo riêng lẻ từng file màu sắc riêng và chỉ cần thay đổi một dòng trong config.h là đã có thể thay đổi tất cả các màu của dwm.

```bash
$ curl https://gitlab.com/khiemtu27/dwm/-/raw/master/patches/colors.diff -o patches/colors.diff
$ patch -i patches/colors.diff
```

Mẫu các bảng màu bạn có thể xem [tại đây.](/st/#showcase)

---

## Thành quả!

Thế là sau khi cài đặt các patches, chúng ta đã có một dwm "đầy đủ" chức năng rồi. Phần còn lại chỉ là tùy chỉnh file config.h, thêm các phím tắt, thay đổi màu sắc, font... sau đó xóa file config.h và cài đặt dwm lên máy của mình là xong. Các bước cài đặt bạn có thể tìm hiểu [tại đây](/suckless/#install).
