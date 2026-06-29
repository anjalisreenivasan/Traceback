# Traceback — an AI-friendly C debugger experiment

Traceback is an early exploration of turning a C program's runtime state into data an AI can reason about. It compiles a program with debug symbols, steps through it with GDB, and records the variables visible at each source line as a JSONL execution trace.

## What works so far

- One-command compile and trace flow for a C source file
- Line-by-line snapshots of local variable names and values
- Sequentially numbered JSONL output, so repeated runs are preserved
- Filtering that skips library and assembly frames outside the workspace
- A Docker environment with GCC, GDB, and Python ready to use
- Exploratory cases covering loops, arrays, pointers, heap allocation, strings, and user input

## Interesting discoveries

- Repeated line entries naturally capture how variables change during loops.
- Pointer traces expose both addresses and dereferenced values, offering a useful starting point for explaining memory behavior.
- Values observed before initialization can look surprising or invalid; preserving them in the trace may help an AI identify undefined behavior rather than only inspecting the final state.
- Stepping into library code creates a lot of noise, so keeping the trace scoped to the user's workspace is important.

## Try it

With GCC, GDB, and Python installed:

```bash
python3 gdb_compiler.py test_2.c
```

This creates a file such as `test_2_1.jsonl` containing the captured execution states.

## Still exploratory

The prototype currently focuses on local variables in the selected stack frame. Global and static variables, richer function-call tracking, multiple stack frames, structured pointer/array values, and the AI question-answering layer are possible next steps.
