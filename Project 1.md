# Project 1
Variational Quantum Eigensolver


## Problem Statement
Find the [expectation value](https://en.wikipedia.org/wiki/Expected_value) of the Hamiltonian Z ⊗ Z, with respect to an arbitrarily constructed trial state using [parametrize quantum circuit](https://medium.com/qiskit/we-need-new-language-to-discuss-parameterized-quantum-circuits-60672f189b5f) in [Qiskit](https://qiskit.org/). Where Z = z-component of Pauli vector.

<img src="https://github.com/qIndia/Techni-Q/blob/main/Images/VQE.png" width="1000">

## VQE Subroutines
The [Variational quantum eigensolver](https://towardsdatascience.com/the-variational-quantum-eigensolver-explained-adcbc9659c3a) consists a total of three subroutines:
* **Trial State Preparation:** This can be done by choosing an arbitrary quantum gate
sequence (ansatz), depending on some parameters i.e. angles.
* **Finding the Energy:** After preparing the initial state one need to find the expectation
value of Hamiltonian (i.e. energy) on the basis of the prepared trial state.
* **Minimize the Energy:** This step can be efficiently done by using a completely classical process, and by choosing an optimization method from the [scipy.optimize](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.minimize.html) documentation of Python.

## Elaboration of Problem Statement
This project can be completed by using only first two subroutines of VQE process. 
Hence your task will be to:
* Construct a trial wave function using an arbitrary parametrized quantum circuit i.e. ansatz based on [single qubit gates](https://qiskit.org/textbook/ch-states/single-qubit-gates.html) and [two qubit rotating gates](https://qiskit.org/textbook/ch-gates/multiple-qubits-entangled-states.html).
* Find the expectation value of Z ⊗ Z, in the basis of the trial wave function i.e. Expectation Value = ⟨trial state|Z ⊗ Z|trial state⟩

 **The Trial state can be obtained by using the [Statevector Simulator](https://qiskit.org/documentation/tutorials/circuits/1_getting_started_with_qiskit.html) after running the parametrized quantum circuit mentioned in the first step i project elaboartion.**

## Other Resources
1. Michał Stęchły's blogpost: https://www.mustythoughts.com/variational-quantum-eigensolver-explained
2. Frank Zickert's Article: https://towardsdatascience.com/the-variational-quantum-eigensolver-explained-adcbc9659c3a
3. JavaFXpert's article: https://medium.com/qiskit/the-variational-quantum-eigensolver-43f7718c2747
4. Prof. K.L. Sebastian's lecture on Variational Methods(the backbone of VQE): https://www.youtube.com/watch?v=T-wXwgS7MuI
