# AGENTS.md

## Course Context

- The mathematical background for the course is in `Lectures/`.
- Main references:
  - `Lectures/slides_VMAM_part1.pdf`: weak/variational formulations, Sobolev spaces, traces, Green formula, Lax-Milgram, mixed boundary conditions.
  - `Lectures/VMAM_slides_part2.pdf`: variational approximation, finite element spaces, P1 finite elements, mesh-based discretization.
  - `Lectures/convergence_L2.pdf` and `Lectures/poly_MVAM.pdf`: additional theory and polynomial/FEM context.
- For every lab or marked session, read the worksheet PDF first, then use the lecture notes to justify the mathematics.
- For coding style and FreeFem++ implementation patterns, refer to previous lab session scripts in the repository before introducing a new structure.

## FreeFem++ Coding And Testing

- Keep FreeFem++ scripts very small and direct.
- Prefer the standard FreeFem++ building blocks used in the worksheets:
  - `border`, `buildmesh`, `square`
  - `fespace`
  - `problem` or `solve`
  - `int2d`, `int1d`
  - `on(...)`
- Use the exact notations of the worksheet whenever possible: `u`, `uh`, `v`, `V`, `Vh`, `f`, `kappa`, `GammaD`, `GammaN`, `theta`, etc.
- Test FreeFem++ code with:
  - `FreeFem++ -nw "path/to/file.edp"` for a non-graphical compile/run check
  - `FreeFem++ "path/to/file.edp"` for the normal graphical run
  - optionally `FreeFem++ -ne "path/to/file.edp"` for a quick error check
- When debugging, first check:
  - wrong mesh name in `int2d(...)` / `int1d(...)`
  - missing semicolons
  - incorrect boundary labels in `on(...)`
  - confusion between `func`, scalar parameters, and FE functions
  - mismatch between the mathematical weak form and the code signs

## Coding Guidelines

- Minimal minimal minimal code.
- Readable.
- Follow the exact notations of the lab session worksheet.
- Do not add extra abstractions, helper layers, or features that are not needed by the question.
- Keep comments short, useful, and mathematical/FreeFem-specific.
- One script should answer the worksheet questions as directly as possible.

## Math Answering Guidelines

- Give detailed, fully rigorous answers.
- Specify the function space precisely.
- Derive everything slowly and with all needed details, as in a math book or exam solution.
- For variational formulations, always make the structure explicit:
  - state the strong problem clearly
  - define the test space precisely
  - multiply by a test function and integrate
  - apply Green's formula / integration by parts carefully
  - split and justify boundary terms one by one
  - define the bilinear form and linear form explicitly
  - state the final weak/variational formulation cleanly
- When relevant, justify well-posedness by checking continuity, coercivity, and the appropriate Poincare-type inequality before invoking Lax-Milgram.
- Never skip regularity assumptions silently: distinguish clearly between the formal derivation and the final weak formulation.
