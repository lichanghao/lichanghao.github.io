---
title: "Onsager reprocity and fluctuation-dissipation-theorem"
date: 2024-12-11
permalink: /posts/2024/11/18/onsager-reciprocity-and-fluctuation-dissipation-theorem
tags: physics
---

This blog post is the notes on the relation (or difference) between Onsager reciprocity and fluctuation-dissipation-theorem.

## Relation Between Fluctuation-Dissipation Theorem and Onsager Reciprocity

The fluctuation-dissipation theorem (FDT) and Onsager reciprocity relations are foundational principles in non-equilibrium statistical mechanics, describing the behavior of systems *near thermodynamic equilibrium*. They are deeply interconnected, as both arise from the underlying time-reversal symmetry of microscopic dynamics.

---

### **Fluctuation-Dissipation Theorem (FDT):**
- The FDT links spontaneous fluctuations in a system at equilibrium to its response to external perturbations.
- Mathematically, it states that the response function (e.g., susceptibility or diffusivity) of a system to a small external force is directly proportional to the correlation function of spontaneous fluctuations in the system.
- Example:

$$ \langle \delta A(t) \delta A(0) \rangle \propto \chi(t), $$

  where $\langle \delta A(t) \delta A(0) \rangle$ is the autocorrelation function of a quantity $A$, and $ \chi(t)$ is the linear response to a perturbation.

- The FDT ensures that the dissipation in the system (energy loss due to an external perturbation) is governed by the same mechanisms that generate equilibrium fluctuations.

---

### **Onsager Reciprocity Relations:**
- The Onsager reciprocity relations state that, for systems near equilibrium, the cross-coupled transport coefficients are symmetric:
  \[
  L_{ij} = L_{ji}.
  \]
  Here, \( L_{ij} \) describes the linear response of a flux \( J_i \) (e.g., heat, mass, or momentum flux) to a thermodynamic force \( X_j \) (e.g., temperature gradient or chemical potential difference).

- This symmetry arises from the **microscopic reversibility** (time-reversal symmetry) of the underlying dynamics.

---

### **Relation Between FDT and Onsager Reciprocity:**

1. **Common Origin**:
   - Both FDT and Onsager reciprocity are consequences of time-reversal symmetry in equilibrium thermodynamics.
   - Onsager’s derivation of the reciprocity relations relies on the same microscopic reversibility that underpins the FDT.

2. **Fluctuations and Linear Response**:
   - The FDT directly connects equilibrium fluctuations to the linear response of a system. Onsager reciprocity extends this idea to cross-coupled responses in systems with multiple interacting forces and fluxes.

3. **Dissipation and Transport Coefficients**:
   - The FDT shows that dissipation (characterized by transport coefficients like conductivity or viscosity) is linked to equilibrium fluctuations.
   - Onsager reciprocity further imposes symmetry conditions on these transport coefficients, ensuring that the cross-terms (\( L_{ij} \)) are symmetric.

4. **Practical Example**:
   - In heat conduction and diffusion, FDT relates the thermal conductivity or diffusion coefficient to the autocorrelation of energy or particle currents.
   - Onsager reciprocity ensures that, for coupled heat and particle transport, the coefficients linking heat to particle flow and particle flow to heat are equal.

---

### **Conclusion**:
The fluctuation-dissipation theorem provides a framework for understanding how systems respond to perturbations based on equilibrium fluctuations, while Onsager reciprocity ensures symmetry in the coupling between fluxes and forces in linear transport phenomena. Both are consequences of the time-reversal symmetry of microscopic dynamics and together provide a comprehensive description of nonequilibrium systems close to equilibrium.
