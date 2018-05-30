---
title: "GPS-Reflectometry: The Lake Huron Ice Thickness Estimation"
date: 2016-08-21
excerpt: "In this post, a new method to estimate lake ice thickness is presently using collocated GPS and tide gauge data sets."
mathjax: "true"
---
## 1. Introduction
The GNSS currently comprises operational systems including the Global Positing System (GPS), the Global Navigation Satellite System (GLONASS), Galileo, and Beidou. It was originally designed for time transfer, global positioning, and navigation anywhere on Earth and under all-weather situations, by using direct radar signals operating in L-band. Creatively, researchers found that the GNSS signals reflected from the sea and ground surface contain useful geophysical information. This remote sensing technique that takes advantage of the opportunistic GNSS reflected signals, known as GNSS Reflectometry (GNSS-R).

<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/1_1.jpg" alt="linearly separable data">

For the ground-based GNSS-R altimetry, Larson et al. (2013) first presented a method based on multipath theory to estimate local sea level variations from a single geodetic GPS receiver. In this post, a new method is present to estimate lake ice thickness and its changes in Lake Huron, using collocated GNSS-R and tide gauge data sets. First, the concept of lake ice thickness retrieval using a single geodetic GPS receiver is introduced. Then, GPS-R data analysis using data collected at the Harbor Beach GNSS site, on Lake Huron is utilized to generate an 11-year lake ice thickness product, 2004–2016, with a location.
  
## 2. Station Installation
The Harbor Beach GPS site is named as “HBCH”, managed by NOAA’s National Geodetic Survey (NGS), is located at the coastal areas of Lake Huron, Michigan. It was originally installed in April 2003 with a Trimble 5700 receiver, a dual-frequency geodetic-quality carrier phase GPS receiver. The GPS antenna was mounted on the top of a steel mast with an unobstructed view of the water. Thus, it is an ideal scenario to retrieve the multipath signals reflected from the water surface as lake level measurements. It should be note that this experiment of lake ice thickness estimate is demonstrated just for one point or one location offshore from Harbor Beach, Lake Huron.

<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/2_1.jpg" alt="linearly separable data">
 
## 3. Methodology
The satellite (laser and radar) altimetry, which provides global and consecutive observations, is a prevailing technique used to derive sea ice thickness especially on the Polar Regions at basin scales (Laxon et al, 2003; Giles et al., 2008; Farrell et al., 2009; Kurtz et al., 2012; Kwok et al, 2009; Laxon et al., 2013). However, the application of satellite altimetry to derive lake ice thickness is very challenging. In winter, the lake begins to freeze starting from the coastal area, then toward to center of the lake. With the large footprint size at several km’s, the accuracy of altimetry measurement decreases drastically at the littoral zone due to the contamination by the multiplicity of the coastal surface. Here, we describe a new approach that uses a single geodetic-quality GPS receiver with a collocated conventional tide gauge, to accurately determine the lake ice thickness at one location offshore of Harbor Beach, on Lake Huron, Michigan. 
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/3_1.png" alt="linearly separable data">

The figure shows the schematic of a GPS antenna affected by multipath signal reflected from the ice surface. The direct signals are interfered by the reflected signals due to the phase difference. Although the ice has different properties of conductivity and relative permittivity with sea water, this interference pattern is visible as oscillations in the SNR data. From the simulated results in Chapter 2, the reflector height between GPS antenna and ice surface can be accurately calculated by the dominant oscillation frequency. In other words, the ice surface height can also be obtained by the SNR technique. Therefore, the lake ice thickness T is as follow,

$$T=h_i-h_w$$ 

The figure shows daily surface heights from GPS observation and conventional tide gauge. The ice thickness is equal the difference between these two measurements.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/3_2.jpg" alt="linearly separable data">
  
## 4. Result and Validation
In order to validate the estimated ice thickness product at one location offshore on Lake Huron, the gridded ice coverage data was used for the identical period, which was provided by Great Lakes Environmental Research Laboratory (GLERL) of NOAA. The ice coverage data was obtained by a model, integrating all available data, including meteorological observations, oceanographic information, shipborne, and airborne measurements (Assel et al. 2003). From the following figure, both of these two data sets have annual freezing and water-clear variations. For the water-clear period, the ice thickness observation with cm-level amplitude is limited by the accuracy of this technique.

<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/4_1.jpg" alt="linearly separable data">

In order to further detect the reliability of ice thickness observation, the freeze-onset and water-clear dates for the whole 12-year time series were collected in figure 5.7. As the results indicate, the freeze-onset occurs in December except 2004 and 2007, and water-clear happens from March to April at Harbor Beach. The GPS observations are missing from Dec. 14th of 2011 to Mar. 9th of 2012, so the freeze-onset date was not collected for ice thickness to lessen the uncertainties. By comparison, the RMS of residual for the freeze-onset date is 0.81 day with a mean value of 0.53 day. For the water-clear date, the RMS difference is 0.86 day with a mean value of 0.69 days. Therefore, the ice thickness observed by the combination of GNSS-R and tide gauge is a reliable and sensitive indicator for the lake ice variation.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/4_2.jpg" alt="linearly separable data">

## 5. Inter-annual Variability and Long-term Trend
We now investigate the inter-annual variability and long-term trend of ice coverage and ice thickness, based on the 11-year GNSS-R Lake Huron ice thickness evolution, 2004–2016, at one location offshore from Harbor Beach. 
The observations outside of the ice season were removed, then an averaging window is applied to generate annual-mean time series of ice coverage and thickness. There was one large GPS data gap that occurred in ice season from Dec. 14th of 2011 to Mar. 9th of 2012. Since the ice thickness might reach the maximum before Mar. 9th, the annual-mean value was slightly underestimated for 2012. The spectral characteristics of the 12-year time series are further investigated. The following figure shows that the two close periods of three years and four years are found for ice coverage and thickness, respectively. Both of these two periods may be related to El Niño-Southern Oscillation (ENSO), which generally has strong inter-annual time scales of 3–5 years (Bai et al. 2010, 2012).

<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/5_2.jpg" alt="linearly separable data">

The linear trend was fitted using least squares regression by the linear equation in the form of $$y=ax+b$$. In the equation, the y is the ice coverage or ice thickness, x represents the time, a is the slope with time-varying, and b is a constant. Both the Lake Huron ice coverage and GNSS-R/tide gauge determined ice thickness (over one offshore location) show a positive trend, implying that the ice volume has been increasing since 2004.

<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_lakeice/5_1.jpg" alt="linearly separable data">

## 6. conclusion
In this study, a new method is introduced, to derive coastal lake ice thickness using a single geodetic-quality GPS receiver with a collocated tide gauge, demonstrated for one offshore location from Harbor Beach and on Lake Huron, Michigan. The derived ice thickness at a location offshore from Harbor Beach is validated with model ice coverage data provided by NOAA/GLERL. The results show that, as ice coverage product, the ice thickness is also a reliable indicator of lake ice variation and a critical input. When the ice coverage stops increasing, the ice thickness can still grow in the freezing period. Therefore, the GNSS-R based ice thickness data set could plausibly be used as additional information, and if they are adequately spatially sampled over or along the entire coastal regions of the Lake, to potentially improve our knowledge on the effect of lake ice thickness on severe weather forecasting over the Great Lakes region.

## reference
+ Assel, R., K. Cronk and D. Norton (2003). "Recent trends in Laurentian Great Lakes ice cover." Climatic Change 57(1): 185-204.
+ Larson, K. M., J. S. Löfgren and R. Haas (2013). "Coastal sea level measurements using a single geodetic GPS receiver." Advances in Space Research 51(8): 1301-1310.
+ Laxon, S., N. Peacock and D. Smith (2003). "High interannual variability of sea ice thickness in the Arctic region." Nature 425(6961): 947-950.
+ Laxon, S. W., K. A. Giles, A. L. Ridout, D. J. Wingham, R. Willatt, R. Cullen, R. Kwok, A. Schweiger, J. Zhang and C. Haas (2013). "CryoSat‐2 estimates of Arctic sea ice thickness and volume." Geophysical Research Letters 40(4): 732-737.
+ Pawlowicz, R., B. Beardsley and S. Lentz (2002). "Classical tidal harmonic analysis including error estimates in MATLAB using T_TIDE." Computers & Geosciences 28(8): 929-937.
+ Sun, J., (2017). Ground-based GNSS-reflectometry sea level and lake ice thickness measurements (Doctoral dissertation). Retrieved from OhioLINK's ETD Center. (OCLC # 1028024162)



