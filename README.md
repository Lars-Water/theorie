## Utopia

Hier staat een korte beschrijving van het probleem evt. met plaatje.

De Amsterdamse gemeente staat voor een nieuwe uitdaging bij het bouwen van een nieuw woningcomplex. Omdat de grond hiervoor beschermd natuurgebied was zijn er namelijk bepaalde restricties gebonden aan de plaatsing van de huizen. Dit maakt het lastig de meest efficiënte en winstgevende plaatsing van de huizen in het vrijgemaakte gebied te bepalen en is dan ook het probleem wat opgelost moet worden. Voor de bouw van het woningcomplex zijn drie mogelijke aantallen aan huizen mogelijk: 20, 40 of 60 huizen. De oplossing van dit planologisch probleem moet daarbij in te zetten zijn voor al deze drie mogelijke woningscomplex varianten.

## Aan de slag (Getting Started)

### Vereisten (Prerequisites)

Deze codebase is volledig geschreven in Python3.6.3. In requirements.txt staan alle benodigde packages om de code succesvol te draaien. Deze zijn gemakkelijk te installeren via pip dmv. de volgende instructie:

pip install -r requirements.txt

### Structuur (Structure)

Alle Python scripts staan in de folder Code. In de map Data zitten alle input waardes en in de map resultaten worden alle resultaten opgeslagen door de code.

### Test (Testing)

Om de code te draaien met de standaardconfiguratie (bv. brute-force en voorbeeld.csv) gebruik de instructie:

python main.py

### State space

Regarding this case the dynamic system can be placed in a model which can predict the behaviour of this system. In order to predict what state the system will be in after x=time parameter info is needed about:
  
  - Model constants:
    - Water: The water in the models is a constant as it's placed in a predetermined area.
    - Default price for houses: The default price does not get higher or lower in the system over time.
    - Number of houses: There is always a predetermined number of houses in the models.
  - Input ( u(t) ):
    - Moving of the houses: The moving of the houses portrays an input into the system which alters the overall state of the system, because the moving of a house changes the price of specific houses.
  - Initial conditions of the model for the variable part of the system:
    - Is the price currently changing? This change in condition can be determined from the difference in coordinates before and after moving.
    
There are 9 versions of the above described dynamic system, regarding a difference in constant values. All these versions can be fully described with the parameters and conditions which were just described.
    
State variables ( x(t) ):
  - Coordinates of individual houses: The coordinates of the individual houses are the only state variables in the system of this case, because this is the minimum set of variables which enables to full describe the system. Other variables like individual housing price and total housing price can be determined from the housing coordinates and are therefore not first order variables. The coordinates can not be determined from pricing. Therefore coordinates are first order state variables.
  
Output ( y(t) ):
  - Total housing price: The output of the dynamic system which changes over time is the total housing price.
    
A dynamic system stores energy. In this particular system the only storage element for energy are the houses and store their indivual prices as energy. The energy of every house is known from the coordinates of the house and the other houses.

### Upper and Lower bound of the system

In order to determine the limits of accuracy for this problem the upper and lower bound has to be determined.

#### Lower bound

The lower bound of the problem for this case can be easily defined. On account if all houses do not have any extra free space they will have their default value prices. The lower bounds have different values between the model variants with a difference in number of houses as the sum of all housing prices accounts for the total housing price:

  - 20 houses:
    - (285,000 * 12 + 399,000 * 5 + 610,000 * 3) = 7,245,000
   - 40 houses:
    - (285,000 * 24 + 399,000 * 10 + 610,000 * 6) = 14,490,000
   - 60 houses:
    - (285,000 * 36 + 399,000 * 15 + 610,000 * 9) = 21,735,000
    
#### Upper bound

We have theorized the upper bound can realized if every house on the map is put together in order to maximize the extra area around a maison. For this 

![linear functions of extra housing price per house type](https://github.com/Lars-Water/theorie/blob/master/upper_bound.png)

Auteurs (Authors)

    Stijn van den Berg
    Rob Burger
    Lars van der Water

Dankwoord (Acknowledgments)

    StackOverflow
    minor programmeren van de UvA

Repository van Rob, Lars &amp; Stijn
