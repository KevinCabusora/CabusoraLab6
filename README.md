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

#Actual Design/Implementation

To implement my actual robot, I made changes to my initial decisions in the prelab.

I would use three wires per motor: an enable, direction control, and PWM.  The connections I used are shown below in the diagram.

## Required Functionality Design

To begin with, I based my code off of Dr. Coulston's provided code on his website.  However, I made some slight modifications to account for my pin assignments.

```
#include <msp430.h>

void main(void) {
    WDTCTL = WDTPW|WDTHOLD;                 // stop the watchdog timer

    P1DIR &= ~BIT3;
    P1REN |= BIT3;

    // Left Motor Declarations
    P2DIR |= BIT0;	            // 1,2EN as OUT
    P2DIR |= BIT1;              // Left motor direction
    P2DIR |= BIT2;							// P2.2 is associated with TA1CCR
    P2SEL |= BIT2;              // P2.2 is associated with TA1CCTL1
    
    // Right Motor Declarations
    
    P2DIR |= BIT5;              // 3,4EN as OUT
   	P2DIR |= BIT3;              // Right motor direction
    P2DIR |= BIT4;							// P2.2 is associated with TA1CCR2
    P2SEL |= BIT4;							// P2.2 is associated with TA1CCTL2

	TA1CTL = ID_3 | TASSEL_2 | MC_1;		// Use 1:8 presclar off MCLK
    TA1CCR0 = 0x0100;						// set signal period

    TA1CCR1 = 0x0020;
    TA1CCTL1 = OUTMOD_7;					// set TACCTL1 to Reset / Set mode

    TA1CCR2 = 0x0020;
    TA1CCTL2 = OUTMOD_3;					// set TACCTL1 to Reset / Set mode
```

I also made the times into constants for which the robot was moving or pausing.  The names are self-explanatory.  I received by the numbers by playing around with the robot until it would rotate enough/satisfactorily.

```
#define move	1000000
#define delay	2000000
#define smTurn	0500000
#define lgTurn	0750000
```

With the constants set I then set forth to create the little "dance routine" of my robot in a while loop.

```
while(1){

        forward();
        __delay_cycles(move);
        stop();
        __delay_cycles(delay);

        reverse();
        __delay_cycles(move);
        stop();
        __delay_cycles(delay);

        left();
        __delay_cycles(smallTurn);
        stop();
        __delay_cycles(delay);

        right();
        __delay_cycles(smallTurn);
        stop();
        __delay_cycles(delay);

        left();
        __delay_cycles(LargeTurn);
        stop();
        __delay_cycles(delay);

        right();
        __delay_cycles(LargeTurn);
        stop();
        __delay_cycles(delay);


    }// end while
```

#Required Functionality Demonstration
https://www.youtube.com/watch?v=rJQJGmCc3G8
