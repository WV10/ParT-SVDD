# ParT-SVDD



## Create conda environment
Before running the code, set up a conda environment. The code is written in `python 3.9`. It uses `pytorch 2.3.0` with CUDA and [weaver](https://github.com/hqucms/weaver-core/tree/main):
```
conda install pytorch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 pytorch-cuda=11.8 -c pytorch -c nvidia
pip install 'weaver-core>=0.4'
```

## Data
Import the JetMed_pthat_350 files from jetquenchingtools.web.cern.ch using
```
wget https://jetquenchingtools.web.cern.ch/JetQuenchingTools/samples/LundPlaneMC/JetMed-pthat_350-vac.res
wget https://jetquenchingtools.web.cern.ch/JetQuenchingTools/samples/LundPlaneMC/JetMed-pthat_350-qhat_1.5-L_4-asmed_0.24.res
```
From this data create a root-file with the input variables for both the vacuum and non-vacuum set using [JetToyHI](https://github.com/mverwe/JetToyHI/blob/master/README_ForBScStudents.md) from Marta Verweij. For this project we wrote `Data.cc` based on `runSimpleJetAnalysis.cc`. To create a root-file run:
```
./Data -hard samples/JetMed-pthat_350-vac.res -nev 100000
```

In the test_kin.yaml file, the datastructure is described.
- New variables: Define new variables using the variables of the root-file
- Inputs: Define which features are used for the training. The pf_features are used for the particle input, the pf_vectors are used for the interaction input, the mask is used to neglect the padded "particles". The pad_mode can be set at "constant" for padding with zeros and "wrap" for padding by repeating the particles in the jet.
- Labels: Define label names, they are not used in the training/testing
- Observers: Define which features are saved afterwards. 

## Run code
With any session, the configuration and results are saved in a `.json` file. Also the observables are saved in a root file for both the training and test data. Finally plots of the squared distance, cost and cost condition are created. All these outputs are saved in `Training/Test`.
To start a session run
```
cd DSVDD/src/
./ParT-SVDD.sh
```
To check all possible command-line arguments as used in `ParT-SVDD.sh`, run `python main.py -h`. 
[ParT](https://github.com/jet-universe/particle_transformer)
[Deep SVDD](https://github.com/lukasruff/Deep-SVDD-PyTorch/tree/master)
[SVDD](https://github.com/hqucms/weaver-core/tree/main)


