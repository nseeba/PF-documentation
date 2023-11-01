# Core PF algo

## Another heading

### ParticleFlow <a href="https://github.com/cms-sw/cmssw/tree/master/RecoParticleFlow/PFProducer/src" target="_blank" rel="noopener">Github</a> repository

### Codeblocks

Some `code` goes here

### C++ codeblock
PFAlgo function:
```c++ title="PFAlgo.cc"
PFAlgo::PFAlgo(double nSigmaECAL,
               double nSigmaHCAL,
               double nSigmaHFEM,
               double nSigmaHFHAD,
               std::vector<double> resolHF_square,
               PFEnergyCalibration& calibration,
               PFEnergyCalibrationHF& thepfEnergyCalibrationHF,
               const edm::ParameterSet& pset)
    : pfCandidates_(new PFCandidateCollection),
      nSigmaECAL_(nSigmaECAL),
      nSigmaHCAL_(nSigmaHCAL),
      nSigmaHFEM_(nSigmaHFEM),
      nSigmaHFHAD_(nSigmaHFHAD),
      resolHF_square_(resolHF_square),
      calibration_(calibration),
      thepfEnergyCalibrationHF_(thepfEnergyCalibrationHF),
      connector_() {
  const edm::ParameterSet pfMuonAlgoParams = pset.getParameter<edm::ParameterSet>("PFMuonAlgoParameters");
  bool postMuonCleaning = pset.getParameter<bool>("postMuonCleaning");
  pfmu_ = std::make_unique<PFMuonAlgo>(pfMuonAlgoParams, postMuonCleaning);
```

```c++ hl_lines="2 3 4" linenums="39" title="PFAlgo.cc"
  // Muon parameters
  muonHCAL_ = pset.getParameter<std::vector<double>>("muon_HCAL");
  muonECAL_ = pset.getParameter<std::vector<double>>("muon_ECAL");
  muonHO_ = pset.getParameter<std::vector<double>>("muon_HO");
  assert(muonHCAL_.size() == 2 && muonECAL_.size() == 2 && muonHO_.size() == 2);
  nSigmaTRACK_ = pset.getParameter<double>("nsigma_TRACK");
  ptError_ = pset.getParameter<double>("pt_Error");
  factors45_ = pset.getParameter<std::vector<double>>("factors_45");
  assert(factors45_.size() == 2);
```
