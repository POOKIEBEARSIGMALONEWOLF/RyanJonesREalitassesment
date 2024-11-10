 IT_ASSESMENT
![IMG-4173](https://github.com/user-attachments/assets/9c2b8e0b-9258-469b-a6fd-e4df210b9c2a)
![IMG-4174](https://github.com/user-attachments/assets/4c78575c-c594-4e83-af9e-27a27f3bd36e)
![IMG-4176](https://github.com/user-attachments/assets/fde57433-c3ba-421c-80c1-b3ecd1fabe2b)
![IMG-4177](https://github.com/user-attachments/assets/5b880781-6abb-4d84-9b47-a9cc1b0ea2b3)
![IMG-4178](https://github.com/user-attachments/assets/1fe28904-b05a-4e92-bd1b-0beb6f00e1bb)
![IMG-4179](https://github.com/user-attachments/assets/b60ac1e9-3dc6-4f14-8ba8-b18c71ca00db)
![IMG-4180](https://github.com/user-attachments/assets/de309036-94f7-4ffc-94d3-9923d4e0aba1)


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



