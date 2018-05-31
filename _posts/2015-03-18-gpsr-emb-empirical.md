---
title: "GPS-Reflectometry: The Electromagnetic Bias Estimation"
date: 2015-03-16
excerpt: "The electromagnetic (EM) bias originates from the non-symmetric property of sea wave due to the sea wave crests are sharper than sea wave troughs. This post focuses on the estimation of EM bias for ground-based GPS-R altimetry."
mathjax: "true"
---
## 1. Introduction
  The electromagnetic (EM) bias originates from the non-symmetric property of sea wave that is significant in sea level measurement by the remote sensing technique. Because the sea wave crests are sharper than sea wave troughs, more electromagnetic signals are reflected from wave troughs than crests that result in the underestimation of sea level height (Park et al., 2016). This post focuses on the estimation of EM bias for ground-based GPS-R altimetry.
    
## 2. Dataset and Station Installation
On the purpose of performing an empirical modeling of the L-band GPS-R forward scattering electromagnetic bias, two GPS sites located in the Gulf of Mexico are selected as the case study as shown in the figure. These two GPS stations are collocated with a tide gauge and an anemometer, respectively, that measure sea level and wind speed. These two stations were also installed on the offshore platforms that have open views of the ocean without obstacles. Therefore, these two GPS sites provided the perfect configurations to conduct the EM Bias modeling experiment, and able to validate the results with the two GPS-R time series.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_empirical/2_1.png" alt="linearly separable data">

## 3. Methodology
For processing of the L1 GPS-R data using the SNR method, the broadcast ephemerides are used to calculate the satellite angles at the Calcasieu Pass site firstly. An elevation mask of 2.5º~40º is used because there is no significant SNR oscillation at higher elevation angles. The range of azimuth angles is from 40º to 320º that ensured the GPS receiver could capture the reflected signal from sea surface without obstacles. For the objective of investigating the EM bias modeling, the one-year time series of sea level height for 2015 is calculated. For the demonstration, a one-week subset is selected that showed the GPS-derived sea level time series is highly correlated with tide gauge sea level time series. Since the GPS receiver can receive radar signals from more than four satellites at the same time, there are about 1-3 sea level observations per hour based on the data processing described in the last section.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_empirical/3_1.png" alt="linearly separable data">

## 4. EMB Modeling
For the direct comparison and classification, the sampling rates of sea levels with wind speed observation are resampled to one hour. This procedure guaranteed that all of the observations are within the same time tags. The following figure shows that the wind speed observations in January of 2015, its maximum was less than 16 m/s. Since the number of observations larger than 10 m/s is inadequate, the range of wind speed values from 0 m/s to 10 m/s is thus chosen for the EM bias model computation. As the wave propagates towards the coastline, wind speed and wind direction can directly affect the wave shape. However, it is out of the scope of this study, and the use of wind direction information will be considered in future investigations.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_empirical/4_1.png" alt="linearly separable data">

When the tide gauge records are taken as “ground truth,” the GPS-derived sea level height residual can be calculated. The following figure shows that the sea level residual with specular point distributions, which are classified by different values of wind speeds. There are about 8,000 GPS-derived sea level observations for the whole year of 2015, the number of specular points varied from 400 to 1,200 for each particular value of wind speed. For clearer illustration, the median value of sea level residuals in a 3-hour window is used in the plotting. The bin of 1 m/s is used to divide the wind speeds into different categories, the sea level residual with the same time tags of specific wind speed values are assigned in the same category. The sea levels residual with wind speeds from 1 m/s to 8 m/s are shown. Overall, the absolute sea level residual increases with increasing wind speeds. For the central specular points, there are no variations with changing wind speeds. On the contrary, the largest rate of sea level residual occurs on the peripheral specular points. The coordinate of the specular point is a function of azimuth and elevation angle for a specific (constant) antenna height. When the elevation angle is larger, the specular points are closer to the antenna marked by the largest white point. As each subplot shows that the sea level residual is not impacted by azimuth but increases with decreasing elevation angles. The same approach has also been used to process the GNSS-R data at the Shell Beach site. Dual-Azimuth masks of 30º–120º and 210º–330º are used to avoid the jetted piles as shown in the following the figure. From the figure, the characteristics of sea level residual agree well with the predicted properties from the theoretical EM bias model. That is EM bias increases with the decreasing incidence angle and increasing wind speed.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_empirical/4_2.png" alt="linearly separable data">

In this study, all the sea level residual is assumed to be caused by EM bias. Thus, the least squares estimation can be employed to empirically model the EM bias using the sea level observations. When the empirical EM bias model is applied to the original GPS-R-derived sea level data from the Calcasieu Pass, the RMS differences are reduced from 7.39 cm to 4.77 cm.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_empirical/4_3.png" alt="linearly separable data">

## 5. Conclusion
In the bistatic forward-scattering GPS-R altimetry, the electromagnetic bias originates from the smaller reflectivity of wave crests than troughs that lead to underestimation of sea level heights. In this post, the GPS station with collocated tide gauge and anemometer located in the Gulf of Mexico is selected to conduct real data analysis. The sea level residuals are calculated between GPS-R-derived sea level heights and tide gauge measurement and divided into different values of wind speeds. Under the hypothesis that all signals of the sea level differences (between GNSS-R and tide gauge) are caused by EM bias, the least squares estimation is employed to generate the empirical EM bias models at the Calcasieu Pass. When the empirical model is applied to the GPS-R derived sea levels, the RMS difference decreased from 7.39 cm to 4.77 cm. We concluded that empirical EM bias model could improve the accuracy of the GPS-R derived sea level effectively. The remaining errors are possibly resulting from tropospheric refraction, other media corrections, or distorted SNR time series, and future study is warranted to further improve the GPS-R sea level accuracy.

## 6. Reference
+ Mobley, C. D. (2014). Modeling Sea Surfaces, Seattle, WA: Sequoia Sceintific Inc.
+ Nouguier, F., C. A. Guérin and B. Chapron (2009). "Choppy wave” model for nonlinear gravity waves." Journal of Geophysical Research: Oceans 114(C9).
+ Park, J., J. T. Johnson and S. T. Lowe (2016). "A study of the electromagnetic bias for GNSS-R ocean altimetry using the choppy wave model." Waves in Random and Complex Media 26(4): 599-612.
+ Park, K. D., P. Elósegui, J. Davis, P. Jarlemark, B. Corey, A. Niell, J. Normandeau, C. Meertens and V. Andreatta (2004). "Development of an antenna and multipath calibration system for Global Positioning System sites." Radio Science 39(5).
+ Pierson, W. J. and L. Moskowitz (1964). "A proposed spectral form for fully developed wind seas based on the similarity theory of SA Kitaigorodskii." Journal of geophysical research 69(24): 5181-5190.
+ Sun, J., (2017). Ground-based GNSS-reflectometry sea level and lake ice thickness measurements (Doctoral dissertation). Retrieved from OhioLINK's ETD Center. (OCLC # 1028024162)

    
