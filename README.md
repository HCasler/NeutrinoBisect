# NeutrinoBisect

NeutrinoBisect is a Python library for reconstructing the (anti-)neutrino momenta resulting from certain electron-positron interactions in particle colliders.

## Physics

This library allows the user to calcuate the final (anti-)neutrino momenta in the following interaction:

![e+e- -> tt~ -> W+W-b b~ -> l+l-nu nu~b b~](https://github.com/HCasler/NeutrinoBisect/blob/master/equation.png)

It assumes the user knows the center-of-mass energy, and the final momenta of the leptons and (anti-)bottom quarks.

## Requirements
* [numpy](http://www.numpy.org/)
* Tested on Python versions 2.6 and 2.7

## Usage
**Example**
```
import numpy as np
from neutrinoBisect import EventInput, NuSolver

inData = EventInput()
inData.totalEnergy = 1000.0
inData.muP = np.array([-55.41906268, -63.96540519,  36.03672536])
inData.antiMuP = np.array([ -26.72478036,  198.19401383, -114.33669775])
inData.bAntiBP = [np.array([ 102.97248438,  158.01606259,  -83.55812619]), np.array([ 31.84552299, -42.69183896,  47.34163184])]

solver = NuSolver(inData)
solutions = solver.findAllSolutions()

for solution in solutions:
    print solution
```
Should result in:
```
{'nu~': array([ -61.46857063, -277.34449352,  174.51469959]), 'nu': array([  8.7944063 ,  27.79166125, -59.99823285]), 'func': 2.4132579861498016}
{'nu~': array([ -70.39007973, -309.91346656,  112.60483213]), 'nu': array([ 17.7159154 ,  60.3606343 ,   1.91163461]), 'func': -9.9560978460913248}
```

**Explanation**

The **EventInput** class is used to hold the known information about an interation:
```
inData = EventInput()
```
**totalEnergy** contains the center-of-mass energy, in GeV
```
inData.totalEnergy = 1000.0
```
The lepton and anti-lepton final three-momenta (in GeV/c) are contained in **muP** and **antiMuP** as numpy arrays.
```
inData.muP = np.array([-55.41906268, -63.96540519,  36.03672536])
inData.antiMuP = np.array([ -26.72478036,  198.19401383, -114.33669775])
```
The bottom and anti-bottom three-momenta (in GeV/c) are contained in **bAntiBP**, which is a list of numpy arrays. They are placed together like this because the code assumes that the user does not know the charges of the (anti-)bottom jets.
```
inData.bAntiBP = [np.array([ 102.97248438,  158.01606259,  -83.55812619]), np.array([ 31.84552299, -42.69183896,  47.34163184])]
```

The **NuSolver** class performs the calculation. It should be initialized with an instance of EventInput containing the known info about the problem:
```
solver = NuSolver(inData)
```
The method to find all the possible final (anti-)neutrino momenta **findAllSolutions()**:
```
solutions = solver.findAllSolutions()
```
This will return a list of ```dict```s in the form of ```{'particle': threeMomentum}```, where the particle is either 'nu' (neutrino) or 'nu~' (antineutrino). The three momentum is a numpy array containing [px, py, pz].


## License
[MIT](https://choosealicense.com/licenses/mit/)
