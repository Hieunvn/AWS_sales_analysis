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

<img width="1740" height="261" alt="image" src="https://github.com/user-attachments/assets/33b7b6ea-10af-4de1-9072-f8c3cfc393d6" />

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

<img width="860" height="483" alt="image" src="https://github.com/user-attachments/assets/de25fb4a-d05f-4de5-bd9c-85f324f05533" />

- Doanh số tăng trưởng qua các năm
- Doanh số nhiều nhất ở khu vực EMEA ghi nhận $1.04M chiếm 45.44%, tiếp đến khu vực AMER ghi nhận $0.84M chiếm 36.47% và cuối cùng là khu vực APJ ghi nhận $0.42M chiếm 18.09%
- Doanh số tập trung phần lớn khách hàng SMB ( DN vừa và nhỏ) ghi nhận 1.16M chiếm 50.56%, tiếp đến là khách hàng Stategic ( Khách hàng chiến lược) ghi nhận 0.71M chiếm 30.74% và cuối cùng là khách hàng Enterprise ( DN lớn) ghi nhận 0.43M chiếm 18.7%. Riêng Ấn Độ (IND) khách hàng Stategic chiếm tỷ trọng lớn như hình dưới.  
- Top 5 sản phẩm đạt doanh số cao: ContractMatcher, FinanceHub, Site Analytics, Marketing suite-Gold, Big OI Database. Nhưng xét theo khu vực AMER sản phẩm Data Smasher lọt vào top 5 thay thế bởi sản phẩm Big OI Database, khu vực APJ sản phẩm Alchemy lọt top 5 thay thế sản phẩm sản phẩm Big OI Database, khu vực EMEA sản phẩm Big OI Database lọt vào top 5.
   
#### 2.2 Phân tích lợi nhuận (profit)

<img width="860" height="486" alt="image" src="https://github.com/user-attachments/assets/5c09d148-fb64-4fe2-a190-3b05b0e35a2c" />

- Lợi nhuận tăng trưởng qua các năm
- Lợi nhuận phân tích theo region khu vực EMEA ghi nhận cao nhất $147.46K chiếm 51.49%, tiếp đến khu vực AMER ghi nhận $127.43K chiếm 44.49% và cuối cùng là khu vực APJ ghi nhận $11.51K chiếm 4.02% nhưng khi phân tích lợi nhuận theo subregion ** có 2 nước lợi nhuận âm ** đó là **Japan và ANZ** .Điều này cho thấy ngoài doanh thu, lợi nhuận còn phụ thuộc vào chi phí theo từng khu vực.
- Lợi nhuận phân tích theo khách hàng SMB ghi nhận cao nhất $134.12K chiếm 46.83%, tiếp đến là khách hàng Stategic ghi nhận $91.98K chiếm 32.12% và cuối cùng là khách hàng Enterprise ghi nhận $60.3K chiếm 21.05%.
- Top 5 sản phẩm mang lại lợi nhuận cao: Alchemy, Site Analytics, Data Smasher, Support, FinanceHub. So với top 5 sản phẩm đạt doanh thu cao có sự thay đổi. Điều này cho thấy không phải sản phẩm doanh thu cao thì lợi nhuận cao

#### 2.3 Phân tích mối tương quan giữa doanh số và lợi nhuận

<img width="859" height="485" alt="image" src="https://github.com/user-attachments/assets/9750b953-09e9-4e39-94ff-98b1017aa997" />

- Phần lớn giao dịch có doanh số thấp < $2K nhưng lợi nhuận dao động từ âm đến dương. Chủ yếu sản phẩm **ContactMatcher**
- Có một số đơn hàng doanh số cao nhưng lợi nhuận âm → khả năng do chi phí hoặc chiết khấu cao làm lỗ. Chủ yếu sản phẩm **Big OI Database**
- Mối quan hệ không hoàn toàn tuyến tính → Doanh số cao chưa chắc Lợi nhuận cao.

#### 2.4 Phân tích mối tương quan giữa chiết khấu và lợi nhuận.

<img width="860" height="485" alt="image" src="https://github.com/user-attachments/assets/1dbcaaa8-3563-478a-99b8-3ad58a88d4d0" />

- Khi chiết khấu tăng, Lợi nhuận thường giảm, nhiều điểm lợi nhuận âm ở mức chiết khấu cao (>0.4). Chủ yếu sản phẩm **Big OI Database, ContactMarcher, Marketing Suite**
- Điều này phù hợp logic kinh doanh: giảm giá sâu làm biên lợi nhuận giảm hoặc lỗ.

#### 2.5 Phân tích mối tương giữa chiết khấu và doanh số.

<img width="857" height="481" alt="image" src="https://github.com/user-attachments/assets/55bec342-2c9c-4951-84c0-0370f127a1f5" />

- Một số điểm cho thấy chiết khấu cao vẫn có doanh số cao → khả năng chương trình khuyến mãi kích thích mua hàng. Sản phẩm **Big OI Database, AIchemy, Site Analysis**
- Tuy nhiên nhiều mức chiết khấu cao không kéo theo doanh số cao → giảm giá không hiệu quả và chỉ làm giảm lợi nhuận như sản phẩm: **ContactMatcher, Marketing Suite**
  
 #### 2.6 Phân tích mối tương quan giữa số lượng và doanh số:

 <img width="860" height="481" alt="image" src="https://github.com/user-attachments/assets/057bdf9b-0cd4-4cc6-88f3-c95b25000885" />
 
- Một số đơn số lượng tăng nhưng doanh số không tăng như sản phẩm -> Điều này cho thấy giá bán thấp hoặc bán giá rẻ như sản phẩm **support**
- Một số đơn số lượng nhỏ nhưng doanh số cao → Đây có thể là sản phẩm giá trị lớn như sản phẩm: **Big OI Database, Alchemy
**
### 3. Kết luận

<img width="861" height="485" alt="image" src="https://github.com/user-attachments/assets/cde491c2-dfa4-43b6-be6c-445fa9c90b86" />

- Doanh thu cao không đồng nghĩa có lợi nhuận tốt.
Cụ thể:
- Sản phẩm Big OI Database, ContactMarcher, Marketing Suite-Gold thuộc top 5 doanh thu cao nhất nhưng lợi nhuận thấp hơn nhiều so với sản phẩm không thuộc top như AIchemy, Data Smasher, Support
Nguyên nhân:
+ Chiết khấu cao nhưng không mang lại lợi nhuận cao như sản phẩm Big OI Database, ContactMatcher. Có sản phẩm còn mang lợi nhuận âm Marketing Suite.
+ Chi phí 
- Những sản phẩm có lợi nhuận tốt như AIchemy, 


