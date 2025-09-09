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
| Read/Write  | R/W | R/W | R/W | R/W | R/W | R/W | R/W |  W   |
| Initial Val |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0   |
Bit 0 is an send register.
By setting the Bit 0, this IP module starts to send the data.
When the data transmission ends, Bit 0 have to be resetted for the next data transfer.

Bit 1 is a selection register.
By setting the Bit 1, LCD receive the signal as character data.
By resetting the Bit 1, LCD receive the signal as command lines. 

### 4. BUSY



# 2. KR
