## A case-by-case proof / analysis of the superdense coding protocol

I've been learning about quantum computing using [this great course](learning.quantum.ibm.com/course/basics-of-quantum-information) and during the lesson on entanglement I noticed that the section on superdense coding didn't have as thorough of an analysis as teleportation did. Since I wanted to make sure I understand the protocol correctly, I decided to write down a case-by-case analysis of the protocol here.


### Objective of the protocol

To send two classical bits of information (marked `c` and `d`) from Alice to Bob by sending a single qubit (and utilizing the fact that it's entangled with a qubit in Bob's posession)

## Analysis

I'll be referencing the circuit used in the aforementioned course ([in particular, here](https://learning.quantum.ibm.com/course/basics-of-quantum-information/entanglement-in-action#superdense-coding)) and using similar notation so make sure you have that open.

### Starting point

The two entangled qubits start in the following state:

$\left| \phi^+ \right\rangle=\frac{|00\rangle + |11\rangle}{\sqrt{2}}$

### Encoding

#### Case 1: c=0 d=0

If both classical bits we want to transmit are 0, then we don't apply any operations to the qubits (= we apply a 4x4 identity matrix) so they are left in the $\left| \phi^+ \right\rangle$ state

#### Case 2: c=0 d=1

Here we apply a Z gate to the qubit we will transfer. Since we only apply it to a part of the system (and the qubits are entangled), the operation is represented by the Identity matrix tensorred with the Z-gate's matrix. 

An easy way to remember the tensor product of two matrices is that for each entry in the matrix on the left you put the entire matrix to the right multiplied by that entry.

In particular:

```math
$I \otimes Z = \begin{bmatrix}1 & 0 \\0 & 1\end{bmatrix} \otimes \begin{bmatrix}1 & 0 \\0 & -1\end{bmatrix} = \begin{bmatrix}1&0&0&0\\0&-1&0&0\\0&0&1&0\\0&0&0&-1\end{bmatrix}$
```

So we apply the above operation to the following state:

$\left| \phi^+ \right\rangle=\frac{|00\rangle + |11\rangle}{\sqrt{2}}$

The state of the two-qubit system changes:

```math
$\begin{bmatrix}1&0&0&0\\0&-1&0&0\\0&0&1&0\\0&0&0&-1\end{bmatrix} \frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\1\end{bmatrix}=\frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\-1\end{bmatrix}=\left| \phi^- \right\rangle$
```
