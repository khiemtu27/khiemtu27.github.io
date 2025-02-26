---
title: Linux, FOSS và FSF
tags:
  - tech
date: 2023-12-23
slug: linux-foss-va-fsf
---
Trong bài viết [Tạm biệt Windows](/tam-biet-windows) tôi đã nói về lí do tôi bỏ dùng Windows. Để tiếp tục kể về hành trình công nghệ của mình, tôi sẽ nói về Linux. Một trong những thay đổi lớn nhất của tôi trong cuộc sống công nghệ tính đến nay.

## Hành trang

Trước hết phải nói về những "cỗ máy" của tôi hiện tại. Tôi có một máy để bàn được đề cập trong bài viết trước. Cấu hình sơ sơ là:

| Loại      | Tên và số lượng            |
| ---       | ---                        |
| Mainboard | Z97X-UD5H-BK               |
| CPU       | Intel i7-4790              |
| GPU       | NVIDIA GeForce GTX 1070 Ti |
| RAM       | 24GB                       |
| HDD       | 2x1TB                      |
| NVME      | 1x512GB                    |
| SSD       | 1x120GB                    |

Máy này tôi đang để chạy liên tục làm server sao lưu dữ liệu, và Pi-hole để dùng làm DNS chặn quảng cáo, cũng như các tên miền thu thập dữ liệu cho những thiết bị của mình. Ngoài ra tôi còn một laptop có cấu hình như sau:

| Loại  | Tên và số lượng                   |
| ---   | ---                               |
| Model | 81LD Lenovo Legion Y7000P         |
| CPU   | Intel i5-8300H                    |
| iGPU  | UHD Graphics 630                  |
| dGPU  | NVIDIA GeForce GTX 1050 Ti Mobile |
| RAM   | 16GB                              |
| NVME  | 1x512GB                           |
| SSD   | 1x120GB                           |

Hầu hết thời gian tôi sử dụng laptop, vì tiện lợi hơn. Một lí do hơi lạ nữa là do màn hình nhỏ, lại có độ phân giải Full HD, việc này giúp mật độ điểm ảnh của laptop cao hơn màn hình máy để bàn (cũng Full HD nhưng tới 27"), trải nghiệm về phần nhìn trên laptop sẽ nét hơn rất nhiều!

Trên đây là hai thiết bị của tôi hiện tại. Máy bàn tôi đang chạy Arch Linux, đang rất muốn cài lại Debian để đúng vai trò làm server, ổn định hơn nhưng tại đang lười... Còn laptop tôi đang dùng Void Linux bản glibc.

## Tại sao lại là Linux?

Ngoài Windows ta có vài lựa chọn khác, mỗi lựa chọn có điểm mạnh và yếu riêng. MacOS thì rất xịn xò, mượt mà với các tác vụ từ phổ thông cho đến coding và chỉnh sửa video... nhưng đối với tôi là quá xa xỉ. Việc mọi thứ trở nên "ngắn hạn" sẽ làm chi phí duy trì hệ sinh thái đó cao hơn gấp bội. Từ _ngắn hạn_ ở đây ý tôi muốn nói về hiện tượng bản quyền các phần mềm trả phí đều chuyển sang dạng đăng ký (subscription) thay vì mua một lần xài mãi mãi, tất nhiên việc này chẳng hề gì với những người có hầu bao dư dả (tiếc là tôi không phải họ). Chưa nói đến thiết bị của Apple cũng cao cấp về chất lượng lẫn giá tiền, mà tôi thì đang có sẵn hai con máy vẫn đang còn chạy ổn.

Bên cạnh đó ta có Chrome OS, hệ điều hành tận dụng xu hướng Web Application, nhỏ gọn chỉ cần có kết nối Internet là làm đủ mọi trò. Tiếc là trong đủ mọi trò đó không có vài trò tôi cần, ví dụ như chỉnh sửa video, thu âm, chỉnh âm bán chuyên. Và trở ngại lớn nhất của Chrome OS đối với tôi không đâu khác là Google. Ông hoàng buôn bán dữ liệu, giúp các nhà quảng cáo tiếp cận người dùng... việc bị thu thập dữ liệu là một trong những lí do chính khiến tôi rời Windows, chẳng có lí gì lại đâm đầu vào một nơi tồi tệ hơn.

Ngoài ra còn FreeBSD, OpenBSD, Haiku... nhưng hầu hết quá đặc thù về chức năng và ngoài nhu cầu của tôi nên xin không có ý kiến.

Sau hồi lâu cân nhắc thì tôi đã yên phận cùng Linux, và dưới đây sẽ là vài lí do cho quyết định này.

## Hiểu đúng về Linux

Công việc xử lý của máy tính được vận hành bởi cá thể nhỏ nhất là những bóng bán dẫn, chúng giữ những giá trị nhị phân gồm 0 và 1. Để giao tiếp với những chấm đen li ti này ta cần phải có một dạng phần mềm, đó là các drivers. Nhưng các drivers không vẫn chưa đủ để chuyển hóa điện năng thành quang năng, dòng chữ bạn đang đọc. Đây là lúc cái tên Linux bắt đầu xuất hiện.

Linux, được công bố năm 1991, là kernel máy tính khởi nguồn và phát triển bởi [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds). ngoài ra còn có NT Kernel (1993) trên Windows, XNU Kernel (1996) của Apple trên các máy macOS. Kernel của máy tính là một phần mềm có nhiệm vụ giao tiếp với driver để điều khiển phần cứng.

Tôi muốn nhấn mạnh rằng kernel và driver là hai phần riêng biệt, vì ban đầu tìm hiểu tôi cứ nghĩ kernel là một gói những driver nhưng thực chất không phải vậy. Giống như một cỗ xe ngựa:

* Cỗ xe là phần cứng.
* Con ngựa là driver.
* Người lái xe ngựa là kernel.
* Chúng ta là hành khách.

Nói đến đây có thể thấy việc chuyển sang dùng Linux về phần thô cũng không quá khác biệt, Linux chỉ là một kernel, một công cụ để ta tương tác với các máy móc của mình. Điểm khác biệt lớn nhất không nằm ở chức năng, mà là thứ triết lý của những con người đằng sau Linux.

## FOSS và lý tưởng cốt lõi của Linux

FOSS viết đầy đủ ra là ***F****ree and **O****pen-****S****ource **S***oftware. Chữ _free_ ở đây không có nghĩa là miễn phí, mà là _tự do_. Mặc dù phần lớn các phần mềm gắn nhãn FOSS đều miễn phí, nhưng không vì vậy mà ta hiểu sai nghĩa của nó rồi lại bắt họ không được thu phí nhé!

Chữ _open-source_ ở đây nghĩa là _mã nguồn mở_. Một thuật ngữ thường chỉ phổ biến với những lập trình viên, người phát triển phần mềm hoặc làm trong lĩnh vực khoa học máy tính. Nhưng đừng lo, tôi sẽ cố dùng lượng kiến thức hạn hẹp của mình để giải thích nghĩa cũng như nguồn gốc của những từ này ngay dưới đây.

### Free - Sự tự do quá mức

Nếu như ở Windows, việc gỡ bỏ trình duyệt web mặc định là bất khả thi, Linux sẽ là một thế giới hoàn toàn trái ngược. Bạn có thể **gỡ hay cài bất cứ thứ gì bạn muốn**. Bạn có thể cài vài chục cái trình duyệt web, vài chục phần mềm chỉnh sửa văn bản, không ai cấm bạn cả. Chỉ cần có một chiếc máy chạy Linux có kết nối mạng, quyền năng hoàn toàn trong tay bạn, bạn sẽ có quyền sinh sát với cái máy của mình. Đúng vậy, quyền sát nữa...

Việc quá tự do cũng có hai mặt. Trong lúc hưởng thụ quyền năng của mình, bạn có thể gỡ phải những thứ thiết yếu với hệ điều hành chẳng hạn như kernel, bootloader, firmwares, display server... những thứ rất có thể đều là xa lạ với bạn, vì ở những thế giới kia, bạn không có quyền động đến hoặc được khuyến cáo là không nên động đến chúng.

Đó là chỉ mới nói đến tự do trong việc sử dụng thôi. Ngoài ra bạn còn có thể tự do _sao chép, phân phối, soi mói_, thậm chí là _sửa đổi_ phần mềm đó nếu bạn muốn. Phong trào này được khởi xướng  bởi [Richard Stallman](https://en.wikipedia.org/wiki/Richard_Stallman), ông đặt tên cho nó là Free Software, bạn có thể tìm hiểu thêm về _free software movement_ [tại đây](https://en.wikipedia.org/wiki/Free_software_movement).

Free software, tóm gọn là một phong trào ưu tiên quyền hạn của người dùng lên hàng đầu trong việc sáng chế, sử dụng và kinh doanh phần mềm. Richard quan niệm phần mềm được viết ra để phục vụ con người nhưng hầu hết ta bị lệ thuộc vào nó bởi chúng là công cụ kiếm tiền của các công ty công nghệ. Để duy trì phong trào này, ông đã thành lập Free Software Foundation, gọi tắt là [FSF](https://www.fsf.org/).

Giả dụ nếu Microsoft Edge là một phần mềm FOSS, tùy vào loại giấy phép (license) Microsoft đăng ký mà bạn có thể chỉnh sửa mã nguồn của nó, lập trình thêm chức năng mà bạn muốn, cắt bỏ đi những chức năng bạn không cần. Thậm chí có một số giấy phép còn cho phép bạn tải về, chỉnh sửa chút ít rồi bán ra ngoài thị trường với cái mác là trình duyệt web của bạn, hoàn toàn không phạm pháp.

Hầu hết phần mềm của các ông trùm công nghệ có mã nguồn đóng (closed-source), nghĩa là file bạn bấm vào để khởi chạy phần mềm, và những file đi kèm đã được tạo thành từ những dòng code mà **chỉ có họ** mới có thể đọc, chỉnh sửa... Nói đến đây ta cần hiểu về phần còn lại của từ viết tắt FOSS.

### Open Source - Trần trụi cách tư duy của một phần mềm

Bất kỳ loại phần mềm nào, từ đơn giản như những drivers, firmwares cho đến phức tạp như Cyberpunk 2077, cũng đều được lập trình từ những dòng mã, gọi là **mã nguồn** (source code).

Mã nguồn có thể được viết bởi nhiều ngôn ngữ (C, C{pp}, Java, Rust...), nhưng để những dòng mã này có nghĩa với các bóng bán dẫn, ta cần _biên dịch_ chúng thành thứ ngôn ngữ khó hiểu với ta, nhưng dễ hiểu với... dòng điện. Gọi là ngôn ngữ cũng không hẳn đúng, mà là một bản kế hoạch, phân chia nhiệm vụ cho CPU, RAM, ổ đĩa...

Biên dịch có động từ trong tiếng Anh là _compile_. Một phần mềm muốn tối ưu và tiện lợi trong lúc trao đổi, tải về, phân bố nên được biên dịch thành những _gói_ (packages), những _files thực thi_ (binaries) nhỏ gọn. Việc người dùng cài đặt phần mềm trên máy tính hầu hết chỉ là giải nén và đặt các files vào đúng nơi chúng thuộc về. Thường thì **mã nguồn phải được biên dịch thành file thực thi mới chạy được**.

Điều quan trọng ở đây là các files đã được biên dịch rất khó, gần như không thể để biến trở về mã nguồn, từ chính xác là **giải mã ngược** (decompile). Nghĩa là hành vi, tương tác của một phần mềm đã được biên dịch chỉ có thể biết chính xác bởi người viết nên mã nguồn của phần mềm đó.

Ví dụ tôi viết một phần mềm mã nguồn đóng gọi là "Uploader" để bạn đăng hình lên Google Drive chẳng hạn. Khi bạn chọn file và bấm tải lên, thấy file đó đã hiện ra trên Google Drive, phần mềm của tôi đã hoàn thành nhiệm vụ của nó. Nhưng nếu tôi là thành phần xấu, trong mã nguồn của  Uploader có một đoạn mã đi kèm, đoạn mã này ra lệnh nó sẽ tải tất cả các mật khẩu lưu trong máy của bạn qua máy của tôi. Nếu chủ quan và không có các kỹ thuật theo dõi phần mềm, đường truyền mạng... bạn sẽ không hề hay biết, hoặc khi biết thì đã quá muộn.

Nhưng nếu trong trường hợp Uploader là một chương trình mã nguồn mở, bạn có thể vào đống mã nguồn của nó và xem **tất cả hành động nó sẽ làm** trên máy của bạn từ lúc bạn mở nó lên tới lúc tắt. Điểm mạnh ở đây là sự minh bạch hoàn toàn.

Thật ra để chắc ăn nhất thì chính bạn phải là người thực hiện thao tác biên dịch đống mã nguồn cung cấp từ nhà phát triển thành file thực thi. Tránh trường hợp tôi đăng thì đăng một đống mã nguồn sạch, nhưng lại dùng đống mã nguồn độc hại để biên dịch ra file thực thi bạn sẽ tải về dùng. ([Gentoo](https://www.gentoo.org/) là một bản phân phối của Linux cho những ai có nhu cầu này.)

Linux là một kernel mã nguồn mở, cốt lõi này thấm nhuần vào phần lớn những lập trình viên viết phần mềm để chạy trên Linux, không có nghĩa là phần mềm nào trên Linux cũng là mã nguồn mở đâu nha!

Cho đến hiện tại, về phần driver và những thứ thiết yếu, máy tôi vẫn còn một vài phần mềm mã nguồn đóng, chẳng hạn như GPU driver của NVIDIA (nvidia-dkms), bản cập nhật phần mềm CPU Intel Microcode (intel-ucode), và những phần mềm trong BIOS đi kèm phần cứng của máy. Ngoài ra tôi không dùng một phần mềm nào không phải FOSS, từ thu âm, lướt web, chỉnh sửa video, chỉnh ảnh, viết blog...

Việc tránh hoàn toàn mã nguồn đóng đối với tôi gần như là bất khả thi, tôi chỉ có thể hạn chế sử dụng chúng mà thôi. Càng sử dụng FOSS, bạn càng có thêm quyền kiểm soát những máy móc của mình. Bạn biết chúng đang làm gì, sẽ làm gì, bạn hiểu cách chúng vận hành, từ đó làm cốt lõi để tận dụng kết quả tiến hóa độc nhất của con người, máy tính.

Lợi ích của FOSS còn rất nhiều, về bảo mật, tiềm năng phát triển, thiện ác... nhưng hiện tại tôi chỉ nói đến cách vận hành, cốt lõi của FOSS cho người dùng phổ thông như tôi và bạn cái đã. Hẹn các bạn dịp tương lai tôi sẽ nói thêm!

## Còn nữa

Đây là một bài viết trong số những bài viết về linux của tôi. FOSS là lý do hàng đầu trong những lí do tôi quyết định dùng Linux suốt ba năm qua.
