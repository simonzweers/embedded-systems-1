# Week 1

## Notities

CPU: Central Processing Unit. Voert alle instructies in een computer uit. Ook wel CVE (Cetrale VerwerkingsEenheid) genoemd.
Digitale gegevens:

- Eenvoudig op te slaan. Digitale gegevens kunnen op allerlei manieren zonder verlies opgeslagen worden voor archivering of latere bewerking.

- Complexe bewerkingen zijn mogelijk. Met een combinatie van veel eenvoudige instructies kunnen ingewikkelde bewerkingen op de gegevens worden uitgevoerd.

- Geen effecten van temperatuurdrift of componentveroudering, zoals die in analoge
technologie een rol speelt. De bewerkingen op digitale gegevens zijn op voorhand te analyseren en geven altijd dezelfde uitkomst.

- Verliesvrij transport over grote afstanden.

ADC (Analo-Digital Converter): Een analoog signaal word omgezet in een digitaal getal.
![adc_graph](../assets/adc_graph.png)

DAC (Digital-Analog Converter): Een digitaal

## Oefen Project Blinkie

### Een korte beschrijving van het project

Blinkie word in de embedded-systems wereld vaak gebruikt to introductie tot het vak. Het is een beetje de "Hello World!" van het embedded programmeren.

Het doel van het project was om een LED met een constant interval te laten knipperen.

Hier is het aansluitschema schematisch weergegeven:
![fritzing-bb](../assets/blinkie/blinkie-schema_bb.png)

Een foto van hoe je fysieke opstelling eruit ziet:
![photo](../assets/blinkie/blinkie_photo.jpg)

Als Laatst: [De code](./blinkie/src/main.c):

```c
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"
  
#define LED_PIN 21
#define HIGH 1
#define LOW 0


  
void app_main() {
    // Setup
    int delay_ms = 1500;
    int count = 10;
    gpio_set_direction(LED_PIN, GPIO_MODE_OUTPUT);
    
    // Main loop voor het blinken
    for (size_t i = 0; i < count; i++)
    
    {
        // Zet led pin aan en delayt
        gpio_set_level(LED_PIN, HIGH);
        printf("Blinkie AAN\n");
        vTaskDelay(delay_ms / portTICK_PERIOD_MS); // 1000 milliseconde = 1 seconde
        
        // Zet led pin uit en delayt
        gpio_set_level(LED_PIN, LOW);
        printf("Blinkie UIT\n");
        vTaskDelay(delay_ms / portTICK_PERIOD_MS);

    }
    
        
} 
```

## Project Hartslag

### Een korte beschrijiving van het project

Het doel van het project is om een led te laten blinken in het patroon van een hartslag. Dit is een geavanceerdere versie van het oefenproject "Blinkie"