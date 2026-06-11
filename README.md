# MBD Longitudinal Vehicle Model

## Project Objective

The objective of this project is to model the longitudinal velocity of a vehicle using Newton's Second Law and implement the equation in Simulink. The model considers traction force and major opposing forces such as rolling resistance, aerodynamic drag, and road grade force.

---

## Basic Equation
A basic equation for modeling vehicle velocity comes from applying Newton's Second Law of Motion to the longitudinal motion of the vehicle.
The general longitudinal vehicle dynamics equation is:

$\large {m \frac{dv} {dt} = F_T - F_R}$

Where,
  * $\large m$ = vehicle mass ($\large {kg}$) 
  * $\large v$ = vehicle velocity ($\large {\frac{m} {s}}$) 
  * $\large \frac{dv} {dt}$ = acceleration ($\large {\frac{m} {s^2}}$) 
  * $\large F_T$ = traction (or driving) force from wheels/engine ($\large N$)  
  * $\large F_R$ = total opposing forces ($\large N$)

---

## Expanded Vehicle Velocity Model
The opposing (or resistive) forces are usually modeled as:

$\large {F_R = F_r + F_a + F_g}$

Thus: 

$\large {m \frac{dv} {dt} =  F_T - (F_r + F_a + F_g)}$

Or:

$\large {\frac{dv} {dt} = \frac{1} {m} [F_T - (F_r + F_a + F_g)]}$

Where,
  * $\large F_T$ = traction force ($\large N$)
  * $\large F_r$ = Rolling Resistance ($\large N$)
  * $\large F_a$ = aerodynamic drag ($\large N$)
  * $\large F_g$ = grade force ($\large N$)

For this model, rolling resistance, aerodynamic drag, and positive road grade are treated as opposing forces to the vehicle motion.

---

## Individual Force Equations
1. Rolling Resistance
2. Aerodynamic Drag
3. Road Grade Force (or Hill Force)

### 1. Rolling Resistance:
Rolling resistance is the force that opposes the motion of a wheel rolling on a surface.

It is mainly caused by:
 * Tire deformation 
 * Road deformation 
 * Internal friction inside the tire material 
 * Small slip losses at the contact patch

Even though the wheel rolls instead of sliding, energy is still lost.

For a vehicle on ground, the rolling resistance($\large F_r$) is defined as follows,

$\large {F_r = C_r m g}$

Where,
 * $\large F_r$ = rolling resistance force ($\large N$)
 * $\large C_r$ = rolling resistance coefficient ($\large {constant}$) 
 * $\large m$ = vehicle mass ($\large {kg}$) 
 * $\large g$ = gravitational acceleration ($\large {\frac{m} {s^2}}$)

Typical:
* $\large C_r \approx$ 0.01 to 0.02

---

### 2. Aerodynamic Drag
Aerodynamic drag is the force exerted by air that opposes the motion of a moving vehicle.

As a vehicle moves through air, it must push air molecules aside. This creates:
 * Pressure differences around the body 
 * Turbulence and wake formation 
 * Friction between air and vehicle surface

All of these contribute to drag.

The standard drag force equation is:

$\large {F_a = \frac{1} {2} \rho C_d A v^2}$

Where,
  * $\large F_a$ = aerodynamic drag force ($\large N$) 
  * $\large \rho$ = air density ($\large {\frac{kg} {m^3}}$) 
  * $\large C_d$ = drag coefficient ($\large {constant}$)  
  * $\large A$ = frontal area ($\large m^2$) 
  * $\large v$ = vehicle velocity relative to air ($\large {\frac{m} {s}}$)
 
**Why EVs Focus Heavily on Aerodynamics:**
 * High drag reduces driving range 
 * Highway efficiency is dominated by drag 
 * EVs therefore aim for:
   * Low drag coefficient
   * Smooth underbody
   * Active grille shutters
   * Aerodynamic wheel designs
  
 ---
 
### 3. Road Grade Force (or Hill Force)
Road grade force (or hill force) is the component of gravity acting along the slope of the road.

When a vehicle moves uphill, gravity pulls it backward.

When moving downhill, gravity assists the motion.

This force is an important part of longitudinal vehicle dynamics.

The standard road grade force equation is:

$\large {F_g = m g \sin(\theta)}$

Where,
  * $\large F_g$ = grade force ($\large N$) 
  * $\large m$ = vehicle mass ($\large {kg}$) 
  * $\large g$ = gravitational acceleration (9.81 $\large {\frac{m} {s^2}}$) 
  * $\large \theta$ = road slope angle ($\large radians$)
 
**Effect on Vehicle Performance:**
 * Acceleration capability 
 * Required engine torque 
 * Fuel consumption 
 * EV battery usage 
 * Regenerative braking 
 * Cruise control

---

## Final Differential Equation

$\large {m \frac {dv} {dt} = F_T - (C_rmg + \frac {1} {2} \rho C_d A v^2 + m g \sin(\theta))}$

Or:

$\large {\frac {dv} {dt} = \frac{1} {m} [F_T - C_rmg - \frac {1} {2} \rho C_d A v^2 - m g \sin(\theta)]}$

---

## Simulink Implementation
### MODEL-1: Longitudinal Vehicle Model (Baseline Model)
In this project, I began by implementing a baseline model representing the longitudinal vehicle dynamics model. The baseline model was implemented by computing each resisting force separately, summing the opposing forces, subtracting them from the input traction force, dividing by vehicle mass to get the acceleration, integrating to obtain velocity, and converting the velocity result from m/s to km/h. The implemented Simulink model is shown below.

![Base Line Model - MDL1](https://i.postimg.cc/QCwQZ8Bm/Model1.png)

The implemented baseline model is available in the folder **'models/'** with the file name **'MDL1_Longitudinal_Vehicle_Model.slx'** in this repository.

## Model Parameters
The following are the model parameters that were used for simulation:
 * F_T = 3000 N (Input Traction Force)
 * m = 1200 kg (Vehicle Mass)
 * C_r = 0.015 (Rolling Coefficient)
 * g = 9.81 m/s² (Gravitational Acceleration)
 * rho = 1.225 kg/m³ (Air Density)
 * C_d = 0.32 (Drag Coefficient)
 * A = 2.2 m² (Vehicle Frontal Area)
 * theta = 0 rad (Road Slope Angle)
 * v = 20 m/s (Vehicle Velocity Relative to Air)

All the above listed simulation parameters were defined in MATLAB script, and the MATLAB script was used as **'initialization function'** within Simulink.

---

## Simulation Result
After performing model simulation, I observed the final velocity to be computed approximately around 159 km/h by the baseline model.

![Model_1_Final_Velocity](https://i.postimg.cc/BQTFgL2n/Model1-Final-Velocity.png)

![Model_1_Scope_Result](https://i.postimg.cc/WbSNTgrR/Model1-Simulation-Result.png)

---

## Refactored Masked Subsystem Version
### MODEL-2: Longitudinal Vehicle Model with Subsystem Masks for each Resistive Force
Though the baseline is able to successfully compute the vehicle velocity, it needed further modeling improvements. The first aspect I wanted to improve was the global declaration of simulation parameters in MATLAB script as **'initialization function'**.

In this model, each resisting force subsystem was refactored as a masked subsystem. The refactored model is shown below.

![Model2-MDL2](https://i.postimg.cc/6qgrxW7M/Model2.png)

The implemented refactored version model is available in the folder **'models/'** with the file name **'MDL2_Longitudinal_Vehicle_Model_Mask.slx'** in this repository.

This refactored model was able to help me simplify the simulation parameter MATLAB script. For better comparison, I am displaying the older and current MATLAB scripts below.

**Old Init Fcn MATLAB script:**

![Old MATLAB script](https://i.postimg.cc/52p3KV4w/Init-Fcn-old.png)

**New Init Fcn MATLAB script:**

![New m-script](https://i.postimg.cc/Hk32vgYM/Init-Fcn-new.png)

Both the MATLAB scripts are available in the **'scripts/'** folder in this repository.

All the remaining simulation parameters were promoted and exposed in the subsystem masks of each resistive force as shown below.

**Rolling Resistance Force block:**
![Rolling Resistance](https://i.postimg.cc/Bv6CdVbT/Rolling-Resistance.png)

![Rolling Resistance Subsystem Mask](https://i.postimg.cc/K8QNp2G9/Rolling-Resistance-Mask-Diag.png)

**Road Grade Force block:**
![Road Grade](https://i.postimg.cc/bwLTVhyL/Road-Grade-Force-Masked-Subsystem.png)

![Road Grade Subsystem Block](https://i.postimg.cc/SxVr1hSr/Road-Grade-Force-Mask-Diag.png)

**Aerodynamic Drag Force block:**
![Aerodynamic Drag Force](https://i.postimg.cc/3x9BfT8p/Aerodynamic-Drag-Masked-Subsystem.png)

![Aerodynamic Drag Subsystem Mask](https://i.postimg.cc/0yZfXskw/Aerodynamic-Drag-Mask-Diag.png)

This refactored subsystem was able to maintain consistent simulation results exactly matching with baseline model even after refactoring as shown below.

![Model_2_Final_Velocity](https://i.postimg.cc/5NBz3Y80/Model2-Final-velocity.png)

![Model 2 Scope Result](https://i.postimg.cc/7hf3RGLK/Model2-Simulation-Result.png)

---

### MODEL-3: Longitudinal Vehicle Model with Reusable Subsystem Masks for each Resistive Force from Custom Simulink Library
Although the masked subsystems helped simplify the InitFcn MATLAB script, expose simulation parameters through subsystem masks, and maintain consistent simulation results, the subsystems were still directly editable inside the model. To improve reusability and reduce accidental modifications, I moved the masked force components into a custom Simulink library and used locked linked library blocks in the main model.

![Opposing Forces Lib](https://i.postimg.cc/Dw5gpTvB/Opposing-Forces-Lib.png)

This library file is available in the **'libraries/'** folder in this repository.

To know more about lock links functionality in Simulink library, [Click Here](https://in.mathworks.com/help/releases/R2025b/simulink/ug/lock-links-to-library.html)

Once the locked library was created, I replaced the older editable masked subsystems with reusable, locked, and linked masked subsystem blocks from the library for each resisting force, as shown below.

![Model3-MDL3](https://i.postimg.cc/Zn28zbBz/Model3.png)

The implemented refactored version model is available in the folder **'models/'** with the file name **'MDL3_Longitudinal_Vehicle_Model_Lib_Mask.slx'** in this repository.

Since no computational modifications were made, the simulation results remained consistent with the previous model versions, as shown below.

![Model 3 Final_Velocity](https://i.postimg.cc/CLG8NBbK/Model3-Final-Velocit.png)

![Model 3 Scope Result](https://i.postimg.cc/02ZkPDm1/Model3-Simulation-Result.png)

---

## Learning Outcomes
By implementing this project, I gained practical exposure to the following important MBD aspects:
 * Translating physics into Simulink
 * Structuring equations into subsystems
 * Using InitFcn
 * Creating masked subsystems
 * Parameter promotion
 * Creating reusable library blocks
 * Refactoring models while preserving simulation behavior

---

## Future Improvements
As per standard MBD workflow, the following are the important next steps:
 * Add test harness
 * Add validation cases
 * Add Stateflow-based cruise control supervisor
 * Add PID cruise controller
 * Add MIL testing
 * Add requirements traceability
---
