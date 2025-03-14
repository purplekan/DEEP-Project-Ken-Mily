```cpp
/**
 * @file stm32_rfid_audio.c
 * @brief Scanner RFID utilisant le MFRC522 et STM32
 * @author ChatGPT
 * @date 2025
 */

#include "stm32f4xx_hal.h"
#include "mfrc522.h"
#include "usart.h"

#define RFID_SPI hspi1 /**< SPI utilisé pour le module RFID */

uint8_t rfid_uid[10]; /**< Tableau pour stocker l'UID du badge scanné */

/**
 * @brief Configure l'horloge système.
 */
void SystemClock_Config(void);

/**
 * @brief Gère les erreurs en faisant clignoter une LED.
 */
void Error_Handler(void);

/**
 * @brief Point d'entrée principal du programme.
 * @return int (non utilisé dans les systèmes embarqués)
 */
int main(void) {
    // Initialisation de la bibliothèque HAL
    HAL_Init();
    
    // Configuration de l'horloge système
    SystemClock_Config();

    // Initialisation des périphériques
    MX_GPIO_Init();
    MX_SPI1_Init();
    MX_USART2_UART_Init();

    // Initialisation du module RFID
    MFRC522_Init();
    printf("\n[INFO] Scanner RFID prêt. Approchez un badge...\n");

    while (1) {
        /**
         * Vérifie si un badge RFID est détecté
         */
        if (MFRC522_Check(rfid_uid) == MI_OK) {
            printf("Badge détecté ! UID : ");
            for (int i = 0; i < 4; i++) {
                printf("%02X ", rfid_uid[i]);
            }
            printf("\n");
            HAL_Delay(1000); /**< Pause après la lecture */
        }
    }
}

/**
 * @brief Configure l'horloge système.
 * Cette fonction est généralement générée par CubeMX.
 */
void SystemClock_Config(void) {
    // Configuration gérée par CubeMX
}

/**
 * @brief Gère les erreurs en faisant clignoter une LED.
 */
void Error_Handler(void) {
    while (1) {
        HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
        HAL_Delay(500);
    }
}
```
