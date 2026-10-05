#include <SPI.h>
#include <LoRa.h>

#define SS 5
#define RST 14
#define DIO0 26
#define BUTTON 27

void setup() {
  Serial.begin(115200);

  pinMode(BUTTON, INPUT_PULLUP);

  SPI.begin(18, 19, 23, SS);
  LoRa.setPins(SS, RST, DIO0);

  if (!LoRa.begin(433E6)) {
    Serial.println("LoRa failed!");
    while (1);
  }

  Serial.println("Tuition Sender Ready");
}

void loop() {

  if (digitalRead(BUTTON) == LOW) {

    Serial.println("Sending PICK ME...");

    LoRa.beginPacket();
    LoRa.print("PICK ME");
    LoRa.endPacket();

    Serial.println("PICK ME Sent!");

    delay(3000);
  }
}
