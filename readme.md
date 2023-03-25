# Data and Code for ADSNet

## Folder Specification

- ```data/```
  - ```ICDMappings-main``` an ICD-10 to ICD-9 conversion tool
  - ```input```
    - ```drug``` drug information for mimic-III and mimic-IV
    - ```mimic-iii```
      - **PRESCRIPTIONS.csv**: the prescription file from MIMIC-III raw dataset
      - **DIAGNOSES_ICD.csv**: the diagnosis file from MIMIC-III raw dataset
      - **PROCEDURES_ICD.csv**: the procedure file from MIMIC-III raw dataset
    - ```mimic-iv```
      - **prescriptions.csv**: the prescription file from MIMIC-IV raw dataset
      - **diagnoses_icd.csv**: the diagnosis file from MIMIC-IV raw dataset
      - **procedures_icd.csv**: the procedure file from MIMIC-IV raw dataset
      - **procedures_icd_9.csv**:  generated using ICDMappings
      - **diagnoses_icd_9.csv**: generated using ICDMappings
  - ```output```
    - ```mimic-iii```
      - **data_final.pkl**: intermediate result
      - **ddi_A_final.pkl**: ddi adjacency matrix
      - **ehr_adj_final.pkl**: used in GAMENet baseline (if two drugs appear in one set, then they are connected)
      - **records_final.pkl**: The final diagnosis-procedure-medication EHR records of each patient, used for train/val/test split.
      - **voc_final.pkl**: diag/prod/med index to code dictionary
      - **idx2SMILES**: ide2SMILES mapping
    - ```mimic-iv```
      - **data_final.pkl**: intermediate result
      - **ddi_A_final.pkl**: ddi adjacency matrix
      - **ehr_adj_final.pkl**: used in GAMENet baseline (if two drugs appear in one set, then they are connected)
      - **records_final.pkl**: The final diagnosis-procedure-medication EHR records of each patient, used for train/val/test split.
      - **voc_final.pkl**: diag/prod/med index to code dictionary
      - **idx2SMILES**: ide2SMILES mapping
- src/
  - ADSNet.py
  - layers.py
  - models.py
  - run_mimic4.py
  - util.py
- saved/ 
  - 0.0015_64_0.1_0.97_0_mimic3: The model we trained for mimic-iii
  - 0.0002_64_0.1_0.97_3_mimic4: The model we trained for mimic-iv

## dependency

python 3.9.15, scipy, pandas, torch 1.12.1, numpy, dill, rdkit

## argument

```
usage: ADSNet.py [--Test] [--resume] [--EPOCH]
               [--lr] [--emb_dim]
               [--drop_rate] [--bce] [--cuda] [--type]
```

## run the code

`cd ADSNet-main` Ensure that the command runs in the current folder

1. MIMIC-IV ICD10 to ICD9

   `python data/ICDMappings-main/mimic-iv-ICD.py`

2. Processing MIMIC-III

   `python data/mimic-iii.py`

3. Processing MIMIC-IV

   `python data/mimic-iv.py`

4. run on MIMIC-III

   `python src/ADSNet.py --type mimic3 --lr 0.0015--EPOCH 5`

5. run on MIMIC-IV

   `python src/ADSNet.py --type mimic4 --lr 0.0002 --EPOCH 20` or `python src/run_mimic4.py`

## Test

### MIMIC-III

`python src/ADSNet.py --type mimic3 --Test --resume saved/0.0015_64_0.1_0.97_0_mimic3/Epoch_3_JA_0.5459_DDI_0.0839.model --cuda 0`

### MIMIC-IV

`python src/ADSNet.py --type mimic4 --Test --resume saved/0.0002_64_0.1_0.97_3_mimic4/Epoch_18_JA_0.5111_DDI_0.0841.model --cuda 3`
