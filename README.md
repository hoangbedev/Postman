# Thực hành kiểm thử API với Postman

## 1. Mục tiêu

- Hiểu Postman là gì và vai trò của nó trong kiểm thử API.
- Thực hiện được các request GET, POST, PUT, DELETE.
- Sử dụng Environment/Variables, viết Test script và chạy Collection Runner.
- Xuất Collection để nộp cùng báo cáo.


## 2. Giới thiệu Postman

Postman là công cụ dùng để thiết kế, gửi và kiểm thử các HTTP request tới API mà không cần viết code client. Các tính năng chính:

| Tính năng | Mô tả |
|---|---|
| Request | Gửi GET/POST/PUT/PATCH/DELETE, cấu hình Params, Headers, Body, Auth |
| Collection | Gom các request liên quan thành một bộ |
| Environment | Quản lý biến theo môi trường (dev, test, prod) |
| Tests | Viết script JavaScript để kiểm tra response tự động |
| Collection Runner | Chạy hàng loạt request và xem kết quả pass/fail |

## 4. Cài đặt

1. Tải Postman tại https://www.postman.com/downloads/ và cài đặt.
2. Đăng nhập hoặc dùng chế độ Lightweight API Client.

> **Hình 1:** Giao diện Postman sau khi cài đặt
>
<img width="1912" height="918" alt="image" src="https://github.com/user-attachments/assets/0b121247-a167-4700-a2f4-67afc38c10c6" />


## 5. Thực hành

API dùng để thực hành: **JSONPlaceholder** (`https://jsonplaceholder.typicode.com`), một API công khai miễn phí.

### 5.1. Tạo Collection

Tạo Collection tên `Bai-thuc-hanh-Postman` và thêm các request bên dưới.
<img width="1578" height="920" alt="image" src="https://github.com/user-attachments/assets/3eaaac19-f691-4cb7-b566-950fbd89e113" />



### 5.2. Request GET – Lấy danh sách bài viết

- Method: `GET`
- URL: `https://jsonplaceholder.typicode.com/posts`
- Kết quả mong đợi: status `200 OK`, trả về mảng 100 bài viết.

>  **Hình 3:** Kết quả GET /posts
>
> <img width="1577" height="917" alt="image" src="https://github.com/user-attachments/assets/a0fd728b-a4e2-4658-bb35-c944f44835b1" />


### 5.3. Request POST – Tạo bài viết mới

- Method: `POST`
- URL: `https://jsonplaceholder.typicode.com/posts`
- Body (raw, JSON):

```json
{
  "title": "Bai viet thu nghiem",
  "body": "Noi dung bai viet",
  "userId": 1
}
```

- Kết quả mong đợi: status `201 Created`, response có `id` mới.

>  **Hình 4:** Kết quả POST /posts
>
> <img width="1567" height="902" alt="image" src="https://github.com/user-attachments/assets/670f210f-760f-4058-b3d7-20049749c428" />


### 5.4. Request PUT – Cập nhật bài viết

- Method: `PUT`
- URL: `https://jsonplaceholder.typicode.com/posts/1`
- Body (raw, JSON):

```json
{
  "id": 1,
  "title": "Tieu de da sua",
  "body": "Noi dung da sua",
  "userId": 1
}
```

- Kết quả mong đợi: status `200 OK`.

>  **Hình 5:** Kết quả PUT /posts/1
>
> <img width="1572" height="914" alt="image" src="https://github.com/user-attachments/assets/9c0f4459-30ed-4af0-b928-90f9755a97a3" />


### 5.5. Request DELETE – Xoá bài viết

- Method: `DELETE`
- URL: `https://jsonplaceholder.typicode.com/posts/1`
- Kết quả mong đợi: status `200 OK`, body `{}`.

> **Hình 6:** Kết quả DELETE /posts/1
>
> <img width="1574" height="912" alt="image" src="https://github.com/user-attachments/assets/0e25d716-dc52-448b-8438-29677c4d1aa1" />


### 5.6. Sử dụng Environment và biến

1. Tạo Environment `Test` với biến `baseUrl = https://jsonplaceholder.typicode.com`.
2. Đổi URL các request thành `{{baseUrl}}/posts`.

> **Hình 7:** Environment và việc dùng biến `{{baseUrl}}`
>
> <img width="1568" height="911" alt="image" src="https://github.com/user-attachments/assets/cc360ce3-4702-4101-b3b0-852de39756c6" />
<img width="1571" height="703" alt="image" src="https://github.com/user-attachments/assets/fe41b23b-4302-4b9a-a147-f4d00c0fd21a" />



### 5.7. Viết Test script

Dán vào tab **Tests** của request GET /posts:

```javascript
pm.test("Status code là 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Thời gian phản hồi dưới 1000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});

pm.test("Response là mảng có 100 phần tử", function () {
    const data = pm.response.json();
    pm.expect(data).to.be.an("array");
    pm.expect(data.length).to.eql(100);
});
```

Test cho request POST:

```javascript
pm.test("Status code là 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Response chứa id", function () {
    pm.expect(pm.response.json()).to.have.property("id");
});
```

> **Hình 8:** Tab Tests và kết quả các test pass
>
> <img width="1103" height="692" alt="image" src="https://github.com/user-attachments/assets/5eefa7c3-c024-4f7f-a6be-b01600fc5a2d" />



## 6. Bảng tổng hợp kết quả

| STT | Request | Method | Kết quả mong đợi | Kết quả thực tế | Pass/Fail |
|---|---|---|---|---|---|
| 1 | Lấy danh sách bài viết | GET | 200 | | |
| 2 | Tạo bài viết | POST | 201 | | |
| 3 | Cập nhật bài viết | PUT | 200 | | |
| 4 | Xoá bài viết | DELETE | 200 | | |


