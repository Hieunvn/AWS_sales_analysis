## **I. MỤC TIÊU DỰ ÁN**

### 1. Tên dự án: "Phân tích doanh thu bán hàng của Amazon Web Services(AWS)"

### 2. Mục đích dự án:
- Biến động doanh thu theo thời gian, theo khu vực, theo sản phẩm, phân khúc khách hàng...
- Biến động lợi nhuận theo thời gian, theo khu vực, theo sản phẩm, phân khúc khách hàng…
- Phân tích mối tương quan giữa doanh thu và lợi nhuận -> doanh thu có liên hệ như thế nào đến lợi nhuận không?
- Phân tích mối tương quan giữa chiết khấu và lợi nhuận và doanh thu -> chiết khấu càng cao thì ảnh hưởng như thế nào đến lợi nhuận và doanh thu không?
- Phân tích mối tương quan giữa số lượng và doanh thu -> số lượng bán nhiều có tạo ra doanh thu tương ứng không?

### 3. Nguồn dữ liệu:
https://www.kaggle.com/code/gabrielenoaro/saas-company-aws-sales-exploratory-data-analys

### 4. Công cụ sử dụng:
- Làm sạch dữ liệu sử dụng Google Colab
- Phân tích dữ liệu sử dụng Power BI
- Tổng hợp dự án sử dụng GitHub
- Trình bày dự án sử dụng Power Point

### 5. Thời hạn : 
- Nộp bài: 15/8/2025
- Trình bày: 22/8/2025 thời gian 19h - 21h

## **II. CÁC BƯỚC THỰC HIỆN DỰ ÁN**

### 1. Upload dữ liệu, kiểm tra và làm sạch trên Colab
#### 1.1 Load file vào pandas
import pandas as pd
#### 1.2 Đổi tên và đọc file csv
df_sales=pd.read_csv('AWS-Sales.csv')
#### 1.3 Xem thông tin dữ liệu bảng
df_sales.head()
|index|Row ID|Order ID|Order Date|Date Key|Contact Name|Country|City|Region|Subregion|Customer|Customer ID|Industry|Segment|Product|License|Sales|Quantity|Discount|Profit|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|1|EMEA-2022-152156|11/9/2022|20221109|Nathan Bell|Ireland|Dublin|EMEA|UKIR|Chevron|1017|Energy|SMB|Marketing Suite|16GRM07R1K|261\.96|2|0\.0|41\.9136|
|1|2|EMEA-2022-152156|11/9/2022|20221109|Nathan Bell|Ireland|Dublin|EMEA|UKIR|Chevron|1017|Energy|SMB|FinanceHub|QLIW57KZUV|731\.94|3|0\.0|219\.582|
|2|3|AMER-2022-138688|6/13/2022|20220613|Deirdre Bailey|United States|New York City|AMER|NAMER|Phillips 66|1056|Energy|Strategic|FinanceHub|JI6BVL70HQ|14\.62|2|0\.0|6\.8714|
|3|4|EMEA-2021-108966|10/11/2021|20211011|Zoe Hodges|Germany|Stuttgart|EMEA|EU-WEST|Royal Dutch Shell|1031|Energy|SMB|ContactMatcher|DE9GJKGD44|957\.5775|5|0\.45|-383\.031|
|4|5|EMEA-2021-108966|10/11/2021|20211011|Zoe Hodges|Germany|Stuttgart|EMEA|EU-WEST|Royal Dutch Shell|1031|Energy|SMB|Marketing Suite - Gold|OIF7NY23WD|22\.368|2|0\.2|2\.5164|

#### 1.4 Kiểm tra thông tin dữ liệu cột đã định dạng đúng kiểu để phân tích.
df_sales.info()

<class 'pandas.core.frame.DataFrame'>

RangeIndex: 9994 entries, 0 to 9993

Data columns (total 19 columns):
#	Column	Count	Non-Null	Dtype
---	------	----	----------	-----
0	Row ID	9994	non-null	int64
1	Order ID	9994	non-null	object
2	Order Date	9994	non-null	object
3	Date Key	9994	non-null	int64
4	Contact Name	9994	non-null	object
5	Country	9994	non-null	object
6	City	9994	non-null	object
7	Region	9994	non-null	object
8	Subregion	9994	non-null	object
9	Customer	9994	non-null	object
10	Customer ID	9994	non-null	int64
11	Industry	9994	non-null	object
12	Segment	9994	non-null	object
13	Product	9994	non-null	object
14	License	9994	non-null	object
15	Sales	9994	non-null	float64
16	Quantity	9994	non-null	int64
17	Discount	9994	non-null	float64
18	Profit	9994	non-null	float64
<img width="302" height="421" alt="image" src="https://github.com/user-attachments/assets/77ed08c3-c112-47e8-843d-c0a7964a9b1d" />

 
dtypes: float64(3), int64(4), object(12)

memory usage: 1.4+ MB

### 2. Phân tích dữ liệu trên Power BI:



