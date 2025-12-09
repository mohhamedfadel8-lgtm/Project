/* ESP32 WiFi Scanning example */

#include "WiFi.h"

int wifi_init(void);

char ssid[] = "Wokwi_GUEST";
char pass[] = "";
unsigned long timestart=0;
unsigned long timeout=0;
void setup()
{
Serial.begin(115200);
wifi_init();
timestart=millis();

 
}
void loop()
{
if(millis()-timestart>2000 && WiFi.status() != WL_CONNECTED)
{
  Serial.print("Reconnecting to wifi...");
  WiFi.disconnect();
 wifi_init();
 WiFi.reconnect();
  timestart=millis();
}
}

int wifi_init(void)
{
   Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid,pass);
  Serial.print("Connecting");
  IPAddress local_IP(192,168,1,180);
  IPAddress gateway(192,168,1,180);
  IPAddress PrimaryDNS (8,8,8,8);
  WiFi.config(local_IP,gateway,PrimaryDNS);
  timeout=millis();
  while(WiFi.status() !=WL_CONNECTED && millis()-timeout <10000)
  {
delay(500);
Serial.print(".");
  }
  if (WiFi.status() != WL_CONNECTED)
  {
    Serial.println("I cannot connect to wifi...");
    return 0;
  }
  else
  {
Serial.println("Connected ..Your IP is:");
  Serial.println(WiFi.localIP());
  Serial.print("Strength : ");
  Serial.print(WiFi.RSSI());
  return 1;
  }
  
}

void wifi_scan(void)
{
  WiFi.mode(WIFI_STA);
  Serial.println("Scanning....................");
  int number = WiFi.scanNetworks();
  Serial.println("Scan Done");
  if(number == 0)
  {
    Serial.println("No Network Found");
  }
  else
  {
    Serial.print("Network Found : ");
Serial.println(number);
for(int i = 0 ; i<number ; i++)
{
  Serial.print(i+1);
  Serial.print(":");
  Serial.print(WiFi.SSID(i));
  Serial.print("       ::         ");
  Serial.print(WiFi.RSSI(i));
  if(WiFi.encryptionType(i)==WIFI_AUTH_OPEN)
  {
    Serial.print("  ::  No Password");
  }
  else
  {
    Serial.println("  ::  have password **");
  }

  delay(10);

}
  }
}

