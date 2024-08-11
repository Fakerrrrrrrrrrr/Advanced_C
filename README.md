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

# Bài 5: Extern - Static - Volatile - Register
## 1. Extern
Khi sử dụng keyword extern thì có nghĩa là chúng ta muốn lấy một biến hoặc một hàm ở một file khác trong chương trình.<br>
Ví dụ: Ta có 2 file main.c và file test.c. Biến ở file test.c được khai báo toàn cục 'int count = 20;' thì khi chúng ta dùng 'extern int count;' ở file main.c thì chúng ta hoàn toàn có thể sử dụng biến count và có giá trị là 20, có nghĩa là nếu count có địa chỉ 0x01 ở file test.c thì ở file main.c nó cũng có địa chỉ là 0x01.<br>
Extern cũng có thể sử dụng để khai báo các hàm ở file khác.<br>
Ví dụ: extern int calculatorDivide(int a, int b); ta lấy hàm calculatorDivide sử dụng mà không cần phải viết lại hàm đó.
> Syntax: extern datatype variable_name;<br>
> extern return_type function_name(parameter_list);

## 2. Static variable
Khi khai báo biến static chương trình sẽ cấp phát cho nó 1 địa chỉ tồn tại hết vòng đời của chương trình (không bị thu hồi), biến static chỉ khởi tạo 1 lần và không khởi tạo lại lần nữa, nếu gặp biến static ở function được gọi nó sẽ bỏ qua và chạy dòng code tiếp theo.</br>
Giá trị của biến static chỉ có phạm vi cục bộ với file hoặc hàm chứa nó. Biến static sẽ được lưu trữ trong vùng nhớ static thay vì vùng nhớ stack như biến cục bộ thông thường.<br>
```
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
```
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
1.Stack: là một cấu trúc dữ liệu có tính chất "Last-In-First-Out" (LIFO), được sử dụng để lưu trữ các biến cục bộ, tham số của hàm, và địa chỉ trở về (return address) khi gọi hàm. Phân vùng stack là vùng nhớ được cấp phát và quản lý theo cấu trúc stack.
2.Cách hoạt động của stack:
- Khi một hàm được gọi, một khung stack (stack frame) mới sẽ được tạo ra và đẩy lên trên cùng của stack.
- Trong khung stack này, các biến cục bộ của hàm, tham số truyền vào, và địa chỉ trở về sẽ được lưu trữ.
- Khi hàm kết thúc, khung stack của hàm đó sẽ được xóa khỏi stack, và giá trị các biến cục bộ sẽ mất đi.
3. Ưu điểm:
- Quản lý bộ nhớ đơn giản và hiệu quả.
- Hỗ trợ đệ quy (recursion) tốt.
- Truy xuất dữ liệu nhanh.
4. Nhược điểm:
- Dung lượng bộ nhớ có giới hạn, không thể mở rộng kích thước stack tùy ý.
- Nếu sử dụng đệ quy quá sâu có thể dẫn đến tràn stack (stack overflow).

## 3. Register
Test:
```
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
```
Thoi gian chay cua chuong trinh: 0.002000 giay
```
Test lại đoạn code trên sửa lại đoạn code 'int i;' thành 'register int i;'
```
Thoi gian chay cua chuong trinh: 0.000000 giay
```
Register là một từ khóa để khai báo biến nên được lưu trữ trong thanh ghi của bộ xử lý thay vì trong bộ nhớ chính. Việc lưu trữ biến trong thanh ghi thay vì bộ nhớ chính có thể giúp tăng tốc độ truy cập và xử lý dữ liệu. Tuy nhiên, số lượng thanh ghi thường bị giới hạn, nên không phải tất cả biến đều có thể được lưu trữ trong thanh ghi.

![Compiler Image](https://github.com/Fakerrrrrrrrrrr/Advanced_C/blob/main/Images/Register.png)

Khi khai báo 'int i = 5;' nó sẽ lưu 1 cái địa chỉ ở RAM và lưu giá trị bằng 5, lưu thông tin phép toán ở RAM, muốn tính toán sẽ đẩy thông tin tính toán qua register sau đó register sẽ đẩy thông tin tính toán qua ALU (Bộ xử lý trung tâm) ở ALU thực hiện tính toán sau đó đẩy kết quả về cho register sau đó register trả về kết quả lại cho RAM.<br>
Vậy nên khi sử dụng biến Register nó được lưu thông tin ở Register thay vì RAM vậy nên tính toán sẽ nhanh vì chỉ có bước đẩy thông tin qua ALU và ALU trả giá trị kết quả về lại Register mà không cần phải thông qua RAM.

## 4. Volatile
Khi sử dụng biến volatile thì nó sẽ thông báo cho chương trình biên dịch không được phép tối ưu code của biến đó. Biến volatile thường được sử dụng để truy cập vào các vùng nhớ liên quan đến phần cứng, như các thanh ghi hoặc bộ nhớ được chia sẻ. Khi sử dụng volatile, trình biên dịch sẽ không tối ưu hóa code liên quan đến biến này, vì giá trị của biến có thể thay đổi bất kỳ lúc nào.<br>
Sau này khi học RTOS thì sử dụng nhiều luồng với nhau ta sẽ luôn luôn sử dụng Volatile bởi vì khi sử dụng ở luồng khác thì ta phải luôn luôn load lại biến đó.
```
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

# Bài 6: Bitmask
## Ứng dụng:
Trong đời sống có rất nhiều trường hợp sẽ rơi vào tình huống phải lựa chọn những trường hợp khác nhau, ví dụ như muốn đi từ Hà Nội tới Đà Nẵng thì phải có thể chọn phương tiện khác nhau để tới được Đà Nẵng như đi máy bay, xe bus, taxi, tàu lửa,... 
Nếu có nhiều sự lựa chọn như vậy thì ta lại phải khai báo từng biến khác nhau rất mất thời gian và tốn bộ nhớ vì mỗi biến sẽ có ít nhất 1byte. Vậy nên chúng ta dùng bitmask để lưu trữ và làm việc với các trạng thái hoặc cấu hình dưới dạng các bit trong một số nguyên.
Bitmask thường dùng để biểu diễn một tập hợp các lựa chọn hoặc thiết lập, trong đó mỗi bit tương ứng với một lựa chọn hoặc cấu hình.<br>
Ví dụ, nếu bạn muốn lưu trữ trạng thái của 8 tính năng khác nhau, sử dụng bitmask để lưu trữ với một số nguyên 8-bit(1 byte) cho mỗi tính năng:<br>
- Bit 0 (giá trị 1): Tính năng 1 được bật //00000001
- Bit 1 (giá trị 2): Tính năng 2 được bật //00000010
- Bit 3 (giá trị 3): Tính năng 3 được bật //00000100
- Và cứ thế<br>

## Ứng dụng các thao tác phổ biến với bitmask bao gồm:
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
```
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

# Bài 8: Struct - Union
## 1. Struct
- Khái niệm:<br>
Struct là một kiểu dữ liệu được định nghĩa bởi người viết code, cho phép nhóm các kiểu dữ liệu khác nhau lại thành một kiểu dữ liệu mới. Một struct thường được sử dụng để lưu trữ các thông tin liên quan với nhau, như một người hoặc thông tin về một sản phẩm,...


# Bài 9: JSON
## 1. Khái niệm và ứng dụng
JSON (Javascript Object Notation) là một dạng database dựa trên văn bản, nó nhẹ, dễ đọc và dễ viết, cú pháp đơn giản và gọn gàng, được sử dụng để trao đổi dữ liệu giữa máy chủ và web app, nó hiệu quả khi truyền dữ liệu qua mạng đặc biệt là trên các thiết bị có băng thông hạn chế và trên ô tô.<br>
JSON được sử dụng để lưu database, lưu các dữ liệu mà có kiểu dữ liệu không xác định được. Mỗi đối tượng JSON bao gồm một tập hợp các cặp key-value, mảng JSON tập hợp các giá trị của mỗi đối tượng hoặc đối tượng.<br>

Cú pháp:
- JSON sử dụng cú pháp gồm các cặp key-value, được bao bởi dấu ngoặc nhọn {}.
- Các giá trị có thể là chuỗi, số, boolean, null, mảng hoặc đối tượng JSON khác.

Ví dụ:
```
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
## 2. Cấu trúc và định dạng của JSON
Cấu trúc của JSON:
```
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
```
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

# Bài 10: Linked List
## Khái niệm
Linked List là cấu trúc dữ liệu mà trong đó các phần tử node được liên kết với nhau thông qua các con trỏ.<br>
Mỗi node trong linked list sẽ có 2 thành phần là:<br>
- Data: Chứa dữ liệu của node
- Next: Là một con trỏ chứa địa chỉ tiếp theo của node tiếp theo

Cấu trúc của một node trong Linked List được định nghĩa bằng một struct như sau:
```
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
## Ứng dụng
Khi chúng ta có 1 mảng 10000 phần tử, để chèn vào phần tử hoặc xóa 1 phần tử ở 1 vị trí chỉ định thì chúng ta phải tốn nhiều tài nguyên của hệ thống để dịch chuyển sang hoặc lùi 1 ô nhớ, vì vậy chúng ta dùng Linked List.<br>
Linked List có thể ứng dụng để tạo một List có kích thước động vì nó cấp phát động cho mỗi khi tạo ra Node mới và các Node trong đó không cần phải có địa chỉ liên tiếp nhau nhưng có thể truy cập tuần tự theo danh sách.<br>
Các thao tác cơ bản của Linked List là:
- push_back: Thêm phần tử Node vào cuối danh sách.
```
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
```
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
```
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
```
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
```
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

# Bài 11: Stack - Queue
## 1. Stack
Stack là một cấu trúc dữ liệu tuân theo nguyên tắc "Last-In-First-Out" (LIFO) - phần tử được thêm vào cuối cùng sẽ được lấy ra đầu tiên.<br>
Stack có thể được triển khai bằng mảng hoặc danh sách liên kết dưới dạng struct,...<br>
![Stack Image](https://github.com/Fakerrrrrrrrrrr/Advanced_C/blob/main/Images/Stack.png)
Ở ví dụ trên ta có thể thấy thì kích cỡ của mảng hoặc danh sách là 7 và 5, bắt đầu với index hoặc top = -1, những phần tử sẽ được thêm vào theo trình tự 1->2->3->4->5->6->7 index hoặc top sẽ tăng dần thêm 1 theo trình tự và chúng sẽ được lấy ra theo trình tự ngược lại 7->6->5->4->3->2->1 và index hoặc top cũng giảm dần đi 1 theo trình tự.<br>
Stack có 3 thao tác chính cơ bản như:
- push: thêm phần tử ở cuối vào mảng stack
- pop: xóa phần tử ở cuối cùng ra khỏi mảng stack
- top: lấy giá trị ở cuối cùng ở mảng stack

Code triển khai Stack dưới dạng mảng:
```
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
```
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

## 2. Queue
Queue là một cấu trúc dữ liệu tuân theo nguyên tắt "Fisrt In, First Out", phần tử đầu tiên được thêm vào mảng Queue sẽ là phần tử được lấy ra đầu tiên từ mảng Queue.<br>
Queue có 3 thao tác chính:
- enqueue: Thêm vào phần tử cuối cùng của mảng Queue.
- dequeue: Lấy ra phần tử đầu tiên của mảng Queue.
- front: lấy ra giá trị đầu tiên của mảng Queue

Code Queue với mảng:
```
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
```
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








