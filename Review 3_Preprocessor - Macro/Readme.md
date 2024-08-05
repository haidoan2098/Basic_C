# Lesson 3: PREPROCESSOR DIRECTIVES. 

## Preprocess Directives - Chỉ thị tiền xử lý
Chỉ thị tiền xử lý là những chỉ thị cung cấp cho trình biên dịch để xử lí những thông tin trước khi bắt đầu quá trình biên dịch.         
Tất cả các chỉ thị tiền xử lý đều bắt đầu với # và các chỉ thị tiền xử lý không phải là lệnh C/C++ vì vậy không có dấu ; khi kết thúc.       
Chỉ thị tiền xử lý gồm 3 nhóm chính:
- Chỉ thị bao hàm tệp (#include).       
- Chỉ thị tạo/hủy định nghĩa Macro (#define, #undef).       
- Chỉ thị biên dịch có điều kiện (#if, #elif, #else, #endif, #ifdef, #ifndef).       
### 1. Chỉ thị bao hàm tệp (#include).      
-- #include cho phép thêm nội dung của file khác vào file đang viết.        
-- Thường thì đó là những Header file hay gọi là thư viện (.h).     
-- Header file gồm 2 nhóm với 2 mục đích khác nhau:        
- File hệ thống (System header file).       
- File người dùng (Your owner header file).  

Có 2 cấu trúc khai báo file header tương ứng với 2 nhóm thư viện:
```cpp
#include<stdlib.h> //Những thư viện chuẩn có sẵn từ hệ thống
```   

```cpp
#include "BUTTON.h" //Những thư viện tự định nghĩa

//Trình biên dịch sẽ tìm kiếm tệp tin này trong thư mục hiện tại và thư mục được chỉ định
```
![](https://i.imgur.com/mM70wbZ.jpeg)

### 2. Chỉ thị tạo/hủy định nghĩa Macro (#define, #undef).      
**2.1. Chỉ thị #define.**      
-- Chỉ thị #define là một từ khóa cho phép tạo ra các Macro (Macro là một tên được định nghĩa để thực hiện một chức năng nào đấy).     
-- Khi trình biên dịch gặp Macro nó sẽ thay thế Macro bằng chức năng đã định nghĩa.     
-- Macro có 2 loại chính:
- Oject-like macro.     
- Function-like macro.  

```cpp
#define new_name_use name_want_swap
```

**a. Oject-like macro - Macro như đối tượng.**
```cpp
#define PI 3.14
#define ARRAY_SIZE 100
#define MY_NAME HAIDOAN
```     
**b. Function-like macro - Macro như hàm.**     
```cpp
#define MAX(a, b) ((a) > (b) ? (a) : (b))

#define SUM(x, y) ((x) + (y))
```
```cpp
//Ta có thể viết Macro trên nhiều dòng bằng \

#include <stdio.h>

#define SUM(a, b)                               \
    printf("This is macro to sum 2 numbers\n"); \
    printf("Result is: %d", a+b);   

int main(){
    SUM(5, 6);

    return 0;
}        
```

**2.2. Chỉ thị #undef.**        
Chỉ thị #undef dùng để hủy bỏ định nghĩa trước đó của nó, kể từ dòng lệnh chứa #undef về sau.        
```cpp
#define PI 3.14
#undef PI

#define SUM(a, b) ((a) + (b))
#undef SUM
```
### 3. Chỉ thị biên dịch có điều kiện (#if, #elif, #else, #endif, #ifdef, #ifndef).    
**3.1. #if, #elif, #else, #endif.**        
Tương tự với các lệnh if, else if, else mà ta thường sử dụng, các chỉ thị điều kiện có chức năng tương đồng. 
* Mỗi chỉ thị điều kiện #if (kể cả ifdef hay ifndef) thì phải ứng với lệnh đóng điều kiện #endif.
```c
#include <stdio.h>

#define STM32   1
#define ESP32   2
#define ARDUINO 3

#define MCU STM32

int main(){
    #if (MCU == STM32)
        printf("STM32\n");
    #elif (MCU == ESP32)
        printf("ESP32\n");
    #else (MCU == ARDUINO)
        printf("ARDUINO\n");
    #endif
}
```
**3.2. #ifdef, #ifndef.**           
```cpp
#ifdef: Nếu đã được định nghĩa.     
#ifndef: Nếu chưa được định nghĩa.
```
* Ứng dụng phổ biến là ngăn chặn sự trùng lặp của cùng một header file khi được gọi nhiều lần trong cùng project.     
```cpp
#ifndef MY_HEADER_H
#define MY_HEADER_H
    //Nội dung Header file
#endif 
```

       

