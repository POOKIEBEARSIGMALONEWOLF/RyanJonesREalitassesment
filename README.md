 IT_ASSESMENT
![IMG-4178](https://github.com/user-attachments/assets/93782178-675d-4e6a-a098-8346e0d44db2)
![IMG-4180](https://github.com/user-attachments/assets/7b70af10-5261-4968-8571-4ee5d0ac3398)
![IMG-4176](https://github.com/user-attachments/assets/84ff126a-be3a-403b-aa2d-f09fee2d0471)
![IMG-4177](https://github.com/user-attachments/assets/4a644afe-c96b-4eca-919a-ba2c5ad801bc)
![IMG-4173](https://github.com/user-attachments/assets/a3d69b8a-2296-456b-a853-ca24fd771233)


 Step-by-Step Guide: Raspberry Pi Pico Project with Potentiometer-Controlled LED and Buzzer

In this project, we’ll build functionality step-by-step: starting by turning on an LED, then controlling it with a button, then a potentiometer, and finally using the potentiometer to adjust brightness and trigger a buzzer.

 Step 1: Turn on an LED
Goal: Start by turning an LED on and off to ensure the setup works.
- Connections: Connect an LED to GPIO15 with a resistor in series to protect it, and ground the other leg.
- Code:
    python
    from machine import Pin
    import time

    led = Pin(15, Pin.OUT)    LED connected to GPIO15
    led.on()                  Turn on LED
    time.sleep(2)             Keep it on for 2 seconds
    led.off()                 Turn off LED
    

 Step 2: Control the LED with a Button
Goal: Add a button to manually control the LED.
- Connections: Connect the button to GPIO14 as an input with a pull-down resistor.
- Code:
    python
    from machine import Pin

    led = Pin(15, Pin.OUT)
    button = Pin(14, Pin.IN, Pin.PULL_DOWN)   Button connected to GPIO14

    while True:
        if button.value() == 1:
            led.on()           Turn on LED when button is pressed
        else:
            led.off()          Turn off LED when button is released
    

 Step 3: Control the LED with a Potentiometer
Goal: Use a potentiometer to turn the LED on or off based on its position.
- Connections: Connect the potentiometer to ADC pin GP26.
- Code:
    python
    from machine import Pin, ADC

    led = Pin(15, Pin.OUT)
    pot = ADC(26)              Potentiometer connected to ADC pin GP26

    while True:
        pot_value = pot.read_u16()   Read potentiometer value (0-65535)
        if pot_value > 32500:        Set threshold for turning on the LED
            led.on()
        else:
            led.off()
    

 Step 4: Turn on the Buzzer with the Potentiometer
Goal: Use the potentiometer to trigger a buzzer when it crosses a specific threshold.
- Connections: Connect the buzzer to GPIO16 with PWM enabled.
- Code:
    python
    from machine import Pin, ADC, PWM
    import utime

    led = Pin(15, Pin.OUT)
    pot = ADC(26)
    buzzer = PWM(Pin(16))       Buzzer connected to GPIO16 with PWM
    buzzer.freq(1000)           Set buzzer frequency to 1000 Hz (audible)

    while True:
        pot_value = pot.read_u16()
        if pot_value > 32500:   Threshold for triggering buzzer
            buzzer.duty_u16(60000)    Turn on buzzer at a loud volume
            utime.sleep(0.2)          Sound duration
            buzzer.duty_u16(0)        Turn off buzzer
        led.value(pot_value > 32500)  Sync LED on/off with buzzer trigger
    

 Step 5: Adjust LED Brightness with the Potentiometer
Goal: Make the LED brightness respond to the potentiometer’s position.
- Connections: Keep the LED on GPIO15 with PWM enabled, potentiometer on GP26, and buzzer on GPIO16.
- Code:
    python
    from machine import Pin, ADC, PWM
    import utime

     Initialize components
    pot = ADC(26)
    led = PWM(Pin(15))           LED connected to GPIO15 with PWM
    led.freq(1000)               Set frequency for smooth brightness control
    buzzer = PWM(Pin(16))        Buzzer connected to GPIO16 with PWM
    buzzer.freq(1000)            Buzzer frequency set to 1000 Hz

    last_pot_value = 0           Store the last potentiometer value

    while True:
        pot_value = pot.read_u16()   Read potentiometer value
        led.duty_u16(pot_value)      Set LED brightness to match potentiometer

  if pot_value > 32500 and last_pot_value <= 32500:
            buzzer.duty_u16(60000)   Loud buzzer sound
            utime.sleep(0.2)         Short beep
            buzzer.duty_u16(0)       Turn off buzzer
        elif pot_value <= 32500 and last_pot_value > 32500:
            buzzer.duty_u16(60000)
            utime.sleep(0.2)
            buzzer.duty_u16(0)

  last_pot_value = pot_value   Update last potentiometer reading
        utime.sleep(0.1)             Small delay for smooth response
    

 Final Outcome
With this final setup, the potentiometer controls:
- LED Brightness: The LED smoothly changes brightness based on the potentiometer’s position.
- Buzzer Sound: The buzzer briefly sounds whenever the potentiometer crosses the threshold of 32,500, both upwards and downwards. 

Each step builds on the last, making the project an interactive way to control both light and sound with the potentiometer.



