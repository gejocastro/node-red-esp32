# node-red-esp32
Project esp32 temperature / humidity control prototype

Supervised controller
Hardware: ESP32 LOLIN32, protoboard, power source, potentiometer, resistors, diodes, cables, etc
Software: mqtt_node_red2.ino, Arduino IDE

MQTT broker: Mosquito at 3.225.83.240:1883

Supervisor software: Node-red at 3.225.83.240:1880 (AWS EC2 hosted)


Esp32 controller simulates random values of temperature (range 20-35) and humidity(range 70-100), and reads analog signal modified by hardware potentiometer.
Also subscribes two input messages: _**led_control**_ and _**retrieve**_. Analog (scaled into range 0-4905 by node-red) sets PWM value for yellow led (0-4095 is the 12 bit range of values for analog output). Update analog value button, when pressed in node-red ui, sends esp32/retrieve value 1, so controller sends all data to mqtt. Every 15 seconds, esp32 publishes temperature and humidity to mqtt, and set _retrieve_ variable in esp to 1, resulting in analog value publishing too.
Esp32 listens too to _output_ variable 

