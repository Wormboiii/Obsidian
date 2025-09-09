# 1. EN
## 1. Register Description

### 1. DEVICE_ADDR

|     Bit     |   7   |   6   |   5   |   4   |   3   |   2   |   1   |   0   |
| :---------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|             | ADDR7 | ADDR6 | ADDR5 | ADDR4 | ADDR3 | ADDR2 | ADDR1 | ADDR0 |
| Read/Write  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |
| Initial Val |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |
Bit 0 to 7 are adress for an external device. In this case, it would be the text lcd.
To use the external device, you have to write the adress of the device on this register.


### 2. SEND_DATA

|     Bit     |   7   |   6   |   5   |   4   |   3   |   2   |   1   |   0   |
| :---------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|             | Data7 | Data6 | Data5 | Data4 | Data3 | Data2 | Data1 | Data0 |
| Read/Write  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |  R/W  |
| Initial Val |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |
Bit 0 to 7 are the registers that saves the actual data you want to send.

### 3. SEND_CNTR

|     Bit     |  7  |  6  |  5  |  4  |  3  |  2  |  1  |  0   |
| :---------: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :--: |
|             |  -  |  -  |  -  |  -  |  -  |  -  | R/S | Send |
| Read/Write  | R/W | R/W | R/W | R/W | R/W | R/W |  W  |  W   |
| Initial Val |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0   |
Bit 0 is a send register.
By setting the Bit 0, this IP module starts to send the data.
When the data transmission ends, Bit 0 have to be resetted for the next data transfer.

Bit 1 is a selection register.
By setting the Bit 1, LCD receive the signal as character data.
By resetting the Bit 1, LCD receive the signal as command lines. 

### 4. BUSY

|     Bit     |  7  |  6  |  5  |  4  |  3  |  2  |  1  |  0   |
| :---------: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :--: |
|             |  -  |  -  |  -  |  -  |  -  |  -  |  -  | Busy |
| Read/Write  | R/W | R/W | R/W | R/W | R/W | R/W | R/W |  R   |
| Initial Val |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0   |
Bit 0 is a busy register.
If the module is in transmission, Bit 0 will automatically be set.
If the module completes the transmission, Bit 0 will automatically be reset.


## 2. Provided Functions

```verilog title:"lcd.h"
#include <stdio.h>
#include "platform.h"
#include "xparameters.h"
#include "sleep.h" 

#define TXTLCD_ADDR XPAR_MYIP_IIC_TXTLCD_0_BASEADDR


volatile unsigned int *DEVICE_ADDR = (volatile unsigned int*)TXTLCD_ADDR;

volatile unsigned int *SEND_DATA = (volatile unsigned int*)(TXTLCD_ADDR+0x04);

volatile unsigned int *SEND_CNTR = (volatile unsigned int*)(TXTLCD_ADDR+0x08);

volatile unsigned int *BUSY = (volatile unsigned int*)(TXTLCD_ADDR+0x0C);

  
void lcdCommand(uint8_t command);
void lcdData(uint8_t data);
void i2cLcd_Init();
void lcdString(char *str);
void moveCursor(uint8_t row, uint8_t col);
void Display_clear();
```

```verilog title:"lcd.c"
#include "lcd.h"

void lcdCommand(uint8_t command) {
	while(*BUSY);
	*DEVICE_ADDR = 0x27;
	*SEND_DATA = command;
	*SEND_CNTR = 0x01; 
	
	while(*BUSY);
	*SEND_CNTR = 0;
}
  
void lcdData(uint8_t data) {
	while(*BUSY);
	*DEVICE_ADDR = 0x27;
	*SEND_DATA = data;
	*SEND_CNTR = 0x03; 
	
	while(*BUSY);
	*SEND_CNTR = 0;
}

void i2cLcd_Init() {

	msleep(50);
	lcdCommand(0x33);
	msleep(5);
	lcdCommand(0x32);
	msleep(5);
	lcdCommand(0x28);
	msleep(5);
	lcdCommand(0x0c);
	msleep(5);
	lcdCommand(0x06);
	msleep(5);
	lcdCommand(0x01); //약 2ms 필요
	msleep(1);

}
  
void lcdString(char *str) {
	while(*str)lcdData(*str++);
} 

void moveCursor(uint8_t row, uint8_t col) {
	lcdCommand(0x80 | row <<6 | col);
}
  
void Display_clear() {
	lcdCommand(0x01);
	usleep(2000);
}
```
# 2. KR
