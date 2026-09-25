# The Stacklan specification (ver. 1.1, rev. 3)

## Overview
Stacklan is a [Brainf\*\*k](https://en.wikipedia.org/wiki/Brainfuck)-inspired
programming language that uses the same commands as Brainf\*\*k, but assigns
different purposes to them, and uses a stack instead of a memory tape. Its
program file extension is `.stck`.

## The stack
Stacklan uses a stack and a register to store data. The stack's size is
implementation-defined, but the specification recommends that the stack is at
least 1000 items tall. Each item in the stack must be an unsigned 8-bit integer
(`unsigned char`). The register (referred to as `reg` from now on) contains an
8-bit unsigned integer that can be transferred from or to the stack, and is
initialized to 0.

## The instruction memory
A Stacklan interpreter must store programs separately from the stack, and
a Stacklan program cannot access or modify its "instruction" memory.

## The commands
Stacklan has 9 commands, mostly from Brainf\*\*k, with only one being unique
to it (`!`). However, `>`, `<`, `+`, and `-`, have different purposes:

`>`: Push 1 onto the stack. If the stack is full, the behaviour is unspecified.

`<`: Pop the topmost item off of the stack and discard it. If the stack is
empty, the behaviour is unspecified.

`+`: Pop the two topmost items off of the stack, sum them (wrapping around to 0
if the result exceeds 255), and push the result onto the stack. If there aren't
enough items on the stack to perform this operation, treat the command as a
no-op.

`-`: Pop the two topmost items off of the stack, subtract the topmost item
from the second topmost item (wrapping around to 255 if the result is less
than 0), and push the result onto the stack. If there aren't enough items on
the stack to perform this operation, treat the command as a no-op.

`!`: Swap `reg` and the topmost value on the stack. If the stack is empty, set
`reg` to 0.

`,`: Get one byte of data from the environment's input (for example, `stdin` on
a terminal), and push it onto the stack. If the stack is full, the behavior is
unspecified. In case of EOF, push 0 onto the stack.

`.`: Output the ASCII code for the value of the topmost item on the stack. If
the stack is empty, output a null character (0).

`[`: If the topmost item on the stack is 0, jump to the next matching `]`.
If the stack is empty, treat the command as a no-op.

`]`: If the topmost item on the stack is 0, jump to the previous matching `[`.
If the stack is empty, treat the command as a no-op.

(If one does not understand `[` or `]`, which this specification probably did
a bad job of explaining, refer to https://brainfuck.org/brainfuck.html)

If any other character is encountered, it must be treated as a no-op.

\- rock3tsprocket (rock-pie@hotmail.com)
