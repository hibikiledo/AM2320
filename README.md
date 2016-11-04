# AM2320
Arduino library for AM2320 temperature and humidity sensor.

## Downloads

[Latest Version](https://github.com/hibikiledo/AM2320/releases)

## API Reference

```cpp
AM2320();
```
#### Summary
A Constructor which should be called first to create an instace(variable) of sensor library.

#### Parameters
None

#### Return Value
An instance of AM2320 class.

#### Example
```cpp
#include "AM2320.h"

// create a variable of sensor library
AM2320 sensor;
```

---

```cpp
void begin(void);
```
#### Summary
Initialize the AM2320 sensor library.

#### Parameters
None

#### Return Value
None

#### Example
```cpp
#include "AM2320.h"
AM2320 sensor;

void setup() {
  // initialize sensor library
  sensor.begin();
}

void loop() {

}
```
---

```cpp
void begin(int sda, int scl);
```
#### Summary
Initialize the AM2320 sensor library with specified SDA and SCL pins.  
This is useful on ESP-01 where SDA and SCL pins aren't default ones.

#### Parameters
- sda - pin number of sda line
- scl - pin number of scl line

#### Return Value
None

#### Example
```cpp
#include "AM2320.h"
AM2320 sensor;

void setup() {
  // initialize sensor library with SDA and SCL pins
  // https://github.com/esp8266/Arduino/blob/master/doc/libraries.md#i2c-wire-library
  sensor.begin(0, 2);
}

void loop() {

}
```

---

```cpp
bool measureTemperature();
```
#### Summary
Tell the sensor to perform temperature acquisition.

#### Parameters
None

#### Return Value
- true - the operation is successful
- false - an error occurs. use `getErrorCode()` to get an error code. [More info](#error-codes)

---

```cpp
bool measureHumidity();
```
#### Summary
Tell the sensor to perform temperature acquisition.

#### Parameters
None

#### Return Value
- true - the operation is successful
- false - an error occurs. use `getErrorCode()` to get an error code. [More info](#error-codes)

---

```cpp
bool measure();
```
#### Summary
Tell the sensor to perform both temperature and humidity acquisition.

#### Parameters
None

#### Return Value
- true - the operation is successful
- false - an error occurs. use `getErrorCode()` to get an error code. [More info](#error-codes)

---

```cpp
int getTemperature();
```
#### Summary
Return temperature from latest acquisition.

#### Parameters
None

#### Return Value
Integer represent temperature in degree celcius.

---

```cpp
int getHumidity();
```
#### Summary
Return humudity from latest acquisition.

#### Parameters
None

#### Return Value
Integer representing humudity in % RH(Relative Humidity).

---

```cpp
int getErrorCode();
```
#### Summary
Return error code from latest operation.

#### Parameters
None

#### Return Value
Integer representing an error code. [More info](#error-codes)

---

## Error Codes
#### Code 0
This indicates no error. Everything works fine.
#### Code 1
Sensor is not online. This happens when Arduino cannot connect to the sensor. Possible causes may be bad connections, wrong wirings, etc. You may want to use I2CScanner sketch to check if Arduino can find the sensor or not.
#### Code 2
CRC validation failed. This happends when data transmitted from the sensor is received with errors. Possible causes may be bad connections, lengthy cable, etc.

## Usage Example
```cpp
// Include library into the sketch
#include <AM2320.h>

// Create an instance of sensor
AM2320 sensor;

void setup() {
  // enable serial communication
  Serial.begin(115200);
  // call sensor.begin() to initialize the library
  sensor.begin();
}

void loop() {

  // measure both temperature and humidity at once.
  // sensor.measure() returns boolean value
  // - true indicates measurement is completed and success
  // - false indicates that either sensor is not ready or crc validation failed
  //   use getErrorCode() to check for cause of error.
  if (sensor.measure()) {
    Serial.println("Measuring both temperature and humidity at once ..");
    Serial.print("Temperature: ");
    Serial.println(sensor.getTemperature());
    Serial.print("Humidity: ");
    Serial.println(sensor.getHumidity());
  }
  else {  // error has occured
    int errorCode = sensor.getErrorCode();
    switch (errorCode) {
      case 1: Serial.println("ERR: Sensor is not online"); break;
      case 2: Serial.println("ERR: CRC validation failed."); break;
    }    
  }

  // measure only temperature
  // sensor.measureTemerature() returns boolean value
  // - true indicates measurement is completed and success
  // - false indicates that either sensor is not ready or crc validation failed
  //   use getErrorCode() to check for cause of error.
  if (sensor.measureTemperature()) {
      Serial.println("Measuring only temperature ..");
      Serial.print("Temperature: ");
      Serial.println(sensor.getTemperature());
  }
  else {  // error has occured
    int errorCode = sensor.getErrorCode();
    switch (errorCode) {
      case 1: Serial.println("ERR: Sensor is not online"); break;
      case 2: Serial.println("ERR: CRC validation failed."); break;
    }    
  }

  // measure only humidity
  // sensor.measureHumidity() returns boolean value
  // - true indicates measurement is completed and success
  // - false indicates that either sensor is not ready or crc validation failed
  //   use getErrorCode() to check for cause of error.
  if (sensor.measureHumidity()) {
      Serial.println("Measuring only humidity ..");
      Serial.print("Humidity: ");
      Serial.println(sensor.getHumidity());
  }
  else {  // error has occured
    int errorCode = sensor.getErrorCode();
    switch (errorCode) {
      case 1: Serial.println("ERR: Sensor is not online"); break;
      case 2: Serial.println("ERR: CRC validation failed."); break;
    }    
  }
  
  delay(1000);

}
```

## The making of AM2320 library

[Part 1](https://hibikiledo.xyz/2016/11/04/writing-am2320-arduino-library-1/)
Part 2 .. working on it
