---
title: "GPS-Reflectometry: The Lake Huron Ice Thickness Estimation"
date: 2017-08-21
excerpt: "In this post, We present a new method to estimate lake ice thickness and its changes in Lake Huron, using collocated GPS-R and tide gauge data sets."
mathjax: "true"

---
## 1.Introduction
  The GNSS currently comprises of operational systems including the Global Positing System (GPS), the Global Navigation Satellite System (GLONASS), Galileo, and Beidou. It was originally designed for time transfer, global positioning and navigation anywhere on Earth and under all-weather situations, by using direct radar signals operating in L-band. Creatively, researchers found that the GNSS signals reflected from the sea and ground surface contain useful geophysical information. This remote sensing technique that takes advantage of the opportunistic GNSS reflected signals, known as GNSS Reflectometry (GNSS-R).
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/1_1.jpg" alt="linearly separable data">
  For the ground-based GNSS-R altimetry, Larson et al. (2013) first presented a method based on multipath theory to estimate local sea level variations from a single geodetic GPS receiver. In this post, a new method is present to estimate lake ice thickness and its changes in Lake Huron, using collocated GNSS-R and tide gauge data sets. First, the concept of lake ice thickness retrieval using a single geodetic GPS receiver is introduced. Then, GNSS-R data analysis using data collected at the Harbor Beach GNSS site, on Lake Huron is utilized to generate a 11-year lake ice thickness product, 2004–2016, with a location.
  
## 2.Station Installation
  The Harbor Beach GPS site is named as “HBCH”, managed by NOAA’s National Geodetic Survey (NGS), is located at the coastal areas of Lake Huron, Michigan. It was originally installed in April of 2003 with a Trimble 5700 receiver, a dual-frequency geodetic-quality carrier phase GPS receiver. The GPS antenna was mounted on the top of a steel mast with unobstructed view of water. Thus, it is an ideal scenario to retrieve the multipath signals reflected from the water surface as lake level measurements. It should be note that this experiment of lake ice thickness estimate is demonstrated just for one point or one location offshore from Harbor Beach, Lake Huron.
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/2_1.jpg" alt="linearly separable data">
  
## 3.Methodology
  The satellite (laser and radar) altimetry, which provides global and consecutive observations, is a prevailing technique used to derive sea ice thickness especially on the Polar Regions at basin scales (Laxon et al, 2003; Giles et al., 2008; Farrell et al., 2009; Kurtz et al., 2012; Kwok et al, 2009; Laxon et al., 2013). However, the application of satellite altimetry to derive lake ice thickness is very challenging. In winter, the lake begins to freeze starting from coastal area, then toward to center of the lake. With the large footprint size at several km’s, the accuracy of altimetry measurement decreases drastically at the littoral zone due to the contamination by the multiplicity of coastal surface. Here, we describe a new approach that uses a single geodetic-quality GPS receiver with a collocated conventional tide gauge, to accurately determine the lake ice thickness at one location offshore of Harbor Beach, on Lake Huron, Michigan. 
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/3_1.png" alt="linearly separable data">
  The figure shows the schematic of a GPS antenna affected by multipath signal reflected from the ice surface. The direct signals are interfered by the reflected signals due the phase difference. Although the ice has different properties of conductivity and relative permittivity with sea water, this interference pattern is visible as oscillations in the SNR data. From the simulated results in Chapter 2, the reflector height between GPS antenna and ice surface, can be accurately calculated by the dominant oscillation frequency. In other words, the ice surface height can also be obtained by the SNR technique. Therefore, the lake ice thickness T is as follow,
  $$T=h_ice-h_water$$ 
  The figure shows daily surface heights from GPS observation and conventional tide gauge. The ice thickness is equal the difference between these two measurements.
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/3_2.png" alt="linearly separable data">
  
## 4.Results
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/4_1.png" alt="linearly separable data">
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/4_2.png" alt="linearly separable data">
  
## 5.Results and Discussion
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/5_2.png" alt="linearly separable data">
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr-lakeice/5_1.png" alt="linearly separable data">
  
## 6.Conclusion
  In this study, a new method is introduced, to derive coastal lake ice thickness using a single geodetic-quality GPS receiver with a collocated tide gauge, demonstrated for one offshore location from Harbor Beach and on Lake Huron, Michigan. The derived ice thickness at a location offshore from Harbor Beach is validated with model ice coverage data provided by NOAA/GLERL. The results show that, as ice coverage product, the ice thickness is also a reliable indicator of lake ice variation, and a critical input. When the ice coverage stops increasing, the ice thickness can still grow in freezing period. Therefore, the GNSS-R based ice thickness data set could plausibly be used as additional information, and if they are adequately spatially sampled over or along the entire coastal regions of the Lake, to potentially improve our knowledge on the effect of lake ice thickness on severe weather forecasting over the Great Lakes region.

