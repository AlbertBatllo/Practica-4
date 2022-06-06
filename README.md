# Practica3: WIFI

En aquesta part de la pràctica utlitzarem el WiFi de la ESP32 per a crear una pàgina web

## Codi

```
#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>

extern String HTML;
void handle_root();

// SSID & Password
const char* ssid = "Xiaomi_11T_Pro"; // Enter your SSID here
const char* password = "f5cbd8a82232"; //Enter your Password here
WebServer server(80);

// Object of WebServer(HTTP port, 80 is defult)
void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);

// Connect to your wi-fi modem
WiFi.begin(ssid, password);
// Check wi-fi is connected to wi-fi network
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}
void loop() {
server.handleClient();
}

String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1> algo ;</h1>\
</body>\
</html>";

void handle_root() {
server.send(200, "text/html", HTML);
}
```

## Explicació del Codi

Per a aquesta pràctica necessitarem dues llibraries, per a connectar-nos al WiFi i per a poder crear i treballar amb un servidor web, aquestes llibraries són 'WiFi.h' i 'WebServer.h':

```
#include <WiFi.h>
#include <WebServer.h>
```

A continuació declararem el nom i la contrasenya del WiFi, seguit de l'objecte del servidor:

```
const char* ssid = "Xiaomi_11T_Pro"; // Enter your SSID here
const char* password = "f5cbd8a82232"; //Enter your Password here
WebServer server(80);
```

**Set Up:**
```
void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);

// Connect to your wi-fi modem
WiFi.begin(ssid, password);
// Check wi-fi is connected to wi-fi network
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}
```

**Loop:**

```
void loop() {
server.handleClient();
}
```

Al final es declararà el contingut que volem mostrar a la pagina Web en format HTML, en el nostre cas nomes mostrarem per pantalla "algo":

```
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1> algo ;</h1>\
</body>\
</html>";
```

Finalment escriurem la funció que envia l'string emès al servidor, la qual hem declarat amb anterioritat:

```
void handle_root() {
server.send(200, "text/html", HTML);
}
```


# Practica 3: Bluetooth

En aquesta part de la pràctica establirem conexió a partir del Bluetooth de la ESP32 amb altres dispositius, de manera que ens podrem enviar i rebre missatges.

## Codi

```
#include <Arduino.h>
#include <BluetoothSerial.h>

#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

BluetoothSerial SerialBT;

void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32test"); //Bluetooth device name
  Serial.println("The device started, now you can pair it with bluetooth!");
}

void loop() {
  if (Serial.available()) {
    SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
    Serial.write(SerialBT.read());
  }
  delay(20);
}
```

## Explicació del Codi

Per a utilitzar el Bluetooth dependrem de la llibreria 'BluetoothSerial.h':

```
#include <BluetoothSerial.h>
```

A continuació declararem l'objecte que ens permetra interactuar amb el dispositiu Bluetooth:

```
BluetoothSerial SerialBT;
```

**Set Up:**

 Inicialitzarem l'objecte i l'anomenarem "ESP32test", el qual serà el nom amb el que altres dispositius connectats al Bluetooth el veuràn:
 
 ```
 void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32test"); //Bluetooth device name
  Serial.println("The device started, now you can pair it with bluetooth!");
}
```

**Loop:**

```
void loop() {
  if (Serial.available()) {
    SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
    Serial.write(SerialBT.read());
  }
  delay(20);
}


