# Particle Flow Block algorithm
<span style="color:red">**PRELIMINARY (COMMENTS WELCOME)**</span>

<a href="https://github.com/cms-sw/cmssw/blob/master/RecoParticleFlow/PFProducer/src/PFBlockAlgo.cc" target="_blank" rel="noopener">PFBlockAlgo.cc</a>

## What are PF elements?
<span style="color:red">**TO DO**</span>

Elaborate on clusters and tracks

## Overview of the PFBlock algorithm

### Link algorithm
The Link algorithm is the first step in particle reconstruction. It aims to connect PF elements from different subdetectors, since a single particle gives rise to many PF elements in the various CMS subdetectors, for example a muon leaves tracks in the tracker and hits in the muon detectors. This is also the part where PF Blocks come into play. 
<br>
PF blocks consist of PF elements associated either by a direct link or indirect link through common elements. The link algorithm takes pairs of PF elements in an event and tests them. If two elements are linked, the algorithm will define a distance between them, quantifying the quality of the link. 

<span style="color:red">**TO DO / WORK IN PROGRESS**</span>

Add here a flowchart describing the physics logic of the PFBlockAlgo and another one for the code organization.

## Code
<span style="color:red">**TO DO**</span>

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