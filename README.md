# HLAsm Simple I/O Macro
This repository contains I/O Macros intorduced in appendix B of 
[Assembler Language Programming for IBM z System Servers ](https://idcp.marist.edu/documents/33945/44724/Assembler.V2.alntext+V2.00.pdf/ad61965e-8485-65e1-f385-e5cd56f08c63?t=1551806232272)
from John R. Ehrman (R.I.P.).

First, place the macro definitions in a macro library accessible to the Assembler. The default print-line
length (121) can be changed in the PRINTLIN macro, and in the $$GENIO macro by modifying the variable
&$$PLL. The default DDnames are

```
Print MVS/CMS=SYSPRINT, VSE=SYSLST
Read MVS/CMS=SYSIN, VSE=SYSIPT
```

and can be changed by modifying the &$$ONAM and &$$INAM variable symbols in macro $$GENIO.

The $$GENIO macro is complex. It generates the $$IOSECT CSECT with six entry points. It can be used in
two ways:

1. The instructions in the $$IOSECT can be generated as part of the user's program, if the variable symbol
&$$LIBIO is set to 0 in the first few lines of the $$GENIO macro. This is simpler, but causes an “invisible
gap” in the statement numbers of the listing. The hidden statements can be displayed by specifying the
Assembler option PCONTROL(GEN,ON) but the generated code will be confusing to all but advanced students.
2. Alternatively, you can generate the $$IOSECT instructions into a separate module. (This has the advantage of hiding the complexities of the $$GENIO macro, but requires a little bit more initial setup.) First,
set the &$$LIBIO variable symbol to 0, and create and assemble this short program:
```
$$GENIO
End
```
Link the generated object module into a library accessible at program linking and loading time. Then,
change the $$LIBIO variable symbol to 1 to suppress subsequent inline generation, and store the macro
back in the macro library.

All symbols generated in the expansions of these macros begin with the two characters $$. If this conflicts
with your conventions (or desires), change each occurrence of '$$' to whatever two other characters you
like. (The symbol cross-reference for any assembly using these macros will include many symbols starting
with those two characters.)

The macros have been extensively tested under MVS, CMS, and VSE, and are set up to run under MVS or
CMS as the default. To run them under VSE, change the &$$DOS SETB statement (near the front of the
$$GENIO macro definition) according to the instructions there. Similarly, to change the default file names or
print line length, modify the specifying SETC statements, as indicated there.
