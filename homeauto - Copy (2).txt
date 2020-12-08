#include <ESP8266WiFi.h>
#include <FirebaseArduino.h>
#define FIREBASE_HOST "home-automation-trojo.firebaseio.com"
#define FIREBASE_AUTH "6HiIX5K8CHWq73Ed8nXw8GnJo2IfAxr8dKvQM7JO"
#define WIFI_SSID "trojan"
#define WIFI_PASSWORD "sk6@1998"
int s1;
int s2;
int s3;
int spd;
 
void setup() {
  // put your setup code here, to run once:
Serial.begin(9600);
pinMode(D7,OUTPUT);
pinMode(D5,OUTPUT);
pinMode(D3,OUTPUT);
pinMode(D4,OUTPUT);
pinMode(D6,OUTPUT);
WiFi.begin(WIFI_SSID,WIFI_PASSWORD);
Serial.print("Connecting");
while(WiFi.status()!=WL_CONNECTED)
{
  Serial.print(".");
  delay(50);
}
Serial.println();
Serial.print("Connected:");
Serial.println(WiFi.localIP());
Firebase.begin(FIREBASE_HOST,FIREBASE_AUTH);
}
void firebaseconnect()
{
  Serial.println("CONNECTING");
Firebase.begin(FIREBASE_HOST,FIREBASE_AUTH);
//Firebase.set("switch1",0);
}

void loop() {
  // put your main code here, to run repeatedly:

  if(Firebase.failed())
  {
    Serial.print("Failed:");
    Serial.println(Firebase.error());
    firebaseconnect();
    return;
  }
  s1=Firebase.getInt("switch/switch1");
  s3=Firebase.getInt("switch/switch3");
  s2=Firebase.getInt("switch/switch2");
  spd=Firebase.getInt("fan/speed");
  
   if (Firebase.failed()) {
      Serial.print("setting /message failed:");
      Serial.println(Firebase.error());  
       firebaseconnect();
      return;
  }
  
  if(s1==1){
    Serial.println(s1);
    Serial.println("LED is ON");
    digitalWrite(D5,LOW);
  }
 else{
    Serial.println(s1);
    Serial.println("LED is OFF");
    digitalWrite(D5,HIGH);
  }
  if(s2==1){
    Serial.println(s2);
    Serial.println("Fan is ON");
    digitalWrite(D4,HIGH);
    analogWrite(D6,spd);
    analogWrite(D7,LOW);
  }
  else{
    Serial.println(s2);
    Serial.println("Fan is OFF");
    digitalWrite(D4,LOW);
  }
  
  
  if(s3==1){
    Serial.println(s3);
    Serial.println("Charger is ON");
    digitalWrite(D3,LOW);
  }
 else{
    Serial.println(s3);
    Serial.println("Charger is OFF");
    digitalWrite(D3,HIGH);
  }
  
}
