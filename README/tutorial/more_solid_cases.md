# Running More Solid Cases

## punch Case

Change directory into the punch case tutorial

```
$> cd $FOAM_RUN/tutorials/
solids/linearElasticity/
punch
```

Run the case using the Allrun script:
```
$> ./Allrun
```

![](images/punch1_2.PNG)


This case demonstrates the use of
“contact” boundary conditions and the
“cfMesh” Cartesian meshing software.

![](images/punch3_4.PNG)
![](images/punch5_6.PNG)

## viscoTube Case

Change directory into the viscoTube tutorial case:
```
$> cd $FOAM_RUN/tutorials/solids/viscoelasticity/
viscoTube
```
Again, you can run the case using the Allrun script; in this case,
blockMesh followed by the solver is all that is needed:
```
$> blockMesh && solids4Foam
```

This case demonstrates a visco-elastic solid
<!-- insert 12combo or 1 than 2 -->
![](images/tube1_2combooption.PNG)
![](images/tube1.PNG)
![](images/tube2.PNG)

### Problem Definition

![](images/tube3_4.PNG)

### Stress relaxation of a viscoelastic tube

![](images/tube5_6.PNG)

See code at: solids4foam/src/solids4FoamModels/materialModels/
mechanicalModel/mechanicalLaws/linearGeometryLaws/
viscousHookeanElastic/viscousHookeanElastic.C
```
 sigma = k_*tr(gradD)*symmTensor(I);
 sigma += gammaInf_*s_;
 // Calculate h: code not shown here for brevity
 forAll(h_, MaxwellModelI)
 {
 sigma += gamma_[MaxwellModelI]*h_[MaxwellModelI];
 }
 ```

## perforatedPlate Case

Change directory into the perforatedPlate tutorial case:
```
$> cd $FOAM_RUN/tutorials/solids/elastoplasticity/
perforatedPlate
```

You can run the case using the Allrun script, or as in the previous case, blockMesh
followed by the solver is all that is needed:

```
$> blockMesh && solids4Foam
```

This case demonstrates a Mises/J2 elasto-plastic solid:

![](images/plate1.PNG)

See code at: solids4foam/src/solids4FoamModels/
materialModels/mechanicalModel/
mechanicalLaws/linearGeometryLaws/
linearElasticMisesPlastic/linearElasticMisesPlastic.C

### Problem definition

![](images/plate2_3.PNG)

### Perforated Elastic Plate

![](images/plate4.PNG)
![](images/plate5.PNG)

### possible media???
![](images/tube1media.AVI)
![](images/tub2media.AVI)
