# End Effector 2023
## Device overview
  End Effector เป็นอุปกรณ์ที่ทำหน้าที่เสมือน Gripper อย่างง่าย โดยใช้การติดต่อสื่อสารผ่าน I2C และแสดงผลการจำลองการทำงานของ gripper ผ่าน laser และ LED แสดงสถานะการณ์ทำงาน โดยใช้ติดตั้งบนหุ่นยนต์หรือแขนกลเพื่อใช้ในการจำลองการหยิบจับชิ้นงาน
  
  ระบบจะจำลองการทำงานของ Gripper แบบ Remote ผ่าน I2C โดยGripperจำลองดังกล่าวจะมีการเปิดปิดการทำงานของGripperได้เพื่อประหยัดพลังงาน และเมื่อเปิดการทำงานแล้วจะสามารถจำลองการหยิบจับและวางสิ่งของได้โดยมีการจำลองระยะเวลาที่ใช้ในการหยิบขึ้นหรือวางลงด้วย
 
 โดยระบบ Endeffector สามารถเข้าสู่ Test mode เพื่อเปิดการทำงานของ laser ค้างเอาไว้เพื่อใช้ในการกำหนดตำแหน่งต่างๆได้
  และมี emergency mode ซึ่งจะหยุดการทำงานปัจจุบันและlockระบบเพื่อจำลองความปลอดภัยให้แ่กผู้ใช้
  

    
## Device infomation
![image](https://github.com/AlphaP2712/EndEffector2023/assets/74948675/e0c55d8c-805f-453d-8da3-c4cd22b5782e)
ในส่วนของend end effecttor จะแสดงสถานะการทำงานโดยใช้สัญญาณไฟLED โดยมีการแสดงผลดังต่อไปนี้

### ไฟแสดงสถานะ POW
  POW แสดงสถานะของได้รับไฟเลี้ยงของ end effector โดยทั่วไปแล้วควรติดตลอดเวลาที่จ่ายไฟ แต่มีโอกาศที่ จะติดโดยที่ไม่ได้จ่ายไฟได้จากการรั่วไหลของไฟฟ้าตาม IO ต่างๆ เช่น I2C ในกรณีนั้นอาจจะส่งผลให้ระบบทำงานผิดพลาดได้
| LED POW | คำอธิบาย|
|-------|--------|
| ติด | ระบบได้รับไฟเลี้ยงสำหรับการทำงาน |
| ดับ | ระบบไม่ได้รับไฟเลี้ยงสำหรับการทำงาน  |



### ไฟแสดงสถานะ L 
  L แสดงถึงสถานะการทำงานโดยรวมของ End Effector โดยใช้การกระพริบในการสื่อสารสถานะปัจจุบัน L จะกระพริบอยู่เสมอเพื่อเป็นการบอกว่าโปรแกรมภายในend effectorยังคงทำงานอยู่ หาก L หยูดการเปลี่ยนแปลงเช่น ติดค้างหรือดับค้าง แสดงถึงข้อผิดพลาดอย่างร้ายแรงในระบบซึ่งส่งผลให้ระบบ End effector ค้าง
| LED L | คำอธิบาย|
|-------|--------|
| ติดกระพริบอย่างต่อเนื่องช้าๆ ด้วยความถี่ประมาณ 1 Hz | ระบบกำลังเริ่มการทำงาน |
| ติดสั้นๆ 1ครั้ง เป็นช่วงๆ | ระบบพร้อมทำงาน / สถานะการทำงานปกติ |
| ติดสั้นๆ 2ครั้ง เป็นช่วงๆ | อยู่ใน TEST MODE |
| กระพริบอย่างรวดเร็ว ด้วยความถี่สูงประมาณ 2 Hz | ระบบอยู่ใน Emergency Mode |
| ติดค้าง ฝ ดับค้าง | ระบบมีข้อผิดพลาดอย่างร้ายแรง กรุณาติดต่อพี่ปั้น  |


### ไฟแสดงสถานะ D1
  D1 แสดงสถานะการเปิด - ปิดการทำงานของ Gripper
| LED D1 | คำอธิบาย|
|-------|--------|
| ติด | Gripper ได้เปิดระบบการทำงานและพร้อมที่จะหยิบ/วางชิ้นงาน |
| ดับ | Gripper อยู่ในสถานะปิดการทำงาน  |

### ไฟแสดงสถานะ D2
  D2 แสดงสถานะการหยิบ/วางของ Gripper
| LED D2 | คำอธิบาย|
|-------|--------|
| ติด | Gripper กำลังจับชิ้นงานเอาไว้|
| ดับ | Gripper ไม่ได้จับชิ้นงานใดๆ  |
| กระพริบ | Gripper อยู๋ในระหว่างการหยิบชิ้นงานขึ้นหรือปล่อยชิ้นงานลง  |

### ไฟแสดงสถานะ laser
  Laser แสดงสถานะการหยิบ/วางของ Gripper เช่นเดียวกับ D2 แต่จะทำงานเมื่อ Gripper หยิบ / วาง เสร็จแล้ว โดยจะใช้การกระพริบผสมระหว่าง กระพริบสั้นและยาว เพื่อบอกการหยิบ / วาง
| Laser | คำอธิบาย|
|-------|--------|
| สั้น สั้น ยาว | Gripper ได้จับชิ้นงานขึ้นมาแล้ว|
| ยาว สั้น สั้น | Gripper ได้วางชิ้นงานลงแล้ว|
| ติดค้าง  | อยู่ใน Test mode  |


### ปุ่ม Rst 
 ใช้ในการรีเซ็ตระบบของ End effector ใหม่




## Mode of operation
ระบบ end effector มีโหมดการทำงานหลักทั้งสิ้น 4 โหมดเพื่อใช้ในการทดสอบและปรับแต่งตำแหน่งของ end effector โดยอาศัยการดูสัญญาณจาก ไฟ LED จะสามารถระบุโหมดการทำงานปัจจุบันได้
### idle mode (L กระพริบ 1 ครั้งสั้นๆ เป็นช่วงๆ)
เป็นโหมดการทำงานเริ่มต้นของระบบ end effector จะยังไม่สามารถใช้งาน Gripper ในการหยิบจับสิ่งของได้
### Test mode (L กระพริบ 2 ครั้งสั้นๆ เป็นช่วงๆ)
เป็นโหมดการทำงานที่สามารถเปิด-ปิดได้ผ่าน I2C โดย Test mode จะเปิดการทำงานของ Laser ค้าง เพื่อให้สามารถใช้ในการปรับตำแหน่งของหุ่นยนต์ / แขนกลในสนามโดยใช้ตำแหน่งของlaser ในการอ้างอิง
### Run mode (L กระพริบ 1 ครั้งสั้นๆ เป็นช่วงๆ และ D1 ติดค้าง)
เป็นโหมดการทำงานที่สามารถเปิด-ปิดได้ผ่าน I2C สามารถใช้งาน Gripper ในการหยิบจับสิ่งของได้ ซึ่งเป็นโหมดที่จะต้องเปิดใช้งานเมื่อจะต้องใช้งาน Gripper
### emergency mode (L กระพริบ 1 อย่างรวดเร็ว)
เป็นโหมดฉุกเฉินเพื่อใช้งานร่วมกับ Emergency emergency switch โดย สั่งคำสั่งผ่าน I2C โดยในโหมดนี้จะหยุดการทำงานปัจจุบันของ Gripper ทั้งหมดเพื่อจำลองในด้านการปลอดภัยของผู้ใช้งาน โหมดดังกล่าวควรได้รับคำสั้งการทำงานทันทีที่มีการกดemergecy switch หรือ แขนกล/หุ่นยนต์ที่ end effector ติดตั้งเข้าสู่โหมด emergency

โหมดการทำงานนี้สามารถปิดได้ผ่านการใช้ I2C โดยใช้เขียนคำสั่งเฉพาะ เพื่อออกจากโหมด Emergency เพื่อป้องกันการผิดพลาดอันเกิดจากสัญญาณรบกวนซึ่งส่งผลให้ออกจาก emergency mode ก่อนได้รับคำสั่งที่ถูกต้อง




## I2C Interface
 End effector ทำงานในโหมด I2C slave โดย รองรับการทำงาน I2C ที่ความถี่ 100kHz โดยการติดต่อใดๆกับ end effector จะกระทำผ่าน  slave address = 0x15 
 
 เนื่องจากระบบของ ARDUINO NANO ใช้แรงดัน 5V จึงต้องใช้ I2C ที่แรงดัน 5V  โดยใช้ resistor pull up ขนาด 1k - 4.7k เข้ากับแรงดัน 5V ทั้งนี้ nucleo สามารถใช้pullup ดังกล่าวในการจ่าย สัญญาณI2C ให้แก่ end effector ได้
 
 
 
 ### คำสั่ง soft reset
 ในกรณีทั่วไปสามารถสั่งreset end effector ได้ผ่านทาง I2C โดยใช้คำสั่ง
 
 |Start + Address + Write|seq1|seq2|seq3|seq4 + stop|
 |-----------------------|-----------|----------|----------|-----------------|
 |0x15 + W               |0x00|0xFF|0x55|0xAA|
 
 โดยเมื่อสิ้นสุดคำสั่งระบบจะ reset ทันที และจะไม่สามารถรับคำสั่งต่อได้จนกว่าระบบจะ reset เสร็จสิ้น
 
  ___________________________________________  
  
  
 ### คำสั่ง Emergency mode
  เมื่อจำเป็นจะต้องเปิด emergency mode ในการหยุดการทำงานของ gripper อย่างฉับพลันเพื่อความปลอดภัยของผู่ใช้งาน สามารถสั่งได้ผ่านคำสั่ง 
 
 |Start + Address + Write| emergency trigger + stop|
 |-----------------------|-----------|
 |0x15 + W               |0xFX|
 
 โดย 0xFX ค่าใดก็ได้ระหว่าง 0xF0 - 0xFF ระบบจะเข้าสู่ emergency Mode ทันที (อาจจะมี delay ที่ไฟแสดงสถานะ L เล็กน้อย แต่ระบบจะเข้าemergencyทันทีที่ได้รับคำสั่ง)
 
 เมื่อต้องการออกจาก emergency สามารถทำได้โดยการส่ง sequence ปลดล็อกความปลอดภัยดังนี้ 
 
  |Start + Address + Write|seq1|seq2|seq3|seq4 + stop|
 |-----------------------|-----------|----------|----------|-----------------|
 |0x15 + W               |0xE5|0x7A|0xFF|0x81|
 
   ___________________________________________  

 
 ### คำสั่งเปิดการใช้งาน Test Mode
  การเปิด ปิด test mode เพื่อเปิดใช้งาน laser ค้างเอาไว้ โดยสามารถใช้งานโหมดดังกล่าวได้ผ่านI2C โดยใช้คำสั่ง
 
 |Start + Address + Write| test mode command| test mode on off + stop|
 |-----------------------|-----------|----------|
 |0x15 + W               |0x01|0xXX|
 
โดย **0xXX** จะเปิด Test mode เมื่อเป็นค่าใดๆ ที่ไม่ใช่ 0 และ จะปิดการทำงานของ test mode เมื่อมีค่าเป็น 0
  ___________________________________________  

### คำสั่งการใช้งาน Gripper
เมื่อ end effector ถูก Reset หรือเริ่มการทำงาน Gripper จะไม่อยู่ใน run mode และอยู่ใน โหมดidle เพื่อเป็นการสมมุติว่าประหยัดพลังงาน เพื่อให้ gripper เข้าสู่ run mode ในการหยิบวางสิ่งของ จะต้องเปิดการทำงานของgripper และเข้าสู่run mode โดยใช้คำสั่ง

 |Start + Address + Write| Run mode command| Run mode on + stop|
 |-----------------------|-----------|----------|
 |0x15 + W               |0x01|0x13|
 
 เมื่อ gripper ทำงาน ไฟแสดงสถานะ D1 จะติดเพื่อแสดงสถานะ Run Mode และ gripper สามารถหยิบ/วางสิ่งของได้
 
 ___________________________________________  
 
เพื่อปิดการทำงาน run mode เพื่อเข้าสู่โหมดประหยัดไฟ สามารถทำได้โดยการใช้คำสั่ง

 |Start + Address + Write| Run mode command| Run mode off + stop|
 |-----------------------|-----------|----------|
 |0x15 + W               |0x01|0x8C|
   
 ___________________________________________  
   
 ถ้าหากGripper อยู่ใน mode run จะสามารถสั่งงานให้ gripper หยิบจับสิ่งของได้โดย ใช้คำสั่ง
  |Start + Address + Write| Run mode command| Run mode Pick Up + stop|
 |-----------------------|-----------|----------|
 |0x15 + W               |0x01|0x5A|
 
 โดย gripper จะใช้เวลาประมาณ 2 วินาที ในการทำงาน ไฟ D2 จะกระพริบแสดงการทำงานตลอดช่วงเวลาการหยิบ และเมื่อ gripper หยิบชิ้นงานเรียบร้อยlaser จะยิงในจังหวะ สั้น สั้น ยาว เพื่อระบุตำแหน่งและเป็นการบอกว่า การหยิบได้เสร็จสิ้นแล้ว ไฟ D2 จะติดค้างเพื่อแสดงว่าgripper กำลัง จับสิ่งของอยู่
   
  ___________________________________________  
   
 ถ้าหากGripper อยู่ใน mode run จะสามารถสั่งงานให้ gripper วางสิ่งของได้โดย ใช้คำสั่ง
  |Start + Address + Write| Run mode command| Run mode place down + stop|
 |-----------------------|-----------|----------|
 |0x15 + W               |0x01|0x69|
 
 โดย gripper จะใช้เวลาประมาณ 2 วินาที ในการทำงาน ไฟ D2 จะกระพริบแสดงการทำงานตลอดช่วงเวลาการวาง และเมื่อ gripper วางชิ้นงานเรียบร้อยlaserจะยิงในจังหวะ ยาว สั้น สั้น เพื่อระบุตำแหน่งและเป็นการบอกว่า การวางได้เสร็จสิ้นแล้ว ไฟ D2 จะดับเพื่อแสดงว่าgripper ไม่ได้กำลังจับสิ่งของอยู่
     
     
### การอ่านสถานะปัจจุบันของ End effector 
สามารถอ่านได้โดยการใช้คำสั่ง 
  |Start + Address + Read| Device status + stop|
 |-----------------------|-----------|
 |0x15 + R               |0xXX|
 
 โดย 0xXX จะเป็นค่า Device status  ขนาด  8 bits ซึ่งส่งกลับมาจาก End effector โดยแต่ละบิตจะแทนสถานะดังต่อไปนี้
 Bit 7 - 5 : Error code (WIP)
 Bit 4     : Emergency mode (1 = on,0 = Off)
 Bit 3     : Test mode (1 = on,0 = Off)
 Bit 2     : Run mode (1 = on,0 = Off)
 Bit 1 - 0 : Gripper status (0b00 = Not pick , 0b01 = picking, 0b10 = placeing ,0b11 = picked )
 
## การ update software ให้กับend effector 
 WIP
