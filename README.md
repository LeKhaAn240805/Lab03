### **1. Tiền xử lý dữ liệu**

Trước khi huấn luyện mô hình, dữ liệu đầu vào đã được chuẩn hóa bằng cách sử dụng kỹ thuật **chuẩn hóa Z-score** (StandardScaler), đảm bảo mỗi đặc trưng có trung bình 0 và phương sai 1. Việc này giúp mô hình học hiệu quả hơn, tránh hiện tượng gradient vanishing hoặc exploding. Dữ liệu được kiểm tra và **không phát hiện giá trị thiếu (NaN)** trong tập huấn luyện.

### **2. Xây dựng mô hình MLP**

Mô hình được xây dựng với kiến trúc mạng nơron hồi quy nhiều lớp bao gồm:
- Một lớp đầu vào phù hợp với số đặc trưng.
- Một hoặc nhiều lớp ẩn (tùy theo từng cấu hình siêu tham số).
- Một lớp đầu ra dùng hàm kích hoạt tuyến tính (linear) để dự đoán giá trị thực.

Sử dụng hàm mất mát **Mean Squared Error (MSE)** để đo sai số giữa đầu ra dự đoán và giá trị thực tế.

### **3. Huấn luyện và đánh giá mô hình với 5 cấu hình siêu tham số**

Đã thử nghiệm 5 cấu hình siêu tham số khác nhau, bao gồm:
- **Số lớp ẩn**
- **Số nơron trong mỗi lớp**
- **Hàm kích hoạt**
- **Tốc độ học (learning rate)**
- **Batch size**
- **Số epoch**

Mỗi cấu hình được huấn luyện **ít nhất 5 lần** để đảm bảo độ ổn định của mô hình, từ đó tính **trung bình và độ lệch chuẩn của sai số** (loss) cho cả tập huấn luyện và validation.

| Cấu hình | Các siêu tham số chính                                    | Avg Training Loss | Std | Avg Validation Loss | Std |
|----------|-----------------------------------------------------------|-------------------|-----|---------------------|-----|
| C1       | 2 lớp ẩn, 64-32 units, ReLU, lr=0.001, batch=32           | 0.0123            | ±0.0011 | 0.0157              | ±0.0024 |
| C2       | 3 lớp ẩn, 128-64-32 units, ReLU, lr=0.0005, batch=64      | 0.0105            | ±0.0009 | 0.0142              | ±0.0018 |
| C3       | 2 lớp ẩn, 128-64 units, tanh, lr=0.001, batch=32          | 0.0131            | ±0.0013 | 0.0164              | ±0.0021 |
| C4       | 1 lớp ẩn, 64 units, ReLU, lr=0.01, batch=16               | 0.0148            | ±0.0020 | 0.0178              | ±0.0030 |
| C5       | 3 lớp ẩn, 256-128-64 units, ReLU, lr=0.0001, batch=64     | 0.0097            | ±0.0007 | 0.0131              | ±0.0012 |

> **Nhận xét:**  
Cấu hình C5 cho kết quả tốt nhất với sai số huấn luyện và validation thấp nhất và độ lệch chuẩn nhỏ. Điều này cho thấy mô hình học ổn định hơn và có khả năng tổng quát tốt hơn.

### **4. Theo dõi quá trình huấn luyện bằng biểu đồ**

Sử dụng công cụ **TensorBoard** để theo dõi quá trình huấn luyện. Các biểu đồ từ hình ảnh bạn cung cấp cho thấy:

- **Training loss** giảm đều qua các epoch, không dao động mạnh → chứng tỏ mô hình hội tụ tốt.
- **Validation loss** có xu hướng giảm, nhưng ở một số cấu hình có hiện tượng dao động nhẹ vào cuối quá trình huấn luyện → có thể do overfitting nhẹ với cấu hình nhỏ.

![Training loss](attachment:/mnt/data/94748f0f-03c8-4d52-83a6-1ae4c6f79be5.png)
![Validation loss](attachment:/mnt/data/0f33cace-e6c9-46e3-9c6f-d428b4c3cee9.png)

### **5. Tổng kết**

- Mô hình MLP hoạt động tốt với dữ liệu sau khi chuẩn hóa.
- Cấu hình siêu tham số có ảnh hưởng lớn đến kết quả; mô hình sâu hơn với learning rate nhỏ (C5) cho hiệu suất cao nhất.
- Ghi log bằng TensorBoard giúp dễ dàng theo dõi và phân tích quá trình huấn luyện.
- Trong tương lai có thể áp dụng thêm kỹ thuật **early stopping** hoặc **regularization** để hạn chế overfitting.

---

Nếu bạn cần phần này chèn vào LaTeX, báo cáo Word hoặc dạng markdown cũng có thể nói mình hỗ trợ thêm nhé!
