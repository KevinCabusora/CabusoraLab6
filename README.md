CabusoraLab6
============

Robot Control

##Prelab

Consider your hardware (timer subsystems, chip pinout, etc.) and how you will use it to achieve robot control. Which pins will output which signals you need? Which side of the motor will you attach these signals to? How will you use these signals to achieve forward / back / left / right movement? Spend some time here, as these decisions will dictate much of how difficult this lab is for you.

- Motor A: Pins 1.0, 1.1, Motor B: Pins 2.0 and 2.1
- Sides: Motor A -- left, Motor B -- right
- Forward: Run motors in opposite directions (remember, they are different orientations)
- Backwards: Switch power source and ground for both motors from forwards
- Turn: Make the motors run in same direction; to turn another way you switch orientation

Consider how you will setup the PWM subsytem to achieve this control. What are the registers you'll need to use? Which bits in those registers are important? What's the initialization sequence you'll need?

Consider what additional hardware you'll need (regulator, motor driver chip, decoupling capacitor, etc.) and how you'll configure / connect it.

- To power the MSP430, I will use the voltage regulator to take the Vdd down from an excessive 5v to a more MSP430-friendly 3.3 V. 
- To drive the motor, a motor driver chip is necessary.  This allows the pulse width output from the MSP430 to be amplified to 12v in order to power the motors.  (Cannot exceed 1A)  The MSP430 will send a signal with a certain duty cycle.  This duty cycle determines the speed of the motors using pulse width modulation.
- The decoupling capacitors will be used to reduce noise and voltage fluctuations and smooth out the signal.

Consider the interface you'll want to create to your motors. Do you want to move each motor invidiually (moveLeftMotorForward())? Or do you want to move them together?
- I would probably move motors by side.  This would then allow for a "tank-like" movement, where in order to turn the robot would go at different speeds on both sides, or go forwards on one side and backwards the other.
- Paradoxically, to go forward I would program the motors for one to go in one direction and one to go in another.  This is because the motors move in default in one direction (say, clockwise) and letting them run in the same direction leads to a spin.  Therefore I will let them move in different directions.
