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