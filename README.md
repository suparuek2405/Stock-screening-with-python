# Stock screening using financial ratios
## Introduction
กลุ่มของเราต้องการศึกษาเกี่ยวกับกลยุทธ์การลงทุนว่าการลงทุนด้วยการกรองค่าสัดส่วนทางการเงินวิธีไหนจะสร้างผลตอบแทนมากที่สุด โดยจะใช้วิธีที่เป็นที่นิยม 3 วิธีคือ การลงทุนแบบเน้นคุณค่า (Value Investing), การลงทุนแบบเติบโต (Growth Investing) และการลงทุนแบบเน้นเงินปันผล (Dividend Income Strategy) โดยในรายงานนี้เราจะใช้ข้อมูลงบการเงินจากปี 2020 ถึง 2022 ในการคัดกรองหุ้น แล้วนำมาคำนวณว่าหากเราซื้อหุ้นตามกลยุทธ์ โดยมีเงินทุน 1 ล้านบาท เราจะได้กำไรหรือขาดทุนเท่าไร โดยจำลองเริ่มซื้อหุ้นตั้งแต่เดือนมีนาคม 2023 (ซึ่งเป็นช่วงที่ประกาศงบปี 2022) จนถึงกันยายนปี 2024 คิดเป็นระยะเวลาลงทุนทั้งหมด 1 ปี 6 เดือน

## Data Source
ข้อมูลที่ใช้นำมาจาก [yfinance](https://github.com/ranaroussi/yfinance/tree/main/yfinance) ซึ่งเป็นไลบรารีใน Python ที่ใช้ดึงข้อมูลทางการเงินจาก Yahoo Finance เช่น ราคาหุ้น ข้อมูลงบการเงินของบริษัทเพื่อช่วยในการวิเคราะห์และทำงานเกี่ยวกับการลงทุน

## Let's get started
### Strategy 1: การลงทุนแบบเน้นคุณค่า (Value Investing) 
การลงทุนแบบเน้นมูลค่า (Value Investing) มุ่งเน้นการเลือกหุ้นที่มีราคาต่ำกว่ามูลค่าที่แท้จริงของบริษัท โดยอาศัยการวิเคราะห์เชิงปริมาณผ่านอัตราส่วนทางการเงินต่างๆ ที่ช่วยบ่งชี้ถึงความมั่นคงทางการเงินและศักยภาพในการเติบโตของบริษัท ในที่นี้เราจะเน้นการใช้อัตราส่วนราคาต่อกำไร (P/E) เป็นเกณฑ์หลักร่วมกับตัวชี้วัดทางการเงินอื่นๆ เพื่อให้การคัดเลือกหุ้นมีความครอบคลุมและมั่นคงมากขึ้น ดังนี้  

**1. อัตราส่วนราคาต่อกำไร (Price-to-Earnings, P/E Ratio):**  
	**คำอธิบาย:** อัตราส่วนนี้แสดงถึงความสัมพันธ์ระหว่างราคาหุ้นปัจจุบันกับกำไรต่อหุ้น (EPS) ของบริษัท หากอัตราส่วน P/E ต่ำเมื่อเทียบกับบริษัทอื่นในอุตสาหกรรมเดียวกัน หรือเมื่อเทียบกับค่าเฉลี่ยในอดีต อาจบ่งบอกว่าหุ้นนั้นอาจถูกประเมินค่าต่ำกว่าความเป็นจริง  
	**วิธีการคำนวณ:**  P/E Ratio= ราคาหุ้น / กำไรต่อหุ้น

 
![image](https://github.com/user-attachments/assets/fb2ddb79-ce77-4dd9-a4ae-e27ba8bb8062)
 
กราฟแสดงการกระจายตัวของ P/E ในแต่ละปี พร้อมค่า Mean เนื่องจากมีบางบริษัทขาดทุนในปีนั้นๆ ส่งผลให้ค่า P/E ของบริษัทติดลบ และอาจจะทำให้การกรองหุ้นผิดเพี้ยนได้ จึงได้ทำการตัดรายการที่ติดลบออก และทำการเฉลี่ยออกมาใหม่เป็นราย Industry  

![image](https://github.com/user-attachments/assets/07d9feab-d6e4-4f68-8873-54c7272c64aa)

**2. กำไรต่อหุ้น (Earnings per Share, EPS):**  
	**คำอธิบาย:** กำไรต่อหุ้นเป็นตัวชี้วัดสำคัญที่แสดงถึงกำไรที่บริษัทสร้างได้สำหรับผู้ถือหุ้นแต่ละราย โดยคำนวณจากการนำกำไรสุทธิหารด้วยจำนวนหุ้นทั้งหมดที่ออกจำหน่ายอยู่ EPS ที่เพิ่มขึ้นแสดงให้เห็นว่าบริษัทมีการเติบโตของกำไรอย่างต่อเนื่อง ซึ่งเป็นสัญญาณที่ดีสำหรับผู้ลงทุน  
	**วิธีการคำนวณ:** คำนวณจากสูตร:
EPS= กำไรสุทธิจำนวนหุ้นทั้งหมด / จำนวนหุ้นทั้งหมดกำไรสุทธิ

![image](https://github.com/user-attachments/assets/714347d9-d16e-4dc0-b826-b477891ca360)  
  

**3. กระแสเงินสดอิสระ (Free Cash Flow, FCF):**  
	**คำอธิบาย:** กระแสเงินสดอิสระหมายถึงเงินสดที่เหลืออยู่หลังจากการดำเนินธุรกิจและการลงทุน หากบริษัทมีกระแสเงินสดอิสระเป็นบวก แสดงว่าบริษัทสามารถสร้างเงินสดได้มากพอที่จะครอบคลุมค่าใช้จ่ายและการลงทุนต่างๆ  
	**วิธีการคำนวณ:**
FCF = เงินสดจากการดำเนินงาน−เงินสดสำหรับการลงทุน  

![image](https://github.com/user-attachments/assets/5c3a23e0-8621-4859-a2be-bfab5639338f)
  

**4. อัตราส่วนราคาต่อมูลค่าทางบัญชี (Price-to-Book, P/B Ratio):**  
	**คำอธิบาย:** อัตราส่วนนี้แสดงถึงความสัมพันธ์ระหว่างราคาหุ้นกับมูลค่าทางบัญชีของบริษัท มูลค่าทางบัญชีคือสิ่งที่เหลืออยู่หากบริษัทขายทรัพย์สินทั้งหมดและจ่ายหนี้สินแล้ว หากค่า P/B ต่ำกว่า 1 หรือ 1.5 แสดงว่าหุ้นอาจถูกประเมินค่าต่ำกว่ามูลค่าที่แท้จริง  
	**วิธีการคำนวณ:** P/B Ratio=ราคาหุ้น / มูลค่าทางบัญชีต่อหุ้น  

![image](https://github.com/user-attachments/assets/8b038c42-518b-4177-994d-72274911d414)
  


**5. อัตราส่วนสภาพคล่อง (Current Ratio):**  
	**คำอธิบาย:** อัตราส่วนนี้วัดความสามารถของบริษัทในการชำระหนี้สินระยะสั้น หากค่า Current Ratio สูงกว่า 1.5 แสดงว่าบริษัทมีสภาพคล่องที่ดี สามารถชำระหนี้สินระยะสั้นได้  
	**วิธีการคำนวณ:**  Current Ratio=สินทรัพย์หมุนเวียน / หนี้สินหมุนเวียน  

**6. อัตราส่วนหนี้สินต่อทุน (Debt-to-Equity, D/E Ratio):**  
	**คำอธิบาย:** อัตราส่วนนี้แสดงถึงระดับการใช้หนี้สินของบริษัทในการดำเนินธุรกิจ หากค่า D/E ต่ำกว่า 1 แสดงว่าบริษัทไม่พึ่งพาหนี้สินมากนัก ซึ่งลดความเสี่ยงในการดำเนินธุรกิจ  
	**วิธีการคำนวณ:** : D/E Ratio=หนี้สินทั้งหมด / ส่วนของผู้ถือหุ้น  

 **เกณฑ์ที่ใช้ในการกรองหุ้นด้วยกลยุทธ์ที่ 1 การลงทุนแบบเน้นมูลค่า (Value Investing)**
1.	P/E ของหุ้นตัวนั้นน้อยกว่าค่าเฉลี่ย P/E ของ Industry เดียวกัน
2.	P/B น้อยกว่า 1.5
3.	Current ratio มากกว่า 1.5
4.	D/E น้อยกว่า 1
5.	Free Cash Flow เป็นบวก

หุ้นที่ผ่านครบทั้ง 5 เงื่อนไขมีทั้งหมด 17 หุ้น โดยแสดงในกราฟที่ผ่านเงื่อนไขทั้ง 3 ปี (2020-2022) ดังนี้


![image](https://github.com/user-attachments/assets/ab71f0f5-6073-4de4-a9f7-912a97a5ff64)
  


รายชื่อหุ้นทั้งหมดที่ผ่านเงื่อนไขมีดังนี้
   
![image](https://github.com/user-attachments/assets/0ed84e45-21e7-4918-8bcc-9c928e23cc75)

เลือกหุ้นที่เข้าเกณฑ์มา 10 รายการแรกโดยพิจารณาจากค่า EPS เพื่อนำไปจำลองการลงทุนว่าจะได้ผลตอบแทนเท่าไร

![image](https://github.com/user-attachments/assets/bdb87c2d-bc81-4266-a3ef-d45716a66abf)  

จากกราฟจะพบว่า เมื่อเลือกซื้อหุ้นแบบ Value Investing ได้ผลตอบแทนทั้งหมด 277,647 บาท คิดเป็น 27.8% โดยผลตอบแทนจาก Capital gain 16.3% และผลตอบแทนจาก Dividend yield 11.5%

![image](https://github.com/user-attachments/assets/47a20a73-ffd9-4df7-a3f0-78df528b8a0e)

##
### Strategy 2: การลงทุนแบบเติบโต (Growth Investing)
การลงทุนแบบเติบโต (Growth Investing) คือแนวทางการลงทุนที่เน้นการเลือกลงทุนในหุ้นหรือธุรกิจที่มีศักยภาพการเติบโตสูงในอนาคต นักลงทุนมองหาบริษัทที่มีแนวโน้มรายได้และกำไรขยายตัวอย่างรวดเร็ว โดยไม่เน้นปัจจัยเรื่องมูลค่าหุ้นปัจจุบัน (เช่น P/E ratio) มากนัก แต่จะเน้นที่การเติบโตของธุรกิจในระยะยาว แนวทางนี้มักเหมาะกับผู้ที่ยอมรับความเสี่ยงได้สูง เพราะหุ้นเติบโตอาจมีความผันผวนมากกว่าหุ้นกลุ่มอื่นๆ โดยสัดส่วนทางการเงินที่จะนำมาใช้ในการคัดกรองหุ้นเติบโตมี 6 ข้อดังนี้

**1. Return on Equity (ROE): เป็นการวัดว่าบริษัทใช้เงินทุนที่ผู้ถือหุ้นลงทุนไปให้เกิดกำไรได้มากน้อยแค่ไหน ค่าที่สูงแปลว่าบริษัททำกำไรได้ดีจากเงินที่ลงทุนไป**

**2. Return on Assets (ROA): วัดว่าบริษัทใช้สินทรัพย์หรือทรัพยากรของตัวเองสร้างกำไรได้มากแค่ไหน ถ้าค่านี้สูงแปลว่าบริษัทใช้ทรัพยากรได้คุ้มค่าในการสร้างรายได้**

**3. Debt-to-Equity (D/E) Ratio: เป็นการวัดว่าบริษัทมีหนี้สินเทียบกับเงินทุนของผู้ถือหุ้นมากแค่ไหน ค่าต่ำๆ บอกว่าบริษัทไม่ได้พึ่งพาหนี้สินมาก ซึ่งมักจะปลอดภัยกว่าในระยะยาว**

โดยเราจะกรองหุ้นที่มีค่า ROE, ROA มากกว่าค่า Median และมีค่า D/E ratio น้อยกว่า 1 ดังที่แสดงในกราฟ Financial ratios screening for Growth Investment #1  

![image](https://github.com/user-attachments/assets/84e726ad-4997-41ad-9041-ca9cb5eb88b9)

**4. %Revenue Growth YoY: คือการดูว่ารายได้ของบริษัทเติบโตขึ้นกี่เปอร์เซ็นต์เทียบจากรายได้ของปีที่แล้ว หากรายได้เพิ่มขึ้นต่อเนื่อง ถือว่าบริษัทมีแนวโน้มเติบโตดี**

**5. %Net Profit Margin: คืออัตรากำไรสุทธิ คำนวณจากกำไรสุทธิเทียบกับรายได้ ยิ่งค่านี้สูงเท่าไร แปลว่าบริษัทสามารถรักษากำไรได้มากขึ้นจากรายได้**

**6. Operating Cash Flow Growth: ดูว่ากระแสเงินสดที่บริษัทสร้างจากการดำเนินงานเติบโตขึ้นแค่ไหน หากบริษัทมีกระแสเงินสดเพิ่มขึ้น ก็แปลว่าบริษัทมีความสามารถในการสร้างเงินสดจากกิจกรรมปกติได้ดีขึ้น**

ต่อมาเราจะกรองหุ้นที่มีค่าเฉลี่ยของ %Revenue Growth YoY ปี 2021 และ 2022 มากกว่าค่า Median, ค่าเฉลี่ยของ %Net profit margin ปี 2020-2022 มากกว่าค่า Median และมีค่า Operating Cash Flow Growth เป็นบวกในปี 2022 ดังที่แสดงในกราฟ Financial ratios screening for Growth Investment #2  

![image](https://github.com/user-attachments/assets/ad8a836f-83ce-4c38-8ed3-ccffcb193b44)

จากสัดส่วนการเงินที่ใช้ในการคัดกรองหุ้นทั้งหมด 6 ข้อ หุ้นที่ผ่านเกณฑ์จะเป็นหุ้นที่ตกอยู่ในพื้นที่สีเขียวและเป็นจุดสีน้ำเงิน โดยแสดงในกราฟ Financial ratios screening for Growth Investment #3  

![image](https://github.com/user-attachments/assets/82575f4b-204e-4db0-a629-7807f1f12017)

สุดท้ายเมื่อเราจะได้ลิสต์ของหุ้นที่ผ่านเกณฑ์ เราจะกรองให้เหลือเพียง 10 ตัวโดยเรียงจาก %Net profit margin สูงสุด 10 อันดับ พร้อมสำหรับนำไปใช้ในการจำลองการลงทุน  

![image](https://github.com/user-attachments/assets/cdde4d5e-7c2b-4334-961b-521994b1e5c1)

และนี่คือผลตอบแทนรายหุ้นจากการลงทุนแบบ Growth Investing เรียงตาม %ROI มากไปน้อย

![image](https://github.com/user-attachments/assets/5964c711-7243-46e6-b10a-69299bf09632)

ภาพรวมการลงทุน ให้ผลตอบแทน -6,595 บาท คิดเป็น -0.7% โดยผลตอบแทนจาก Capital gain -8.5% และผลตอบแทนจาก Dividend yield 7.8%

![image](https://github.com/user-attachments/assets/31d12d37-fc0e-48e2-92dc-7e7f3ce3cba5)

##
### Strategy 3: การลงทุนแบบเน้นเงินปันผล (Dividend Income Strategy)
**1.คำนวณค่า D/E (Debt-to-Equity Ratio)**
ค่านี้คืออัตราส่วนระหว่าง หนี้:ทุน โดยคำนวณจาก
	D/E = Debt / Equity
D/E ที่มีค่าต่ำๆ หมายความว่าบริษัทที่เราจะลงทุนไม่ได้กู้เงินจากภายนอกมากจนเกินไป (ในกลยุทธ์นี้จะกรองค่า D/E ที่ต่ำกว่า 0.25)

![image](https://github.com/user-attachments/assets/afd771c7-a233-4790-94c7-6a8a49fc1ba4)

Figure 3.1: กราฟ Histogram แสดงค่า D/E กับจำนวนบริษัท (เกณฑ์ในการคัดหุ้น D/E < 0.25)

![image](https://github.com/user-attachments/assets/f6a87796-9733-4567-b29e-6f647dedd8bf)

Figure 3.2: กราฟ Column แสดงจำนวนบริษัททั้งหมด และบริษัทที่มีค่า D/E ต่ำกว่า 0.25
	เมื่อเราพิจารณาจากค่า D/E แล้ว เราจะตัดหุ้นออกไปได้ 546 ตัว (เหลือ 310 ตัวที่จะนำมาพิจารณาเกณฑ์ถัดไป)

**2.คำนวณค่า FCF (Free Cash Flow Yield)**
ค่านี้คืออัตราส่วนระหว่าง Free Cash Flow ต่อ Market Cap โดยคำนวณจาก
FCF = Free Cash Flow / Market Cap
FCF ที่มีค่าสูงๆ หมายความว่าบริษัทสามารถจัดการกระแสเงินสดได้ดี รวมถึงเงินปันผลที่ต้องจ่ายให้ผู้ถือหุ้น (ในกลยุทธ์นี้จะกรองค่า FCF ที่สูงกว่า 0.08)

![image](https://github.com/user-attachments/assets/d50e8d52-c654-41a8-95b1-8ea58d2e1ab5)

Figure 3.3: กราฟ Histogram แสดงค่า DE กับจำนวนบริษัท (เกณฑ์ในการคัดหุ้น FCF > 0.08)

![image](https://github.com/user-attachments/assets/af5e03bf-4080-4c62-bdfb-f1ac63cc9405)

Figure 3.4: กราฟ Column แสดงจำนวนบริษัททั้งหมด, บริษัทที่มีค่า DE ต่ำกว่า 0.25 และบริษัทที่มีค่า FCF > 0.08

หลังจากพิจารณา 2 เกณฑ์ที่กล่าวมา จำนวนหุ้นจะลดลงประมาณ 90% ขั้นตอนถัดไปเราจะหาค่าเฉลี่ยของ % เงินปันผลตั้งแต่ปี 2020 ถึง 2022 ที่มากกว่า 3%

![image](https://github.com/user-attachments/assets/572b13e8-e4f2-4392-8d62-9110c2fd360c)

Figure 3.5: กราฟ Dot แสดง % เงินปันผลของบริษัทที่ผ่านเกณฑ์ DE, FCF และเงินปันผลมากกว่า 3%
จากข้อมูลนี้จะทำให้ได้หุ้น 10 ตัวได้แก่ 'SSSC', 'RCL', 'UPF', 'AYUD', 'RPC', 'UPOIC', 'TPA', 'TMD', 'HFT', 'PRAKIT' ซึ่งแต่ละตัวจะให้ผลตอบแทนดังนี้

![image](https://github.com/user-attachments/assets/345a3115-ec0b-456b-9cd1-c43e084b48b6)

Figure 3.6: ตารางแสดงผลตอบแทนและเงินปันผลของหุ้น 10 ตัวที่ผ่านการกรองโดย Dididend Income Strategy

![image](https://github.com/user-attachments/assets/4d947efd-94fd-4aaf-bf21-d77fdc281560)

Figure 3.7: Line plot แสดงการเพิ่มขึ้น/ลดลงของเงินทุนตั้งต้น (1,000,000 บาท) และเงินปันผลสะสม ตลอดระยะเวลาการลงทุนตั้งแต่ มีนาคม 2023 จนถึงกันยายนปี 2024

จากกราฟจะพบว่า เมื่อเลือกซื้อหุ้นแบบ Dividend Income Strategy ได้ผลตอบแทนทั้งหมด 131,865 บาท คิดเป็น 13.2% โดยผลตอบแทนจาก Capital gain -3.2% และผลตอบแทนจาก Dividend yield 13.0%

##
### สรุปผล/เปรียบเทียบการลงทุนทั้ง 3 แบบ

![image](https://github.com/user-attachments/assets/15ef57c3-ffa5-4fa2-8eb6-efa6c70488a0)


Figure 4.1: ผลตอบแทนของการลงทุนด้วยจำนวนเงิน 1 ล้านบาท ตามรูปแบบกลยุทธ์

หากเราลงทุนโดยเลือกหุ้นที่คัดกรองมา Strategy ละ 10 ตัว โดยลงแบ่งลงทุนด้วยเงินต้นเท่าๆกัน (Strategy ละ 1,000,000 บาท แล้วแบ่งซื้อตัวละ 100,000 บาท)
จะได้ผลตอบแทนดังนี้ (ระยะเวลาลงทุน มีนาคม 2023 จนถึงกันยายนปี 2024)

1. Value Investment: ได้ผลตอบแทนมากที่สุด คือประมาณ 270,000 บาท หรือ 28% โดยมีหุ้นที่ให้กำไรสูงสุดอยู่ 2 ตัว (SUC, SCCC) มีค่า ROI 35% และ 32% ตามลำดับ
2. Dividend Strategy: ได้ผลตอบแทนรองลงมา คือประมาณ 130,000 บาท หรือ 10% โดยหุ้นที่ทำกำไรและให้เงินปันผลสูงที่สุดคือ RCL (Dividend yield 19%)
3. Growth Investment: ขาดทุน 6,600 บาท หรือ -0.7% เพราะเกิดจากหุ้น BE8 ที่ราคาร่วงอย่างหนัก (ROI -61%)
