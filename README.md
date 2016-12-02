<h2><strong>ESP8266 CoAP Client</strong></h2>

This project provides a Web Server Framework that supports 3 simultaneous protocols:

1. http
2. MQTT
3. CoAP

Caution: It has been observed that running these 3 protocols simultaneously pushes the limits
of the ESP8266. Perhaps beyond stable limits as unwanted module resets have been observed with
all 3 protocols activated. It is recommended that only 2 of the 3 protocols be enabled at the 
same time. For the purpose of this CoAP client project, it is recommended to disable the mqtt protocol.


Setup:<br>

1.0 Copy the http_coap_mqtt_client folder to your Arduino sketch folder.<br>
2.0 Copy the UtilityFunctions folder to your Arduino libraries folder.<br>
3.0 Copy the coap folder to your Arduino libraries folder.<br>
4.0 Copy the webserver folder to your Arduino libraries folder.<br>
5.0 Change the following in the http_coap_mqtt_client.ino file to match your network settings:<br>

const char* ssid          = "YOURWIFISSID";<br>
const char* password      = "YOURWIFIPASSWORD";<br>
const char* domaincoapsvr = "YOURCOAPSERVERDOMAIN";<br>

const IPAddress  ipadd(192,168,0,141);  //This ESP8266 IP<br>   
const IPAddress  ipgat(192,168,0,1);    //Local router IP<br>      

6.0 Server Setting<br>

Change the following in the sketch.h file per your preference<br>

define SVR_TYPE SVR_HTTP_SDK      //Use SDK HTTP Server<br>
define MQTT_SVR_ENABLE 0          //Disable MQTT Server<br>
define EXTERNAL_COAP_SVR 1        //Use external domain for coap server if 1, local server if 0<br>

define SERBAUD 115200<br>
define SVRPORT 9706               //This local ESP http server port<br>

Option 1:<br> 
If coap server set to #define EXTERNAL_COAP_SVR 0, then<br> 
void SetCoapSvrIP(void) {<br>
    ipcoapsvr[0]  = 192;<br>
    ipcoapsvr[1]  = 168;<br>
    ipcoapsvr[2]  = 0;<br>
    ipcoapsvr[3]  = 140;<br>
in http_coap_mqtt_client.ino must be set to the local coap server IP<br>

Option 2:<br> 
If coap server set to #define EXTERNAL_COAP_SVR 1, then<br> 
const char* domaincoapsvr = "YOURCOAPSERVERDOMAIN";<br>
must be set to a valid external coap server domain name<br>

Operation:

While not necessary, the code assumes an LED is connected to GPIO16. This LED is ON upon 
power-up and is turned OFF once initialization completes.

Compile and install this CoAP client on an ESP8266.<br> 
Start the ESP8266 CoAP client as well as your CoAP Server.

CoAP Client test (Assuming client IP set to 192.168.0.141 and port set to 9706, adjust per your setup):

<strong>Test Case 1.1: Set CoAP Server LED Off</strong><br>
192.168.0.141:9706/?request=CoAPLedOff<br>
Http Server Reply: http GET 'CoAPGetReply' required to get CoAP reply<br>
<strong>Test Case 1.2: Set CoAP Server LED Off</strong><br>
192.168.0.141:9706/?request=CoAPGetReply<br>
CoAP Server Reply: 0<br>
<strong>Test Case 2.1: Set CoAP Server LED O</strong>n<br>
192.168.0.141:9706/?request=CoAPLedOff<br>
Http Server Reply: http GET 'CoAPGetReply' required to get CoAP reply<br>
<strong>Test Case 2.2: Set CoAP Server LED On</strong><br>
192.168.0.141:9706/?request=CoAPGetReply<br>
CoAP Server Reply: 1<br>
<strong>Test Case 3.1: Set CoAP Server LED Blinking 3 times</strong><br>
192.168.0.141:9706/?request=CoAPLedBlink&cnt=3<br>
Http Server Reply: http GET 'CoAPGetReply' required to get CoAP reply<br>
<strong>Test Case 4.1: Get CoAP Server Sensor Values</strong><br>
192.168.0.141:9706/?request=CoAPGetSensors<br>
Http Server Reply: http GET 'CoAPGetReply' r<br>equired to get CoAP reply<br>
<strong>Test Case 4.2: Get CoAP Server Sensor Values</strong><br>
192.168.0.141:9706/?request=CoAPGetReply<br><br>
CoAP Server Reply:<br>

A JSON string is returned with the sensor values in this format:<br>

{<br>
"Ain0":"316.00",<br>
"Ain1":"326.00",<br>
"Ain2":"325.00",<br>
"Ain3":"314.00",<br>
"Ain4":"316.00",<br>
"Ain5":"163.00",<br>
"Ain6":"208.00",<br>
"Ain7":"333.00",<br>
"SYS_Heap":"25408",<br>
"SYS_Time":"26"<br>
}<br>

