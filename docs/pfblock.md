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

Charged-particle track reconstruction was originally aimed at measuring the momentum of energetic and isolated muons, identifying energetic and isolated hadronic $\tau$ decays, and at tagging b-jets, meaning it was mainly aimed at energetic particles and limited to only well-measured tracks. These well-measured tracks are reconstructed in 3 stages by a combinatorial track finder based on Kalman filtering (KF):

  1. Initial seed generation with some compatible hits related to charged particles
  2. Trajectory building/pattern recognition - gather hits from all tracker layers along the charged-particle trajectory
  3. Final fitting in order to determine the properties of the charged-particle, such as origin, $p_{T}$, direction.

In order to keep these tracks for further analysis, the following requirements have to be satisfied:

  * The tracks have to be seeded with 2 hits in consecutive layers in the pixel detector
  * The tracks are reconstructed with at least 8 or more hits in total, with at most 1 missing hit along the way
  * The tracks have to originate from within a cylinder of a few mm radius centered around the beam axis
  * Track $p_{T}$ is larger than 0.9 GeV

For a charged particle to accumulate 8 hits along its trajectory, it must travel through the beam pipe, the pixel detector, the inner tracker, and the first layers of the outer tracker before the first significant nuclear interaction. In the case of hadrons, the probability to interact within the tracker material before reaching the 8 hits treshold ranges between 10 and 30%, making also the track be missed. The tracking efficiency is further reduced for hadrons with $p_{T}$ above 10 GeV, since these types of particles are found mostly in collimated jets, where the tracking efficiency is limited by the capacity to disentangle hits from overlapping particles.

Charged hadrons that are missed by the tracker will be detected by the calorimeters (if detected at all) as neutral hadrons with reduced efficiency, largely degraded energy resolution, and biased direction due to the bending of its trajectory in the magnetic field. Two thirds of the jet energy in a jet are on average carried by charged hadrons, so a 20% tracking inefficiency would double the energy fraction of identified neutral hadrons in a jet from 10% to over 20%. This would degrade the jet energy and angular resolutions by about 50%. Therefore it is critical for PF event reconstruction to increase the track reconstruction efficiency while at the same time keep the misreconstruction rate unchanged.

The tracking inefficiency is reduced by accepting tracks with smaller $p_{T}$ and with fewer hits. Tracks with smaller $p_{T}$ enable us to recover charged particles with small probability to deposit measurable energy in the calorimeters. Fewer hits allow us to catch particles interacting with the material of the tracker inner layers. The improvement sadly increases the combinatorical rate of misreconstructed tracks exponentially. The increase is by a factor 5 when the $p_{T}$ threshold is loosened to 300 MeV and increases by another order of magnitude when the total nr of hits is lowered to 5. If both criteria are loosened together the rate reaches 80%. The misreconstructed tracks, which are made out of randomly associated hits, have randomly distributed momenta and cause large excess energies in PF reconstruction. As a means to fix this problem, iterative tracking is used.

###  <span style="color:#00bdd6">Iterative tracking</span>
In order to increase the tracking efficiency and decrease the misreconstruction rate, the combinatorial track finder is applied in several successive iterations, each with moderate efficiency but with as high a purity as possible. During each step, the reduction of the misreconstruction rate is done by applying a number of quality criteria on the track seeds, on the track fit $\chi^{2}$, and on the track compatibility. In practice no quality criteria are applied to tracks with at least 8 hits since the misreconstruction rate is already small enough. The hits associated with the tracks that are selected are removed from further processing in the next iteration. The hits that remain are then used in the next iteration to form new seeds and tracks with relaxed quality criteria, which then increases the total tracking efficiency without degrading the purity. This is then repeated several times with progressively more complex and time-consuming seeding, filtering, and tracking algorithms. In total, there are 10 tracking iterations.

##  <span style="color:#00bdd6">Overview of the PFBlock algorithm</span>

###  <span style="color:#00bdd6">Link algorithm</span>
The Link algorithm is the next step in particle reconstruction following the creation of PF elements, and is the first step in the PFBlock algorithm. It aims to connect the PF elements from different subdetectors, since a single particle gives rise to many PF elements in the various CMS subdetectors. For example a muon leaves tracks in the tracker and hits in the muon detectors.

PF blocks consist of PF elements associated either by a direct link or indirect link through common elements. The link algorithm takes pairs of PF elements in an event and tests them. If two elements are linked, the algorithm will define a distance between them, quantifying the quality of the link. 

In conclusion, the Link algorithm connects together PF elements (tracks and clusters) based on their spatial proximity. Links can be formed between:

  * Tracks and ECAL clusters/HCAL clusters
  * ECAL clusters and HCAL clusters
  * Inner tracks and Muon tracks
  * Muon tracks and ECAL clusters/HCAL clusters

After all links are established between the compatible PF elements, PF blocks are formed from groups of linked PF elements. Blocks are also built from PF elements that were not linked to other PF elements, such as single tracks or single clusters.

<span style="color:red">**TO DO / WORK IN PROGRESS**</span>

Add here a flowchart describing the physics logic of the PFBlockAlgo and another one for the code organization.