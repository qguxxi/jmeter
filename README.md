# Báo Cáo Kiểm Tra Hiệu Năng với Apache JMeter

## Mục Lục

- [Kiểm Tra Hiệu Năng Trang Web](#1-kiểm-tra-hiệu-năng-trang-web)
- [Kiểm Tra Hiệu Năng API Thời Tiết](#2-kiểm-tra-hiệu-năng-api-thời-tiết-open-meteo)

---

## 1. Kiểm Tra Hiệu Năng Trang Web

### Mục Tiêu

Sử dụng Apache JMeter để mô phỏng người dùng truy cập trang web **https://phenikaa-uni.edu.vn/vi**, từ đó đánh giá hiệu năng thực tế của hệ thống.

### Kịch Bản Kiểm Tra

#### Thread Group

| Thông số | Giá trị |
|---|---|
| Số lượng thread (người dùng) | 100 |
| Thời gian chạy | 60 giây |
| Ramp-up period | 10 giây |

#### HTTP Request

| Thông số | Giá trị |
|---|---|
| URL | `https://phenikaa-uni.edu.vn/vi` |
| Method | GET |
| Content Encoding | UTF-8 |

#### Listeners

- View Results Tree
- Aggregate Report

### Kết Quả Kiểm Tra

| Chỉ số | Giá trị |
|---|---|
| Tổng số yêu cầu | 1.000 |
| Số yêu cầu thành công | 998 (99,8%) |
| Số yêu cầu thất bại | 2 (0,2%) |
| Thời gian phản hồi trung bình | 40 ms |
| Thời gian phản hồi trung vị (50th) | 38 ms |
| Thời gian phản hồi Percentile 90 | 70 ms |
| Chuyển tải (Throughput) | 16 yêu cầu/giây |

### Phân Tích

- **Tỷ lệ thành công cao:** 998/1000 yêu cầu thành công (99,8%) cho thấy hệ thống hoạt động ổn định.
- **Tỷ lệ lỗi thấp:** Chỉ 0,2% yêu cầu thất bại — mức chấp nhận được trong môi trường tải cao.
- **Thời gian phản hồi tốt:** Trung bình 40 ms và trung vị 38 ms đều nằm dưới ngưỡng 100 ms, đảm bảo trải nghiệm người dùng mượt mà.
- **Percentile 90 ở mức 70 ms:** 90% người dùng nhận phản hồi trong vòng 70 ms — rất tốt.
- **Throughput ổn định:** 16 yêu cầu/giây là mức chuyển tải hợp lý với 100 người dùng đồng thời.

### Kết Luận

> ✅ **Trang web https://phenikaa-uni.edu.vn/vi có hiệu năng tốt.** Hệ thống xử lý tốt tải đồng thời 100 người dùng trong 60 giây, với thời gian phản hồi thấp và tỷ lệ lỗi không đáng kể.

---

## 2. Kiểm Tra Hiệu Năng API Thời Tiết (Open-Meteo)

### Mục Tiêu

Sử dụng Apache JMeter để kiểm tra hiệu năng API thời tiết mã nguồn mở **Open-Meteo**, cụ thể endpoint dự báo thời tiết theo giờ tại tọa độ Berlin (latitude=52.52, longitude=13.41).

### Endpoint API

```
GET https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m
```

#### Mô tả tham số

| Tham số | Giá trị | Mô tả |
|---|---|---|
| `latitude` | `52.52` | Vĩ độ (Berlin, Đức) |
| `longitude` | `13.41` | Kinh độ (Berlin, Đức) |
| `hourly` | `temperature_2m` | Nhiệt độ 2m theo giờ (°C) |

#### Ví dụ phản hồi JSON

```json
{
  "latitude": 52.52,
  "longitude": 13.419998,
  "generationtime_ms": 0.123,
  "utc_offset_seconds": 0,
  "timezone": "GMT",
  "hourly_units": {
    "time": "iso8601",
    "temperature_2m": "°C"
  },
  "hourly": {
    "time": ["2024-01-01T00:00", "2024-01-01T01:00", "..."],
    "temperature_2m": [3.2, 2.8, 2.5, "..."]
  }
}
```

### Kịch Bản Kiểm Tra

#### Thread Group

| Thông số | Giá trị |
|---|---|
| Số lượng thread (người dùng) | 100 |
| Thời gian chạy | 60 giây |
| Ramp-up period | 10 giây |

#### HTTP Request

| Thông số | Giá trị |
|---|---|
| Server Name | `api.open-meteo.com` |
| Protocol | `HTTPS` |
| Path | `/v1/forecast` |
| Method | `GET` |
| Content Encoding | `UTF-8` |

#### HTTP Request Parameters

| Tên | Giá trị |
|---|---|
| `latitude` | `52.52` |
| `longitude` | `13.41` |
| `hourly` | `temperature_2m` |

#### Listeners

- View Results Tree
- Aggregate Report

### Kết Quả Kiểm Tra

| Chỉ số | Giá trị |
|---|---|
| Tổng số yêu cầu | 1.000 |
| Số yêu cầu thành công | 996 (99,6%) |
| Số yêu cầu thất bại | 4 (0,4%) |
| Thời gian phản hồi trung bình | 10 ms |
| Thời gian phản hồi trung vị (50th) | — |
| Thời gian phản hồi Percentile 90 | — |
| Chuyển tải (Throughput) | — |

### Phân Tích

- **Tỷ lệ thành công cao:** 996/1000 yêu cầu thành công (99,6%) — API hoạt động ổn định dưới tải cao.
- **Tỷ lệ lỗi thấp:** 0,4% yêu cầu thất bại, có thể do rate limiting hoặc timeout mạng thoáng qua.
- **Thời gian phản hồi rất nhanh:** Trung bình chỉ 10 ms — API Open-Meteo có hiệu suất cực kỳ cao, phù hợp tích hợp real-time.
- **API miễn phí, không cần API key:** Open-Meteo là dịch vụ mã nguồn mở, không yêu cầu xác thực cho các truy vấn cơ bản.

### Kết Luận

> ✅ **API Open-Meteo có hiệu năng xuất sắc.** Với thời gian phản hồi trung bình chỉ 10 ms và tỷ lệ thành công 99,6%, API này phù hợp cho các ứng dụng yêu cầu dữ liệu thời tiết theo thời gian thực với lưu lượng cao.

---

## So Sánh Tổng Quan

| Chỉ số | Trang Web Phenikaa | API Open-Meteo |
|---|---|---|
| Tỷ lệ thành công | 99,8% | 99,6% |
| Tỷ lệ thất bại | 0,2% | 0,4% |
| Thời gian phản hồi TB | 40 ms | 10 ms |
| Throughput | 16 req/s | — |
| Đánh giá | ✅ Tốt | ✅ Xuất sắc |

---

## Công Cụ Sử Dụng

- **Apache JMeter** — Công cụ kiểm tra hiệu năng mã nguồn mở
- **Thread Group** — Mô phỏng người dùng đồng thời
- **HTTP Request Sampler** — Gửi yêu cầu HTTP/HTTPS
- **View Results Tree** — Xem chi tiết từng yêu cầu
- **Aggregate Report** — Tổng hợp thống kê kết quả

---

*Báo cáo được thực hiện với cấu hình 100 thread, ramp-up 10 giây, thời gian chạy 60 giây.*
