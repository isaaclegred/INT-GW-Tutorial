# INT-GW-Tutorial
A tutorial on GW data analysis for obtaining EoS constraints for the 2023 INT Nuclear matter workshop



**Useful links**
[GWPY](https://gwpy.github.io/) A library for working with gravitational wave strain data

[Bilby](https://git.ligo.org/lscsoft/bilby): a bayesian parameter estimation pipeline for gravitational wave data analysis.

[LWP](https://git.ligo.org/reed.essick/lwp): a module for hierarchicla inference of the nuclear EoS which depends individual event GW posterior samples.

[GWOSC API](https://gwosc.org/apidocs/): The GWOSC api, this gives a bunch of
examples for how to work with GW data without having to get your hands dirty.


---
*General Tutorial Outline*
Equation of State (EoS) analyses using astrophysical data based on Bayesian statistics are "Hierarachical"; informally, this means that the overall analysis consists of multiple levels forming a "hierarchy".  The first step in inferring EoS constraints from gravitational wave signals of merging neutron stars is to have a gravitational wave signal in realistic detector data.  

There are several ways to do this, one option is to inject a simulated signal into simulated detector data.  Bilby can do this!  There are multiple tutorials for how to do this but it will not be the focus of this tutorial.  

Instead we will use real detector data of a real gravitational wave-merger: GW170817.
An excellent source of information/data for Gravitational waves is the gravitational-wave open 
science center, or GWOSC.  In this case the [GWOSC page](https://gwosc.org/events/GW170817/)  has a lot of helpful links and in particular allow us to download real detector data from GW 170817.  We will want to use data which has had certain noise sources which were understood via correlations with other channels at the detectors subtracted.  Look for the download links labled "After noise subtraction".  Download the `.gwf` files (4096 Hz sampling rate is fine for our purposes) from each detector, these are gravitational-wave frame files.  They store the strain data from the event in chunks so that smaller segments can be more cheaply opened and used. Gravitational-wave strain segments for this reason are sometimes called "frames" within the LVK.

Usually gravitational-wave strain data should be accompanied by a Power Spectral Density (PSD), which characterizes the diagonal of the covariance of the noise in a detector (in the frequency domain).  When the noise is Gaussian and stationary (which it isn't always, it wasn't during 170817), the PSD completely characterizes the noise.  In this case we will cheat and use a PSD estimated from the gravitational-wave strain data itself (this is not a terrible approximation, but not what we use for production purposes.)

Follow the Notebook in order to see an example for how to run bilby.  In general there's lots of great examples along with documentation
for bilby the main limitation is that sampling is slow!  It can take multiple days to produce parameter estimation results for binary
neutron star mergers.  This is because they are in the LIGO band for a very long time, up to several minutes, much longer than BBHs. 


## The Hierarchical Step
Once we have parameter estimation which includes estimates of Neutron star masses and tidal deformabilities, we are 
prepared to convert these estimates to equation of state constraints.  There are many different software packages available to do this.  
The code I will demonstrate is one of the tools LIGO will be using in O4, based on [universality](https://github.com/reedessick/universality) developed by Reed Essick.  

The basic idea is to evaluate the likelihood of each EoS via marginalizing over a large number of potential NS sources.  This is the statistical framework which (from a Bayesian perspective) is the correct way to infer the EoS as a parameter of many different inference subproblems. See, for example,  Landry, Essick, and Chatziioannou Phys.Rev.D 101 (2020) 12, 123007.  The main point is that the ingredients we need are a set of equations of state, and binary neutron star parameter estimation samples.  

Install lwp with:

`git clone https://git.ligo.org/reed.essick/lwp.git`

Or if you have ssh keys registered with git:

`git clone git@git.ligo.org:reed.essick/lwp.git`

Then `lwp` can be installed with pip

`cd lwp`
`pip install lwp`





	
	
