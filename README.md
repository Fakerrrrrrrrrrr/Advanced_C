# **Bài 3: Pointer**
## 1. Pointer
Con trỏ (Pointer) là một biến lưu lại địa chỉ ô nhớ trong bộ nhớ máy tính.
> Cú pháp: <kiểu_dữ_liệu> *<tên_biến>;

Ví dụ: 
```
int *ptr = 0x01;
```
Để lấy được địa chỉ của biến được khai báo bình thường thì dùng toán tử &
Ví dụ: 
```
int a = 10;
int *ptr = &a;
```
