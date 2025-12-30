# T25th-MALLORN-Astronomical-Classification

## Kaggle competition: 

- **Kaggle**: [MALLORN Astronomical Classification Challenge](https://www.kaggle.com/competitions/mallorn-astronomical-classification-challenge/)
- **Dataset**: 20 splits của lightcurves
- **Target**: Binary classification (TDE vs Non-TDE)
- **Evaluation**: F1 Score

## Giới thiệu vấn đề:

**Tidal Disruption Events (TDEs)** là một trong những hiện tượng hiếm có và giá trị khoa học cao:
- Xảy ra khi một ngôi sao bị nuốt chửng khi đi quá gần hố đen siêu khối lượng (Super Massive Black Hole), thường là tại tâm các thiên hà.
- Việc quan sát và phát hiện những sự kiện thiên văn này đem lại rất nhiều thông tin cho Vật Lý Thiên Văn nói riêng và tri thức vũ trụ nói chung.
- Tuy nhiên, các sự kiện này rất hiếm khi xảy ra, và thời điểm con người bắt đầu chính thức quan sát được các sự kiện này kể từ năm 1990, dẫn tới lượng dữ liệu về TDE còn vô cùng ít ỏi giữa vô vàn nguồn dữ liệu thiên văn khác.

**Thách thức chính:** Làm sao ta có thể phát hiện, dự đoán các TDEs này trong số hàng nghìn hiện tượng thiên văn quan sát được mỗi năm? Hay cụ thể trong cuộc thi này, tìm ra TDEs lẩn khuất giữa những ghi nhận SuperNova (SN) và Active Galactic Nucleus (AGN).

## Tổng quan

Dự án này thực hiện phân loại các sự kiện thiên văn để xác định đâu là Tidal Disruption Events (TDEs) và đâu là các sự kiện khác dựa trên dữ liệu lightcurve từ bộ dữ liệu trong 10 năm quan sát được của kính thiên văn Large Synoptic Survey Telescope - LSST.

### Cấu trúc thư mục

````
ML/
├── mallorn-data-on-the-first-sight.ipynb      # EDA: những quan sát ban đầu về dữ liệu và đặc điểm của các phân loại.
├── Single Model LGBM/                         # Bước đầu thử nghiệm với mô hình LGBM đơn lẻ.
│   └── lightgbm-vers-3.4.ipynb
├── Single Model XGBoost/                      # Features Engineering và huấn luyện triệt để với mô hình XGBoost đơn lẻ.
│   ├── single_model_xgboost.ipynb
│   └── submission_single_model_best.csv       # Test Output cho điểm số tốt nhất
└── Ensemble Model XGB+CAT/                    # Thử nghiệm mô hình Ensemble, Cascading và một số phương pháp hậu kiểm khác.
    └── ensemble_model_xgb_cat.ipynb
````

## Chiến lược chính

### 1. Feature Engineering

Dựa trên phân tích dữ liệu thực tế và đặc điểm vật lý của TDEs:

- **Global Statistics & Morphology** :
  - Min, Max, Mean, Median, Std
  - Skewness, Kurtosis
  - MAD (Median Absolute Deviation), IQR
  - Amplitude, Peak-to-median-time,...

- **Time-Series Variability & Noise** :
  - Stetson J & K, Von Neumann Ratio
  - Đạo hàm & hình thái biến đổi
  - Zero-crossings, Fraction negative flux (lọc AGN)
 
- **Physics-based Modeling & Curve Fitting** :
  - Bazin & Exponential fitting (đo tốc độ tăng sáng cho SN và TDE)
  - Gaussian Process (GP)
 
- **Color & Spectral Evolution** :
  - Weighted flux, weighted colors, 
  - Color centroid, Max-based color, ...
  - Color shifting (theo thời gian)
 
- **Cross-band Aggregation** :
  - Colors: u-g, u-r, u-i, g-r, g-i, r-i
  - Peak Lags & Color Slope (đo tốc độ thay đổi không đồng nhất giữa các color)

### 2. Modeling Approaches

#### v1: LightGBM
- Gradient boosting framework 
- Class imbalance handling với `scale_pos_weight`
- K-Fold Cross Validation

#### v2: XGBoost
- Quick Feature Selection (chọn lọc đặc trưng tiền kỳ)
- Pseudo-labeling (tăng cường dữ liệu bằng các test-sample có độ chắc chắn cao)
- Seed Averaging
- Threshold optimization cho F1-score

#### v3: Ensemble model
- Cascading: XGBoost (dự đoán sơ lược) + CatBoost (tập trung xử lý case khó)
- Ensemble optimization (factor | metadata model)
- Cross-validation cho từng model
- Ranking và Decision Rule (chọn lọc ưu tiên tối đa cho nhãn TDE)

## Kết quả

F1-score = 0.6304 (Leaderboard)

## Contributors

- Nguyễn Xuân Kiên
- Nguyễn Trường Sơn
- Nguyễn Văn Vượng 

---
