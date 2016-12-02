<h2><strong>ESP8266 CoAP Client</strong></h2>

This project provides a Web Server Framework that supports 3 simultaneous protocols:

1. http
2. MQTT
3. CoAP

Caution: It has been observed that running these 3 protocols simultaneously pushes the limits
of the ESP8266. Perhaps beyond stable limits as unwanted module resets have been observed with
all 3 protocols activated. It is recommended that only 2 of the 3 protocols be enabled at the 
same time. For the purpose of this CoAP client project, it is recommended to disable the mqtt protocol.


Setup:

1.0 Copy the http_coap_mqtt_client folder to your Arduino sketch folder.
2.0 Copy the UtilityFunctions folder to your Arduino libraries folder.
3.0 Copy the coap folder to your Arduino libraries folder.
4.0 Copy the webserver folder to your Arduino libraries folder.
5.0 Change the following in the http_coap_mqtt_client.ino file to match your network settings:

const char* ssid          = "YOURWIFISSID";
const char* password      = "YOURWIFIPASSWORD";
const char* domaincoapsvr = "YOURCOAPSERVERDOMAIN";

const IPAddress  ipadd(192,168,0,141);  //This ESP8266 IP   
const IPAddress  ipgat(192,168,0,1);    //Local router IP      

6.0 Server Setting

Change the following in the sketch.h file per your preference

define SVR_TYPE SVR_HTTP_SDK      //Use SDK HTTP Server
define MQTT_SVR_ENABLE 0          //Disable MQTT Server
define EXTERNAL_COAP_SVR 1        //Use external domain for coap server if 1, local server if 0

define SERBAUD 115200
define SVRPORT 9706               //This local ESP http server port

Option 1: 
If coap server set to #define EXTERNAL_COAP_SVR 0, then 
void SetCoapSvrIP(void) {
    ipcoapsvr[0]  = 192;
    ipcoapsvr[1]  = 168;
    ipcoapsvr[2]  = 0;
    ipcoapsvr[3]  = 140;
in http_coap_mqtt_client.ino must be set to the local coap server IP

Option 2: 
If coap server set to #define EXTERNAL_COAP_SVR 1, then 
const char* domaincoapsvr = "YOURCOAPSERVERDOMAIN";
must be set to a valid external coap server domain name

Operation:

While not necessary, the code assumes an LED is connected to GPIO16. This LED is ON upon 
power-up and is turned OFF once initialization completes.

Compile and install this CoAP client on an ESP8266. 
Start the ESP8266 CoAP client as well as your CoAP Server.

CoAP Client test (Assuming client IP set to 192.168.0.141 and port set to 9706, adjust per your setup):


Test Case                             URL (192.168.0.141:9706) Suffix	http Reply
-----------------------               -------------------------------   --------------------------------------------------
Set CoAP Server LED Off	              /?request=CoAPLedOff	            http GET 'CoAPGetReply' required to get CoAP reply
Set CoAP Server LED Off	              /?request=CoAPGetReply	        0
Set CoAP Server LED On	              /?request=CoAPLedOn	            http GET 'CoAPGetReply' required to get CoAP reply
Set CoAP Server LED On	              /?request=CoAPGetReply	        1
Set CoAP Server LED Blinking 3 times  /?request=CoAPLedBlink&cnt=3	    http GET 'CoAPGetReply' required to get CoAP reply
Get CoAP Server Sensor Values	      /?request=CoAPGetSensors	        http GET 'CoAPGetReply' required to get CoAP reply
Get CoAP Server Sensor Values	      /?request=CoAPGetReply	        JSON String (See below)


A JSON string will be returned with the sensor values in this format:

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

