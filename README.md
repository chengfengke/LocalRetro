# LocalRetro
Implementation of Retrosynthesis Prediction with LocalRetro developed by prof. Yousung Jung group at KAIST (contact: ysjn@kaist.ac.kr).

## Developer
Shuan Chen<br>

## Requirements
* Python (version >= 3.6) 
* Numpy (version >= 1.16.4) 
* PyTorch (version >= 1.0.0) 
* RDKit (version >= 2019)
* DGL (version >= 0.5.2)
* DGLLife (version >= 0.2.6)

## Publication
TBD

## Usage
### [1] Download the raw data of USPTO-50K or USPTO-MIT dataset
See the README in `./data` to download the raw data files for training and testing the model.

### [2] Data preprocessing
A two-step data preprocessing is needed to train the LocalRetro model.

#### 1) Local reaction template derivation 
First go to the data processing folder
```
cd preprocessing
```
and extract the reaction template with specified dataset name (default: USPTO_50K).
```
python Extract_from_train_data.py -d USPTO_50K
```
This will give you four files, including 
(1) atom_templates.csv
(2) bond_templates.csv
(3) smiles2smarts_templates.csv
(4) template_rxnclass.csv (if train_class.csv exists in data folder)<br>

#### 2) Assign the derived templates to raw data
By running
```
python Run_preprocessing.py -d USPTO_50K
```
You can get four preprocessed files, including 
(1) preprocessed_train.csv
(2) preprocessed_val.csv
(3) preprocessed_test.csv
(4) labeled_data.csv<br>


### [3] Train LocalRetro model
Go to the localretro folder
```
cd ../scripts
```
and run the following to train the model with specified dataset (default: USPTO_50K) and use GRA or not (default: True)
```
python Train.py -d USPTO_50K
```
The trained model will be saved at ` LocalRetro/models/USPTO_50K.pth`<br>

### [4] Test LocalRetro model
To use the model to test on test set, simply run 
```
python Test.py -d USPTO_50K
```
to get the raw prediction file saved at ` LocalRetro/outputs/raw_prediction/USPTO_50K.txt`<br>
Finally you can get the reactants of each prediciton by decoding the raw prediction file
```
python Decode_predictions.py -d USPTO_50K
```
The decoded reactants will be saved at 
`LocalRetro/outputs/decoded_prediction/USPTO_50K.txt`<br>and 
`LocalRetro/outputs/decoded_prediction_class/USPTO_50K.txt`<br>

## Data Release
For users to reproduce the results shown in paper and quick retrosynthesis, we shared the trained models and results on the GitHub Release in this repo.

## Retrosynthesis on desired product
For quick implementation of retrosynthesis prediction via our model, see `Retrosynthesis.ipynb` for quick start.

## Reproduce the prediction accuracy in the paper
The exact and MaxFrag accuracy can be obtained by running the python notebook `Top_K_accuracy.ipynb` with changing using GRA or given reaction class.

#### Exact match accuracy (%) on USPTO-50K dataset without given reaction class 
*AT = Augmented Transformer

| Method | Top-1 | Top-3 | Top-5 | Top-10 | Top-50 |
| -------- | -------- | -------- | -------- | -------- | -------- |
| GLN         | 52.5 | 69.0 | 75.6 | 83.7 | 92.4 |
| G2Gs        | 48.9 | 67.6 | 72.5 | 75.5 |  /   |
| GraphRetro  | **63.8** | **80.5** | 84.1 | 85.9 | 87.2 |
| AT     | 53.5 | 69.4 | 81.0 | 85.7 | / |
| MEGAN | 48.1 | 70.7 | 78.4 | 86.1 | 93.2 |
| LocalRetro  | 53.4 | 77.5 | **85.9** | **92.4** | **97.7** |

#### Exact match accuracy (%) on USPTO-50K dataset with given reaction class

| Method | Top-1 | Top-3 | Top-5 | Top-10 | Top-50 |
| -------- | -------- | -------- | -------- | -------- | -------- |
| GLN         | 64.2 | 79.1 | 85.2 | 90.0 | 93.2 |
| G2Gs        | 61.0 | 81.3 | 86.0 | 88.7 |  /   |
| GraphRetro  | **67.8** | 82.7 | 85.3 | 87.0 | 87.9 |
| MEGAN | 60.7 | 82.0 | 87.5 | 91.6 | 95.3 |
| LocalRetro  | 63.8 | **86.8** | **92.4** | **96.3** | **97.9** |
