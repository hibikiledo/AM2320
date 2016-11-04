# AM2320
Arduino library for AM2320 temperature and humidity sensor.

## Downloads

[Latest Version](https://github.com/hibikiledo/AM2320/releases)

## API Reference

```cpp
AM2320()
```
Constructor does not accept any parameter.

```cpp
void begin(void);
```
Initialize the AM2320 sensor library.

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
- false - an error occurs. use `getErrorCode()` to get an error code. [More info](#what-is-me)

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
- false - an error occurs. use `getErrorCode()` to get an error code. More info


## Error Codes
- 0 - No Error
- 1 - Sensor is not online
- 2 - CRC validation failed
