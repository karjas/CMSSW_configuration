# CMSSW_configuration

This git includes steps for creating and analysing SUSY physics with MadGraph, Pythia8 and CMSSW

The following abbreviations will be used

| Thing | Abbr |
|------- | :-------: |
| MadGraph5 | MG5 |
| Pythia8 | P8 |

## 1. Generate the correct masses and branching ratios with SuSPect

[SuSPect](https://www.coulomb.univ-montp2.fr/perso/jean-loic.kneur/Suspect/) is a program used to calculate supersymmetric particle spectrum from specified input parameters. The output is a text file in [SLHA-format](https://arxiv.org/abs/hep-ph/0311123)

## 2. Generate the desired event with MadGraph

### 2a. Generate the event

The script used is in file:XXXXX

### 2b. Construct correct parameter card

To get the desired event one needs to modify the parameter- and run cards accordingly. By default the parameter card included in the MSSM_SLHA2 model do not include correct branching ratios, decay widths nor masses for many of the cases we wish to study.

The inclusion can be made manually, or the card can be parsed automatically with this script.

### 2c. Copy the correct run and param -cards to the Cards -folder and run the Event Generator

Pretty self explanatory, include the desired run and event cards in the MadGraphFolder/process/Cards/ -folder

If the run completed without any errors, the desired output file can be found in MadGraphFolder/process/Events/Run_XX/ -folder as unweighted_events.lhe.gz 

If unzipped, the unweighted_events.lhe includes the events generated by MG in [Les Houches Event -format](https://arxiv.org/abs/hep-ph/0609017) (LHE) More compact presentation of the may be found [here](https://github.com/karjas/slepton_decay_analysis/blob/master/LHE_description.md)

## 3. Run the CMSSW

For this project, CMSSW_9_2_3 running on lxplus was used. The guide for setting up your own computing environment may be foudn [here](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookSetComputerNode). 

### 3a. Import the correct libraries

When the computing environment has been set up (cmsenv has been run in the src/ -folder succesfully) the following packages need to be forked from git.

```bash
git cms-addpkg GeneratorInterface/LHEInterface

git cms-addpkg Configuration/Generator
```
### 3b. Create the runnable config.py file with cmsDriver

cmsDriver.py is used with to create the config.py file with a correct fragment. The fragment contains necessary information for making the lhe file compatible with the CMSSW framework via pythia 8 hadronization. By default the config file will be read from 'src/Configuration/Generator/python/' -folder. For example, the path to testi.py -fragment is 'src/Configuration/Generator/python/testi.py'.

The following example command creates and runs a configuration file testi11.py which uses slr.lhe (which is located in the same directory as the command is being run from) as input and then performs GENeration and SIMulation steps on the data and saves the output into slrTesti.root -file.

```bash
cmsDriver.py testi.py -s GEN,SIM -n 100 --conditions auto:mc --filein file:slr.lhe --filetype=LHE --python_filename testi11.py --fast --pileup=NoPileUp --fileout file:slrTesti.root --datatier GEN-SIM --eventcontent RAWSIM
```

### 3c. Profit
























