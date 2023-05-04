The API that we are using for our data is in the unit µg/m<sup>3</sup>. However, we need an amount, not a concentration so we know how many particles we need to display in our application. For this, we need to know how much fine dust particles weigh so we can determine how many particles there are relative to the concentration.

However, there are a few problems we are encountering because of this. We are going to be describing what the problems are, why it is a problem for us, and what assumptions we made to be as accurate as possible within the scope of our project.

## Problems
### Composition of Fine Dust
The first problem is with the composition of fine dust. The weight can vary a lot based on what type of fine dust it is. The elemental compositions are complex, they can contain elements fromt the organic and inorganic phase. (Verma et al., 2017) These elements all have different weights, in different amounts. Often enough, they are primarily composed of a single element. You still need to know how many atoms of that element are there, but you can approximate the weight of a particle based on its primary element and size. (Li & Shao, 2012)

Even when you do know the weight of the particle, these particles, especially the smaller ones, are vulnerable for chemical reactions (Li & Shao, 2012). These reactions can change the weight of these atoms. We know heat increases the chance of chemical reactions (J.P. Close, personal communication, March 7, 2023). This means that even if we know the composition of fine dust particles in an area, it can be vastly different based on the time of day, or season the measurements were taken. 

### Measuring
Another thing to consider is how it was measured. There are a variety of ways to measure the weight of fine dust particles, or the amount. You cannot just pick them up and put them on a measuring scale. During the course of research, multiple techniques have been discovered, so we are going to be researching a couple of them to see what we can learn from them. 

You could measure the weight using filter paper and analysing the fine dust particles. By using a Fine Particulate Sampler at a constant flow on preweighed filter s, you can calculate the particulate mass concentrations from the difference in weight of filters before and after sampling. This still only gives you a concentration, though. (Verma et al., 2017) After that you could perform tests on them to see what the particulate matters were composed of, for example how much of them are water soluble and continuing from there.

Another option is computer-controled scanning electron microscopy coupled with energy dispersive x-ray spectroscopy. This method can provide information on the size, morphology, and elemental composition of individual particles. Using this, you can more accurately determine the weight of the particles in an environment, passively. (Byeon et al., 2015)

A different approach would be through the use of the change in resonant properties. This technique divides its studies into two categories. Chemically or biologically bonded to the surface and physically bonded to the surface of the sensor. With the chemical or biological method, a coating will attract and bond the particles to the surface of the sensor, while physically relies on different forces such as gravity and van der Waals. These physical attractions detail how they calculate their adhesion through various assumptions. To counteract the accumulation of the dust and its impact, the sensors might use vibration-based removal methods or electrostatic forces to clear the previously measured dust and make room for more measurements. (Kim et al., 2018)

The problem is, that this does not help us in our project, and some of them don't even give us a weight. The Fine Particulate Sampler gives us a concentration by using the weight and surface. The combination of micoscopy and spectroscopy comes close, but is only applicable for the location that it is in, and we do not have access to one. The research on resonant properties is not useful to us by how it is measured, but it gives us rational assumptions to base our widespread calculations on.

## Our Project
### Scope
The scope of our project is Eindhoven. Within Eindhoven there are multiple sectors being researched, namely the airport, traffic and agricultural sector. This means that we know that there are different compositions of particles within our scope. We either need to find research that covers this scope, or make rational assumptions so we can simulate the fine dust to the user. This can always be adjusted if the project is continued if their scope allows it.

### Zoom
The goal of this project is to depict a zoomed in version of the amount of fine dust particles in the area. We get a concentration from the API, however we do not know how much we need to zoom in to get a good ratio of fine dust being visualized and the scale being comprehensible. So we need to figure out how many particles approximately go in our current concentration, so we can scale it to the most manageable concentration. If there are a million particles in a cubic meter, then that is impossible for us to visualize. The same concentration in a cubic centimeter would be much more manageable. 

### Sensors
{ The sensors know the amount, and do a calculation for the wait and calibrate it, with XXXXX as the formula, but we don't know which values are used in that formula so we cant deduce it from that }
{ Describe sensor setup exactly }


## Assumptions
xxxx
{density = 1,65 g/cm^3}
{Perfect Sphere}



---
Chantara, Somporn. ‘PM10 and Its Chemical Composition: A Case Study in Chiang Mai, Thailand’. In _Air Quality - Monitoring and Modeling_, edited by Sunil Kumar. InTech, 2012. [https://doi.org/10.5772/33086](https://doi.org/10.5772/33086).

Byeon, Sang-Hoon, Robert Willis, and Thomas Peters. ‘Chemical Characterization of Outdoor and Subway Fine (PM2.5–1.0) and Coarse (PM10–2.5) Particulate Matter in Seoul (Korea) by Computer-Controlled Scanning Electron Microscopy (CCSEM)’. _International Journal of Environmental Research and Public Health_ 12, no. 2 (13 February 2015): 2090–2104. [https://doi.org/10.3390/ijerph120202090](https://doi.org/10.3390/ijerph120202090).{ijerph}

Verma, N., Satsangi, A., Lakhani, A., & Kumari, K. M. (2017). Low Molecular Weight Monocarboxylic Acids in PM2.5 and PM10: Quantification, Seasonal Variation and Source Apportionment. _Aerosol and Air Quality Research_, _17_(2), 485–498. [https://doi.org/10.4209/aaqr.2016.05.0183](https://doi.org/10.4209/aaqr.2016.05.0183) {aaqr}

Li, W., & Shao, L. (2012). Chemical Modification of Dust Particles during Different Dust Storm Episodes. _Aerosol and Air Quality Research_, _12_(6), 1095–1104. [https://doi.org/10.4209/aaqr.2011.11.0188](https://doi.org/10.4209/aaqr.2011.11.0188) {aaqr}

Kim, Min-Geon, Ji-Seob Choi, and Woo-Tae Park. ‘MEMS PZT Oscillating Platform for Fine Dust Particle Removal at Resonance’. _International Journal of Precision Engineering and Manufacturing_ 19, no. 12 (December 2018): 1851–59. [https://doi.org/10.1007/s12541-018-0214-9](https://doi.org/10.1007/s12541-018-0214-9). {S12541}

