# Getting Started

## Installation

alibi-detect can be installed from [PyPI](https://pypi.org/project/alibi-detect/):

```bash
pip install alibi-detect
```

## Features

[Alibi Detect](https://github.com/SeldonIO/alibi-detect) is an open source Python library focused on 
**outlier**, **adversarial** and **drift** detection. The package aims to cover both 
online and offline detectors for tabular data, text, images and time series. 
Both **TensorFlow** and **PyTorch** backends are supported for drift detection. Alibi Detect does however 
not install PyTorch for you. Check the [PyTorch docs](https://pytorch.org/) how to do this.

To get a list of respectively the latest outlier, adversarial and drift detection algorithms, you can type:

```python
import alibi_detect
alibi_detect.od.__all__
```

```
['OutlierAEGMM',
 'IForest',
 'Mahalanobis',
 'OutlierAE',
 'OutlierVAE',
 'OutlierVAEGMM',
 'OutlierProphet',  # requires prophet: pip install alibi-detect[prophet]
 'OutlierSeq2Seq',
 'SpectralResidual',
 'LLR']
```

```python
alibi_detect.ad.__all__
```

```
['AdversarialAE',
'ModelDistillation']
```

```python
alibi_detect.cd.__all__
```

```
['ChiSquareDrift',
 'ClassifierDrift',
 'ClassifierUncertaintyDrift',
 'KSDrift',
 'LearnedKernelDrift',
 'LSDDDrift',
 'LSDDDriftOnline',
 'MMDDrift',
 'MMDDriftOnline',
 'RegressorUncertaintyDrift',
 'SpotTheDiffDrift',
 'TabularDrift']
```

Summary tables highlighting the practical use cases for all the algorithms can be found [here](../overview/algorithms.md).

For detailed information on the **outlier detectors**:

* [Isolation Forest](../methods/iforest.ipynb)

* [Mahalanobis Distance](../methods/mahalanobis.ipynb)
    
* [Auto-Encoder (AE)](../methods/ae.ipynb)

* [Variational Auto-Encoder (VAE)](../methods/vae.ipynb)

* [Auto-Encoding Gaussian Mixture Model (AEGMM)](../methods/aegmm.ipynb)

* [Variational Auto-Encoding Gaussian Mixture Model (VAEGMM)](../methods/vaegmm.ipynb)

* [Likelihood Ratios](../methods/llr.ipynb)
    
* [Prophet Detector](../methods/prophet.ipynb)
    
* [Spectral Residual Detector](../methods/sr.ipynb)
    
* [Sequence-to-Sequence (Seq2Seq) Detector](../methods/seq2seq.ipynb)

Similar for **adversarial detection**:

* [Adversarial AE Detector](../methods/adversarialae.ipynb)
  
* [Model Distillation Detector](../methods/modeldistillation.ipynb)

And **data drift**:
    
* [Kolmogorov-Smirnov Drift Detector](../methods/ksdrift.ipynb)

* [Least-Squares Density Difference Drift Detector](../methods/lsdddrift.ipynb)

* [Maximum Mean Discrepancy (MMD) Drift Detector](../methods/mmddrift.ipynb)

* [Learned Kernel MMD Drift Detector](../methods/learnedkerneldrift.ipynb)

* [Chi-Squared Drift Detector](../methods/chisquaredrift.ipynb)

* [Mixed-type Tabular Data Drift Detector](../methods/tabulardrift.ipynb)

* [Classifier Drift Detector](../methods/classifierdrift.ipynb)

* [Spot-the-diff Drift Detector](../methods/spotthediffdrift.ipynb)

* [Classifier and Regressor Uncertainty Drift Detectors](../methods/modeluncdrift.ipynb)

* [Online Maximum Mean Discrepancy Drift Detector](../methods/onlinemmddrift.ipynb)

* [Online Least-Squares Density Difference Drift Detector](../methods/onlinelsdddrift.ipynb)


## Basic Usage

We will use the [VAE outlier detector](../methods/vae.ipynb) to illustrate the usage of outlier and adversarial detectors in alibi-detect.

First, we import the detector:

```python
from alibi_detect.od import OutlierVAE
```

Then we initialize it by passing it the necessary arguments:

```python
od = OutlierVAE(
    threshold=0.1,
    encoder_net=encoder_net,
    decoder_net=decoder_net,
    latent_dim=1024
)
```

Some detectors require an additional `.fit` step using training data:

```python
od.fit(X_train)
```

The detectors can be saved or loaded as follows:

```python
from alibi_detect.utils.saving import save_detector, load_detector

filepath = './my_detector/'
save_detector(od, filepath)
od = load_detector(filepath)
```

Finally, we can make predictions on test data and detect outliers or adversarial examples.

```python
preds = od.predict(X_test)
```

The predictions are returned in a dictionary with as keys `meta` and `data`. `meta` contains the detector's metadata while `data` is in itself a dictionary with the actual predictions. It has either `is_outlier`, `is_adversarial` or `is_drift` (filled with 0's and 1's) as well as optional `instance_score`, `feature_score` or `p_value` as keys with numpy arrays as values.

The exact details will vary slightly from method to method, so we encourage the reader to become
familiar with the [types of algorithms supported](../overview/algorithms.md) in alibi-detect.