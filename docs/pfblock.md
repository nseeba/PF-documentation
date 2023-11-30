# Particle Flow Block algorithm
Add overall flowcharts of the PFBlock algo, both physical and code

## Overview of the PFBlock algorithm
PF blocks are collections of PF elements that are connected either through a direct link or an indirect link through common elements. 

In each PF block the identification and reconstruction sequence proceeds as so:

1. Muon candidates are identified and reconstructed and the corresponding PF elements are removed from the PF block. 
2. Electron identification and reconstruction with the aim of collecting the energy of all bremsstrahlung photons. Energetic and isolated photons, both converted and unconverted, are also identified here. Corresponding tracks and ECAL or preshower clusters are removed from further consideration.
3. The remaining elements in the PF block are then subject to a cross-identification of charged hadrons, neutral hadrons, and photons, arising from parton fragmentation, hadronization, and decays in jets.
4. Nuclear interactions coming from hadrons create secondary particles. These hadrons are identified and reconstructed.
5. When the global event description becomes available, meaning when all blocks have been processed and all particles identified, the reconstructed event is revisited by a post-processing step.

So in conclusion particles are dealt with in this order: muons, electrons, hadrons, photons, nuclear interactions. When one of them is processed, the corresponding tracks-clusters-links are removed from further consideration. 
<br>
One can think like this -  LINKS-BLOCKS-PARTICLES.

## Code
PLACEHOLDER
```c++ title="PFBlockAlgo.cc"
PFBlockAlgo::PFBlockAlgo()
    : debug_(false),
      elementTypes_({INIT_ENTRY(PFBlockElement::TRACK),
                     INIT_ENTRY(PFBlockElement::PS1),
                     INIT_ENTRY(PFBlockElement::PS2),
                     INIT_ENTRY(PFBlockElement::ECAL),
                     INIT_ENTRY(PFBlockElement::HCAL),
                     INIT_ENTRY(PFBlockElement::GSF),
                     INIT_ENTRY(PFBlockElement::BREM),
                     INIT_ENTRY(PFBlockElement::HFEM),
                     INIT_ENTRY(PFBlockElement::HFHAD),
                     INIT_ENTRY(PFBlockElement::SC),
                     INIT_ENTRY(PFBlockElement::HO),
                     INIT_ENTRY(PFBlockElement::HGCAL)}) {}
```