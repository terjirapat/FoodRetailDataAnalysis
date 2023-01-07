# FoodRetailDataAnalysis
Question
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



