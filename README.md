# EAGLE ULPs

Autodesk EAGLE User Language Programs (ULPs)

## Trace BMP

ULP that draws custom board shapes using EAGLE's built-in `import-bmp.ulp`.

I wrote a [blog post](https://www.nathancheek.com/blog/developing-an-eagle-user-language-program.html) about the development of this program.

## CMD Renumber

Fork of `cmd-renumber.ulp` provided by EAGLE. This fixes a bug which caused the specified `bottom suffix` to be unnecessarily ignored.

This program allows the user to specify a starting suffix of all components on the bottom of the board. This makes it easy to know at a glance where a part is on the board. For example, if you specify a bottom suffix of `100`, then the first resistor on the bottom will be named `R100` and so on. The first resistor on the top of the board will be named `R1`. But what if you have more than 100 resistors on the top? That option will no longer work, so it is ignored and the program simply keeps counting where it left off when switching board sides.

However, there was a bug embedded in this logic. Relying on `B.elements(E)` to loop through the elements ordered by name, the program simply has to increment a counter (`number_prefix`) until it begins to see a new prefix. At this point, the counter is compared against the `largest_number_prefix` variable, which is updated if a larger number has just been found. Later, the program checks `largest_number_prefix` against the user-supplied `bottom suffix` to decide whether or not the suffix feature can be used. The problem was that `number_prefix` was only reset when `largest_number_prefix` got updated. That counter should actually be reset every time a new prefix is found. This version fixes that bug by moving the reset statement out of the `largest_number_prefix` conditional statement.