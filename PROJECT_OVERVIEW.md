# Hệ thống AI Xử lý Văn bản Y khoa

## Tổng quan bài toán

Xây dựng hệ thống AI xử lý văn bản y khoa phi cấu trúc (free-text) — bao gồm ghi chú bác sĩ, giấy xuất viện, kết quả xét nghiệm, hồ sơ EHR — để phát hiện, chuẩn hóa và liên kết các khái niệm y tế.

Mục tiêu cuối cùng: biến dữ liệu lâm sàng phi cấu trúc thành dữ liệu có cấu trúc, liên thông, khai thác được ở quy mô lớn phục vụ chẩn đoán, nghiên cứu dịch tễ và các ứng dụng AI y khoa.

---

## Các chức năng cốt lõi

### 1. Nhận diện thực thể y tế (NER)
Phân loại khái niệm y tế trong văn bản thành các nhóm:
- Triệu chứng (symptoms)
- Kết quả xét nghiệm (lab results)
- Bệnh / chẩn đoán (diagnoses)
- Thuốc (medications)
- Thông tin bệnh nhân (patient demographics)

### 2. Chuẩn hóa (Normalization / Mapping)
- **Bệnh / chẩn đoán** → ánh xạ sang **ICD-10** (Bộ Y tế Việt Nam đã phát hành bản tiếng Việt)
- **Thuốc / hoạt chất** → ánh xạ sang **RxNorm** (bộ từ vựng chuẩn quốc tế về tên thuốc và hoạt chất)

### 3. Suy luận ngữ cảnh
- Phủ định: "không có triệu chứng X"
- Người nhà: "cha bệnh nhân mắc..."
- Tiền sử: "tiền sử bệnh Y"
- Quan hệ giữa các khái niệm (thuốc dùng cho bệnh gì, triệu chứng liên quan đến xét nghiệm nào...)

---

## Kiến trúc dữ liệu

### Database theo chuẩn OMOP CDM
Dữ liệu sau khi xử lý sẽ được lưu vào database theo chuẩn **OMOP Common Data Model (CDM)** — chuẩn quốc tế cho dữ liệu quan sát y tế, cho phép:
- Liên thông dữ liệu giữa các cơ sở y tế
- Truy vấn và nghiên cứu dịch tễ trên quy mô lớn
- Tích hợp với các công cụ phân tích OHDSI ecosystem

### Bộ từ vựng chuẩn (Vocabularies)
| Từ vựng | Phạm vi | Ghi chú |
|---------|---------|---------|
| **ICD-10** | Bệnh, chẩn đoán, triệu chứng | Có bản tiếng Việt từ Bộ Y tế |
| **RxNorm** | Tên thuốc, hoạt chất | Chuẩn quốc tế |

---

## Luồng xử lý tổng quát

```
Văn bản y khoa (giấy / PDF / EHR)
        ↓
   OCR / Text extraction
        ↓
   NLP Pipeline (NER + Context)
        ↓
   Mapping → ICD-10 / RxNorm
        ↓
   Lưu vào OMOP CDM Database
        ↓
   Retrieval / Research / AI Applications
```

---

## Ứng dụng đầu ra

- **Retrieval**: Tra cứu dữ liệu lâm sàng theo khái niệm chuẩn hóa
- **Research**: Nghiên cứu dịch tễ, phân tích xu hướng bệnh tật
- **AI y khoa**: Làm nền tảng dữ liệu cho các mô hình chẩn đoán, gợi ý điều trị
- **Chuyển đổi số y tế**: Số hóa và liên thông hồ sơ bệnh án giữa các cơ sở
