# Chapter 1: MEMORY LAYOUT IN C         
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
### 2. Intialized Data Segment (DS).     
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
