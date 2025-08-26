The Arduino Framework
=====================

Many people treat Arduino like it's its own language. In actuality, it's just
plain old C++.

You can use things like ``std::vector`` and ``std::string``,
but you won't find those used often in the embedded world because they allocate
memory dynamically, which is a big no-no when your memory is severely limited.

Setup and Loop
--------------

In ordinary C++, every program starts with the `main()` function.

Serial Debugger
---------------

.. doxygenindex::
    :project: EmbeddedLibs

Basic I/O
---------

.. .. doxygenclass:: RoveCommEthernet
..    :project: EmbeddedLibs
..    :members:
