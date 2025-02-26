---
title: "Vim #1: Cài đặt cơ bản"
tags:
  - tech
date: 2024-03-29
slug: vim-1
---
Tôi viết và thiết kế blog này bằng Vim. Một phần mềm chỉnh sửa văn bản thường được dùng bởi số ít lập trình viên. Thường thì mọi người dùng VS Code, Sublime Text, Atom... nhưng vì không phải một lập trình viên, cũng mắc bệnh muốn tối giản hóa không gian làm việc của mình và ưu tiên những phần mềm nhỏ gọn, có thể ném vào trong terminal và chớp mắt đã khởi động xong nên tôi ưu tiên Vim. Xét cho cùng thì lí do lớn nhất khiến Vim vượt trội hẳn so với những phần mềm khác là khả năng tùy biến của nó, nếu bạn chưa tin hãy đọc hết bài viết này.

## Tại sao không phải Neovim?

Tôi cũng có dùng qua [Neovim](https://neovim.io/) được vài tháng nhưng thật tình mà nói, tôi không phải lập trình viên, do đó sự hiện diện của [TreeSitter](https://github.com/nvim-treesitter/nvim-treesitter), [LSP](https://github.com/neovim/nvim-lspconfig)... là ngoài nhu cầu của tôi. Cũng có phần do tôi đã quen với cách cài đặt Vim bằng Vimscript và lười học [Lua](https://www.lua.org/) để có thể khai thác hết tính vượt trội của Neovim nên cũng chưa thấy tha thiết học nó lắm. Tôi muốn nhấn mạnh rằng tôi không dùng Neovim không phải vì Neovim không tốt, mà là khác biệt nó mang lại nằm ngoài nhu cầu của tôi.

## Vimrc của tôi

Bạn có thể xem trọn file vimrc của tôi tại [GitLab](https://gitlab.com/khiemtu27/dotfiles/-/tree/master/vim). Còn bây giờ tôi sẽ giải thích chi tiết từng phần một để nói lí do vì sao chúng có trong file vimrc của tôi.

### Phần hiển thị

```vim
set conceallevel=2
```

`conceallevel` sẽ phát huy tác dụng khi soạn thảo file markdown như tôi đang làm khi soạn bài viết này. Ví dụ trong markdown để chèn một đường dẫn vào bài viết ta gõ `[chữ hiển thị](đường dẫn)`. Khi `conceallevel` là 2, sau khi đưa con trỏ ra khỏi dòng đó, phần syntax sẽ bị ẩn đi chúng ta chỉ còn thấy `chữ hiển thị`.

Sau khi đặt dòng này vào vimrc đừng ngạc nhiên quá khi file markdown của bạn gọn gàng đến lạ nhé! Và cũng đừng hoảng hốt khi không biết phải chỉnh sửa như thế nào khi syntax đều bị ẩn hết, tôi sẽ có cách giải quyết vấn đề này ở [phần hai](/vim-2).

---

```vim
set scrolloff=10
```

Giữ con trỏ luôn cách rìa trên và dưới của cửa sổ Vim một khoảng cách là 10 dòng khi di chuyển lên xuống trong văn bản. Điều này đảm bảo bạn sẽ luôn biết phía dưới và trên con trỏ mình đang là cái gì. Đồng thời giữ mắt bạn ở khu trung tâm màn hình đỡ phải mỏi mắt khi liếc xuống đáy hay lên trên mép màn hình...

---

```vim
autocmd VimEnter * silent !echo -ne "\e[2 q"
let &t_SI="\e[6 q"
let &t_EI="\e[2 q"
```

Trong Normal mode con trỏ sẽ ở dạng `block ▉` và dạng `line ▏` khi trong Insert mode. Tránh nhầm lẫn và để hạn chế phải nhìn xuống thanh trạng thái mỏi mắt lắm!

---

```vim
set number
set relativenumber
```

Hiển thị cột số hàng _(line number)_ bên trái màn hình. Tùy chọn `relativenumber` đếm số dòng từ dòng hiện tại, thay vì bắt đầu đếm từ đầu văn bản. Dòng sát trên và dưới của dòng hiện tại sẽ là dòng 1, dòng ngay trên và dưới nữa sẽ là dòng 2,... cứ thế tăng dần về hai hướng.

---

```vim
set splitbelow splitright
```

Vim sẽ đặt buffer _(có thể hiểu là cửa sổ)_ mới ở dưới khi chia ngang và ở bên phải khi chia cửa sổ bằng lệnh `:split` _(chia ngang)_ và `:vsplit` _(chia dọc)_.

---

```vim
set laststatus=2
set showcmd
```

Hiển thị thanh trạng thái ở dưới màn hình để hoạt động cùng plugin `lightline`. Nếu không dùng plugin này, bạn có thể để `laststatus=0` để ẩn hẳn thanh trạng thái, Vim sẽ trông tối giản hẳn ra!

---

```vim
syntax on
```

Những cài đặt này ai cũng nên có, bật syntax highlight cho văn bản và các dòng code, một tí màu sắc cho mắt đỡ rối! `filetype` sẽ giúp nhận diện loại file đang chỉnh sửa.

---

```vim
set wrap
set linebreak
set showbreak=⤷\
```

Vim sẽ tự động xuống dòng khi dòng hiện tại dài quá bề ngang cửa sổ terminal, lưu ý đây chỉ là hành động gói dòng mềm _(soft wrap)_, nghĩa là chỉ hiển thị như có xuống dòng, chứ file văn bản của bạn không hề bị thay đổi.

`showbreak=⤷` đặt ký tự này ở đầu những dòng bị gói, cài đặt này mục đích là để cho đẹp thôi. Lưu ý là sau biểu tượng `\` có dấu cách nữa đấy nhé!

### Phần chức năng

```vim
set undolevels=1000
```

Giữ lịch sử thao tác gồm 1000 thao tác từ hiện tại về trước, hiểu đại khái là chúng ta sẽ có thể undo 1000 lần.

---

```vim
set timeout
set timeoutlen=200
set ttimeoutlen=0
```

Cài đặt này cần giải thích hơi dài dòng tí. Ví dụ phím kbd:[a] bảo Vim thực hiện tác vụ A. Tôi gán một chuỗi phím _(keystroke)_ như kbd:[a] kbd:[b] để thực hiện tác vụ B. Lúc này khi bạn bấm kbd:[a], Vim sẽ phải chờ xem bạn có ấn kbd:[b] hay không để còn biết thực hiện tác vụ A hay B.

Khoảng chờ này mặc định là một giây (`timeoutlen=1000`). Nghĩa là nếu bạn bấm kbd:[a], sau một giây Vim sẽ thực hiện tác vụ A. Nếu trong một giây chờ đó bạn tiếp tục bấm kbd:[b], Vim sẽ thực hiện tác vụ B.

Tương tự trong Insert mode với phím kbd:[Esc]. Sau khi nhận lệnh bấm kbd:[Esc] lần thứ nhất, Vim sẽ không thoát Insert mode ngay lập tức mà sẽ chờ một khoảng thời gian `ttimeoutlen` **(chú ý là có hai chữ "t")**. Điều này khiến việc thoát Insert mode có hiện tượng trễ, thể hiện rõ ở thanh trạng thái, quan trọng là nó ảnh hưởng chức năng gõ tiếng Việt tôi sẽ nói ở [phần 3](/vim-3).

Chữ "t" thứ hai ở đây nghĩa là terminal. Sự khác nhau giữa `timeout` và `ttimeout` hơi phức tạp, cần phải nắm khái niệm về Vim keycode và Terminal keycode. Bạn có thể tìm hiểu [ở đây](https://vi.stackexchange.com/questions/10249/what-is-the-difference-between-mapped-key-sequences-and-key-codes-timeoutl). Chung là `timeoutlen=200` đặt khoảng chờ giữa các keystroke mâu thuẫn với nhau là 0.2 giây, `ttimeoutlen=0` hủy khoảng chờ để thoát Insert mode bằng kbd:[Esc].

---

```vim
set nobackup
set nowritebackup
set noswapfile
```

Đây là cài đặt có thể bạn sẽ không đồng ý với tôi nhưng tôi không thích Vim tạo các backup files nằm trong thư mục tạm _(cache)_. Đồng thời cũng tránh các thao tác thừa khi edit một file từng được edit lúc trước và bị bỏ dở. Bạn nên cân nhắc cài đặt này vì nó có thể khiến mất dữ liệu khi vô tình Vim bị thoát đột ngột mà chưa kịp lưu file.

---

```vim
set ignorecase
set smartcase
set incsearch
set nohlsearch
```

Khi tìm kiếm bằng nút `/`, tùy chọn `ignorecase` sẽ giúp Vim tìm mọi từ giống với từ khóa bạn nhập và không quan tâm viết hoa hay thường (tìm `hoa` thì `Hoa` cũng hiện lên). Tùy chọn `smartcase` giúp tìm kiếm linh hoạt hơn bằng cách nếu trong từ khóa tìm kiếm của bạn có ký tự viết hoa, Vim sẽ tạm thời tắt `ignorecase` và tìm đúng từ khóa bạn gõ vào (tìm `hOa` thì sẽ chỉ thấy `hOa` chứ không thấy `Hoa` hay `hoA`...).

`incsearch` khiến Vim highlight và nhảy đến vị trí từ khóa bạn đang tìm ngay lúc bạn đang nhập từ khóa thay vì đợi sau khi bấm Enter.

Mặc định Vim sẽ highlight tất cả vị trí có từ khóa bạn đang tìm trong files, việc này làm tôi khó nhìn thấy chính xác con trỏ đang ở vị trí nào. `nohlsearch` sẽ khiến Vim chỉ highlight duy nhất kết quả tìm kiếm mà con trỏ đang ở, và sẽ highlight kết quả tiếp theo khi bấm `n` hay `N`.

---

```vim
set list
set listchars=tab:▸\ ,trail:·
set smarttab smartindent
set shiftwidth=4
set tabstop=4 softtabstop=4
```

Ký tự `tab` sẽ được đặt ở những nơi đặt `tab`, trong trường hợp cài đặt của chúng ta, mỗi `tab` sẽ được biểu hiện bằng một dấu `▸` và ba dấu cách vì `tabstop` và `softtabstop` đều bằng bốn.

Ký tự `trail`, trong trường hợp này là `·`, sẽ xuất hiện ở cuối **những dòng kết thúc bằng dấu cách**.

Những cài đặt còn lại liên quan đến cách Vim xử lý thụt lề và đặt tab trong văn bản, tôi sẽ không giải thích kỹ chúng vì khá dài dòng. Cơ bản là tôi cũng không code nhiều nên cũng không rõ về mấy cái tab và indent này, bạn có thể tham khảo thêm về chúng [ở bài viết này](https://edryd.org/posts/vim-indenting/).

---

```vim
set textwidth=0
set wrapmargin=0
set formatoptions-=t
```

Một trong những tính năng được đặt mặc định của Vim là sẽ tự động cắt xuống dòng mới khi bạn gõ quá 80 ký tự trên một dòng. Hành động cắt xuống dòng này là tương tự với bấm kbd:[Enter] luôn chứ không phải là soft wrap.

Có thể nhà phát triển đặt thế vì một lý do gì đó hiệu quả nhưng đối với tôi, một người dùng Vim để viết blog thì việc làm trên là rất khó chịu... Ba dòng này chỉ đơn giản là để tắt triệt để chức năng này.

## Kết lại

Vậy là chúng ta đã tìm hiểu qua một số cài đặt cơ bản về phần hiển thị và chức năng của Vim. Sang [bài viết số hai](/vim-2), tôi sẽ nói về các phím tắt tôi dùng. Cảm ơn bạn đã đọc đến đây!
