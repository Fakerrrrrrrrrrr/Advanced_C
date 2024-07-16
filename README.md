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

Có 3 đặc điểm c ủa con trỏ hằng:
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
2. Kiểm tra: Trước khi sử dụng một con trỏ, nên kiểm tra xem nó có phải là null pointer không bằng cách so sánh nó với NULL
if (ptr != NULL) {
    // Sử dụng con trỏ
} else {
    // Xử lý trường hợp con trỏ null
}
```
