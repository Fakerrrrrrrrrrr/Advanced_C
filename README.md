# **Bài 1: Compiler - Marco**

<details>
<summary> Details </summary>

## 1. Compiler
<details>
<summary> Details </summary>

**Compiler** (trình biên dịch) là một phần mềm được sử dụng để chuyển đổi một ngôn ngữ lập trình cấp cao (high-level programming language) như C, C++, Java, Python, v.v. thành mã máy (machine code) có thể được thực thi trực tiếp bởi bộ xử lý (CPU) của máy tính.

![Compiler Image](https://github.com/Fakerrrrrrrrrrr/Advanced_C/blob/main/Images/Complier.png)

Chuyển đổi file main.c sang file main.i (Bước tiền xử lý) (Bước Preprocessor)
> gcc -E main.c -o main.i

Chuyển đổi file main.i sang file main.s (Chương trình mã nguồn của assembly) (Bước Compiler) (Chủ yếu lập trình liên quan đến hệ điều hành) (RTOS)
> gcc main.i -S -o main.s

Chuyển đổi file main.s sang main.o (File Hex) (Bước Assembler (Phần bên phải là địa chỉ, phần bên trái là giá trị của địa chỉ đó) (Nó sẽ ghi vào bộ nhớ Flash) khi tắt nguồn đi nó vẫn không bị mất dữ liệu, copy tất cả chương trình ở bộ nhớ Flash vào RAM rồi mới thực thi chương trình trên RAM. (Học RTOS tìm hiểu sau tra keyword booting process tìm hiểu, câu hỏi phỏng vấn)
> gcc - c main.s -o main.o

File main.o đã là chương trình của mình nhưng vẫn chưa chạy được trên hệ điều hành của mình vì nó chỉ chạy trên con vi điều khiển (Bước linker) (Khi viết chương trình sẽ có nhiều file như file Head, file main, file library,...)
> gcc test1.o test2.o main.o -o main<br>
> ./main

Quá trình biên dịch thường bao gồm các bước sau:

1. Quét mã nguồn (Lexical Analysis): Trình biên dịch sẽ đọc mã nguồn, phân tích các từ khóa, toán tử, tên biến, hằng số, v.v. và tạo ra một chuỗi các token.
2. Phân tích cú pháp (Syntax Analysis): Trình biên dịch sẽ kiểm tra tính hợp lệ của các token theo quy tắc cú pháp của ngôn ngữ lập trình và tạo ra cây cú pháp (parse tree).
3. Phân tích ngữ nghĩa (Semantic Analysis): Trình biên dịch sẽ kiểm tra tính hợp lệ về ngữ nghĩa của chương trình, chẳng hạn như kiểu dữ liệu, phạm vi biến, v.v.
4. Tối ưu hóa (Optimization): Trình biên dịch sẽ tối ưu hóa mã nguồn để tạo ra mã máy hiệu quả hơn.
5. Tạo mã (Code Generation): Cuối cùng, trình biên dịch sẽ tạo ra mã máy tương ứng với chương trình nguồn.

Các ưu điểm chính của việc sử dụng trình biên dịch bao gồm:

- Tăng tốc độ thực thi chương trình.
- Tạo ra mã máy có thể được thực thi trực tiếp trên phần cứng.
- Kiểm tra và phát hiện lỗi trong quá trình biên dịch.
- Tối ưu hóa mã nguồn.
- Trình biên dịch là một thành phần cơ bản và quan trọng trong quá trình phát triển phần mềm, đặc biệt là đối với các ngôn ngữ lập trình cấp cao.
</details>

## 2. Macro
<details>
<summary> Details </summary>

**Macro** là một tính năng trong lập trình cho phép người lập trình định nghĩa một tập hợp các lệnh hoặc quy tắc được đặt tên và có thể được sử dụng nhiều lần trong chương trình.

Khi gặp một macro trong chương trình, trình biên dịch sẽ thay thế macro bằng đoạn mã tương ứng trước khi chuyển đổi chương trình sang mã máy. Macro xảy ra ở giai đoạn tiền xử lý.

Macro có thể được sử dụng để:

1. Thay thế văn bản: Thay thế một đoạn văn bản bằng một đoạn khác.
2. Thực hiện tính toán: Thực hiện các phép tính toán tại thời điểm biên dịch.
3. Tạo mã lập trình động: Tạo ra các đoạn mã mới dựa trên các tham số đầu vào.
4. Tái sử dụng mã: Tái sử dụng các đoạn mã thường xuyên được sử dụng.

Định nghĩa Macro function nối bằng dấu \.
```cpp
#include <stdio.h>

#define CREATE(name,cmd)    \
void name(){                \
    printf(cmd);            \
}

CREATE (test1, "OK")

int main(){
    test1(); //OK
    return 0;
}
```
So sánh Macro với Function:
- Đối với Macro thì tốc độ xử lý nhanh hơn so với function bởi vì khi gọi function thì máy tính phải cấp 1 bộ nhớ tạm thời cho function ở thanh ghi trong khi đó Macro thì không cần thực hiện hành động này giúp tiết kiệm thời gian.
- Tuy nhiên, nhược điểm của Marco đó chính phần vùng nhớ cố định sẽ được cấp cho Macro nên kích thước của chương trình sẽ lớn hơn khi dùng Macro so với function.

Ứng dụng nối chuỗi tên biến trong Macro:
```cpp
#define CREATE(name)    \
int int_##name;         \
double double_##name;   \
char char_##char;

CREATE(test) //int int_test; double double_test; char char_test;
```
Chuyển hóa đoạn văn bản thành chuỗi
```cpp
#include <stdio.h>
#define CREATE(cmd) printf(#cmd);
int main(){
    CREATE(okdesu) //okdesu
    return 0;
}
```
Trong trường hợp không biết số lượng tham số không xác định trước ở Macro thì chúng ta dùng Variadic Macro.
```cpp
#Define PRINT_MENU_ITEM(number,item) printf("%d,%s\n",number,item);
#Define PRINT_MENU(...)                        \
    const char* item[] = {_VA_ARGS_};          \
    int n = sizeof(items)/sizeof(items[0]);    \
    for (int i = 0; i < n; i++ ) {             \
        PRINT_MENU_ITEM(i+1,items[i])          \
    }
```
Giải thích:
%d là một specifier(định dạng) dùng để in ra một số nguyên.<br>
%f là một specifier dùng để in ra một số thực trong dạng dấu phẩy động.<br>
%s sẽ được thya thế b ởi giá trị của biến/ biểu thức chuỗi ký tự tương ứng.<br>
__VA_ARGS_ đại diện cho tất cả các tham số được truyền vào khi Macro được gọi.

Để bỏ định nghĩa Macro để định nghĩa lại lần nữa vì trong source code không được định nghĩa Macro lần thứ 2:
> #undef <tên_Macro>

Để kiểm tra xem Macro đã được định nghĩa hay chưa nếu chưa thì đoạn code được sử dụng ta dùng if not define.<br>
Điều này rất quan trọng để tạo ra những thư viện.
```cpp
#ifndef _LIB_MACO_
#define _LIB_MACO_
    {code}
#endif
```
Ví dụ về ứng dụng ifndef:
```cpp
#Define STM32 0
#Define PFC 1
#Define ATMEGA 2

#Define MCU STM32
typedef enum{
    PIN0,
    PIN1,
    PIN2,
    PIN3,
    PIN4,
    PIN5,
    PIN6,
    PIN7
}Pins;

typedef enum{
    LOW,
    HIGH
} Status;

#ifndef MCU == STM32
    {code}
#elif MCU == ATMEGA
    {code}
#endif
```
</details>

</details>

# Bài 2: STDARG - ASSERT

<details>
<summary> Details </summary>

## 1. STDARG

<details>
<summary> Details </summary>
Thư viện stdarg trong C là một thư viện tiêu chuẩn được sử dụng để xử lý các tham số không xác định số lượng (tham số đầu vào) của các hàm. Thư viện này cung cấp một số macro và hàm cho phép xử lý các tham số không xác định số lượng.

Các macro và hàm chính trong thư viện stdarg bao gồm:

1. va_list: Đây là một kiểu dữ liệu được sử dụng để lưu trữ thông tin về các tham số không xác định số lượng. (Sử dụng typedef để định nghĩa lại, bản chất nó giống như 1 con trỏ lưu kiểu dữ liệu: typedef char* va_list;)
2. va_start(va_list ap, last_named_arg): Macro này khởi tạo va_list bằng cách lấy địa chỉ của tham số cuối cùng được đặt tên. (Bản chất của nó là 1 cái Macro: #define va_start(args, temp)) Ví dụ: ta có 1 chuỗi char* = {5,1,3,4,5,6} thì last_named_arg được nhập bằng 5, nó sẽ tạo ra con trỏ so sánh vị trí đầu tiên xem giá trị có bằng với chuỗi last_named_arg này không nếu không thì bỏ qua so sánh tiếp vị trí tiếp theo, nếu bằng thì nó xác định cái dấu ... thì sẽ bắt đầu từ đoạn vị trí tiếp theo trở đi. Tạo ra mã ký tự khác để lưu mảng còn lại. Khi 
3. va_arg(va_list ap, type): Macro này lấy giá trị của tham số tiếp theo từ va_list và chuyển đổi nó sang kiểu dữ liệu được chỉ định. type được hiểu là ép kiểu dữ liệu. Khi gọi hàm va_arg(va_list ap, type) thì nó đọc giá trị tại ô phía sau va_start và trỏ tới ô tiếp theo.
4. va_end(va_list ap): Macro này thực hiện dọn dẹp và hoàn thành việc sử dụng va_list.

Code:
```cpp
#include <stdio.h>
#include <stdarg.h>

void display(int count, ...) {
    va_list args;
    va_start(args, count);
   

    for (int i = 0; i < count; i++) {
        printf("Value at %d: %d\n", i, va_arg(args,int)); 
    }

    va_end(args);
}

int main()
{
    display(5, 5, 8, 15, 10, 13);

    return 0;
}
```
Output:
```cpp
Value at 0: 5
Value at 1: 8
Value at 2: 15
Value at 3: 10
Value at 4: 13
```
Thư viện stdarg là một công cụ hữu ích khi cần xử lý các tham số không xác định số lượng, chẳng hạn như trong các hàm printf(), scanf(), hoặc các hàm tự định nghĩa khác.<br>
Có thể ép kiểu của struct:
```cpp
#include <stdio.h>
#include <stdarg.h>

typedef struct Data
{
    int x;
    double y;
} Data;

void display(int count, ...) {
    va_list args;
    va_start(args, count);
    int result = 0;
    for (int i = 0; i < count; i++)
    {
        Data tmp = va_arg(args,Data);
        printf("Data.x at %d is: %d\n", i,tmp.x);
        printf("Data.y at %d is: %f\n", i,tmp.y);
    }
    va_end(args);
}

int main() {
    display(3, (Data){2,5.0} , (Data){10,57.0}, (Data){29,36.0});
    return 0;
}
```
</details>

## 2. Assert
<details>
<summary> Details </summary>
    
Khi phát triển project lớn có rất nhiều file, bình thường Debug bằng If Else do chương trình nhỏ nên khó phân biệt ở đoạn nào, khi đó ta dùng Assert Lib để Debug.
> Syntax: assert(condition && #cmd);

Nếu điều kiện đúng (true), không có gì xảy ra và chương trình tiếp tục thực thi. Nếu điều kiện sai (false), chương trình dừng lại và thông báo một thông điệp lỗi. Dùng trong debug, dùng #define NDEBUG để tắt debug

```cpp
#include <stdio.h>
#include <assert.h>

// Macro dùng để debug
#define LOG(condition, cmd) assert(condition && #cmd)

int main() {
    int x = 5;

    LOG(x == 5, "x khac 5");

    // Chương trình sẽ tiếp tục thực thi nếu điều kiện là đúng.
    printf("X is: %d", x);
    return 0;
}
```
</details>

</details>

# **Bài 3: Pointer**

<details>
<summary> Details </summary>

## Bài tập về nhà

<details>
<summary> Details </summary>
    
> Kích thước của con trỏ

Kích thước của con trỏ phụ thuộc vào kiến trúc máy tính (32-bit hay 64-bit) và kiểu dữ liệu mà con trỏ đang trỏ đến.

Hầu hết ở các kiểu dữ liệu thì kích cỡ của con trỏ thường là **4** bytes (32 bit) trên hệ thống 32-bit và **8** bytes (64 bit) trên hệ thống 64-bit.

Tuy nhiên ở kiểu dữ liệu **double** là **8** bytes (64 bit), kiểu dữ liệu **long long** là **8** bytes (64 bit), kiểu dữ liệu **bool** là **1** bytes (8 bit) trên cả hệ thống 32-bit và 64-bit.

**Input**
```cpp
#include <stdio.h>

int main() {
    printf("%zu bytes\n", sizeof(int *));
    printf("%zu bytes\n", sizeof(char *));
    printf("%zu bytes\n", sizeof(float *));
    printf("%zu bytes\n", sizeof(double *));
    printf("%zu bytes\n", sizeof(long *));
    printf("%zu bytes\n", sizeof(short *));
    printf("%zu bytes\n", sizeof(long long *));
    printf("%zu bytes\n", sizeof(bool *));
    printf("%zu bytes\n", sizeof(struct *));
    printf("%zu bytes\n", sizeof(union *));
    printf("%zu bytes\n", sizeof(enum *));

    return 0;
}
```
**Output**
```cpp
4 bytes
4 bytes
4 bytes
8 bytes
4 bytes
4 bytes
8 bytes
1 bytes
4 bytes
4 bytes
4 bytes
```

</details>

## 1. Pointer

<details>
<summary> Details </summary>
    
- Con trỏ (Pointer) là một biến lưu lại địa chỉ ô nhớ trong bộ nhớ máy tính.
> Cú pháp: <kiểu_dữ_liệu> *<tên_biến>;

Ví dụ: 
```cpp
int *ptr = 0x01;
```
- Để lấy được địa chỉ của biến được khai báo bình thường thì dùng toán tử &

Ví dụ: 
```cpp
int a = 10;
int *ptr = &a;
```
- Đối với 1 mảng để lấy địa chỉ của 1 mảng thì ta chỉ cần nhập tên của mảng đó

Ví dụ: 
```cpp
int array[] = {1,2,3,4,5};
int *ptr = array;
```
- Để lấy giá trị của con trỏ đó dùng toán tử *

Ví dụ 3 kết quả dưới đều tương đương nhau: 
```cpp
*0x01 = 10
*ptr = 10
*(&a) = 10
```

</details>
    
## 2. Function Pointer

<details>
<summary> Details </summary>
    
- Con trỏ hàm (Function Pointer) là một biến có thể lưu địa chỉ của hàm.
> Cú pháp: <kiểu_dữ_liệu_trả_về> (*<tên_con_trỏ>)(<các_tham_số>) = <tên_hàm>;

Ngoài ra con trỏ hàm cũng có thể làm tham số của 1.

Ví dụ: 
```cpp
#include <stdio.h>

void tong(int *a, int *b){
    printf("Tong %d va %d = %d\n", *a, *b, *a+*b);
}

void hieu(int *a, int *b){
    printf("Hieu %d va %d = %d\n", *a, *b, *a-*b);
}

void tich(int *a, int *b){
    printf("Tich %d va %d = %d\n", *a, *b, (*a)*(*b));
}

void thuong(int *a, int *b){
    printf("Thuong %d va %d = %.3f\n", *a, *b, *a/(float)(*b));
}

void TinhToan(int *a,int *b,void (*phepToan)(int *, int *)){
    phepToan(a,b);
}

int main(){
    int a = 7;
    int b = 5;
    void (*phepToan[])(int *, int *) = {tong, hieu, &tich, &thuong};

    phepToan[0](&a,&b);  //Tong 7 va 5 = 12
    phepToan[1](&a,&b);  //Hieu 7 va 5 = 2
    phepToan[2](&a,&b);  //Tich 7 va 5 = 35
    phepToan[3](&a,&b);  //Thuong 7 va 5 = 1.400

    TinhToan(&a,&b,&thuong);  //Thuong 7 va 5 = 1.400
}
```

</details>
    
## 3. Void Pointer

<details>
<summary> Details </summary>
    
Con trỏ void (void pointer) là một loại con trỏ đặc biệt, có thể trỏ đến bất kỳ kiểu dữ liệu nào, bao gồm cả các kiểu do người dùng định nghĩa.
> Cú pháp: void *<tên_con_trỏ>;

Có 3 đặc điểm của con trỏ void:
1. Có thể trỏ đến bất kỳ kiểu dữ liệu nào, không phụ thuộc vào kiểu.
2. Không thể trực tiếp truy cập/thay đổi giá trị mà nó trỏ đến, vì không xác định được kiểu dữ liệu.
3. Trước khi sử dụng, cần phải ép kiểu con trỏ void về kiểu dữ liệu đúng.

Ví dụ:
```cpp
int x = 10;
double y = 10.5;
char z = 'A';

void *myPointer;
int *intPointer;

myPointer = &x;
myPointer = &y;
myPointer = &z;

intPointer = (int*)myPointer;
```
</details>

## 4. Pointer to Constant

<details>
<summary> Details </summary>
    
Con trỏ hằng (Pointer to Constant) là không thể sử dụng con trỏ để thay đổi giá trị mà nó trỏ .
> Cú pháp: <kiểu_dữ_liệu> const *<tên_con_trỏ>;<br>           const <kiểu_dữ_liệu> *<tên_con_trỏ>;

Có 3 đặc điểm của con trỏ hằng:
1. Không thể sử dụng con trỏ để thay đổi giá trị mà nó trỏ đến (vì đó là hằng số).
2. Có thể gán địa chỉ của một biến có thể thay đổi cho con trỏ, nhưng không thể thay đổi giá trị của biến đó thông qua con trỏ.
3. Có thể gán địa chỉ của một hằng số cho con trỏ.

```cpp
int x = 10;
const int *p = &x; // Gán địa chỉ của x cho con trỏ p, nhưng không thể thay đổi giá trị của x thông qua p

*p = 20; // Lỗi, không thể thay đổi giá trị mà p trỏ đến vì đó là hằng số

const int y = 30;
p = &y; // Hợp lệ, có thể thay đổi địa chỉ mà p trỏ đến
```
</details>

## 5. Null Pointer

<details>
<summary> Details </summary>
    
Con trỏ Null (Null Pointer) là một con trỏ không trỏ đến bất kỳ vùng nhớ nào và có giá trị bằng 0.
Khi chúng ta khai báo bất kỳ
> Cú pháp: <kiểu_dữ_liệu> *<tên_con_trỏ> = NULL;

Khi khai báo 1 con trỏ bất kỳ nhưng không gán giá trị cho nó thì sẽ xảy ra trường hợp con trỏ được cấp phát vùng nhớ ngẫu nhiên. <br> Vì vậy nên gán biến con trỏ đó thành con trỏ NULL nếu chưa sử dụng ngay biến con trỏ đó.

Ví dụ:
```cpp
//Việc sử dụng null pointer có thể gây ra lỗi chương trình.
1. Khởi tạo: Khởi tạo một con trỏ null bằng cách gán giá trị 0 hoặc NULL cho nó:
int *ptr = NULL;
2. Kiểm tra: Trước khi sử dụng một con trỏ, nên kiểm tra xem nó có phải là null pointer
không bằng cách so sánh nó với NULL
if (ptr != NULL) {
    // Sử dụng con trỏ
} else {
    // Xử lý trường hợp con trỏ null
}
```
</details>

</details>
    
# Bài 4: Goto - setjmp.h

<details>
<summary> Details </summary>

## 1. Goto

<details>
<summary> Details </summary>
    
Goto có khá nhiều ứng dụng, khi code không khuyến khích sử dụng Goto vì nó thay đổi luồng chạy của chương trình chỉ dành cho những người Master ngôn ngữ lập trình mới kiểm soát luồng chạy của nó.
Khi viết chương trình có rất rất nhiều phức tạp thì chúng ta không thể Break hết tất cả hoặc chương trình bị lỗi ta không thể thoát ngay lập tức thì chúng ta sử dụng Goto.
> Syntax: goto label; //label là tên được sử dụng để xác định vị trí mà goto sẽ nhảy đến -> label:

Cách hoạt động: 
- Khi gặp lệnh goto label; chương trình sẽ ngay lập tức chuyển đến vị trí (trỏ tới vị trí) được xác định bởi label và tiếp tục thực hiện các câu lệnh tại đó. 
- Label cso thể được định nghĩa bất kỳ đâu trong chương trình, miễn là nó nằm trong cùng phạm vị với lệnh goto sử dụng nó.

```cpp
#include <stdio.h>

int main() {
    int x = 5;

    if (x > 0) {
        goto positive;
    } else {
        goto negative;
    }

positive:
    printf("x is positive\n");
    return 0;

negative:
    printf("x is negative\n");
    return 0;
}
```
Lưu ý:
- Việc sử dụng goto quá mức có thể dẫn đến mã nguồn trở nên khó đọc, khó bảo trì và khó tái sử dụng.
- Thay vì sử dụng goto, nên sử dụng các cấu trúc điều khiển như if-else, switch-case, while, for, do-while để đạt được cùng mục đích.
- Tuy nhiên, trong một số trường hợp đặc biệt, việc sử dụng goto vẫn có thể được chấp nhận, ví dụ như thoát khỏi vòng lặp khi gặp lỗi hoặc chuyển đến các khối xử lý lỗi.

**Con led Matrix**<br>
Quy tắc thiết kế led Matrix (Nếu muốn tạo led Matrix mà nối mỗi con led 1 chân thì rất nhiều chân) quét từng hàng mỗi hàng theo vòng lặp For i,j quét mỗi hàng xong tắt, nhưng vì mắt con người có kỹ thuật lưu ảnh trên giác mạt nên mới có thể nhìn thấy dù những con led này đã tắt, tốc độ rất là nhanh tầm 10ms

Trong chương trình thì sử dụng vòng lặp while -> tới switch -> tới for -> tới if -> Rồi mới tới for i,j để thoát ra thì phải break xong phải tạo điều kiện để vòng lặp trước đó break có rất nhiều và phức tạp. Bây giờ ta sẽ dùng Goto để thoát ra đến vị trí mình mong muốn.
```cpp
#include <stdio.h>

void delay()
{
    double start;
    while (start < 60000000)
    {
        start++;
    }
}

char letter = 'A';

char first_sentence[] = "HELLO";
char second_sentence[] = "FASHION SUIT";
char third_sentence[] = "SUITABLE PRICE";

int letter_A[8][8] = {  {0,0,1,0,0,0,0,0},
                        {0,1,0,1,0,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,1,1,1,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},  };

int letter_H[8][8] = {  {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,1,1,1,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},
                        {1,0,0,0,1,0,0,0},  };

int letter_L[8][8] = {  {1,0,0,0,0,0,0,0},
                        {1,0,0,0,0,0,0,0},
                        {1,0,0,0,0,0,0,0},
                        {1,0,0,0,0,0,0,0},
                        {1,0,0,0,0,0,0,0},
                        {1,0,0,0,0,0,0,0},
                        {1,0,0,0,0,0,0,0},
                        {1,1,1,1,1,0,0,0},  };


/*
H, e, l,o, F, a, ....
*/

int button = 0;

typedef enum
{
    FIRST,
    SECOND,
    THIRD
}   Sentence;

int main() {
    Sentence sentence = FIRST;
    while(1)
    {
        switch (sentence)
        {
        case FIRST:
            for (int index = 0; index < sizeof(first_sentence); index++)
            {
                if (first_sentence[index] == 'H')
                {
                    for (int i = 0; i < 8; i++) 
                    {    
                        for (int j = 0; j < 8; j++) 
                        {
                            if (letter_H[i][j] == 1) 
                            {
                                printf("Turn on led at [%d][%d]\n", i,j);
                                if (button == 1)
                                {
                                   goto exit_loops;
                                }
                                
                            }
                        }
                        // tat den
                    }
                }
                if (first_sentence[index] == 'e')
                {
                    // in ra chu e
                }
            }
            printf("first sentence is done\n");
            delay();
            goto logic;
        case SECOND:
            for (int index = 0; index < sizeof(second_sentence); index++)
            {
                if (second_sentence[index] == 'A')
                {
                    for (int i = 0; i < 8; i++) 
                    {    
                        for (int j = 0; j < 8; j++) 
                        {
                            if (letter_A[i][j] == 1) 
                            {
                                printf("Turn on led at [%d][%d]\n", i,j);
                                if (button == 1)
                                {
                                   goto exit_loops;
                                }
                                
                            }
                        }
                        // tat den led
                    }
                }
                if (second_sentence[index] == 'F')
                {
                    // in ra chu F
                }
            }
            printf("second sentence is done\n");
            delay();
            goto logic;
        case THIRD:
            for (int index = 0; index < sizeof(third_sentence); index++)
            {
                if (third_sentence[index] == 'L')
                {
                    for (int i = 0; i < 8; i++) 
                    {    
                        for (int j = 0; j < 8; j++) 
                        {
                            if (letter_L[i][j] == 1) 
                            {
                                printf("Turn on led at [%d][%d]\n", i,j);
                                if (button == 1)
                                {
                                   goto exit_loops;
                                }
                                
                            }
                        }
                        // tat den led
                    }
                }
                if (third_sentence[index] == 'E')
                {
                    // in ra chu H
                }
            }
            printf("third sentence is done\n");
            delay();
            //button = 1;
            goto logic;
        }
        logic:
            if (sentence == FIRST)
            {
                sentence = SECOND;
            }
            else if (sentence == SECOND)
            {
                sentence = THIRD;
            }
            else if (sentence == THIRD)
            {
                sentence = FIRST;
            }
            goto exit;
        exit_loops:
            printf("Stop!\n");
            break;
        exit:;      
    }
    return 0;
}

```
</details>

## 2. Setjmp

<details>
<summary> Details </summary>
    
setjmp là một hàm được sử dụng để lưu trữ trạng thái hiện tại của chương trình. Nó được sử dụng trong kết hợp với hàm longjmp để thực hiện nhảy trỏ tới vị trí khác trong chương trình.
> Syntax: int setjmp(jmp_buf buf);
> int exception_code = setjmp(buf); //Với buf đã được khai báo jmp_buf buf; trước đó rồi

Trong đó:
- jmp_buf là một kiểu dữ liệu được định nghĩa trong thư viện <setjmp.h> dùng để lưu trữ vị trí trong trường hợp nào đó.
- buf là một biến của kiểu jmp_buf được sử dụng để lưu vị trí đó.

Ở phần này cần lưu ý:
- setjmp(buf) == 0
- longjmp(buf,(x))

Setjmp có thể toàn cục trong khi Goto chỉ ở cục bộ nhảy từ function này sang function khác được. Giá trị đầu tiên khi gọi setjmp nó sẽ mặc định là 0. Nếu xảy ra lỗi muốn quay lại vị trí được lưu trữ bởi setjmp thì ta dùng hàm longjmp.
> Syntax: void longjmp(jmp_buf buf, int val);

Trong đó:
- buf là biến jmp_buf được sử dụng để lưu trữ vị trí.
- val là một giá trị được trả về bởi setjmp khi nó được gọi lại.

Khi gọi longjmp thì nó sẽ nhảy tới vị trí đoạn code đã được lưu trữ trước đó bởi setjmp và tiếp tục chạy chương trình từ vị trí đó với giá trị là val.

Dùng để xử lý ngoại lệ bằng define
```cpp
#define TRY if ((exception_code = setjmp(buf)) == 0) 
#define CATCH(x) else if (exception_code == (x)) 
#define THROW(x) longjmp(buf, (x))
```
Code ví dụ:
```cpp
#include <stdio.h>
#include <setjmp.h>

jmp_buf buf;
int exception_code;

#define TRY if ((exception_code = setjmp(buf)) == 0) 
#define CATCH(x) else if (exception_code == (x)) 
#define THROW(x) longjmp(buf, (x))

double divide(int a, int b) {
    if (b == 0) {
        THROW(1); // Mã lỗi 1 cho lỗi chia cho 0
    }
    return (double)a / b;
}

int main() {
    int a = 10;
    int b = 0;
    double result = 0.0;

    TRY {
        result = divide(a, b);
        printf("Result: %f\n", result);
    } CATCH(1) {
        printf("Error: Divide by 0!\n");
    }
    // Các xử lý khác của chương trình

    return 0;
}
```
</details>

</details>
    
# Bài 5: Extern - Static - Volatile - Register

<details>
<summary> Details </summary>
    
## 1. Extern

<details>
<summary> Details </summary>
    
Khi sử dụng keyword extern thì có nghĩa là chúng ta muốn lấy một biến hoặc một hàm ở một file khác trong chương trình.<br>
Ví dụ: Ta có 2 file main.c và file test.c. Biến ở file test.c được khai báo toàn cục 'int count = 20;' thì khi chúng ta dùng 'extern int count;' ở file main.c thì chúng ta hoàn toàn có thể sử dụng biến count và có giá trị là 20, có nghĩa là nếu count có địa chỉ 0x01 ở file test.c thì ở file main.c nó cũng có địa chỉ là 0x01.<br>
Extern cũng có thể sử dụng để khai báo các hàm ở file khác.<br>
Ví dụ: extern int calculatorDivide(int a, int b); ta lấy hàm calculatorDivide sử dụng mà không cần phải viết lại hàm đó.
> Syntax: extern datatype variable_name;<br>
> extern return_type function_name(parameter_list);

</details>
    
## 2. Static variable

<details>
<summary> Details </summary>
    
Khi khai báo biến static chương trình sẽ cấp phát cho nó 1 địa chỉ tồn tại hết vòng đời của chương trình (không bị thu hồi), biến static chỉ khởi tạo 1 lần và không khởi tạo lại lần nữa, nếu gặp biến static ở function được gọi nó sẽ bỏ qua và chạy dòng code tiếp theo.</br>
Giá trị của biến static chỉ có phạm vi cục bộ với file hoặc hàm chứa nó. Biến static sẽ được lưu trữ trong vùng nhớ data hoặc bss thay vì vùng nhớ stack như biến cục bộ thông thường.<br>
```cpp
void myFunction() {
    static int counter = 0;
    counter++;
    printf("Counter value: %d\n", counter);
}

int main() {
    myFunction(); // Output: Counter value: 1
    myFunction(); // Output: Counter value: 2
    myFunction(); // Output: Counter value: 3
    return 0;
}
```
Khi sử dụng static toàn cục (khai báo bên ngoài các hàm) (static hàm) thì hàm đó chỉ sử dụng được ở trong file đó. Nghĩa là hàm static có thể gọi bên trong file chứa nó, không thể gọi từ các file khác. Việc sử dụng static hàm giúp bạn ẩn các hàm hỗ trợ bên trong, không cho phép chúng được gọi từ bên ngoài.
```cpp
// file1.c
static int calculateSum(int a, int b) {
    return a + b;
}

int main() {
    int result = calculateSum(5, 3); // Có thể gọi hàm calculateSum() ở đây
    return 0;
}

// file2.c
int calculateDifference(int a, int b) {
    return a - b;
}

int main() {
    int result = calculateSum(5, 3); // Lỗi, hàm calculateSum() không thể gọi được ở đây
    return 0;
}
```

</details>
    
## 3. Register

<details>
<summary> Details </summary>
    
Test:
```cpp
#include <stdio.h>
#include <time.h>

int main() {
    // Lưu thời điểm bắt đầu
    clock_t start_time = clock();
    int i; //register int i;

    // Đoạn mã của chương trình
    for (i = 0; i < 2000000; ++i) {
        // Thực hiện một số công việc bất kỳ
    }

    // Lưu thời điểm kết thúc
    clock_t end_time = clock();

    // Tính thời gian chạy bằng miligiây
    double time_taken = ((double)(end_time - start_time)) / CLOCKS_PER_SEC;

    printf("Thoi gian chay cua chuong trinh: %f giay\n", time_taken);

    return 0;
}
```
Output
```cpp
Thoi gian chay cua chuong trinh: 0.002000 giay
```
Test lại đoạn code trên sửa lại đoạn code 'int i;' thành 'register int i;'
```cpp
Thoi gian chay cua chuong trinh: 0.000000 giay
```
Register là một từ khóa để khai báo biến nên được lưu trữ trong thanh ghi của bộ xử lý thay vì trong bộ nhớ chính. Việc lưu trữ biến trong thanh ghi thay vì bộ nhớ chính có thể giúp tăng tốc độ truy cập và xử lý dữ liệu. Tuy nhiên, số lượng thanh ghi thường bị giới hạn, nên không phải tất cả biến đều có thể được lưu trữ trong thanh ghi.

![Compiler Image](https://github.com/Fakerrrrrrrrrrr/Advanced_C/blob/main/Images/Register.png)

Khi khai báo 'int i = 5;' nó sẽ lưu 1 cái địa chỉ ở RAM và lưu giá trị bằng 5, lưu thông tin phép toán ở RAM, muốn tính toán sẽ đẩy thông tin tính toán qua register sau đó register sẽ đẩy thông tin tính toán qua ALU (Bộ xử lý trung tâm) ở ALU thực hiện tính toán sau đó đẩy kết quả về cho register sau đó register trả về kết quả lại cho RAM.<br>
Vậy nên khi sử dụng biến Register nó được lưu thông tin ở Register thay vì RAM vậy nên tính toán sẽ nhanh vì chỉ có bước đẩy thông tin qua ALU và ALU trả giá trị kết quả về lại Register mà không cần phải thông qua RAM.

</details>
    
## 4. Volatile

<details>
<summary> Details </summary>
    
Khi sử dụng biến volatile thì nó sẽ thông báo cho chương trình biên dịch không được phép tối ưu code của biến đó. Biến volatile thường được sử dụng để truy cập vào các vùng nhớ liên quan đến phần cứng, như các thanh ghi hoặc bộ nhớ được chia sẻ. Khi sử dụng volatile, trình biên dịch sẽ không tối ưu hóa code liên quan đến biến này, vì giá trị của biến có thể thay đổi bất kỳ lúc nào.<br>
Sau này khi học RTOS thì sử dụng nhiều luồng với nhau ta sẽ luôn luôn sử dụng Volatile bởi vì khi sử dụng ở luồng khác thì ta phải luôn luôn load lại biến đó.
```cpp
#include <stdio.h>

int main() {
    volatile int flag = 0; // Biến flag được khai báo là volatile

    // Một luồng (thread) khác có thể thay đổi giá trị của biến flag
    // mà không cần sự tương tác của luồng này
    while (flag == 0) {
        // Làm gì đó
    }

    printf("Kết thúc vòng lặp.\n");
    return 0;
}
```
</details>

</details>

# Bài 6: Bitmask

<details>
<summary> Details </summary>
    
## Ứng dụng:

<details>
<summary> Details </summary>
    
Trong đời sống có rất nhiều trường hợp sẽ rơi vào tình huống phải lựa chọn những trường hợp khác nhau, ví dụ như muốn đi từ Hà Nội tới Đà Nẵng thì phải có thể chọn phương tiện khác nhau để tới được Đà Nẵng như đi máy bay, xe bus, taxi, tàu lửa,... 
Nếu có nhiều sự lựa chọn như vậy thì ta lại phải khai báo từng biến khác nhau rất mất thời gian và tốn bộ nhớ vì mỗi biến sẽ có ít nhất 1byte. Vậy nên chúng ta dùng bitmask để lưu trữ và làm việc với các trạng thái hoặc cấu hình dưới dạng các bit trong một số nguyên.
Bitmask thường dùng để biểu diễn một tập hợp các lựa chọn hoặc thiết lập, trong đó mỗi bit tương ứng với một lựa chọn hoặc cấu hình.<br>
Ví dụ, nếu bạn muốn lưu trữ trạng thái của 8 tính năng khác nhau, sử dụng bitmask để lưu trữ với một số nguyên 8-bit(1 byte) cho mỗi tính năng:<br>
- Bit 0 (giá trị 1): Tính năng 1 được bật //00000001
- Bit 1 (giá trị 2): Tính năng 2 được bật //00000010
- Bit 3 (giá trị 3): Tính năng 3 được bật //00000100
- Và cứ thế<br>

</details>
    
## Ứng dụng các thao tác phổ biến với bitmask bao gồm:
<details>
<summary> Details </summary>
- Kiểm tra xem một bít có được bật hay không dùng toán tử (AND)(&), nó sẽ so sánh từng bit tương ứng của hai số và trả về 1 nếu cả hai bit đều bằng 1 (True), ngược lại nó sẽ trả về 0 (False)  <br>
Ví dụ: Nếu bitmask là 0b10101010 và muốn kiểm tra bit thứ 2 (tính từ bit 0), sử dụng bitmask 0b00000100. Kết quả của bitmask & 0b00000100 sẽ là 0b00000000, bit thứ 2 không được bật.<br>
- Bật một bit dùng toàn tử (OR)(|), nó sẽ so sánh từng bit tương ứng của hai số và trả về 1 nếu 1 trong hai có ít nhất 1 giá trị bằng 1 và trả về 0 nếu cả hai đều bằng 0 <br>
Ví dụ: Nếu bitmask là 0b10101010 và muốn bật bit thứ 2, sử dụng bitmask 0b00000100. Kết quả của bitmask | 0b00000100 sẽ là 0b10101110, như vậy đã bật bit thứ 2<br>
- Đảo ngược giá trị của mỗi bit dùng toán tử (NOT)(~), chuyển 0 thành 1 và chuyển 1 thành 0.<br>
Ví dụ: Nếu bitmask là 0b10101010 và muốn đảo ngược giá trị của mỗi bit, sử dụng ~bitmask, kết quả sẽ là 0b01010101. <br>
- Chuyển đổi trạng thái của một bit dùng toán tử (XOR)(^), nó sẽ so sánh từng bit tương ứng của hai số và trả về 1 nếu các bit khác nhau, ngược lại trả về 0 nếu hai bit giống nhau. <br>
Ví dụ: Nếu bitmask là 0b10101010 và muốn chuyển đổi trạng thái của bit thứ 2, sử dụng bitmask 0b00000100. Kết quả của bitmask ^ 0b00000100 se là 0b10101110,như vậy bit thứ 2 đã được chuyển đổi.<br>
- Tắt một bit sử dụng toán tử (AND NOT)(&~), nó kết hợp toán tử AND và NOT để tắt 1 bit cụ thể nào đó<br>
Ví dụ: Nếu bitmask là 0b10101010 và muốn tắt bit thứ 1, sử dụng bitmask 0b11111101. Kết quả của bitmask & 0b11111101 sẽ là 0b10101000.<br>
- Dịch bit sang phải sử dụng toán tử (>>), nó sẽ dịch sang phải một vị trí cố định và các bit bên trái sẽ mất đi và chuyển thành 0.<br>
Ví dụ: Nếu bitmask là 0b10101010 và muốn dịch sang phải 2 vị trí. Kết quả của bitmask >> 2 sẽ là 0b00101010.<br>
- Dịch bit sang trái sử dụng toán tử (<<), nó sẽ dịch sang trái một vị trí cố định và các bit bên phải sẽ mất đi và chuyển thành 0.<br>
Ví dụ: Nếu bitmask là 0b10101010 và muốn dịch sang trái 2 vị trí. Kết quả của bitmask << 2 sẽ là 0b00101000.<br>

**Code tạo thông số kỹ thuật của một chiếc xe bằng bitmask:**
```cpp
#include <stdio.h>
#include <stdint.h>
#define COLOR_RED 0
#define COLOR_BLUE 1
#define COLOR_BLACK 2
#define COLOR_WHITE 3
#define POWER_100HP 0
#define POWER_150HP 1
#define POWER_200HP 2
#define ENGINE_1_5L 0
#define ENGINE_2_0L 1

typedef uint8_t CarColor;
typedef uint8_t CarPower;
typedef uint8_t CarEngine;

#define SUNROOF_MASK 1 << 0     // 0001
#define PREMIUM_AUDIO_MASK 1 << 1 // 0010
#define SPORTS_PACKAGE_MASK 1 << 2 // 0100
// Thêm các bit masks khác tùy thuộc vào tùy chọn

typedef struct {
    uint8_t additionalOptions : 3; // 3 bits cho các tùy chọn bổ sung
    CarColor color : 2;
    CarPower power : 2;
    CarEngine engine : 1;
    
} CarOptions;

void configureCar(CarOptions *car, CarColor color, CarPower power, CarEngine engine, uint8_t options) {
    car->color = color;
    car->power = power;
    car->engine = engine;
    car->additionalOptions = options;
}

void setOption(CarOptions *car, uint8_t optionMask) {
    car->additionalOptions |= optionMask;
}

void unsetOption(CarOptions *car, uint8_t optionMask) {
    car->additionalOptions &= ~optionMask;
}


void displayCarOptions(const CarOptions car) {
    const char *colors[] = {"Red", "Blue", "Black", "White"};
    const char *powers[] = {"100HP", "150HP", "200HP"};
    const char *engines[] = {"1.5L", "2.0L"};

    printf("Car Configuration: \n");
    printf("Color: %s\n", colors[car.color]);
    printf("Power: %s\n", powers[car.power]);
    printf("Engine: %s\n", engines[car.engine]);
    printf("Sunroof: %s\n", (car.additionalOptions & SUNROOF_MASK) ? "Yes" : "No");
    printf("Premium Audio: %s\n", (car.additionalOptions & PREMIUM_AUDIO_MASK) ? "Yes" : "No");
    printf("Sports Package: %s\n", (car.additionalOptions & SPORTS_PACKAGE_MASK) ? "Yes" : "No");
}

int main() {
    CarOptions myCar = {COLOR_BLACK, POWER_150HP, ENGINE_2_0L};

    setOption(&myCar, SUNROOF_MASK);
    setOption(&myCar, PREMIUM_AUDIO_MASK);
    
    displayCarOptions(myCar);

    unsetOption(&myCar, PREMIUM_AUDIO_MASK); 
    displayCarOptions(myCar);

    printf("size of my car: %d\n", sizeof(CarOptions));

    return 0;
}
```
</details>

</details>
    
# Bài 7: Struct - Union

<details>
<summary> Details </summary>
    
## 1. Struct

<details>
<summary> Details </summary>
    
- Khái niệm:<br>
Struct là một kiểu dữ liệu được định nghĩa bởi người viết code, cho phép nhóm các kiểu dữ liệu khác nhau lại thành một kiểu dữ liệu mới. Một struct thường được sử dụng để lưu trữ các thông tin liên quan với nhau, như một người hoặc thông tin về một sản phẩm,...<br>

Cú pháp:
```cpp
struct StrucName{
    <data_type1> <member1>;
    <data_type2> <member2>;
    ...
};

//syntax: struct StrucName Variable;
```
Sau khi định nghĩa Struct ta có thể khai báo biến và truy cập biến thành viên của Struct đó thông qua dấu "." nếu đó là con trỏ (địa chỉ) thì ta phải dùng  "->" để truy cập. Mỗi thành viên trong Struct có địa chỉ riêng và chiếm không gian riêng trong bộ nhớ,
khi truy cập thành phần của struct, bạn có thể truy cập độc lập của từng thành phần đó. Ngoài ra có thể sử dụng typedef tạo bí danh (alias) để rút ngắn syntax mỗi lần khai báo biến.
```cpp
typedef struct {
    char name[50];
    int age;
    char address[100];
} Person;
int main(){
    Person person1;
    strcpy(person1.name, "Nhat");
    person1.age = 35;
    strcpy(person1.address, "Hai Chau, Da Nang");
    
    Person *person2 = malloc(sizeof(struct Person));
    strcpy(person2->name, "Nhat");
    person2->age = 35;
    strcpy(person2->address, "Nam Tu Liem, Ha Noi");
}
```

</details>
    
## 2. Union

<details>
<summary> Details </summary>
    
- Khái niệm:<br>
Union là một kiểu dữ liệu có thể lưu trữ nhiều giá trị khác nhau cùng một lúc, nhưng chỉ có thể truy cập một giá trị tại một thời điểm. Các thành viên trong một union sử dụng cùng một vùng nhớ, nghĩa là kích thước của Union sẽ bằng kích thước của thành viên lớn nhất.

Syntax:
```cpp
union <union_name> {
    <data_type1> <member1>;
    <data_type2> <member2>;
    ...
};
```
Union thường được dùng trong trường hợp muốn tiết kiệm bộ nhớ khi cần lưu nhiều kiểu dữ liệu khác nhau nhưng chỉ sử dụng một trong số đó tại thời điểm đó, truy cập dữ liệu dưới nhiều dạng khác nhau như read/write dữ liệu input/output file ở nhiều định dạng khác nhau,
các giao thức mạng và cấu trúc dữ liệu như khi cần lưu các kiểu dữ liệu khác nhau trong cùng một cấu trúc. Để truy cập biến thành viên của Union ta dùng toán tử "."
```cpp
union MyUnion {
    int i;
    float f;
    char c[4];
};

union MyUnion data;
data.i = 42;
printf("Integer value: %d\n", data.i); // Output: 42
data.f = 3.14;
printf("Float value: %f\n", data.f); // Output: 3.140000
printf("Integer value: %d\n", data.i); // Output: 1078530000 (không phải 42)
```

</details>
    
## 3. Sự khác nhau giữa Struct và Union
<details>
<summary> Details </summary>
    
3.1. Cấu trúc dữ liệu:
- Struct:
    - Cho phép lưu trữ nhiều kiểu dữ liệu khác nhau trong cùng một biến.
    - Mỗi thành phần trong struct có địa chỉ riêng và chiếm không gian riêng trong bộ nhớ.
    - Khi truy cập các thành phần của struct, có thể truy cập độc lập vào từng thành phần.
- Union:
    - Union cũng cho phép lưu trữ nhiều kiểu dữ liệu khác nhau trong cùng một biến, nhưng chỉ có thể sử dụng một thành phần của union tại một thời điểm.
    - Tất cả các thành phần của union chia sẽ cùng một vùng nhớ, do đó khi gán giá trị cho một thành phần, giá trị của thành phần khác sẽ bị thay đổi.
    - Union thường được sử dụng để tiết kiệm bộ nhớ khi chỉ cần lưu trữ một trong số các kiểu dữ liệu.
3.2 Dung lượng bộ nhớ:
- Struct:<br>
Dung lượng bộ nhớ của một biến struct bằng tổng dung lượng của tất cả các thành phần trong struct và khi sắp xếp chúng hợp lý thì chúng ta có thể tiết kiệm được bộ nhớ.<br>
Ví dụ:
```cpp
typedef struct{
    uint32_t var3;   //4 byte
    uint8_t var2;    //1 byte
    uint16_t var1;   //2 byte
}frame;
```
Tổng dung lượng bộ nhớ của struct trên sẽ là 8 byte. Nhưng khi sắp xếp lại như này thì nó sẽ thành 12 byte. Bởi vì kiểu dữ liệu lớn nhất của struct này 4 byte vậy complier nó sẽ hiểu là 4 byte sẽ đại diện cho 1 lần quét của nó, hệ thống sẽ quét theo thứ tự từ trên xuống dưới, 
nếu biến tiếp theo còn thừa đủ ô nhớ để chứa thì nó sẽ điền vào những ô nhớ tiếp theo còn trống đó. Như vậy trong ví dụ thì ở ví dụ trên lần quét đầu tiên là 4 byte var3 đã chiếm đủ, 
lần quét thứ 2 thì var2 chỉ chứ 1 ô do 1 byte và còn thừa 3 byte nên var1 sẽ điền vào phía sau đó vì var1 chỉ có 2 byte, và hệ thống sẽ còn thừa 1 byte. Ngược lại ở ví dụ dưới thì ở lần quét đầu tiên là var2 1byte còn thừa 3 byte nhưng var3 có kích thước 4 byte nên không đủ để lưu nên nó sẽ chuyển qua lần quét thử 2,
và sau khi đã quét đủ 4 byte thì var1 chỉ có thể thực hiện lần quét tiếp theo là 2 byte như vậy hệ thống đã cung cấp 12 ô nhớ và thừ 5 ô nhớ (5byte).
```
typedef struct{
    uint8_t var2;    //1 byte
    uint32_t var3;   //4 byte
    uint16_t var1;   //2 byte
}frame;
```
Ví dụ cụ thể:
```cpp
typedef struct{
    uint8_t var2[9];
    uint64_t var4[3];
    uint16_t var1[10];
    uint32_t var3[2];
    uint8_t var5;
}frame;
```
Ở ví dụ trên thì kiểu dữ liệu lớn nhất là 8 byte nên complier sẽ quét 1 lần 8 byte, vậy nên mỗi var2 là 1 byte mà mảng có 9 var2 sẽ là 8 byte cho lần quét đầu tiên và 1 byte cho lần quét thứ 2, lần quét tiếp theo vì là mỗi var4 đều là 8 byte nên không đủ ô nhớ để điền vào phần dư 7 byte vậy thì sẽ là tổng 2*8 + 3*8 = 40 byte,
tiếp theo thì mỗi var1 là 2 byte nên cần 4 var1 để đủ 8 byte như vậy còn dư 2 var1 và 4 byte, mỗi var3 thì là 4 byte nên sẽ điền vào 4 byte còn dư đó và 1 var3 sẽ điền vào 4 byte ở lần quét tiếp theo, như vậy chỉ còn var5 1byte nên sẽ điền vào 4 byte còn dư đó, như vậy kết quả sẽ là 40 + 2*8(8 var1) + 8(2 var1 +1 var3) + 8(1 var3 + 1 var5) 
= 72 byte.<br>
- Union:<br>
Dung lượng bộ nhớ của một biến union bằng với dung lượng của thành phần lớn nhất trong union. Vì chúng sử dụng chung vùng nhớ nên giá trị của 1 biến thành viên trong đó chỉ có thể chứa được tối đa giá trị mà số byte cũng như số ô nhớ mà biến đó sở hữu
Ví dụ cụ thể:
```cpp
typedef union{            
    uint8_t var2[9];     //9 byte => 16 byte
    uint64_t var4[3];    //24 byte => 24 byte
    uint16_t var1[10];   //20 byte => 24 byte
    uint32_t var3[2];    //8 byte => 8 byte
    uint8_t var5;        //1 byte => 8 byte
}frame;
```
Ở ví dụ trên thì dung lượng của bộ nhớ của biến union là 24 byte bởi vì kiểu dữ liệu lớn nhất là uint64_t có kích thước là 8 byte nên sẽ quét mỗi lần với 8 byte, vậy nên kích thước của Union sẽ là 24 byte.<br>
3.3 Truy cập dữ liệu:
- Struct: Truy cập đồng thời nhiều thành phần khác nhau của struct.
- Union: Chỉ có thể truy cập vào một thành phần của union tại một thời điểm.

</details>

</details>
    
# Bài 8: Memory Layout

<details>
<summary> Details </summary>
    
## Khái niệm

<details>
<summary> Details </summary>
    
Chương trình của mình code ở file main.c hoặc main.exe thì file đó được lưu trên bộ nhớ ROM hoặc Flash, khi click run chương trình thì sẽ copy qua vùng nhớ RAM rồi mới chạy chương trình, RAM tốc độ truy xuất nhanh, ở trong vi điều khiển (main.hex) thì khi chạy nó sẽ nạp chương trình vào và nằm ở phân vùng nhớ FLASH hoặc SSD và 
khi nó chạy (cấp nguồn cho vi điều khiển) nó sẽ copy chương trình ở flash qua RAM, quá trình này được gọi là Putting progress: quá trình chuyển dữ liệu từ bộ nhớ flash (nơi lưu trữ mã chương trình và dữ liệu cố định) qua bộ nhớ RAM (nơi lưu trữ dữ liệu tạm thời và biến trong quá trình thực thi chương trình).<br>
Những biến chúng ta code ra đều được chạy trên bộ nhớ RAM (Câu hỏi phỏng vấn: Những biến này được lưu ở đâu)<br>
Có tổng cộng 5 phân vùng nhớ bao gồm: Text, data (initialized data), bss (uninitialized data), heap và stack.

</details>
    
## 1. Vùng nhớ text

<details>
<summary> Details </summary>
    
Phân vùng nhớ text (Text Segment) là những chương trình khi được copy từ vùng nhớ Flash sang RAM mà chương trình đó không có cách nào để sửa (chỉ đọc) thì nó sẽ được lưu ở phân vùng Text ngoài ra các biến Const hoặc con trỏ ký tự nó cũng sẽ lưu ở phân vùng Text.<br>
Ví dụ:
```cpp
const int a = 10;
char *ptr = "Hello worlds";

int main(){
    return 0;
}
```

</details>
    
## 2. Vùng nhớ data

<details>
<summary> Details </summary>
    
Phân vùng nhớ data (Data segment) là phân vùng nhớ chứa các biến toàn cục được khởi tạo đầu tiên giá trị khác 0, chứa các biến Static được khởi tạo ở giá trị khác 0, quyền truy cập của nó là đọc và ghi nghĩa là có thể đọc và thay đổi giá trị của biến và tất cả các biến sẽ được thu hồi sau khi chương trình kết thúc.
Ví dụ:
```cpp
int a = 10; //Biến toàn cục
static int a = 20; // Biến giá trị static toàn cục
void test(){
    static int x = 5; // Biến giá trị static cục bộ
}

int main(){
    a = 50; //Thay đổi giá trị vẫn ở phân vùng nhớ data
    return 0;
}
```
</details>
    
## 3. Vùng nhớ bss

<details>
<summary> Details </summary>
    
Phân vùng nhớ BSS (BSS segment) là phân vùng nhớ chứa biến toàn cụ và các biến Static được khởi tạo đầu tiên giá trị bằng 0 hoặc không gán giá trị ban đầu cho biến đó và phân vùng này cũng tồn tại hết vòng đời ở chương trình, quyền truy cập của nó cũng là đọc và ghi có nghĩa là có thể đọc và thay đổi giá trị của biến đó.
Ví dụ:
```cpp
int a = 0; //Biến toàn cục
static int a; // Biến giá trị static toàn cục
void test(){
    static int x = 0; // Biến giá trị static cục bộ
}

int main(){
    a = 50; //Thay đổi giá trị vẫn ở phân vùng nhớ bss
    return 0;
}
```

</details>

## 4. Vùng nhớ Stack

<details>
<summary> Details </summary>
    
4.1. Khái niệm:
- Phân vùng stack là vùng nhớ được cấp phát và quản lý theo cấu trúc stack, một vùng nhớ tĩnh, được sử dụng để lưu trữ các biến cục bộ và tham số truyền vào các hàm. Khi chúng ta gọi hàm các biến trong hàm sẽ được cấp địa chỉ, sau khi kết thúc hàm các biến đó sẽ bị thu hồi mà những địa chỉ được cấp phát và thu đều ở phân vùng nhớ Stack. Cấu trúc Stack là một cấu trúc dữ liệu có tính chất "Last-In-First-Out" (LIFO), được sử dụng để lưu trữ các biến cục bộ, tham số của hàm, và địa chỉ trở về (return address) khi gọi hàm. <br>
Lưu ý: Kể cả dùng biến const bên trong function thì vẫn ở phân vùng nhớ Stack

4.2.Cách hoạt động của stack:
- Khi một hàm được gọi, một khung stack (stack frame) mới sẽ được tạo ra và đẩy lên trên cùng của stack.
- Trong khung stack này, các biến cục bộ của hàm, tham số truyền vào, và địa chỉ trở về sẽ được lưu trữ.
- Khi hàm kết thúc, khung stack của hàm đó sẽ được xóa khỏi stack, và giá trị các biến cục bộ sẽ mất đi.<br>

4.3. Ưu điểm:
- Quản lý bộ nhớ đơn giản và hiệu quả.
- Hỗ trợ đệ quy (recursion) tốt.
- Truy xuất dữ liệu nhanh.<br>

4.4. Nhược điểm:
- Dung lượng bộ nhớ có giới hạn, không thể mở rộng kích thước stack tùy ý.
- Nếu sử dụng đệ quy quá sâu có thể dẫn đến tràn stack (stack overflow).

</details>
    
## 5. Vùng nhớ Heap:

<details>
<summary> Details </summary>
    
Phân vùng nhớ Heap là một vùng nhớ động, được sử dụng để cấp phát và giải phóng bộ nhớ một cách linh hoạt trong quá trình chạy chương trình.<br>

5.1. Khái niệm:
- Heap là một vùng nhớ được sử dụng để cấp phát và giải phóng bộ nhớ động, không như vùng nhớ Stack, nơi chỉ lưu trữ các biến cục bộ và tham số truyền vào các hàm.
- Bộ nhớ Heap được quản lý bởi hệ thống cấp phát bộ nhớ động, cung cấp các hàm như malloc(), calloc(), realloc() và free() để người dùng có thể cấp phát và giải phóng bộ nhớ.

5.2. Cách hoạt động:
- Khi chương trình cần cấp phát bộ nhớ động, nó sẽ gọi các hàm cấp phát bộ nhớ như malloc() hoặc calloc() hoặc realloc() để yêu cầu một vùng nhớ từ Heap.
- Hệ thống cấp phát bộ nhớ sẽ tìm kiếm một khu vực trống trong Heap đủ lớn để chứa dữ liệu cần cấp phát, và trả lại một con trỏ trỏ đến vùng nhớ đó.
- Khi không còn cần sử dụng vùng nhớ đó nữa, chương trình sẽ gọi hàm free() để giải phóng vùng nhớ, để hệ thống có thể sử dụng lại nó sau này.

5.3. Ưu điểm:
- Linh hoạt: Heap cho phép cấp phát và giải phóng bộ nhớ động, không giới hạn kích thước như Stack.
- Hiệu quả: Heap cho phép sử dụng bộ nhớ hiệu quả hơn, tránh lãng phí bộ nhớ.
- Phù hợp với các cấu trúc dữ liệu phức tạp: Heap phù hợp với việc tạo ra các cấu trúc dữ liệu động như linked list, tree, v.v.

5.4. Nhược điểm:
- Quản lý phức tạp: Việc cấp phát và giải phóng bộ nhớ trên Heap đòi hỏi người lập trình phải quản lý cẩn thận, tránh rò rỉ bộ nhớ.
- Hiệu suất có thể giảm: Quá trình quản lý Heap có thể làm giảm hiệu suất chung của chương trình, đặc biệt nếu có nhiều yêu cầu cấp phát/giải phóng bộ nhớ.
- Nguy cơ lỗi: Nếu không quản lý cẩn thận, việc sử dụng Heap có thể dẫn đến các lỗi như truy cập vùng nhớ không hợp lệ, rò rỉ bộ nhớ, v.v.

5.5. Rò rỉ bộ nhớ:
- Rò rỉ bộ nhớ (memory leak) là một vấn đề trong lập trình, đề cập đến tình trạng bộ nhớ được cấp phát nhưng không được giải phóng khi không còn sử dụng nữa.

Khái niệm:
- Khi một chương trình cấp phát bộ nhớ để lưu trữ dữ liệu, nhưng không giải phóng lại bộ nhớ đó khi không cần dùng nữa, thì xảy ra rò rỉ bộ nhớ.
- Tình trạng này dẫn đến bộ nhớ của hệ thống bị chiếm dụng ngày càng nhiều, làm chậm hoạt động của chương trình và hệ thống.

Nguyên nhân:
- Quên giải phóng bộ nhớ khi không cần dùng nữa.
- Lỗi logic trong việc quản lý động bộ nhớ.
- Sử dụng không đúng cách các hàm cấp phát/giải phóng bộ nhớ.

Ảnh hưởng
- Chương trình chạy chậm dần do bộ nhớ bị chiếm dụng ngày càng nhiều.
- Trong trường hợp nghiêm trọng, chương trình có thể bị treo hoặc crash do không đủ bộ nhớ.
- Tốn tài nguyên hệ thống, ảnh hưởng đến hiệu suất chung của máy.

Phòng ngừa và khắc phục:
- Sử dụng các công cụ kiểm tra rò rỉ bộ nhớ.
- Thực hiện đúng quy trình cấp phát và giải phóng bộ nhớ.
- Sử dụng ngôn ngữ lập trình có garbage collector tự động quản lý bộ nhớ.
- Viết mã chương trình cẩn thận, kiểm tra kỹ các điểm cấp phát và giải phóng bộ nhớ.

5.6. Hàm malloc:
- Hàm malloc cấp phát một khối bộ nhớ có kích thước được chỉ định bởi người dùng. Kích thước của khối bộ nhớ này được tính bằng bytes và các ô nhớ được cấp phát sẽ liền kề nhau.

Cú pháp:
> void *malloc(size_t size);

- size là kích thước của khối bộ nhớ cần được cấp phát, tính bằng bytes.
- Hàm trả về một con trỏ void * trỏ đến vùng nhớ đã được cấp phát. Nếu không đủ bộ nhớ, hàm sẽ trả về giá trị NULL.<br>

**Lưu ý: ** vì size là kích thước của khối bộ nhớ đó nên chúng ta sẽ dùng sizeof cho kiểu dữ liệu cần cấp phát ví dụ sizeof(int)*n với n là số phần tử cần cấp phát và sẽ tạo ra n ô nhớ liền kề, hàm trả về là một con trỏ void * vậy nên khi khai báo cần ép kiểu dữ liệu mà chúng ta cần trỏ tới ví dụ như (int*)malloc(sizeof(int)*n), khi sử dụng xong thì cần dùng hàm free() để giải phóng bộ nhớ tránh rò rỉ bộ nhớ.

Ứng dụng: hàm malloc thường được sử dụng khi chúng ta cần cấp phát bộ nhớ động, ví dụ như khi cần cấp phát bộ nhớ cho một mảng có kích thước chưa biết trước, hoặc khi chúng ta cần cấp phát bộ nhớ cho 
các cấu trúc dữ liệu trức tạp như linked list, tree,...

5.7. Hàm calloc:
- Hàm calloc cấp phát một khối bộ nhớ có kích thước được chỉ định bởi người dùng, và khởi tạo tất cả các byte trong vùng nhớ này thành giá trị 0.

Cú pháp:
> void *calloc(size_t nmemb, size_t size);

- nmemb là số lượng phần tử cần được cấp phát
- size là kích thước của mỗi phần tử, tính bằng bytes.
- Hàm trả về một con trỏ void * trỏ đến vùng nhớ đã được cấp phát và khởi tạo. Nếu không đủ bộ nhớ, hàm sẽ trả về giá trị NULL.

**Lưu ý: ** vì nmembn là số lượng phần tử cần được cấp phát sẽ tạo ra nmembn ô nhớ liền kề theo kích thước mỗi phần tử, size là kích thước của mỗi phần tử đó nên chúng ta sẽ dùng sizeof cho kiểu dữ liệu cần cấp phát ví dụ sizeof(int), hàm trả về là một con trỏ void * vậy nên khi khai báo cần ép kiểu dữ liệu mà chúng ta cần trỏ tới ví dụ như (int*)calloc(nmembn,sizeof(int)), khi sử dụng xong thì cần dùng hàm free() để giải phóng bộ nhớ tránh rò rỉ bộ nhớ.

Ứng dụng: Hàm calloc thường được sử dụng khi chúng ta cần cấp phát bộ nhớ động và khởi tạo tất cả các byte trong vùng nhớ thành giá trị 0. Ví dụ như khi chúng ta cần cấp phát bộ nhớ cho một mảng và khởi tạo tất cả các phần tử của mảng thành 0.

5.8. Hàm realloc
- Hàm realloc được sử dụng để thay đổi kích thước của một vùng nhớ đã được cấp phát trước đó. Nó có thể tăng hoặc giảm kích thước của vùng nhớ, tùy thuộc vào yêu cầu.

Cú pháp:
> void *realloc(void *ptr, size_t size);

- ptr là con trỏ trỏ đến vùng nhớ đã được cấp phát trước đó.
- size là kích thước mới của vùng nhớ, tính bằng bytes.
- Hàm trả về một con trỏ void * trỏ đến vùng nhớ mới. Nếu không đủ bộ nhớ, hàm sẽ trả về giá trị NULL.

**Lưu ý: ** vì size là kích thước của khối bộ nhớ đó nên chúng ta sẽ dùng sizeof cho kiểu dữ liệu cần cấp phát ví dụ sizeof(int)*n với n là số phần tử cần cấp phát và sẽ tạo ra n ô nhớ liền kề, hàm trả về là một con trỏ void * vậy nên khi khai báo cần ép kiểu dữ liệu mà chúng ta cần trỏ tới ví dụ như (int*)realloc(ptr,sizeof(int)*n), khi sử dụng xong thì cần dùng hàm free() để giải phóng bộ nhớ tránh rò rỉ bộ nhớ.

Ứng dụng: Hàm realloc thường được sử dụng khi chúng ta cần thay đổi kích thước của một vùng nhớ đã được cấp phát trước đó. Ví dụ, khi chúng ta cần tăng kích thước của một mảng động, hoặc khi chúng ta cần thay đổi kích thước của một cấu trúc dữ liệu phức tạp.

5.9. Hàm free
- Hàm free được sử dụng để giải phóng vùng nhớ đã được cấp phát trước đó. Sau khi sử dụng xong vùng nhớ, chúng ta cần giải phóng nó để tránh rò rỉ bộ nhớ.

Cú pháp:
> void free(void *ptr);

- ptr là con trỏ trỏ đến vùng nhớ cần được giải phóng.

Ứng dụng: Hàm free() thường được sử dụng sau khi chúng ta đã sử dụng xong vùng nhớ được cấp phát bởi các hàm malloc(), calloc() hoặc realloc(). Nếu không giải phóng vùng nhớ, chúng ta sẽ gặp phải vấn đề rò rỉ bộ nhớ (memory leak).

</details>

</details>

# Bài 9: JSON

<details>
<summary> Details </summary>
    
## 1. Khái niệm và ứng dụng

<details>
<summary> Details </summary>
    
JSON (Javascript Object Notation) là một dạng database dựa trên văn bản, nó nhẹ, dễ đọc và dễ viết, cú pháp đơn giản và gọn gàng, được sử dụng để trao đổi dữ liệu giữa máy chủ và web app, nó hiệu quả khi truyền dữ liệu qua mạng đặc biệt là trên các thiết bị có băng thông hạn chế và trên ô tô.<br>
JSON được sử dụng để lưu database, lưu các dữ liệu mà có kiểu dữ liệu không xác định được. Mỗi đối tượng JSON bao gồm một tập hợp các cặp key-value, mảng JSON tập hợp các giá trị của mỗi đối tượng hoặc đối tượng.<br>

Cú pháp:
- JSON sử dụng cú pháp gồm các cặp key-value, được bao bởi dấu ngoặc nhọn {}.
- Các giá trị có thể là chuỗi, số, boolean, null, mảng hoặc đối tượng JSON khác.

Ví dụ:
```cpp
{
  "name": "Nhat",
  "age": 26,
  "city": "Da Nang",
  "isStudent": false,
  "Hobies" : ["Code","Jogging","Cycling"],
  "C++" : {
    "Pointer":"Lesson 3",
    "JSON":"Lesson 9"
    }
}
```
</details>

## 2. Cấu trúc và định dạng của JSON

<details>
<summary> Details </summary>
    
Cấu trúc của JSON:
```cpp
typedef enum {
    JSON_NULL,
    JSON_BOOLEAN,
    JSON_NUMBER,
    JSON_STRING,
    JSON_ARRAY,
    JSON_OBJECT
} JsonType;

typedef struct JsonValue {
    JsonType type;
    union {
        int boolean;
        double number;
        char *string;
        struct {
            struct JsonValue *values;
            size_t count;
        } array;
        struct {
            char **keys;
            struct JsonValue *values;
            size_t count;
        } object;
    } value;
} JsonValue;
```
Ở đoạn code trên Json sẽ có 6 kiểu dữ liệu:
- NULL: Chứa giá trị NULL
- Boolean: Chứa giá trị True hoặc False
- Number: Chứa giá trị là số (số nguyên hoặc số thực)
- String: Chứa giá trị là chuỗi
- Array: Chứa giá trị là mảng được bao bọc với dấu ngoặc vuông [], nó được cấu trúc bởi số lượng mảng (count) và các giá trị JsonValue ở trong mảng đó mà mỗi phần tử trong mảng sẽ được biểu diễn bằng một cấu trúc JsonValue.
- Object: Chứa các đối tượng của JSON khác được bao bọc bởi dấu ngoặc nhọn {}, nó được cấu trức bởi một mảng chứa keys và một mảng các JsonValue (cấu trúc JsonValue) tương ứng, cùng với số lượng của mảng.

Định dạng Key-Value:
Mỗi cặp key-value được tách bằng dấu hai chấm ":" và các đối tượng phân tách nhau bằng dấu phẩy ",". Ngoài ra bất kỳ khoảng trắng nào cũng không ảnh hưởng tới tính chính xác của JSON và thường làm cho JSON dễ đọc hơn.

Source code của JSON ở trên C
```cpp
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <stddef.h>
#include <ctype.h>
#include <stdbool.h>

typedef enum {
    JSON_NULL,
    JSON_BOOLEAN,
    JSON_NUMBER,
    JSON_STRING,
    JSON_ARRAY,
    JSON_OBJECT
} JsonType;

typedef struct JsonValue {
    JsonType type;
    union {
        int boolean;
        double number;
        char *string;
        struct {
            struct JsonValue *values;
            size_t count;
        } array;
        struct {
            char **keys;
            struct JsonValue *values;
            size_t count;
        } object;
    } value;
} JsonValue;

JsonValue *parse_json(const char **json);

void free_json_value(JsonValue *json_value);

static void skip_whitespace(const char **json) {
    while (isspace(**json)) {
        (*json)++;
    }
}

JsonValue *parse_null(const char **json) {
    skip_whitespace(json);
    if (strncmp(*json, "null", 4) == 0) {
        JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
        value->type = JSON_NULL;
        *json += 4;
        return value;
    }
    return NULL;
}

JsonValue *parse_boolean(const char **json) {
    skip_whitespace(json);
    JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
    if (strncmp(*json, "true", 4) == 0) {
        value->type = JSON_BOOLEAN;
        value->value.boolean = true;
        *json += 4;
    } else if (strncmp(*json, "false", 5) == 0) {
        value->type = JSON_BOOLEAN;
        value->value.boolean = false;
        *json += 5;
    } else {
        free(value);
        return NULL;
    }
    return value;
}

JsonValue *parse_number(const char **json) {
    skip_whitespace(json);
    char *end;


    double num = strtod(*json, &end);
    if (end != *json) {
        JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
        value->type = JSON_NUMBER;
        value->value.number = num;
        *json = end;
        return value;
    }
    return NULL;
}

JsonValue *parse_string(const char **json) {
    skip_whitespace(json);


    if (**json == '\"') {
        (*json)++;
        const char *start = *json;
        while (**json != '\"' && **json != '\0') {
            (*json)++;
        }
        if (**json == '\"') {
            size_t length = *json - start; // 3
            char *str = (char *) malloc((length + 1) * sizeof(char));
            strncpy(str, start, length);
            str[length] = '\0';


            JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
            value->type = JSON_STRING;
            value->value.string = str;
            (*json)++;
            return value;
        }
    }
    return NULL;
}

JsonValue *parse_array(const char **json) {
    skip_whitespace(json);
    if (**json == '[') {
        (*json)++;
        skip_whitespace(json);

        JsonValue *array_value = (JsonValue *)malloc(sizeof(JsonValue));
        array_value->type = JSON_ARRAY;
        array_value->value.array.count = 0;
        array_value->value.array.values = NULL;

        /*
        double arr[2] = {};
        arr[0] = 30;
        arr[1] = 70;
        */

        while (**json != ']' && **json != '\0') {
            JsonValue *element = parse_json(json); // 70
            if (element) {
                array_value->value.array.count++;
                array_value->value.array.values = (JsonValue *)realloc(array_value->value.array.values, array_value->value.array.count * sizeof(JsonValue));
                array_value->value.array.values[array_value->value.array.count - 1] = *element;
                free(element);
            } else {
                break;
            }
            skip_whitespace(json);
            if (**json == ',') {
                (*json)++;
            }
        }
        if (**json == ']') {
            (*json)++;
            return array_value;
        } else {
            free_json_value(array_value);
            return NULL;
        }
    }
    return NULL;
}

JsonValue *parse_object(const char **json) {
    skip_whitespace(json);
    if (**json == '{') {
        (*json)++;
        skip_whitespace(json);

        JsonValue *object_value = (JsonValue *)malloc(sizeof(JsonValue));
        object_value->type = JSON_OBJECT;
        object_value->value.object.count = 0;
        object_value->value.object.keys = NULL;
        object_value->value.object.values = NULL;



        while (**json != '}' && **json != '\0') {
            JsonValue *key = parse_string(json);
            if (key) {
                skip_whitespace(json);
                if (**json == ':') {
                    (*json)++;
                    JsonValue *value = parse_json(json);
                    if (value) {
                        object_value->value.object.count++;
                        object_value->value.object.keys = (char **)realloc(object_value->value.object.keys, object_value->value.object.count * sizeof(char *));
                        object_value->value.object.keys[object_value->value.object.count - 1] = key->value.string;

                        object_value->value.object.values = (JsonValue *)realloc(object_value->value.object.values, object_value->value.object.count * sizeof(JsonValue));
                        object_value->value.object.values[object_value->value.object.count - 1] = *value;
                        free(value);
                    } else {
                        free_json_value(key);
                        break;
                    }
                } else {
                    free_json_value(key);
                    break;
                }
            } else {
                break;
            }
            skip_whitespace(json);
            if (**json == ',') {
                (*json)++;
            }
        }
        if (**json == '}') {
            (*json)++;
            return object_value;
        } else {
            free_json_value(object_value);
            return NULL;
        }
    }
    return NULL;
}

JsonValue *parse_json(const char **json) { 
    while (isspace(**json)) {
        (*json)++;
    }

    switch (**json) {
        case 'n':
            return parse_null(json);
        case 't':
        case 'f':
            return parse_boolean(json);
        case '\"':
            return parse_string(json);
        case '[':
            return parse_array(json);
        case '{':
            return parse_object(json);
        default:
            if (isdigit(**json) || **json == '-') {
                return parse_number(json);
            } else {
                // Lỗi phân tích cú pháp
                return NULL;
            }
    }
}

void free_json_value(JsonValue *json_value) {
    if (json_value == NULL) {
        return;
    }

    switch (json_value->type) {
        case JSON_STRING:
            free(json_value->value.string);
            break;

        case JSON_ARRAY:
            for (size_t i = 0; i < json_value->value.array.count; i++) {
                free_json_value(&json_value->value.array.values[i]);
            }
            free(json_value->value.array.values);
            break;

        case JSON_OBJECT:
            for (size_t i = 0; i < json_value->value.object.count; i++) {
                free(json_value->value.object.keys[i]);
                free_json_value(&json_value->value.object.values[i]);
            }
            free(json_value->value.object.keys);
            free(json_value->value.object.values);
            break;

        default:
            break;
    }
}

void test(JsonValue* json_value){
    if (json_value != NULL && json_value->type == JSON_OBJECT) {
        // Truy cập giá trị của các trường trong đối tượng JSON
        size_t num_fields = json_value->value.object.count;
        size_t num_fields2 = json_value->value.object.values->value.object.count;
        for (size_t i = 0; i < num_fields; ++i) {

            char* key = json_value->value.object.keys[i];
            JsonValue* value = &json_value->value.object.values[i];

            JsonType type = (int)(json_value->value.object.values[i].type);


            if(type == JSON_STRING){
                printf("%s: %s\n", key, value->value.string);
            }

            if(type == JSON_NUMBER){
                printf("%s: %f\n", key, value->value.number);
            }

            if(type == JSON_BOOLEAN){
                printf("%s: %s\n", key, value->value.boolean ? "True":"False");
            }

            if(type == JSON_OBJECT){
                printf("%s: \n", key);
                test(value);
            }

            if(type == JSON_ARRAY){
                printf("%s: ", key);
                for (int i = 0; i < value->value.array.count; i++)
                {
                   test(value->value.array.values + i);
                } 
                printf("\n");
            }
        }
     
    }
    else 
    {
            if(json_value->type == JSON_STRING){
                printf("%s ", json_value->value.string);
            }

            if(json_value->type == JSON_NUMBER){
                printf("%f ", json_value->value.number);
            }

            if(json_value->type == JSON_BOOLEAN){
                printf("%s ", json_value->value.boolean ? "True":"False");
            }

            if(json_value->type == JSON_OBJECT){
                printf("%s: \n", json_value->value.object.keys);
                test(json_value->value.object.values);
                           
            }
    }

}


int main(int argc, char const *argv[])
{
     // Chuỗi JSON đầu vào
    
    const char* json_str = "{"
                        "\"1001\":{"
                          "\"SoPhong\":3,"
                          "\"NguoiThue\":{"
                            "\"Ten\":\"Nguyen Van A\","
                            "\"CCCD\":\"1920517781\","
                            "\"Tuoi\":26,"
                            "\"ThuongTru\":{"
                              "\"Duong\":\"73 Ba Huyen Thanh Quan\","
                              "\"Phuong_Xa\":\"Phuong 6\","
                              "\"Tinh_TP\":\"Ho Chi Minh\""
                            "}"
                          "},"
                          "\"SoNguoiO\":{"
                            "\"1\":\"Nguyen Van A\","
                            "\"2\":\"Nguyen Van B\","
                            "\"3\":\"Nguyen Van C\""
                          "},"
                          "\"TienDien\": [24, 56, 98],"
                          "\"TienNuoc\":30.000"
                        "},"
                        "\"1002\":{"
                          "\"SoPhong\":5,"
                          "\"NguoiThue\":{"
                            "\"Ten\":\"Phan Hoang Trung\","
                            "\"CCCD\":\"012345678912\","
                            "\"Tuoi\":24,"
                            "\"ThuongTru\":{"
                              "\"Duong\":\"53 Le Dai Hanh\","
                              "\"Phuong_Xa\":\"Phuong 11\","
                              "\"Tinh_TP\":\"Ho Chi Minh\""
                            "}"
                          "},"
                          "\"SoNguoiO\":{"
                            "\"1\":\"Phan Van Nhat\","
                            "\"2\":\"Phan Van Nhi\","
                            "\"2\":\"Phan Van Tam\","
                            "\"3\":\"Phan Van Tu\""
                          "},"
                          "\"TienDien\":23.000,"
                          "\"TienNuoc\":40.000"
                        "}"
                      "}";
    

    // Phân tích cú pháp chuỗi JSON
    JsonValue* json_value = parse_json(&json_str);

    test(json_value);

    // Kiểm tra kết quả phân tích cú pháp

       // Giải phóng bộ nhớ được cấp phát cho JsonValue
    free_json_value(json_value);

        //printf("test = %x", '\"');

       // hienthi(5);
    
    return 0;
}
```
</details>

</details>
    
# Bài 10: Linked List

<details>
<summary> Details </summary>
    
## Khái niệm

<details>
<summary> Details </summary>
    
Linked List là cấu trúc dữ liệu mà trong đó các phần tử node được liên kết với nhau thông qua các con trỏ.<br>
Mỗi node trong linked list sẽ có 2 thành phần là:<br>
- Data: Chứa dữ liệu của node
- Next: Là một con trỏ chứa địa chỉ tiếp theo của node tiếp theo

Cấu trúc của một node trong Linked List được định nghĩa bằng một struct như sau:
```cpp
typedef struct Node {
    int data;
    Node *next;
} Node;

//Create New Node
Node *createNode(int data){
    Node *ptr = (Node*)malloc(sizeof(Node));
    ptr->data = data;
    ptr->next = NULL;
    return ptr;
}
```

</details>
    
## Ứng dụng

<details>
<summary> Details </summary>
    
Khi chúng ta có 1 mảng 10000 phần tử, để chèn vào phần tử hoặc xóa 1 phần tử ở 1 vị trí chỉ định thì chúng ta phải tốn nhiều tài nguyên của hệ thống để dịch chuyển sang hoặc lùi 1 ô nhớ, vì vậy chúng ta dùng Linked List.<br>
Linked List có thể ứng dụng để tạo một List có kích thước động vì nó cấp phát động cho mỗi khi tạo ra Node mới và các Node trong đó không cần phải có địa chỉ liên tiếp nhau nhưng có thể truy cập tuần tự theo danh sách.<br>
Các thao tác cơ bản của Linked List là:
- push_back: Thêm phần tử Node vào cuối danh sách.
```cpp
void push_back(Node **array, int data) {
    if (*array == NULL) {
        *array = createNode(data);
    }
    else {
        Node *temp = *array;
        while (temp->next != NULL) {
            temp = temp->next;
        }
        temp->next = createNode(data);
    }
}
```
- pop_back: Xóa phần tử cuối cùng ở danh sách Node.
```cpp
void pop_back(Node **array){
    Node *temp = *array;
    if (temp->next == NULL){
        free(temp);
    }
    else {
        while(temp->next->next!=NULL) {
            temp = temp->next;
        }
        free(temp->next);
        temp->next = NULL;
    }
}
```
- insert: Chèn Node mới vào vị trí bất kỳ.
```cpp
void insert(Node **array,int data,int position){
    Node *temp = *array;
    if (temp == NULL && position == 1) {
        *array = createNode(data);
        return;
    }
    if (position < 1 || temp == NULL){
        printf("Error: Incorrect position\n");
        return;
    }
    if (position == 1){
        Node *newNode = createNode(data);
        newNode->next = *array;
        *array = newNode;
        return;
    }

    for (int i = 1; i < position - 1 && temp != NULL; i++){
        temp = temp->next;
    }

    if (temp == NULL) {
        printf("Error: Incorrect position\n");
        return;
    }

    Node *newNode = createNode(data);
    newNode->next = temp->next;
    temp->next = newNode;
}
```
- erase: Xóa Node với vị trí được chỉ định.
```cpp
void erase(Node **array,int position){
    Node *temp = *array;
    
    if (temp == NULL) {
        return;
    }
    if (position < 1) {
        printf("Error: Position < 1\n");
        return;
    }
    if (position == 1){
        *array = temp->next;
        free(temp);
        return;
    }

    for (int i = 1; i < position - 1; i++){
        temp = temp->next;
    }
    if (temp->next == NULL) {
        printf("Error: Position > list size\n");
        return;
    }
    if (temp->next->next == NULL) {
        temp->next = NULL;
        free(temp->next);
    } else {
        Node *current = temp->next;
        temp->next = temp->next->next;
        current = NULL;
        free(current);
    }
}
```
- clear: Xóa toàn bộ các phần tử ở trong danh sách Node.
```cpp
void clear(Node **array){
    Node *temp = *array;
    Node *nextTemp;

    while (temp != NULL) {
        nextTemp = temp->next;
        free(temp);
        temp = nextTemp;
    }

    *array = NULL;
}
```

</details>

</details>
    
# Bài 11: Stack - Queue

<details>
<summary> Details </summary>
    
## 1. Stack

<details>
<summary> Details </summary>
    
Stack là một cấu trúc dữ liệu tuân theo nguyên tắc "Last-In-First-Out" (LIFO) - phần tử được thêm vào cuối cùng sẽ được lấy ra đầu tiên.<br>
Stack có thể được triển khai bằng mảng hoặc danh sách liên kết dưới dạng struct,...<br>
![Stack Image](https://github.com/Fakerrrrrrrrrrr/Advanced_C/blob/main/Images/Stack.png)
Ở ví dụ trên ta có thể thấy thì kích cỡ của mảng hoặc danh sách là 7 và 5, bắt đầu với index hoặc top = -1, những phần tử sẽ được thêm vào theo trình tự 1->2->3->4->5->6->7 index hoặc top sẽ tăng dần thêm 1 theo trình tự và chúng sẽ được lấy ra theo trình tự ngược lại 7->6->5->4->3->2->1 và index hoặc top cũng giảm dần đi 1 theo trình tự.<br>
Stack có 3 thao tác chính cơ bản như:
- push: thêm phần tử ở cuối vào mảng stack
- pop: xóa phần tử ở cuối cùng ra khỏi mảng stack
- top: lấy giá trị ở cuối cùng ở mảng stack

Code triển khai Stack dưới dạng mảng:
```cpp
#include <stdio.h>
#include <stdlib.h>

#define SIZE 10

int arr_stack[SIZE];
int top = -1;

//Function element empty
int is_empty(){
    return top == -1;
}

//Function element full
int is_full(){
    return top == SIZE - 1;
}

//Function import element into stack
void push(int value){
    if (is_full()){
        printf("Stack overflow\n");
        return;
    }
    arr_stack[++top] = value;
}

//Function export element into stack
int pop(){
    if (is_empty()) {
        printf("Stack is empty\n");
        return -1;
    }
    return arr_stack[top--];
}

//Function get last element in stack
int top(){
    if (is_empty()) {
        printf("Stack is empty\n");
        return -1;
    }
    return arr_stack[top];
}

int main()
{
    push(10);
    push(20);
    push(30);
    push(40);
    push(50);
    printf("%d\n",pop());   //50
    printf("%d\n",top());  //40
    printf("%d\n",top);     //3
    printf("%d\n",pop());   //40
    printf("%d\n",top());  //30
    printf("%d\n",top);     //2
    return 0;
}
```
Code triển khai Stack dưới dạng Struct:
```cpp
#include <stdio.h>
#include <stdlib.h>

//Create Stack Struct
typedef struct{
    int* items;
    int size;
    int top;
} Stack;

//Initialize Stack
void initialize(Stack *stack, int size) {
    stack->items = (int*) malloc(sizeof(int) * size);
    stack->size = size;
    stack->top = -1;
}

//Check Stack is element empty
int is_empty(Stack stack) {
    return stack.top == -1;
}

//Check Stack is element full
int is_full( Stack stack) {
    return stack.top == stack.size - 1;
}

//Insert last element into Stack
void push(Stack *stack, int value) {
    if (!is_full(*stack)) {
        stack->items[++stack->top] = value;
    } else {
        printf("Stack overflow\n");
    }
}

//Erase last element from Stack
int pop(Stack *stack) {
    if (!is_empty(*stack)) {
        return stack->items[stack->top--];
    } else {
        printf("Stack underflow\n");
        return -1;
    }
}

//Get last value from Stack
int top(Stack stack) {
    if (!is_empty(stack)) {
        return stack.items[stack.top];
    } else {
        printf("Stack is empty\n");
        return -1;
    }
}

int main() {
    Stack stack1;
    initialize(&stack1, 5);

    push(&stack1, 10);
    push(&stack1, 20);
    push(&stack1, 30);
    push(&stack1, 40);
    push(&stack1, 50);
    push(&stack1, 60); //Stack overflow

    printf("Top element: %d\n", top(stack1)); //Top element: 50
    printf("Pop element: %d\n", pop(&stack1)); //Pop element: 50
    printf("Pop element: %d\n", pop(&stack1)); //Pop element: 40
    printf("Top element: %d\n", top(stack1)); //Top element: 30
    return 0;
}
```

</details>
    
## 2. Queue

<details>
<summary> Details </summary>
    
Queue là một cấu trúc dữ liệu tuân theo nguyên tắt "Fisrt In, First Out", phần tử đầu tiên được thêm vào mảng Queue sẽ là phần tử được lấy ra đầu tiên từ mảng Queue.<br>
Queue có 3 thao tác chính:
- enqueue: Thêm vào phần tử cuối cùng của mảng Queue.
- dequeue: Lấy ra phần tử đầu tiên của mảng Queue.
- front: lấy ra giá trị đầu tiên của mảng Queue

Code Queue với mảng:
```cpp
#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 100

typedef struct {
    int data[MAX_SIZE];
    int front;
    int rear;
} Queue;

void initQueue(Queue *q) {
    q->front = q->rear = -1;
}

int isEmpty(Queue *q) {
    return q->front == -1 && q->rear == -1;
}

int isFull(Queue *q) {
    return q->rear == MAX_SIZE - 1;
}

void enqueue(Queue *q, int value) {
    if (isFull(q)) {
        printf("Queue is full\n");
        return;
    }
    if (isEmpty(q)) {
        q->front = q->rear = 0;
    } else {
        q->rear++;
    }
    q->data[q->rear] = value;
}

int dequeue(Queue *q) {
    if (isEmpty(q)) {
        printf("Queue is empty\n");
        return -1;
    }
    int value = q->data[q->front];
    if (q->front == q->rear) {
        q->front = q->rear = -1;
    } else {
        q->front++;
    }
    return value;
}

int front(Queue *q) {
    if (isEmpty(q)) {
        printf("Queue is empty\n");
        return -1;
    }
    return q->data[q->front];
}

int main() {
    Queue q;
    initQueue(&q);

    enqueue(&q, 10);
    enqueue(&q, 20);
    enqueue(&q, 30);

    printf("Peek value: %d\n", front(&q)); //10
    printf("Dequeued value: %d\n", dequeue(&q)); //10
    printf("Peek value: %d\n", front(&q)); //20

    return 0;
}
```
Code với mảng Struct
```cpp
#include <stdio.h>
#include <stdlib.h>

typedef struct{
    int* items;
    int size;
    int front, rear;
} Queue;

void initialize(Queue *queue, int size)
{
    queue->items = (int*) malloc(sizeof(int)* size);
    queue->front = -1;
    queue->rear = -1;
    queue->size = size;
}

int is_empty(Queue queue) {
    return queue.front == -1;
}

int is_full(Queue queue) {
    return (queue.rear + 1) % queue.size == queue.front;
}

void enqueue(Queue *queue, int value) {
    if (!is_full(*queue)) {
        if (is_empty(*queue)) {
            queue->front = queue->rear = 0;
        } else {
            queue->rear = (queue->rear + 1) % queue->size;
        }
        queue->items[queue->rear] = value;
    } else {
        printf("Queue overflow\n");
    }
}

int dequeue(Queue *queue) {
    if (!is_empty(*queue)) {
        int dequeued_value = queue->items[queue->front];
        if (queue->front == queue->rear) {
            queue->front = queue->rear = -1;
        } else {
            queue->front = (queue->front + 1) % queue->size;
        }
        return dequeued_value;
    } else {
        printf("Queue underflow\n");
        return -1;
    }
}

int front(Queue queue) {
    if (!is_empty(queue)) {
        return queue.items[queue.front];
    } else {
        printf("Queue is empty\n");
        return -1;
    }
}

int main() {
    Queue queue;
    initialize(&queue, 3);

    enqueue(&queue, 10);
    enqueue(&queue, 20);
    enqueue(&queue, 30);

    printf("Front element: %d\n", front(queue)); //10

    printf("Dequeue element: %d\n", dequeue(&queue)); //10
    printf("Dequeue element: %d\n", dequeue(&queue)); //20

    printf("Front element: %d\n", front(queue)); //30

    enqueue(&queue, 40);
    enqueue(&queue, 50);
    printf("Dequeue element: %d\n", dequeue(&queue));//30
    printf("Front element: %d\n", front(queue));//40

    return 0;
}
```
</details>

</details>

# Bài 12 : Binary search - File operations - Code standards

<details>
<summary> Details </summary>
    
## 1. Binary search:
<details>
<summary> Details </summary>

- Tìm kiếm nhị phân (Binary Search) là một thuật toán tìm kiếm hiệu quả để tìm vị trí của một phần tử trong một danh sách đã được sắp xếp. Thay vì tìm kiếm tuần tự từng phần tử như tìm kiếm tuyến tính, tìm kiếm nhị phân chia danh sách thành hai phần và chỉ tiếp tục tìm kiếm trong nửa có khả năng chứa phần tử cần tìm.

Tìm kiếm nhị phân dựa trên nguyên lý "chia để trị" (divede and conquer):
- So sánh phần tử cần tìm với phần tử giữa của danh sách.
- Nếu phần tử cần tìm nhỏ hơn phần tử giữa, tìm kiếm tiếp tục trong nửa trái.
- Nếu phần tử cần tìm lớn hơn phần tử giữa, tìm kiếm tiếp tục trong nửa phải.
- Quá trình này lặp lại cho đến khi tìm thấy phần tử hoặc không còn phần tử nào để tìm kiếm.

Đặc điểm:
- Hiệu suất: Tìm kiếm nhị phân có thời gian tìm kiếm rất ngắn hơn so với tìm kiếm tuyến tính, đặc biệt đối với các danh sách lớn.
- Yêu cầu: Danh sách phải được sắp xếp trước khi áp dụng tìm kiếm nhị phân.

Ứng dụng:

Tìm kiếm nhị phân có nhiều ứng dụng trong các lĩnh vực khác nhau, đặc biệt là trong các bài toán và hệ thống cần tìm kiếm nhanh và hiệu quả. Một số ứng dụng tiêu biểu bao gồm:
- Tìm kiếm trong cơ sở dữ liệu: Các hệ quản trị cơ sở dữ liệu thường sử dụng tìm kiếm nhị phân để tìm kiếm dữ liệu nhanh chón trong các bảng hoặc chỉ mục đã sắp xếp (Thẻ RFID thang máy)
- Thư viện phần mềm và API: Nhiều thư viện và API cung cấp các chức năng tìm kiếm nhị phân để giúp lập trình viên thực hiện tìm kiếm nhanh chóng trong các tập dữ liệu lớn.
- Các bài toán trên mảng hoặc danh sách: Tìm kiếm nhị phân thường được sử dụng trong các bài toán xử lý mảng hoặc danh sách đã được sắp xếp, chẳng hạn như tìm kiếm số lần xuất hiện của một phần tử, tìm phần tử lớn nhất nhỏ hơn một giá trị cho trước, ...
- Cấu trúc dữ liệu dạng cây: Các cấu trúc dữ liệu như cây tìm kiếm nhị phân (Binary Search Tree - BST) sử dụng nguyên lý của tìm kiếm nhị phân để tổ chức và tìm kiếm dữ liệu.
- Ứng dụng trong thuật toán tối ưu hóa: Tìm kiếm nhị phân được sử dụng trong các thuật toán tối ưu hóa để tìm kiếm giá trị tối ưu trong một khoảng, ví dụ như tìm nghiệm của một phương trình hoặc tối thiếu hóa/ đại hóa một hàm số.
- Chương trình lập lịch trong hệ điều hành: Trong các hệ điều hành, tìm kiếm nhị phân có thể được sử dụng để quản lý và tìm kiếm nhanh chòng trong các bảng thời gian, quản lý bộ nhớ hoặc lập lịch CPU
- Ngành tài chính: Tìm kiếm nhị phân có thể được sử dụng trong các ứng dụng tài chính để tìm kiếm nhanh chóng trong các bảng giá cổ phiếu đã sắp xếp, hoặc xác định mức giá tối ưu trong giao dịch.

Ví dụ Thực Tiễn:

Một ví dụ thực tế về ứng dụng của tìm kiếm nhị phân là trong việc tìm kiếm một cuốn sách trong thư viện. Nếu thư viện sắp xếp sách theo bảng chữ cái, bạn có thể nhanh chóng chia danh sách sách thành hai phần và quyết định tìm kiếm tiếp theo ở nửa nào dựa trên tên cuốn sách mà bạn cần tìm. Điều này giúp tiết kiệm thời gian hơn nhiều so với việc tìm kiếm từng cuốn một.

Bài tập: Sử dụng tìm kiếm nhị phân với đối với database có struct là Họ và tên, Tuổi, Địa chỉ, Số điện thoại
```cpp
#include <stdio.h>
#include <stdlib.h>
#include "..\include\mString.h"

typedef struct Node {
    struct {
        char *fullName;
        int age;
        char *address;
        long long phoneNumber;
    }value;
    struct Node *next;
}Node;

typedef struct{
    struct Node *treeFullname;
    struct Node *treePhonenumber;
}mainTree;

Node *createNode(char *strName, int valueAge, char *strAddress, long long valueNumber){
    Node *temp = (Node*)malloc(sizeof(Node));
    temp->value.fullName = strName;
    temp->value.age = valueAge;
    temp->value.address = strAddress;
    temp->value.phoneNumber = valueNumber;
    temp->next = NULL;
    return temp;
};

//add list with phone number
void addNodeWithPhoneNumber(Node **head, char *strName, int valueAge, char *strAddress, long long valueNumber) {
    Node *newNode = createNode(strName, valueAge, strAddress, valueNumber);

    if (*head == NULL || (*head)->value.phoneNumber >= valueNumber){
        newNode->next = *head;
        *head = newNode;
    }
    else {
        Node *temp = *head;
        while (temp->next != NULL && temp->next->value.phoneNumber < valueNumber)
        {
            temp = temp->next;
        }
        newNode->next = temp->next;
        temp->next = newNode;
    }
};

//add list with full name
void addNodeWithFullName(Node **head, char *strName, int valueAge, char *strAddress, long long valueNumber) {
    Node *newNode = createNode(strName, valueAge, strAddress, valueNumber);

    if (*head == NULL || compareString((*head)->value.fullName,strName) > 0){
        newNode->next = *head;
        *head = newNode;
    }

    else {
        Node *temp = *head;
        while (temp->next != NULL && compareString(temp->next->value.fullName,strName) <= 0)
        {
            temp = temp->next;
        }
        newNode->next = temp->next;
        temp->next = newNode;
    }
};

void addNode(mainTree **head, char *strName, int valueAge, char *strAddress, long long valueNumber){
    if (*head == NULL){
        *head = (mainTree*)malloc(sizeof(mainTree));
        (*head)->treeFullname = NULL;
        (*head)->treePhonenumber = NULL;
    }
    addNodeWithFullName(&((*head)->treeFullname),strName, valueAge, strAddress, valueNumber);
    addNodeWithPhoneNumber(&((*head)->treePhonenumber),strName, valueAge, strAddress, valueNumber);
};

void freeList(Node **head) {
    Node *current = *head;
    Node *nextNode;

    while (current != NULL) {
        nextNode = current->next;
        free(current);
        current = nextNode;
    }

    *head = NULL;
};

void freeMainTree(mainTree **tree) {
    if (*tree == NULL) {
        return;
    }

    freeList(&((*tree)->treeFullname));

    freeList(&((*tree)->treePhonenumber));

    free(*tree);

    *tree = NULL;
}


//Search to Type
typedef enum{
    SEARCH_FULLNAME,
    SEARCH_PHONENUMBER,
    SEARCH_AGE,
    SEARCH_ADDRESS
}SearchType;

//Binary Search Struct
typedef struct CenterPoint {
    union {
        char *fullName;
        long long phoneNumber;
    } value;
    struct CenterPoint *left;
    struct CenterPoint *right;
} CenterPoint;

//Create build Tree with Fullname
CenterPoint *buildTree(Node *head, int start, int end, SearchType type) {
    if (head == NULL || start > end) {
        return NULL;
    }

    int mid = (start + end)/2;
    Node *node = head;
    for (int i = start; i < mid; i++) {
        if (node->next == NULL) {
            break;
        }
        node = node->next;
    }

    CenterPoint *root = (CenterPoint *) malloc(sizeof(CenterPoint));
    switch (type)
    {
    case 0:
        root->value.fullName = node->value.fullName;
        break;
    case 1:
        root->value.phoneNumber = node->value.phoneNumber;
        break;
    default:
        free(root);
        return NULL;
        break;
    }

    root->left = buildTree(head, start, mid - 1,type);
    root->right = buildTree(node->next, mid + 1, end,type);

    return root;
}

CenterPoint *centerPoint(Node *head, SearchType type) {
    int length = 0;
    Node *node = head;
    while (node != NULL) {
        node = node->next;
        length++;
    }

    return buildTree(head, 0, length - 1,type);
}

//Binary Search With String
CenterPoint *binarySearchString(CenterPoint *root, const char *str,SearchType type) {
    static int loop = 0;
    loop++;
    printf("so lan lap: %d\n", loop);
    if (root == NULL) {
        return NULL;
    }
    
    switch (type)
    {
    case 0:
        if (compareString(root->value.fullName,str) == 0) {
            return root;
        }

        if (compareString(root->value.fullName,str) > 0) {
            return binarySearchString(root->left, str, type);
        } else {
            return binarySearchString(root->right, str, type);
        }

        break;
    // case 3:
    //     if (compareString(root->value.address) == 0) {
    //     return root;
    //     }

    //     if (compareString(root->value.address,str) < 0) {
    //         return binarySearch(root->left, str, type);
    //     } else {
    //         return binarySearch(root->right, str, type);
    //     }

    //     break;
    default:
        return NULL;
        break;
    }

}

//Binary Search With String
CenterPoint *binarySearchNumber(CenterPoint *root, long long value,SearchType type) {
    static int loop = 0;
    loop++;
    printf("so lan lap: %d\n", loop);
    if (root == NULL) {
        return NULL;
    }
    
    switch (type)
    {
    case 1:
        if (root->value.phoneNumber == value) {
            return root;
        }

        if (value < root->value.phoneNumber) {
            return binarySearchNumber(root->left, value, type);
        } else {
            return binarySearchNumber(root->right, value, type);
        }
        break;
    // case 2:
    //     if (root->value.age == value) {
    //         return root;
    //     }

    //     if (value < root->value.age) {
    //         return binarySearch(root->left, value, type);
    //     } else {
    //         return binarySearch(root->right, value, type);
    //     }
    //     break;
    default:
        return NULL;
        break;
    }
}

//Print Node list
void print_list(Node *head){
    Node *temp = head;
    while (temp != NULL)
    {
        printf("Full name is: %s, ", temp->value.fullName);
        printf("Age is: %d, ", temp->value.age);
        printf("Address is: %s, ", temp->value.address);
        printf("Phone number is: %lld, ", temp->value.phoneNumber);
        temp = temp->next;
    }

    printf("\n");
};

int main()
{
    mainTree *Person = NULL;
    addNode(&Person,"Nguyen Thi Hoa", 22, "Ha Noi 1", 84345771057);
    addNode(&Person,"Nguyen Hoang Nhat", 23, "Ha Noi 2", 84365871058);
    addNode(&Person,"Nguyen Huu Y", 24, "Ha Noi 3", 84345471056);
    addNode(&Person,"Nguyen Phong Hoang", 25, "Ha Noi 4", 84365871057);
    addNode(&Person,"Nguyen Thi Thuy Lan", 26, "Ha Noi 5", 84342871059);

    print_list(Person->treeFullname);

    CenterPoint *ptrFullname = centerPoint(Person->treeFullname,SEARCH_FULLNAME);
    CenterPoint *ptrPhonenumber = centerPoint(Person->treePhonenumber,SEARCH_PHONENUMBER);

    char *Name = "Nguyen Hoang Nhat";
    CenterPoint *cprFullname = binarySearchString(ptrFullname, Name,SEARCH_FULLNAME);
    if (cprFullname != NULL){
        printf("Tim thay: %s\n",Name);
    }
    else {
        printf("Khong tim thay: %s\n",Name);
    }

    long long value = 84342871159;
    CenterPoint *cprPhonenumber = binarySearchNumber(ptrPhonenumber, value,SEARCH_PHONENUMBER);
    if (cprPhonenumber != NULL){
        printf("Tim thay: %lld\n",value);
    }
    else {
        printf("Khong tim thay: %lld\n",value);
    }
    
}
```

</details>

## 2. File operations:
<details>
<summary> Details </summary>

- File operation cho phép đọc và ghi dữ liệu vào các tệp tin trên hệ thống.

Để mở một file, sử dụng hàm fopen(). Hàm này trả về một con trỏ FILE.
> FILE *file = fopen(const char *file_name, const char *access_mode);

Các chế độ của file operators:
- r : Mở file ở chế độ chỉ đọc file. Nếu mở thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không trả về NULL.
- rb : Mở file với chế độ chỉ đọc file theo định dạng binary. Nếu mở thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không trả về NULL.
- w : Mở file với chế độ ghi vào file. Nếu file đã tồn tại, thì sẽ ghi đè vào nội dung bên trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.
- wb : Mở file với chế độ ghi vào file theo định dạng binary. Nếu file đã tồn tại, thì sẽ ghi đè vào nội dung bên trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.
- a : Mở file với chế độ nối. Nếu mở file thành công thì trả về địa chỉ của phần tử cuối cùng trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.
- ab : Mở file với chế độ nối dưới định dạng binary. Nếu mở file thành công thì trả về địa chỉ của phần tử cuối cùng trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.
- r+ : Mở file với chế độ đọc và ghi file. Nếu mở file thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không thì trả về NULL.
- rb+ : Mở file với chế độ đọc và ghi file dưới định dạng binary. Nếu mở file thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không thì trả về NULL.
- w+ : Mở file với chế độ ghi và đọc file. Nếu file đã tồn tại thì trả về địa chỉ của phần tử đầu tiên của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.
- wb+ : Mở file với chế độ ghi và đọc file dưới định dạng binary. Nếu file đã tồn tại thì trả về địa chỉ của phần tử đầu tiên của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.
- a+ : Mở file với chế độ nối và đọc file. Nếu file đã tồn tại thì trả về địa chỉ của phần tử cuối cùng của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.
- ab+ :  Mở file với chế độ nối và đọc file dưới định dạng binary. Nếu file đã tồn tại thì trả về địa chỉ của phần tử cuối cùng của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.

Các hàm của file operators: <br>
* Đọc file:
- fscanf() : Sử dụng chuỗi được định dạng và danh sách đối số biến để lấy đầu vào từ một File.
- fgets() : Copy nội dung trong File vào mảng dùng để lưu trữ với tối đa số lượng phần tử của mảng hoặc tới khi gặp ký tự xuống dòng.
- fgetc() : Lấy giá trị tại địa chỉ hiện tại của file, sau đó di chuyển tới địa chỉ tiếp theo. Kiểu trả về là char
- fread() : Đọc một số lượng byte được chỉ định từ File .bin

* Ghi file:
- fprintf() : Ghi chuỗi vào File, và có thể thêm danh sách các đối số.
- fputs() : Ghi chuỗi vào File.
- fputc() : Ghi một ký tự vào File.
- fwrite() : Ghi một số byte được chỉ định vào File .bin

Các hàm khác:
- fclose(): Đóng File đã mở.
- feof(): Để kiểm tra địa chỉ hiện tại có phải ký tự cuối cùng của File hay chưa.

</details>

## 3. Code standards:
<details>
<summary> Details </summary>

[Các quy tắc về đặt tên theo tiêu chuẩn "Autosar C Coding Guidelines"](https://hala.edu.vn/c-co-ban/cac_quy_tac_ve_dat_ten_theo_tieu_chuan_autosar_c_coding_guidelines/)

</details>

</details>


# Bài 13 : Class

<details>
<summary> Details </summary>

## 1. Class:
<details>
<summary> Details </summary>

- Class là một kiểu dữ liệu do người dùng tự định nghĩa, chứa các thuộc tính (properties) và phương thức (methods) để mô tả một đối tượng cụ thể. Class là phần cốt lõi của lập trình hướng đối tượng (OOP) trong C++.

```cpp
class ClassName {
public:
    // Properties and methods
};
```
</details>

## 2. Object

<details>
<summary> Details </summary>

- Object là một thực thể cụ thể được tạo ra từ class. Mỗi object chứa dữ liệu và có thể thực hiện các hành động được mô tả bởi class.

```cpp
Animal animal1;  // Create object from class Animal
animal1.speak(); // Call speak methods of object
```

</details>

## 3. Constructer

<details>
<summary> Details </summary>

- Constructor là một hàm đặc biệt được gọi tự động khi một đối tượng của class được tạo ra. Constructor giúp khởi tạo các thuộc tính của đối tượng. Trong C++, constructor có cùng tên với class và không có kiểu trả về (kể cả void).

```cpp
class Animal {
public:
    string name;
    
    // Constructor
    Animal(string animalName) {
        name = animalName;
    }
    
    void speak() {
        cout << name << " is speaking!" << endl;
    }
};
```
Khi tạo đối tượng, constructor sẽ tự động được gọi:
```cpp
Animal dog("Buddy");
dog.speak();
```

</details>

## 4.  Destructor

<details>
<summary> Details </summary>

- Destructor là một hàm đặc biệt được gọi tự động khi đối tượng bị hủy hoặc không còn được sử dụng nữa. Destructor có cùng tên với class, nhưng có dấu ~ ở đầu và không có tham số.

```cpp
class Animal {
public:
    string name;
    
    // Constructor
    Animal(string animalName) {
        name = animalName;
        cout << name << " is created!" << endl;
    }
    
    // Destructor
    ~Animal() {
        cout << name << " is destroyed!" << endl;
    }
};
```
Khi đối tượng ra khỏi phạm vi, destructor sẽ tự động được gọi:
```cpp
int main() {
    Animal dog("Buddy");
    return 0;
}
```
Output:
```cpp
Buddy is created!
Buddy is destroyed!
```

</details>

## 5. Properties

<details>
<summary> Details </summary>

- Properties là các biến thành viên (member variables) của class, lưu trữ dữ liệu hoặc trạng thái của đối tượng. Chúng có thể là public, private, hoặc protected.

```cpp
class Animal {
public:
    string name;
    int age;
};
```

</details>

## 6. Method

<details>
<summary> Details </summary>

- Method là các hàm thành viên (member functions) của class, dùng để thực hiện các hành động hoặc thao tác trên các thuộc tính của đối tượng. Phương thức có thể được định nghĩa bên trong hoặc bên ngoài class.

```cpp
class Animal {
public:
    string name;

    //Inside class
    void speak() {
        cout << name << " is speaking!" << endl;
    }
};

//Outside class
void Animal::speak() {
    cout << name << " is speaking!" << endl;
}
```

</details>

## 7. Access Modifiers

<details>
<summary> Details </summary>

- Trong C++, các thuộc tính và phương thức của class có thể được kiểm soát thông qua các từ khóa public, private, và protected.

a. Public

- Các thuộc tính và phương thức public có thể được truy cập từ bất kỳ đâu (bên trong hoặc bên ngoài class).

```cpp
class Animal {
public:
    string name;    // Public attribute
    void speak() {  // Public method
        cout << name << " is speaking!" << endl;
    }
};
```
Có thể truy cập từ bên ngoài class:
```cpp
Animal dog;
dog.name = "Buddy";
dog.speak();
```

b. Private

- Các thuộc tính và phương thức private chỉ có thể được truy cập từ bên trong class hoặc bởi các phương thức của class đó. Không thể truy cập trực tiếp từ bên ngoài class.

```cpp
class Animal {
private:
    string name;    // Private attribute
public:
    Animal(string animalName) {
        name = animalName;  // Có thể truy cập name từ bên trong class
    }
    
    void speak() {
        cout << name << " is speaking!" << endl;
    }
};
```
dog.name sẽ không thể truy cập được thuộc tính private name
```cpp
Animal dog("Buddy");
// dog.name = "Charlie";  // Error: private attribute is not accessible
dog.speak();              // Call public methods
```

c. Protected

- Các thuộc tính và phương thức protected chỉ có thể được truy cập từ bên trong class hoặc từ các class dẫn xuất (class con). Không thể truy cập từ bên ngoài class.

```cpp
class Animal {
protected:
    string name;    // Protected attribute
    
public:
    Animal(string animalName) {
        name = animalName;
    }
};
```

</details>

## 8. Inheritance

<details>
<summary> Details </summary>

- Inheritance là cơ chế cho phép một class (class con) kế thừa các thuộc tính và phương thức từ một class khác (class cha). Điều này giúp tái sử dụng mã và mở rộng tính năng của class cha.

```cpp
class ChildClass : public ParentClass {
    // Methods và properties of child class
};
```
Example:
```cpp
class Animal {
protected:
    string name;
    
public:
    Animal(string animalName) {
        name = animalName;
    }
    
    void speak() {
        cout << name << " is speaking!" << endl;
    }
};

class Dog : public Animal {
public:
    Dog(string dogName) : Animal(dogName) {}  // Gọi constructor của class cha
    
    void bark() {
        cout << name << " is barking!" << endl;
    }
};
```
```cpp
Dog dog("Buddy");
dog.speak();
dog.bark();
```

</details>

## 9. Static Members

<details>
<summary> Details </summary>

- Các thành viên tĩnh (static members) thuộc về class chứ không thuộc về một đối tượng cụ thể. Điều này có nghĩa là tất cả các đối tượng của class sẽ chia sẻ cùng một bản sao của thành viên tĩnh.

```cpp
class ClassName {
public:
    static int staticVariable;  // Static Variable
    static void staticMethod(); // Static Method
};
```
Example:
```cpp
class Counter {
public:
    static int count;  // Static Variable
    
    Counter() {
        count++;
    }
    
    static void showCount() {  // Static Method
        cout << "Count: " << count << endl;
    }
};

int Counter::count = 0;  // Khởi tạo biến tĩnh

int main() {
    Counter c1, c2;
    Counter::showCount();  // Output: Count: 2
    return 0;
}
```

</details>
    
## 10. Tổng kết:

<details>
<summary> Details </summary>

- Class là khuôn mẫu định nghĩa các thuộc tính và hành vi của đối tượng.<br>
- Object là thực thể cụ thể được tạo ra từ class.<br>
- Constructor và Destructor là các hàm đặc biệt để khởi tạo và hủy đối tượng.<br>
- Properties là các biến lưu trữ dữ liệu của đối tượng.<br>
- Method là các hàm thực hiện hành động của đối tượng.<br>
- Access Modifiers như public, private, và protected để kiểm soát quyền truy cập.<br>
- Inheritance cho phép tái sử dụng và mở rộng class.<br>
- Static Members là các thành viên thuộc về class, không phải đối tượng.<br>

</details>

</details>

# Bài 14 : OOP

<details>
<summary> Details </summary>

## 1. Các phương pháp lập trình:

<details>
<summary> Details </summary>

Có 4 phương pháp lập trình phổ biến hiện nay là:

### 1.1. Phương pháp lập trình hướng lệnh

Xem chương trình là tập hợp các lệnh. Khi đó việc viết chương trình là xác định xem chương trình gồm những lệnh nào, thứ tự thực hiện của các lệnh ra sao.

Ví dụ: Một chương trình có nhiều câu lệnh nhập xuất dữ liệu viết chung trong hàm main.

### 1.2. Phương pháp lập trình hướng thủ tục, hàm

Trong phương pháp này người ta xem chương trình là một hệ thống các thủ tục và hàm. Trong đó, mỗi thủ tục và hàm là một dãy các lệnh được sắp thứ tự. Khi đó, việc viết chương trình là xác định xem chương trình gồm các thủ tục và hàm nào, mối quan hệ giữa chúng ra sao?

Ví dụ: Chương trình được chia rõ ra thành các hàm riêng biệt và khi thực hiện 1 chức năng cụ thể thì gọi hàm tương ứng. 

### 1.3. Phương pháp lập trình hướng đơn thể/ Lập trình cấu trúc

Phương pháp lập trình đơn thể xem chương trình là một hệ thống các đơn thể, mỗi một đơn thể là một hệ thống các thủ tục và hàm.

Có 2 dạng đơn thể: 

- Đơn thể chức năng: Bao gồm các hàm và thủ tục thực hiện 1 chức năng nào đó. Ví dụ như thư viện <math.h> thực hiện các chức năng tính toán.<br>
- Đơn thể dữ liệu: Bao gồm các hàm và thủ tục cho 1 kiểu dữ liệu. Ví dụ thư viện <string.h> cung cấp các hàm xử lý chuỗi.<br>
- Phương pháp lập trình cấu trúc sử dụng kiểu dữ liệu Struct. Trong đó, mỗi một thực thể sẽ bao gồm nhiều thuộc tính khác nhau được gom chung thành một kiểu dữ liệu mới.

Ví dụ như thực thể "Con người" sẽ bao gồm nhiều thuộc tính như: tên, tuổi, năm sinh,... vv

### 1.4. Phương pháp lập trình hướng đối tượng (OOP).

Xem chương trình là một hệ thống các đối tượng. Mỗi một đối tượng là sự bao bọc bên trong nó 2 thành phần: thuộc tính và phương thức.

- Thuộc tính: là các thông tin của đối tượng đó. Ví dụ đối tượng “Con Người” có các thuộc tính là màu da, quốc tịch, chiều cao, cân nặng, vvv…<br>
- Phương thức: Là các hành động mà đối tượng đó có thể thực hiện. Ví dụ đối tượng “Con Người” có các phương thức là ăn, chạy, nói, vvv…<br>
- Các đối tượng được tạo ra như 1 đơn thể chứa dữ liệu và các phương thức của riêng đối tượng đó. Người lập trình có thể tạo ra nhiều đối tượng khác nhau để sử dụng.

Lập trình hướng đối tượng có 4 tính chất:<br>
- Tính đóng gói
- Tính kế thừa
- Tính đa hình
- Tính trừu tượng

</details>

## 2. Encapsulation (Tính đóng gói)

<details>
<summary> Details </summary>

**Tính đóng gói** cho phép chúng ta che dấu thông tin và hành vi của một đối tượng trong một lớp, và chỉ cho phép truy cập vào chúng thông qua các phương thức công khai (public methods).

Để thực hiện tính đóng gói trong C++, chúng ta sử dụng các phạm vi truy cập (access specifiers) như public, private, và protected trong class:

- public: Các thành viên hoặc phương thức được khai báo ở phạm vi public có thể truy cập từ bất kỳ đâu bên ngoài lớp.
- private: Các thành viên hoặc phương thức được khai báo ở phạm vi private chỉ có thể truy cập từ bên trong lớp đó.
- protected: Các thành viên hoặc phương thức được khai báo ở phạm vi protected có thể truy cập từ bên trong lớp đó và các lớp kế thừa từ lớp đó.

**Ý nghĩa của tính đóng gói**

- Bảo vệ dữ liệu: Các thuộc tính (biến thành viên) của lớp có thể được khai báo là private để ngăn chặn việc truy cập trực tiếp từ ngoài lớp. Việc thay đổi dữ liệu chỉ có thể được thực hiện thông qua các phương thức công khai (public methods), giúp kiểm soát và bảo vệ dữ liệu.
- Kiểm soát cách thức truy cập và thay đổi giá trị của thuộc tính: Tính đóng gói cho phép nhà phát triển quy định cách thức mà dữ liệu được truy cập hoặc thay đổi bằng cách sử dụng các phương thức getter và setter.
- Giảm sự phụ thuộc: Các chi tiết bên trong của lớp được ẩn đi, giúp giảm sự phụ thuộc giữa các phần của chương trình. Điều này làm cho mã dễ bảo trì và thay đổi hơn.

Example:
```cpp
#include <iostream>
using namespace std;

class Person {
private:
    string name;
    int age;

public:
    Person(string n, int a);
    void setAge(int a);
    int getAge();
    void display();
};

// Constructor
Person::Person(string n, int a) {
    name = n;
    age = a;
};

// Setter
void Person::setAge(int a){
    if (a > 0) {
        age = a;
    } else {
        cout << "Age must be positive!" << endl;
    }
};

// Getter
int Person::getAge() {
    return age;
};

// Display method
void Person::display() {
    cout << "Name: " << name << ", Age: " << age << endl;
};

int main() {
    Person person1("John", 30);

    person1.display();

    person1.setAge(25);
    person1.display();

    person1.setAge(-5);
    person1.display();

    return 0;
}
```
Output
```cpp
Name: John, Age: 30
Name: John, Age: 25
Age must be positive!
Name: John, Age: 25
```

</details>

## 3. Inheritance (Tính kế thừa)

<details>
<summary> Details </summary>

**Tính kế thừa** cho phép một lớp con (subclass) kế thừa các thuộc tính và phương thức từ một lớp cha (superclass). Lớp con có thể sử dụng lại mã của lớp cha và có thể mở rộng hoặc thay đổi các hành vi của nó.<br>
Trong C++, chúng ta sử dụng từ khóa class hoặc struct để định nghĩa lớp con và sử dụng dấu hai chấm : để chỉ ra lớp cha mà lớp con muốn kế thừa.

```cpp
class Animal {
public:
    void eat() {
        cout << "Animal is eating" << endl;
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "Dog is barking" << endl;
    }
};
```

Các thuộc tính và phương thức của lớp cha có thể được kế thừa với các phạm vi truy cập khác nhau như public, private, và protected.

- Nếu một thuộc tính hoặc phương thức của lớp cha được khai báo là public, thì nó sẽ được kế thừa với phạm vi public trong lớp con. Điều này có nghĩa là các thành viên của lớp con và bên ngoài lớp con đều có thể truy cập vào chúng.
- Nếu một thuộc tính hoặc phương thức của lớp cha được khai báo là private, thì nó sẽ không được kế thừa với phạm vi private trong lớp con. Điều này có nghĩa là chỉ các phương thức của lớp cha mới có thể truy cập vào chúng, còn lớp con không thể truy cập trực tiếp.
- Nếu một thuộc tính hoặc phương thức của lớp cha được khai báo là protected, thì nó sẽ được kế thừa với phạm vi protected trong lớp con. Điều này có nghĩa là các thành viên của lớp con có thể truy cập vào chúng, nhưng bên ngoài lớp con không thể truy cập trực tiếp.

Example 1:
```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    void eat() {
        cout << "Animal is eating" << endl;
    }

private:
    void sleep() {
        cout << "Animal is sleeping" << endl;
    }

protected:
    void move() {
        cout << "Animal is moving" << endl;
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "Dog is barking" << endl;
    }

    void play() {
        move(); // Có thể truy cập vào phương thức protected của lớp cha
        // sleep(); // Không thể truy cập vào phương thức private của lớp cha
    }
};

int main() {
    Dog myDog;
    myDog.eat(); // Có thể truy cập vào phương thức public của lớp cha
    // myDog.sleep(); // Không thể truy cập vào phương thức private của lớp cha
    // myDog.move(); // Không thể truy cập vào phương thức protected của lớp cha
    myDog.bark();
    myDog.play();

    return 0;
}
```
Example 2:
```cpp
#include <iostream>
#include <string>

using namespace std;

class DoiTuong{
    protected:
        int ID;
        string TEN;
    public:
        DoiTuong();
        void input(string ten);
        void display();
};

Doituong::Doituong() {
    static int id = 100;
    ID = id;
}

void DoiTuong::input(string ten) {
    TEN = ten;
}

void DoiTuong::display() {
    cout << "ID: " << ID << endl;
    cout << "TEN: " << TEN << endl;
}

class SinhVien : public DoiTuong {
    private:
        string LOP;
        string HOCKY;
    public:
        void input(string ten, string lop, string hocky); //override
        void display();
}

void SinhVien::input(string ten, string lop, string hocky){
    TEN = ten;
    LOP = lop;
    HOCKY = hocky;
}

void Sinhvien::display(){
    cout << "TEN: " << TEN << endl;
    cout << "LOP: " << LOP << endl;
    cout << "HOC KY: " << HOCKY << endl;
}

int main() {
    SinhVien sv;
    sv.input("Hoang", "CK12" , "HOCKY3");
    sv.display();
    return 0;
}

```

Trên là trường hợp kế thừa theo public DoiTuong (superclass), khi muốn kế thừa theo protected DoiTuong thì các method và đối tượng ở class cha sẽ chuyển qua class con ở chế độ protected, trường hợp còn lại thì muốn kế thừa theo private DoiTuong thì các method và đối tương ở class cha sẽ chuyển qua class con ở chế độ private.

Câu hỏi đặt ra: Nếu chúng ta khai báo 1 class cha (superclass) với 1 con trỏ DoiTuong *ptr thì khi chúng ta trỏ đến địa chỉ 1 class con (subclass) ptr = &sv thì ở phần ptr->display() in ra gì?

Nó ra sẽ trỏ ở class cha (superclass) nhưng vẫn sử dụng dữ liệu của class con.

Vậy để có thể display của sv thì chúng ta sử dụng kĩ thuật overload hoặc override của tính đa hình.

Ví dụ: Ta tạo class cha và class con mà trong class con không có phương thức display thì khi gọi display method thì nó sẽ gọi object hoặc method cùng cấp với nó, chẳng hạn như cả 2 cùng có function char *word nhưng ở đây nó sẽ gọi function ở class cha nếu display có chứa object word.

</details>

## 4. Polymorphism (Tính đa hình)

<details>
<summary> Details </summary>

**Tính đa hình** cho phép các đối tượng thuộc các lớp khác nhau có thể được xử lý như là các đối tượng của cùng một lớp cha. Điều này giúp chương trình linh hoạt hơn trong việc xử lý các đối tượng khác nhau dựa trên cùng một giao diện hoặc phương thức.

Có hai cách thực hiện tính đa hình:
- Đa hình qua ghi đè phương thức (Method Overriding): Lớp con có thể ghi đè lại phương thức của lớp cha.
- Đa hình qua nạp chồng phương thức (Method Overloading): Nhiều phương thức trong cùng một lớp có thể có cùng tên nhưng khác tham số.

Example:<rbr>
### 3.1. Đa hình qua kế thừa (Runtime Polymorphism)
Đây là loại đa hình mà bạn thấy khi sử dụng phương thức ảo. Điều này cho phép bạn gọi phương thức của class con thông qua một con trỏ hoặc tham chiếu của class cha.
```cpp
#include <iostream>
using namespace std;

class Hinh {
public:
    virtual void draw() { // Phương thức ảo
        cout << "Vẽ hình chung" << endl;
    }
};

class HinhTron : public Hinh {
public:
    void draw() override { // Ghi đè phương thức
        cout << "Vẽ hình tròn" << endl;
    }
};

class HinhVuong : public Hinh {
public:
    void draw() override { // Ghi đè phương thức
        cout << "Vẽ hình vuông" << endl;
    }
};

int main() {
    Hinh* ptr; // Con trỏ đến class cha
    HinhTron h1; // Đối tượng của class con
    HinhVuong h2; // Đối tượng của class con

    ptr = &h1; // Gán địa chỉ của hình tròn
    ptr->draw(); // Gọi phương thức draw, in ra "Vẽ hình tròn"

    ptr = &h2; // Gán địa chỉ của hình vuông
    ptr->draw(); // Gọi phương thức draw, in ra "Vẽ hình vuông"

    return 0;
}

```

### 3.2. Đa hình qua nạp chồng (Compile-time Polymorphism)
Đây là loại đa hình mà bạn thấy khi sử dụng nạp chồng hàm (function overloading) hoặc nạp chồng toán tử (operator overloading).<br>
Ví dụ về nạp chồng hàm: Khi chúng ta khai báo parameter thì chúng ta có thể khai báo khác nhau về kiểu và số lượng.
```cpp
#include <iostream>
using namespace std;

class ToanHoc {
public:
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
};

int main() {
    ToanHoc th;
    cout << "Tổng nguyên: " << th.add(5, 10) << endl; // Gọi hàm add(int, int)
    cout << "Tổng thực: " << th.add(5.5, 10.5) << endl; // Gọi hàm add(double, double)
    return 0;
}
```
Ví dụ về nạp chồng toán tử:
```cpp
#include <iostream>
using namespace std;

class PHAN_SO {

    int tuSo;
    int mauSo;
public:
    PHAN_SO(){
        
    }
    PHAN_SO(int tu, int mau)
    {
        tuSo = tu;
        mauSo = mau;
    }
    
    PHAN_SO operator+ (PHAN_SO ps)          //Nap chong toan tu +
    {
        PHAN_SO ketqua;
        ketqua.tuSo = tuSo*ps.mauSo + mauSo*ps.tuSo;
        ketqua.mauSo = mauSo*ps.mauSo;
        return ketqua; 
    }
    void xuatThongTin()
    {
        cout << "Tu so: " << tuSo << endl;
        cout << "Mau so: " << mauSo << endl;
    }
};


int main()
{
    PHAN_SO ps1(2, 5);
    PHAN_SO ps2(3, 4);
    PHAN_SO tong = ps2 + ps1;                //Su dung toan tu + de tinh tong 2 phan so
    tong.xuatThongTin();

    return 0;
}
```
Tóm tắt:
- Đa hình qua kế thừa (Runtime Polymorphism) cho phép bạn gọi các phương thức của class con thông qua con trỏ hoặc tham chiếu của class cha bằng cách sử dụng phương thức ảo.
- Đa hình qua nạp chồng (Compile-time Polymorphism) cho phép bạn sử dụng nhiều hàm hoặc toán tử với cùng một tên nhưng với các tham số khác nhau.

</details>

## 5. Abstraction (Tính trừu tượng)

<details>
<summary> Details </summary>

Tính trừu tượng là quá trình ẩn đi các chi tiết phức tạp của hệ thống và chỉ cung cấp cho người dùng các chức năng chính. Điều này giúp đơn giản hóa việc sử dụng hệ thống và chỉ tập trung vào các khía cạnh cần thiết.

Example:
```cpp
class phuongTrinhBacHai(){
    private:
        int X1;
        int X2;
        float tinhDelta(int a, int b, int c);
        double KET_QUA;
    public:
        phuongTrinhBacHai(int a, int b, int c){
            tinhDelta(int a, int b, int c) >= 0;

            ...
        }
        double ketQua();
}
```
Ở ví dụ trên thì, quá trình tính delta đã được ẩn đi khi nó nằm trong private và protected. Đây là cách sử dụng tính trừu tượng khác với tính đóng gói bởi vì mặc dù nó cùng nằm trong private và protected nhưng ý nghĩa khác nhau.

Trong OOP, tính trừu tượng thường được thực hiện thông qua lớp trừu tượng (abstract class) hoặc giao diện (interface).

### 5.1. Lớp Trừu Tượng (Abstract Class)
Một lớp được coi là trừu tượng nếu nó có ít nhất một phương thức ảo thuần túy (pure virtual function). Lớp này không thể được khởi tạo và thường được sử dụng làm cơ sở cho các lớp khác.

Example:
```cpp
#include <iostream>
using namespace std;

class Hinh {
public:
    // Phương thức ảo thuần túy
    virtual void draw() = 0; 
};

class HinhTron : public Hinh {
public:
    void draw() override { // Ghi đè phương thức
        cout << "Vẽ hình tròn" << endl;
    }
};

class HinhVuong : public Hinh {
public:
    void draw() override { // Ghi đè phương thức
        cout << "Vẽ hình vuông" << endl;
    }
};

int main() {
    Hinh* hinh1 = new HinhTron();
    Hinh* hinh2 = new HinhVuong();

    hinh1->draw(); // In ra "Vẽ hình tròn"
    hinh2->draw(); // In ra "Vẽ hình vuông"

    delete hinh1;
    delete hinh2;
    return 0;
}

```
### 5.2. Giao Diện (Interface)

C++ không hỗ trợ giao diện theo cách như một số ngôn ngữ khác (như Java, Python), nhưng có thể mô phỏng giao diện bằng cách sử dụng lớp trừu tượng với các phương thức ảo thuần túy mà không có bất kỳ thuộc tính nào.
Hàm thuần ảo không thể tạo đối tượng cho class đó. Khi một lớp kế thừa từ một lớp trừu tượng, nó phải triển khai tất cả các hàm ảo thuần ảo.

Example:
```cpp
#include <iostream>
using namespace std;

class IAnimal {
public:
    virtual void speak() = 0; // Phương thức ảo thuần túy
};

class Dog : public IAnimal {
public:
    void speak() override {
        cout << "Woof!" << endl;
    }
};

class Cat : public IAnimal {
public:
    void speak() override {
        cout << "Meow!" << endl;
    }
};

int main() {
    IAnimal* dog = new Dog();
    IAnimal* cat = new Cat();

    dog->speak(); // In ra "Woof!"
    cat->speak(); // In ra "Meow!"

    delete dog;
    delete cat;
    return 0;
}

```

Tóm tắt:
- Tính trừu tượng giúp giảm thiểu độ phức tạp bằng cách chỉ hiển thị các thuộc tính và phương thức thiết yếu, ẩn đi các chi tiết không cần thiết.
- Lớp trừu tượng cho phép định nghĩa các phương thức mà các lớp con phải triển khai, tạo ra một giao diện chung cho các đối tượng khác nhau.
- Giao diện mô phỏng qua lớp trừu tượng giúp định nghĩa các phương thức mà các lớp thực thi cần phải có mà không cần xác định cách thức thực hiện.

</details>

</details>

# Bài 15 : Standard template library

<details>
<summary> Details </summary>

**Standard template library** là một thư viện mẫu chuẩn của C++, cung cấp các thành phần lập trình tổng quát để xử lý các cấu trúc dữ liệu và thuật toán. Nó được phát triển với mục tiêu cung cấp các công cụ hiệu quả, tái sử dụng và có thể mở rộng, giúp lập trình viên giải quyết các bài toán về thao tác dữ liệu mà không cần tự viết lại các cấu trúc dữ liệu và thuật toán cơ bản.

Standard template library bao gồm ba thành phần chính: container (cấu trúc dữ liệu), iterator (bộ lặp), và algorithm (thuật toán). Chúng kết hợp với nhau tạo nên một hệ thống linh hoạt và mạnh mẽ cho việc thao tác dữ liệu.

# 1. Containers (Cấu trúc dữ liệu)

<details>
<summary> Details </summary>

**Containers** là các cấu trúc dữ liệu tổng quát, cho phép lưu trữ và quản lý các đối tượng. STL cung cấp nhiều loại container khác nhau, mỗi loại có đặc điểm riêng để phục vụ các trường hợp sử dụng khác nhau. Dưới đây là một số container chính trong STL:

**Sequence Containers** (Cấu trúc dữ liệu tuần tự)

- **vector**: Một mảng động, có thể thay đổi kích thước. Truy cập phần tử nhanh (O(1)) nhưng việc chèn/xóa phần tử ở giữa có thể chậm (O(n)).
- **deque**: Hàng đợi hai đầu, cho phép thêm và xóa phần tử ở cả hai đầu với độ phức tạp O(1).
- **list**: Danh sách liên kết đôi, cho phép chèn/xóa phần tử ở bất kỳ đâu với độ phức tạp O(1), nhưng truy cập ngẫu nhiên chậm (O(n)).

**Associative Containers** (Cấu trúc dữ liệu ánh xạ)

- **set**: Tập hợp các phần tử duy nhất, được sắp xếp theo thứ tự. Chèn, xóa và tìm kiếm phần tử có độ phức tạp O(log n).
- **map**: Ánh xạ khóa-giá trị (key-value), với các khóa được sắp xếp. Tương tự như set, chèn/xóa/tìm kiếm có độ phức tạp O(log n).
- **multiset và multimap**: Tương tự như set và map, nhưng cho phép nhiều phần tử có giá trị khóa giống nhau.

**Unordered Containers** (Cấu trúc dữ liệu không sắp xếp)

- **unordered_set**: Tập hợp các phần tử duy nhất nhưng không đảm bảo thứ tự. Chèn/xóa/tìm kiếm có độ phức tạp trung bình O(1).
- **unordered_map**: Ánh xạ khóa-giá trị, không đảm bảo thứ tự, nhưng có độ phức tạp trung bình O(1) cho các thao tác chèn/xóa/tìm kiếm.
- **unordered_multiset và unordered_multimap**: Tương tự như unordered_set và unordered_map, nhưng cho phép các phần tử trùng lặp.

**Container Adaptors** (Bộ điều hợp cấu trúc dữ liệu)

- **stack**: Cấu trúc dữ liệu ngăn xếp (LIFO - Last In First Out), cho phép thêm và xóa phần tử ở đầu ngăn xếp.
- **queue**: Cấu trúc dữ liệu hàng đợi (FIFO - First In First Out), cho phép thêm phần tử vào cuối và xóa phần tử ở đầu.
- **priority_queue**: Hàng đợi ưu tiên, phần tử có độ ưu tiên cao nhất được xử lý trước.

</details>

## 2. Iterators (Bộ lặp)

<details>
<summary> Details </summary>

**Iterators** là các đối tượng trỏ đến phần tử trong container và cho phép duyệt qua các phần tử này. Chúng hoạt động tương tự như con trỏ. STL cung cấp nhiều loại iterator:

- **Input Iterator**: Cho phép đọc dữ liệu từ container.
- **Output Iterator**: Cho phép ghi dữ liệu vào container.
- **Forward Iterator**: Cho phép duyệt qua container theo hướng tiến.
- **Bidirectional Iterator**: Cho phép duyệt qua container theo cả hướng tiến và lùi.
- **Random Access Iterator**: Cho phép truy cập ngẫu nhiên vào các phần tử, tương tự như con trỏ mảng.

</details>

## 3. Algorithms (Thuật toán)

<details>
<summary> Details </summary>

Standard template library cung cấp một tập hợp phong phú các thuật toán để thao tác trên containers, chẳng hạn như sắp xếp, tìm kiếm, và xử lý các phần tử. Các thuật toán này phần lớn dựa trên các iterator để hoạt động trên mọi loại container.

Một số thuật toán phổ biến:

- **sort**: Sắp xếp các phần tử trong phạm vi chỉ định theo thứ tự tăng dần.
- **find**: Tìm kiếm phần tử trong phạm vi chỉ định.
- **for_each**: Áp dụng một hàm lên mỗi phần tử trong phạm vi chỉ định.
- **copy**: Sao chép các phần tử từ một phạm vi này sang phạm vi khác.
- **transform**: Áp dụng một hàm lên các phần tử và lưu kết quả vào phạm vi khác.
- **binary_search**: Tìm kiếm nhị phân trên một dãy đã sắp xếp.

</details>

## 4. Độ phức tạp của một thuật toán:

<details>
<summary> Details </summary>

**Độ phức tạp của một thuật toán** là một cách để đánh giá hiệu suất của nó, đặc biệt là khi kích thước đầu vào của bài toán tăng lên. Độ phức tạp thường được biểu diễn bằng ký hiệu Big-O (O), dùng để mô tả tốc độ tăng của thời gian thực hiện hoặc không gian bộ nhớ mà một thuật toán yêu cầu khi kích thước đầu vào tăng lên.

### 4.1. O(1) - Thời gian hằng số

- O(1) có nghĩa là thời gian thực hiện của thuật toán không thay đổi dù cho kích thước đầu vào tăng lên.
- Nói cách khác, thuật toán sẽ luôn thực hiện trong thời gian cố định hoặc thời gian hằng số, bất kể kích thước đầu vào là bao nhiêu.

Ví dụ: Truy cập một phần tử trong mảng bằng chỉ số (index) là O(1) vì thời gian truy cập một phần tử không phụ thuộc vào kích thước của mảng.

```cpp
int arr[] = {1, 2, 3, 4, 5};
int x = arr[2];  // Truy cập phần tử thứ 3 là O(1)
```

**Đặc điểm**:

- Nhanh nhất trong tất cả các độ phức tạp.
- Thường gặp trong các thao tác đơn giản như truy cập hoặc gán giá trị cho một biến.

### 4.2. O(n) - Thời gian tỷ lệ với kích thước đầu vào

- O(n) có nghĩa là thời gian thực hiện của thuật toán tăng tuyến tính theo kích thước đầu vào.
- Nếu đầu vào tăng gấp đôi, thời gian thực hiện cũng tăng gấp đôi.

Ví dụ: Duyệt qua tất cả các phần tử trong một mảng có n phần tử là O(n), vì thuật toán phải thực hiện một phép toán cho mỗi phần tử.

```cpp
int sum = 0;
for (int i = 0; i < n; ++i) {
    sum += arr[i];  // Tổng các phần tử của mảng có n phần tử
}
```
**Đặc điểm**:
- Tốc độ tuyến tính: Thời gian thực hiện tăng đều với kích thước đầu vào.
- Thường gặp trong các vòng lặp đơn xử lý từng phần tử một.

### 4.3. O(log n) - Thời gian tỷ lệ với logarit của kích thước đầu vào

- O(log n) có nghĩa là thời gian thực hiện chỉ tăng theo logarit của kích thước đầu vào. Điều này xảy ra khi mỗi bước của thuật toán cắt giảm lượng công việc cần làm theo một tỷ lệ cố định (ví dụ như chia đôi).
- Nếu đầu vào tăng lên gấp đôi, thì thời gian thực hiện chỉ tăng lên một lượng nhỏ (log₂(2n) - log₂(n) = 1).

Ví dụ:Tìm kiếm nhị phân trong một mảng đã sắp xếp là O(log n), vì mỗi lần tìm kiếm sẽ chia đôi kích thước của mảng.

```cpp
int binarySearch(int arr[], int left, int right, int x) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == x)
            return mid;
        if (arr[mid] < x)
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1;
}
```
**Đặc điểm**:
- Rất nhanh khi xử lý các bộ dữ liệu lớn.
- Thường gặp trong các thuật toán chia để trị (divide and conquer).

### 4.4. O(n²) - Thời gian tỷ lệ với bình phương kích thước đầu vào

- O(n²) có nghĩa là thời gian thực hiện của thuật toán tăng theo bình phương của kích thước đầu vào.
- Nếu đầu vào tăng gấp đôi, thời gian thực hiện sẽ tăng lên gấp bốn lần.

Ví dụ:Thuật toán sắp xếp chèn (insertion sort) hoặc sắp xếp chọn (selection sort) có độ phức tạp O(n²) vì chúng phải thực hiện nhiều vòng lặp lồng nhau.

```cpp
for (int i = 0; i < n; ++i) {
    for (int j = i + 1; j < n; ++j) {
        if (arr[i] > arr[j]) {
            std::swap(arr[i], arr[j]);
        }
    }
}
```
**Đặc điểm**:

- Chậm khi xử lý các bộ dữ liệu lớn.
- Thường gặp trong các thuật toán sắp xếp đơn giản hoặc các thuật toán có vòng lặp lồng nhau.

### 4.5. O(n log n) - Thời gian tỷ lệ với n log n

- O(n log n) là độ phức tạp của các thuật toán tốt hơn O(n²) nhưng chậm hơn O(n). Đây là sự kết hợp của việc duyệt qua tất cả các phần tử (O(n)) và xử lý logarit với từng phần tử (O(log n)).

Ví dụ:Thuật toán sắp xếp nhanh (quicksort) hoặc sắp xếp trộn (mergesort) có độ phức tạp O(n log n).

```cpp
void merge(int arr[], int l, int m, int r) {
    // Merges two subarrays of arr[]
    // Left subarray is arr[l..m]
    // Right subarray is arr[m+1..r]
}

void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}
```
**Đặc điểm**:
- Nhanh và thường được sử dụng trong các thuật toán sắp xếp hiệu quả.
- Thường gặp trong các thuật toán chia để trị với độ sâu đệ quy là logarit và xử lý mỗi bước là tuyến tính.

### 4.6. Tóm tắt các độ phức tạp phổ biến

| Độ phức tạp | Mô tả | Ví dụ |
|-------------|-------|-------|
| **O(1)**    | Thời gian hằng số, không phụ thuộc vào kích thước đầu vào. | Truy cập phần tử trong mảng. |
| **O(n)**    | Thời gian tỷ lệ với kích thước đầu vào. | Duyệt qua mảng. |
| **O(log n)**| Thời gian tỷ lệ với logarit của kích thước đầu vào. | Tìm kiếm nhị phân. |
| **O(n²)**   | Thời gian tỷ lệ với bình phương kích thước đầu vào. | Sắp xếp chọn, sắp xếp chèn. |
| **O(n log n)**| Thời gian tuyến tính nhân logarit. | Sắp xếp nhanh, sắp xếp trộn. |

</details>

</details>

# Bài 16 : OOP Assignment

<details>
<summary> Details </summary>


</details>

# Bài 17 : Template

<details>
<summary> Details </summary>

## 1. Khái niệm

<details>
<summary> Details </summary>

**Template** là một tính năng cho phép viết các hàm và lớp mà không cần xác định trước kiểu dữ liệu cụ thể. Khi sử dụng template, có thể viết một hàm hay lớp có khả năng làm việc với nhiều kiểu dữ liệu khác nhau mà không cần phải viết lại code cho từng kiểu.

Template giúp tăng tính linh hoạt và khả năng tái sử dụng của code, đồng thời giúp tránh việc trùng lặp code khi làm việc với các kiểu dữ liệu khác nhau.

</details>

## 2. Phân loại

<details>
<summary> Details </summary>

Template có 2 loại:
- Function template (Template hàm)
- Class template (Template lớp)

### 2.1. Function Template (Template hàm)

**Function Template** cho phép chúng ta định nghĩa một hàm có thể hoạt động với nhiều kiểu dữ liệu khác nhau mà không cần phải viết lại cho từng kiểu dữ liệu cụ thể.

Syntax:
```cpp
template <typename T>
T functionName(T param) {
    //Code
}
```
Trong đó:
- template <typename T> là cú pháp khai báo một template. T là một kiểu dữ liệu chưa biết (có thể là bất kỳ kiểu dữ liệu nào).
- T functionName(T param) là một hàm sử dụng kiểu T vừa được định nghĩa.

Example:
```cpp
#include <iostream>
using namespace std;

// Hàm template để tìm giá trị lớn hơn giữa hai số
template <typename T>
T findMax(T a, T b) {
    return (a > b) ? a : b;
}

int main() {
    int a = 10, b = 20;
    double x = 12.5, y = 9.3;

    // Sử dụng template với kiểu int
    cout << "Max giữa " << a << " và " << b << " là: " << findMax(a, b) << endl;

    // Sử dụng template với kiểu double
    cout << "Max giữa " << x << " và " << y << " là: " << findMax(x, y) << endl;

    return 0;
}
```
Output:
```cpp
Max giữa 10 và 20 là: 20
Max giữa 12.5 và 9.3 là: 12.5
```

### 2.2. Class Template (Template lớp)

Tương tự như function template, class template cho phép xây dựng các lớp có thể hoạt động với nhiều kiểu dữ liệu khác nhau mà không cần phải định nghĩa lại lớp cho từng kiểu dữ liệu.

Syntax:
```cpp
template <typename T>
class ClassName {
    T memberVariable;
public:
    ClassName(T param) : memberVariable(param) {}
    void display() {
        cout << "Member Variable: " << memberVariable << endl;
    }
};
```
Example:
```cpp
#include <iostream>
using namespace std;

// Class template
template <typename T>
class Box {
    T content;
public:
    Box(T c) : content(c) {}
    
    void showContent() {
        cout << "Nội dung trong hộp: " << content << endl;
    }
};

int main() {
    // Sử dụng class template với kiểu int
    Box<int> intBox(123);
    intBox.showContent();

    // Sử dụng class template với kiểu string
    Box<string> stringBox("Chào C++");
    stringBox.showContent();

    return 0;
}
```
Output
```cpp
Nội dung trong hộp: 123
Nội dung trong hộp: Chào C++
```

### 2.3. Template với nhiều tham số

Template cũng có thể chấp nhận nhiều tham số kiểu, điều này cho phép định nghĩa các hàm hoặc lớp có thể làm việc với nhiều kiểu dữ liệu khác nhau cùng một lúc.

Example:
```cpp
#include <iostream>
using namespace std;

// Hàm template với hai tham số kiểu
template <typename T1, typename T2>
void showPair(T1 a, T2 b) {
    cout << "First: " << a << ", Second: " << b << endl;
}

int main() {
    showPair(10, 20.5);           // int, double
    showPair("Hello", 100);        // const char*, int
    showPair(3.14, "World");       // double, const char*

    return 0;
}
```
Output:
```cpp
First: 10, Second: 20.5
First: Hello, Second: 100
First: 3.14, Second: World
```

</details>

## 3. Ứng dụng của Template trong thực tế

<details>
<summary> Details </summary>
    
**Tính tổng quát**:Template giúp giảm thiểu việc viết lại mã cho nhiều kiểu dữ liệu khác nhau. Ví dụ, thay vì viết nhiều hàm addInt, addFloat, addDouble, bạn chỉ cần một hàm template duy nhất.

**Tính an toàn kiểu**:Template giúp đảm bảo tính an toàn kiểu trong quá trình biên dịch, vì nếu kiểu dữ liệu không khớp, chương trình sẽ báo lỗi ngay khi biên dịch.

**Thư viện chuẩn C++ (STL)**: Các container như vector, list, map trong STL đều được xây dựng dựa trên template, nhờ đó chúng có thể chứa bất kỳ kiểu dữ liệu nào mà người dùng yêu cầu.

</details>

</details>

# Bài 18 : Lambda - Namespace - Pattern

<details>
<summary> Details </summary>


</details>

# Bài 19 : Makefile

<details>
<summary> Details </summary>


</details>

# Bài 20 : Smart pointer - Unique - Shared - Weak

<details>
<summary> Details </summary>

## 1. Smart pointer

<details>
<summary> Details </summary>

**Smart pointer** (con trỏ thông minh) là một cấu trúc dữ liệu được sử dụng để quản lý tài nguyên động, đặc biệt là bộ nhớ, một cách an toàn và tự động. Smart pointer không chỉ hoạt động như một con trỏ thông thường (để trỏ đến một vùng nhớ), mà còn có khả năng tự động giải phóng tài nguyên khi nó không còn được sử dụng, giúp tránh các vấn đề về quản lý bộ nhớ như rò rỉ bộ nhớ (memory leak).

Smart pointer là các lớp (classes) trong đó đối tượng quản lý một con trỏ thô (raw pointer). Một số đặc trưng chính của smart pointer bao gồm:
- Tự động quản lý vòng đời của đối tượng: Smart pointer sẽ tự động xóa đối tượng mà nó trỏ đến khi smart pointer không còn được sử dụng. Điều này giúp tránh việc quên giải phóng bộ nhớ, một lỗi phổ biến trong lập trình với con trỏ thô.
- Chia sẻ hoặc độc quyền sở hữu tài nguyên: Tùy thuộc vào loại smart pointer, nó có thể chia sẻ quyền sở hữu (shared ownership) hoặc độc quyền sở hữu (exclusive ownership) đối với đối tượng mà nó trỏ đến.
- Tích hợp với RAII (Resource Acquisition Is Initialization): Smart pointer tận dụng RAII, nghĩa là tài nguyên được cấp phát tại thời điểm khởi tạo và được giải phóng khi smart pointer bị hủy.

Có ba loại smart pointer chính được cung cấp bởi thư viện chuẩn (STL) từ C++11 trở đi. Chúng bao gồm:
- std::unique_ptr - Con trỏ thông minh sở hữu độc quyền đối tượng.
- std::shared_ptr - Con trỏ thông minh sở hữu chia sẻ đối tượng.
- std::weak_ptr - Con trỏ thông minh quan sát đối tượng mà không sở hữu nó.

</details>

## 2. Unique

<details>
<summary> Details </summary>

**Khái niệm:**

**std::unique_ptr** là một smart pointer sở hữu độc quyền đối tượng mà nó quản lý, có nghĩa là chỉ có một unique_ptr duy nhất có thể sở hữu một đối tượng nhất định tại một thời điểm. Khi unique_ptr bị hủy hoặc chuyển nhượng (move), đối tượng sẽ tự động bị giải phóng.

**Đặc điểm:**

- Sở hữu độc quyền: Tại một thời điểm, chỉ có một unique_ptr duy nhất sở hữu đối tượng. Không thể sao chép một unique_ptr (copy), nhưng có thể di chuyển (move) nó.
- Quản lý tài nguyên: Khi unique_ptr bị hủy (ra khỏi phạm vi hoặc gọi reset()), nó sẽ tự động giải phóng tài nguyên mà nó quản lý (gọi delete cho con trỏ thô).
- Hiệu suất tốt: Do không có bộ đếm tham chiếu, unique_ptr có chi phí thấp hơn so với shared_ptr.

Example:
```cpp
#include <iostream>
#include <memory>  // std::unique_ptr

class MyClass {
public:
    MyClass() { std::cout << "MyClass created\n"; }
    ~MyClass() { std::cout << "MyClass destroyed\n"; }
};

int main() {
    std::unique_ptr<MyClass> ptr1 = std::make_unique<MyClass>();  // Tạo unique_ptr

    // ptr1 sở hữu đối tượng, khi nó ra khỏi phạm vi, đối tượng sẽ bị hủy
    return 0;
}
```
Chuyển nhượng đối tượng giữa các unique_ptr
```cpp
std::unique_ptr<MyClass> ptr2 = std::move(ptr1);  // Chuyển quyền sở hữu từ ptr1 sang ptr2
```
Sau khi chuyển nhượng, ptr1 không còn sở hữu đối tượng nữa và trở thành nullptr.

</details>

## 3. Shared

<details>
<summary> Details </summary>

**Khái niệm:**

**std::shared_ptr** là một smart pointer cho phép chia sẻ quyền sở hữu một đối tượng giữa nhiều con trỏ. Nó sử dụng một bộ đếm tham chiếu (reference count) để theo dõi số lượng shared_ptr trỏ đến đối tượng. Khi bộ đếm tham chiếu giảm về 0 (tức là không còn shared_ptr nào trỏ đến), đối tượng sẽ tự động bị giải phóng.

**Đặc điểm:**
- Sở hữu chia sẻ: Nhiều shared_ptr có thể cùng sở hữu một đối tượng.
- Bộ đếm tham chiếu: Mỗi khi một shared_ptr mới sao chép từ shared_ptr hiện có, bộ đếm tham chiếu tăng lên. Khi một shared_ptr bị hủy, bộ đếm giảm đi. Khi bộ đếm về 0, đối tượng được giải phóng.
- Chi phí quản lý cao hơn: Do sử dụng bộ đếm tham chiếu, shared_ptr có chi phí quản lý cao hơn so với unique_ptr.

Example:
```cpp
#include <iostream>
#include <memory>  // std::shared_ptr

class MyClass {
public:
    MyClass() { std::cout << "MyClass created\n"; }
    ~MyClass() { std::cout << "MyClass destroyed\n"; }
};

int main() {
    std::shared_ptr<MyClass> ptr1 = std::make_shared<MyClass>();  // Tạo shared_ptr
    {
        std::shared_ptr<MyClass> ptr2 = ptr1;  // ptr2 chia sẻ quyền sở hữu với ptr1
        std::cout << "Use count: " << ptr1.use_count() << std::endl;  // Đếm số lượng shared_ptr
    }
    // Khi ptr2 ra khỏi phạm vi, bộ đếm tham chiếu giảm xuống

    std::cout << "Use count: " << ptr1.use_count() << std::endl;  // ptr1 vẫn còn sở hữu
    return 0;  // Khi ptr1 ra khỏi phạm vi, đối tượng bị hủy
}
```
Output:
```cpp
MyClass created
Use count: 2
Use count: 1
MyClass destroyed
```

</details>

## 4. Weak

<details>
<summary> Details </summary>

**Khái niệm:**

**std::weak_ptr** là một smart pointer đặc biệt được sử dụng để quan sát đối tượng được quản lý bởi shared_ptr mà không sở hữu nó. Nó không tăng bộ đếm tham chiếu của đối tượng được quản lý. weak_ptr thường được sử dụng để tránh vòng tham chiếu (cyclic reference), nơi hai shared_ptr có thể trỏ lẫn nhau, gây ra rò rỉ bộ nhớ.

**Đặc điểm:**
- Quan sát không sở hữu: weak_ptr không sở hữu đối tượng mà nó trỏ đến và không làm tăng hoặc giảm bộ đếm tham chiếu.
- Tránh vòng tham chiếu: Khi có hai hoặc nhiều shared_ptr trỏ lẫn nhau, weak_ptr có thể được sử dụng để phá vỡ vòng tham chiếu.
- Kiểm tra hợp lệ: Để truy cập đối tượng mà weak_ptr trỏ đến, bạn cần sử dụng phương thức lock(), phương thức này trả về một shared_ptr. Nếu đối tượng đã bị hủy, lock() trả về một shared_ptr null.

```cpp
#include <iostream>
#include <memory>  // std::shared_ptr, std::weak_ptr

class MyClass {
public:
    MyClass() { std::cout << "MyClass created\n"; }
    ~MyClass() { std::cout << "MyClass destroyed\n"; }
};

int main() {
    std::shared_ptr<MyClass> sharedPtr = std::make_shared<MyClass>();
    std::weak_ptr<MyClass> weakPtr = sharedPtr;  // weak_ptr quan sát sharedPtr

    // Kiểm tra xem đối tượng còn tồn tại không
    if (auto tempPtr = weakPtr.lock()) {  // Tạo shared_ptr tạm thời từ weak_ptr
        std::cout << "Object is still alive\n";
    } else {
        std::cout << "Object has been destroyed\n";
    }

    return 0;
}
```
Output:
```
MyClass created
Object is still alive
MyClass destroyed
```

</details>

## 5. Conclude

<details>
<summary> Details </summary>

So sánh các loại Smart Pointer:

| Loại Smart Pointer | Sở hữu | Bộ đếm tham chiếu | Chuyển nhượng (Move) | Sao chép (Copy) | Ứng dụng chính |
|--------------------|--------|-------------------|----------------------|-----------------|----------------|
|std::unique_ptr|Độc quyền|Không|Có|Không|Quản lý tài nguyên với quyền sở hữu duy nhất|
|std::shared_ptr|Chia sẻ|Có|Có|Có|Quản lý tài nguyên với quyền sở hữu chia sẻ|
|std::weak_ptr|Quan sát|Không|Không|Không|Quan sát đối tượng, tránh vòng tham chiếu|

**Kết luận:**
- std::unique_ptr: Được sử dụng khi bạn muốn đảm bảo rằng chỉ có một con trỏ duy nhất sở hữu một đối tượng, và đối tượng đó sẽ được tự động giải phóng khi không cần thiết nữa.
- std::shared_ptr: Được sử dụng khi bạn cần chia sẻ quyền sở hữu đối tượng giữa nhiều con trỏ và đối tượng sẽ chỉ bị giải phóng khi không còn con trỏ nào trỏ đến nó.
- std::weak_ptr: Được sử dụng khi bạn muốn giữ một tham chiếu đến đối tượng được quản lý bởi shared_ptr mà không sở hữu nó, thường để tránh vòng tham chiếu.


</details>

</details>

# Bài 21 : Design Pattern

<details>
<summary> Details </summary>

# Là những khuôn mẫu
# Các tính chất
# - Tái sử dụng
# - Linh hoạt
# - Đã được kiểm chứng
# - Dễ bảo trì
# - Tăng khả năng mở rộng

Ở Việt Nam có các nhóm chính như sau:
- creational patterns:(Khởi tạo 1 cách linh hoạt)
+ Singeleton , Factory Method, ...
- structural pattern:
+ Adapter, Composite
- Behavioral pattern:
+ Observer, Stategy, Command, iterator

Observer:
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

// Interface to observers (display, logger, etc.)
class Observer {
public:
    virtual void update(float temerature, float humidity, float light) = 0;
};

//Subject (SensorManager) holds the state and notifies observers
class SensorManager {
    float temperature;
    float humidity;
    float light;
    std::vector<Observer*> observers;
public:
    void registerObserver(Observer* observer) {
        observers.push_back(observer);
    }

    void removeObserver(Observer* observer) {
        observers.erase(std::remove(observers.begin(), observers.end(), observer), observers.end());
    }

    void notifyObservers() {
        for (auto observer : observers) {
            observer->update(temperature, humidity, light);
        }
    }

    void setMeasurements(float temp, float hum, float lightLvl) {
        temperature = temp;
        humidity = hum;
        light = lightLvl;
        notifyObservers();
    }
};

//Display component (an observer)
class Display : public Observer {
public:
    void update(float temperature, float humidity, float light) override {
        std::cout << "Display: Temperature: " << temperature
                  << ", Humidity: " << humidity
                  << ", Light: " << light << std::endl;
    }
};

//Logger component (an observer)
class Logger : public Observer {
public:
    void update(float temperature, float humidity, float light) override {
        std::cout << "Logging data... Temp: " << temperature
                  << ", Humidity: " << humidity
                  << ", Light: " << light << std::endl;
    }
};

int main(){
    SensorManager observerTemp;

    Display display;
    Logger logger;

    observerTemp.registerObserver(&display);
    observerTemp.registerObserver(&logger);

    observerTemp.setMeasurements(25.0, 60.0, 700.0);
    observerTemp.setMeasurements(26.0, 65.0, 600.0);
    return 0;
}
```


</details>

















