#ในหัวข้อนี้จะเป็นการอธิบาย code ของ 6 การทดลองครับ
## 1.ex01  
 1.1 void setup() : เป็นการตั้งค่าความเร็วของ microcontroller กับ computer ให้มีค่าเหมือนกันเพื่อให้ทำงานร่วมกันได้ เราเรียกว่า Boardurate(bits/sec) ซึ่งเราสามารถ  set ให้มีค่ามาก/น้อย ขึ้นอยู่กับตัวบอร์ดว่ารับได้มากแค่ไหน  
 1.2 void loop() : จะเป็นการวนซ้ำไปเรื่อยๆ โดยการเพิ่มค่า cnt ทีละ 1 แล้วให้ผลลัพธ์ที่ได้อยู่ในตัวแปร A โดยมีดีเลย์อยู่ที่ 0.3 sec  
### 2.ex02  
 2.1 void setup() : WiFi.mode(WIFI_STA) STA : โหมด station accession point เป็นการติดต่อระหว่าง board กับตัวปล่อยสัญญาณ(Router)  
 2.2 void loop() : เริ่มต้นค้นหา Wifi ถ้า n = 0 : ไม่เจอ wifi , กรณี n ไม่เปน 0 , SSID (Service Set Identifier) : ชื่อเครือข่าย , RSSI(Received Signal Strength Indication) : ตัวบ่งชี้ความแรงของสัญญาณ , WiFi.encryptionType(i) --29 เป็นการบอกสถานะการเข้ารหัส wifi ถ้าเข้าได้ให้แสดงว่า " " ถ้าไม่ได้ให้แสดง "*"
#### 3.ex03
 3.1
