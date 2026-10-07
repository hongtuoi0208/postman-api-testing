# BÁO CÁO KIỂM THỬ API BẰNG POSTMAN

## 1. Thông tin bài thực hành

**Môn học:** Đánh giá và kiểm định chất lượng phần mềm
**Nội dung:** Kiểm thử API bằng công cụ Postman
**Công cụ:** Postman
**API sử dụng:** Postman Echo API
**Sinh viên thực hiện:** Bùi Thị Hồng Tươi - 23015124

---

## 2. Mục tiêu

Bài thực hành nhằm làm quen với công cụ Postman và thực hiện kiểm thử API thông qua các phương thức HTTP.

Các nội dung thực hiện:

* Tạo Collection trong Postman.
* Thực hiện GET Request.
* Thực hiện POST Request.
* Viết các Test Script để kiểm tra kết quả API.
* Kiểm tra trường hợp kiểm thử PASS và FAIL.
* Chạy nhiều Request bằng Collection Runner.
* Lưu Collection để phục vụ việc nộp bài và tái sử dụng.

---

## 3. Công cụ và môi trường

| Thành phần        | Thông tin                       |
| ----------------- | ------------------------------- |
| Công cụ kiểm thử  | Postman                         |
| API               | Postman Echo                    |
| Phương thức GET   | `https://postman-echo.com/get`  |
| Phương thức POST  | `https://postman-echo.com/post` |
| Định dạng dữ liệu | JSON                            |
| Collection        | Postman API Testing             |

---

# 4. Thực hiện kiểm thử GET

## 4.1. Tạo GET Request

Tạo Request có tên:

`GET - Basic Request`

URL:

```text
https://postman-echo.com/get
```

Phương thức:

```text
GET
```

Kết quả trả về thành công với HTTP Status Code:

```text
200 OK
```


<img width="952" height="1021" alt="Screenshot 2026-10-07 163626" src="https://github.com/user-attachments/assets/50908b74-e5ad-467e-8d78-9a9cd919ddae" />

---

## 4.2. Xây dựng Test Script cho GET

Các kiểm thử được thực hiện gồm:

1. Kiểm tra Status Code bằng 200.
2. Kiểm tra Response có định dạng JSON.
3. Kiểm tra Response có trường URL.
4. Kiểm tra Response có Header `Content-Type`.
5. Kiểm tra thời gian phản hồi nhỏ hơn 1000 ms.

Test Script:

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("Response contains URL", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.url).to.exist;
});

pm.test("Content-Type header exists", function () {
    pm.response.to.have.header("Content-Type");
});

pm.test("Response time is less than 1000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});
```

### Kết quả

Tất cả 5 kiểm thử đều đạt:

| STT | Nội dung kiểm thử       | Kết quả |
| --- | ----------------------- | ------- |
| 1   | Status Code = 200       | PASS    |
| 2   | Response là JSON        | PASS    |
| 3   | Response có URL         | PASS    |
| 4   | Có Content-Type Header  | PASS    |
| 5   | Response Time < 1000 ms | PASS    |

<img width="1222" height="1025" alt="Screenshot 2026-10-07 170928" src="https://github.com/user-attachments/assets/c65333c3-a09f-419d-ad57-db1ca67b8726" />


---

# 5. Thực hiện kiểm thử POST

## 5.1. Tạo POST Request

Tạo Request có tên:

`POST - Send Data`

URL:

```text
https://postman-echo.com/post
```

Phương thức:

```text
POST
```

Body được chọn:

```text
raw → JSON
```

Dữ liệu gửi lên:

```json
{
    "name": "Hong Tuoi",
    "course": "Software Testing",
    "subject": "API Testing"
}
```

API trả về thành công với:

```text
200 OK
```

<img width="1288" height="1018" alt="Screenshot 2026-10-07 165436" src="https://github.com/user-attachments/assets/a552e3ae-9b4a-453a-acbf-a3c40645d119" />

---

## 5.2. Kiểm thử POST

Các kiểm thử được thực hiện:

1. Kiểm tra Status Code = 200.
2. Kiểm tra Response là JSON.
3. Kiểm tra dữ liệu gửi lên được trả về chính xác.

Test Script:

```javascript
pm.test("POST Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("POST Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("POST data is correct", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.data.name).to.eql("Hong Tuoi");
    pm.expect(jsonData.data.course).to.eql("Software Testing");
    pm.expect(jsonData.data.subject).to.eql("API Testing");
});
```

### Kết quả

| STT | Nội dung kiểm thử      | Kết quả |
| --- | ---------------------- | ------- |
| 1   | POST Status Code = 200 | PASS    |
| 2   | Response là JSON       | PASS    |
| 3   | Dữ liệu POST chính xác | PASS    |

<img width="1287" height="1012" alt="Screenshot 2026-10-07 165756" src="https://github.com/user-attachments/assets/c96f81c2-173c-4d28-9faa-ed741d2b15c9" />

---

# 6. Kiểm thử trường hợp FAIL

Để kiểm tra khả năng phát hiện lỗi của Postman, tạo một test yêu cầu Status Code bằng `404`, trong khi API thực tế trả về `200`.

Test:

```javascript
pm.test("POST Status code should be 404", function () {
    pm.response.to.have.status(404);
});
```

Kết quả kiểm thử không đạt vì:

```text
Expected: 404
Actual: 200
```

Điều này chứng minh Postman có khả năng phát hiện trường hợp kết quả thực tế không đáp ứng điều kiện kiểm thử.

<img width="1281" height="1016" alt="Screenshot 2026-10-07 165703" src="https://github.com/user-attachments/assets/8a7cc4a4-4d4d-427c-afc4-a9488005623c" />


Sau khi kiểm tra trường hợp FAIL, Test Script được khôi phục lại điều kiện đúng là `200`.

---

# 7. Chạy Collection Runner

Collection được tạo với hai Request:

```text
GET - Basic Request
POST - Send Data
```

Collection Runner được sử dụng để chạy tự động các Request trong Collection.

Cấu hình:

* Source: Runner
* Environment: None
* Iterations: 1

Kết quả:

* GET Request: PASS
* POST Request: PASS
* Các Test Script được thực hiện thành công.


<img width="1282" height="1020" alt="Screenshot 2026-10-07 170740" src="https://github.com/user-attachments/assets/5f65fa7e-c348-4950-ac9b-45f5e113f191" />


# 8. Tổng hợp kết quả kiểm thử

| Request             | Số Test | Kết quả          |
| ------------------- | ------: | ---------------- |
| GET - Basic Request |       5 | PASS             |
| POST - Send Data    |       3 | PASS             |
| Test trường hợp sai |       1 | FAIL có chủ đích |

Trường hợp FAIL được tạo ra nhằm kiểm tra khả năng phát hiện lỗi của bộ kiểm thử, không phải lỗi của API.

---

# 9. Collection Postman

Collection đã được export để có thể import và chạy lại trên Postman.

File:

`postman/Postman_API_Testing.json`

Collection bao gồm:

* GET - Basic Request
* POST - Send Data
* Các Test Script kiểm tra kết quả API.

---

# 10. Kết luận

Qua bài thực hành, em đã làm quen với công cụ Postman và thực hiện được quy trình kiểm thử API cơ bản.

Các nội dung đã thực hiện gồm tạo Collection, gửi GET và POST Request, kiểm tra Status Code, kiểm tra JSON Response, kiểm tra Header, kiểm tra thời gian phản hồi, kiểm tra dữ liệu trả về và sử dụng Collection Runner.

Kết quả cho thấy các API được kiểm thử hoạt động đúng theo các điều kiện đã đặt ra. Đồng thời, việc tạo một trường hợp FAIL có chủ đích giúp hiểu rõ hơn cách Postman phát hiện kết quả không đúng với Expected Result.

Bài thực hành giúp em hiểu được vai trò của kiểm thử API trong quá trình đánh giá chất lượng phần mềm và cách sử dụng Postman để tự động hóa một phần quá trình kiểm thử.
