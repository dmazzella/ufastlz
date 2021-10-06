ufastlz
=====

Micropython wrapper for [FastLZ](https://github.com/ariya/FastLZ), a lightning-fast lossless compression library.

Compiling the cmodule into MicroPython
=====

To build such a module, compile MicroPython with an extra make flag named ```USER_C_MODULES``` set to the directory containing all modules you want included (not to the module itself).


#### unix port

```bash
➜  ~ git clone https://github.com/micropython/micropython.git
➜  ~ cd micropython
➜  micropython (master) ✗ git clone https://github.com/dmazzella/ufastlz.git ports/unix/cmodules/ufastlz
➜  micropython (ufastlz) ✗ cd ports/unix/cmodules/ufastlz
➜  micropython (ufastlz) ✗ git submodule update --init
➜  micropython (ufastlz) ✗ cd ../../../../
➜  micropython (master) ✗ make -j8 -C mpy-cross && make -j2 -C ports/unix/ VARIANT="dev" USER_C_MODULES="$(pwd)/ports/unix/cmodules"
```


#### stm32 port

```bash
~ git clone https://github.com/micropython/micropython.git micropython
➜  ~ cd micropython
➜  micropython (master) ✗ git submodule update --init
➜  micropython (master) ✗ git clone https://github.com/dmazzella/ufastlz.git ports/stm32/boards/PYBD_SF6/cmodules/ufastlz
➜  micropython (ufastlz) ✗ cd ports/stm32/boards/PYBD_SF6/cmodules/ufastlz
➜  micropython (ufastlz) ✗ git submodule update --init
➜  micropython (ufastlz) ✗ cd ../../../../../../
➜  micropython (master) ✗ make -j2 -C mpy-cross && make -j2 -C ports/stm32/ BOARD="PYBD_SF6" USER_C_MODULES="$(pwd)/ports/stm32/boards/PYBD_SF6/cmodules"
```

Usage
=====

```python
MicroPython v1.17-74-gd42cba0d2-dirty on 2021-10-06; PYBD-SF6W with STM32F767IIK
Type "help()" for more information.
>>> import _fastlz
>>> d = "test " * 100
>>> d
'test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test '
>>> c = _fastlz.compress(d)
>>> c
b'\x04test \xe0\xfd\x04\xe0\xdb\x04\x04test '
>>> _fastlz.decompress(c)
b'test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test test '
>>> 
```

API
=====
 - ```_fastlz.compress(data, level=2)```
 - ```_fastlz.decompress(data, maxout=2048)```