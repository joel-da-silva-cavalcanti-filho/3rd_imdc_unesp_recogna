
<!-- <strong> Important: </strong>Don't forget to set it as a public repository. In addition, please ensure your model repository includes a clear and complete `README.md` file containing the following information: -->
<strong>Team and Contributors</strong>
---
State University of São Paulo, Department of Computing, Recogna Laboratory

Joel da Silva Cavalcanti Filho<br>
João Renato Ribeiro Manesco<br>
Eduardo Roldão Nonato Perondini<br>


<strong> 2. Repository Structure </strong>
---
<!-- A brief description of the contents and purpose of >each folder and file in the repository.-->


<strong> 3. Libraries and Dependencies </strong>
---
<!-- A list of all libraries and packages used to process the data and train your model. -->

### Python Dependencies

| Package | Imported Modules / Objects | Purpose |
|---------|----------------------------|---------|
| `torch` | `torch`, `TensorDataset`, `DataLoader`, `AdamW`, `OneCycleLR` | Deep learning framework used for model training, optimization, data loading, and learning rate scheduling. |
| `transformers` | `PatchTSTConfig`, `PatchTSTForPretraining`, `PatchTSTForPrediction`, `get_scheduler` | Hugging Face implementation of the PatchTST architecture and training utilities. |
| `tsfm-public` | `get_model` | Loads the IBM Granite Time Series TinyTimeMixer (TTM) foundation model. |
| `pandas` | `pandas` (`pd`) | Data manipulation and preprocessing. |
| `numpy` | `numpy` (`np`) | Numerical computations and array operations. |
| `wandb` | `wandb` | Experiment tracking and logging of training metrics. |
| `tqdm` | `tqdm` | Progress bars for training and evaluation loops. |
| `pathlib` | `Path` | Platform-independent file and directory handling. |
| `os` | `os` | Operating system utilities (file handling, CPU detection, paths). |
| `json` | `json` | Reading and writing JSON files for configuration and prediction outputs. |

<strong> 4. Data and Variables </strong>
---

| Dataset | Variables Used |
|---------|----------------|
| `dengue.csv.gz` | `casos` |
| `climate.csv.gz` | `temp_min`, `temp_med`, `temp_max`<br>`precip_min`, `precip_med`, `precip_max`<br>`pressure_min`, `pressure_med`, `pressure_max`<br>`rel_humid_min`, `rel_humid_med`, `rel_humid_max`<br>`thermal_range`, `rainy_days` |
<!--
* Which datasets and variables were used?
* How was the data pre-processed?
* How were the variables selected? Please point to the relevant part of the code.
-->

<strong>5. Model Training </strong> 
---
### TTM-R2 

| Item                    | Description                                                                                               |
| ----------------------- | --------------------------------------------------------------------------------------------------------- |
| Model                   | IBM Granite Time Series TinyTimeMixer (TTM-R2) (`ibm-granite/granite-timeseries-ttm-r2`)                  |
| Fine-tuning             | Only the **decoder** and **prediction head** were trained; all remaining backbone parameters were frozen. |
| Input context           | Sliding window applied in the datasets `train1`, `train2`, `train3`, `train4`                             |
| Forecast horizon        | 52 weekly observations                                                                                    |
| Input transformation    | `log1p` applied to both input and target tensors                                                          |
| Batch size              | 256                                                                                                       |
| Optimizer               | AdamW                                                                                                     |
| Learning rate           | 3 × 10⁻³                                                                                                  |
| Weight decay            | 1 × 10⁻²                                                                                                  |
| Learning rate scheduler | OneCycleLR                                                                                                |
| Number of epochs        | 100                                                                                                       |
| Gradient clipping       | 1.0                                                                                                       |
| Model selection         | Lowest average training loss                                                                              |
| Saved model             | `./melhor_modelo_ttm_dengue`                                                                              |

| Item                | Description                                                                                                                          |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Input data          | `national_tensor_train_1.pt`, `national_tensor_train_2.pt`, `national_tensor_train_3.pt`,`national_tensor_train_4.pt`                |
| Output              | Weekly forecasts for the required target locations in the IMDC submission format.                                                    |
| Execution           | Run the training script to fine-tune the model, then run the prediction script using the saved model (`./melhor_modelo_ttm_dengue`). |

### PatchTST
| Parameter                          | Value                    |
| ---------------------------------- | ------------------------ |
| Architecture                       | `PatchTSTForPretraining` |
| Model type                         | `patchtst`               |
| Activation function                | `gelu`                   |
| Context length                     | `305`                    |
| Prediction length                  | `24`                     |
| Number of input channels           | `15`                     |
| Number of targets                  | `1`                      |
| Hidden dimension (`d_model`)       | `128`                    |
| Feed-forward dimension (`ffn_dim`) | `512`                    |
| Number of hidden layers            | `3`                      |
| Number of attention heads          | `4`                      |
| Patch length                       | `16`                     |
| Patch stride                       | `1`                      |
| Stride                             | `8`                      |
| Pooling type                       | `mean`                   |
| Distribution output                | `student_t`              |
| Loss function                      | `mse`                    |
| Scaling                            | `std`                    |
| Positional encoding                | `sincos`                 |
| Normalization                      | `batchnorm`              |
| Normalization epsilon              | `1e-5`                   |
| Pre-normalization                  | `True`                   |
| Bias                               | `True`                   |
| Channel attention                  | `True`                   |
| Share embedding                    | `True`                   |
| Share projection                   | `True`                   |
| Use CLS token                      | `False`                  |
| Input masking                      | `True`                   |
| Mask type                          | `random`                 |
| Random mask ratio                  | `0.5`                    |
| Forecast mask patches              | `[2]`                    |
| Channel-consistent masking         | `False`                  |
| Unmasked channel indices           | `None`                   |
| Mask value                         | `0`                      |
| Dropout (attention)                | `0.0`                    |
| Dropout (feed-forward)             | `0.0`                    |
| Dropout (head)                     | `0.0`                    |
| Dropout (path)                     | `0.0`                    |
| Dropout (positional)               | `0.0`                    |
| Weight initialization std          | `0.02`                   |
| Number of parallel samples         | `100`                    |
| Output range                       | `None`                   |
| Data type                          | `float32`                |
| Transformers version               | `5.3.0`                  |


| Item                        | Description                                                                                                                                                                  |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Model                       | PatchTST (`PatchTSTForPretraining`)                                                                                                                                          |
| Architecture                | Transformer-based self-supervised pretraining model                                                                                                                          |
| Training objective          | Self-supervised pretraining using masked patch reconstruction                                                                                                                |
| Input variables             | Dengue cases and climate variables                                                                                                                                           |
| Input context               | Sliding window applied in the datasets `train1`, `train2`, `train3`, `train4`                                                                                               |
| Prediction horizon          | 52 weeks (configuration parameter)                                                                                                                                           |
| Patch length                | 16                                                                                                                                                                           |
| Patch stride                | 8                                                                                                                                                                            |
| Number of input channels    | Equal to the number of variables in the input tensor                                                                                                                         |
| Channel attention           | Enabled (`True`)                                                                                                                                                             |
| Shared embedding            | Enabled (`True`)                                                                                                                                                             |
| Optimizer                   | AdamW                                                                                                                                                                        |
| Backbone learning rate      | 1 × 10⁻⁵                                                                                                                                                                     |
| Head learning rate          | 1 × 10⁻³                                                                                                                                                                     |
| Weight decay                | 1 × 10⁻²                                                                                                                                                                     |
| Learning rate scheduler     | Linear scheduler                                                                                                                                                             |
| Warm-up steps               | 0                                                                                                                                                                            |
| Batch size                  | 200                                                                                                                                                                          |
| Number of epochs            | 1                                                                                                                                                                            |
| Hyperparameter optimization | None. Hyperparameters were manually selected.                                                                                                                                |
| Model checkpoint            | Saved after each training iteration as `weights/PatchTST-Dengue-ClimateExogs_v{1,2,3,4}.pth` and in Hugging Face format under `weights/Pretrained_PatchTST_dengue_climate_exogs_v{1,2,3,4}`. |

| Item                | Description                                                                                                                                                                                              |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Input data          | Tensor containing weekly dengue cases and climate variables.                                                                                                                                             |
| Output              | Pretrained PatchTST model weights used for downstream forecasting.                                                                                                                                       |
| Execution           | Run the pretraining script to generate the pretrained weights. Afterwards, run the forecasting script that loads `weights/Pretrained_PatchTST_dengue_climate_exogs_v{1,2,3,4}` to produce the final predictions. |

### Fine-tuning Routine | PatchTST

| Item                          | Description                                                                                    |
| ----------------------------- | ---------------------------------------------------------------------------------------------- |
| Model                         | PatchTST (`PatchTSTForPrediction`)                                                             |
| Initialization                | Pretrained weights loaded from `weights/Pretrained_PatchTST_dengue_climate_exogs_v{1,2,3,4}`   |
| Architecture                  | Transformer-based PatchTST                                                                     |
| Input variables               | Dengue cases and climate variables                                                             |
| Input context                 | Sliding window applied in the datasets `train1`, `train2`, `train3`, `train4`                  |
| Forecast horizon              | 52 weeks                                                                                       |
| Patch length                  | 16                                                                                             |
| Patch stride                  | 8                                                                                              |
| Number of input channels      | Equal to the number of variables in the input tensor                                           |
| Channel attention             | Enabled (`True`)                                                                               |
| Shared embedding              | Enabled (`True`)                                                                               |
| Optimizer                     | AdamW                                                                                          |
| Backbone learning rate        | 1 × 10⁻⁵                                                                                       |
| Prediction head learning rate | 1 × 10⁻³                                                                                       |
| Weight decay                  | 1 × 10⁻²                                                                                       |
| Learning rate scheduler       | Linear scheduler                                                                               |
| Warm-up steps                 | 0                                                                                              |
| Number of epochs              | 3                                                                                              |
| Mini-batch size               | 183 samples                                                                                    |
| Validation batch size         | 200                                                                                            |
| Early stopping                | Enabled (based on validation loss)                                                             |
| Hyperparameter optimization   | None. Hyperparameters were manually selected.                                                  |
| Model checkpoint              | Saved after each epoch as both PyTorch (`*.pth`) and Hugging Face (`save_pretrained`) formats. |

| Item             | Description                                                                                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Fine-tuning code | `finetuning_PatchTST.py` (replace with the actual script name if different)                                                                           |
| Pretrained model | `weights/Pretrained_PatchTST_dengue_climate_exogs_v{1,2,3,4}`                                                                                                 |
| Training data    | Strided training windows generated from the dengue and climate tensor.                                                                                |
| Validation data  | Validation tensor loaded through `ValidationDataset`.                                                                                                 |
| Output           | Fine-tuned PatchTST model for weekly dengue forecasting.                                                                                              |
| Saved model      | `weights_v2/climate_exogs/<train_file>_FineTuned_Dengue-PatchTST-ClimateExogs_v{1,2,3,4}.pth` and the Hugging Face checkpoint specified by `checkpoint_file`. |
| Execution        | Load the pretrained PatchTST weights, run the fine-tuning script, and use the resulting checkpoint to generate forecasts for the target locations.    |


<!---
* Description of how the model was trained. If applicable, describe any hyperparameter optimization techniques used.

* Please specify where the code for training and generating forecasts is located, and provide instructions on how to run it.
-->

<strong> 5. Data Usage Restriction </strong>
---
<!--
Describe how you handled the requirement of using only data up to EW 25 of the current year to generate predictions from EW 41 of the same year to EW 40 of the next year.
-->

<strong> 6. Predictive Uncertainty </strong> 
---
<!-- How are your prediction intervals computed? -->

```python
z = {
    "50": 0.67448975,
    "80": 1.28155157,
    "90": 1.64485363,
    "95": 1.95996398
}

resultado = pd.DataFrame({
    "date": datas_validacao,
    "lower_95": np.clip(pred - z["95"] * sigma.iloc[i], 0, None),
    "lower_90": np.clip(pred - z["90"] * sigma.iloc[i], 0, None),
    "lower_80": np.clip(pred - z["80"] * sigma.iloc[i], 0, None),
    "lower_50": np.clip(pred - z["50"] * sigma.iloc[i], 0, None),
    "median": pred,
    "predicted": pred,
    "upper_50": pred + z["50"] * sigma.iloc[i],
    "upper_80": pred + z["80"] * sigma.iloc[i],
    "upper_90": pred + z["90"] * sigma.iloc[i],
    "upper_95": pred + z["95"] * sigma.iloc[i],
})
```

<strong> 7. References </strong>
---

* EKAMBARAM, Vijay et al. Tiny time mixers (ttms): Fast pre-trained models for enhanced zero/few-shot forecasting of multivariate time series. Advances in Neural Information Processing Systems, v. 37, p. 74147-74181, 2024.
* NIE, Yuqi et al. A time series is worth 64 words: Long-term forecasting with transformers. arXiv preprint arXiv:2211.14730, 2022.
* MOSQLIMATE. Infodengue-Mosqlimate Dengue Sprint 2026: Instructions. [S. l.], 2026. Disponível em: https://sprint.mosqlimate.org/instructions/. Acesso em: 3 jul. 2026.

<!--
If your model is based on a published or preprint (e.g., arXiv) paper, include the citation, DOI, and link.

<strong> An illustrative example from the previous edition is available [here](https://mosqlimate.org/davibarreira/jbd-mosqlimate-sprint).</strong>

### Using `mosqlient package`: 

**Checking for minimal configuration requirements on your operating system**

Since you are going to use a python library to submit your work, before installing it you need to make sure that you OS has a working Python installation.

On a debian-based Linux distribution(Ubuntu, Mint, etc.), just run the following command:

```bash
sudo apt install python3-dev jupyter python3-venv python3-pip
```

On Mac OS (using homebrew):
```
# Install Python and Jupyter
brew install python
pip3 install jupyter

# Install virtual environment and development headers
pip3 install virtualenv
```

**Installing the Mosqlient library**

To install the library for Python, from the OS terminal type: 

```bash
$ pip install -U mosqlient
```

Ensure your Python version is 3.10 or higher to install the latest version of the package (2.1).

for R:

```R
> library(reticulate)
> py_install("mosqlient")
```

You need to already have the `reticulate` package installed.

**Starting your work!**
We prepared a couple of demo Jupyter notebooks to get you started.  In you local computer make sure you have Python 3.10 or higher and Jupyter installed.

If you are an R user, make sure you have the R kernel installed in your Jupyter notebook. you can install it by running the following command **in an R terminal**:

```R
> install.packages("IRkernel")
> IRkernel::installspec()
```

After this you can just open the notebooks indicated below and follow the instructions in them.

Follow the [R demo rmd](/Demo%20Notebooks/R%20demo.Rmd) or [Python demo notebook](/Demo%20Notebooks/Python%20demo.ipynb) to learn of the essential steps you must follow to complete a submission of your work. For more details check the [mosqlient documentation](https://mosqlimate-client.readthedocs.io/en/latest/tutorials/API/registry/).  Video tutorials for prediction submission are available here: one for [R](https://www.youtube.com/watch?v=57hM-dVY4hA&list=PLh4FLfhFN5irN_IoZvy4c3cf4ZWrSWrwF&index=3) and another for [Python](https://www.youtube.com/watch?v=YorYQ6phAfw&list=PLh4FLfhFN5irN_IoZvy4c3cf4ZWrSWrwF&index=2). If you run into dificulties, please reach out fo help at our [discord server](https://discord.gg/yqtgW4TC). 


-->

