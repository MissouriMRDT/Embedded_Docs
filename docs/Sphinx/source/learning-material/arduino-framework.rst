The Arduino Framework
=====================

Many people treat Arduino like it's its own language. In actuality, it's just
plain old C++.

You can use things like ``std::vector``, ``std::string``, or anything else you
learned about in C++ or data structures class. But you won't find those used
often in the embedded world because they allocate memory dynamically, which is
a big no-no when your memory is severely limited. Some standard library files
like ``std::iostream`` are simply too big to fit in the flash memory of some
microcontrollers.

The Arduino Framework is a collection of libraries you can include in your C++
code that call other microcontroller-specific code. This allows you to write
high-level code that works on any microcontroller that someone has written a
version of Arduino for.

For example, prior to Promethius (2021-2022), MRDT used a microcontroller
called the Tiva C which ran on the Ti Hercules chip. That year, we switched
to the smaller, more powerful Teensy 4.1, which runs on the NXP iMXRT1062 chip.
Because both the Tiva and Teensy were programmed with the Arduino library, we
did not have to rewrite any of our embedded libraries.

The implementation of Arduino for Teensy devices is called **Teensyduino**.

Setup and Loop
--------------

In ordinary C++, every program starts with the ``main()`` function. This is no
different on a microcontroller. Arduino, however, hides the ``main()`` function
by wrapping it in two different functions: ``setup()`` and ``loop()``.

This is how they are called:

.. code:: c++

    // Somewhere in Arduino.h

    void setup();
    void loop();

    int main() {
        setup();
        while (true) {
            loop();
        }
        return 0;
    }

As you can see, ``setup()`` just runs on startup, and ``loop()`` repeats
forever. Now, all you have to do is define these functions:

.. code:: c++

    #include <Arduino.h> // Don't forget to include this file!

    void setup() {
        // Run setup code, like initializing serial
        Serial.begin(9600);
        Serial.println("Finished setting things up.")
    }

    void loop() {
        delay(1000); // Delay for 1000 milliseconds
        Serial.println("Tick!");
    }

>>> Finished setting things up.
>>> Tick
>>> Tick
>>> Tick

Serial Debugger
---------------

Unfortunately, though the Teensy's iMXRT1062 chip has support for
`JTAG <https://en.wikipedia.org/wiki/JTAG>`_ and `SWD <https://en.wikipedia.org/wiki/JTAG#Similar_interface_standards>`_
(two common debugging protocols), the Teensy boards themselves do not expose
the pins needed to access them. To figure out what your program is actually
doing, your best bet is to use the serial monitor.

Serial Communication
    Sending or receiving one bit at a time.

"Serial" can actually refer to many different protocols, but the Arduino
framework's ``Serial`` library usually refers to either UART or USB. To
distinguish between the two, documentation will often refer to UART as
"hardware serial".

To access USB, all you have to do is say:

.. code:: c++

    Serial.begin(9600);

Here, ``Serial`` is an "object" (an instance of a class or struct that was
defined somewhere in the Arduino library), and ``begin()`` is an "instance
method" (a member function of the ``Serial`` class). This function requires one
argument -- the *baud rate* -- which we have set to ``9600``.

Baud Rate
    The number of bits sent each second in a serial protocol. Both of the
    communicating devices must have the same baud rate or else data will be
    lost!

``9600`` is just a common value used for baud rate (and the default used by
PlatformIO's serial monitor). It's actually quite slow for most modern devices.
Other common baud rates include ``9600``, ``19200``, ``38400``, ``57600``, and
``115200``. The Teensy 4.1's processor is so fast, there's really no reason not
to always use ``115200``. To change PlatformIO's baud rate, add the following
line to your ``platformio.ini`` file::

    monitor_speed = 115200

You can send messages over serial with ``print`` commands:

.. code:: c++

    #include <Arduino.h>

    void setup() {
        Serial.begin(115200);
        Serial.print("Today");
        Serial.print(" Tomorrow");
        Serial.print(" Forever\n");
        Serial.println("#RoveSoHard");
        Serial.print("MRDT was founded in ")
        Serial.println(2012);
    }

>>> Today Tomorrow Forever
>>> #RoveSoHard
>>> MRDT was founded in 2012

As for UART, the Teensy 4.1 supports up to 8 UART connections at once, which
can be accessed with the library objects ``Serial1`` through ``Serial8`` and
used in the same way as the USB ``Serial`` object.

Basic I/O
---------

A microcontroller that just prints things to your console isn't very useful.
The main thing that separates microcontrollers from other computers like your
laptop is their **GPIO**.

GPIO - General-Purpose Input/Output
    Programmable pins on a microcontroller that can send or receive digital
    signals.

Digital
    A signal that can have two distinct values: high (1) or low (0).

The Teensy 4.1 has a whopping 42 GPIO pins. When configured to "output" mode,
each of these can be driven to a *logical high* (3.3V) or a *logical low* (0V).
When configured to "input" mode, each of these pins can measure whether the
externally-driven voltage at the pin is logical high or logical low.

For this example, suppose we have some input signal, say, driven by a button
attached to pin 0, and an output signal, say, connected to an LED at pin 1:

.. code:: c++

    #include <Arduino.h>

    #define BUTTON_PIN 0
    #define LED_PIN 1

    void setup() {
        // Configure GPIO pin 0 to act as an input pin
        pinMode(BUTTON_PIN, INPUT); // All pins are INPUT by default
        // Configure GPIO pin 1 to act as an output pin
        pinMode(LED_PIN, OUTPUT);
    }

    void loop() {
        if (digitalRead(BUTTON_PIN)) {
            // Turn the LED on if the button pin senses a logical high
            digitalWrite(LED_PIN, HIGH);
        } else {
            // Otherwise, turn the LED off
            digitalWrite(LED_PIN, LOW);
        }
        delay(10); // Give time for the button to settle into place
    }


Nearly all microcontrollers will support multiple protocols, like SPI, I2C,
UART, PWM, and CAN. To make them all fit, the chip designers make the GPIO pins
work double-duty. You use software to configure what each pin will be used for.

Pinout
    A diagram showing what each pin on a microcontroller or other IC can be
    used for.

Here is the pinout of the Teensy 4.1. We have tons of physical cards with this
useful diagram. Ask a lead for one!

.. image:: teensy_pinout.png
    :alt: Teensy GPIO

We'll revisit these protocols in later sections.

.. .. doxygenindex:: d
..     :project: EmbeddedLibs
.. .. doxygenclass:: RoveCommEthernet
..    :project: EmbeddedLibs
..    :members:
