# Particle Flow Block algorithm
<span style="color:red">**PRELIMINARY (COMMENTS WELCOME)**</span>

<a href="https://github.com/cms-sw/cmssw/blob/master/RecoParticleFlow/PFProducer/src/PFBlockAlgo.cc" target="_blank" rel="noopener">PFBlockAlgo.cc</a>

##  <span style="color:#00bdd6">What are PF elements?</span>

PF elements are the basic inputs for the PF algorithm. They can be divided into:

  * Tracks from the inner tracker
  * ECAL clusters
  * HCAL clusters
  * Muon tracks

Elaborate on clusters and tracks

Pf elements - what are they, how do we reconstruct them?

##  <span style="color:#00bdd6">Overview of the PFBlock algorithm</span>

###  <span style="color:#00bdd6">Link algorithm</span>
The Link algorithm is the first step in particle reconstruction following the creation of PF elements. It aims to connect the PF elements from different subdetectors, since a single particle gives rise to many PF elements in the various CMS subdetectors. For example a muon leaves tracks in the tracker and hits in the muon detectors. This is also the part where PF blocks come into play. 
<br>
PF blocks consist of PF elements associated either by a direct link or indirect link through common elements. The link algorithm takes pairs of PF elements in an event and tests them. If two elements are linked, the algorithm will define a distance between them, quantifying the quality of the link. 

In conclusion, the Link algorithm connects together PF elements (tracks and clusters) based on their spatial proximity. Links can be formed between:

  * Tracks and ECAL clusters/HCAL clusters
  * ECAL clusters and HCAL clusters
  * Inner tracks and Muon tracks
  * Muon tracks and ECAL clusters/HCAL clusters


<span style="color:red">**TO DO / WORK IN PROGRESS**</span>

Add here a flowchart describing the physics logic of the PFBlockAlgo and another one for the code organization.
Add here also the order of particles built from the blocks. muons, electrons and iso photons, hadrons and non iso photons.