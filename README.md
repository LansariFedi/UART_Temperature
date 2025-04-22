# STM32 DHT11 Temperature & Humidity Monitor

A robust DHT11 temperature and humidity sensor driver implementation for STM32 microcontrollers with UART output. This project uses hardware timer for precise microsecond timing and implements proper error handling for reliable sensor communication.

## Features
- Hardware timer-based microsecond precision timing
- UART output (115200 baud rate) for real-time monitoring
- Comprehensive error handling (timeout, checksum, no response)
- Easy GPIO configuration
- Temperature range: 0-50°C
- Humidity range: 20-95%
- Sampling rate: 1 Hz (configurable)

## Hardware Setup
- Tested on STM32F407G-DISC1 board
- DHT11 temperature and humidity sensor
- Voltage divider for reliable data line operation

### Wiring
```
DHT11 Pin    STM32 Connection
VDD         -> 5V
GND         -> GND
DATA        -> PA8 (with voltage divider) (configurable)
```

### Voltage Divider Configuration
```
    5V
    |
   R1 = 10k
    |
    +-----> STM32 GPIO + DHT11 data
    |
   R2 = 20k
    |
   GND
```

This voltage divider provides ~3.3V for the data line, ensuring reliable communication while protecting the STM32 GPIO.

## Usage

### Driver Initialization
```c
// Initialize with Timer6 and PA8
DHT11_Init(&htim6, GPIOA, GPIO_PIN_8);
```

### Reading Sensor Data
```c
DHT11_Data data;
DHT11_Status status = DHT11_ReadData(&data);

if (status == DHT11_OK) {
    float temperature = DHT11_GetTemperature(&data);
    float humidity = DHT11_GetHumidity(&data);
}
```

### UART Output Format
The sensor readings are output via UART in the following format:
```
Humidity: XX.X%, Temp: YY.YC
```

## Troubleshooting

During development, I encountered reliability issues with the sensor communication. Investigation with a voltmeter revealed that the board's VDD was providing only ~2.9V, insufficient for reliable DHT11 operation which requires minimum 3.3V for the data line.

The solution was implementing a voltage divider to properly level-shift the 5V to 3.3V for the data line while powering the sensor directly from 5V. This significantly improved the communication reliability.

## Hardware Requirements
- STM32F407G-DISC1 board (or compatible)
- DHT11 temperature and humidity sensor
- 2x resistors (10kΩ and 20kΩ) for voltage divider
- USB cable for power and UART communication

## Software Requirements
- STM32CubeIDE
- HAL libraries
- Timer configured for microsecond operation
- UART configured at 115200 baud rate

## Configuration
The driver uses:
- Timer6 for microsecond delays
- UART2 for data output
- PA8 for sensor communication
- 2000ms sampling interval (configurable)

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Authors
- Yassine Gharbi
- Fedi Lansari