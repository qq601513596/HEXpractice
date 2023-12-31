File Structure
  py_project/HEXpractice (current):
    - DistributedHEX.py: main program of distributed HEX designing
    - UQSimulation.py: main program of uncertainty qualification, based on above
    - LumpHEX.py: main program of lumped HEX designing
    - LumpMonitor.py: main program of lumped HEX monitoring
    - Visualisation.ipynb: jupyter file for some simple plotting
    - HQisualisation.ipynb: jupyter file for uncertainty qualification plotting
    - utils:
      - HexClasses.py: HEX, fluid, and fouling layer classes
      - DataframeGenerator.py: generate dataframe to save
      - HEXSimulation: main progress of simulating HEX
      - utils: some other util functions
    - readme.md
    - SimpleExamples: samples for practice, not important
  py_data/HEXPractice (not commited): data generated by the program
    - disHEX: Distributed HEX
      - cinlet: constant inlets
      - rinlet: random inlets
    - lumpHEX: Lumped HEX
      - cinlet
      - rinlet
    - UQ: uncertainty qualification
      - norm: random sampling from normal distribution
        - mLTiM: Low uncertain m, middle uncertain Ti
        - mMTiM: Middle uncertain m, middle uncertain Ti
        - mHTiM
        - mMTiL
        - mMTiH
      - uniform: ... from uniform distribution
        ...
    - UQ: prediciton of uncertainty qualified data

DistrubutedHEX:
  Some key notes
    1 The convective coefficient in the shell side is calculated in the same way as tube side (by Nu), instead of the Bell-Delaware method.
    2 Some variables, e.g., heat capacity, conductivity, are supposed constant at this stage.
  Steps:
    1 Initialisation:
      -Initialise HEX, e.g., inner/outer radius, conductivity, etc.
      -Initialise fluids, e.g., physical properties, Reynolds, convective coefficients, etc.
      -Initialise fouling, deposit thickness = 0.
      Randomly generate inlet temperatures and mass rates.
    2 Run codes for the first day until we get the steady state.
    3 update parameters:
      -Update deposit layers.
      -Update HEX and fluid parameters dependent on fouling layer.
      Regenarate inlet temperatures and mass rates.
    4 Run recursively for further days.
  Description of objects:
    object: fluid 1 (cold)
      -mass rate: m = 0.3 kg/s or random
      -heat compacity: Cp
      -density: rho
      -Inlet temperature: Ti 573 K or random
      -conductivity: k
      -viscosity: mu = 4e-6 Pa*s
    object: fluid2 (hot)
      -mass rate: m = 1 kg/s or random
      -heat compacity: Cp
      -density: rho
      -Inlet temperature: Ti
      -conductivity: k
      -viscosity: mu
    object: hex
      -hex length: 6.1 m
      -tube inner radius: ri = 9.93e-3 m
      -tube outer radius: ro = 12.7e-3 m
      -shell outer radius: R = 0.6 m
      -material conductivity: 397 W/m*K
      -number of nodes: n = 100
      -initial temperature: 273 K
    object: deposit1/deposit2
      -sigma: m, fouling thickness
      -k_l0: material thermal conductivity of a freshly deposited material (lower limit)

Metadata of dataframe
  -Day: the HEX running day
  -F1m: kg/s, fluid 1 mass flow
  -F1i: K, flui11 inlet temperature
  -F1o: K, fluid 1 outlet temperature
  -F1v: m/s, fluid 1 AVERAGE velocity
  -F1D: m, fluid 1 AVERAGE hydraulic diameter
  -F1Re: fluid 1 AVERAGE Reynolds number
  -F1Nu: fluid 1 Nusselt number
  -F1h: W/m^2*K, fluid 1 AVERAGE convective coefficient
  -F1Cf: fluid 1 friction factor
  -F1Tau: Pa, fluid 1 shear stress
  -dP1: Pa, tube side OVERALL pressure drop
  -Sigma1: tube side AVERAGE fouling thickness
  -Rf1: m^2*K/W, tube side area fouling resistance
  -F2*: fluid 2 parameters, see above
  -UA: W/K OVERALL heat transfer coefficient time surface area (1/Rtotal).
  -Q: J/t, OVERALL heat duty