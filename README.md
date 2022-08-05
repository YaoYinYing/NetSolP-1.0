# NetSolP-1.0
NetSolP-1.0 predicts the solubility and usability for purification of proteins expressed in E. coli. The usability objective includes the solubility and expressibility of proteins. NetSolP-1.0 is based on protein language models (ESM12, ESM1b).

The webserver can be found at https://services.healthtech.dtu.dk/service.php?NetSolP. A standalone version of the software is available from the Downloads tab.
It contains the code for the webserver. Additionally, it also has the datasets, code for training and testing, and the trained models

**Edited by Yinying for clear Setup and usage.**

## Setup

install conda environment
```shell
conda create -n  NetSolP python
conda activate NetSolP
pip install -r requirements.txt
```
Fetch all pretrained models from official server.
```shell
aria2c -x https://services.healthtech.dtu.dk/services/NetSolP-1.0/netsolp-1.0.ALL.tar.gz
mkdir netsolp
pushd netsolp
tar xf ../netsolp-1.0.ALL.tar.gz
popd 

db_path='/mnt/db/netsolp'
mkdir -p ${db_path}
mv netsolp/models/ ${db_path}
rm -rf netsolp
```

## Usage

All models go into `${db_path}/models/`
Model name format: `{PREDICTION_TYPE}_{MODEL_TYPE}_{Fold number 0-4}_quantized.onnx`

Example Usage: 

```shell
python predict.py --MODELS_PATH ${db_path}/models/ --FASTA_PATH ./test_fasta.fasta --OUTPUT_PATH ./test_preds.csv --MODEL_TYPE ESM12 --PREDICTION_TYPE S
```

For solubility `PREDICTION_TYPE` is `S`, for usability `PREDICTION_TYPE` is `U` and to output `both` together in the same file it is `SU`
`MODEL_TYPE` is `Distilled` or `ESM12` or `ESM1b` or `Both` (ensemble prediction)

Recommended model is Distilled (`NetSolP-D` from the paper) since it has the best trade-off between speed and performance.

For training:
```shell
cd TrainAndTest/
python train.py
```
more details and requirements in the README of folder TrainAndTest.

## License

The code is licensed under the [BSD 3-Clause license](https://github.com/TviNet/NetSolP-1.0/blob/main/LICENSE).

## Citation

```
@article {Thumuluri2021,
	author = {Thumuluri, Vineet and Martiny, Hannah-Marie and Armenteros, Jose J. Almagro and Salomon, Jesper and Nielsen, Henrik and Johansen, Alexander R.},
	title = {NetSolP: predicting protein solubility in E. coli using language models},
	elocation-id = {2021.07.21.453084},
	year = {2021},
	doi = {10.1101/2021.07.21.453084},
	publisher = {Cold Spring Harbor Laboratory},
	URL = {https://www.biorxiv.org/content/early/2021/07/22/2021.07.21.453084},
	eprint = {https://www.biorxiv.org/content/early/2021/07/22/2021.07.21.453084.full.pdf},
	journal = {bioRxiv}
}

```

