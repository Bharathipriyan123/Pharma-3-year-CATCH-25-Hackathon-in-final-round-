# Pharma-3-year-CATCH-25-Hackathon-in-final-round-
https://youtu.be/Itc2p_czm8I?si=qiH05Hcwdk-5WIRO
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

const int heartbeatSensorPin = A0; // Heartbeat sensor pin
const int buzzerPin = 8;           // Buzzer pin

int heartRate = 0;
int threshold = 110; // Set your threshold value for heartbeat alert

void setup() {
    pinMode(heartbeatSensorPin, INPUT);
    pinMode(buzzerPin, OUTPUT);
    
    Serial.begin(9600);
    
    if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
        Serial.println(F("SSD1306 allocation failed"));
        for (;;);
    }
    display.clearDisplay();
}

void loop() {
    int sensorValue = analogRead(heartbeatSensorPin);
    heartRate = map(sensorValue, 0, 1023, 60, 150); // Mapping sensor values to realistic BPM
    
    Serial.print("Heartbeat: ");
    Serial.println(heartRate);
    
    display.clearDisplay();
    display.setTextSize(2);
    display.setTextColor(WHITE);
    display.setCursor(10, 20);
    display.print("BPM: ");
    display.print(heartRate);
    display.display();
    
    if (heartRate > threshold) {
        digitalWrite(buzzerPin, HIGH);
    } else {
        digitalWrite(buzzerPin, LOW);
    }
    
    delay(1000); // Update every second
}
