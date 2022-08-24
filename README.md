# Bomb Lab Write-up
In this write-up, I will show you how i solve bomb lab challenge. <br>
First bomb lab is a **Reverse Engineering** challenge, you have to read its assembly to find the message that expected by program. <br>

First we will face the main function:
![](/main_asm.png)

## Phase_1:

![](/phase_1_out.png)

First, the program prints these strings **"Welcome to my fiendish little bomb. You"** and **"which to blow yourself up. Have a nice "**
Then, we will find a calling of **read_line** function which take an input from user, and then mov this input to rdi as an argument to function **phase1**, then calling this function

![](/phase1_internal.png)

In internal code of phase 1 function, we will find that the function copy string **"I am just a renegade hockey mom."** into **rsi** register, and then calling **strings_not_equal** function, function take the user input and the value in **rsi** and if they are the same it will return **0**, and then **test** xor the **eax** if it not equal zero "which means that strings not the same", it will jump to this location **loc_55FBFAC475C4** which will call **explode_bomb** function, and if it equal zero the function will return to main and calling **phase_defused**, and print this string **"Phase 1 defused. How about the next one"**, and here we completed this phase.<br>

## Phase_2:

![](/phase2_internal.png)

In this phase, we will find some function initialization firstly and clear the **eax** register, and we will see a calling for function **read_six_number** which tells us how our input format should be.
And then, we will see a comparison between a number and 1, and if this number doesn’t equal 1, then a bomb will be exploded, so we can make sure that our first number is 1.

So, now we will jump to **loc_55901F1135F3** which the program will prepare to loop
![](/pics/prepare_to_loop_p2.png)

Then, we will jump to this location **loc_55901F113612**. <br>
In this location we will see that moving a value in **rbx** to **eax** "we notice that that the value of **rbx** is 1", then we will find that the program adding eax to itself and repeat it. <br>
So if we if we keep tracing this loop with 6 iterations "read_six_numbers", We will figure out that our right sequence should be **1 2 4 8 16 32**, and here we completed this phase. <br>

## Phase_3:

![](/pics/phase3_internal.png)

In this phase, we will find some function initialization firstly and clear the **eax** register, and then we will see a loading of two intergers in rsi, then calling **___isoc99_sscanf** which check the number of inputs and return this number as an output, so if we see the content of **eax** we will see that it contains 2. 
![](/pics/eax.png) <br>

If we continue with the asm we will find that there is checking on the first input with 7 so we can figure out that there are 7 switch cases in this phase, and if the first input is greater than 7 it will call **explode_bomb**
![](/pics/checking_cases.png) <br>

In this section, if we move with asm step by step we will figure out that these instructions move the location of case 4 into **rax**
![](/pics/switch_jump.png)
<br>Jumping to case 4:
![](/pics/case4.png) <br>

In this section, we will see that there is an adding and subtracting with the same value with **eax** 
![](/pics/sec_input_form.png)<br>
If we view the content of **eax** we will see that it will be (0)
![](/pics/second_input_4.png)<br>

After these operations we will see that the program compare the second input we typed it with (0) and if this input not equal, then **explode_bomb** will be called <br>
 
So, we can conclude that the second input for 4 is 0 <br>

**Password: 4 0**
![](/pics/phase3_defused.png) <br>

## Phase_4:
In this phase also we will as usuall some function intailization, and then we will find calling an **___isoc99_sscanf** function and comparing the **eax** which contain the output or return from this function with 2 and if not equal will call **explode_bomb** <br>
![](/pics/phase4/phase4_internal.png) <br>
So, the first information we founded is that we must enter 2 inputs. <br>

So, we enter 2 inputs and keep going in the code, we will find another comparing , at this time between first input and 0Eh (14 in decimal) and jumping if below or equal so our first input should not be greater than 14. <br>
![](/pics/phase4/first_input_14.png) <br>

So, if our first input is less than or equal 14, we will move to another piece of code which move 0Eh to **edx** and 0 to **esi**, our first input to **edi**, and then calling **func4** and compare the return value from it with 0Ah (10 in decimal) if not equal will explode the bomb so let's walk through func4. <br>
![](/pics/phase4/func4_out.png) <br>

If we look inside this function we will see some math inside. <br>
![](/pics/phase4/end_phase4.png) <br>

So we can conclude from the rest of code that the second input shoud be 0Ah (10 in decimal), and the first input should be the number that if we pass it as a parameter to **func4** will produce 10, so if we keep tracking this math we will find that the first input should be 3. <br>
![](/pics/phase4/phase4_defused.png)
**Password: 3 10**
