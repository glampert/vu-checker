# PlayStation 2 Vector Unit (VU) microprogram checker and validator

Checks PlayStation 2 Vector Unit microprograms for `openvcl/dvp-as` silent failures.

In the PS2 VU build pipeline, `openvcl` allocates registers and schedules, `dvp-as`
assembles - and none of them owns the whole picture, so each can turn out a
program that builds cleanly and is broken at runtime. `openvcl` also has bugs that
result in misallocation of registers and other syntactically valid but broken code.

`check_vu_code.py` performs the following checks:

- `crossloop`:   a value `openvcl` loses between two sibling loops
- `regalloc`:    `openvcl`'s register allocation, proven by reaching definitions
- `loopvar`:     a loop counter `openvcl` hands to a temporary
- `immediates`:  an immediate `dvp-as` truncates to its field width
- `latency`:     a clip flag or Q read `openvcl` did not pad across a branch
- `branches`:    a branch offset `dvp-as` wrapped, decoded from the object

`check_vu_code.py` itself contains detailed documentation for each check.

`Usage: check_vu_code.py [program.o ...]`
