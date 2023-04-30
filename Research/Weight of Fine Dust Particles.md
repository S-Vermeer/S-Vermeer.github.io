The API that we are using for our data is in the unit µg/m<sup>3</sup>. However, we need an amount, not a concentration so we know how many particles we need to display in our application. For this, we need to know how much fine dust particles weigh so we can determine how many particles there are relative to the concentration.

However, there are a few problems we are encountering because of this. We are going to be describing what the problems are, why it is a problem for us, and what assumptions we made to be as accurate as possible within the scope of our project.

## Problems
### Composition of Fine Dust
The first problem is with the composition of fine dust. The weight can vary a lot based on what type of fine dust it is. The elemental compositions are complex, they cna contain elements fromt the organic and inorganic phase. These elements all have different weights, in different amounts. Often enough, they are primarily composed of a single element. You still need to know how many atoms of that element are there, but you can approximate the weight of a particle based on its primary element and size. 

Even when you do know the weight of the particle, these particles, especially the smaller ones, are vulnerable for chemical reactions. These reactions can change the weight of these atoms. We know heat increases the chance of 

{ Varies a lot based on what type of fine dust it is }
{ Usually primarily one type of element, ex Si-based, Ca-based etc }
{ Depends on the weight of that element }
{ Chemical reactions, especially in smaller particles can change the weight of these atoms }
{ Elemental compositions are complex, contain elements from the organic and inorganic phase }

### Measuring
{ There are a variety of ways to measure it all, for example with vanderwaals forces or changes in frequency }
{ These particles aren't exactly a sphere so not even the radius can be said exactly }
{ It also depends on how 'nauwkeurig' the measurements are }


## Our Project
### Scope
xxx
{ Our scope is Eindhoven, but it also concerns multiple inner scopes that are being researched, namely airport, traffic and agriculture. }


### Zoom
{ The goal of this project is to depict a zoomed in version of the amount of fine dust particles in that area. The API gives us values in ug/m3, but seeing as we don't know what the particles weigh (approximately) we dont know at all how much particles we should visualize / how much we should zoom in. For example if 1ug/m3 is a million particles, than we should zoom in to a more manageble ratio such as cm3. }

### Sensors
{ The sensors know the amount, and do a calculation for the wait and calibrate it, with XXXXX as the formula, but we don't know which values are used in that formula so we cant deduce it from that }
{ Describe sensor setup exactly }


## Assumptions
xxxx
{density = 1,65 g/cm^3}
{Perfect Sphere}

