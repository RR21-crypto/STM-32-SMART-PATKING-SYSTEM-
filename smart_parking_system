#include "stm32f4xx.h" // Device header

// Macros for trigger pins
#define Trig1_high GPIOA->BSRR = GPIO_BSRR_BS_0 // Turn on PA0 (trig pin 1)
#define Trig1_low GPIOA->BSRR = GPIO_BSRR_BR_0  // Turn off PA0 (trig pin 1)
#define Trig2_high GPIOA->BSRR = GPIO_BSRR_BS_2 // Turn on PA2 (trig pin 2)
#define Trig2_low GPIOA->BSRR = GPIO_BSRR_BR_2  // Turn off PA2 (trig pin 2)

uint32_t duration1, duration2;
float distance1, distance2;

// Prototypes of the used functions
void delaymS(uint32_t ms);
void delayuS(uint32_t us);
uint32_t read_echo(uint32_t timeout, uint32_t echo_pin);

int main(void)
 {
    // Enable GPIOA Clock
    RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;

    // Configure PA0 as output for trigger 1
    GPIOA->MODER |= (1 << 0);

    // Configure PA1 as input for echo 1
    GPIOA->MODER &= ~(3 << 2);

    // Configure PA2 as output for trigger 2
    GPIOA->MODER |= (1 << 4);

    // Configure PA3 as input for echo 2
    GPIOA->MODER &= ~(3 << 6);

    // Configure PA5 as output for LED 1
    GPIOA->MODER |= (1 << 10);  // Set PA5 to Output
    GPIOA->BSRR = GPIO_BSRR_BS_5; // Turn on LED 1 (PA5 High)

    // Configure PA6 as output for Buzzer
    GPIOA->MODER |= (1 << 12);  // Set PA6 to Output
    GPIOA->BSRR = GPIO_BSRR_BR_6; // Ensure Buzzer is off (PA6 Low)

    // Configure PA7 as output for LED 2
    GPIOA->MODER |= (1 << 14); // Set PA7 to Output
    GPIOA->BSRR = GPIO_BSRR_BR_7; // Turn off LED 2 (PA7 Low)

    // Configure Timer1 for microseconds delay
    RCC->APB2ENR |= RCC_APB2ENR_TIM1EN; // Enable TIM1 clock
    TIM1->PSC = 16 - 1;                 // 16 MHz / 16 = 1 MHz
    TIM1->ARR = 1;                      // Auto-reload value for 1 µs
    TIM1->CNT = 0;                      // Reset counter
    TIM1->CR1 = 1;                      // Enable Timer1

    while (1)
    {
        // Sensor 1 (Trigger: PA0, Echo: PA1)
        Trig1_low;               // Ensure trigger 1 is low
        delayuS(10);             // Short delay
        Trig1_high;              // Turn on trigger 1
        delayuS(10);             // Pulse duration
        Trig1_low;               // Turn off trigger 1

        duration1 = read_echo(600000, GPIO_IDR_ID1); // Echo from sensor 1
        distance1 = duration1 / 58.0;               // Convert duration to cm

        if (distance1 > 1.0 && distance1 < 4.0)
        {
            GPIOA->BSRR = GPIO_BSRR_BR_5; // Turn off LED 1
            GPIOA->BSRR = GPIO_BSRR_BS_6; // Turn on Buzzer
        }
        else
        {
            GPIOA->BSRR = GPIO_BSRR_BS_5; // Turn on LED 1
            GPIOA->BSRR = GPIO_BSRR_BR_6; // Turn off Buzzer
        }

        delaymS(50); // Add delay to avoid interference

        // Sensor 2 (Trigger: PA2, Echo: PA3)
        Trig2_low;               // Ensure trigger 2 is low
        delayuS(10);             // Short delay
        Trig2_high;              // Turn on trigger 2
        delayuS(10);             // Pulse duration
        Trig2_low;               // Turn off trigger 2

        duration2 = read_echo(600000, GPIO_IDR_ID3); // Echo from sensor 2
        distance2 = duration2 / 58.0;               // Convert duration to cm

        if (distance2 > 2.0 && distance2 < 8.0)
        {
            GPIOA->BSRR = GPIO_BSRR_BS_7; // Turn on LED 2
        }
        else
        {
            GPIOA->BSRR = GPIO_BSRR_BR_7; // Turn off LED 2
        }

        delaymS(1000); // Delay for next measurement
    }
}

void delaymS(uint32_t ms)
{
    SysTick->LOAD = 16000 - 1; // 1 ms delay (16 MHz clock)
    SysTick->VAL = 0;          // Clear current value
    SysTick->CTRL = 0x5;       // Enable SysTick with processor clock

    for (int i = 0; i < ms; i++)
    {
        while (!(SysTick->CTRL & 0x10000))
            ; // Wait for count flag
    }

    SysTick->CTRL = 0; // Disable SysTick
}

void delayuS(uint32_t us)
{
    for (uint32_t i = 0; i < us; i++)
    {
        while (!(TIM1->SR & 1))
            ;        // Wait for update event
        TIM1->SR &= ~1; // Clear update flag
    }
}

uint32_t read_echo(uint32_t timeout, uint32_t echo_pin)
{
    uint32_t duration = 0;

    // Wait for echo pin to go high
    while (!(GPIOA->IDR & echo_pin))
    {
        duration++;
        delayuS(1);
        if (duration > timeout)
        {
            return 0; // Timeout occurred
        }
    }

    duration = 0;

    // Measure how long echo pin stays high
    while (GPIOA->IDR & echo_pin)
    {
        duration++;
        delayuS(1);
        if (duration > timeout)
        {
            return 0; // Timeout occurred
        }
    }

    return duration;
}
