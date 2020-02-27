## **My First Solid Case**: hotSphere



### Case Overview



### Expected Results



### Theory

- We expect the vessel to deform due to the applied pressure and also due to the thermal gradient.

- The deformations (strains/rotations) are expected to be “small”: this means we can use a small strain (linear geometry) approach, where the displacements are assumed not to affect the material geometry.

- The conservation equations are linear momentum (linear geometry form) and energy (heat equation form):


<!-- Photos of equations-->

- Finally we will assume the deformation to be elastic (no permanent deformation) and the stress to be given by the Duhamel-Neumann form of Hooke’s law (i.e. Hooke’s law with thermal stress term):

<!-- Photos of equations-->

- Three mechanical properties must be specified: Elastic/Young’s modulus (E), Poisson’s ratio (ν) and the coefficient of linear thermal expansion (α). The Lamé parameters are then calculated as:

<!-- Photos of equations-->

### Theory: solution methodology

- This case employs a segregated solution methodology, where a loop is performed over the momentum equation (solved for displacement) and the energy equation (solved for temperature) until convergence is achieved. This loop is performed within each time-step resulting in an overall method that is implicit in time.

```
for all time-steps
    do
        solve energy equation for T (terms depending on D            
        are calculated explicitly)

        solve momentum equation for D (terms depending on 
        T are calculated explicitly)
    while not converged
end
```
### Running the Case

The solids4foam tutorials are organised into fluids, solids and fluid-solid interaction cases, where the solid tutorials are further categorised by physical phenomena:

```
tutorials
├── …
├── fluidSolidInteraction
│   └── …
├── fluids
│   └── …
└── solids
    ├── elastoplasticity
    ├── fracture
    ├── hyperelasticity
    ├── linearElasticity
    ├── multiMaterial
    ├── poroelasticity
    ├── thermoelasticity
    └── viscoelasticity
```

Change directory to the “run” directory (create the directory if needed):
```
$> mkdir -p $FOAM_RUN && run
```

Copy the solids4foam tutorials to the run directory; note: it is assumed here that solids4foam is installed at “$FOAM_RUN/..”, like in the Docker image:
```
$> cp -r ../solids4foam/tutorials .
```

Change directory into the hotSphere tutorial case:
```
$> cd tutorials/solids/thermoelasticity/hotSphere
```

Run the case using the Allrun script and look at the results in ParaView:
```
$> ./Allrun
$> paraFoam   # or “paraFoam -nativeReader”
$> # or “touch case.foam && paraview case.foam”
```
If you are using the solids4foam docker image, it is not possible to directly open ParaView from within the image; so a workaround is to copy the tutorials to the shared directory in the docker container “/home/app/foam/app-4.0/sharedRun”: this directory points directly to the $HOME directory on your physical computer:
```
$> run
$> cd ../sharedRun
$> cp -r ../solids4foam/tutorials .
```

You can now open a second terminal (on your physical computer, NOT in the docker container) and use ParaView installed on your physical computer to view the cases in your $HOME directory.

Example of using ParaView on your physical computer to view cases created in the docker container:

- Docker terminal
```
$> mkdir -p $FOAM_RUN && run
$> cd ../sharedRun
$> cp -r ../solids4foam/tutorials .
$> cd tutorials/solids/thermoelasticity/hotSphere
$> ./Allrun
```
- Followed by: Physical computer terminal
```
$> cd
$> cd tutorials/solids/thermoelasticity/hotSphere
$> touch case.foam && paraview case.foam
$> # or paraFoam
$> # or paraFoam -nativeReader
```

<!-- Pictures of paraview -->



### **hotSphere**: running the solver

Let us now examine the output from the solids4Foam solver; first clean the case and prepare the mesh:
$> ./Allclean && fluentMeshToFoam hotSphere.msh

Next run the solids4Foam solver:
$> solids4Foam


We will look at the solver output …
```
Time = 1

Evolving thermal solid solver
Solving coupled energy and displacements equation for T and D
    Corr, res (T & D), relRes (T & D), matRes, iters (T & D)
    100, 3.37856e-10, 9.48897e-06, 0, 3.1907e-05, 0, 0, 12
    200, 1.97288e-10, 2.07022e-06, 0, 7.17093e-06, 0, 0, 10
    300, 9.26738e-10, 4.86841e-07, 0, 1.6726e-06, 0, 0, 10
    The residuals have converged
    337, 1.64639e-10, 2.83573e-07, 0, 9.86025e-07, 0, 0, 12

Max T = 340
Min T = 301.118
Max magnitude of heat flux = 321505
Max epsilonEq = 0.000254994
Max sigmaEq (von Mises stress) = 5.56883e+07
ExecutionTime = 8.73 s  ClockTime = 9 s
```

In general, the solver checks 3 types of residuals for the “solid”:
- res (linear solver residual)
- relRes (relative residual: change of the primitive variable)
- matRes (material residual: for nonlinear material laws)

where the tolerance is specified in the solidProperties dictionary. In this case, there are residuals for T and D;  also, the material residual is zero because a linear mechanical law was selected (no need to iterate)

### **hotSphere**: running the solver in parallel

To run the solids4Foam solver in parallel for solid and fluid cases (we will examine fluid-solid interaction later), copy the decomposeParDict into the case:
```
$> cp $FOAM_UTILITIES/parallelProcessing/decomposePar/decomposeParDict system/
```

Edit the decomposeParDict (e.g. using emacs - in the docker container, you can install emacs or vim using e.g. “apt-get install vim”) to use the “simple” method with 4 cores:
```
numberOfSubdomains  4;
method              simple;
simpleCoeffs
{
    n           (2 2 1);
    delta       0.001;
}
```

Then decompose the case (the cellist option creates a field for visualisation in ParaView showing the decomposition):
```
$> decomposePar -cellDist
```

Run the solver in parallel:
```
$> mpirun -np 4 solids4Foam -parallel
```

View the results in ParaView in parallel, or reconstruct the results first and view them in serial:
```
$> reconstructPar
$> paraFoam # or “paraFoam -nativeReader”
$> # or “touch case.foam && paraview case.foam”
```
### **hotSphere**: parallelisation

<!--images-->

### Case Settings

#### Case numerical settings: solidProperties
#### Case numerical settings: fvSchemes
#### Case numerical settings: fvSolution


### Code

#### *hotSphere*: **examining the code**

#### Physics Model

#### *hotSphere*: **examining the code**

#### Physics Model: Fluid Models

#### Physics Model: Solis Models
#### Physics Model: Fluid-Solid Interaction Models

#### hotSpere: code

#### MechanicalLaw: code
