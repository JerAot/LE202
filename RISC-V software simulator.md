ในส่วนนี้จะเป็นการอธิบายการทำงานของโปรแกรม emulsiV simulator
# คำสั่งในการใช้งานพร้อมตัวอย่างมีดังนี้
1. addi x1, x0, 32 : คือการเอาค่า 32 บวกกับ x0 แล้วใส่แทนที่ x1 
2. lui x2, 0xc0000000 : คือการเอาค่าด้านขวา 20 bits แรก ใส่ไปใน x2  (lui มาจาก load upper immediate)
3. lbu x3, 0(x1) : คือการเอา 1 byte จากตัว x1 ไปใส่ตัว x3   (lbu มาจาก load byte unsigned)
4. beq x3, x0, +16 : ถ้า x3 = x0 จะ Jump ไป 16 หรือโดดไป4แถว(address)ด้านล่าง   (beq มาจาก branch on equal)
5. sb x3, 0(x2) : เอา x3 เก็บใน x2   (sb มาจาก store byte)
