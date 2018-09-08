# ESP32_BLE_client_uart
### Central mode (client) BLE UART for ESP32

This sketch is a central mode (client) Nordic UART Service that connects automatically to a peripheral (server) Nordic UART Service. NUS is what most typical "blueart" servers emulate. This sketch will connect to your BLE uart device in the same manner the nRF Connect app does.

Once connected, this sketch will switch notification on using BLE2902 for the charUUID_TX characteristic which is the characteristic that our server is making data available on via notification. The data received from the server characteristic charUUID_TX will be printed to Serial on this device. Every five seconds this device will send the string "Time since boot: #" to the server characteristic charUUID_RX, this will make that data available in the BLE uart and trigger a notifyCallback or similar depending on your BLE uart server setup.


A brief explanation of BLE client/server actions and rolls:

##### Central Mode (client) - Connects to a peripheral (server).
* Scans for devices and reads service UUID.
* Connects to a server's address with the desired service UUID.
* Checks for and makes a reference to one or more characteristic UUID in the current service.
* The client can send data to the server by writing to this RX Characteristic.
* If the client has enabled notifications for the TX characteristic, the server can send data to the client as notifications to that characteristic. This will trigger the notifyCallback function.

##### Peripheral (server) - Accepts connections from a central mode device (client).
* Advertises a service UUID.
* Creates one or more characteristic for the advertised service UUID
* Accepts connections from a client.
* The server can send data to the client by writing to this TX Characteristic.
* If the server has enabled notifications for the RX characteristic, the client can send data to the server as notifications to that characteristic. This the default function on most "Nordic UART Service" BLE uart sketches.


### Testing
Pictured below is the demo setup I tested this code on. The server is an nRF52 bluefruit feather with various sensors and an OLED. The client is a cheap ESP32 board with integrated OLED "Heltec WiFi Kit 32". Two way communication between both devices works as intended.

![demo](https://github.com/ThingEngineer/ESP32_BLE_client_uart/raw/master/images/demo.jpg)

### Issues with BLE_uart
I also tried programing another ESP board "ESP32 Dev Module V1.0" with the BLE_uart sketch but this client will not connect to it. As you can see in the photos below, BLE_uart does not appear to be properly advertising the service UUID. You can see the device and it does work once you connect with the app but the client does not see the service UUID advertised in the scan so it never attempts a connection. If someone wants to tackle this I'd be glad to test it again with the fix.

I also noticed that none of the other BLE_* server sketches advertised a serviceUUID that is visible in a scan.

![nRF Connect Scan](https://github.com/ThingEngineer/ESP32_BLE_client_uart/raw/master/images/nrf_connect_scan.png)

![BLE Client Scan](https://github.com/ThingEngineer/ESP32_BLE_client_uart/raw/master/images/client_scan.png)

Based on the "BLE_Client" example by Neil Kolban:
https://github.com/nkolban/esp32-snippets/blob/master/cpp_utils/tests/BLETests/Arduino/BLE_client/BLE_client.ino
With help from an example by Andreas Spiess:
https://github.com/SensorsIot/Bluetooth-BLE-on-Arduino-IDE/blob/master/Polar_Receiver/Polar_Receiver.ino
The BLE_uart sketch that does not advertise serviceUUID properly:
https://github.com/nkolban/ESP32_BLE_Arduino/blob/master/examples/BLE_uart/BLE_uart.ino
Nordic UART Service info:
https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk5.v14.0.0%2Fble_sdk_app_nus_eval.html
