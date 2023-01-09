# FoodRetailDataAnalysis

## Cleaning

พบ column 'dollar_sales' มีค่าติดลบ และ column 'units' มีค่า outliner จึงทำการลบ data ส่วนนั้นออกโดย outliner ลบในส่วนที่เกิน mean+-3SD

ซึ่ง data หลัง clean แล้วเหลือทั้งหมด 5,184,441 row จาก 5,197,681 row คิดเป็น ~99.74%

ก่อน

![image](https://user-images.githubusercontent.com/77285026/211143718-174cf270-058a-46c5-ac4a-dfee494d5ef7.png)

หลัง

![image](https://user-images.githubusercontent.com/77285026/211143998-786daa28-2df2-4f50-9522-3664f0141fcc.png)


```python
def filter_outliner(df, column_name):
    q_low = df[column_name].quantile(0.003) # mean-3sd
    q_hi  = df[column_name].quantile(0.997) # mean+3sd
    df_filtered = df[(df[column_name] <= q_hi) & (df[column_name] >= q_low)]
    return df_filtered
    
df =  filter_outliner(df, 'units')
# before removed 5,197,681 row
# after removed 5,184,441 row
```


## Question
1. ต้องการจัด segment ของลูกค้า ?
2. สินค้ากลุ่มไหนที่คนมักจะซื้อด้วยกัน ?
3. เวลาไหนที่ขายดีที่สุด ?
4. ร้านไหนที่ลูกค้าเยอะ และใช้จ่ายต่อครั้งเยอะที่สุด ?
5. จะเพิ่มกำไรอย่างไรดี ?

## Question 1: ต้องการจัด segment ของลูกค้า ?
### RFM Analysis
นำข้อมูลลูกค้าแต่ละคนมาวิเคราะห์ เพื่อแบ่ง Segmentation ของลูกค้าออกเป็นกลุ่มๆ ผ่าน 3 criteria หลักๆ คือ
- R – Recency ลูกค้าซื้อล่าสุดเมื่อไหร่
- F – Frequency ความถี่ในการซื้อ 
- M – Monetary ปริมาณการซื้อ หรือ Basket Size

โดยจัดกลุ่มลูกค้าจาก RFM Score ดังนี้
- rfm score >4.5 : Top Customer
- 4.5 > rfm score > 4 : High Value Customer
- 4>rfm score >3 : Medium value customer
- 3>rfm score>1.6 : Low-value customer
- rfm score<1.6 : Lost Customer

![image](https://user-images.githubusercontent.com/77285026/211134775-bc68c51f-4b35-4b28-819f-d5b33fb08f5b.png)

![image](https://user-images.githubusercontent.com/77285026/211135103-756ed5f6-0cb3-4036-9036-9ba6284ffb5b.png)

โดย segment ของลูกค้าที่ได้มาสามารถนำไปเสนอ promotion สินค้าที่ต่างกันเพื่อดึงดูดให้ลูกค้ากลับมาใช้บริการหรือใช้บริการมากขึ้นได้ เช่น ให้สิทธิพิเศษแก่ Top Customer, ส่งโปรโมชั่นให้ Lost Customer เพื่อดึงกลับมาใช้จ่าย

## Question 2: สินค้ากลุ่มไหนที่คนมักจะซื้อด้วยกัน ?
### Basket Analysis
เป็นการใช้ Association Rule เพื่อหาความสัมพันธ์ระหว่างสินค้ากับสินค้าด้วยกัน เพื่อรู้ว่าคนซื้อสินค้าชิ้นนี้คู่กับอะไร

เนื่องจาก Data ที่ที่ใช้ไม่มีความสัมพันธ์ของสินค้า จึงทำในมุมของประเภทสินค้าแทน

![image](https://user-images.githubusercontent.com/77285026/211135195-90e4a344-1f55-4e9f-b979-c72590526236.png)

พบสองความสัมพันธ์ได้แก่ (syrups, pancake mixes) และ (pasta sauce, pasta) ที่คนมันซื้อคู่กันโดยดูได้จากค่า lift ที่มากกว่า 1 และ support ที่บอกว่าในการซื้อสินค้าทั้งหมดมีโอกาสเท่าไรที่คนจะซื้อสิ่งนี้ ซึ่งมีค่า 10%, 81% ตามลำดับ

ซึ่งผลที่ได้มาสามารถนำไปจัดโปรโมชั่นคู่กันเพื่อเพิ่มยอดขายได้

## Question 3: เวลาไหนที่ขายดีที่สุด ?
![image](https://user-images.githubusercontent.com/77285026/211334774-e5e829db-b7be-403b-ad12-55927ff595ee.png)

พบว่าช่วงเวลาที่ขายดีคือประมาณชั่วโมงที่ 15-18 ของวัน และสินค้าทุกชนิดขายดีในเวลานั้นเหมือนกันหมด

![image](https://user-images.githubusercontent.com/77285026/211139094-857e1124-7837-4cf4-bcd4-aaac84790c62.png)

และมองจาก average basket size คนจะใช้จ่ายต่อครั้งเยอะในประมาณชั่วโมงที่ 20-23 และ 0-3 ของวัน

แต่ยังสงสัยว่าชั่วโมงที่ average basket size สูง เกิดจากคนซื้อสินค้าต่อครั้งเยอะหรือซื้อสินค้าต่อชิ้นในราคาแพงขึ้น จึงหาค่าเฉลี่ยจำนวนสินค้าต่อการซื้อหนึ่งครั้ง และค่าเฉลี่ยนราคาสินค้าต่อชิ้น

![image](https://user-images.githubusercontent.com/77285026/211338343-5946cfb6-5984-4c40-ab6d-dd2393cc30e9.png)

พบว่าช่วงที่ average basket size เกิดจากคนซื้อสินค้าต่อครั้งเยอะหรือซื้อสินค้าต่อชิ้นในราคาแพงขึ้น

## Question 4: ร้านไหนที่ลูกค้าเยอะ และใช้จ่ายต่อครั้งเยอะที่สุด ?

![image](https://user-images.githubusercontent.com/77285026/211144038-7ac36b11-8ca7-47d3-acab-fd990c18efcd.png)

![image](https://user-images.githubusercontent.com/77285026/211350859-4987d1e9-b96b-48b8-a114-ec519ceccecf.png)

![image](https://user-images.githubusercontent.com/77285026/211352555-41155353-1420-4d27-8342-474dda10c585.png)

## Question 5: จะเพิ่มกำไรอย่างไรดี ?
โดยขั้นแรกจะมาทำการ EDA กันก่อนในส่วนเพื่อหาว่าจะเพิ่มกำไรในส่วนไหนได้บ้างเริ่มจากส่วน product

![image](https://user-images.githubusercontent.com/77285026/211304706-5bbb8f6b-0ac5-44a9-ba44-4bfd413afcbf.png)

product ที่ยอดขายสูงสุดสิบอันดับส่วนใหญ่เป็น pasta รองลงมา pasta sauce

![image](https://user-images.githubusercontent.com/77285026/211305456-1088be7b-2cd6-4ac2-a099-7c5f7a43d6d3.png)

แต่ถ้าดูยอดขายแยกตาม commodity แล้ว pasta sauce สร้างยอดขายได้เยอะที่สุด

![image](https://user-images.githubusercontent.com/77285026/211305516-4df2af4f-5d28-41bc-a5f5-4a584d76555e.png)

brand ที่ได้ยอดขายสูงสุดเป็น private label

assumption : private label น่าจะได้กำไรเยอะที่สุด เพราะ ผลิตเอง ซึ่งถ้าสามารถเพิ่มยอดขาย brand private label ได้น่าจะทำให้กำไรเพิ่มขึ้น

ทีนี้มาดูว่าจะเพิ่มยอดขาย brand นี้ได้ยังไงบ้างเริ่มจากสำรวจต่อไป

![image](https://user-images.githubusercontent.com/77285026/211330296-72b17c94-72be-4654-a129-545a2752c44b.png)

private label ขาย pasta ได้เยอะแต่ pasta sauce, pancake และ syrups ยังมียอดขายน้อยกว่า brand อื่น

![image](https://user-images.githubusercontent.com/77285026/211330673-7cc3d06e-8681-4dd7-95a1-9ac00c839eb8.png)

ถ้าดูสินค้าที่ยอดขายสูงสุดของแต่ละ category 5 อันดับ private label มีสินค้า pasta ขายดีมาก ส่วน pasta sauce, pancake และ syrups มีสินค้าที่ขายดีแค่ชิ้นสองชิ้น

assumption : สินค้าของ private label อาจมีน้อยเกินไปหรือไม่ ทำให้ยอดขายใน pasta sauce, pancake และ syrups มีน้อย

![image](https://user-images.githubusercontent.com/77285026/211331193-21cf75a9-97f2-4748-8692-d272a02ba9bc.png)

เมื่อลองดูแล้วมีสินค้าเยอะแต่แค่ขายไม่ดี

solution : อาจจับคู่ชนิดสินค้าที่คนมักจะซื้อด้วยกันทำเป็นโปรโมชั่นลดราคา และวางขายใกล้ๆกัน ซึ่งอ้างอิงจาก basket analysis ที่พบสองความสัมพันธ์ของ (syrups, pancake mixes) และ (pasta sauce, pasta) จึงควรจับคู่สองกลุ่มนี้ เพื่อเพิ่มยอดขายแบรน private label ในสินค้าชนิด pasta sauce, pancake และ syrups



