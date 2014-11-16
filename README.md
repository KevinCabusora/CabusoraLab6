CabusoraLab6
============

Robot Control

#Prelab

Consider your hardware (timer subsystems, chip pinout, etc.) and how you will use it to achieve robot control. Which pins will output which signals you need? Which side of the motor will you attach these signals to? How will you use these signals to achieve forward / back / left / right movement? Spend some time here, as these decisions will dictate much of how difficult this lab is for you.

Consider how you will setup the PWM subsytem to achieve this control. What are the registers you'll need to use? Which bits in those registers are important? What's the initialization sequence you'll need?

Consider what additional hardware you'll need (regulator, motor driver chip, decoupling capacitor, etc.) and how you'll configure / connect it.

Consider the interface you'll want to create to your motors. Do you want to move each motor invidiually (moveLeftMotorForward())? Or do you want to move them together?
- I would probably move motors by side (One side moving at same speed all times).  This would then allow for a "tank-like" movement, where in order to turn the robot would go at different speeds on both sides, or go forwards on one side and backwards the other.
