---
# Mandatory fields. See more on aka.ms/skyeye/meta.
title: Q# basics | Microsoft Docs 
description: Basic concepts of Q#
author: QuantumWriter
ms.author: Christopher.Granade@microsoft.com
ms.date: 12/11/2017
ms.topic: article
uid: microsoft.quantum.manual.basics
# Use only one of the following. Use ms.service for services, ms.prod for on-prem. Remove the # before the relevant field.
# For Quantum products none of these categories have been defined  yet.
# ms.service: service-name-from-white-list
# ms.prod: product-name-from-white-list
# ms.technology: tech-name-from-white-list
---

# Q# Basics

In this section we will present a brief introduction to the basic building blocks of Q#.

## What is a quantum program?

From a technical perspective, a quantum program can be seen as a particular set of classical functions which, when called, perform certain operations on a quantum system.
An important consequence of that view is that a program written in Q# does not directly model qubits themselves, but rather describes how a classical control computer interacts with those qubits.
By design, Q# thus does not define quantum states or other properties of quantum mechanics directly.
For instance, consider the state $\ket{+} = \left(\ket{0} + \ket{1}\right) / \sqrt{2}$ discussed in the [Quantum Computing Concepts](xref:microsoft.quantum.concepts.intro) guide.
To prepare this state in Q#, we use the facts that the qubits are initialized in the $\ket{0}$ state, and that $\ket{+} = H\ket{0}$, where $H$ is the [Hadamard](xref:microsoft.quantum.intrinsic.h) transform:

```qsharp
using (qubit = Qubit()) {
    // At this point, qubit is in the state |0>.
    H(qubit);
    // We've now applied H, such that our qubit is in H|0> = |+>, as we wanted.
}
```

## Quantum states in Q#

Importantly, in writing the above program, we did not explicitly refer to the state within Q#, but rather described how the state was *transformed* by our program.
Thus, similar to how a graphics shader program accumulates a description of transformations to each vertex, a quantum program in Q# accumulates transformations to quantum states.
This allows us to be entirely agnostic about what a quantum state even *is* on each target machine, which might have different interpretations depending on the machine. 

A Q# program has no ability to introspect into the state of a qubit.
Rather, a program can call operations such as `Measure` to learn information from a qubit, and call operations such as `X` and `H` to act on the state of a qubit.
What these operations actually *do* is only made concrete by the target machine we use to run the particular Q# program.
For example, if running the program on our [[full-state simulator](xref:microsoft.quantum.machines.full-state-simulator), the simulator will perform the corresponding mathematical operations to the simulated quantum system.
But looking toward the future, when the target machine is a real quantum computer, calling such operations in Q# will direct the quantum computer to perform the corresponding *real* operations on the *real* quantum system (e.g. precisely timed laser pulses).

A Q# program recombines these operations as defined by a target machine to create new, higher-level operations to express quantum computation.
In this way, Q# makes it easy to express the logic underlying quantum and hybrid quantum-classical algorithms, while also being general with respect to the structure of a target machine or simulator.

## Q# operations and functions

Concretely, a Q# program is comprised of *operations*, *functions*, and any user-defined types. 
Operations are used to describe the transformations of quantum systems and are the most basic building block of Q# programs. 
Each operation defined in Q# may then call any number of other operations.

In contrast to operations, functions are used to describe purely classical behavior and do not have any effects besides computing classical values. 
For example, suppose we would like to measure our qubits at the end of a program, and add the measurement results to an array.
In this case `Measure` is an *operation* which instructs the target machine to perform a measurement on the (real or simulated) qubits, and the classical process of adding the returned results to an array will be handled by *functions*.

Q# is a strongly typed language and comes with a set of built-in primitive types---both standard (`Int`, `Bool`, `String`, etc.) and quantum-specific (e.g. `Qubit` and `Result`, the type returned by `Measure`)---as well as support for user defined types. 

Throughout the rest of this guide, we will show you how to use Q# to construct complex quantum programs through the basic building blocks of operations, functions, and types. 

## Q# syntax overview

The syntax of a language describes the different combinations of symbols that form a syntactically correct program.
In Q# we can classify the elements of its syntax in three different groups: types, expressions and statements.

### Types
Q# is a strongly-typed language, such that careful use of types can help the compiler provide strong guarantees about Q# programs at compile time.
Q# provides both primitive types, which can be used directly, and a variety of ways to produce new types from other types.

### Expressions
An expression in a programming language is a combination of one or more constants, variables, operators, and functions that the programming language interprets and computes to produce another value.
For example, `2+3` is an arithmetic and programming expression which evaluates to `5`.
Q# has its own expressions and rules.

### Statements 
  A statement is a syntactic unit of an imperative programming language that expresses some action to be carried out.
  Statements contrast with expressions in that statements do not return results and are executed solely for their side effects, while expressions always return a result and often do not have side effects at all.
  This distinction is frequently observed in wording: a statement is executed, while an expression is evaluated.
  An example of a Q# statement is the loop `for` which supports iteration over an integer range.


## Q# source and host files

`******` ADDITION NEEDED: picture and text describing the purpose/role of source files vs. host files. I think we have have this written somewhere, but a diagram would be nice too.
Might be good to have this even before we get into the above section

Minimally, a Q# source file consists of a *namespace declaration*, which specifies a .NET namespace which will contain the definitions in the source file.
Definitions from other Q# source files and libraries can be included using the `open` statement.
For instance, most of the operations defining fundamental gates are defined in the <xref:microsoft.quantum.intrinsic> namespace.
To make this available to our code, we simply `open` that namespace at the top of each file:

```qsharp
namespace Example {
    open Microsoft.Quantum.Intrinsic;

    // ...
}
```

Within a namespace, each Q# source file can define any combination of *operations*, *functions*, and *user-defined types*.
Within this manual, we will describe each in turn and provide examples of how they can be used in practice to make useful quantum programs.


`*****` ADDITION NEEDED: direct them to the proper next page in the manual