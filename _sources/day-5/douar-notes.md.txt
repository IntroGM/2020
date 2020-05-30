# DOUAR notes

## Plasticity in DOUAR

DOUAR solves for Stokes flow, which means all materials need to have a viscosity. Thus, in order to have frictional plastic behavior of a material, the material needs a viscosity that makes it behave as if it was on plastic yield.

### Viscosity of a plastic element

Perfectly plastic materials undergo no deformation at stresses below their yield stress $\sigma_{\mathrm{y}}$, but and experience infinite deformation once the yield stress is reached. To simulate plastic behavior in a viscous fluid, the code must identify elements that are at the plastic yield stress and then find a viscosity that will ensure the stress in the element is equal to the yield stress.

In DOUAR, the elemental stress is calculated as

\begin{equation}
  \sigma = 2 \eta_{\mathrm{mat}} \dot{\varepsilon}
\end{equation}

where $\eta_{\mathrm{mat}}$ is the assigned material viscosity and $\dot{\varepsilon}$ is the strain rate in the element. In cases where the calculated elemental stress is below the yield stress, the assigned viscosity in the element will be used as the viscosity for that velocity iteration. In cases where the calculated stress exceeds exceeds the yield stress for the material, the viscosity must be recalculated such that the stress in the element is equal to the yield stress. This is done by solving for the effective viscosity $\eta_{\mathrm{eff}}$ that would satisfy this condition:

\begin{equation}
  \eta_{\mathrm{eff}} = \frac{\sigma_{\mathrm{y}}}{2 \dot{\varepsilon}}
\end{equation}

So, to summarize mathematically, we can say the viscosity $\eta$ used for an element with plasticity will be

\begin{cases}
  \eta = \eta_{\mathrm{eff}},& \text{if } \sigma \ge \sigma_{\mathrm{y}} \newline
  \eta = \eta_{\mathrm{mat}},& \text{otherwise}
\end{cases}