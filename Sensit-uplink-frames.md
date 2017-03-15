# Sensit v2 uplink frames

## About

This doc describes how the frames sent by the Sensit v2 are formatted.

Bytes are read from left to right, the _first byte_ being the most significant one

Bits are numbered the other way, from the LSB to the MSB. _Bit 0_ being the LSB & _bit 7_ the MSB of the said byte.

Example : received frame is _A9670d19_.  
First byte is `0xA9` or `0b10101001`.  
Or `{bit 7}{bit 6}{bit 5}{bit 4}{bit 3}{bit 2}{bit 1}{bit 0}`


## Overall


### First byte
| Mode | Timeframe | Type | Battery MSB | 
| ------------ | ------------- | ------------ | ------------ |
| b0-b2 | b3-b4 | b5-b6 | b7 |
| 3 bits | 2 bits | 2 bits | 1 bit |

### Second byte
| T° MSB | Battery LSB |
| ------------ | ------------- |
| b0-b3 | b4-b7 | 
| 4 bits | 4 bits | 

### Following bytes
Data bytes, between 2 and 10 bytes depending on the active mode

## Mode : int

| Value | Mode |
| ----- | ---- |
| 0 | Button |
| 1 | T° + humidity |
| 2 | Light |
| 3 | Door |
| 4 | Move |
| 5 | Reed switch |

## Timeframe
| Value | Timeframe |
| ----- | ---- |
| 0 | 10 mins |
| 1 | 1 hour |
| 2 | 6 hours |
| 3 | 24 hours |

## Type
| Value | Type |
| ----- | ---- |
| 0 | Regular, no alert |
| 1 | Button call |
| 2 | Alert |
| 3 | New mode |

## Battery

Use the following formula to get the value in V (between 2.7 & 4.25V)  
`((({battery MSB} * 16) + {battery LSB}) + 54) / 20`

## Temperature (MSB)

Sent along each frame (not only in temp mode)

Value in °C : `({value} * 6.4) - 20`  
Value in °F : `((({value * 6.4) - 20) * 1.8) - 30)`


## Data 

### Third byte

#### Classic mode (excludes Light & Door regular frames)

| T° LSB | Reed Switch state | Unused |
| ----- | ---- | ---- |
| b0 - b5 | b6 | b7 |

Temperature in °C = `({value}-200) / 8`

#### Light mode
| Value | Multiplier |  
| ----- | ---- |
| b0 - b5 | b6 - b7 |

| Multiplier value | Final multiplier |
| ---- | ---- |
| 0 | 1|
| 1 | 8|
| 2 | 64|
| 3 | 2014


Light (lux) = `{final multiplier} * {value} * 0.01`

#### Door mode

Byte 3 is unused there, as it's reserved for configuration values
### Fourth byte

#### Button frames  
Byte 4 contains the software version

| Minor version | Major version |
| ----- | ----- |
| b0-b3 | b4-b7 |

#### Temperature mode 
Byte 4 contains the humidity value
Humidity (%) = `value * 0.5`

#### Other modes
This byte contains the number of alerts, as in movemenet detection mode for ex.


## Examples

### Ex 1 : E9671854

#### Binary representation

| Byte number | Hex value | Binary |
| ---- | ---- | ---- |
| 0| 0x`E9`| 0b`11101001` |
| 1| 0x`67`| 0b`01100111` |
| 2| 0x`18`| 0b`00011000` |
| 3| 0x`54`| 0b`01010100` |

#### Sensit mode

3 LSB from first byte : 11101`001`  
1 means _temperature & humidity_ mode

#### Frame type
Bits 5 & 6 from first byte : 1`11`01001  
4 means _new mode_

#### Data

##### Humidity
Humidity % is byte 4 * 0.5

4th byte is `0x54` or 84, meaning humidity is `42%`

##### Temperature

***MSB***

The most significant bits are the last (or most significant) 4 bits of the second byte.

Here, the second byte is `0x67` or `0b01100111`

This means the temperature MSB are `0110`


***LSB***

The least significant bits are the first 6 bits of the third byte.

Here, the third byte is `0x18` or `0b00011000`.

This means the temperature LSB are `011000`


***Temperature value***

We need to combine MSB & LSB. So our temperature value is `0110` `011000`, or `0b0110011000` or `408`.

The formula to get the value in Celsius degrees is `({value}-200) / 8`.

This means our temperature is `(408-200)/8` or `26°C`


### Ex 2: C2646418
#### Binary representation


| Byte number | Hex value | Binary |
| ---- | ---- | ---- |
| 0| 0x`C2`| 0b`11000010` |
| 1| 0x`64`| 0b`01100100` |
| 2| 0x`64`| 0b`01100100` |
| 3| 0x`18`| 0b`00011000` |

#### Sensit mode

3 LSB from first byte : 11000`010`  
2 means _Light sensor_ mode

#### Frame type
Bits 5 & 6 from first byte : 1`10`00010  
2 means _alert_

#### Data

***Raw Value***

Bits 0 to 5 from third byte : 01`100100`  
Meaning the raw value is `0b100100` or `36`

***Multiplier***

Bits 6 & 7 (2 most significant bits) from third byte : `01`100100  
`1` means we'll have to multiply by `8`

***Light level value***

Formula is Light (lux) = `{final multiplier} * {value} * 0.01`

Here, Light (lux) = `8 * 36 * 0.01` or `288 * 0.01` or `2.88 lux`
