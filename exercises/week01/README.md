# Tuần 1: Tổng Quan C++ & Big-O — Bài tập

## 🎯 Mục tiêu tuần này
Hiểu Big-O, phân tích độ phức tạp, ôn tập C++ cơ bản.

---

### Bài 1: Phân tích Big-O ⭐
Xác định Big-O của 10 đoạn code C++ cho trước. Giải thích tại sao.

O(1):
int getFirst(int arr[], int n) {
    return arr[0];
}
# Truy cập trực tiếp vào 1 phần tử mà không phụ thuộc n

O(n):
int tinhTong(int arr[], int n) {
    int sum = 0;
    for (int i = 0; i < n; i++)
        sum += arr[i];
    return sum;
}
# Chạy vòng lặp n lần, tỉ lệ thuận với n

O(n^2):
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n - i - 1; j++)
            if (arr[j] > arr[j+1])
                swap(arr[j], arr[j+1]);
}
# 2 vòng lặp lồng nhau: 1 vòng = nxn

O(logn):
int binarySearch(int arr[], int n, int target) {
    int lo = 0, hi = n - 1;
    while (lo <= hi) {
        int mid = (lo + hi) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
# Mỗi bước chia đôi mảng, sau log2(n) bước thì tìm xong

O(nlogn):
void mergeSort(int arr[], int l, int r) {
    if (l >= r) return;
    int mid = (l + r) / 2;
    mergeSort(arr, l, mid);
    mergeSort(arr, mid + 1, r);
    merge(arr, l, mid, r);
}
# Chia 1/2 logn lần, mỗi lần merge cần O(n)

O(n^3):
void nhanMaTran(int A[][N], int B[][N], int C[][N], int n) {
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            for (int k = 0; k < n; k++)
                C[i][j] += A[i][k] * B[k][j];
}
# 3 vòng lặp lồng nhau: 1 vòng = nxnxn

O(2^n):
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
# Số lần gọi tăng gần gấp đôi mỗi khi n tăng 1

O(sqrt(n)):
bool laSoNguyenTo(int n) {
    if (n < 2) return false;
    for (int i = 2; i * i <= n; i++)
        if (n % i == 0) return false;
    return true;
}
# Kiểm tra kết quả đến sqrt(n) thay vì đến n

O(n+m):
void inHai(int a[], int n, int b[], int m) {
    for (int i = 0; i < n; i++) cout << a[i];
    for (int i = 0; i < m; i++) cout << b[i];
}
# 2 vòng độc lập và cộng lại với nhau

O(n!):
void inHai(int a[], int n, int b[], int m) {
    for (int i = 0; i < n; i++) cout << a[i];
    for (int i = 0; i < m; i++) cout << b[i];
}
# Cho hoán vị chuỗi n ký tự

### Bài 2: Đo thời gian thực tế ⭐⭐
Dùng `chrono` đo thời gian chạy của O(n), O(n²), O(log n) với n = 1.000 → 100.000. In bảng kết quả.

#include <iostream>
#include <chrono>
#include <iomanip>
#include <functional>
#include <vector>
#include <algorithm>
using namespace std;
using namespace chrono;

double measure(function<void()> fn) {
    auto start = high_resolution_clock::now();
    fn();
    auto end = high_resolution_clock::now();
    return duration<double, milli>(end - start).count();
}
# O(logn):
int ologn(vector<int>& arr, int target) {
    int lo = 0, hi = arr.size() - 1;
    while (lo <= hi) {
        int mid = (lo + hi) / 2;
        if      (arr[mid] == target) return mid;
        else if (arr[mid] <  target) lo = mid + 1;
        else                         hi = mid - 1;
    }
    return -1;
}
# O(n):
long long on(vector<int>& arr) {
    long long sum = 0;
    for (int x : arr) sum += x;
    return sum;
}
# O(n^2):
void on2(vector<int> arr) {
    int n = arr.size();
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n - i - 1; j++)
            if (arr[j] > arr[j+1])
                swap(arr[j], arr[j+1]);
}
# thiếu bảng
### Bài 3: Tối ưu hàm ⭐⭐
Cho 3 hàm O(n²) — tối ưu xuống O(n) hoặc O(n log n). Chứng minh bằng cách đo thời gian.

### Bài 4: 🔥 Dự Án Mini — Big-O Benchmark Tool ⭐⭐⭐
> **Cảm hứng:** [algorithm-visualizer.org](https://algorithm-visualizer.org)

Viết chương trình **BenchmarkTool** hiển thị bảng so sánh tốc độ các thuật toán:
```
╔══════════════╦══════════╦══════════╦══════════╗
║   Thuật toán ║  n=1000  ║  n=10000 ║ n=100000 ║
╠══════════════╬══════════╬══════════╬══════════╣
║    O(1)      ║  0.001ms ║  0.001ms ║  0.001ms ║
║    O(log n)  ║  0.003ms ║  0.004ms ║  0.005ms ║
║    O(n)      ║  0.12ms  ║  1.2ms   ║  12ms    ║
║    O(n²)     ║  8ms     ║  800ms   ║  80000ms ║
╚══════════════╩══════════╩══════════╩══════════╝
```

**Yêu cầu:** dùng `std::chrono`, hiển thị bảng căn chỉnh đẹp, xuất ra file `benchmark.txt`.

---
📁 Tham khảo: `Chuong1_TongQuan/Chuong1_TongQuan.cpp`
