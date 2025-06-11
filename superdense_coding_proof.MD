## A case-by-case proof / analysis of the superdense coding protocol

I've been learning about quantum computing using [this great course](https://learning.quantum.ibm.com/course/basics-of-quantum-information) and during the lesson on entanglement I noticed that the section on superdense coding didn't have as thorough of an analysis as teleportation did. Since I wanted to make sure I understand the protocol correctly, I decided to write down a case-by-case analysis of the protocol here.


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
I \otimes Z = \begin{bmatrix}1 & 0 \\0 & 1\end{bmatrix} \otimes \begin{bmatrix}1 & 0 \\0 & -1\end{bmatrix} = \begin{bmatrix}1&0&0&0\\0&-1&0&0\\0&0&1&0\\0&0&0&-1\end{bmatrix}
```

So we apply the above operation to the following state:

$\left| \phi^+ \right\rangle=\frac{|00\rangle + |11\rangle}{\sqrt{2}}$

The state of the two-qubit system changes:

```math
\begin{bmatrix}1&0&0&0\\0&-1&0&0\\0&0&1&0\\0&0&0&-1\end{bmatrix} \frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\1\end{bmatrix}=\frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\-1\end{bmatrix}=\left| \phi^- \right\rangle
```

#### Case 3: c=1 d=0

We only apply X-gate to the second qubit, so let's calculate what I tensorred with X is:

```math
I \otimes X = \begin{bmatrix}1 & 0 \\0 & 1\end{bmatrix} \otimes \begin{bmatrix}0 & 1 \\1 & 0\end{bmatrix} = \begin{bmatrix}0&1&0&0\\1&0&0&0\\0&0&0&1\\0&0&1&0\end{bmatrix}
```

We apply it to the $\left| \phi^+ \right\rangle$ state:

```math
\begin{bmatrix}0&1&0&0\\1&0&0&0\\0&0&0&1\\0&0&1&0\end{bmatrix} \frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\1\end{bmatrix}=\frac{1}{\sqrt{2}}\begin{bmatrix}0\\1\\1\\0\end{bmatrix}=\left| \psi^+ \right\rangle
```


#### Case 4: c=1 d=1

We apply Pauli Z, followed by Pauli X to the second qubit. Since applying two gates in a sequence is represented by multiplication of two matrices (remembering that the order of the two matrices is reversed in matrix notation), we can represent this operation (on the second qubit) as a single matrix:

```math
XZ = \begin{bmatrix}0 & 1 \\1 & 0\end{bmatrix}\begin{bmatrix}1 & 0 \\0 & -1\end{bmatrix}=\begin{bmatrix}0 & -1 \\1 & 0\end{bmatrix}
```

Since this is only applied on the second qubit, we tensor Pauli-I with the above matrix, resulting in:

```math
\begin{bmatrix}1 & 0 \\0 & 1\end{bmatrix} \otimes \begin{bmatrix}0 & -1 \\1 & 0\end{bmatrix} = \begin{bmatrix}0&-1&0&0\\1&0&0&0\\0&0&0&-1\\0&0&1&0\end{bmatrix}
```

Finally, we apply this operation (to the $\left| \phi^+ \right\rangle$ state we always start with):

```math
\begin{bmatrix}0&-1&0&0\\1&0&0&0\\0&0&0&-1\\0&0&1&0\end{bmatrix} \frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\1\end{bmatrix}=\frac{1}{\sqrt{2}}\begin{bmatrix}0\\1\\-1\\0\end{bmatrix}=\left| \psi^- \right\rangle
```

As we can see, by performing operations on her own qubit Alice puts the two entangled qubits in one of the four Bell states. 

Since the Bell states form an orthonormal basis, they are perfectly distinguishable. This means that (after applying the operations analyzed in the next section), Bob can always tell which of the cases above occured. For now, Alice sends the single qubit (her half of the entangled pair) to Bob.

### Decoding

Bob now performs a CNOT operation (with the qubit that was sent being the control and his part of the entangled pair the target), followed by a Hadamard on the previously-transferred qubit.

What we need to show now is that these two operations correctly transform each of the Bell basis states to the standard basis vectors corresponding to the original values of the classical bits.

Let's first try to represent the two operations (CNOT, Hadamard) with a single matrix.

CNOT is represented by the following matrix:

```math
\begin{bmatrix}1&0&0&0\\0&0&0&1\\0&0&1&0\\0&1&0&0\end{bmatrix}
```

This can be checked column-by-column. I.e. the first column (corresponding to the $\left|00 \right\rangle$ state) has a 1 in the first entry, which mean means that it transforms that state to itself. The second column (corresponding to the $\left|01 \right\rangle$ state) has a 1 in the third entry, meaning that upon acting on that state it transforms it to $\left|11 \right\rangle$ , which is the correct behaviour considering that the second qubit acts as the control.

Since the Hadamard gate only acts on the second qubit in this protocol, we need to calculate I tensorred with H:

```math
I \otimes H = \frac{1}{\sqrt{2}}\begin{bmatrix}1 & 0 \\0 & 1\end{bmatrix} \otimes \frac{1}{\sqrt{2}}\begin{bmatrix}1 & 1 \\1 & -1\end{bmatrix} = \frac{1}{\sqrt{2}}\begin{bmatrix}1&1&0&0\\1&-1&0&0\\0&0&1&1\\0&0&1&-1\end{bmatrix}
```

Therefore the operation that Bob performs on the two qubits he now hold can be represented by the following matrix:

```math
\frac{1}{\sqrt{2}}\begin{bmatrix}1&1&0&0\\1&-1&0&0\\0&0&1&1\\0&0&1&-1\end{bmatrix}\begin{bmatrix}1&0&0&0\\0&0&0&1\\0&0&1&0\\0&1&0&0\end{bmatrix} = \frac{1}{\sqrt{2}}\begin{bmatrix}1&0&0&1\\1&0&0&-1\\0&1&1&0\\0&-1&1&0\end{bmatrix}
```

Let's analyze how it transforms each of the bell states:

#### Case 1: $\left| \phi^+ \right\rangle$ state

```math
\frac{1}{\sqrt{2}}\begin{bmatrix}1&0&0&1\\1&0&0&-1\\0&1&1&0\\0&-1&1&0\end{bmatrix}\frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\1\end{bmatrix}=\frac{1}{2}\begin{bmatrix}2\\0\\0\\0\end{bmatrix}=\begin{bmatrix}1\\0\\0\\0\end{bmatrix}
```

Measuring the two qubits will always result in the classical result 00

#### Case 2: $\left| \phi^- \right\rangle$ state

```math
\frac{1}{\sqrt{2}}\begin{bmatrix}1&0&0&1\\1&0&0&-1\\0&1&1&0\\0&-1&1&0\end{bmatrix}\frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\-1\end{bmatrix}=\frac{1}{2}\begin{bmatrix}0\\2\\0\\0\end{bmatrix}=\begin{bmatrix}0\\1\\0\\0\end{bmatrix}
```

The calculation above is a good example of how relative phase causes some amplitudes in the quantum state vector to get amplified, while others are cancelled out due to the -1 entry.

Measuring the two qubits will always result in the classical result 01

#### Case 3: $\left| \psi^+ \right\rangle$ state

```math
\frac{1}{\sqrt{2}}\begin{bmatrix}1&0&0&1\\1&0&0&-1\\0&1&1&0\\0&-1&1&0\end{bmatrix}\frac{1}{\sqrt{2}}\begin{bmatrix}0\\1\\1\\0\end{bmatrix}=\frac{1}{2}\begin{bmatrix}0\\0\\2\\0\end{bmatrix}=\begin{bmatrix}0\\0\\1\\0\end{bmatrix}
```

Measuring the two qubits will always result in the classical result 10

#### Case 4: $\left| \psi^- \right\rangle$ state

```math
\frac{1}{\sqrt{2}}\begin{bmatrix}1&0&0&1\\1&0&0&-1\\0&1&1&0\\0&-1&1&0\end{bmatrix}\frac{1}{\sqrt{2}}\begin{bmatrix}0\\1\\-1\\0\end{bmatrix}=\frac{1}{2}\begin{bmatrix}0\\0\\0\\-2\end{bmatrix}=\begin{bmatrix}0\\0\\0\\-1\end{bmatrix}
```

Here we get a standard basis vector  $\left| 11 \right\rangle$ modified by a global phase factor of -1. Since global phase does not matter, when the two qubits are measured they will always result in two classical bits 11.
