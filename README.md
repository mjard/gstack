gstack
======


A stack-based VM implementing a very small instruction set
meant for use in genetic programming. It is very forgiving
to lend flexibility to automatically generated input code.



**Features include**
- Very forgiving input evaluation
- Useful debugging mode prints instructions, stack, and friendly error info
- Continues quickly on most errors
    - Example: trying to jump past end of program is just ignored
    - If stack doesn't contain what an instruction expects, continues silently
    - On error, stack is retained and flag is set so the managing app can
      decide to keep or kill the program itself
- Major errors that will stop execution: bad instruction or arity
- Simple type system
    - Type is implied when value is pushed onto stack
    - Strongly, statically typed (type can never change)
    - All numbers pushed on the stack are Python floats
- Relative conditional jumping makes code segments more likely to work
  after crossover or mutation operations
- Easy to add or remove instructions
- Program ends when there are no more lines, an error, reaches END,
  or user-adjustable number of instructions have been executed (no infinite loops)

    
**Example input**

    PUSH 2
    JMP 5
    PUSH -4.8
    MUL
    PUSH 60
    END
    PUSH 3409843028     ; This is where the "JMP 5" lands
    ROT
    PUSH -1
    JNE -4              ; This is true, so jumps to "END" above

**Example output (with Debug on)**

    1 >  PUSH 2
    [2.0] 
    
    2 >  JMP 5
    [2.0] 
    
    7 >  PUSH 3409843028
    [2.0, 3409843028.0] 
    
    8 >  ROT
    [3409843028.0, 2.0] 
    
    9 >  PUSH -1
    [3409843028.0, 2.0, -1.0] 
    
    10 >  JNE -4
    [3409843028.0, 2.0, -1.0] 
    
    6 >  END

**TODO**
- Add feature to unroll, optimize, and pretty-print
- Add [de-]serialize so machine can be suspended, restarted
- Once stable, write a faster intrepeter in Go
