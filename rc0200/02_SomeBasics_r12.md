## Problem

You are getting tired of typing long sequences of commands, and especially tired of typing the same ones over and over.

## Solution

Open an editor window and accumulate your reusable blocks of R commands there. Then, execute those blocks directly from that window. Reserve the console window for typing brief or one-off commands.

When you are done, you can save the accumulated code blocks in a script file for later use.

## Discussion

The typical R beginner types an expression in the console window and sees what happens. As he gets more comfortable, he types increasingly complicated expressions. Then he begins typing multiline expressions. Soon, he is typing the same multiline expressions over and over, perhaps with small variations, in order to perform his increasingly complicated calculations.

The experienced R user does not often retype a complex expression. She may type the same expression once or twice, but when she realizes it is useful and reusable she will cut and paste it into an editor window. To execute the snippet thereafter, she selects the snippet in the editor window and tells R to execute it, rather than retyping it. This technique is especially powerful as her snippets evolve into long blocks of code.

In RStudio, a few shortcuts in the IDE facilitate this work style. Windows and Linux machines have slightly different keys than Mac machines: Windows/Linux uses the _Ctrl_ and _Alt_ modifiers, whereas the Mac uses _Cmd_ and _Opt_.

To open an editor window

From the main menu, select File → New File, then select the type of file you want to create—in this case, an R script. Or if you know you want an R script, you can press Shift-Ctrl-N (Windows) or Shift-Cmd-N (Mac).

To execute one line of the editor window

Position the cursor on the line and then press Ctrl-Enter (Windows) or Cmd-Enter (Mac) to execute it.

To execute several lines of the editor window

Highlight the lines using your mouse; then press Ctrl-Enter (Windows) or Cmd-Enter (Mac) to execute them.

To execute the entire contents of the editor window

Press Ctrl-Alt-R (Windows) or Cmd-Opt-R (Mac) to execute the whole editor window. Or from the menu, click Code → Run Region → Run All.

You can find these keyboard shortcuts and dozens more within RStudio by choosing the Tools → Keyboard Shortcuts Help menu item.

Reproducing lines from the console window in the editor window is simply a matter of copy and paste. When you exit RStudio, it will ask if you want to save the new script. You can either save it for future reuse or discard it.