from machine import Pin, ADC, PWM
import utime

# Initialize components
pot = ADC(26)
led = PWM(Pin(15))          # LED connected to GPIO15 with PWM
led.freq(1000)              # Set frequency for smooth brightness control
buzzer = PWM(Pin(16))       # Buzzer connected to GPIO16 with PWM
buzzer.freq(1000)           # Buzzer frequency set to 1000 Hz

last_pot_value = 0          # Store the last potentiometer value

while True:
    pot_value = pot.read_u16()  # Read potentiometer value
    led.duty_u16(pot_value)     # Set LED brightness to match potentiometer

    # Trigger buzzer when potentiometer crosses threshold
    if pot_value > 32500 and last_pot_value <= 32500:
        buzzer.duty_u16(60000)  # Loud buzzer sound
        utime.sleep(0.2)        # Short beep
        buzzer.duty_u16(0)      # Turn off buzzer
    elif pot_value <= 32500 and last_pot_value > 32500:
        buzzer.duty_u16(60000)
        utime.sleep(0.2)
        buzzer.duty_u16(0)

    last_pot_value = pot_value  # Update last potentiometer reading
    utime.sleep(0.1)            # Small delay for smooth response
