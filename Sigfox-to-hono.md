# Sigfox Adapter for Hono
* Adapter Status: POST `http://hono.bosch-iot-suite.com:10050/status`
* Last Device Call: GET `http://hono.bosch-iot-suite.com:10050/status/bcx/deviceId`
* Downlink: GET  `http://hono.bosch-iot-suite.com:10050/dl/bcx/[deviceId]`

## Setting Up Pre-requisites
9. ```
{
"duplicate" : "{duplicate}", 
"snr" : "{snr}", 
"avgSnr" : "{avgSnr}", 
"rssi" : "{rssi}",
 "lat" : "{lat}",
"data" : "{data}"
```

### Uplink







#### Last Device Status


* `/status/err - Last reported/known error/exception encountered by the adapter
## Payload Format
 {
```

### Downlink