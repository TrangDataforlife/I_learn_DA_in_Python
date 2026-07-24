# I_learn_DA_in_Python

# 🐼 Pandas & Python DB-API Cheat Sheet

Tài liệu tổng hợp và hệ thống hóa kiến thức xử lý dữ liệu cơ bản trong Python, bao gồm làm việc với **Pandas DataFrame**, khám phá dữ liệu (**EDA**), kết nối cơ sở dữ liệu qua **Python DB-API** và tiền xử lý dữ liệu (**Data Wrangling**).

---

## 📑 Mục lục

- [1. Data I/O (Đọc & Xuất Dữ Liệu)](#1-data-io-đọc--xuất-dữ-liệu)
  - [1.1. Thao tác với File CSV](#11-thao-tác-với-file-csv)
  - [1.2. Thao tác với các định dạng khác](#12-thao-tác-với-các-định-dạng-khác)
- [2. Exploratory Data Analysis (EDA)](#2-exploratory-data-analysis-eda)
  - [2.1. Kiểm tra & Chuyển đổi Kiểu dữ liệu](#21-kiểm-tra--chuyển-đổi-kiểu-dữ-liệu)
  - [2.2. Phân phối Dữ liệu & Thống kê Mô tả](#22-phân-phối-dữ-liệu--thống-kê-mô-tả)
  - [2.3. Làm sạch dữ liệu ban đầu](#23-làm-sạch-dữ-liệu-ban-đầu)
- [3. Working with Databases (Python DB-API)](#3-working-with-databases-python-db-api)
  - [3.1. Các đối tượng cốt lõi](#31-các-đối-tượng-cốt-lõi)
  - [3.2. Phương thức chính của Connection Object](#32-phương-thức-chính-của-connection-object)
  - [3.3. Quy trình thao tác mẫu](#33-quy-trình-thao-tác-mẫu)
- [4. Data Wrangling & Pre-processing](#4-data-wrangling--pre-processing)
  - [4.1. Xử lý giá trị khuyết (Null) & Trùng lặp](#41-xử-lý-giá-trị-khuyết-null--trùng-lặp)
  - [4.2. Chuẩn hóa Định dạng & Tên cột](#42-chuẩn-hóa-định-dạng--tên-cột)
  - [4.3. Chuẩn hóa Thang đo (Data Normalization)](#43-chuẩn-hóa-thang-đo-data-normalization)
  - [4.4. Gom nhóm dữ liệu số (Data Binning)](#44-gom-nhóm-dữ-liệu-số-data-binning)
  - [4.5. Mã hóa biến phân loại (Categorical to Numeric)](#45-mã-hóa-biến-phân-loại-categorical-to-numeric)
- [5. Exploratory Data Analysis (EDA)](#5-exploratory-data-analysis-eda)
- [Steps](#steps)
  - [5.1. Descriptive Statistics (Thống kê mô tả)](#51-descriptive-statistics-thống-kê-mô-tả)
  - [5.2. Group By & Heatmap](#52-group-by--heatmap)
  - [5.3. ANOVA (Analysis of Variance)](#53-anova-analysis-of-variance)
  - [5.4. Correlation Statistics — Pearson Correlation](#54-correlation-statistics--pearson-correlation)
  - [5.5. Chi-Square Test (χ²)](#55-chi-square-test-χ²)
  - [5.6. Tổng kết: Chọn phương pháp theo loại biến](#56-tổng-kết-chọn-phương-pháp-theo-loại-biến)
  - [5.7. matplotlib & seaborn](#57-matplotlib--seaborn)
- [6. Model development](#6-model-development)
> 📝 **Ghi chú:** Bổ sung ví dụ thực tế / lưu ý riêng của bạn ở đây (optional).

---

## 1. Data I/O (Đọc & Xuất Dữ Liệu)

```python
import pandas as pd

url_var = 'url_path_or_file_path'
```

### 1.1. Thao tác với File CSV

**Đọc file CSV:**

```python
df = pd.read_csv(url_var, header=None)  # Thêm header=None nếu file chưa có tên cột
```

**Gán tên cột (Headers):**

```python
df.columns = ['col1', 'col2', 'col3']
```

**Xuất file CSV:**

```python
df.to_csv('output_path.csv', index=False)
```

### 1.2. Thao tác với các định dạng khác

| Định dạng | Đọc dữ liệu (Read) | Xuất dữ liệu (Write) |
|---|---|---|
| JSON | `pd.read_json('file.json')` | `df.to_json('file.json')` |
| Excel | `pd.read_excel('file.xlsx')` | `df.to_excel('file.xlsx')` |
| SQL | `pd.read_sql(query, connection)` | `df.to_sql('table_name', connection)` |

---

## 2. Exploratory Data Analysis (EDA)

### 2.1. Kiểm tra & Chuyển đổi Kiểu dữ liệu

**Kiểm tra kiểu dữ liệu các cột:**

```python
print(df.dtypes)
```

**Ép/Chuyển kiểu dữ liệu (Type Casting):**

```python
df['colname'] = df['colname'].astype('int')
```

### 2.2. Phân phối Dữ liệu & Thống kê Mô tả

**Tóm tắt thống kê mô tả:**

```python
df.describe(include='all')
```

**Ghi chú các chỉ số:**

- **Biến định lượng (Numerical):** `count`, `mean`, `std`, `min`, `max`, các phân vị `25%`, `50%`, `75%`.
- **Biến định tính (Categorical):** `unique` (số giá trị duy nhất), `top` (giá trị xuất hiện nhiều nhất), `freq` (tần suất xuất hiện).

**Xem tổng quan cấu trúc DataFrame:**

```python
df.info()  # Cung cấp số lượng hàng/cột, số lượng non-null và dung lượng bộ nhớ
```

### 2.3. Làm sạch dữ liệu ban đầu

**Thay thế ký tự đại diện cho giá trị khuyết (VD: đổi `?` thành `NaN`):**

```python
import numpy as np

df1 = df.replace('?', np.nan)
```

**Xóa dòng thiếu giá trị ở cột cụ thể:**

```python
df1.dropna(subset=["col1"], axis=0, inplace=True)
# axis=0: Xóa các hàng (rows) có giá trị NaN tại cột 'col1'
```

---

## 3. Working with Databases (Python DB-API)

### 3.1. Các đối tượng cốt lõi

- **Connection Object:** Quản lý kết nối và giao tiếp trực tiếp với cơ sở dữ liệu.
- **Cursor Object:** Thực thi các câu lệnh SQL và duyệt qua kết quả trả về.

### 3.2. Phương thức chính của Connection Object

| Phương thức | Chức năng |
|---|---|
| `.cursor()` | Tạo và trả về một đối tượng Cursor mới |
| `.commit()` | Xác nhận và lưu các thay đổi (transaction) vào CSDL |
| `.rollback()` | Hoàn tác các thay đổi nếu xảy ra lỗi trong quá trình thực thi |
| `.close()` | Đóng kết nối CSDL để giải phóng tài nguyên |

### 3.3. Quy trình thao tác mẫu

```python
# 1. Import thư viện kết nối tương ứng (VD: sqlite3, psycopg2, pymysql,...)
import sqlite3

# 2. Khởi tạo Connection Object
connection = sqlite3.connect('database_name.db')

# 3. Khởi tạo Cursor Object
cursor = connection.cursor()

# 4. Thực thi truy vấn SQL
query = 'SELECT * FROM mytable'
cursor.execute(query)

# 5. Lấy toàn bộ kết quả trả về
results = cursor.fetchall()

# 6. Giải phóng tài nguyên
cursor.close()
connection.close()
```

---

## 4. Data Wrangling & Pre-processing

### 4.1. Xử lý giá trị khuyết (Null) & Trùng lặp

**Xóa dữ liệu khuyết:**

```python
df.dropna(subset=['col_name'], axis=0, inplace=True)
# axis=0: Xóa hàng | axis=1: Xóa cột | inplace=True: Cập nhật trực tiếp trên DataFrame

df.reset_index(drop=True, inplace=True)  # Đặt lại chỉ số hàng (index)
```

**Điền / Thay thế giá trị thiếu (Imputation):**

```python
# Thay bằng Mean (Dành cho biến liên tục)
mean_val = df['col_name'].mean()
df['col_name'].replace(np.nan, mean_val, inplace=True)

# Thay bằng Mode / Yếu vị (Dành cho biến phân loại)
most_frequent = df['col_name'].value_counts().idxmax()
df['col_name'].replace(np.nan, most_frequent, inplace=True)
```

### 4.2. Chuẩn hóa Định dạng & Tên cột

**Đổi tên cột:**

```python
df.rename(columns={'old_name1': 'new_name1', 'old_name2': 'new_name2'}, inplace=True)
```

### 4.3. Chuẩn hóa Thang đo (Data Normalization)

Đưa các biến liên tục về cùng một quy mô giá trị phục vụ cho mô hình hóa:

```python
# 1. Simple Feature Scaling (Chia cho giá trị Max -> Đưa về khoảng [0, 1]):
df['income'] = df['income'] / df['income'].max()

# 2. Min-Max Scaling (Đưa chính xác về khoảng [0, 1]):
df['income'] = (df['income'] - df['income'].min()) / (df['income'].max() - df['income'].min())

# 3. Z-score Normalization (Đưa về Phân phối chuẩn Mean = 0, Std = 1):
df['income'] = (df['income'] - df['income'].mean()) / df['income'].std()
```

### 4.4. Gom nhóm dữ liệu số (Data Binning)

Chia tập giá trị liên tục thành các khoảng/nhóm (bins) rời rạc:

```python
import numpy as np
import pandas as pd

# Bước 1: Tạo các điểm ranh giới chia khoảng giá thành 3 nhóm bằng nhau
bins = np.linspace(min(df["price"]), max(df["price"]), 4)

# Bước 2: Đặt tên nhãn cho từng nhóm
group_names = ["Low", "Medium", "High"]

# Bước 3: Áp dụng pd.cut() để phân loại dữ liệu vào các nhóm
df["price-binned"] = pd.cut(df["price"], bins, labels=group_names, include_lowest=True)

# Kiểm tra số lượng phân bổ ở mỗi nhóm
print(df["price-binned"].value_counts())
```

### 4.5. Mã hóa biến phân loại (Categorical to Numeric)

Biến đổi các giá trị định tính thành dạng số (One-Hot Encoding / Dummy Variables):

```python
# 1. Tạo Dummy Variables từ cột dữ liệu định tính
dummy_df = pd.get_dummies(df['Fuel'], dtype=int)

# 2. Gộp Dummy Variables vào DataFrame chính và xóa cột gốc
df = pd.concat([df, dummy_df], axis=1)
df.drop('Fuel', axis=1, inplace=True)
```

## 5. Exploratory Data Analysis (EDA)
# 📊 Exploratory Data Analysis (EDA) trong Python

Tài liệu tổng hợp các kỹ thuật **EDA** cơ bản nhằm trả lời câu hỏi trọng tâm:

> ❓ **"Những đặc điểm (features) nào có ảnh hưởng lớn nhất đến giá xe (car price)?"**

---

## 📑 Mục lục

- [5. Exploratory Data Analysis (EDA)](#5-exploratory-data-analysis-eda)
  - [5.1. Descriptive Statistics (Thống kê mô tả)](#51-descriptive-statistics-thống-kê-mô-tả)
  - [5.2. Group By & Heatmap](#52-group-by--heatmap)
  - [5.3. ANOVA (Analysis of Variance)](#53-anova-analysis-of-variance)
  - [5.4. Correlation Statistics — Pearson Correlation](#54-correlation-statistics--pearson-correlation)
  - [5.5. Chi-Square Test (χ²)](#55-chi-square-test-χ²)
  - [5.6. Tổng kết: Chọn phương pháp theo loại biến](#56-tổng-kết-chọn-phương-pháp-theo-loại-biến)

> 💡 **Ghi chú:** Toàn bộ ví dụ dùng `pandas`, `seaborn`, `matplotlib`, `scipy.stats`.

---

## 5. Exploratory Data Analysis (EDA)
## Steps
- Exploring "Data types"
- Analyzing Individual Feature Patterns Using Visualization in Continuous numerical variables (regplot) & Category variables (box plot)
- Descriptive Statistical Analysis
### 5.1. Descriptive Statistics (Thống kê mô tả)

**Thống kê biến số (Numerical):**

```python
df.describe()
```

**Thống kê biến phân loại (Categorical):**

```python
df['drive-wheels'].value_counts().to_frame()
```

**Box Plot — so sánh phân phối giữa các nhóm**

Phù hợp khi so sánh một **biến numeric** theo từng nhóm của một **biến categorical**.

```python
import seaborn as sns

sns.boxplot(x="drive-wheels", y="price", data=df)
```

> 🔑 **Điểm chính:** Box plot cho thấy median, IQR (khoảng tứ phân vị), và outlier của từng nhóm → dễ nhận diện nhóm nào có giá trị cao/thấp bất thường.

**Scatter Plot — quan hệ giữa hai biến liên tục (continuous)**

- Mỗi điểm dữ liệu (observation) là một điểm trên đồ thị.
- Hai biến được thể hiện:
  - **Predictor / Independent variable** → trục **x**
  - **Target / Dependent variable** → trục **y**

```python
import matplotlib.pyplot as plt

plt.scatter(x=df["engine-size"], y=df["price"])
plt.title("Engine Size vs Price")
plt.xlabel("Engine Size")
plt.ylabel("Price")
plt.show()
```

---

### 5.2. Group By & Heatmap

**Câu hỏi ví dụ**

> ❓ "Có mối quan hệ nào giữa các loại **drive system** (hệ dẫn động) và **giá xe** không? Nếu có, loại nào làm tăng giá trị xe nhiều nhất?"

**`groupby()` — gom nhóm dữ liệu**

```python
df_group = df[['drive-wheels', 'body-style', 'price']].groupby(
    ['drive-wheels', 'body-style'], as_index=False
).mean()
```

**`pivot()` — chuyển bảng dài sang bảng ma trận**

```python
df_pivot = df_group.pivot(index='drive-wheels', columns='body-style')
# Lưu ý: index là hàng (row), columns là cột
```

**Heatmap — trực quan hóa biến mục tiêu theo nhiều biến**

```python
sns.heatmap(df_pivot, cmap="RdBu")
-------------------------------------
plt.pcolor(df_pivot, cmap="RdBu")  # Màu đỏ = giá trị thấp, màu xanh = giá trị cao
plt.colorbar()
-------------------------------------
plt.show()
```

> 🔑 **Điểm chính:** Heatmap giúp nhìn nhanh biến nào (tổ hợp categorical) có ảnh hưởng mạnh nhất tới biến target (price) mà không cần đọc từng con số.

---

### 5.3. ANOVA (Analysis of Variance)

**Mục đích:** Kiểm định xem có sự khác biệt **có ý nghĩa thống kê** về giá trị trung bình (mean) của biến numeric **giữa từ 3 nhóm categorical trở lên** hay không.

```python
from scipy import stats

df_anova = df[['make', 'price']]
grouped_anova = df_anova.groupby(['make'])

f_val, p_val = stats.f_oneway(
    grouped_anova.get_group('honda')['price'],
    grouped_anova.get_group('subaru')['price']
)
```

| Chỉ số | Ý nghĩa |
|---|---|
| **F-statistic (F-value)** | Tỷ lệ phương sai *giữa các nhóm* so với phương sai *trong từng nhóm*. F càng lớn → sự khác biệt giữa các nhóm càng rõ rệt |
| **p-value** | Xác suất để kết quả quan sát được là do ngẫu nhiên (xem cách đọc ở mục 5.4) |

> 🔑 **Điểm chính:** ANOVA phù hợp khi biến độc lập là **categorical (≥ 3 nhóm)** và biến phụ thuộc là **numeric**.

---

### 5.4. Correlation Statistics — Pearson Correlation

**Mục đích:** Đo **độ mạnh và chiều hướng** của mối quan hệ **tuyến tính** giữa hai biến liên tục (continuous).

```python
from scipy import stats

pearson_coef, p_value = stats.pearsonr(df['horsepower'], df['price'])
```

**Correlation Coefficient (Hệ số tương quan)**

| Giá trị | Ý nghĩa |
|---|---|
| ≈ **0** | Không có mối quan hệ tuyến tính |
| Tiến gần **+1** | Mối quan hệ **đồng biến** (dương) càng mạnh |
| Tiến gần **-1** | Mối quan hệ **nghịch biến** (âm) càng mạnh |

**p-value — độ tin cậy của kết quả**

| p-value | Mức độ tin cậy |
|---|---|
| `< 0.001` | 🟢 Độ tin cậy **rất mạnh** (Strong certainty) |
| `< 0.05` | 🟡 Độ tin cậy **trung bình** (Moderate certainty) |
| `< 0.1` | 🟠 Độ tin cậy **yếu** (Weak certainty) |
| `> 0.1` | 🔴 **Không** có độ tin cậy (No certainty) |

> 🔑 **Điểm chính:** Cần đọc **cả 2 chỉ số cùng lúc** — hệ số tương quan cho biết *độ mạnh/chiều* của quan hệ, còn p-value cho biết *kết quả đó có đáng tin không*. Hệ số cao nhưng p-value lớn (> 0.1) thì kết quả không có ý nghĩa thống kê.

**Correlation Heatmap (trực quan hóa nhiều biến cùng lúc)**

```python
import seaborn as sns

corr_matrix = df.corr(numeric_only=True)
sns.heatmap(corr_matrix, annot=False, cmap="RdBu", center=0)
```

> 🔑 **Điểm chính:** Dùng để quét nhanh toàn bộ biến numeric, tìm ra các cặp biến có tương quan mạnh với biến target (price) — bước quan trọng để chọn đặc trưng (feature selection) trước khi mô hình hóa.

---

### 5.5. Chi-Square Test (χ²)

**Mục đích:** Kiểm định mối quan hệ giữa **hai biến categorical** — trả lời câu hỏi "hai biến này có độc lập với nhau hay không?"

- Áp dụng khi cả 2 biến đều là **categorical** (ví dụ: `fuel-type` và `body-style`).
- Dựa trên **bảng tần suất chéo (contingency table)** so sánh giá trị **quan sát được (observed)** với giá trị **kỳ vọng (expected)** nếu hai biến độc lập.

```python
import pandas as pd
from scipy.stats import chi2_contingency

contingency_table = pd.crosstab(df['fuel-type'], df['body-style'])

chi2_stat, p_value, dof, expected = chi2_contingency(contingency_table)

print(f"Chi-square statistic: {chi2_stat}")
print(f"p-value: {p_value}")
print(f"Degrees of freedom: {dof}")
```

| Chỉ số | Ý nghĩa |
|---|---|
| **Chi-square statistic (χ²)** | Càng lớn → sự khác biệt giữa observed và expected càng lớn → hai biến càng có khả năng liên quan |
| **p-value** | `< 0.05` → bác bỏ giả thuyết "hai biến độc lập" → **có mối quan hệ** giữa 2 biến categorical (đọc theo bảng ở mục 5.4) |
| **Degrees of freedom (dof)** | `(số hàng - 1) × (số cột - 1)` trong contingency table |

> 🔑 **Điểm chính:** Pearson dùng cho **numeric vs numeric**, ANOVA dùng cho **categorical vs numeric**, còn **Chi-square dùng cho categorical vs categorical**.

---

### 5.6. Tổng kết: Chọn phương pháp theo loại biến

| Biến 1 | Biến 2 | Phương pháp phù hợp |
|---|---|---|
| Numeric | Numeric | Scatter plot + **Pearson Correlation** |
| Categorical | Numeric | Box plot + **ANOVA** |
| Categorical | Categorical | Bar/Count plot + **Chi-Square Test** |
| Nhiều biến Numeric | Numeric (target) | **Correlation Heatmap** |
| Nhiều biến Categorical | Numeric (target) | `groupby()` + `pivot()` + **Heatmap** |

> ✅ **Ghi nhớ nhanh:** EDA không chỉ để "xem dữ liệu trông như thế nào", mà là bước **kiểm định giả thuyết ban đầu** về mối quan hệ giữa các biến — làm nền tảng cho việc chọn đặc trưng (feature selection) trước khi xây dựng mô hình.

### 5.7. matplotlib & seaborn
### a. matplotlib

```python
from matplotlib import pyplot as plt
```

Ngoài ra, câu lệnh trên cũng có thể được viết như sau:

```python
import matplotlib.pyplot as plt
```

Lưu ý rằng hầu hết các biểu đồ mà chúng ta quan tâm trong thư viện này đều nằm trong thư mục con `pyplot` của package.

Các hàm trong `matplotlib` trả về một đối tượng plot (biểu đồ), và cần thêm câu lệnh khác để hiển thị nó. Khi sử dụng `matplotlib` trong Jupyter Notebook, chúng ta cần biểu đồ được hiển thị ngay bên trong giao diện notebook. Vì vậy, việc thêm câu lệnh "magic" sau đây ngay sau khi import thư viện là điều cần thiết:

```python
%matplotlib inline
```
### Các biểu đồ trong pyplot: line chart, scatter, histogram, bar, heatmap

```python
plt.plot(x,y)
plt.scatter(x,y)
plt.hist(x,bins)
plt.bar(x,height)
plt.pcolor(C , cmap = (optional))
```

### b. seaborn

`seaborn` thường được import trong code bằng câu lệnh sau:

```python
import seaborn as sns
```
### Các biểu đồ trong pyplot: Regression plot, Box and whisker plot, Residual Plot, KDE plot, Distribution Plot.

```python
sns.boxplot(x= df[''], y= df[''])
sns.kdeplot(X)
sns.regplot(x= df[''], y= df[''])
sns.residplot(x= df[''], y= df[''])
```
### c. Kernel Density Estimate (KDE) plot & Box plot

#### 1. `sns.kdeplot(X)`

- **Cách hoạt động:** Ước lượng và vẽ đường cong **phân phối mật độ xác suất (probability density)** của một biến liên tục, dựa trên phương pháp làm mượt (smoothing) dữ liệu thay vì chia thành các cột (bins) như histogram.
- **Mục tiêu:** Thể hiện hình dạng phân phối của dữ liệu (lệch trái/phải, đối xứng, đa đỉnh...) một cách trực quan và mượt mà hơn histogram.
- **Câu hỏi đặt ra:** *Dữ liệu tập trung nhiều nhất ở khoảng giá trị nào? Phân phối có đối xứng không, có bị lệch (skewed) hay có nhiều đỉnh (multimodal) không?*

#### 2. `sns.boxplot(x=df[''], y=df[''])`

- **Cách hoạt động:** Vẽ **hộp (box) thể hiện các phân vị** của dữ liệu (Q1, trung vị Q2, Q3) cùng với "râu" (whiskers) thể hiện khoảng giá trị bình thường, và các điểm nằm ngoài được đánh dấu là **outliers**.
- **Mục tiêu:** So sánh phân phối, độ phân tán và phát hiện giá trị ngoại lai của một biến số liên tục theo từng nhóm phân loại (categorical).
- **Câu hỏi đặt ra:** *Phân phối của biến số có khác nhau giữa các nhóm không? Nhóm nào có độ phân tán lớn hơn? Có xuất hiện outliers không?*

## So sánh Regression plot và Residual plot

### 1. Regression plot (`sns.regplot`)

- **Trục tung (y):** giá trị thực tế của biến phụ thuộc (VD: `price`), không phải phần dư.
- **Đường thẳng xanh:** đường hồi quy tuyến tính (best-fit line) mô tả xu hướng dữ liệu, kèm theo dải màu nhạt xung quanh thể hiện khoảng tin cậy (confidence interval).
- **Mục đích:** cho biết mối quan hệ **tổng thể** giữa biến độc lập và biến phụ thuộc có tuyến tính hay không, và độ mạnh/yếu của mối quan hệ đó.
- **Ví dụ:** với `engine-size` vs `price`, các điểm dữ liệu bám khá sát quanh đường thẳng, xu hướng đi lên rõ ràng → hai biến có mối quan hệ tuyến tính dương khá mạnh.

### 2. Residual plot (`sns.residplot`)

- **Trục tung (y):** không phải giá trị thực tế mà là **phần dư** (residual = giá trị thực tế − giá trị dự đoán bởi mô hình).
- **Không có đường hồi quy**, chỉ có đường tham chiếu ngang tại `y = 0`.
- **Mục đích:** dùng để kiểm tra độ phù hợp (validity) của mô hình tuyến tính sau khi đã fit — xem sai số có phân bố ngẫu nhiên hay không.
- **Ví dụ:** với `highway-mpg` vs `price`, các điểm residual tạo thành hình cong (không ngẫu nhiên) → cho thấy mô hình tuyến tính chưa phù hợp với dữ liệu.

### 3. Bảng tóm tắt sự khác biệt

| Tiêu chí | Regression plot | Residual plot |
|---|---|---|
| Trục y | Giá trị thực tế (VD: `price`) | Phần dư (residual) |
| Đường tham chiếu | Đường hồi quy + khoảng tin cậy (CI) | Đường ngang tại `y = 0` |
| Mục đích | Xem xu hướng / mối quan hệ giữa 2 biến | Kiểm tra mô hình có phù hợp hay không |
| Dấu hiệu tốt | Điểm bám sát đường thẳng → tương quan tuyến tính mạnh | Điểm phân tán ngẫu nhiên quanh 0 → mô hình phù hợp |
| Dấu hiệu xấu | Điểm phân tán rời rạc, không theo xu hướng | Điểm tạo hình dạng (cong, phễu...) → mô hình chưa phù hợp |

> **Ghi nhớ:** Regression plot dùng để khám phá mối quan hệ ban đầu (EDA), còn Residual plot dùng để đánh giá/chẩn đoán mô hình sau khi đã fit — hai loại biểu đồ này thường đi kèm nhau khi phân tích hồi quy tuyến tính.

# 🚗 Model Development — Dự đoán giá xe cũ bằng Python

Tài liệu tổng hợp quy trình xây dựng mô hình dự đoán, nhằm trả lời câu hỏi trọng tâm:

> ❓ **"Làm sao xác định một mức giá hợp lý (fair value) cho một chiếc xe cũ?"**

---

##6. Model development
- [1. Simple & Multiple Linear Regression](#1-simple--multiple-linear-regression)
  - [1.1. Simple Linear Regression (SLR)](#11-simple-linear-regression-slr)
  - [1.2. Multiple Linear Regression (MLR)](#12-multiple-linear-regression-mlr)
- [2. Model Evaluation bằng trực quan hóa (Distribution Plot)](#2-model-evaluation-bằng-trực-quan-hóa-distribution-plot)
- [3. Polynomial Regression và Pipelines](#3-polynomial-regression-và-pipelines)
  - [3.1. Polynomial Regression một biến (one dimension)](#31-polynomial-regression-một-biến-one-dimension)
  - [3.2. Polynomial Regression nhiều biến (multi-dimensional)](#32-polynomial-regression-nhiều-biến-multi-dimensional)
  - [3.3. Pre-processing — Chuẩn hóa dữ liệu (StandardScaler)](#33-pre-processing--chuẩn-hóa-dữ-liệu-standardscaler)
  - [3.4. Pipeline — Gộp các bước thành 1 quy trình](#34-pipeline--gộp-các-bước-thành-1-quy-trình)
- [4. R-squared và MSE — Đánh giá mô hình trên tập huấn luyện](#4-r-squared-và-mse--đánh-giá-mô-hình-trên-tập-huấn-luyện)
- [5. Prediction và Decision Making](#5-prediction-và-decision-making)

---

## 1. Simple & Multiple Linear Regression

### 1.1. Simple Linear Regression (SLR)

**Ý tưởng đơn giản:** Giả sử giá xe chỉ phụ thuộc vào **một** đặc điểm duy nhất, ví dụ mức tiêu hao nhiên liệu (`highway-mpg`). SLR sẽ cố gắng vẽ một **đường thẳng** khớp tốt nhất qua các điểm dữ liệu, để từ mức tiêu hao nhiên liệu ta suy ra được giá xe dự đoán.

**Công thức:**

```
ŷ = b0 + b1.x
```

| Ký hiệu | Ý nghĩa | Tương ứng trong code |
|---|---|---|
| `x` | Biến độc lập / predictor (VD: `highway-mpg`) | — |
| `ŷ` | Giá trị dự đoán (VD: giá xe dự đoán) | — |
| `b0` | Hệ số chặn (intercept) — giá trị `ŷ` khi `x = 0` | `lm.intercept_` |
| `b1` | Hệ số góc (slope/coefficient) — `x` tăng 1 đơn vị thì `ŷ` thay đổi bao nhiêu | `lm.coef_` |

**Các bước thực hiện trong Python:**

```python
# Bước 1: Import thư viện
from sklearn.linear_model import LinearRegression

# Bước 2: Tạo đối tượng Linear Regression
lm = LinearRegression()

# Bước 3: Xác định biến predictor (x) và biến target (y)
X = df[['highway-mpg']]
Y = df['price']

# Bước 4: Huấn luyện mô hình — tìm ra b0 và b1 tốt nhất khớp với dữ liệu
lm.fit(X, Y)

# Bước 5: Dự đoán
yhat = lm.predict(X)
```

> 💡 **Lưu ý output:** Kết quả `yhat` là một **mảng (array)** có **cùng số lượng phần tử** với số dòng đầu vào `X` — mỗi phần tử là 1 giá xe dự đoán tương ứng với 1 dòng dữ liệu đầu vào.

---

### 1.2. Multiple Linear Regression (MLR)

**Ý tưởng:** Thực tế giá xe không chỉ phụ thuộc vào 1 yếu tố — nó còn phụ thuộc vào mã lực (`horsepower`), trọng lượng (`curb-weight`)... MLR mở rộng SLR để dùng **nhiều biến predictor cùng lúc**.

**Công thức:**

```
ŷ = b0 + b1.x1 + b2.x2 + ... + bn.xn
```

**Các bước thực hiện (tương tự SLR, chỉ khác ở bước chọn biến):**

```python
# Trích nhiều cột predictor cùng lúc, lưu vào biến "a"
a = df[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']]

# Huấn luyện mô hình với nhiều biến predictor
lm.fit(a, df['price'])

# Dự đoán và lấy ra các hệ số b0, b1, ..., bn
yhat = lm.predict(a)
print(lm.intercept_)   # b0
print(lm.coef_)        # [b1, b2, ..., bn]
```

> 🔑 **Điểm chính:** SLR dùng 1 biến → dễ vẽ đồ thị 2D để trực quan hóa. MLR dùng nhiều biến → không vẽ trực tiếp được nữa, nên cần cách đánh giá khác — đó là lý do có mục 2 tiếp theo.

---

## 2. Model Evaluation bằng trực quan hóa (Distribution Plot)

**Vấn đề:** Sau khi huấn luyện mô hình, làm sao biết mô hình dự đoán **tốt hay tệ**? Một cách trực quan là so sánh **phân phối của giá trị thực tế** với **phân phối của giá trị dự đoán** — nếu 2 đường này gần trùng nhau, mô hình dự đoán khá tốt.

```python
import seaborn as sns

# Đường 1 (đỏ): phân phối giá trị THỰC TẾ
ax1 = sns.kdeplot(data=df['price'], color="r", label="Actual Value")

# Đường 2 (xanh): phân phối giá trị DỰ ĐOÁN, vẽ chồng lên cùng trục ax1
sns.kdeplot(data=Yhat, color="b", label="Fitted Values", ax=ax1)
```

**Cách đọc biểu đồ:**

| Trường hợp | Ý nghĩa |
|---|---|
| 2 đường gần như trùng khít | Mô hình dự đoán khá sát với thực tế |
| 2 đường lệch nhau nhiều | Mô hình chưa khớp tốt, cần cải thiện (thêm biến, đổi mô hình, v.v.) |

> 🔑 **Điểm chính:** Đây chỉ là đánh giá **định tính bằng mắt** (visual). Ở mục 4, chúng ta sẽ học cách đánh giá **định lượng bằng con số** (R-squared, MSE).

---

## 3. Polynomial Regression và Pipelines

**Vấn đề của Linear Regression:** Đường thẳng chỉ khớp tốt khi quan hệ giữa `x` và `y` là **tuyến tính**. Nhưng thực tế, nhiều mối quan hệ có dạng **cong (curvilinear)** — ví dụ giá xe không tăng đều đều theo mã lực mà tăng nhanh dần. Lúc này cần **Polynomial Regression** (hồi quy đa thức) để khớp đường **cong** thay vì đường thẳng.

### 3.1. Polynomial Regression một biến (one dimension)

**Công thức (đa thức bậc n với 1 biến x1):**

```
ŷ = b0 + b1.x1 + b2.(x1)² + b3.(x1)³ + ...
```

**Ví dụ: tính đa thức bậc 3**

```python
import numpy as np

# Tìm các hệ số của đa thức bậc 3 khớp tốt nhất với dữ liệu (x, y)
# theo phương pháp least squares.
# Kết quả trả về là mảng hệ số, sắp xếp từ bậc cao → bậc thấp
f = np.polyfit(x, y, 3)

# Chuyển mảng hệ số f thành một đối tượng hàm đa thức p,
# cho phép gọi p(giá_trị_x) để tính ŷ trực tiếp
p = np.poly1d(f)

print(p)
```

> ⚠️ Lưu ý chính tả hàm: là `np.poly1d` (số **1** + chữ **d**), không phải `np.polyld` (chữ L) — gõ nhầm sẽ báo lỗi.

### 3.2. Polynomial Regression nhiều biến (multi-dimensional)

**Công thức (đa thức bậc 2 với 2 biến x1, x2):**

```
ŷ = b0 + b1.x1 + b2.x2 + b3.x1.x2 + b4.(x1)² + b5.(x2)²
```

> 🔑 **Điểm chính:** `np.polyfit` / `np.poly1d` ở mục 3.1 **chỉ dùng cho 1 biến**. Khi có **nhiều biến predictor cùng lúc**, ta cần công cụ khác: `PolynomialFeatures` — nó tự động sinh ra tất cả số hạng bậc cao **và** số hạng tương tác (như `x1.x2` ở trên) mà `np.polyfit` không làm được.

```python
from sklearn.preprocessing import PolynomialFeatures

# Tạo đối tượng PolynomialFeatures:
#   degree=2            -> sinh ra các số hạng bậc 2 (bao gồm cả tương tác x1*x2)
#   include_bias=False  -> không tự thêm cột hằng số (bias/intercept) vào X,
#                          vì LinearRegression() sẽ tự tính intercept riêng
pr = PolynomialFeatures(degree=2, include_bias=False)

# fit_transform: sinh ra ma trận đặc trưng đa thức từ 2 cột gốc
x_polly = pr.fit_transform(df[['horsepower', 'curb-weight']])

# Sau đó đưa x_polly vào LinearRegression() như bình thường để huấn luyện
lm.fit(x_polly, df['price'])
```

---

### 3.3. Pre-processing — Chuẩn hóa dữ liệu (StandardScaler)

**Vấn đề:** `horsepower` có thể dao động 50–250, còn `highway-mpg` chỉ 15–50. Nếu để nguyên đơn vị khác nhau như vậy, mô hình có thể **hiểu nhầm** rằng feature có con số lớn hơn (`horsepower`) quan trọng hơn — dù thực tế chưa chắc vậy. `StandardScaler` giải quyết vấn đề này bằng cách đưa mọi feature về **cùng một thang đo chuẩn** (mean = 0, std = 1).

```python
from sklearn.preprocessing import StandardScaler

# Tạo đối tượng StandardScaler
SCALE = StandardScaler()

# fit(): học thông số chuẩn hóa (mean, std) từ dữ liệu huấn luyện,
# cho đồng thời nhiều cột feature ("simultaneously")
SCALE.fit(x_data[['horsepower', 'highway-mpg']])

# transform(): áp dụng công thức chuẩn hóa đã học ở bước fit()
# để biến đổi dữ liệu gốc thành dữ liệu đã scale
x_scale = SCALE.transform(x_data[['horsepower', 'highway-mpg']])
```

> 🔑 **Điểm chính:** Khác với cách chuẩn hóa thủ công bằng pandas (`(df['income'] - df['income'].mean()) / df['income'].std()`), `StandardScaler` của sklearn có 2 ưu điểm:
> 1. **Tách riêng `fit` và `transform`** — cho phép học tham số (mean, std) từ tập **train**, rồi dùng đúng tham số đó để scale tập **test**, tránh **data leakage** (rò rỉ thông tin từ tập test vào lúc huấn luyện).
> 2. **Xử lý nhiều cột cùng lúc**, tiện lợi khi chuẩn bị dữ liệu đầu vào cho `Pipeline` hoặc Polynomial Regression nhiều biến.

---

### 3.4. Pipeline — Gộp các bước thành 1 quy trình

**Ý tưởng (dây chuyền sản xuất):** Thay vì tự tay gọi `fit`/`transform` cho từng bước ở trên (chuẩn hóa → sinh đa thức → huấn luyện mô hình), `Pipeline` gộp cả 3 bước thành **một đối tượng duy nhất**, chỉ cần gọi `.fit()` và `.predict()` một lần — giống như đưa nguyên liệu vào đầu dây chuyền và nhận thành phẩm ở cuối:

```
        Chuẩn hóa           Sinh đặc trưng          Hồi quy
  X ──▶ (StandardScaler) ──▶ đa thức (Polynomial) ──▶ tuyến tính (LinearRegression) ──▶ ŷ
```

```python
# ------------------------------------------------------------
# Bước 1: Import các thư viện cần thiết
# ------------------------------------------------------------
from sklearn.preprocessing import StandardScaler
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline

# ------------------------------------------------------------
# Bước 2: Khởi tạo Pipeline
# ------------------------------------------------------------
# Input là 1 list các tuple ('tên bước', đối tượng xử lý),
# các bước được thực thi TUẦN TỰ đúng theo thứ tự khai báo
Input = [
    ('scale', StandardScaler()),
    ('polynomial', PolynomialFeatures(degree=2)),
    ('model', LinearRegression())
]

pipe = Pipeline(Input)

# ------------------------------------------------------------
# Bước 3: Huấn luyện toàn bộ pipeline chỉ với 1 dòng lệnh
# ------------------------------------------------------------
# Dữ liệu X tự động đi qua LẦN LƯỢT: chuẩn hóa -> sinh đa thức -> huấn luyện
pipe.fit(df[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']], y)

# ------------------------------------------------------------
# Bước 4: Dự đoán, cũng chỉ với 1 dòng lệnh
# ------------------------------------------------------------
# Dữ liệu mới cũng tự động đi qua đúng các bước xử lý đã học ở trên
yhat = pipe.predict(X[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']])
```

> ⚠️ **Lưu ý khi tự code:** Tên biến pipeline nên viết **nhất quán chữ hoa/thường** (luôn `pipe`, không lẫn `Pipe`) — Python phân biệt hoa thường nên viết sai sẽ báo lỗi `NameError`.

**Tóm tắt lợi ích của Pipeline:**

| Không dùng Pipeline | Dùng Pipeline |
|---|---|
| Gọi `fit`/`transform` thủ công từng bước | Chỉ gọi `.fit()` một lần |
| Dễ quên bước, dễ áp sai thứ tự cho tập test | Thứ tự cố định, áp dụng nhất quán cho train/test |
| Code dài, khó bảo trì khi có nhiều bước | Code ngắn gọn, dễ mở rộng |

---

## 4. R-squared và MSE — Đánh giá mô hình trên tập huấn luyện

> 📝 *Mục này sẽ trình bày cách đánh giá mô hình bằng con số cụ thể — R-squared (hệ số xác định) và MSE (sai số bình phương trung bình) — thay vì chỉ đánh giá bằng mắt như ở mục 2. Nội dung sẽ được bổ sung.*

## 5. Prediction và Decision Making

> 📝 *Mục này sẽ trình bày cách dùng mô hình đã huấn luyện để dự đoán giá xe thực tế và ra quyết định (VD: so sánh nhiều mô hình để chọn mô hình phù hợp nhất). Nội dung sẽ được bổ sung.*
