---
title: "Xây dựng blog các nhân đơn giản bằng Jekyll trên Ubuntu 20.04"
excerpt: ""
toc: true
toc_sticky: true
tags: 
  - Jekyll
  - Installation
  - Tutorial
---

## I. Blog with Jekyll
Nếu bạn đang muốn xây dựng một cách nhanh chóng và hiệu quả một website/blog tĩnh cho cá nhân, Jekyll sẽ là một giải pháp tuyệt vời.
{: style="text-align: justify;"}

Là một công cụ mã nguồn mở giúp tạo trang web tĩnh từ template (static-site generator), Jekyll cho phép thực thi các dòng lệnh hỗ trợ việc quản lý website từ lúc khởi tạo tới khi triển khai sản phẩm một cách nhanh chóng.
{: style="text-align: justify;"}

Jekyll dễ dàng giúp bạn tạo nội dung cho bài viết cũng như quản lý chúng khi nó cung cấp nhiều công cụ tạo các categories, post hay layout cho website hay import nội dung. Nếu bạn thường xuyên làm việc offline, thường sử dụng các các trình soạn thảo nhỏ gọn (lightweight editor) để viết blog, Jekyll là một sự lựa chọn không thể tuyệt vời hơn.
{: style="text-align: justify;"}

Bài viết sẽ hướng dẫn các bạn cài đặt Jekyll trên Ubuntu cũng như tạo một blog đơn giản từ một template có sẵn.
{: style="text-align: justify;"}

## II. Cài đặt môi trường xây dựng website tĩnh bằng Jekyll

### Bước 0 - Cài đặt Ubuntu 20.04

Trước khi cài đặt Jekyll, các bạn cần cài đặt hệ điều Ubuntu 20.04. Sau khi hoàn tất cài đặt Ubuntu, cập nhật các package và các dependencies của chúng:
{: style="text-align: justify;"}

```terminal
$ sudo apt update -y && sudo apt upgrade -y
$ sudo apt autoremove
```

### Bước 1 - Cài đặt jekyll
Sau đó, cài đặt cái package hỗ trợ compile các thư viện của Jekyll. Vì Jekyll được viết bằng Ruby nên các bạn cũng cài đặt Ruby và các thư viện lên quan:
{: style="text-align: justify;"}

```terminal
$ sudo apt install -y make build-essential ruby ruby-dev
```

Để có thể dùng bộ quản lý package `gem` của Ruby và gọi lệnh Jekyll không cần phải `sudo`, các bạn thêm thêm các dòng sau trong file `.bashrc`:
Mở `.bashrc`:
{: style="text-align: justify;"}

```terminal
$ nano .basrhc
```

Thêm vào cuối file các dòng sau:

```sh
# Ruby export
export GEM_HOME=$HOME/gems
export PATH=$HOME/gems/bin:$PATH
```

Lưu và đóng file `.bashrc`, sau đó kích hoạt các export:

```terminal
$ source ~/.bashrc
```

Sau khi hoàn tất, chúng ta sẽ dùng lệnh `gem` để cài đặt Jekyll và Bundler:

```terminal
$ gem install jekyll bundler
```

Kiểm tra version của phiên bản Jekyll vừa cài đặt:

```terminal
$ jekyll -v
jekyll 4.2.0
```

Bước tiếp theo sẽ hướng dẫn các bạn mở firewall cho Jekyll server.

### Bước 2 - mở Firewall

Trước tiên, chúng ta kiểm tra firewall đã bật chưa. Nếu firewall đã mở, các bạn cần đảm bảo traffic được phép đến website server.
{: style="text-align: justify;"}

```terminal
$ sudo ufw status
```

Nếu status cho thấy `inactive`, chạy dòng lệnh sau:

```terminal
$ sudo ufw allow OpenSSH
$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
Status: active
To Action  From
-- ------  ----
OpenSSH ALLOW   Anywhere
OpenSSH (v6)   ALLOW   Anywhere (v6)
```

Mở port 4000 vì đây là port mặc định của jekyll server:

```terminal
$ sudo ufw allow 4000
Output
To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
4000                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
4000 (v6)                  ALLOW       Anywhere (v6)
```

## III. Thiết đặt Jekyll cùng GitHub Pages

Để tạo website trên host Github, các bạn tạo một repository có tên là `username.github.io`. URL của repository này nên có dạng `https://gitbub.com/username/username.github.io.git`. Trên máy tính cá nhân, các bạn có thể clone repository hoặc tạo một thư mục website bằng lệnh sau:
{: style="text-align: justify;"}

```terminal
$ jekyll new <repo-name>
$ cd <repo-name>
$ git init
```

Remote repository bạn vừa tạo trên Github vào thư mục trên máy bằng lệnh:

```terminal
$ git remote add origin https://github.com/username/username.github.io.git
```

Đến lúc này, bạn có thể lên mạng, chọn một theme Jekyll mà mình thấy ưng ý và tải về. Bạn có thể tải miễn phí các theme của Jekyll tại:
- [jamstackthemes.dev](https://jamstackthemes.dev)
- [jekyllthemes.org](http://jekyllthemes.org)
- [jekyllthemes.io](https://jekyllthemes.io)
- [jekyll-themes.com](https://jekyll-themes.com)
{: style="text-align: justify;"}

Sau khi tải theme về, copy tất cả các file của thêm vào thư mục repo bạn vừa tạo và đảm bảo rằng file Gemfile nằm trong thư mục. Cài đặt các plugin và dependencies được liệt kê trong Gemfile:
{: style="text-align: justify;"}

```terminal
$ cd <repo-name>
$ bundle install
```

Để host webserber trên máy, chạy các lệnh sau:

```terminal
$ bundle exec jekyll build
$ bundle exec jekyll serve
```

Webserver sẽ được host trên máy tại địa chỉ `http://127.0.0.1:4000/`.

## IV. Tùy chỉnh theme Jekyll

Thông thường các theme Jekyll sẽ có hướng dẫn để các bạn tùy chỉnh theo ý mình. Để tùy chỉnh sâu hơn hoặc tạo theme mới, các bạn có thể tìm hiểu thêm Sass, Javascript, Markdown.
{: style="text-align: justify;"}

## V. Host website lên Github và bào trì website

Sau khi hoàn tất các thiết đặt cũng như các bài viết, các bạn thêm tất cả các file muốn đưa lên trang web:
{: style="text-align: justify;"}

```terminal
$ git add .
```

Commit các file vào nhánh:

```temrinal
$ git commit -m "comments"
```

Đưa các file đã commit lên remote repository:

```terminal
$ git push origin master
```

Bây giờ truy cập repository trên Github và kiểm tra xem mọi thứ đã cập nhật chưa. Đợi vài phút và truy cập `https://username.github.io` và tận hưởng thành quả!

## VI. Tham khảo

- [https://www.creativebloq.com/how-to/jekyll-blog](https://www.creativebloq.com/how-to/jekyll-blog)
- [https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-20-04)
- [https://github.com/mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)
- [https://dev.to/wapenshaw/guide-setting-up-a-github-user-page-from-scratch-with-jekyll-7f5](https://dev.to/wapenshaw/guide-setting-up-a-github-user-page-from-scratch-with-jekyll-7f5)
- [http://www.stephaniehicks.com/githubPages_tutorial/pages/githubpages-jekyll.html](http://www.stephaniehicks.com/githubPages_tutorial/pages/githubpages-jekyll.html)
{: style="text-align: justify;"}

