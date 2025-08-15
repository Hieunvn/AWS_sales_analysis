## **I. MỤC TIÊU DỰ ÁN**

### 1. Tên dự án: "Phân tích doanh thu bán hàng của Amazon Web Services(AWS)"

### 2. Mục đích dự án:
- Phân tích doanh số theo thời gian, theo khu vực, theo sản phẩm, phân khúc khách hàng...
- Phân tích lợi nhuận theo thời gian, theo khu vực, theo sản phẩm, phân khúc khách hàng…
- Phân tích mối tương quan giữa doanh số và lợi nhuận -> doanh thu có liên hệ như thế nào đến lợi nhuận không?
- Phân tích mối tương quan giữa chiết khấu và lợi nhuận và doanh số -> chiết khấu càng cao thì ảnh hưởng như thế nào đến lợi nhuận và doanh số không?
- Phân tích mối tương quan giữa số lượng và doanh số -> số lượng bán nhiều có tạo ra doanh số tương ứng không?

### 3. Nguồn dữ liệu:
https://www.kaggle.com/code/gabrielenoaro/saas-company-aws-sales-exploratory-data-analys

<img width="454" height="711" alt="image" src="https://github.com/user-attachments/assets/5679d005-19de-4b87-a3a5-b832bd924c70" />




<img width="177" height="221" alt="image" src="https://github.com/user-attachments/assets/8473ffee-ab7a-4399-acb4-b8c14578d637" />

<img width="220" height="301" alt="image" src="https://github.com/user-attachments/assets/775ede84-6de0-40a3-a5b2-3f0e22b5d925" />

<img width="122" height="81" alt="image" src="https://github.com/user-attachments/assets/69e005cc-61b5-4aa1-9b90-c8973af39173" />

<img width="205" height="261" alt="image" src="https://github.com/user-attachments/assets/20356726-68a1-4bbc-80c5-7361f63503ae" />

### 4. Công cụ sử dụng:
- Làm sạch dữ liệu sử dụng Google Colab
- Phân tích dữ liệu sử dụng Power BI
- Tổng hợp dự án sử dụng GitHub
- Trình bày dự án sử dụng Power Point

### 5. Thời hạn : 
- Nộp bài: 15/8/2025
- Trình bày: 22/8/2025 thời gian 19h - 21h

## **II. CÁC BƯỚC THỰC HIỆN DỰ ÁN**

### 1. Upload dữ liệu và làm sạch trên Colab
#### 1.1 Upload dữ liệu
##### 1.1.1 Load file vào pandas
`import pandas as pd`
##### 1.1.2 Đổi tên và đọc file csv
`df_sales=pd.read_csv('AWS-Sales.csv')`
##### 1.1.3 Kiểm tra thông tin dữ liệu bảng
`df_sales.head()`

|index|Row ID|Order ID|Order Date|Date Key|Contact Name|Country|City|Region|Subregion|Customer|Customer ID|Industry|Segment|Product|License|Sales|Quantity|Discount|Profit|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|1|EMEA-2022-152156|11/9/2022|20221109|Nathan Bell|Ireland|Dublin|EMEA|UKIR|Chevron|1017|Energy|SMB|Marketing Suite|16GRM07R1K|261\.96|2|0\.0|41\.9136|
|1|2|EMEA-2022-152156|11/9/2022|20221109|Nathan Bell|Ireland|Dublin|EMEA|UKIR|Chevron|1017|Energy|SMB|FinanceHub|QLIW57KZUV|731\.94|3|0\.0|219\.582|
|2|3|AMER-2022-138688|6/13/2022|20220613|Deirdre Bailey|United States|New York City|AMER|NAMER|Phillips 66|1056|Energy|Strategic|FinanceHub|JI6BVL70HQ|14\.62|2|0\.0|6\.8714|
|3|4|EMEA-2021-108966|10/11/2021|20211011|Zoe Hodges|Germany|Stuttgart|EMEA|EU-WEST|Royal Dutch Shell|1031|Energy|SMB|ContactMatcher|DE9GJKGD44|957\.5775|5|0\.45|-383\.031|
|4|5|EMEA-2021-108966|10/11/2021|20211011|Zoe Hodges|Germany|Stuttgart|EMEA|EU-WEST|Royal Dutch Shell|1031|Energy|SMB|Marketing Suite - Gold|OIF7NY23WD|22\.368|2|0\.2|2\.5164|

#### 1.2 Làm sạch dữ liệu
Ghi chú: Bỏ qua outliers mục đích để thấy insights khi phân tích.
##### 1.2.1 Chuyển đổi cột "Order Date" thành kiểu datetime
`df_sales['Order Date'] = pd.to_datetime(df_sales['Order Date'], errors='coerce')`
##### 1.2.2 Chuyển đổi các cột dạng phân loại (categorical)
`categorical_cols = ['Country', 'City', 'Region', 'Subregion', 'Customer',
                    'Industry', 'Segment', 'Product', 'License', 'Order ID']
for col in categorical_cols:
    df_sales[col] = df_sales[col].astype('category')`
##### 1.2.3 Chuyển đổi các cột dạng 2 số thập phân
`df_sales['Sales'] = df_sales['Sales'].round(2)
df_sales['Discount'] = df_sales['Discount'].round(2)
df_sales['Profit'] = df_sales['Profit'].round(2)`
##### 1.2.4 Loại bỏ các cột không cần thiết
`df_sales = df_sales.drop(columns=['Row ID', 'Contact Name', 'License', 'Date Key'])`
##### 1.2.5 Tách cột "Order Date" theo từng tháng, từng năm
`df_sales['Month'] = df_sales['Order Date'].dt.month
df_sales['Year'] = df_sales['Order Date'].dt.year`
##### 1.2.6 Kiểm tra dữ liệu bị thiếu
`missing_data = df_sales.isnull().sum()
print(missing_data[missing_data > 0])`

Series([], dtype: int64)
##### 1.2.7 Kiểm tra lại dữ liệu sau khi làm sạch
`df_sales.info()`

<img width="565" height="414" alt="image" src="https://github.com/user-attachments/assets/924def1d-6714-43f2-8afc-33db5128102d" />

##### 1.2.8 Kết xuất dữ liệu ra file CSV và upload trên Power BI
`df_sales.to_csv("Cleaned_AWS_Sales.csv", index=False)`

### 2. Phân tích dữ liệu trên Power BI tìm insights
#### 2.1 Phân tích doanh số (sales)

<img width="876" height="493" alt="image" src="https://github.com/user-attachments/assets/daee8680-87e4-4d5e-a395-870a4c68b92f" />

- Doanh số tăng trưởng qua các năm
- Doanh số nhiều nhất ở khu vực EMEA ( khu vực Châu Âu-Trung Đông-Châu Phi) đạt 299.07K chiếm 43,25%, tiếp đến khu vực AMER ( Châu Mỹ) đạt 268.69K chiếm 38.05% và cuối cùng là khu vực APJ ( Châu Á-Thái Bình Dương và Nhật Bản) đạt 138.39K chiếm 19.6%
- Doanh số tập trung phần lớn phân khúc khách hàng SMB ( DN vừa và nhỏ), tiếp đến là khách hàng Stategic ( Khách hàng chiến lược) và cuối cùng là khách hàng Enterprise ( DN lớn).
- Sản phẩm đạt doanh số cao: Alchemy, Big OI Database.
- Nhóm ngành mang doanh số cao: Customer Products, Healthcare, Finance, Energy và Manufacturing.
   
#### 2.2 Phân tích lợi nhuận (profit)

<img width="874" height="489" alt="image" src="https://github.com/user-attachments/assets/f9f877bb-a9e4-4ef5-92a0-74fa083cb488" />

- Lợi nhuận tăng trưởng qua các năm
- Lợi nhuận mang lại nhiều nhất nằm ở khu vực EMEA đạt 147.46K chiếm 51.49%, tiếp đến khu vực AMER đạt 127.43K chiếm 44.49% và cuối cùng là khu vực APJ đạt 11.51K chiếm 4.02% nhưng khi phân tích chi tiết subregion ** có 2 nước lợi nhuận âm **. Đó là **Japan và ANZ** ( gồm Úc và New Zealand)
- Lợi nhuận tập trung tập khách hàng SMB, tiếp đến là khách hàng Stategic và cuối cùng là khách hàng Enterprise.
- Sản phẩm mang lại lợi nhuận cao: Alchemy. Sản phẩm **Big OI Database** thuộc top 2 về doanh số nhưng mang lại **lợi nhuận âm**.
- Nhóm ngành mang lợi nhuận cao: Customer Products, Finance, Energy, Manyfacring và Tech 

#### 2.3 Phân tích mối tương quan giữa doanh số và lợi nhuận



#### 2.4 Phân tích mối tương quan giữa chiết khấu và lợi nhuận và doanh số


### 3. Kết luận
- 



