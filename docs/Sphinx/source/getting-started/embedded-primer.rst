Embedded Primer
===============

What *is* Embedded Software, Anyways?
-------------------------------------

In short, embedded software bridges the gap between the rover's electronics and
higher-level software like Autonomy and Basestation. We write our embedded
software in **C++** using the **Arduino Framework**. More on that
:doc:`here <../learning-material/arduino-framework>`.

Writing embedded software is in many ways similar to writing code for a regular
computer just like you would in CS 1570, but with some extra constraints.

To manage different versions of our software and to deal with multiple people
working on the same project simultaneously, we use **git**. More on that
:doc:`here <../learning-material/intro-to-git>`.

Embedded Software Glossary
--------------------------

Here's some software terms you might hear:

Integrated Circuit (IC)
    One or more entire circuits shrunk down and fit into a single chip. These
    chips typically adhere to some standard package like SOIC or TSSOP.

Microcontroller
    A single-chip processor that contains all the hardware it needs to
    function. We mostly use one called the Teensy 4.1, but many others exist.

Microprocessor
    A processor chip that requires external hardware to function; just the CPU.

Architecture
    What family a microcontroller belongs to. Each of these "families" shares
    similar hardware and run on similar machine code.

Integrated Development Environment (IDE)
    A text editor specifically for editing code. Integrated means that it can
    do other stuff like compile and upload code. Examples include Arduino IDE,
    Platform IO, and Atmel Microchip Studio.

Library
    Code written to be reused between projects. No point in reinventing the
    wheel!

Application Programming Interface (API)
    The interface where two bits of code meet. Basically a list of each thing a
    library can do.

Hardware Abstraction Layer (HAL)
    A library written to simplify programming of a microcontroller. Instead of
    setting specific registers to control the hardware, you can call a nicely
    named function that will do all that for you.

Dependency
    A library that a project relies on.

Compiler
    The program that turns your human-readable code into binary that your
    specific microcontroller understands. This is normally integrated into your
    IDE and runs automatically as part of the upload process. You may need a
    different compiler depending on your microcontroller's architecture.

Flashing
    The process of uploading compiled code to a microcontroller. The code is
    stored in the "flash memory". A Teensy has a lot of flash memory, but on
    other, less powerful microcontrollers, you might have to worry about making
    your code small enough to fit into this memory. A Teensy has 2 megabytes of
    flash.

Random Access Memory (RAM)
    Memory a program uses while it is running, which is separate from the flash
    memory. Unlike flash memory RAM is "volatile", meaning it is lost when the
    microcontroller loses power. A Teensy has 1 megabyte of RAM.

Protocol
    A method of sending information from one place to another that both sides
    agree on. Some protocols you will learn about include SPI, I2C, CAN,
    Ethernet, USB, and our own custom protocol RoveComm, among others.

.. toctree::
    :maxdepth: 2
