# FoodRetailDataAnalysis

## Cleaning

![image](https://user-images.githubusercontent.com/77285026/211143718-174cf270-058a-46c5-ac4a-dfee494d5ef7.png)

พบ column 'dollar_sales' มีค่าติดลบ และ column 'units' มีค่า outliner จึงทำการลบ data ส่วนนั้นออกโดย outliner ลบในส่วนที่เกิน mean+-3SD

ซึ่ง data หลัง clean แล้วเหลือทั้งหมด 5,184,441 row จาก 5,197,681 row คิดเป็น ~99.74%

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
![image](https://user-images.githubusercontent.com/77285026/211138914-3cb11080-0009-4a9e-a79b-7bcb823cd410.png)
![image](https://user-images.githubusercontent.com/77285026/211139146-ee1716f7-5b48-450c-ae83-42ad8025653a.png)

พบว่าช่วงเวลาที่ขายดีคือประมาณชั่วโมงที่ 10-17 ของวัน และสินค้าทุกชนิดขายดีในเวลานั้นเหมือนกันหมด

![image](https://user-images.githubusercontent.com/77285026/211139094-857e1124-7837-4cf4-bcd4-aaac84790c62.png)

และมองจาก average basket size คนจะใช้จ่ายต่อครั้งเยอะในประมาณชั่วโมงที่ 20-23 และ 0-3 ของวัน

แต่ยังสงสัยว่าชั่วโมงที่ average basket size สูงคนซื้อสินค้าต่อครั้งเยอะหรือซื้อของราคาแพงขึ้น จึงหาค่าเฉลี่ยจำนวนสินค้าต่อการซื้อหนึ่งครั้ง

![image](https://user-images.githubusercontent.com/77285026/211143079-4c26af3f-dead-4539-865d-96aa5dd555e6.png)

พบว่าช่วงที่ average basket size สูงคนจะซื้อสินค้าต่อครั้งเยอะขึ้น

## Question 4: 
![image](https://user-images.githubusercontent.com/77285026/211144038-7ac36b11-8ca7-47d3-acab-fd990c18efcd.png)

## Question 5: 
![image](https://user-images.githubusercontent.com/77285026/211144071-de87a45e-a05a-44fe-8a91-45011be60aaf.png)
![image](https://user-images.githubusercontent.com/77285026/211144080-9d15e1c1-b879-423e-90db-03d1f2d865e9.png)

product ที่ยอดขายสูงสุดสิบอันดับส่วนใหญ่เป็น pasta รองลงมา pasta sauce

![image](https://user-images.githubusercontent.com/77285026/211144133-dd6caa87-1a28-4da4-b81b-ea40d3ff8be2.png)

แต่ถ้าดูรวมๆ pasta sauce สร้างยอดขายได้เยอะสุด

![image](https://user-images.githubusercontent.com/77285026/211144149-1b5567d1-c4b7-49c6-9a9d-d8dfcfd1289e.png)

brand ที่ได้ยอดขายสูงสุดเป็น private label

![image](https://user-images.githubusercontent.com/77285026/211144166-9366a1f4-f045-4d45-9429-def98c01aaa6.png)

private label ขาย pasta ได้เยอะแต่ pasta sauce น้อย ส่วน pancake และ syrups ก็ไม่เป็นที่ 1

![image](https://user-images.githubusercontent.com/77285026/211144288-30f1524f-cb40-40b3-9bd2-4f9999edd148.png)

ถ้าดูสินค้าที่ยอดขายสูงสุดของแต่ละ category 5 อันดับ private label ชนะขาดแค่ pasta ส่วนอันอื่นโดนแซงหมดแทบไม่ติดอันดับเลย

![image](https://user-images.githubusercontent.com/77285026/211144296-f7b47d1f-f9d3-4343-a0e2-1213f6c2fc15.png)

assumption ว่ามีสินค้าน้อยไปไม่หลากหลายพอมา filter ดูสินค้ามีเยอะแต่ไม่เป็นที่นิยม อาจจะไม่อร่อยหรือคนไม่เคยลอง


