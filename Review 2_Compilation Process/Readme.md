# Lesson 2: COMPILATION PROCESS . 

**Làm thế nào để chúng ta chạy và biên dịch một chương trình C ?**    
-> Step 1: Tạo file Source C.     
-> Step 2: Sử dụng GCC để biên dịch.             
```
$ gcc filename.c -o filename 

// -o để chỉ định tên tệp đầu ra.  
```     
-> Step 3: Thực thi chương trình.        
```
$ ./filename
```             
### 1. Biên dịch là gì ?
Biên dịch là quá trình chuyển từ ngôn ngữ C sang ngôn ngữ máy. Vì C là ngôn ngữ bậc trung nên cần một quá trình dịch ngôn ngữ C sang ngôn ngữ máy có thể hiểu được.     

### 2. Quá trình biên dịch diễn ra như thế nào ?             
![](https://tapit.vn/wp-content/uploads/2017/07/GCC_CompilationProcess.png)       

Có 4 giai đoạn để một chương trình C được thực thi: 
- Giai đoạn tiền xử lý (Pre-processor).  
- Giai đoạn dịch NNBC sang Asembly (Compiler).      
- Giai đoạn dịch Assembly sang ngôn ngữ máy (Assembler).     
- Giai đoạn liên kết (Linker).            
#### a. Giai đoạn tiền xử lý - Preprocessor.     
- Xử lý các chỉ thị tiền xử lý.     
-- #include: Thay thế các chỉ thị #include bằng nội dung của file header.       
-- #define: Thay thế các macro định nghĩa bằng giá trị tương ứng.       
-- #ifdef, #ifndef, #else, #endif: Xử lý các chỉ thị điều kiện để bao gồm hoặc loại bỏ đoạn mã.        
- Xóa bỏ các Comments, chú thích.                

```
$ gcc -E filename.c -o filename.i
```         
==> Giai đoạn này tạo ra filename.i.

#### b. Giai đoạn dịch NNBC sang Assembly - Compiler.
- Phân tích cú pháp của NNBC.      
- Tối ưu hóa (loại bỏ đoạn mã thừa,...)
- Chuyển sang dạng mã Assembly là ngôn ngữ bậc thấp (hợp ngữ) gần với tập lệnh của bộ vi xử lý.

```
$ gcc -S filename.i -o filename.s
```   
==> Giai đoạn này tạo ra filename.s.

#### c. Giai đoạn dịch Assembly sang ngôn ngữ máy - Assembler.      
- Dịch chương trình sang mã máy (0 và 1).
- Tạo ra tệp mã máy (.o) sinh ra trong hệ thống.     

```
$ gcc -c filename.s -o filename.o
```   
==> Giai đoạn này tạo ra filename.o.        

#### d. Giai đoạn liên kết - Linker.      
- Trong giai đoạn này mã máy của một chương trình sẽ được dịch từ nhiều nguồn (file .c, .h, lib) được liên kết lại với nhau tạo thành file duy nhất.       
- Linker sẽ tìm kiếm và liên kết với các tham chiếu bên ngoài như các hàm, biến được định nghĩa ở nơi khác. Nếu tham chiếu tới thì mã máy của các hàm, biến được gọi cũng được đưa vào chương trình cuối giai đoạn này.       
-> Chính vì vậy, các lỗi liên quan tới việc gọi hàm hay sử dụng biến tổng thể mà không tồn tại sẽ bị phát hiện. Kể cả lỗi viết chương trình chính không có hàm main() cũng được phát hiện trong liên kết.        

```
$ gcc filename.o -o filename
``` 
==> Kết thúc quá trình, tất cả các đối tượng được liên kết lại với nhau thành một chương trình thống nhất filename.exe.


     
