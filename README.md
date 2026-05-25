# Vietnam E-commerce 2023 — Phân tích dữ liệu

Notebook phân tích bộ dữ liệu đơn hàng thương mại điện tử Việt Nam năm 2023, tập trung vào các insight về doanh thu, chiết khấu, hoàn hàng, giao hàng và phân khúc khách hàng.

---

## Dữ liệu đầu vào

| Trường | Mô tả |
|---|---|
| `order_id` | Mã đơn hàng |
| `month` | Tháng phát sinh đơn |
| `category` | Ngành hàng |
| `region` | Miền (Bắc / Trung / Nam) |
| `channel` | Kênh bán (App / Web / khác) |
| `age_group` | Nhóm tuổi khách hàng |
| `gender` | Giới tính khách hàng |
| `net_value` | Giá trị đơn hàng (VND) |
| `discount_pct` | Tỉ lệ chiết khấu |
| `profit` | Lợi nhuận (VND) |
| `returned` | Đơn bị hoàn (0/1) |
| `delivery_days` | Số ngày giao hàng |
| `rating` | Đánh giá của khách (1–5) |

**Nguồn file:** `vietnam_ecommerce_2023.csv`

---

## Cấu trúc phân tích

### Tổng quan (EDA)
Khám phá sơ bộ: shape, kiểu dữ liệu, thống kê mô tả, kiểm tra missing values, phân bố doanh thu theo tháng và theo ngành hàng.

### S03 — Chiết khấu sâu làm tăng hoàn hàng và ăn mòn lợi nhuận
Phân nhóm `discount_pct` thành 4 tier (`<10%`, `10–20%`, `20–30%`, `≥30%`), so sánh tỉ lệ hoàn hàng và lợi nhuận trung bình. Biểu đồ cột đôi trục Y.

### S04 — Miền Trung giao hàng chậm hơn nhưng đơn giá cao nhất
Nhóm theo `region`, so sánh thời gian giao hàng trung bình và AOV (Average Order Value). Biểu đồ ngang đôi cột, highlight Miền Trung.

### S05 — Thời trang có tỉ lệ hoàn hàng cao nhất toàn sàn
Nhóm theo `category`, tính `return_rate` và profit margin. Biểu đồ cột ngang, highlight ngành rủi ro cao nhất.

### S06 — Giao hàng trễ làm rating giảm rõ rệt
Nhóm theo `delivery_days`, vẽ đường rating trung bình, đánh dấu ngưỡng 4 ngày là điểm rating bắt đầu sụt giảm.

### S07 — App tạo phần lớn đơn hàng, nhưng Web có AOV cao hơn
Nhóm theo `channel`, so sánh tỉ trọng đơn và AOV. Biểu đồ cột + đường, dual-axis.

### S08 — Nhóm Nam 45–54 có giá trị đơn hàng cao nhất nhưng quy mô nhỏ
Heatmap `age_group × gender` theo AOV, kèm số đơn trong từng ô để nhận diện phân khúc tiềm năng chưa được khai thác.

---

## Thư viện sử dụng

```
pandas
numpy
matplotlib
seaborn
```

---

## Cách chạy

```bash
# Cài thư viện (nếu chưa có)
pip install pandas numpy matplotlib seaborn

# Mở notebook
jupyter notebook vietnam_ecomerce.ipynb
```

Đặt file `vietnam_ecommerce_2023.csv` cùng thư mục với notebook trước khi chạy.

---

## Các insight chính

- **Chiết khấu ≥30%** làm tăng tỉ lệ hoàn hàng và kéo lợi nhuận trung bình xuống thấp hơn đáng kể so với nhóm chiết khấu <10%.
- **Miền Trung** giao hàng chậm nhất nhưng có AOV cao nhất trong 3 miền — đây là phân khúc địa lý cần cân bằng giữa logistics và khai thác giá trị đơn hàng.
- **Thời trang** dẫn đầu về tỉ lệ hoàn hàng, cần kiểm soát sai size/sai kỳ vọng và tối ưu chi phí hoàn.
- **Giao hàng trong ≤3 ngày** là ngưỡng bảo vệ rating; từ 4 ngày trở lên rating giảm rõ rệt.
- **App** chiếm phần lớn số đơn; **Web** có AOV cao hơn — chiến lược kênh nên phân tách: App tối ưu chuyển đổi, Web tập trung giỏ hàng giá trị cao.
- **Nam 45–54** là phân khúc có AOV cao nhất nhưng chiếm tỉ trọng nhỏ — tiềm năng để thử nghiệm targeting riêng.
