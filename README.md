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


Based on the "BLE_Client" example by Neil Kolban:
https://github.com/nkolban/esp32-snippets/blob/master/cpp_utils/tests/BLETests/Arduino/BLE_client/BLE_client.ino
With help from an example by Andreas Spiess:
https://github.com/SensorsIot/Bluetooth-BLE-on-Arduino-IDE/blob/master/Polar_Receiver/Polar_Receiver.ino
Nordic UART Service info:
https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk5.v14.0.0%2Fble_sdk_app_nus_eval.html
