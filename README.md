# Lesson 1: MEMORY LAYOUT IN C         
Khi chạy một chương trình C, nó sẽ tải vào bộ nhớ RAM của máy tính. Cụ thể quá trình diễn ra như sau:
- Chương trình C được biên dịch thành mã máy - đây là các chỉ thị mà bộ xử lý CPU có thể hiểu và thực thi.      
- Mã máy của chương trình được tải vào bộ nhớ RAM. Nó được phân bổ vào các vùng nhớ khác nhau.      
- Khi chương trình chạy CPU sẽ truy cập và thực hiện các chỉ thị trong vùng code được tải trong RAM.        
## Memory Layout
![](https://scaler.com/topics/images/Diagram-for-memory-structure-of-C.webp)
Memory Layout của một chương trình C/C++ gồm 5 phần chính: Text/Code Segment, Intialized Data Segment (Vùng nhớ hằng và Vùng nhớ lưu các biến toàn cục), Unintialized Data Segment, Heap và Stack.        

### 1. Text/Code Segment.
- Lưu trữ mã nguồn của chương trình    
    -- Toàn bộ mã lệnh, hàm, biến và các thành phần khác của chương trình C được lưu trữ ở phân vùng này.       
    -- Nó là vùng nhớ chỉ đọc, ko thể ghi/sửa trong quá trình chạy chương trình.        
- Thực thi mã nguồn     
    -- Khi chương trình chạy, bộ vi xử lý sẽ đọc và thực thi lần lượt các lệnh được lưu trữ trong Text/Code Segement.           
- Chia sẻ mã nguồn      
    -- Nếu có nhiều luồng (Threads) trong một chương trình, thay vì mỗi luồng khi chạy nó sẽ đưa mã nguồn vào RAM thì hệ điều hành cho phép chúng dùng chung vùng Text/Code Segment còn các phân vùng khác thì sẽ tạo ra riêng.     
    -- Khi nhiều luồng cùng sử dụng chung một chương trình, Text/Code Segment sẽ chỉ được nạp vào bộ nhớ một lần.       
### 2. Intialized Data Segment (Data).     
Phần này có thể chia nhỏ ra làm 2 phần: Vùng nhớ hằng (Ko thay đổi được giá trị hay là vùng nhớ Read only) và vùng nhớ lưu các biến toàn cục.       
#### 2.1 Vùng nhớ lưu trữ biến toàn cục.    
* Vùng nhớ này chuyên lưu trữ các biến toàn cục, các biến Static đã được khai báo giá trị (≠0).
* Các biến này sẽ tồn tại trong cả quá trình chương trình chạy và có thể đọc và ghi được.       
```cpp
#include<stdio.h>

int x = 5;                  // Intialized Data Segment
int arr[3] = {1, 2, 3};     // Intialized Data Segment

void cout(){
    static int cnt = 0;     // Intialized Data Segment
    cnt++;
}

int main(){
    static int y = 1;       // Intialized Data Segment

    return 0;
}
```
#### 2.2 Vùng nhớ hằng.      
- Đây là vùng nhớ chỉ cho phép đọc và ghi vào lần đầu, sau đó giá trị được ghi sẽ tồn tại trong suốt quá trình mà ko thể thay đổi giá trị. 
- Nếu cố tình thay đổi sẽ sinh ra lỗi.      
```cpp
#include<stdio.h>

const int x = 15;                      // Lưu vào vùng nhớ hằng
const int arr[5] = {1, 2, 3, 4, 5};    // Lưu vào vùng nhớ hằng

int main(){
    char *chuoi = "hello"; // Lưu vào vùng nhớ hằng

    int x = 6;          // Error
    int arr[0] = 10;    // Error

    return 0;
}
```

### 3. Unintialized Data Segment (BSS).
* Vùng nhớ chuyên lưu trữ các biến toàn cục, biến Static chưa được khai báo giá trị.      
* Trong quá trình thực thi, các biến này sẽ khởi tạo giá trị 0 mặc định.
* Các biến này sẽ tồn tại trong cả quá trình chương trình chạy và có thể đọc và ghi được.
```cpp
#include<stdio.h>

int x;                  // Unintialized Data Segment
int arr[3];             // Unintialized Data Segment

void cout(){
    static int cnt;     // Unintialized Data Segment
    cnt++;
}

int main(){
    static int y;       // Unintialized Data Segment

    return 0;
}
```
### 4. Stack.  
- Vùng nhớ được cấp phát tự động.     
- Lưu trữ dữ liệu tạm thời.                
  -- Stack để lưu các biến cục bộ và tham số truyền vào của các hàm.                      
  -- Khi hàm được gọi, các biến và tham số của hàm sẽ đẩy (push) vào Stack.                 
  -- Khi hàm kết thúc, các biến và tham số của hàm sẽ được lấy ra (pop) khỏi Stack.            
- LIFO (thực hiện theo cơ chế Last In Firt Out).  
- Trong Stack, có thể thay đổi được giá trị thông con trỏ.      
- Kích thướt:            
  -- Có kích thướt cố định khoảng vài MB.            
  -- Nếu vượt quá sẽ sinh lỗi Stack Overflow.      
### 5. Heap
- Vùng nhớ cấp phát bộ nhớ động.
- Quản lí cấp phát bộ nhớ                
  -- Dùng các hàm malloc(), calloc() để lấy vùng nhớ và giải phóng vùng nhớ free().            
  -- Khi cấp phát bộ nhớ thì vùng nhớ đó sẽ trả về địa chỉ.            
- Kích thướt                
  -- Ko cố định.            
  -- Vùng nhớ này sẽ phình ra mỗi lần cấp phát, nếu ta sử dụng mà ko giải phóng (mượn ko trả) thì sẽ gây ra hiện tượng rò rỉ bộ nhớ.            

__________________________________________________________________________________________________________________________________________________________________________

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

__________________________________________________________________________________________________________________________________________________________________________

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

__________________________________________________________________________________________________________________________________________________________________________


# Lesson 4: C FILE STRUCTURE. 

**C File Structure - Cấu trúc file C**     
```
project/
├── Header/
│   ├── module1.h
│   └── module2.h
├── Source/
│   ├── module1.c
│   ├── module2.c
│   └── main.c
├── Makefile
└── README.md
```      
Trong Project này:      
-- Header/ chứa các tệp tiêu đề . 
* Tệp tiêu đề (Header file) là file có đuôi là **.h** chứa các khai báo hàm và các định nghĩa kiểu dữ liệu mà bạn muốn sử dụng trong nhiều tệp nguồn **.c** .     
Giúp tách biệt phần khai báo và định nghĩa, làm cho mã nguồn dễ quản lí và bảo trì hơn.  

-- Source/ chứa các tệp nguồn.   
* Tệp nguồn (Source file) là file có duôi là **.c** chứa các định nghĩa hàm.    
Là nơi thực sự triển khai các logic của chương trình.     

-- Makefile chứa các hướng dẫn để biên dịch và liên kết chương trình.                
-- README.md chứa thông tin về dự án.   

### 1. Hướng dẫn viết Header File trong lập trình C.
**Cấu trúc của Header File**.      
```cpp 
#ifndef HEADER_FILE
#define HEADER_FILE

//Nội dung header file

#endif
```    
**Cấu trúc của Source File**.
```cpp
#include "header_file"

//Nội dung Header file
``` 
**Cấu trúc main.c**
```cpp
#include "header_file"

int main(){
    //Nội dung main.c

    return 0;
}
```

**Ví dụ:**    
```cpp
// calculation.h

#ifndef LED_H
#define LED_H

int Cong(int a, int b);
int Nhan(int a, int b);

#endif
```

```cpp
// calculation.c

#include "calculation.h"
#include <stdio.h>

void Cong(int a, int b){
    printf("Tổng 2 số là: %d\n", a + b);
}

int Nhan(int a, int b){
    return a * b;
}
```

```cpp
// main.c

#include <stdio.h>
#include "calculation.h"

int main(){
    int a = 3, b = 10;

    Cong(a, b);

    int result = Nhan(a, b);


    return 0;
}
```

### 2. Những lưu ý.      
#### **a. Header File.**     
**1. Sử dụng bảo vệ tệp tiêu đề.**      
Sử dụng macro bảo vệ để ngăn tệp tiêu đề bị bao gồm nhiều lần, điều này có thể gây lỗi biên dịch.   

```cpp
#ifndef HEADER_FILE_H
#define HEADER_FILE_H

// Nội dung tệp tiêu đề

#endif // HEADER_FILE_H
```     
**2. Định nghĩa, không phải triển khai.**       
Tệp tiêu đề nên chứa các khai báo hàm, định nghĩa kiểu dữ liệu (struct, enum, typedef), và hằng số nhưng không chứa triển khai hàm.

```cpp
#ifndef MATH_OPERATIONS_H
#define MATH_OPERATIONS_H

#define PI 3.14159

typedef struct {
    int x;
    int y;
} Point;

int add(int a, int b);
int subtract(int a, int b);

#endif // MATH_OPERATIONS_H
```  
**3. Tránh khai báo biến toàn cục.**       
Tránh khai báo biến toàn cục trực tiếp trong tệp tiêu đề. Thay vào đó, sử dụng từ khóa **extern** để khai báo trong tệp tiêu đề và định nghĩa trong tệp nguồn.

```cpp
// math_operations.h
#ifndef MATH_OPERATIONS_H
#define MATH_OPERATIONS_H

extern int global_variable;

#endif // MATH_OPERATIONS_H


// math_operations.c
#include "math_operations.h"

int global_variable = 0;
``` 

#### **b. Source File.**     
**1. Bao gồm tệp tiêu đề tương ứng trước tiên.**       
Bao gồm tệp tiêu đề tương ứng trước tiên để đảm bảo rằng các khai báo hàm và biến được xác định.

```cpp
#include "math_operations.h"
#include <stdio.h>
``` 
**2. Triển khai hàm.**  
```cpp
int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}
```
**3. Sử dụng các biến toàn cục một cách cẩn thận.**         
Nếu cần sử dụng biến toàn cục, hãy đảm bảo rằng chúng được khai báo extern trong tệp tiêu đề và định nghĩa trong tệp nguồn.
```cpp
#include "math_operations.h"

int global_variable = 0;
```      


     
