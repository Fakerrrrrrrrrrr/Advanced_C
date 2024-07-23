# **Bài 1: Compiler - Marco**
## 1. Compiler
Compiler (trình biên dịch) là một phần mềm được sử dụng để chuyển đổi một ngôn ngữ lập trình cấp cao (high-level programming language) như C, C++, Java, Python, v.v. thành mã máy (machine code) có thể được thực thi trực tiếp bởi bộ xử lý (CPU) của máy tính.

![Compiler Image](https://github.com/Fakerrrrrrrrrrr/Advanced_C/blob/main/Images/Complier.png)

Chuyển đổi file main.c sang file main.i (Bước tiền xử lý) (Bước Preprocessor)
> gcc -E main.c -o main.i

Chuyển đổi file main.i sang file main.s (Chương trình mã nguồn của assembly) (Bước Compiler) (Chủ yếu lập trình liên quan đến hệ điều hành) (RTOS)
> gcc main.i -S -o main.s

Chuyển đổi file main.s sang main.o (File Hex) (Bước Assembler (Phần bên phải là địa chỉ, phần bên trái là giá trị của địa chỉ đó) (Nó sẽ ghi vào bộ nhớ Flash) khi tắt nguồn đi nó vẫn không bị mất dữ liệu, copy tất cả chương trình ở bộ nhớ Flash vào RAM rồi mới thực thi chương trình trên RAM. (Học RTOS tìm hiểu sau tra keyword booting process tìm hiểu, câu hỏi phỏng vấn)
> gcc - c main.s -o main.o

File main.o đã là chương trình của mình nhưng vẫn chưa chạy được trên hệ điều hành của mình vì nó chỉ chạy trên con vi điều khiển (Bước linker) (Khi viết chương trình sẽ có nhiều file như file Head, file main, file library,...)
> gcc test1.o test2.o main.o -o main
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
## 2. Macro
Macro là một tính năng trong lập trình cho phép người lập trình định nghĩa một tập hợp các lệnh hoặc quy tắc được đặt tên và có thể được sử dụng nhiều lần trong chương trình.

Khi gặp một macro trong chương trình, trình biên dịch sẽ thay thế macro bằng đoạn mã tương ứng trước khi chuyển đổi chương trình sang mã máy. Macro xảy ra ở giai đoạn tiền xử lý.

Macro có thể được sử dụng để:

1. Thay thế văn bản: Thay thế một đoạn văn bản bằng một đoạn khác.
2. Thực hiện tính toán: Thực hiện các phép tính toán tại thời điểm biên dịch.
3. Tạo mã lập trình động: Tạo ra các đoạn mã mới dựa trên các tham số đầu vào.
4. Tái sử dụng mã: Tái sử dụng các đoạn mã thường xuyên được sử dụng.

Định nghĩa Macro function nối bằng dấu \.
```
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
```
#define CREATE(name)    \
int int_##name;         \
double double_##name;   \
char char_##char;

CREATE(test) //int int_test; double double_test; char char_test;
```
Chuyển hóa đoạn văn bản thành chuỗi
```
#include <stdio.h>
#define CREATE(cmd) printf(#cmd);
int main(){
    CREATE(okdesu) //okdesu
    return 0;
}
```
Trong trường hợp không biết số lượng tham số không xác định trước ở Macro thì chúng ta dùng Variadic Macro.
```
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
```
#ifndef _LIB_MACO_
#define _LIB_MACO_
    {code}
#endif
```
Ví dụ về ứng dụng ifndef:
```
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

# Bài 2: STDARG - ASSERT
## 1. STDARG
Thư viện stdarg trong C là một thư viện tiêu chuẩn được sử dụng để xử lý các tham số không xác định số lượng (tham số đầu vào) của các hàm. Thư viện này cung cấp một số macro và hàm cho phép xử lý các tham số không xác định số lượng.

Các macro và hàm chính trong thư viện stdarg bao gồm:

1. va_list: Đây là một kiểu dữ liệu được sử dụng để lưu trữ thông tin về các tham số không xác định số lượng. (Sử dụng typedef để định nghĩa lại, bản chất nó giống như 1 con trỏ lưu kiểu dữ liệu: typedef char* va_list;)
2. va_start(va_list ap, last_named_arg): Macro này khởi tạo va_list bằng cách lấy địa chỉ của tham số cuối cùng được đặt tên. (Bản chất của nó là 1 cái Macro: #define va_start(args, temp)) Ví dụ: ta có 1 chuỗi char* = {5,1,3,4,5,6} thì last_named_arg được nhập bằng 5, nó sẽ tạo ra con trỏ so sánh vị trí đầu tiên xem giá trị có bằng với chuỗi last_named_arg này không nếu không thì bỏ qua so sánh tiếp vị trí tiếp theo, nếu bằng thì nó xác định cái dấu ... thì sẽ bắt đầu từ đoạn vị trí tiếp theo trở đi. Tạo ra mã ký tự khác để lưu mảng còn lại. Khi 
3. va_arg(va_list ap, type): Macro này lấy giá trị của tham số tiếp theo từ va_list và chuyển đổi nó sang kiểu dữ liệu được chỉ định. type được hiểu là ép kiểu dữ liệu. Khi gọi hàm va_arg(va_list ap, type) thì nó đọc giá trị tại ô phía sau va_start và trỏ tới ô tiếp theo.
4. va_end(va_list ap): Macro này thực hiện dọn dẹp và hoàn thành việc sử dụng va_list.

Code:
```
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
```
Value at 0: 5
Value at 1: 8
Value at 2: 15
Value at 3: 10
Value at 4: 13
```
Thư viện stdarg là một công cụ hữu ích khi cần xử lý các tham số không xác định số lượng, chẳng hạn như trong các hàm printf(), scanf(), hoặc các hàm tự định nghĩa khác.<br>
Có thể ép kiểu của struct:
```
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
## 2. Assert
Khi phát triển project lớn có rất nhiều file, bình thường Debug bằng If Else do chương trình nhỏ nên khó phân biệt ở đoạn nào, khi đó ta dùng Assert Lib để Debug.
> Syntax: assert(condition && #cmd);

Nếu điều kiện đúng (true), không có gì xảy ra và chương trình tiếp tục thực thi. Nếu điều kiện sai (false), chương trình dừng lại và thông báo một thông điệp lỗi. Dùng trong debug, dùng #define NDEBUG để tắt debug

```
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

# **Bài 3: Pointer**
## Bài tập về nhà
> Kích thước của con trỏ

Kích thước của con trỏ phụ thuộc vào kiến trúc máy tính (32-bit hay 64-bit) và kiểu dữ liệu mà con trỏ đang trỏ đến.

Hầu hết ở các kiểu dữ liệu thì kích cỡ của con trỏ thường là **4** bytes (32 bit) trên hệ thống 32-bit và **8** bytes (64 bit) trên hệ thống 64-bit.

Tuy nhiên ở kiểu dữ liệu **double** là **8** bytes (64 bit), kiểu dữ liệu **long long** là **8** bytes (64 bit), kiểu dữ liệu **bool** là **1** bytes (8 bit) trên cả hệ thống 32-bit và 64-bit.

**Input**
```
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
```
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

## 1. Pointer
- Con trỏ (Pointer) là một biến lưu lại địa chỉ ô nhớ trong bộ nhớ máy tính.
> Cú pháp: <kiểu_dữ_liệu> *<tên_biến>;

Ví dụ: 
```
int *ptr = 0x01;
```
- Để lấy được địa chỉ của biến được khai báo bình thường thì dùng toán tử &

Ví dụ: 
```
int a = 10;
int *ptr = &a;
```
- Đối với 1 mảng để lấy địa chỉ của 1 mảng thì ta chỉ cần nhập tên của mảng đó

Ví dụ: 
```
int array[] = {1,2,3,4,5};
int *ptr = array;
```
- Để lấy giá trị của con trỏ đó dùng toán tử *

Ví dụ 3 kết quả dưới đều tương đương nhau: 
```
*0x01 = 10
*ptr = 10
*(&a) = 10
```
## 2. Function Pointer
- Con trỏ hàm (Function Pointer) là một biến có thể lưu địa chỉ của hàm.
> Cú pháp: <kiểu_dữ_liệu_trả_về> (*<tên_con_trỏ>)(<các_tham_số>) = <tên_hàm>;

Ngoài ra con trỏ hàm cũng có thể làm tham số của 1.

Ví dụ: 
```
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
## 3. Void Pointer
Con trỏ void (void pointer) là một loại con trỏ đặc biệt, có thể trỏ đến bất kỳ kiểu dữ liệu nào, bao gồm cả các kiểu do người dùng định nghĩa.
> Cú pháp: void *<tên_con_trỏ>;

Có 3 đặc điểm của con trỏ void:
1. Có thể trỏ đến bất kỳ kiểu dữ liệu nào, không phụ thuộc vào kiểu.
2. Không thể trực tiếp truy cập/thay đổi giá trị mà nó trỏ đến, vì không xác định được kiểu dữ liệu.
3. Trước khi sử dụng, cần phải ép kiểu con trỏ void về kiểu dữ liệu đúng.

Ví dụ:
```
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
## 4. Pointer to Constant

Con trỏ hằng (Pointer to Constant) là không thể sử dụng con trỏ để thay đổi giá trị mà nó trỏ .
> Cú pháp: <kiểu_dữ_liệu> const *<tên_con_trỏ>;<br>           const <kiểu_dữ_liệu> *<tên_con_trỏ>;

Có 3 đặc điểm của con trỏ hằng:
1. Không thể sử dụng con trỏ để thay đổi giá trị mà nó trỏ đến (vì đó là hằng số).
2. Có thể gán địa chỉ của một biến có thể thay đổi cho con trỏ, nhưng không thể thay đổi giá trị của biến đó thông qua con trỏ.
3. Có thể gán địa chỉ của một hằng số cho con trỏ.

```
int x = 10;
const int *p = &x; // Gán địa chỉ của x cho con trỏ p, nhưng không thể thay đổi giá trị của x thông qua p

*p = 20; // Lỗi, không thể thay đổi giá trị mà p trỏ đến vì đó là hằng số

const int y = 30;
p = &y; // Hợp lệ, có thể thay đổi địa chỉ mà p trỏ đến
```

## 5. Null Pointer
Con trỏ Null (Null Pointer) là một con trỏ không trỏ đến bất kỳ vùng nhớ nào và có giá trị bằng 0.
Khi chúng ta khai báo bất kỳ
> Cú pháp: <kiểu_dữ_liệu> *<tên_con_trỏ> = NULL;

Khi khai báo 1 con trỏ bất kỳ nhưng không gán giá trị cho nó thì sẽ xảy ra trường hợp con trỏ được cấp phát vùng nhớ ngẫu nhiên. <br> Vì vậy nên gán biến con trỏ đó thành con trỏ NULL nếu chưa sử dụng ngay biến con trỏ đó.

Ví dụ:
```
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

# Bài 4: Goto - setjmp.h
## 1. Goto
Goto có khá nhiều ứng dụng, khi code không khuyến khích sử dụng Goto vì nó thay đổi luồng chạy của chương trình chỉ dành cho những người Master ngôn ngữ lập trình mới kiểm soát luồng chạy của nó.
Khi viết chương trình có rất rất nhiều phức tạp thì chúng ta không thể Break hết tất cả hoặc chương trình bị lỗi ta không thể thoát ngay lập tức thì chúng ta sử dụng Goto.
> Syntax: goto label; //label là tên được sử dụng để xác định vị trí mà goto sẽ nhảy đến -> label:

Cách hoạt động: 
- Khi gặp lệnh goto label; chương trình sẽ ngay lập tức chuyển đến vị trí (trỏ tới vị trí) được xác định bởi label và tiếp tục thực hiện các câu lệnh tại đó. 
- Label cso thể được định nghĩa bất kỳ đâu trong chương trình, miễn là nó nằm trong cùng phạm vị với lệnh goto sử dụng nó.

```
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
```
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
## 2. Setjmp
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
```
#define TRY if ((exception_code = setjmp(buf)) == 0) 
#define CATCH(x) else if (exception_code == (x)) 
#define THROW(x) longjmp(buf, (x))
```
Code ví dụ:
```
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













