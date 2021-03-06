ในส่วนนี้จะเป็นการอธิบายการทำงานของโปรแกรม emulsiV simulator
# คำสั่งในการใช้งานพร้อมตัวอย่างมีดังนี้
1. addi x1, x0, 32 : คือการเอาค่า 32 บวกกับ x0 แล้วใส่แทนที่ x1 
2. lui x2, 0xc0000000 : คือการเอาค่าด้านขวา 20 bits แรก ใส่ไปใน x2  (lui มาจาก load upper immediate)
3. lbu x3, 0(x1) : คือการเอา 1 byte จากตัว x1 ไปใส่ตัว x3   (lbu มาจาก load byte unsigned)
4. beq x3, x0, +16 : ถ้า x3 = x0 จะ Jump ไป 16 หรือโดดไป4แถว(address)ด้านล่าง   (beq มาจาก branch on equal)
5. sb x3, 0(x2) : เอา x3 เก็บใน x2   (sb มาจาก store byte)

## ขั้นตอนการทำงานของโปรแกรม

* 1st loop 
1. เริ่มจากที่ address00000000(จากนี้จะขอเรียกย่อๆว่า add00) จะทำการเรียง word จากหลังไปหน้าแล้วส่งไปที่ data ที่อยู่ใน Bus ในขั้นตอนนี้จะเป็นการเรียงข้อมูลใหม่ > จากนั้นจะส่งข้อมูลที่ได้ไปที่ instr เพื่อทำการแปลคำสั่งที่ได้ โดยเมื่อแปลคำสั่งจะแสดงขั้นตอนคำนวณที่ ALU > ใน ALU แสดงขั้นตอนคำนวณได้ว่า เอาค่าของ x0 + 32 แล้วนำค่าที่ได้ใส่ไปที่ x1
2. ที่ add04 ทำการเรียง word เพื่อส่งไปที่ data เป็นการเรียงข้อมูล > ส่งไปที่ instr เพื่อแปลคำสั่งแล้วส่งไปที่ ALU > ALU แสดงการคำนวณที่ได้คือ นำ 20 bits แรกของ c0000000 ไปใส่ที่ x2
3. ที่ add08 ทำการเรียง word เพื่อส่งไปที่ data เป็นการเรียงข้อมูล > ส่งไปที่ instr เพื่อแปลคำสั่งแล้วส่งไปที่ ALU > ALU แสดงการคำนวณที่ได้คือ นำ 1byte จาก x1 ซึ่งชี้ไปที่ add20 ได้ค่า 41 เมื่อดูตาราง ASCII ที่ ฐาน 16 จะได้ตัวอักษร A (Akawat) ไปที่ x3
4. ที่ add0c ทำการเรียง word ส่งไปที่ data > ส่งไปที่ instr เพื่อแปลคำสั่งแล้วส่งไปที่ ALU > ALU แสดงการคำนวณที่ได้คือ ถ้า x3 = x0 จะ Jump ไป 16 (โดดไป 4 แถว) ซึ่งในที่นี้ x3 ไม่เท่า x0 จึง skip ไป
5. ที่ add10 ทำเหมือนเดิม จนไปถึง ALU แล้วแสดงการคำนวณ ได้ว่า นำ x3(A) ส่งไปที่ x2(c0000000)ซึ่งเปรียบได้กับหน้าจอ microcontroller(ซึ่งถ้ารันขั้นตอนนี้จะไปแสดงที่หน้าจอ) ก็จะได้ตัว A แสดงที่หน้าจอ
6. ที่ add14 ทำเหมือนเดิม จนไปถึง ALU แสดงการคำนวณได้ว่า นำ x1(00000020) + (00000001) = 00000021 แล้วใส่ทับที้ x1 (เป็นการเพิ่มค่า x1 ทีละ 1 ทุกครั้งที่วนลูป)
7. ที่ add18 ทำเหหมือนเดิม ไปถึง ALU แปลคำสั่งได้ว่า jal(jump and link) -16 คือถอยกลับไป 4 step(4แถว) จาก add18>jal-16>add08 
*จบลูปที่ 1
<img width="1257" alt="emul02" src="https://user-images.githubusercontent.com/98943405/160885891-04c5cd22-9c6e-40bf-9179-3fec903315eb.png">

* 2nd loop  
 1.ที่ add08 lbu จะนำค่าของ 1 byte ของ x1 ซึ่งชี้ไปที่ add21 คือ 43 เทียบตาราง ASCII ได้เป็นตัวอักษร C(Chartsirilertsakul) แล้วนำ 00000043 ไปเก็บแทนที่ x3  
 2.add 0c ทำการเทียบค่าว่า x3 = x0 หรือไม่ซึ่งถ้าไม่เท่าจะ skip ขั้นตอนนี้ไป  
 3.add 10 sb ทำการเอาค่า x3(00000043) ไปใส่ที่ x2 ซึ่งก็จะปรากฎตัว C ที่หน้าจอ microcontroller ได้เป็น AC (Akawat Chartsirilertsakul)   
 4.add 14 addi เอา x1(00000021) + 1 แล้วใส่ค่าทับที่ x1(00000022)   
 5.add 18 jal-16 จะเป็นการ Jump ถอยหลังไป 4 บรรทัด ในที่นี้กลับไปที่ add08

* 3rd loop  
 1.add08 lbu นำค่า 1byte ของ x1(00000022) ซึ่งชี้ไปที่ add22 คือ 00 เทียบตาราง ASCII คือ null ไม่มีค่าอะไร ไปทับที่ x3(00000000)  
 2.add0c beq เทียบค่า x3 = x0(00000000) ซึ่งเท่ากัน จึง jump +16> add1c ซึ่งถือว่าเสร็จสิ้นกระบวนการที่จุดนี้ เพราะจะวนลูปที่คำสั่งนี้ไปเรื่อยๆไม่สิ้นสุด
![image](https://user-images.githubusercontent.com/98943405/160885473-54c821ed-4637-46bf-ba30-477573b91a8e.png)

