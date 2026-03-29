<img width="1872" height="965" alt="image_2026-03-29_174453127" src="https://github.com/user-attachments/assets/513e93f4-33bf-4776-a4fb-a3d5284c7e68" />
# serre-connectee
Projet de serre intelligente utilisant des capteurs et de l’automatisation.


# Serre connectée

## Description du projet

Ce projet consiste à concevoir une serre intelligente capable de surveiller les conditions de culture grâce à des capteurs.

L'objectif est d'améliorer la gestion des plantes en automatisant certaines actions.

## Objectifs du projet

- Mesurer la température et l’humidité
- Surveiller les conditions de culture
- Automatiser certaines actions

## Technologies et concepts étudiés

- Capteurs environnementaux
- Microcontrôleurs
- Automatisation
- Systèmes connectés (IoT)

## Ce que j’ai appris

Ce projet m’a permis de découvrir le fonctionnement des capteurs et la manière dont ils peuvent être utilisés pour surveiller un environnement.

[cahier des charge -htmlcode-.pdf](https://github.com/user-attachments/files/25782028/cahier.des.charge.-htmlcode-.pdf)


[recap des composant -htmlcode-.pdf](https://github.com/user-attachments/files/25782008/recap.des.composant.-htmlcode-.pdf)


[htmlCode.pdf](https://github.com/user-attachments/files/25781999/htmlCode.pdf)


<img width="1255" height="823" alt="image_2026-03-06_010634826" src="https://github.com/user-attachments/assets/1aa7ca6d-2692-4986-812e-f19c995e394f" />

```cpp
#include <Servo.h>

const int LED_BLEUE = 2;   
const int LED_VERTE = 3;   
const int LED_JAUNE = 4;   
const int LED_ROUGE = 5;   
const int MOTEUR_VENTILO = 9;      
const int MOTEUR_POMPE = 8; 
const int BUZZER = 13;     
const int PIN_SERVO = A3;  

const int PIN_TEMPERATURE = A0;
const int PIN_PHOTORESISTANCE = A1;
const int PIN_HUMIDITE = A2;
const int PIN_GAZ = A4;

const float TEMP_CRITIQUE = 30.0;
const int HUMIDITE_MIN = 30; 
const int LDR_SOLEIL = 600;
const int GAZ_EXCELLENT = 150; 
const int GAZ_MAUVAIS = 300;   

Servo monServo;

void setup() {
  Serial.begin(9600);
  pinMode(LED_BLEUE, OUTPUT);
  pinMode(LED_VERTE, OUTPUT);
  pinMode(LED_JAUNE, OUTPUT);
  pinMode(LED_ROUGE, OUTPUT);
  pinMode(MOTEUR_VENTILO, OUTPUT);
  pinMode(MOTEUR_POMPE, OUTPUT);
  pinMode(BUZZER, OUTPUT);
  monServo.attach(PIN_SERVO);
  monServo.write(0);
}

void loop() {
 
  float tempC = (analogRead(PIN_TEMPERATURE) * (5.0 / 1023.0) - 0.5) * 100.0;
  
 
  int hum = map(analogRead(PIN_HUMIDITE), 0, 1023, 0, 100); 
  
  int lux = analogRead(PIN_PHOTORESISTANCE);
  int gaz = analogRead(PIN_GAZ);

  
  if (hum < HUMIDITE_MIN) { 
    digitalWrite(MOTEUR_POMPE, HIGH);
    digitalWrite(BUZZER, HIGH);
  } else {
    digitalWrite(MOTEUR_POMPE, LOW);
    digitalWrite(BUZZER, LOW);
  }

 
  digitalWrite(LED_VERTE, LOW);
  digitalWrite(LED_JAUNE, LOW);
  digitalWrite(LED_ROUGE, LOW);
  if (gaz < GAZ_EXCELLENT) digitalWrite(LED_VERTE, HIGH);
  else if (gaz < GAZ_MAUVAIS) digitalWrite(LED_JAUNE, HIGH);
  else digitalWrite(LED_ROUGE, HIGH);

  
  if (gaz >= GAZ_MAUVAIS || tempC > TEMP_CRITIQUE) digitalWrite(MOTEUR_VENTILO, HIGH);
  else digitalWrite(MOTEUR_VENTILO, LOW);

  if (lux > LDR_SOLEIL) monServo.write(180);
  else monServo.write(0);

  
  if (tempC <= TEMP_CRITIQUE && hum >= HUMIDITE_MIN && gaz < GAZ_MAUVAIS) {
    digitalWrite(LED_BLEUE, HIGH);
  } else {
    digitalWrite(LED_BLEUE, LOW);
  }

  
  Serial.print("Humidite reelle : "); 
  Serial.print(hum); 
  Serial.print("% | Pompe : ");
  if (hum < HUMIDITE_MIN) Serial.println("MARCHE");
  else Serial.println("ARRET");

  delay(500);
}

```


https://www.tinkercad.com/things/4Nm7G9fcjOB-serre-connecter-v10?sharecode=1ial8jLRF2aqocEwZPg-TSnNkD8yON8lZEM-uT4cnJA
