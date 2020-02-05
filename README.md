Polly - Polyhedral optimizations for LLVM
-----------------------------------------
http://polly.llvm.org/

Polly uses a mathematical representation, the polyhedral model, to represent and
transform loops and other control flow structures. Using an abstract
representation it is possible to reason about transformations in a more general
way and to use highly optimized linear programming libraries to figure out the
optimal loop structure. These transformations can be used to do constant
propagation through arrays, remove dead loop iterations, optimize loops for
cache locality, optimize arrays, apply advanced automatic parallelization, drive
vectorization, or they can be used to do software pipelining.

Declarative Loop Tactics for Domain-specific Optimization 
---------------------------------------------------------
Increasingly complex hardware makes the design of effective compilers
difficult. To reduce this problem, we introduce Declarative Loop Tactics, which
is a novel framework of composable program transformations based on an internal
tree-like program representation of a polyhedral compiler. The framework is
based on a declarative C++ API built around easy-to-program matchers and
builders, which provide the foundation to develop loop optimization strategies.
Using our matchers and builders, we express computational patterns and core
building blocks, such as loop tiling, fusion, and data-layout transformations,
and compose them into algorithm-specific optimizations. Declarative Loop
Tactics (Loop Tactics for short) can be applied to many domains. For two of
them, stencils and linear algebra, we show how developers can express
sophisticated domain-specific optimizations as a set of composable
transformations or calls to optimized libraries. By allowing developers to add
highly customized optimizations for a given computational pattern, we expect
our approach to reduce the need for DSLs and to extend the range of
optimizations that can be performed by a current general-purpose compiler.

Contributors:

Alex Zinenko (https://ozinenko.com/)

Lorenzo Chelini (l.chelini@tue.nl / l.chelini@icloud.com)

Tobias Grosser (https://www.grosser.es/)

Publications:

TACO
https://dl.acm.org/doi/abs/10.1145/3372266

HAL
https://hal.inria.fr/hal-01965599/document

How to install:

```
#!/bin/bash 

export BASE=`pwd`
export LLVM_SRC=${BASE}/llvm
export POLLY_SRC=${LLVM_SRC}/tools/polly
export CLANG_SRC=${LLVM_SRC}/tools/clang
export LLVM_BUILD=${BASE}/llvm_build

set -e

if [ -e /proc/cpuinfo ]; then
    procs=`cat /proc/cpuinfo | grep processor | wc -l`
else
    procs=1
fi

if ! test -d ${LLVM_SRC}; then
    git clone http://llvm.org/git/llvm.git ${LLVM_SRC}
    (cd ${LLVM_SRC} && git reset --hard origin/release_80)
fi

if ! test -d ${POLLY_SRC}; then
    git clone https://github.com/LoopTactics/5LIM0-Parallelization-Compilers-and-Platforms-Polly.git ${POLLY_SRC}
fi

if ! test -d ${CLANG_SRC}; then
    git clone http://llvm.org/git/clang.git ${CLANG_SRC}
    (cd ${CLANG_SRC} && git reset --hard origin/release_80)
fi

mkdir -p ${LLVM_BUILD}
cd ${LLVM_BUILD}

cmake ${LLVM_SRC} -DLLVM_TARGETS_TO_BUILD="host" -DCMAKE_BUILD_TYPE=Release
make -j$procs -l$procs
make check-polly
```

What is: 
A modified version of Polly with Loop Tactics embedded. It shows how
we can declaratively recognize and optimize a GEMM kernel. We follow the
optimization steps described in:

https://core.ac.uk/download/pdf/61493164.pdf

https://dl.acm.org/doi/10.1145/3235029

This is an assignment for the compiler course at TU Eindhoven
(http://www.es.ele.tue.nl/~rjordans/5LIM0.php)

More information:
Write to l.chelini@tue.nl / l.chelini@icloud.com

