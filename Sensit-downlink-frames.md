# Sensit v2 downlink frames

## About this doc

This doc describes how the frames sent to the Sensit v2 are formatted.

Bytes are read from left to right, the _first byte_ being the most significant one

Bits are numbered the other way, from the LSB to the MSB. _Bit 0_ being the LSB & _bit 7_ the MSB of the said byte.


## Downlink frame

_Downlink_ messages are requested by the Sensit :

* daily
* After a 10-seconds press of the button

Upon receiving this request, the backend will reply with an 8-byte config frame structured as described below




## Frame composition

Byte 0 | Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7
--- | --- | --- | --- | --- | --- | --- | ---
Temperature | Temperature | Light | Light | Accelerometer | Accelerometer | Accelerometer | Magnet + flag

###Temperature 

Lower Threshold | Frequency LSB | Upper Threshold | Frequency MSB
--- | --- | --- | ---
b0-b6 | b7 | b7-b14 | b15




#### Values 

Temperature is in °C , with an offset of 20.  

 Value  | Interpretation
 --- | ---
 `0x00` | -20°C
`0xFF` | 107°C



#### Period :
 Value  | Interpretation
 --- | ---
 `0b00` | 10min
 `0b01` | 1h
 `0b10` | 6h
 `0b11` | 24h


#### Default value

Default value is `0x00FF` 

### Light

Lower Threshold | Upper Threshold
--- | ---
b0-b7 | b8-15

#### Values

The first 5 bits are the value, the last two being the multiplying factor to apply

| Value | Multiplier |	
| ----- | ---- |
| b0 - b5 | b6 - b7 |

###### Multiplier :
Value  | Interpretation
--- | --- | 
`0b00` | 1
`0b01` | 8
`0b10` | 64
`0b11` | 2014


#### Default value

Default value is `0x008F` (0 lux & 9.6 lux as thresholds)


### Accelerometer 



Value  | Interpretation
--- | ---
`0x100373` | Very little sensitivity
`0x080273` | Not very sensitive
`0x040273`| Standard
`0x030173`| Sensitive
`0x010173`| Very sensitive
#### Default value

Default value is `0x040173` (normal)

### Magnet (7 bits)

Value  | Interpretation
`0x16` | Not very sensitive
`0x32`| Standard
`0x50`| Sensitive
#### Default value

Default value is `0x16`: not very sensitive

### Flag (Duty cycle)

If flag is set to `0`, the device won't limit itself regarding the number of messages sent each hour