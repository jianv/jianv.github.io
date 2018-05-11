---
title: "GPS-Reflectometry: The Electromagnetic Bias Simulation"
date: 2015-03-21
excerpt: "The electromagnetic (EM) bias originates from the non-symmetric property of sea wave due to the sea wave crests are sharper than sea wave troughs. This post focuses on the estimation of theoretical EM bias for ground-based GPS-R altimetry."
mathjax: "true"
---
## 1.Introduction
  The electromagnetic (EM) bias originates from the non-symmetric property of sea wave that is significant in sea level measurement by the remote sensing technique. Because the sea wave crests are sharper than sea wave troughs, more electromagnetic signals are reflected from wave troughs than crests that result in the underestimation of sea level height (Park et al., 2016).This post focuses on the estimation of theoretical EM bias for ground-based GPS-R altimetry.
    
## 2.Methodology
  The figures demonstrates the primary theoretical EM modeling procedures, in which the cool (green to blue) tones represent the linear processes, and the warm (pink to red) tones are nonlinear processes. In the first step, a set of linear sea surface is generated and go through a hydrodynamic transformation to obtain the corresponding nonlinear sea surface realizations. Then, the pulse returned from both linear and nonlinear sea surfaces are achieved by the bistatic geometry of GNSS-R altimetry. Finally, the theoretical EM bias model is estimated by comparing the averaged linear and nonlinear surface pulse returns.
<img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_simulation/2_1.png" alt="linearly separable data">
    
## 3.Sea Surface Modeling
  For the linear sea surfaces, the Pierson-Moskowitz spectrum was used as an initial model that was expected to have no differences in surface properties in the wave and trough portions of the surface, and therefore produce no EM bias (Pierson et al., 1964; Mobley et al., 2014). The essential process of sea surface realization can be described as follows,
  a. Choose the domain size to generate a time series at given point, and the number of points for the Fast Fourier Transforms (FFT);
  b. Choose the variance spectrum;
  c. Create random Hermitian Fourier amplitudes for both positive and negative frequencies;
  d. Extract the sea surface elevation by the inverse FFT.
  For the nonlinear sea surfaces, the “Choppy wave” model is used for approximating nonlinear hydrodynamic effects. The choppy wave method is based on a non-uniform horizontal displacement of the original linear surface. For one realization, the choppy wave transformation from the linear surface creates a nonlinear surface (Nouguier et al., 2009).
  The figure illustrates the linear and nonlinear realizations of an example profile. In this example, a 20 m s-1 wind speed was used with a 200-m surface profile. For two thousand data numbers, the two-point wavelength or Nyquist wavelength was 2 cm. Because the choppy wave transformation can modify the spectrum of the original linear surface, a spectrum “undressing” procedure was required to ensure that the linear and nonlinear sea surfaces retain the same spectral properties (Nouguier et al., 2009).
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_simulation/3_1.png" alt="linearly separable data">
    
## 4.Pulse Return
  The figure illustrates the geometry of the bistatic configuration in which a GNSS-R transmitter illuminates a sea surface facet. 
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_simulation/4_1.png" alt="linearly separable data">
  With the generated sea surface model, the pulse returns of the GPS receiver in the time domain are expressed by Park et al. (2016).
    
## 5.Results
  The EM bias results from the offset between the means of the true surface height and the specular surface height. The EM bias can be defined as the difference between the normalized first moments of the linear pulse return P_L (t) and the nonlinear pulse returns P_NL (t) (Naenna et al., 2010),
  <img src="{{ site.url }}{{ site.baseurl }}/images/gpsr_emb_simulation/5_1.png" alt="linearly separable data">
  With the ensemble, averaged pulse returns for the two cases were obtained as described in the previous section. Because the difference between these waveforms can be small, a large number of surface realizations are necessary. The computation utilized L1 band of 1.575 GHz with a bandwidth of 24 MHz. The Figure shows the simulated EM bias as a function of the incident angle and wind speed. Simulations with 100,000 linear and nonlinear surface realizations were performed for each geometry. The property of EM bias can be concluded that the absolute EM bias increases with the decreasing incidence angle and increasing wind speed.

## 5.Reference
* Mobley, C. D. (2014). Modeling Sea Surfaces, Seattle, WA: Sequoia Sceintific Inc.
* Nouguier, F., C. A. Guérin and B. Chapron (2009). "“Choppy wave” model for nonlinear gravity waves." Journal of Geophysical Research: Oceans 114(C9).
* Park, J., J. T. Johnson and S. T. Lowe (2016). "A study of the electromagnetic bias for GNSS-R ocean altimetry using the choppy wave model." Waves in Random and Complex Media 26(4): 599-612.
* Pierson, W. J. and L. Moskowitz (1964). "A proposed spectral form for fully developed wind seas based on the similarity theory of SA Kitaigorodskii." Journal of geophysical research 69(24): 5181-5190.
    
