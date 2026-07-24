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
- [5. Exploratory Data Analysis (EDA)](#5-Exploratory-Data-Analysis-eda)


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
### 5.2. matplotlib & seaborn
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

