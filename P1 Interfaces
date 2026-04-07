#include <WiFi.h>
#include <HTTPClient.h>
#include "DHT.h"

const char* ssid = "JOU2022";
const char* password = "merchan2022";

const char* serverAddress = "https://api.tago.io/data";
const char* tokenHeader = "43aae551-90e8-495c-b56b-8725fbf3a80f";

WiFiClient wifi;
HTTPClient client;

#define DHTPIN 5
#define DHTTYPE DHT22
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(115200);

  WiFi.begin(ssid, password);
  Serial.print("Conectando");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nConectado!");
  Serial.println(WiFi.localIP());

  dht.begin();
}

void loop() {
  delay(2000);

  float h = dht.readHumidity();
  float t = dht.readTemperature();

  if (isnan(h) || isnan(t)) {
    Serial.println("Error en DHT");
    return;
  }

  // Crear JSON
  String postData = "{\"variable\":\"Temperature\",\"value\":" + String(t) + "}";

  client.begin(serverAddress);
  client.addHeader("Content-Type", "application/json");
  client.addHeader("Device-Token", tokenHeader);

  int statusCode = client.POST(postData);

  Serial.print("Codigo: ");
  Serial.println(statusCode);

  client.end();

  delay(15000);
}
