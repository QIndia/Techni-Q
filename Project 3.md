
# Project 3
Make your own QC simulator

## Problem Statement
As we saw in the Qoffee session, quantum computers can be simulated on classical computers (albeit with exponential cost), i.e. quantum computers are not super-Turing.
Why make one? Well, we are currently in the NISQ era with not many qubits, but also those qubits are noisy. So as long as you just want to develop a proof-of-concept of your quantum application and test the algorithm, you are better off with a QCSim as you don't have to worry about noise and limits on gates, connectivity, etc.
Modern QC simulators (like those in Qiskit, QX, Forest, etc.) use many smart techniques to reduce the total computational cost of simulating.
It's a battle that cannot be won, yet must nevertheless be fought (at least till we have Fault-Tolerant QC).

In this project we will code up our own handy QCSim and experience the 'curse of dimensionality' first-hand.

## Implementation
* Though it is more efficient to make a QCSim in C++, in this project we will stick to Python and NumPy for a working demo.
* The first thing you need is a parser for a langage definition. This is a rudimentary compiler that will interpret the assembly code (QASM) written by the programmer. Start with a basic QASM format, e.g.
```
Q [3]
H [0]
T [2]
CX [1,2]
D
MZ [2]
```
* each line is a keyword followed by a bracket defining the parameter. The (case-sensitive) keyword can be either Q, H, T, CX, D, MZ standing for total Qubits; Hadamard; T-gate; Controlled NOT; Display state, Measure-Z.
* you can add your own set of quantum gates like X, Y, Z, RX, RY, RZ, S, SWAP... but the above set of H, T, CX is 'theoretically' universal and can do everything a quantum computer can. Why 'theoretically'? Well, it might not be efficient to break every unitary down to these gate-set and in the worst case be exponential, removing the benefit of using a QC in the first place. But for now, let's take that for our simple QCSim.
* parse such a text file based on the filename supplied at runtime, by reading each line, detecting the keywork and the parameters within the bracket.
* as an if-else statement, if encountered with:
    * `Q [n]`, initialize a complex NumPy array `Psi` of size `2^n` with `1+0j` at the 0th index (for the all-zero computational basis), and `0+0j` otherwise. This is the ground state.
    * When encountered with [H](https://qiskit.org/documentation/stubs/qiskit.circuit.library.HGate.html#qiskit.circuit.library.HGate) or [T](https://qiskit.org/documentation/stubs/qiskit.circuit.library.TGate.html), first extend the matrix of H/T to `U` of `2^n x 2^n` by using the [kroneker product](https://numpy.org/doc/stable/reference/generated/numpy.kron.html) of NumPy with the Identity matrix. So, if it is `H [1]`, and there are 4 qubits, you need to do `I2.I2.H.I2` (LSB is qubit 0, the dot represent the kron).
    * When encountered with CX [a,b], check if `a=b` then flag an error. For `a<b` or `a>b` and select the [appropriate](https://qiskit.org/documentation/stubs/qiskit.circuit.library.CXGate.html#qiskit.circuit.library.CXGate) matrix. Then apply the kroneker product to extend the unitary `U` to the  `2^n x 2^n` size. Note that things can get tricky if `b` is not `a+1` or `a-1`, so for now consider only local CNOTs. In fact 2-local Hamiltonians are also universal, so with only nearest neighbour interactions we can implement all quantum algorithm 'theoretically'. By now you know what 'theoretically' means!
    * Apply the extended unitary `U` on the state by multiplying it as a matrix product from the left side. i.e. `U Psi`
    * If Display state is encountered print the `Psi` state. You can format it to make it more readable. Note that this is strictly not possible in an actual QC, but since we are keeping track of all the hidden variables in the QCSim, we can access the full state vector to use it for debugging our quantum algorithm.
    * If Measure-Z is encountered, for each basis state, find the probability of measuring that qubit in the 0 or 1 state. Add them up to find the total probability of measuring that qubit in the 0 or 1 state. Use an uniform random number of Python to generate a number between 0 and 1. If that number if below p (the probability of getting a 0), print that the qubit collapsed to 0, otherwise print the qubit collapsed to 1.

## Testing
Check the implemented QCSim is working using:
* A simple quantum coin flip
```
Q [1]
H [0]
MZ [0]
```
* A Bell state preparation
```
Q [2]
H [0]
CX [0,1]
D
```
* [extended] Grover search on 3 qubits
* [optional] Check the limits of your computing hardware. All you need to do is initialize more and more qubits using the Q command. You can check how long it takes to execute a single Hadamard gate when the number of qubits increase by putting a timer around the code of the Hadamard gate. !!! warning, the final limit might crash or hang your system !!!
```
Q [n]
H [0]
```

## Reflections
* QCSim is a very active field of research. E.g after the Google supremacy experiment, IBM used a smart simulation technique to counterargue that it can be simulated on their supercomputer in reasonable time.
* There can be many tricks to speed up the computation. Why are we storing `0+0j` basis states!? Well most QCSims use sparse matrix representations to store only non-zero amplitudes. That works well, unless your algorithm start with a Hadamard on all qubits! So there's no general technique to speedup but many techniques use the structure of the algorithm to make the simulator efficient (that's why we use random circuit for Q supremacy so that such techniques won't work).
* If there are 2 gates on different qubits, e.g. `H [0]` and `H [1]`, instead of doing `U = kron(I,H)`, apply `U Psi`, `U = kron(H,I)`, apply `U Psi`; we can do `U = kron(H,H)`, apply `U Psi`. That is a huge reduction in computation, not only for simulators, but also in real QC. If some gates can be applied in parallel, it is better to do it. This is called scheduling. There are many algorithms to find a good schedule for a given circuit/qasm code.
* So now you know how to tame the Quantum dragon! (at least when it is not breathing fire ...)

## Queries
Ping Aritra (prince-ph0en1x#9529) on the techniq-discussions Discord channel of QIndia.
