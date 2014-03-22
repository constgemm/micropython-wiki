This page is a proof-of-concept effort to document differences between CPython3 (considered to be a reference implementation of Python3 language) and MicroPython. It classifies differences in 3 categories, each category having a different status regarding possibility that items belonging to it will change.

## Differences By Design
MicroPython is intended for constrained environments, in particular, microcontrollers, which have orders of magnitude less performance and memory than "desktop" systems on which CPython3 runs. This means that MicroPython must be designed with constrained environment in mind, leaving out features which simply won't fit, or won't scale, with target systems. "By design" differences are unlikely to change.

1. MicroPython does not ship with extensive standard library of modules.
2. print() function does not check for recursive data structures, and if fed with such, may run in infinite loop. Note that if this is concern to you, you can fix this on Python application level - by writing a function which keeps history of each object visited, and overriding builtin print() with your custom function. Such an implementation may of course use lot of memory - why it's not implemented on MicroPython level.

## Implementation Differences
Some features don't cater for constrained systems, and at the same time not easy to implement - efficiently or at all. Such features are known as "implementation differences" and some of them are potentially subject for future development after corresponding discussion and consideration. Note that many of them would affect size and performance of MicroPython implementation, so sometime not implementing some feature (or it part) is called by MicroPython's target usage.

1. No unicode support is actually implemented. Python3 calls for strict difference between ``str`` and ``bytes`` data types (unlike Python2, which has neutral unified data type for strings and binary data, and separates out unicode data type). MicroPython faithfully implements ``str``/``bytes`` separation, but currently, underlying ``str`` implementation is the same as ``bytes`` - meaning, strings in MicroPython are not unicode, but 8-bit characters (fully binary-clean).
1. Object finalization (``__del__()`` method) is not currently implemented. This is tracked by [#245](//github.com/micropython/micropython/issues/245).
1. It's not possible to subclass builtin types. [#317](//github.com/micropython/micropython/issues/317)

## Known Issues
Known issues are essentially bugs, misfeaturures, and omissions considered such, and scheduled to be fixes. So, ideally any entry here should be accompanied by bug ticket reference. But note that known issues may have different priorities, especially within wider development process, so if you are actually affected by some issue, please add details of your case to the ticket (or open it if does not yet exist) to help planning (submitting patches is even more productive).

1. Some functions don't perform well enough argument checking, which potentially may lead to crash if wrong argument types are passed.
1. Some functions use ``assert()`` for argument checking, which will lead to crash if wrong argument types are passed/errornous conditions are faced. This should be replaced with Python exception raising.
1. It's not possible to stably override builtin functions so far - not "builtins" module is implemented, so any overrides work only within current module only.
1. There's no support for persistent bytecode, and running bytecode (vs running Python source). [#222](//github.com/micropython/micropython/issues/222)
1. print() function doesn't use Python stream infrastructure, but underlying C printf() function directly. This means if you override sys.stdout, print() won't be affected. [#209](//github.com/micropython/micropython/issues/209)
1. Some more advanced usages of package/module importing not fully implemented. [#298](//github.com/micropython/micropython/issues/298)
1. Exception handling with generators is not fully implemented. [#243](//github.com/micropython/micropython/issues/243)
1. ``struct`` module not implemented.
1. ``re`` module not implemented. [#13](//github.com/micropython/micropython/issues/13)
1. Only beginning of ``io`` module and class hierarchy exists so far.