## Vehicle Longitudinal Dynamics Model Background

Before the current masking exercise, I had previously explored the mathematical background of a vehicle longitudinal velocity model.

The model was based on the expanded vehicle velocity equation:


$m\frac{dv}{dt}=F_t-(F_r+F_a+F_g)$

where:

* (F_t) = traction force
* (F_r) = rolling resistance
* (F_a) = aerodynamic drag
* (F_g) = road grade or hill force

The individual resisting force equations were:

$F_r = C_rmg$

$F_a = \frac{1}{2}\rho C_dAv^2$

$F_g = mg\sin(\theta)$

I implemented this model in Simulink earlier, with separate subsystems for:

* Rolling Resistance
* Aerodynamic Drag
* Road Grade Force

I also used an `InitFcn` callback to initialize model parameters such as:

* Traction force
* Vehicle mass
* Rolling resistance coefficient
* Gravity
* Air density
* Drag coefficient
* Frontal area
* Vehicle velocity
* Road slope angle

This earlier model served as the foundation for this masking and library-component refactoring activity.

---

## Masking Learning Journey

Before implementing masks, I realized I needed better workflow knowledge and conceptual understanding of subsystem masking. So I went to the MathWorks documentation, which is something I used to do in my previous work whenever I got stuck with MATLAB/Simulink implementation.

After browsing the documentation for around 15–20 minutes, I found a MathWorks video series on creating masks in Simulink.

The first video reinforced what I had already learned from the documentation. The second video was a eureka moment because it introduced different ways of exposing parameters, especially parameter promotion.

Important concept learned:

* Masking is not just for beautifying subsystems.
* Masking creates a public interface for a subsystem.
* The internal implementation can remain hidden, while important parameters can be exposed to the user.
* Parameter promotion makes it easier to expose internal block parameters through the mask dialog.

I used one-to-one parameter promotion today. Even though I have not yet used one-to-many promotion practically, discovering the workflow gave me a lot of confidence.

---

## Refactoring the Vehicle Velocity Model Using Masks

I revisited my earlier vehicle longitudinal dynamics model and refactored the resisting force subsystems into masked components.

Masked components created:

1. Rolling Resistance Force
2. Road Grade Force / Hill Force
3. Aerodynamic Drag Force

Each masked subsystem included:

* A short description of what the subsystem computes
* The mathematical expression used
* Explanation of variables
* Exposed mask parameters

For example:

### Rolling Resistance Force

Equation:

$F_r = C_rmg$

Exposed parameters:

* Rolling resistance coefficient
* Acceleration due to gravity

### Road Grade Force

Equation:

$F_g = mg\sin(\theta)$

Exposed parameters:

* Road slope angle
* Acceleration due to gravity

### Aerodynamic Drag Force

Equation:

$F_a = \frac{1}{2}\rho C_dAv^2$

Exposed parameters:

* Air density
* Drag coefficient
* Vehicle frontal area
* Vehicle velocity relative to air

This changed the model from a simple internal-subsystem implementation into a more reusable and component-oriented architecture.

---

## InitFcn Improvement

In the old model, many parameters were initialized globally in `InitFcn`.

After masking and parameter promotion, most component-specific parameters were moved into their respective masked subsystem interfaces.

The new `InitFcn` became much simpler and only contained top-level parameters such as:

```matlab
F_t = 3000; % N
m = 1200;  % kg
```

This was an important design improvement because it separated:

* Top-level scenario parameters
* Component-specific configuration parameters

This reduced global parameter clutter and made the model architecture cleaner.

---

## Simulink Library Creation

I also created a simple Simulink library containing the reusable force-component blocks:

* Rolling Resistance Force
* Road Grade Force
* Aerodynamic Drag Force

This helped me understand that masked subsystems can be used not just inside one model, but also as reusable components that can be stored in libraries and used across models.

This was an important step toward thinking like an MBD component developer rather than only a model builder.

---

## Refactored Model Validation

After replacing the original subsystems with masked/library-style components, I verified that the refactored model still produced the same final velocity result as before.

The final velocity remained approximately:

$159 \text{ km/h}$

This was an important observation because the model architecture changed, but the behavior remained consistent.

This introduced the idea of behavior-preserving refactoring, which is related to future validation concepts such as:

* Regression testing
* MIL testing
* Baseline comparison
* Model equivalence checking

---

## Key Learning Outcomes

The major learning outcomes so far are:

1. I understand subsystem types better, especially the difference between condition-based, event-driven, and scheduler-driven execution.

2. I learned that not every Simulink construct is suitable for every type of behavior.

3. I understood that masking is mainly about creating clean, reusable, configurable subsystem interfaces.

4. I discovered parameter promotion as a very useful workflow for exposing internal subsystem parameters.

5. I refactored an existing vehicle longitudinal dynamics model into reusable masked force components.

6. I reduced dependency on global `InitFcn` parameters by moving component-specific values into masked subsystem dialogs.

7. I created a simple reusable component library.

8. I preserved model behavior after refactoring, which is an early step toward validation thinking.

9. I shifted from “building a model” to “designing reusable model components.”

---

## Current Confidence Gain

## Confidence and Skill Development

This activity improved my confidence in using subsystem masks to create reusable, parameterized, and maintainable Simulink components.

Initially, subsystem masking felt unfamiliar and slightly intimidating. After studying the MathWorks documentation, watching the masking workflow videos, and applying parameter promotion in my own model, I gained a clearer understanding of how masks help convert ordinary subsystems into cleaner engineering components with well-defined public interfaces.

The key learning was that masking is not only a cosmetic feature. It is useful for abstraction, parameter exposure, documentation, reuse, and component-level model organization.


---

## Next Logical Steps

The next recommended steps are:

1. Document the masked vehicle velocity model properly.
2. Add simple test cases for flat road, uphill, downhill, and no traction force.
3. Create a small test harness.
4. Compare old and refactored model outputs more systematically.
5. Move toward Stateflow after completing this masking/refactoring learning arc.
6. Eventually extend the vehicle model into a cruise control project with:

   * Vehicle plant
   * PID controller
   * Stateflow supervisory logic
   * Requirements
   * MIL tests
   * Code generation
