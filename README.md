# STM32 GPIO External Interrupt - Shared Interrupt Line Button Control LED

## 📌 Project Overview
This project demonstrates how to use **GPIO External Interrupt (EXTI)** on STM32 to control LEDs with push buttons.

Two push buttons are connected to GPIO pins and trigger interrupts.  
Since both buttons use pins within the same EXTI group, they share one interrupt handler in NVIC.

When a button is pressed, the corresponding LED toggles ON/OFF.

---

## 🎯 Features

- Button 1 toggles LED 1
- Button 2 toggles LED 2
- Shared interrupt line handling
- STM32 HAL Library implementation
- Interrupt-driven design

---

## 🛠 Hardware Used

- STM32 Development Board  
- 2 Push Buttons  
- 2 LEDs  
- Breadboard / Jumper Wires

---

## 🔌 Pin Configuration

| Component | GPIO Pin |
|----------|----------|
| Button 1 | GPIO_PIN_6 |
| Button 2 | GPIO_PIN_7 |
| LED 1 | GPIO_PIN_13 |
| LED 2 | GPIO_PIN_14 |

---

## ⚙️ Main Code

```c
#define button1 GPIO_PIN_6
#define button2 GPIO_PIN_7
#define LED1 GPIO_PIN_13
#define LED2 GPIO_PIN_14

void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
    if(GPIO_Pin == button1)
    {
        HAL_GPIO_TogglePin(GPIOA, LED1);
    }
    else if(GPIO_Pin == button2)
    {
        HAL_GPIO_TogglePin(GPIOA, LED2);
    }
}

🧠 How It Works
Button 1 and Button 2 are configured as external interrupt inputs.
GPIO_PIN_6 and GPIO_PIN_7 belong to the same EXTI interrupt group.
Both signals use the same NVIC interrupt handler:
EXTI9_5_IRQn
When either button is pressed:
STM32 enters the shared interrupt handler.
HAL checks which pin triggered the interrupt.
HAL_GPIO_EXTI_Callback() receives the active pin number.
The corresponding LED toggles.
📷 Example Behavior
Action	Result
Press Button 1	LED 1 toggles
Press Button 2	LED 2 toggles
🚀 STM32CubeMX Configuration
GPIO Input
PA6 → External Interrupt Rising/Falling Edge
PA7 → External Interrupt Rising/Falling Edge
GPIO Output
PA13 → Output Push Pull
PA14 → Output Push Pull
NVIC Enabled
EXTI line[9:5] interrupts
🔍 Why Shared Interrupt Line?

STM32 groups some EXTI pins into common interrupt vectors:

EXTI Pins	IRQ Handler
EXTI0	EXTI0_IRQn
EXTI1	EXTI1_IRQn
EXTI2	EXTI2_IRQn
EXTI3	EXTI3_IRQn
EXTI4	EXTI4_IRQn
EXTI5 ~ EXTI9	EXTI9_5_IRQn
EXTI10 ~ EXTI15	EXTI15_10_IRQn

Since Button 1 = Pin 6 and Button 2 = Pin 7, both use:

EXTI9_5_IRQn
⚠️ Notes
Mechanical buttons may bounce; debounce may be required.
PA13 / PA14 may be used by SWD debugger on some boards.
Shared interrupt lines are normal STM32 architecture behavior.
📚 Learning Topics
GPIO Interrupts
Shared NVIC Interrupt Vector
HAL Callback Functions
Event-Driven Embedded Programming