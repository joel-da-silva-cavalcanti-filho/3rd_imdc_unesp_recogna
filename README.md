
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

<strong> 7. References </strong>
---
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

