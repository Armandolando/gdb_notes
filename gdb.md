# GDB real user interface, python interpreter and reverse debugging
												 
## TUI mode (Text User Interface)

The TUI mode is a more user friendly UI that allows you to run thruoght the debugging of the program highlighting the last line that has been executed.

Standard UI:

[with_A](images/gdb1.png)

In order to activate the TUI mode you have to press ```Ctrl```+```X```+```A``` after you already started the program.

TUI mode:

[with_A](images/gdb2.png)

Type ``` next ``` to do a step.

[with_A](images/gdb3.png)

Sometimes it is possible that after a step it starts to display duplicate lines of code or stuff like that: 

[with_A](images/gdb4.png)

in order to fix this just repaint the display with ```Ctrl```+```L```.

During the dubugging if you press ```Ctrl```+```X``` and then ```2```  you can see the assemly of the program highlighting where you arrived executing it.

[with_A](images/gdb5.png)

And if you do it again it shows the registers and if you want to see the float registers type "tui reg float".

[with_A](images/gdb6.png)

If you want to go back press ```Ctrl```+```X``` and then ```1``` one time to see the source code and the assembly or two times to see only the source code. 

With the up and down arrow keys you can scroll the soucre code and can be tedious because usualy that keys are use to see the previous commands. In order to see the previous command use ```Ctrl```+```P``` for the previous and ```Ctrl```+```N``` for the next.

To go back to the standard ui press ```Ctrl```+```X```+```A``` again.

Sometimes says "No source aviable" try to scroll or resize the window to fix it.

## Python interpreter

There's a python interpreter built in gdb. You can execute single function like print or an entire script by typing python followed by the lines you want to execute. The last line has to be "end" in order to execute.

[with_A](images/gdb7.png)

You can define functions and then call them from the command line or make them first class gdb command.
Also you can interact with gdb itself for example printing the breakpoints you added typing:

```sh
python print(gdb.breakpoints())
```
or finding information about them like the location typing:

```sh
python print(gdb.breakpoints()[0].location)
```
You can even set breakpoints from python for example typing:

```sh
python gdb.Breakpoint('7')
#7 is the line. 
```
[with_A](images/gdb8.png)

## Reversable Debugging

When a program faults is possible to step back and get the context. When the fault occur you can type:

```sh
reverse-stepi
```
to see what cause it, most of the time is a corruption of a return address on the stack. In order to see what corrupted the address we start from find what changed the element on top of the stack by using the command "watch" followed by the address pointed by the $sp:

```sh
(gdb) p	$sp
$2 = (void *) 0x7fffffffd598
watch *(long **) 0x7fffffffd598
Hardware watchpoint 4: *(long **) 0x7fffffffd598
```
and then typing:
 
```sh
reverse-continue
```
in order to go backward instead of forward. After that the program breaks at the time the element on the stack was modified, if you are in TUI mode the line which cause it will be highlighted.  
