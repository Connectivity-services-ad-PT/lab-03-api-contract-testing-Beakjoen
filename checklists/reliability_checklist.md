# Reliability Checklist — FIT4110 Lab 03 — IoT Ingestion

## 1. Functional tests

- [x] Có test cho endpoint health: `GET /health`.
- [x] Có test happy path cho endpoint chính: `POST /readings` (temperature reading).
- [x] Có kiểm tra status code 2xx: `200` và `201`.
- [x] Có kiểm tra field quan trọng trong response: `status`, `service`, `reading_id`, `device_id`, `metric`, `accepted`.
- [x] Có test đọc dữ liệu danh sách: `GET /readings/latest`.

## 2. Auth tests

- [x] Có test request có token hợp lệ.
- [x] Có test thiếu token.
- [x] Có test sai token hoặc token rỗng.
- [x] Endpoint public được khai báo rõ nếu không cần auth: `GET /health`.
- [x] Test thể hiện đúng expected status `401/403` cho service thật; với mock, invalid-token case được skip có kiểm soát vì Prism không chứng minh auth middleware thật.

## 3. Negative tests

- [x] Có test thiếu field bắt buộc: thiếu `device_id`.
- [x] Có test sai enum: `metric=temp_hot` (không có trong enum).
- [x] Có test sai kiểu dữ liệu: `value` gửi dạng string.
- [x] Lỗi trả về theo cùng một error model: `ProblemDetails` (RFC 9457).

## 4. Boundary tests

- [x] Có test max boundary: `value=80` (maximum theo schema).
- [x] Có test out-of-range: `value=81` bị từ chối.
- [x] Có test limit/pagination: `limit=101` bị từ chối.
- [x] Có ghi chú kỳ vọng xử lý dữ liệu biên trong test-case matrix.

## 5. Reliability tests cơ bản

- [x] Có kiểm tra response time trong folder `06_Local_only_NonFunctional`.
- [x] Có mô tả timeout mong muốn: local response time dưới `1000ms`.
- [x] Có test rate limit: `POST /readings` trả `429 Too Many Requests`.
- [x] Có consumer-side smoke test với mock của AI Vision: gọi `POST /detect`.

## 6. Evidence

- [x] Collection export JSON: `postman/collections/FIT4110_lab03_iot_ingestion.postman_collection.json`.
- [x] Environment mock export JSON: `postman/environments/FIT4110_lab03_mock.postman_environment.json`.
- [x] Environment local export JSON: `postman/environments/FIT4110_lab03_local.postman_environment.json`.
- [x] Newman report XML/HTML: `reports/newman-report.xml`, `reports/newman-report.html`.
- [x] Contract lint report: `reports/contract-lint-report.txt`.
- [x] Test-case matrix đã điền: `templates/test-case-matrix.csv`.
- [x] Biên bản handshake đã điền: `templates/consumer-provider-handshake.md`.

## Ghi chú

- Contract từ Lab 02, bổ sung response `429` cho Lab 03.
- Consumer-side smoke test gọi AI Vision mock — dependency theo pairing matrix.
- Contract lint: Pass.
- Mock test: Pass (đã xuất report HTML).
