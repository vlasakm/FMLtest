# FMLtest

A simple test framework and suite of blackbox correctness tests for implementations of FML interpreters and bytecode interpreters.

There are two types of tests: **source interpretation** and **bytecode interpretation**. 

Source interpretation tests provide FML source code, which is interpreted by the provided interpreter (using the `run` command). The output of the interpreter is collected and compared against expected output (injected in the comments of the source code).

Bytecode interpretation tests provide bytecode as input. The input is executed (using the interpreter's `execute` command) and the output of the interpreter is collected. This output is compared against expected output which is injected into an accompanying text file.

# Usage

In order to use the test suite, one should specify the location of the interpreter executable via the `FML` environmental variable:

```bash
export FML=/path/to/interpreter/fml
./suite fml hello_world
```

Alternatively, for one-time use:

```bash
FML=/path/to/interpreter/fml ./suite fml hello_world
FML=/path/to/elsewhere/fml ./suite fml hello_world
```

The test suite will call the interpreter using either the `run` or `execute` command. The former is used to do source interpretation form an `.fml` file, while the latter is used to do bytecode interpretation.

for example, given `FML=/path/to/interpreter/fml`, the following commands will run the following interpreter commands:

```bash
./suite fml hello_world
# /path/to/interpreter/fml run tests/fml/hello_world.fml 
```

```bash
./suite bc hello_world
# /path/to/interpreter/fml execute tests/bc/hello_world.bc 

```

## Listing available tests

To list all available source code tests:

```bash
./fml show fml 
```

Similarly, for bytecode tests:

```bash
./fml show bc
```

## Executing source interpreter tests

Run a single source interpreter test:

```bash
./suite fml hello_world
```

Multiple tests can be specified:

```bash
./suite fml hello_world roman fibonacci
```

## Executing bytecode interpreter tests

A single bytecode interpreter test:

```bash
./suite bc hello_world
```

Analoously, multiple tests:

```bash
./suite bc hello_world push_pop
```

## Executing all available tests (in bash)

With the source interpreter:

```bash
./suite fml $(./suite show fml)
```

With the bytecode interpreter:

```bash
./suite bc $(./suite show bc)
```