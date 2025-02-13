# Molecular Simulation

Molecular Dynamics is a method to perform Molecular Simulation research.

Aim:
Generate enough representative confirmations of the molecular system in such a way that accurate values of a property can be obtained.

Example:

Protein ion channel in a lipid bi-layer

Typical questions:
- How fast do ions pass through channels?
- What happens when protein residues are altered?
- How and where do ligands bind to the channel?
- How is the lipid bi-layer composed locally?


One Method:
Molecular Dynamics

## Molecular Dynamics

Uses Newton's equations of motion to generate configurations (a trajectory) for atoms of a molecular system.


$ m\frac{d^2x}{dt^2} = F $


$ F = - \nabla U $

These are integrated using a Verlet-Like integrator, the Velocity-Verlet formulation is


## GROMACS
GROMACS is a command line MD package that can perform most aspects of a MD workflow.