# Consumer–Provider Handshake

## Thông tin chung

- Lab: FIT4110 Lab 03
- Ngày: 2026-06-01
- Provider team: `AI Vision Service`
- Consumer team: `IoT Ingestion`
- Provider service: AI Vision REST API (detection endpoint)
- Consumer service: IoT Ingestion contract test suite / consumer-side smoke client

## Contract

- Contract file: `contracts/ai-vision.openapi.yaml`
- Mock base URL: `http://localhost:4011`
- Auth method: Bearer token, dùng biến Postman `{{authToken}}`
- Endpoint được test:
  - `POST /detect` — gửi frame ảnh để phân tích (camera/IoT trigger)

## Smoke test

### Request: POST /detect

```http
POST /detect
Authorization: Bearer {{authToken}}
Content-Type: application/json
```

```json
{
  "camera_id": "CAM01",
  "image_url": "https://example.com/frame.jpg"
}
```

### Expected response

```json
{
  "detection_id": "det-20260601-001",
  "label": "person",
  "confidence": 0.95
}
```

## Kết quả

- [x] Consumer gọi mock thành công (POST /detect → 200).
- [x] Consumer parse được field cần dùng: `detection_id`, `label`, `confidence`.
- [x] Consumer hiểu lỗi 4xx/5xx provider trả về qua `ProblemDetails` trong contract AI Vision.
- [x] Có Newman report: `reports/newman-report.html`, `reports/newman-report.xml`.

## Ghi chú thay đổi hợp đồng

| Nội dung | Trước | Sau | Người đồng ý |
|---|---|---|---|
| Smoke test provider phụ thuộc | Chưa có bằng chứng chạy được | Thêm request `POST {{aiVisionMockUrl}}/detect` trong folder `05_Consumer_side_Smoke` | Provider (AI Vision) và Consumer (IoT Ingestion) |
| Field consumer cần parse | Chưa ghi rõ | `detection_id`, `label`, `confidence` | Provider (AI Vision) và Consumer (IoT Ingestion) |
| Auth trong test | Có nguy cơ hardcode token | Dùng `Authorization: Bearer {{authToken}}` từ environment | Provider (AI Vision) và Consumer (IoT Ingestion) |

## Xác nhận

- Provider representative: `AI Vision Service`
- Consumer representative: `IoT Ingestion`

## Evidence

- Newman mock report: `reports/newman-report.html`
- Newman XML report: `reports/newman-report.xml`
- Collection: `postman/collections/FIT4110_lab03_iot_ingestion.postman_collection.json`
- Mock environment: `postman/environments/FIT4110_lab03_mock.postman_environment.json`
