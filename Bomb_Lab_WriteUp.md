# Bomb Lab Write-up
In this write-up, I will show you how i solve bomb lab challenge. <br>
First bomb lab is a **Reverse Engineering** challenge, you have to read its assembly to find the message that expected by program. <br>

First we will face the main function:
![](/main_asm.png)

## Phase 1:

![](/phase_1_out.png)

First, the program prints these strings **"Welcome to my fiendish little bomb. You"** and **"which to blow yourself up. Have a nice "**
Then, we will find a calling of **read_line** function which take an input from user, and then mov this input to rdi as an argument to function **phase1**, then calling this function

![](/phase1_internal.png)

In internal code of phase 1 function, we will find that the function copy string **"I am just a renegade hockey mom."** into **rsi** register, and then calling **strings_not_equal** function, function take the user input and the value in **rsi** and if they are the same it will return **0**, and then **test** xor the **eax** if it not equal zero "which means that strings not the same", it will jump to this location **loc_55FBFAC475C4** which will call **explode_bomb** function, and if it equal zero the function will return to main and calling **phase_defused**, and print this string **"Phase 1 defused. How about the next one"**, and here we completed this phase.<br>

