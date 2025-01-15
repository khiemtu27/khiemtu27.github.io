---
title: Khái niệm bản vá của phần mềm suckless
tags:
- tech
date: 2024-06-12
slug: suckless-patches
---
## Khái niệm

Bản chất những phần mềm của cộng đồng Suckless lúc ra lò cực kỳ đơn giản vì thế khó lòng đáp ứng nhu cầu của người dùng. Điều này là chủ ý của họ, họ cung cấp một phần mềm mang tính nền móng, tối giản, cực kỳ ổn định _(stable)_ và làm tốt nhiệm vụ của mình. Phần còn lại như thêm bớt các tính năng, tinh chỉnh, tùy biến giao diện... là do toàn quyền người dùng quyết định. Những sửa đổi tùy biến màu sắc, phím tắt đơn giản đối với tôi nên được gọi là tinh chỉnh.

Patch là những thay đổi về mặt chức năng, các hàm, phần cốt lõi và tinh vi hơn trong mã nguồn. Khái niệm patch (có thể hiểu là bản vá, chỉnh sửa) là một thứ xa lạ với người dùng máy tính phổ thông, nếu bạn thấy khó hiểu và có phần e ngại thì đừng lo! Hồi mới dùng các phần mềm của Suckless tôi cũng không hiểu gì cả... Vì tâm lý người dùng trước giờ dạng như mỳ ăn liền, tải về dùng ngay. Chuyển sang các phầm mềm Suckless không thể không bỡ ngỡ, vì giống như được trao một đặc quyền là trước giờ chưa từng được nắm giữ, tùy chỉnh 100% phần mềm của mình, từng dòng code một!

Tôi có thói quen cài đặt các patches theo thứ tự độ phức tạp giảm dần. Nghĩa là tôi sẽ xem file diff của từng patch, xem độ dài ngắn, mức độ phức tạp (thay đổi file gốc nhiều hay ít) để rồi sẽ cài đặt những patches phức tạp trước. Đây là một kinh nghiệm tôi học được sau nhiều lần làm việc với các phần mềm Suckless. Tin tôi đi, khi các patches phức tạp được cài đặt từ đầu, việc cài các patches đơn giản về sau là dễ hơn nhiều!

## Cơ chế

Các patches sẽ được lưu dưới định dạng file diff _(có thể là viết tắt của differences?)_. Nó đơn giản chỉ là file thể hiện những thay đổi của các văn bản, khi một người chỉnh sửa mã nguồn của phần mềm, họ sẽ dùng lệnh `diff` để so sánh mã nguồn cũ với mã nguồn mới và xuất ra file diff. Bạn chỉ cần chú ý:

* Tên của file được so sánh
* Vị trí diễn ra thay đổi trong các files
* Dòng có dấu `+` thì copy vào file gốc
* Dòng có dấu trừ `-` thì xóa đi

Để cài đặt patch bằng lệnh, tất nhiên chúng ta cần lệnh `patch`. Lệnh này có thể được cung cấp thông qua gói `base-devel` trên Arch Linux và Void Linux, `build-essential` trên Debian, Ubuntu... Hoặc trên máy của tôi (chạy Void Linux), `patch` sẽ được cài riêng lẻ không theo một gói meta _(meta-package)_ nào cả bằng lệnh `sudo xbps-install -S patch`.

> These packages _(meta-packages)_ do not contain actual software, they simply depend on other packages to be installed.
>
> -- [Ubuntu Documentation](https://help.ubuntu.com/community/MetaPackages)

Meta-packages có thể hiểu đơn giản là gói của các gói. Nó là một chiếc hộp chứa các gói khác, bản thân nó không là gì cả, chỉ là một tên gọi để các Trình quản lý gói _(Package Manager)_ hiểu là hãy cài đặt cả thảy những gói này nhé!
