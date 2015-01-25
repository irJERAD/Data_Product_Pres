A better Calculation of Blood Aclohol Content
========================================================
author: Jerad Acosta
date: January 24th, 2015
transition: rotate

Motivation
========================================================

Why do we need an product for calculating Blood Alcohol Content?
<small>What does this even mean?</small>
![What does this even mean?](http://www.lawfirmhost.net/spotora/images/dui_chart.jpg)

*Do you bring this to the bar with you?*
![Do you bring this to the bar?](http://www.tcptraining.com/Portals/1/EasyDNNnews/41/41StandardDrinksGuide.jpg)


We can do more
========================================================
With **robust inputs**, what was once a confusing guess can become an
improved *approximation* based on _research_.

## Design Goal
* Dynamic User Defined Input
* Simple
* Safe (error on the side of caution)
* Account for variables that can't be done with simple arithmatic
* Science and research based results

The estimation of Widmark’s factor
========================================================
A common calculation of blood alcohol content

$$
        \begin{aligned}
        BAC = \frac{Alcohol * \% * adj }
        {W * r}  - (\beta*t)
        \end{aligned}
$$
### Where:
+ Alcohol is the amount or number of alcohol beverages consumed
+ % is the percentage of alcohol content in beverage
+ adj is some adjustment factor
+ W is weight
+ r is **Widmark's factor**
+ $\beta$ is an alcohol metabolism constant
+ t is time since started drinking

*even at its most basic form these equations can be too much for those most in
need of its function*

Using modern research
====

$$
        \begin{aligned}
        BAC = \frac{G(1-e^{-k*t})}
        {W*\tau}  - (\beta*t)
        \end{aligned}
$$
### Where:
* G is alcohol consumed in grams <small>*more accurate from the outset</small>
* k is an absorption rate
  + based on how recently one ate
* W is weight in kg
* $\tau$ is Forrest's more recent approximation of the Widmark factor
* $\beta$ is an alcohol metabolism constant
* t is time since drinking beverage

Code
=====
With programing and a user interface we can get even more dynamic and accurate

```r
# aclohol mass consumed in grams, BMI, acohol metabolism rate and time
BAC <- function(volume_ml, gender, weight_kg, height_cm, alc_metabolismRate, time, absRate) {
        bodyMassIndex <- BMI(weight_kg, height_cm)
        # please see notes for reference to publication by Forrest (1986)
        fr <- ForrestRate(gender, bodyMassIndex)
        alcohol_mass <- AlcoholByMass(volume_ml)
        # calculating BAC according to Forrest (1986)
        bac <- (alcohol_mass) / (weight_kg * fr * 10) - (alc_metabolismRate * time * 0.8)
        ## TODO Check significance and units
        apxBAC <- round(bac, 4)
        if (apxBAC < 0) return(0)
        return(apxBAC)
}
```

Results
====
## What this means for users:
### Dynamic:
* We can calculate the effect of each beverage on BAC
    + then sum the results for a more accurate approximation
* We can adjust for individuals different absorption rates based on hunger
* We can also adjust alcohol metabolic rates

### Simple:
* No more dealing with a __Standard Drink__
    + <small><em>whatever that is</em></small>
* Accessability: We always have our smartphones
    + now we know when to use them to call a cab or a friend


Notes and References
====

1. [Using Calculations to Estimate Blood Alcohol Concentrations for Naturally Occurring Drinking Episodes: A Validity Study](http://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=0CCkQFjAB&url=http%3A%2F%2Fwww.researchgate.net%2Fpublication%2F7906019_Using_calculations_to_estimate_blood_alcohol_concentrations_for_naturally_occurring_drinking_episodes_a_validity_study%2Flinks%2F00463519b7747666ee000000.pdf&ei=J6vEVJTZL4e4ggTq0YDwBg&usg=AFQjCNFKmmC29jeEUuEQvD--qgodWRFk4A&sig2=SNrP_NVG4Rzhxf82KqlFaA&bvm=bv.84349003,d.eXY)

2. [Forensic Magazine: How to Extrapolate Alcohol with Certainty](http://www.forensicmag.com/articles/2011/08/how-extrapolate-alcohol-certainty)

3. [AVVO legal guide: Explaining Blood Alcohol Levels](http://www.avvo.com/legal-guides/ugc/explaining-blood-alcohol-levels-bac-what-does-the-08-bac-mean-how-many-drinks-is-that-1)

4. [NIH, Alcohol Metabolism: An Update](http://pubs.niaaa.nih.gov/publications/AA72/AA72.htm)

5. [Play with the App](https://irjerad.shinyapps.io/final/)
