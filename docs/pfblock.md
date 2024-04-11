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

###  <span style="color:#00bdd6">Charged particle tracks and vertices</span>

Charged-particle track reconstruction was originally aimed at measuring the momentum of energetic and isolated muons, identifying energetic and isolated hadronic $\tau$ decays, and at tagging b-jets, meaning it was mainly aimed at energetic particles and limited to only well-measured tracks. The tracks are reconstructed by a combinatorial track finder and are further required to satisfy certain conditions, such as a minimum number of required hits along a track’s trajectory. 

In the case of hadrons, complications arise, such as loss in tracking efficiency and increase in the misreconstruction rate. Charged hadrons that are missed by the tracker will be detected by the calorimeters (if detected at all) as neutral hadrons with reduced efficiency, largely degraded energy resolution, and biased direction due to the bending of its trajectory in the magnetic field. Two thirds of the jet energy in a jet is on average carried by charged hadrons, so a 20% tracking inefficiency would double the energy fraction of identified neutral hadrons in a jet from 10% to over 20%. This would degrade the jet energy and angular resolutions by about 50%. Therefore it is critical for PF event reconstruction to increase the track reconstruction efficiency while at the same time keep the misreconstruction rate unchanged. This is done by applying the combinatorial track finder in several successive iterations, labelled as iterative tracking. During each step, the reduction of the misreconstruction rate is done by applying a number of quality criteria on the track seeds, on the track fit $\chi^{2}$, and on the track compatibility. The hits associated with the tracks that are selected are removed from further processing in the next iteration. The hits that remain are then used in the next iteration to form new seeds and tracks with relaxed quality criteria, which then increases the total tracking efficiency without degrading the purity. This is then repeated several times with progressively more complex and time-consuming seeding, filtering, and tracking algorithms. To read more about iterative tracking and charged-particle tracks and vertices, read chapter 3 of the original <a href="https://arxiv.org/pdf/1706.04965.pdf" target="_blank" rel="noopener">PF paper</a>.

##  <span style="color:#00bdd6">Overview of the PFBlock algorithm</span>

###  <span style="color:#00bdd6">Link algorithm</span>
The Link algorithm is the next step in particle reconstruction following the creation of PF elements, and is the first step in the PFBlock algorithm. It aims to connect the PF elements from different subdetectors, since a single particle gives rise to many PF elements in the various CMS subdetectors. For example a muon leaves tracks in the tracker and hits in the muon detectors.

PF blocks consist of PF elements associated either by a direct link or indirect link through common elements. The link algorithm takes pairs of PF elements in an event and tests them. If two elements are linked, the algorithm will define a distance between them, quantifying the quality of the link. 

In conclusion, the Link algorithm connects together PF elements (tracks and clusters) based on their spatial proximity. Links can be formed between:

  * Tracks and ECAL clusters/HCAL clusters
  * ECAL clusters and HCAL clusters
  * Inner tracks and Muon tracks
  * Muon tracks and ECAL clusters/HCAL clusters

After all links are established between the compatible PF elements, PF blocks are formed from groups of linked PF elements. Blocks are also built from PF elements that were not linked to other PF elements, such as single tracks or single clusters. The overall logic of the PFBlock algorithm can be seen in the flowchart below.

 ![blocklog](assets/PFBlockAlgo_logic.drawio.svg){ width="800" height="600" style="display: block; margin: 0 auto" }
 
<span style="color:red">**TO DO / WORK IN PROGRESS**</span>
Add here a flowchart describing the code organization.