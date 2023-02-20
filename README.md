# FMLtest

A simple test framework and suite of blackbox correctness tests for implementations of FML interpreters and bytecode interpreters.

There are two types of test files: **fml** (FML source files) and **bc** (FML bytecode files).

There are three types of tests: **source AST interpretation**, **bytecode interpretation**, and **bytecode compilation**.

Source interpretation tests provide FML source code, which is interpreted by the provided interpreter (using the `ast_interpret` command of the interpreter). The output of the interpreter is collected and compared against expected output (injected in the comments of the source code).

Bytecode interpretation tests provide bytecode as input. The input is executed (using the interpreter's `bc_interpret` command) and the output of the interpreter is collected. This output is compared against expected output which is injected into an accompanying text file.

Bytecode compilation tests provide FML source code as input, which is parsed into an AST (using the reference parser). The AST is compiled using the compiler-under-test's `bc_compile` command and the the bytecode file is collected. The bytecode file is then executed using a (reference) bytecode interpreter. The output of the execution is collected and compared against expected output (injected in the comments of the source code).

# Usage

In order to use the test suite, one should specify the location of the interpreter executable via the `FML` environmental variable:

```bash
export FML=/path/to/interpreter/fml
./suite ast_interpret hello_world
```

Alternatively, for one-time use:

```bash
FML=/path/to/interpreter/fml ./suite ast_interpret hello_world
FML=/path/to/elsewhere/fml ./suite ast_interpret hello_world
```

The test suite will call the interpreter using either the `ast_interpret`, `bc_interpret` or `bc_compile` command, corresponding to the respective types of tests (source AST interpretation, bytecode interpretation, bytecode compilation).

For example, given `FML=/path/to/interpreter/fml`, the following commands will run the following interpreter commands:

```bash
./suite ast_interpret hello_world
# /path/to/interpreter/fml ast_interpret tests/fml/hello_world.fml
```

```bash
./suite bc_interpret hello_world
# /path/to/interpreter/fml bc_interpret tests/bc/hello_world.bc
```

```bash
./suite bc_compile hello_world
# /path/to/interpreter/fml bc_compile tests/bc/hello_world.fml > output/bc_compile/hello_world.bc
# /path/to/interpreter/fml bc_interpret output/bc_compile/hello_world.bc
```

Compilation tests can also use a specific bytecode interpreter by setting `FML_REF_BC_INT`. You usually want to set `FML_REF_BC_INT` as the reference interpreter to catch differences against the spec, but you can also test against your own interpreter (which is the default).

If the reference interpreter is configured as `FML_REF_BC_INT=/path/to/reference/interpreter/fml` the following command will run the following interpreter commands:

```bash
./suite bc_compile hello_world
# /path/to/interpreter/fml bc_compile tests/bc/hello_world.fml > output/bc_compile/hello_world.bc
# /path/to/reference/interpreter/fml bc_interpret output/bc_compile/hello_world.bc
```


## Listing available tests

To list all available source code tests:

```bash
./suite show fml
```

Similarly, for bytecode tests:

```bash
./suite show bc
```

## Executing tests

Run a single AST interpreter test:

```bash
./suite ast_interpret hello_world
```

Multiple tests can be specified:

```bash
./suite ast_interpret hello_world roman fibonacci
```

## Executing bytecode interpreter tests

A single bytecode interpreter test:

```bash
./suite bc_interpret hello_world
```

Analogously, multiple tests:

```bash
./suite bc_interpret hello_world push_pop
```

## Executing bytecode compilation tests

Before executing compilation tests, configure reference bytecode interpreter via environemtnal variable (if other than FML):

```bash
FML_REF_BC_INT=/path/to/interpreter/fml
```

A single bytecode compiler test:

```bash
./suite bc_compile hello_world
```

Analogously, multiple tests:
```bash
./suite bc_compile hello_world variable_scope
```

## Executing all available tests (in bash)

Ast interpreter tests:

```bash
./suite ast_interpret $(./suite show fml)
```

Bytecode interpreter tests:

```bash
./suite bc_interpret $(./suite show bc)
```

Bytecode compiler tests:

```bash
./suite bc_compile $(./suite show fml)
```

All tests (equivalent to all above):

```bash
./suite all
```
