# node-red-esp32
Project esp32 temperature / humidity control prototype

Supervised controller
Hardware: ESP32 LOLIN32, protoboard, power source, potentiometer, resistors, diodes, cables, etc
Software: mqtt_node_red2.ino, Arduino IDE

MQTT broker: Mosquito at 3.225.83.240:1883

Supervisor software: Node-red at 3.225.83.240:1880 (AWS EC2 hosted)


Esp32 controller simulates random values of temperature (range 20-35) and humidity(range 70-100), and reads analog signal modified by hardware potentiometer.
Also subscribes two input messages: _**led_control**_ and _**retrieve**_. Led control (scaled into range 0-255 by ESP) sets PWM value for yellow led (0-255 is the range of values accepted by led_cWrite function (Wire library). Update analog value button, when pressed in node-red ui, sends esp32/retrieve value 1, so controller sends all data to mqtt. Every 15 seconds, esp32 publishes temperature and humidity to mqtt, and set _retrieve_ variable in esp to 1, resulting in analog value publishing too.

Esp32 listens too to _esp32/output_ mqtt message, and sets digital output to 1 or 0, depending on the value of output (setted on or off node-red)

Node-red supervisor has several flows involving retrieving and sending mqtt messages, and represents in different ui elements in the dashboard. 


UI description
7 tabs implemented:
1. Principal (Main): it contents links to othes 6 tabs. 
2. ESP32: A dashboard with 3 groups of elements:
- Group 1, Comandos, shows switches and buttons to operate green led, a status indicator for the green led being on or off, a sliding 0-100% controller to set pwm value for yellow led. 
- Group 2, Temperatura, contains a INdicator gauge for temperature, and a chart. 
- Gruup 3, same as G2, but for humidity
3. Tablero general: a dashboard showing node-red side simulated electric variables (Voltage, Current and Power factor) and analog value control) in 4 groups :
- Grupo 1, PArametros electricos, Voljage, Current and Power factor, in 3 coloured gauges.
- Goup 2, Variable analogica: a gauge showing analog value hardware setted by potentiometer in esp32, a button for retrieving analog value from esp when pressed (in spite of 15s timer). A sliding switch for controlling sonoff device
- Grupo 3: a bargraph with the 3 electrical variables, and a multi line graph showing evolution in time (last minute) of these variables. 
- Gruup 4, Tendencias (Trends), 3 single line trends, 1 for each variable, with a 1 hour window. 
4. Informacion de secuencia (Sequence data), An input window with increment-decrement arrows, to introduce numeric values. A cicling function increments value by one, restarting in 0 after 6, and a parsing function returns a diferent operation mode corresponding to this number. 
5. Logfile: 
- Group 1, shows a list with filters of the daily data record files saved in server log folder. Clicking each, loads selected day log in table in group two.
- Group 2, historical file data table, with order_by each column, and scroll bars. 
- Group 3, retrieves and shows data log from google sheet in a filtered table (Timestamp, Temperature, Humidity, Analog, green led status

# NR Modules 

node-red | 1.3.3
node-red-contrib-ewelink | 2.0.0
node-red-contrib-git-nodes
0.2.2
node-red-contrib-git-ui
0.1.10
node-red-contrib-google-sheets
1.1.2
node-red-contrib-machine-learning-v2
1.0.9
node-red-contrib-play-audio
2.5.0
node-red-contrib-ui-clock
1.0.2
node-red-contrib-ui-led
0.4.11
node-red-contrib-ui-level
0.1.46
node-red-contrib-xstate-machine
1.2.4
node-red-dashboard
3.1.7
node-red-node-email
1.15.1
node-red-node-ping
0.3.1
node-red-node-random
0.4.0
node-red-node-rbe
0.5.0
node-red-node-serialport
1.0.1
node-red-node-smooth
0.1.2
node-red-node-tail
0.3.2
node-red-node-ui-table



