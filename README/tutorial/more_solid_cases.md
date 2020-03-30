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

<!-- insert pic -->


This case demonstrates the use of
“contact” boundary conditions and the
“cfMesh” Cartesian meshing software.

<!-- insert 3-4pic -->
<!-- insert 5-6pic -->

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

### Problem Definition

<!-- insert 34 -->

### Stress relaxation of a viscoelastic tube

<!-- insert 56 -->

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

<!-- insert 1 -->

See code at: solids4foam/src/solids4FoamModels/
materialModels/mechanicalModel/
mechanicalLaws/linearGeometryLaws/
linearElasticMisesPlastic/linearElasticMisesPlastic.C

### Problem definition

<!-- insert 2-3 -->

### Perforated Elastic Plate

<!-- insert 4 -->
<!-- insert 5 -->

### possible media???
