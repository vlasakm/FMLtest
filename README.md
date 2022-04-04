# FMLtest

A simple test framework and suite of blackbox correctness tests for implementations of FML interpreters and bytecode interpreters.

There are three types of tests: **source interpretation**, **bytecode interpretation**, and **bytecode compilation**. 

Source interpretation tests provide FML source code, which is interpreted by the provided interpreter (using the `run` command). The output of the interpreter is collected and compared against expected output (injected in the comments of the source code).

Bytecode interpretation tests provide bytecode as input. The input is executed (using the interpreter's `execute` command) and the output of the interpreter is collected. This output is compared against expected output which is injected into an accompanying text file.

Bytecode compilation tests provide FML source code as input, which is parsed into an AST (using the reference parser). The AST is compiled using the compiler-under-test's `compile` command and the the bytecode file is collected. The bytecode file is then executed using a (reference) bytecode interpreter. The output of the execution is collected and compared against expected output (injected in the comments of the source code).

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

The test suite will call the interpreter using either the `run` or `execute` command. The former is used to do source interpretation from an `.fml` file, while the latter is used to do bytecode interpretation.

For example, given `FML=/path/to/interpreter/fml`, the following commands will run the following interpreter commands:

```bash
./suite fml hello_world
# /path/to/interpreter/fml run tests/fml/hello_world.fml 
```

```bash
./suite bc hello_world
# /path/to/interpreter/fml execute tests/bc/hello_world.bc 
```

Compilation tests are additionally configurable in terms of what parser and AST format to use by setting the environmental variables `AST_FORMAT` and `FML PARSER`:

For example, given the following settings:

```bash
AST_FORMAT=json
FML_PARSER=/path/to/parser/fml
```

The following command is executed to parse the source.

```bash
/path/to/parser/fml parse tests/comp/hello_world.fml -o output/comp/hello_world.json
```

Compilation tests can also use a specific bytecode interpreter by setting `FML_REF_BC_INT`.

If the reference interpreter is configured as `FML_REF_BC_INT=/path/to/interpreter/fml` the following command is executed to run some compiled bytecode file:

```bash
/path/to/interpreter/fml execute 
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

Analogously, multiple tests:

```bash
./suite bc hello_world push_pop
```

## Executing bytecode compilation tests

Before executing compilation tests, configure reference parser and bytecode interpreters via environemtnal variables (if other than FML).

```bash
AST_FORMAT=json
FML_PARSER=/path/to/parser/fml
FML_REF_BC_INT=/path/to/interpreter/fml
```

A single bytecode compiler test:

```bash
./suite comp hello_world
```

Analogously, multiple tests:
```bash
./suite comp hello_world variable_scope
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