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

Regarding the case of Amstelhaege, the placement and moving of the houses using the algorithms can be seen as a dynamic system over time. The houses have certain coordinates, but are moved over time in order to get a higher total price. The increase in total price hereby depicts the system getting to a higher energy level state.

This dynamic system can be represented in a state space model. This model consists out a set of input values, output values and state variables. Furthermore this dynamic system also has certain constant values.
  
#### The constant values in the dynamic system:
  - Water: The water in the models is a constant as it is placed in a predetermined area and does not change over time
  - Default price for houses: The default price does not get higher or lower in the system over time.
  - Number of houses: There is always a predetermined number of houses in the models.

#### Input values into the dynamic system ( u(t) ):
  - Moving of the houses: The moving of the houses portrays an input into the system which alters the overall state of the system, because the moving of a house changes so does the price of specific houses.

#### State variables ( x(t) ):
  - Coordinates of individual houses: The coordinates of the individual houses are the only state variables in the system of this case, because this is the minimum set of variables which enables to fully describe the system. Other values like individual housing price and total housing price can be determined from the housing coordinates. These values are therefore not first order variables. The coordinates can not be determined from pricing. Therefore coordinates are first order state variables.

#### Output ( y(t) ):
  - Total housing price: The output of the dynamic system which changes over time is the total housing price.

#### Initial conditions of the model for the variable part of the system:
Understanding the initial conditions will help to show why the movement of the houses are the state variables. This is because it should be possible to predict what state the system will be in over time if you know the constant values, the input values and the initial conditions. 
Example: the condition is a change in total price. This change can be determined from the difference in coordinates before and after moving.
    
#### Change in energy of the system over time
A dynamic system stores energy. In this particular system the only storage element for energy are the houses and store their indivual prices. The price is the form of energy in this system. The energy of every house is known from the coordinates of the house and the other houses.

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

Our theory for determining the upper bound is based on maximizing the increase in price for one house, while minimizing the increase for the other houses. Both the single family household and the bungalow receive a lower added value when given extra free surrounding compared to the maison:

  - Single family household = 285,000 * 0.03 = 8,550 added for every meter
  - Bungalow                = 399,000 * 0.04 = 15,960 added for every meter
  - Maison                  = 610,000 * 0.06 = 36,600 added for every meter

The upper bound could be determined by eliminating the increase in value for the lower earning houses. Furthermore the total price should be higher if only one of the highest earning houses is isolated. The increase in individual housing price will be terminated once another house is met. Isolating more than one house should therefore eliminate the increase in housing price prematurely.
By placing all other houses at the furthest distance possible from this single isolated house, the upper bound could therefore be determined:

##### Example upper bound: 1st neighbourhood with 20 houses

  - First the gridmap area is determined without the water:
    - Normal area    = 160x180
    - Water area     = 32x180
    - Grassland area = 132x180 -> the total area to place houses therefore is: 23,040
  - Secondly subtract the total area of all houses AND 
  calculate the mandatory free space for every house type:
    - Total area houses            = Single(8 * 8 * 12 = 768) + Bungalow(11 * 7 * 5 = 385) + Maison(12 * 10 * 3 = 360) = 1,513
    - Free space for one single    = 4 * 12 + 4 * 8 = 80 -> for 12 households = 12 * 80 = 960
    - Free space for one bungalow  = 4 * 10 + 4 * 11 = 84 -> for 5 households = 5 * 84 = 420
    - Free space for one maison    = 4 * 16 + 4 * 12 = 336 -> for 3 households = 3 * 336 = 1,008
    - Grassland area - total housing area - mandatory free space = 23,040 - 1,513 - 2,388 = 19,139 area left
  - Thirdly calculate with the area left what the isolated maison could earn maximally:
    - square root area left √(19,139) ~ 138
    - 138 * 36,600 = 5,050,800 euro's extra added to the maison's value
  
However there is still some area which could increase the total price of the neighbourhood. The pricing of the houses is calculated from the outline of the mandatory free space till the outline of another house's wall. Therefore if placed in a right way the houses with a lower mandatory free space can benefit from the houses with a higher mandatory free space. A single family household has 2 meter mandatory free surrounding, while a maison should have 6 meter. The difference in 4 meter can be accounted as extra free space for the single family household, which means that is two/third from the mandatory free space of the maison:

  - Beneficial extra area for bungalow = 1,008(maison) * 0.5 = 504 extra area:
    - √(504) ~ 22 -> 22 * 15,960 = 351,120 euro's extra added to the bungalow value
    - 351,120 * 5 = 1,755,600 euro's extra added for all bungalows
  - Beneficial extra area for single = 1,008(maison) * 0.333 + 420(bungalow) * 0.667 ~ 615 extra area
    - √(615) ~ 24 -> 24 * 8,550 = 205,200 euro's extra added to the single value
    - 205,200 * 12 = 2,462,400 euro's extra added for all single households
    
Taken from this hypothetical state of the model the upper bound could be calculated by adding the default values of the houses to the increase in value for all the houses:

  * 7,245,000 + 5,050,800 + 1,755,600 + 2,462,400 = 16,513,800 euro's as a total price

Auteurs (Authors)

    Stijn van den Berg
    Rob Burger
    Lars van der Water

Dankwoord (Acknowledgments)

    StackOverflow
    minor programmeren van de UvA

Repository van Rob, Lars &amp; Stijn
