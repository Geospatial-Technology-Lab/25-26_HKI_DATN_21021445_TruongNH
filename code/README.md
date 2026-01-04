# Dự báo sinh khối rừng sử dụng Google Earth Engine và Machine Learning

Dự án nghiên cứu và dự báo sinh khối rừng (Above Ground Biomass - AGBD) sử dụng dữ liệu viễn thám Sentinel-1, Sentinel-2 và GEDI kết hợp mô hình LightGBM.


## 📂 Cấu trúc dự án

```
├── GEE/              # Code Google Earth Engine
│   ├── final_image.java       # Tiền xử lý Sentinel & xuất ảnh composite
│   ├── lulc.java             # Xử lý dữ liệu LULC & mặt nạ rừng
│   └── trainingdata.java     # Xuất dữ liệu training
│
├── python/           # Scripts Python xử lý dữ liệu & training model
│   ├── 1-4: Tiền xử lý GEDI
│   ├── 5-9: Đào tạo mô hình 2019
│   └── 10-16: Dự báo tương lai
│
└── README.md         # File này
```

## 📊 Dữ liệu

### Nguồn dữ liệu
- **Sentinel-1 (SAR):** VV, VH polarization + texture features
- **Sentinel-2 (Optical):** Các band quang học + chỉ số thực vật + texture
- **GEDI (LiDAR):** Dữ liệu sinh khối thực tế từ vệ tinh
- **Topography:** Độ cao, độ dốc, hướng dốc từ DEM
- **LULC:** Dữ liệu lớp phủ để lọc vùng rừng

### Quy trình xử lý
1. **Tiền xử lý dữ liệu GEDI:** Lọc nhiễu, xử lý duplicates, tính ma trận tương quan
2. **Tiền xử lý ảnh vệ tinh:** Lọc mây, tính chỉ số, tính texture, gộp ảnh composite
3. **Feature selection:** RFE + tối ưu hóa với Optuna

## 🧠 Mô hình Machine Learning

### Thuật toán
- **LightGBM:** Gradient Boosting Decision Tree
- **Hyperparameter Tuning:** Optuna framework
- **Feature Selection:** Recursive Feature Elimination (RFE)

### Đặc điểm
- Xử lý dữ liệu theo chunks để tối ưu bộ nhớ
- Lọc nhiễu bằng residual analysis
- Post-processing với LULC mask
- Pipeline dự báo đa năm (2019-2050)

## 🛠️ Quy trình thực hiện

### PHẦN A: Tiền xử lý trên Google Earth Engine
1. Chạy `final_image.java` → Xuất ảnh composite Sentinel
2. Chạy `lulc.java` → Xuất mặt nạ rừng
3. Chạy `trainingdata.java` → Xuất dữ liệu training CSV

### PHẦN B: Tiền xử lý dữ liệu GEDI (Python)
4. `1_matrantuongquan1` → Phân tích tương quan biến
5. `2_matrantuongquan2` → Loại bỏ biến dư thừa
6. `3_loc_nhieu_GEDI` → Lọc nhiễu bằng LightGBM
7. `4_loc_duplicates_GEDI` → Xử lý duplicates

### PHẦN C: Đào tạo mô hình cho năm 2019
8. `5_cell1_colab_khai_bao` → Cài đặt môi trường
9. `6_cell2_colab_RFE_Optuna` → Feature selection & tuning
10. `7_cell3_colab_train_mo_hinh` → Train model
11. `8_cell4_colab_Scatter_Plots` → Đánh giá kết quả
12. `9_cell5_colab_create_map_2019_2025` → Tạo bản đồ sinh khối

### PHẦN D: Dự báo tương lai (2025-2050)
13. `10_cau_hinh_` → Setup môi trường Colab
14. `11_cell1_load_data` → Load & EDA
15. `12_cell2_loc_nhieu` → Lọc nhiễu
16. `13_cell3_optuna` → Tối ưu hyperparameters
17. `14_cell4_train_model` → Train model dự báo
18. `15_cell5_prediction` → Dự đoán bản đồ
19. `16_cell5_5_post_process` → Post-processing với LULC

**Lặp lại bước 18-19 để dự báo các năm tiếp theo (2030, 2035, 2040, ...)**

## 🎯 Kết quả

- Bản đồ sinh khối rừng độ phân giải 10m
- Dự báo xu hướng biến đổi sinh khối từ 2019-2050
- Scatter plots đánh giá độ chính xác mô hình
- Phân tích Delta AGB theo thời gian

## 📝 Lưu ý

- Dữ liệu được xử lý theo chunks để tối ưu bộ nhớ
- Sử dụng nén LZW khi xuất file raster
- Chạy trên Google Colab với GPU để tăng tốc
- Cần kết nối Google Drive để lưu trữ dữ liệu

## 👤 Tác giả

**Trương**

> Dự án phát triển cho mục đích nghiên cứu & học thuật.

**Last updated:** January 2026