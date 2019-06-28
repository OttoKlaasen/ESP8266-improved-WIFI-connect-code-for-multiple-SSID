# ESP3266-improved-WIFI-connect-code-for-multiple-SSID
ESP3266-improved-WIFI-connect-code-for-multiple-SSID, example shown is for two SSID, if first fails than second is tried.

Of course the code can be extended to have more than two SSID's , feel free to play with this.
This example code is only published to show how to connect to multiple SSID's.
Feel free to modify and improve.

//  #include <Arduino.h> in case you use PlatformIO IDE

// Replace with your network credentials
const char* ssid     = "YourSSID";
const char* password = "12345678";
// Alternative ssid office. make sure not to publish this in public domein.
const char* ssid_p     = "Your second SSID";
const char* password_p = "12345678";
const char* ipport = "80";
String MyLocalIp;  // A variable to save IP address

// Set web server port number to 80
WiFiServer server(80);

void setup()  
{
  Serial.begin(115200);
// The following code select a static or dynamic IP address, select in the if stament 1 for static and 0 for dynamic
// Connect to Wi-Fi network with SSID and password
      Serial.print("Connecting to ");
      Serial.println(ssid);
      WiFi.begin(ssid, password);
      int WifiCounter = 0;
      while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
        WifiCounter++;
        if (WifiCounter >=20) {
            Serial.print("Failed to connect to ");
            Serial.println(ssid);
            Serial.print("Connecting to ");
            Serial.println(ssid_p);
            WiFi.begin(ssid_p, password_p);
            WifiCounter = 0;
            while (WiFi.status() != WL_CONNECTED) {
                  delay(500);
                  Serial.print(".");
                  WifiCounter++;
                  if (WifiCounter >=20) {
                       Serial.print("Failed to connect to ");
                       Serial.println(ssid_p);
                       WifiCounter = 0;
                       Serial.print("Connecting to ");
                       Serial.println(ssid);
                       WiFi.begin(ssid, password);
                       break;
                  }
            }
         }
      }
      // Print local IP address and start web server
      Serial.println("");
      Serial.println("WiFi connected.");
      Serial.print("DHCP IP address: ");
      Serial.println(WiFi.localIP());

      if (0) {    // Select 1 for static IP, select 0 for dynamic IP, this is needed for the Philips WLAN-PUB
                  // config static IP, in this program the IP address is set in two places.
          IPAddress ip(192, 168, 1, 101); // where xx is the desired IP Address
          IPAddress gateway(192, 168, 1, 1); // set gateway to match your network
          Serial.print(F("Setting IP Address to static ip: "));
          IPAddress subnet(255, 255, 255, 0); // set subnet mask to match your network
          Serial.println(ip);
          WiFi.config(ip, gateway, subnet);
          MyLocalIp = WiFi.localIP().toString();  // Set the current IP adress to a local variable of type String
          Serial.print("Local Ip set to IP address: ");
          Serial.println(MyLocalIp);
      }
      server.begin();
      }

void loop()
{
// Your code ............
}
