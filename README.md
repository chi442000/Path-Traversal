# Path-Traversal
### Contents
- [Khái niệm Path-Traversal](https://github.com/chi442000/Path-Traversal#Concept)
- [Khai thác](https://github.com/chi442000/Path-Traversal#Exploit)
### Concept
- Path traversal( hay còn gọi là Directory traversal) là một lỗ hổng web cho phép kẻ tấn công đọc các file không mong muốn trên server. Nó dẫn đến việc bị lộ thông tin nhạy cảm của ứng dụng như `thông tin đăng nhập` , một số `file hoặc thư mục` của hệ điều hành. Trong một số trường hợp cũng có thể ghi vào các `files` trên server, cho phép kẻ tấn công có thể thay đổi dữ liệu hay thậm chí là `chiếm quyền điều khiển server`.
### Exploit
Một ứng dụng load ảnh như sau:

![image](https://github.com/chi442000/Path-Traversal/assets/84699930/07cef025-47cb-45fd-aaa4-850f5963b1e8)
Khi chúng ta gửi một request với một param `filename=test_image.png` thì sẽ trả về nội dung của file được chỉ định với tệp hình ảnh ở `/var/www/images/test_image.png`.
	 Ứng dụng không thực hiện việc phòng thủ cuộc tấn công `path traversal`, kẻ tấn công có thể thực hiện một yêu cầu tùy ý để có thể đọc các file trong hệ thống.
	 Ví dụ: 
  
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/ede019d0-3b80-4d56-99b9-cc65e6d39401)
  Khi đó ứng dụng sẽ đọc file với đường dẫn là `/var/www/images/../../../etc/passwd` với mỗi `../` là trở về thư mục cha của thư mục hiện tại. Như vậy với `../../../` thì từ thư mục `/var/www/images/` đã trở về thư mục gốc và file `/etc/passwd` chính là file được đọc.
	 Trên các hệ điều hành dựa trên Unix thì `/etc/passwd/` là một file chứa thông tin về các người dùng.
	Trên Windows thì có thể dùng cả hai `../` và `..\` để thực hiện việc tấn công này.
	 Đây là một trang web và chúng ta có thể xem các sản phẩm của họ dưới dạng hình ảnh. `ảnh`
	 Thử gửi một request đọc file `/etc/passwd` xem sao:
  
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/e7ca109c-203f-49b4-8aea-8ea492a7b32f)
  Và kết quả là chúng ta đã đọc được file `/etc/passwd`:
  
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/a40a3951-23ec-4391-8214-c4c2923becce)
  Thế nhưng chả có lý do gì mà các dụng không thực hiện việc phòng thủ các cuộc tấn công `path traversal` khi mà lỗ hổng này rất nghiệm trọng. Vậy làm sao để bypass?
	 Sau đây là một số cách bypass:
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/6a45e615-4720-4839-a040-73558fe130bd)
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/2245106b-5825-46c4-8bd9-b6cb363f3e33)
  Ta thử với `../../../etc/passwd` thì bị chặn, ta có thể thử bằng cách dùng đường dẫn tuyệt đối như `/etc/passwd`
  
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/1331070e-ed85-4e76-9291-af0686ea2aa9)
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/c6e12ec7-f512-4ece-80fa-321cc2f31591)
   Như vậy là param đã nhận đường dẫn tuyệt đối và chúng ta có thể đọc được file `/etc/passwd`
	 Hay một cách khác bạn có thể bypass bằng cách sử dụng `../` lồng nhau giống như trong trường hợp này:
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/57aa17ae-303f-4883-93c8-3dfc67f8c4bd)
  Một cách khác nữa chúng ta có thể encode hoặc null byte để vượt qua filter input
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/1d9c2bf1-59fc-4a41-8a01-bc7dbb78f7b7)
  ![image](https://github.com/chi442000/Path-Traversal/assets/84699930/8e8614c4-9cd9-4011-a4f9-0781c07ed510)
 - Ngoài ra các bạn có thể sử dụng
    
    - utf-8 unicode encoding
        - . = %u002e
        - / = %c0%af, %e0%80%af, %c0%2f
        - \ = %c0%5c, %c0%80%5c
    - 16-bit unicode encoding
        - . = %u002e
        - / = %u2215
        - \ = %u2216







  




