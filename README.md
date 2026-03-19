# quantum-exploration

> A personal sandbox for exploring quantum computing algorithms.

![Status](https://img.shields.io/badge/status-work%20in%20progress-yellow)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## Overview

`quantum-exploration` is an experimental repository documenting a hands-on journey through quantum computing algorithms. The focus is on building intuition from the ground up. Implementing, breaking, and understanding foundational and advanced quantum algorithms through code and experimentation.

Topics currently being explored:

- **Superposition & Interference** — single and multi-qubit circuits
- **Grover's Algorithm** — unstructured search with quadratic speedup
- **Shor's Algorithm** — integer factorization via quantum Fourier transform
- **Variational Quantum Eigensolver (VQE)** — hybrid classical-quantum optimization
- **Quantum Teleportation** — entanglement-based state transfer protocols

> 🚧 This repo is a living notebook. Implementations may be incomplete, comments may be dense, and approaches may change as understanding deepens.

---

## Usage Examples

### Running a Circuit (Qiskit)

```python
from qiskit import QuantumCircuit, transpile
from qiskit_aer import AerSimulator

# Build a simple Bell state (entangled pair)
qc = QuantumCircuit(2, 2)
qc.h(0)       # Hadamard gate — puts qubit 0 in superposition
qc.cx(0, 1)   # CNOT gate — entangles qubit 0 and qubit 1
qc.measure([0, 1], [0, 1])

# Simulate
simulator = AerSimulator()
compiled = transpile(qc, simulator)
result = simulator.run(compiled, shots=1024).result()
print(result.get_counts())
# Expected: {'00': ~512, '11': ~512}
```

### Grover's Search (2-qubit example)

```python
from qiskit import QuantumCircuit

def grover_2qubit(target: str) -> QuantumCircuit:
    """Build a 2-qubit Grover circuit for a given target bitstring."""
    qc = QuantumCircuit(2, 2)

    # Initialization — equal superposition
    qc.h([0, 1])

    # Oracle — phase flip on target state
    if target == "11":
        qc.cz(0, 1)
    # (extend for other targets as needed)

    # Diffusion operator
    qc.h([0, 1])
    qc.x([0, 1])
    qc.cz(0, 1)
    qc.x([0, 1])
    qc.h([0, 1])

    qc.measure([0, 1], [0, 1])
    return qc

circuit = grover_2qubit("11")
circuit.draw("text")
```

### Quantum Fourier Transform

```python
import numpy as np
from qiskit import QuantumCircuit

def qft(n: int) -> QuantumCircuit:
    """Build an n-qubit Quantum Fourier Transform circuit."""
    qc = QuantumCircuit(n)
    for j in range(n):
        qc.h(j)
        for k in range(j + 1, n):
            qc.cp(np.pi / 2 ** (k - j), j, k)
    # Swap qubits to correct bit ordering
    for i in range(n // 2):
        qc.swap(i, n - i - 1)
    return qc

qft_circuit = qft(4)
qft_circuit.draw("text")
```

---

## Structure

```
quantum-exploration/
├── circuits/          # Reusable circuit building blocks
├── algorithms/        # Full algorithm implementations
│   ├── grover.py
│   ├── shor.py
│   └── vqe.py
├── notebooks/         # Jupyter notebooks for experimentation
└── utils/             # Helpers for visualization and simulation
```

---

## Requirements

- Python 3.9+
- [Qiskit](https://qiskit.org/) — `pip install qiskit`
- [Qiskit Aer](https://github.com/Qiskit/qiskit-aer) — `pip install qiskit-aer`
- Jupyter (optional, for notebooks) — `pip install notebook`

---

## Roadmap

- [ ] Complete Shor's algorithm implementation
- [ ] Add noise models to simulations
- [ ] Explore QAOA (Quantum Approximate Optimization Algorithm)
- [ ] Benchmark circuit depths across algorithms
- [ ] Add visual circuit diagrams to each example

---

## License

MIT — use freely, attribution appreciated.
