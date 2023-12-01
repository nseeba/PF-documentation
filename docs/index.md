# Overview
<import assets/javascript/glightbox.min.js>
<import assets/stylesheets/glightbox.min.css>

<span style="color:red">**PRELIMINARY (COMMENTS WELCOME)**</span>

Welcome to the documentation of the CMS Particle Flow Algorithm. <br> 
The aim of this page is to provide a centralized overview of the CMS Particle Flow algorithm with the main focus being on:

* [Particle Flow Block algorithm](pfblock.md)
* [Core Particle Flow algorithm](corepf.md)


This page is based on <a href="https://arxiv.org/pdf/1706.04965.pdf" target="_blank" rel="noopener">[1]</a>.

## What is Particle Flow?

The Particle Flow (PF) algorithm is used to reconstruct and identify individually all particles produced in CMS by combining information from all subdetectors.<br>
These particles include:<br>

* [Muons](corepf.md#muons)
* [Electrons](corepf.md#electrons)
* [Photons](corepf.md#photons)
* [Charged hadrons](corepf.md#charged-hadrons)
* [Neutral hadrons](corepf.md#neutral-hadrons)

Link to the <a href="https://github.com/cms-sw/cmssw/tree/master/RecoParticleFlow/PFProducer/src" target="_blank" rel="noopener">Particle Flow Github repository</a>

## How do particles traverse through the CMS detector?

Starting from the beam interaction region, particles will enter:

* Inner tracker
* Electromagnetic calorimeter (ECAL)
* Hadronic calorimeter (HCAL)
* Muon chambers

The CMS detector has a large superconducting solenoid magnet as its central feature. The tracker system and both calorimeters are immersed in the magnetic field of the solenoid, which bends the trajectory of charged particles allowing to precisely measure their momentum and identify their charge. A transverse slice of the CMS detector can be seen in Figure 1, showing how different particles interact with the detector elements and travel through the detector. A detailed description of the CMS detector with all of its subdetectors can be found in <a href="https://iopscience.iop.org/article/10.1088/1748-0221/3/08/S08004/pdf" target="_blank" rel="noopener">[2]</a>. A short summary of the particle interactions in the detector elements is given below. 

<figure markdown>
  ![cmsslice](assets/cmsslice.png){align="center"}
  <figcaption>Figure 1. Schematic of the specific particle interactions in a transverse slice of the CMS detector <a href="https://iopscience.iop.org/article/10.1088/1748-0221/3/08/S08004/pdf" target="_blank" rel="noopener">[2]</a> </figcaption> 
 </figure>

1. After the collisions, particles first enter the inner tracking system, where charged particle trajectories (tracks) and origins (vertices) are reconstructed from hits in the sensitive layers of the CMS tracking system. The tracks are bent due to the magnetic field, which enables the measurement of charged particle momentum and electric charge.
2. After the tracker, particles enter the ECAL. Electrons and photons are fully absorbed in the ECAL - coming into contact with the ECAL material, they start showering and are detected as clusters of energy.
3. The same thing happens in the HCAL with charged and neutral hadrons, which may start showering in the ECAL as well but they get fully absorbed in the HCAL.
4. After the calorimeters, the remaining particles are muons, which have little or no interactions with the calorimeters, and neutrinos, which escape the detector undetected. Muons produce hits in the muon detectors.

## Code organization overview
<span style="color:red">**WORK IN PROGRESS**</span>

<figure markdown>
 ![pfalgo](assets/pfalgo.drawio.svg)
 <figcaption>Figure 2. Physics-based overview of the Particle Flow algorithm </figcaption> 
 </figure>

[PFElements](pfblock.md/#What-are-PF-elements) -(<a href="https://github.com/cms-sw/cmssw/blob/master/RecoParticleFlow/PFProducer/src/PFBlockAlgo.cc" target="_blank" rel="noopener">PFBlockAlgo</a>)> [PFBlock](pfblock.md/#overview-of-the-pfblock-algorithm) -(<a href="https://github.com/cms-sw/cmssw/blob/master/RecoParticleFlow/PFProducer/src/PFAlgo.cc" target="_blank" rel="noopener">PFAlgo</a>)> [PFCandidates](corepf.md/#identification-and-reconstruction-of-pf-candidates)

References
-----

- [1]  [The CMS Collaboration, 2017, "Particle-flow reconstruction and global event description with the CMS detector"][PF]
- [2]  [The CMS Collaboration, 2008, "The CMS experiment at the CERN LHC"][CMS]

[PF]: https://arxiv.org/pdf/1706.04965.pdf
[CMS]: https://iopscience.iop.org/article/10.1088/1748-0221/3/08/S08004/pdf
