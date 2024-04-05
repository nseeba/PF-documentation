# Particle Flow Block algorithm
<span style="color:red">**PRELIMINARY (COMMENTS WELCOME)**</span>

<a href="https://github.com/cms-sw/cmssw/blob/master/RecoParticleFlow/PFProducer/src/PFBlockAlgo.cc" target="_blank" rel="noopener">PFBlockAlgo.cc</a>

##  <span style="color:#00bdd6">What are PF elements?</span>

PF elements are the basic inputs for the PFBlock algorithm. They can be divided into:

  * Charged particle tracks from the inner tracker
  * ECAL clusters
  * HCAL clusters
  * Muon tracks

The reconstruction of PF elements is done by specifically set up advanced algorithms and is the first step in PF particle reconstruction. The algorithms include:

 * The reconstruction of charged particle tracks and vertices in the inner tracker using iterative tracking
 * Electron and muon track reconstruction
 * The reconstruction and calibration of calorimeter clusters

 A short description of the reconstruction of the PF elements from the last two bullet points can be found in the ["Identification and reconstruction of PF candidates"](corepf.md#identification-and-reconstruction-of-pf-candidates) section, where they are tied in with the reconstruction of the corresponding particles. Do keep in mind that electron tracks are still charged particle tracks that are built using the information from the inner tracker. 

<span style="color:red">Add here the charged particle tracks and vertices, iterative tracking text - **WORK IN PROGRESS**</span>

###  <span style="color:#00bdd6">Charged particle tracks and vertices</span>

Charged-particle track reconstruction was originally aimed at energetic particles (measuring the momentum of energetic and isolated muons, id energetic and isolated had tau decays, tag b-jets). Limited to well-measured tracks that are reconstructed in 3 stages:

  1. Initial seed generation with some compatible hits related to charged particles
  2. Trajectory building/pattern recognition, gather hits from all tracker layers along the charged-particle trajectory
  3. Final fitting in order to determine the properties of the charged-particle, such as origin, pT, direction.

Tracks kept for further analysis are:

  * Seeded with 2 hits in consecutive layers in the pixel detector
  * Required to be reconstructed with >=8 hits in total, at most 1 missing hit along the way
  * Required to originate from within a cylinder of a few mm radius centered around the beam axis
  * pT larger than 0.9 GeV

Charged hadrons that are missed by the tracker will be detected by the calorimeters as neutral hadrons with reduced efficiency, largely degraded energy resolution, and biased direction due to the bending of its trajectory in the magnetic field. ⅔ of the jet energy in a jet are on average carried by charged hadrons so a 20% tracking inefficiency would double the energy fraction of Id-d neutral hadrons in a jet from 10% to over 20%. This would degrade the jet energy and angular resolutions by about 50%. 

The tracking inefficiency can be reduced by accepting tracks with smaller pT and with fewer hits. Tracks with smaller pT allow us to recover charged particles with small probability to deposit measurable energy in the calorimeters. Fewer hits allow us to catch particles interacting with the material of the tracker inner layers - check out what the whole tracking system is made out of (pixel detector, silicone detector, inner tracking system, outer tracker).

The improvement sadly increases the combinatorical rate of misreconstructed tracks exponentially. The increase is by a factor 5 when the pT threshold is loosened to 300 MeV and increases by another order of magnitude when the total nr of hits is lowered to 5. If both criteria are loosened together the rate reaches 80%. The misreconstructed tracks, which are made out of randomly associated hits, have randomly distributed momenta and cause large excess energies in PF reconstruction.

###  <span style="color:#00bdd6">Iterative tracking</span>
In order to decrease the misreconstruction rate a number of quality criteria on the track seeds, on the track fit Chi squared, and on the track compatibility  is applied. In practice no quality criteria are applied to tracks with at least 8 hits since the misreconstruction rate is already small enough. 
There are 10 tracking iterations. Basically this iterative tracking is done in order to decrease the misreconstruction rate of the tracks. Nuclear interactions with the tracker material also might occur, which lead to either kinks in the original hadron trajectory or produce secondary particles, which on average 2/3 of are charged. The efficiency of the reconstruction of these particles is enhanced by the 6th and 7th iteration of the iterative tracking. 


##  <span style="color:#00bdd6">Overview of the PFBlock algorithm</span>

###  <span style="color:#00bdd6">Link algorithm</span>
The Link algorithm is the next step in particle reconstruction following the creation of PF elements. It aims to connect the PF elements from different subdetectors, since a single particle gives rise to many PF elements in the various CMS subdetectors. For example a muon leaves tracks in the tracker and hits in the muon detectors. This is also the part where PF blocks come into play. 
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