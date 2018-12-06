# Code Review Comments

## Comments From David Frey - 20181205
* `fbws.c/h` should be named to something more descriptive.  Maybe `fb_waveshare_213.c/h`
* Format code according to the [Linux kernel coding
  style](https://www.kernel.org/doc/html/v4.19/process/coding-style.html). This will result in many
  changes including, but not limited to:
  * remove trailing whitespace at end of lines
  * insert spaces after commas between params
  * insert spaces around binary operators
  * line length not greater than 80 columns
* Remove all commented out code. Debugging messages should be left in, but should use `dev_dbg()`
* What is the meaning of "ourfb"?  I think "ws213fb" is a better prefix.
* Why is `rpifb.c` named the way that it is?  Wouldn't `mangOH_red_ws213.c` be a better name?
* It seems that `rpifb.h` isn't included by either `fbws.c` or `rpifb.c`. If the file isn't
  required, delete it.
* `struct our_function` in fbws.h isn't used.  Delete it.
* `struct ourfb_function` in fbws.h isn't used.  Delete it.
* `enum ourfb_cmd` in fbws.h isn't used.  Delete it.
* fbws.h declares `struct ourfb_par`. What is the meaning of "par"? Is there a better name that
  could be chosen?
* fbws.h should only contain definitions that are required in multiple C files. If it's only needed
  in fbws.c, then just put it in fbws.c. I think the only thing that rpifb.c actually uses from
  fbws.h is `struct ourfb_platform_data`.



  
