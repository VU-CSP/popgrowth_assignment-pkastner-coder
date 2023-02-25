::: {.cell .markdown colab_type="text" id="view-in-github"}
<a href="https://colab.research.google.com/github/VU-CSP/quantbio-assignments/blob/main/PopGrowthLecture_assignment.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>
:::

::: {.cell .markdown id="03994108-9a10-4b4c-b4bf-0a48f6d1c023"}
Analysis of Cell Proliferation
==============================

Objectives
----------

This notebook has sections designed to: 1) Provide background on cell
proliferation\
2) Describe basic mathematical models of population growth\
3) Analyze dynamic cell count data\
4) Perform linear and nonlinear regression of models fit to cell count
data\
5) Create functions in Python to perform calcuations

Cell proliferation
------------------

### Nomenclature

-   *Growth* can mean an increase in mass or volume (at the individual
    level)
-   *Growth* can also indicate an increase in population size (e.g. the
    number of cells in a tumor)
-   *Proliferation* generally refers to change in the number of
    individuals within a group (e.g. exponential population growth)
-   *Tumor growth* generally refers to the size of the tumor, but an
    increase in the number of tumor cells is implicit

### Typical assays of proliferation, survival and death

-   Usually performed to test effects of perturbation (e.g. a drug)\
-   Most are static assays (take a snapshot)\
-   Multiple measurements require multiple samples (since they are
    destructive)\
-   Typically reported as fraction of control or percent change
    (relative, not absolute metrics)\
-   Biomarkers of the processes that change cell population size

Images below are representatives of 1) fluorescence microscopy of
calcein/propidium iodide stained cells, 2) flow cytometry of cells
stained with FxCycle violet and phospho-histone H3, 3) fluorescence
microscopy of annexin A5-fluorescein-stained cells and 4) fluorescence
microscopy of cells after addition of caspase 3 substrate that becomes
fluorogenic upon cleavage by caspase 3 (pink).

<table>
    <tr>
      <td>
      <img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/calcein-PI.png?raw=1" style="width: 200px;" />
      </td>
      <td>
      <img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/flow-mitosis.png?raw=1" style="width: 200px;" />
      </td>
      <td>
      <img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/Anx5.png?raw=1" style="width: 200px;" />
      </td>
      <td>
      <img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/activeCasp3.png?raw=1" style="width: 200px;" />
      </td>
     </tr>
</table>
:::

::: {.cell .markdown id="7ac8861e-0e5d-4d2f-b442-238f48bafd7d"}
### Exponential growth

#### *A math refresher*

<img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/Math-ugh.png?raw=1" style="width: 500px;"/>

-   Definition of Logarithm: $log_e e^x = x$, where $e > 0$ and $e ≠ 1$\
-   Product Rule: $log(xy) = log(x) + log(y)$
-   Base change rule: $log_a(n) = log_b(n)/log_a(b)$

#### **Exponential growth equation**

$P(t) = P_0e^{at}$, where\
$P(t)$ is the population size at time = $t$,\
$P_0$ is initial population size, and\
$a$ is the growth rate constant.

*The logarithm of the growth equation is linear*\
$log_e P(t) = log_e P_0e^{at} = log_e P_0 + log_e e^{at} = log_e P_0 + at$

*(reorganizing to the form of $y = ax + b$)*\
$log_e P(t) = at + log_e P_0$, where,\
growth rate = $a$ = *slope of the line* and log of initial population
size = *y intercept*

##### **This means that you can fit exponentially growing population data with linear models**

This also works in log2 scale using a population doubling function:\
$P(t) = P_02^{at}$,\
which allows for an easier biological interpretation.
:::

::: {.cell .markdown id="25b229c6-432b-4d6c-9979-0f67c3246d34"}
Plotting and interpreting cell population growth data
-----------------------------------------------------

These graphs were previously generated and are shown for reference. Data
are shown in linear, log2 and normalized log2 scales. Lines shown on
log2 and normalized log2 plots represent linear model fits and the slope
(proliferation rate) and doubling time (1/proliferation rate) parameters
of the optimal model fit are shown.

<img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/pop_growth_difft_scales.png?raw=1" style="width: 800px;" />
:::

::: {.cell .markdown id="f5d5336f-8a1c-4beb-adc0-6ba4e0b63804"}
### Divergence from exponential growth

There are numerous conditions that result in non-exponential cell
population growth, for example, when cells fill in their available space
(a.k.a. contact inhibition).
:::

::: {.cell .markdown id="095e56de"}
<img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/contact_inhibition.png?raw=1" style="width: 300px;"/>
:::

::: {.cell .markdown id="bcafe3ca"}
There are some specific models that have been used to model growth
inhibition, including the **Logistic** and **Gompertz** functions.
:::

::: {.cell .markdown id="fa4c4a18-c3cc-47ea-9e68-5f468497ff94"}
<img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/evidence_for_Gompertz_tumors.png?raw=1" style="width: 800px;"/>\
<img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/logistic_growth_model.jpeg?raw=1" style="width: 800px;"/>\
<img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/gompertz_growth_model.jpeg?raw=1" style="width: 800px;"/>
:::

::: {.cell .markdown id="ec305d36-2c3b-4d52-b806-054b615a84f7"}
*We will do some exploratory data analysis of a small cell count data set to interpret how populations of cells grow in culture.* {#we-will-do-some-exploratory-data-analysis-of-a-small-cell-count-data-set-to-interpret-how-populations-of-cells-grow-in-culture}
---------------------------------------------------------------------------------------------------------------------------------
:::

::: {.cell .markdown id="40e0a719-37e3-4a84-8668-769a294dd293"}
### First, import necessary Python packages
:::

::: {.cell .code id="549f8f33-de74-4ddd-9645-22a758ea2814"}
``` {.python}
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import os
```
:::

::: {.cell .markdown id="Sfrp0w8cysS1"}
See whether we are running in Google Colab or not.
:::

::: {.cell .code id="CLSZT8xMyphI"}
``` {.python}
try:
  import google.colab
  IN_COLAB = True
except:
  IN_COLAB = False
```
:::

::: {.cell .markdown id="ad299e4b-bd09-40c9-93ce-a8959d76a375"}
### Load custom functions

File name `growth_fxns.py` includes functions for:

-   flattening a matrix (nested lists): `flatten_matrix`\
-   stochastic simulation of exponential growth: `gillespie_growth`\
-   deterministic solution of exponential growth: `exp_growth`

-   deterministic solution of logistic growth: `mylogistic`
-   deterministic solution of Gompertzian growth: `gompertz`
:::

::: {.cell .code id="c72e3dc2-f48c-447a-95de-f428c2f6b3da" outputId="f0565970-6a02-4f86-a435-ad7f43da0c2f"}
``` {.python}
if IN_COLAB and not os.path.exists("growth_fxns.py"):
    !wget "https://raw.githubusercontent.com/VU-CSP/quantbio-assignments/main/growth_fxns.py"
from growth_fxns import *
```
:::

::: {.cell .markdown id="95fd8d3f-61c2-463d-8b01-1e961269af9c"}
### Adjust seaborn plot settings

*To make the graphics prettier.* You can learn more about seaborn at the
[seaborn website]()
:::

::: {.cell .code id="f28ff5a6-dc57-4fe3-a49f-d448f8bbc226"}
``` {.python}
sns.set_context("notebook", font_scale=1.5, rc={"lines.linewidth": 2.5})
```
:::

::: {.cell .markdown id="fb6587a1-2b3d-4aaf-ad96-c50e6eb7547a"}
#### Load a small dataset

Data from multiwell plate of cells, some of which have been treated with
a drug.
:::

::: {.cell .code id="5ffdbde1-987c-4817-9487-288cce2d3509" outputId="e0bc3046-bb70-466a-c087-7e5cb782c443"}
``` {.python}
if IN_COLAB and not os.path.exists("MCF10A_trametinib.csv"):
    !wget "https://raw.githubusercontent.com/VU-CSP/quantbio-assignments/main/MCF10A_trametinib.csv"
d = pd.read_csv("MCF10A_trametinib.csv")
d.head()
```
:::

::: {.cell .markdown id="4ee5278b-1d1e-4427-a596-8151c61ead1a"}
### Add new columns

Add values for log2(cell.count) (`l2`) and population doublings (`pd`).
Calculating log2 values are easy since it is calculated directly from
the `cell.count` values. Population doublings must be normalized for
each well independently. To better understand each step, try to pull out
the code in smaller chunks and run them in different cells.
:::

::: {.cell .code id="4312e53f-babd-475b-91c0-a20cc04b03c1"}
``` {.python}
d.insert(2,"l2",np.empty_like(d["cell.count"]))
d.insert(3,"pd",np.empty_like(d["cell.count"]))
d["l2"] = np.round(np.log2(d["cell.count"]),3)
d["pd"] = flatten_matrix([np.round(d[d["well"]==w]["l2"] - d[d["well"]==w]["l2"].to_list()[0],3) for w in d["well"].unique()])
```
:::

::: {.cell .code id="e1348e04-bfe8-44f4-9069-bc4a277a6e27" outputId="a61d58b8-3fe2-49fa-89a1-5f5542a232b8"}
``` {.python}
d.head()
```
:::

::: {.cell .markdown id="56245c23-c8a9-4d8e-ace1-a372b934377f"}
:::

::: {.cell .markdown id="d1222d88-bd97-494f-83c7-658156b50806"}
Assignment task \#1
-------------------

Copy and execute a chunk of code from the line assigning values to
`d["pd"]`. Describe what you did and the output in the following markup
block.
:::

::: {.cell .code id="5c2bb7c4-bb47-4c3b-99b2-70a002d3a163"}
``` {.python}
# Add your code here
```
:::

::: {.cell .markdown id="ae735ec1-1f4c-45ff-9290-56c9def398ed"}
*Put your description of the code in the previous block here.*
:::

::: {.cell .markdown id="5b56a147-be3b-4a8d-b833-37357a76d1c5"}
#### Plot data in different scales

We will look at a single well (`A02`) of data in linear scale (direct
cell counts), log2 scale and as population doublings (log2 scale
normalized to 1 cell at the first time point).
:::

::: {.cell .code id="7adfe491-1870-4265-97e7-24551c6c1569" outputId="6b5ffb12-aebe-44f9-b1c0-12dd62a6d680"}
``` {.python}
ss = d[d["well"]=="A02"]
# names = ['Linear', 'Log2', 'Normalized log2']
if IN_COLAB:
    fig, axs = plt.subplots(figsize=(12, 4))
else:
    fig, axs = plt.subplots(figsize=(12, 4), layout="constrained")
plt.subplot(131)
plt.ylabel('Cell number')
sns.scatterplot(data=ss, x="time", y="cell.count")
plt.subplot(132)
plt.ylabel('Log2(cell number)')
sns.scatterplot(data=ss, x="time", y="l2")
plt.subplot(133)
plt.ylabel('Population doublings')
sns.scatterplot(data=ss, x="time", y="pd")
plt.show()
```
:::

::: {.cell .markdown id="95a0731e-59a7-4cfb-9087-7f0232c3b0da"}
#### Look at aggregated data

Many seaborn functions will automatically perform the statistical
estimation. In the plots below, data will be grouped by `drug1` using
the seaborn `hue` argument.
:::

::: {.cell .code id="72a45a3e-b307-43cd-af53-1ac27c02fec0" outputId="c5f629e3-3e96-41c3-ca49-e50cda37fb0a"}
``` {.python}
if IN_COLAB:
    fig, (ax1, ax2, ax3) = plt.subplots(nrows=1, ncols=3, figsize=(12, 4))
else:
    fig, (ax1, ax2, ax3) = plt.subplots(nrows=1, ncols=3, figsize=(12, 4), layout="constrained")

plt.subplot(131)
plt.xlabel('Time (h)')
plt.ylabel('Cell number')
sns.lineplot(data=d, x="time", y="cell.count", hue="drug1")
plt.legend(loc='upper left', fontsize='8')
plt.subplot(132)
plt.xlabel('Time (h)')
plt.ylabel('Log2(cell number)')
sns.lineplot(data=d, x="time", y="l2", hue="drug1")
plt.legend(loc='upper left', fontsize='8')
plt.subplot(133)
plt.xlabel('Time (h)')
plt.ylabel('Population doublings')
sns.lineplot(data=d, x="time", y="pd", hue="drug1")
plt.legend(loc='upper left', fontsize='8')
plt.show()
```
:::

::: {.cell .markdown id="aaf8399f-9670-46fc-af7e-fe2cc85ebb88"}
The shaded areas represent confidence intervals. Compare the confidence
interval between the log2 and normalized log2 plots, especially at time
\< 100 h.
:::

::: {.cell .markdown id="154dba49-a823-4eb6-a004-63a9698bb955"}
How many samples of each type are there? Calculate this by counting the
number of unique `well`s there are in each group (`drug1`==trametinib or
control)
:::

::: {.cell .code id="21c868af-fb1b-4f07-8ef1-6bd912932254" outputId="09190b77-9f7f-42c7-8e58-c6a9bbe1a2b2"}
``` {.python}
n_tram = len(d[d["drug1"]=="trametinib"]["well"].unique())
n_ctrl = len(d[d["drug1"]!="trametinib"]["well"].unique())

print(f"Wells with trametinib treatment: n = {n_tram}\nControl wells: n = {n_ctrl}")
```
:::

::: {.cell .markdown id="893b067d-b9d0-47bb-b4e1-7051aa6a3421"}
#### Look at data by well

To see each well of data individually we will set `hue` to color data by
`well`.
:::

::: {.cell .code id="fe669618-97e3-4dd6-8b98-74d85ccc42b1" outputId="b3ca6e0e-593f-4a43-bbaa-efdc7d75311e"}
``` {.python}
if IN_COLAB:
    fig, axs = plt.subplots(nrows=1, ncols=3, figsize=(12, 4))
else:
    fig, axs = plt.subplots(nrows=1, ncols=3, figsize=(12, 4), layout="constrained")
plt.subplot(131)
plt.xlabel('Time (h)')
plt.ylabel('Cell number')
sns.lineplot(data=d, x="time", y="cell.count", hue="well")
plt.legend(loc='upper left', fontsize='8')
plt.subplot(132)
plt.xlabel('Time (h)')
plt.ylabel('Log2(cell number)')
sns.lineplot(data=d, x="time", y="l2", hue="well")
plt.legend(loc='upper left', fontsize='8')
plt.subplot(133)
plt.xlabel('Time (h)')
plt.ylabel('Population doublings')
sns.lineplot(data=d, x="time", y="pd", hue="well")
plt.legend(loc='upper left', fontsize='8')
plt.show()
```
:::

::: {.cell .markdown id="b70a0be3-611d-425c-8f75-14e1b9c5588d"}
How well do the individual lines reflect your expectations from the
aggregated data with confidence intervals? Do any wells clearly stand
out? Let\'s look only at wells A04, A05 and A07 in log2 scale and
visualize each individual data point using `scatterplot`.
:::

::: {.cell .code id="b0458a96-3200-4988-a61c-0c44aec6b0d4" outputId="6e4d3af2-d15b-4588-f5ae-31ab447f1e44"}
``` {.python}
# dtp = data to plot
dtp = d[(d["well"] == "A04") | (d["well"] == "A05") | (d["well"] == "A07")]
sns.scatterplot(data=dtp, x="time", y="l2", hue="well")
plt.legend(loc='upper left', fontsize='8')
```
:::

::: {.cell .markdown id="759f0e12-f434-48d4-a846-598badd68e64"}
Assignment task \#2
-------------------

Generate a scatterplot of population doublings over time for the same
wells as the block above (wells A04, A05 and A07). Describe the
difference you see between the new graph and the graph of data in log2
scale.
:::

::: {.cell .code id="b22f7170-76bc-42c9-af8e-fd623469b30c"}
``` {.python}
# Add your code for scatterplot of population doublings here
```
:::

::: {.cell .markdown id="cad14622-98de-4521-9cc7-8c6b01c7c583"}
*Describe your comparison of the data shown in log2 (`l2`) and
normalized log2 (`pd`).*
:::

::: {.cell .markdown id="b2ad573a-5ba5-4e3a-a6c3-eebc3730e83d"}
Apart from visually inspecting the data, we should use model fitting to
extract parameter values that can help us interpret the data
quantitatively.
:::

::: {.cell .markdown id="52ab9eb0-8019-4f34-8a94-f095019df421"}
Model fitting
-------------

### Use SciPy\'s `linregress` function or Seaborn\'s `lmplot` function

Because an exponential growth rate is directly proportional to the log
of the number of components (i.e., cells), we can fit each well of data
independently with a linear model to help interpret the data. Linear
models are easy to fit and fitting functions are commonly provided by
many different Python packages. We will find optimum parameters using
two different packages:
[`scipy.stats.lingress`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.linregress.html)
and [`seaborn.lmplot`](). You can learn more about these algorithms by
clicking on their names to get a link to online documentation.
:::

::: {.cell .code id="8fea67ea-f287-431b-b895-174fc71940d2"}
``` {.python}
from scipy.stats import linregress
```
:::

::: {.cell .markdown id="f800f6e5-54be-4420-8395-1f2a235cb829"}
Let\'s fit only the control data (`drug1.conc` == 0)
:::

::: {.cell .code id="9178cdf4-ea40-4404-9968-d2b4e8d8916f"}
``` {.python}
# dtf = data to fit
dtf = d[d["drug1.conc"]==0]
ctrl_wells = dtf["well"].unique()
```
:::

::: {.cell .markdown id="64346a18-877b-48df-ac54-5dc39d364c04"}
We will perform a list comprehension to fit data for each well
independently.
:::

::: {.cell .code id="0cbfeb0c-2bbd-445f-a35f-457fe69937f3"}
``` {.python}
res = [linregress(dtf[dtf["well"]==ctrl_wells[i]][["time","l2"]]) for i in np.arange(len(ctrl_wells))]
```
:::

::: {.cell .markdown id="b35b0afe-3e3b-4b70-ae4f-dc651f50d9ca"}
The `linregress` function returns a `namedtuple` of length 5. Let\'s
look at an example output.
:::

::: {.cell .code id="e5f82ff0-829a-44cb-8548-afd68b4fab4d" outputId="390400b8-55a3-489c-9a69-fe7957f8b837"}
``` {.python}
example_well = dtf["well"].unique()[1]
print(f"Example linregress output for well {example_well}")
res[1]
```
:::

::: {.cell .markdown id="4bf15674-73c4-407e-9774-4e1fd6d7ca08"}
Each value can be pulled out independently using the respective names.
We will pull out all `slope` values, which represent the population
doubling (proliferation) rate. To make it a bit easier to read we will
also round the values to 4 decimal places.
:::

::: {.cell .code id="ca448f0e-839c-4bf6-bd73-1102897d1196" outputId="33b9e357-7f72-4f0e-aed6-0d87b95b82ed"}
``` {.python}
prates = pd.Series(data=[np.round(r.slope,4) for r in res], index=ctrl_wells)
prates
```
:::

::: {.cell .markdown id="0866a88c-9647-4c9f-bc19-11cdf04e3af4"}
Most values are above 0.05 and most are close to 0.06, but one value
looks low (A05: 0.0421). We can check for goodness of fit for linear
regression by calculating $R^2$. (Only `rvalue` is provided, so we need
to square it using `**2`.)
:::

::: {.cell .code id="f286e02f-4282-431c-bd00-5782b1ec398d" outputId="32ae92c1-1a44-4d01-f5ba-b3309f8ccce2"}
``` {.python}
r2_vals = pd.Series(data=[np.round(r.rvalue**2,4) for r in res], index=ctrl_wells)
r2_vals
```
:::

::: {.cell .markdown id="56c980c4-7a5e-4584-bb46-929e993244df"}
Only well A05 has an $R^2$ value \< 0.99.
:::

::: {.cell .markdown id="aebafc13-a5dd-4cf8-a932-e44d057a416d"}
So, 9 of 10 control wells are explained well by linear models. This fits
with the visual evidence that something anomalous happens to the cell
counts in well A05 after \~75. This is consistent with a possible
technical problem when medium is changed in the experiment @ \~ 72h.
:::

::: {.cell .markdown id="14240290-81c5-4de3-bef8-274eb4338c04"}
Assignment task \#3
-------------------

Perform linear regression using the `linregress` function on the
trametinib-treated wells. Describe how the proliferation rates compare
to the rates of the control wells in the subsequent markdown block.
:::

::: {.cell .code id="3a6d7961-96e5-4f57-aa99-9a808a3dbe27"}
``` {.python}
# perform linear regression on the trametinib-treated wells
```
:::

::: {.cell .markdown id="68348d8a-9f8c-44ba-bc41-0a9548286b76"}
*Describe here your comparison of the rates between trametinib-treated
wells and the control wells.*
:::

::: {.cell .markdown id="a543b1dd-6136-4c6b-81a8-f4beec76df7c"}
We will visualize the linear model fitting using the seaborn `lmplot`
function, which uses SciPy `linregress` function itself. This is a
simple way to visualize the fits and their confidence intervals.
:::

::: {.cell .code id="65b0b21d-ffc3-42c7-b1d7-913d281e39ac" outputId="3b2f4ce1-b3db-4f39-8f74-38817800820b"}
``` {.python}
p = sns.lmplot(data=d, x="time", y="pd", hue="well")
```
:::

::: {.cell .markdown id="dbbd32cc-aa59-4839-922f-8443c38ea42b"}
We can also get fit parameter values from models fitting to all data
from each condition (control or trametinib-treated).
:::

::: {.cell .code id="cdeaa57f-6ae9-4f2b-a254-08bdfbc8da22" outputId="c30c48ef-4ac0-4389-8ce1-b375d51ec48c"}
``` {.python}
p = sns.lmplot(data=d, x="time", y="pd", hue="drug1")
```
:::

::: {.cell .markdown id="86f8611a-f42f-4b12-b162-f7ebd8e26466"}
Non-loglinear data
------------------

When cells are in conditions that limit their proliferation or increase
cell death, such as when contact inhibited or treated with drugs, their
growth may appear nonlinear. To analyze data like this we can interpret
the data using nonlinear model fitting. For this exercise we will use
simulated data.
:::

::: {.cell .code id="1dd0cd18-72e6-4d70-ab38-b0bc3109352a" outputId="0ca63a2e-e051-4722-db30-0c61371319b8"}
``` {.python}
np.random.seed(7)
times_by3 = np.arange(0,126,3)
mycounts = mylogistic(t=times_by3, P0=100, rate=0.06, K=1000)
sim_data = pd.DataFrame.from_dict({"time":times_by3,"cell.count":flatten_matrix([np.random.normal(x,0.05*x,1) for x in mycounts])})
sim_data["pd"] = np.log2(sim_data["cell.count"]/sim_data["cell.count"][0])
sim_data.head()
```
:::

::: {.cell .code id="f9d9c4a9-903e-4ae1-9dac-7dae6b90f08d" outputId="688a0518-634a-433d-9307-bd88de77561e"}
``` {.python}
sns.scatterplot(data=sim_data, x="time", y="pd")
```
:::

::: {.cell .markdown id="5725808c-f3b2-47ed-b7a8-514a63b8deb9"}
Nonlinear model fitting with SciPy\'s `curve_fit` function
----------------------------------------------------------

Nonlinear model fitting is more complicated and there aremany ways that
optimal parameter values can be found. There is an entire field of
research around parameter optimization! We will use a specific method
employed by SciPy (the
[`scipy.optimize.curve_fit`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html)
function) that has the objective to minimize the sum of the squared
residuals of model to data. It returns optimized coefficient values
(`popt`) and the estimated covariance of `popt`. The diagonals provide
the variance of the parameter estimates. To compute one standard
deviation of parameter errors, we will use
`perr = np.sqrt(np.diag(pcov))`.

This approach to identify optimal parameter values is referred to as
least squares regression and attempts to minimize the function
$\sum_i(f(x_i,\beta)-y_i)^2$
:::

::: {.cell .code id="112958e8-43f8-489b-b32b-234f1b34852c"}
``` {.python}
from scipy.optimize import curve_fit
```
:::

::: {.cell .markdown id="46860fe6-0035-4c59-989e-a5c123672c2c"}
The function `{0:.4g}".format(x)` is for limiting the number of digits
of the calculated values (making viewing the values easier).
:::

::: {.cell .code id="889c0d0b-4721-438a-9017-344f56ab858c"}
``` {.python}
logistic_popt, logistic_pcov = curve_fit(mylogistic, sim_data["time"], sim_data["cell.count"])
logistic_popt_str = ["{0:.4g}".format(x) for x in logistic_popt]
```
:::

::: {.cell .markdown id="c95a0c96-8b81-4455-ac57-3b0a78a5fd7f"}
Printing the optimal parameter values.
:::

::: {.cell .code id="411cf5b6-294e-44fb-9627-176ba593a8be" outputId="9d29daa6-8a70-4ad0-a0e2-5f3b270ddf60"}
``` {.python}
print(f"Optimal parameter values for P0: {logistic_popt_str[0]}, rate: {logistic_popt_str[1]}, and K: {logistic_popt_str[2]}\n")
```
:::

::: {.cell .markdown id="09903214-aaff-458e-9e84-2429f5136a45"}
### Plot the data and overlay the model fit

To visualize on the plot we must convert to normalized log2 scale. We
will also include a linear model fit for comparison (standard output of
seaborn\'s `regplot`.
:::

::: {.cell .code id="2c0cd14d-55a1-4c7a-ad11-f868d49addfa" outputId="13c036df-d09e-46cd-f304-f5828428e01c"}
``` {.python}
x_pred = np.linspace(min(times_by3),max(times_by3),100)
y_pred = mylogistic(x_pred, *logistic_popt)
y_pred = np.log2(y_pred/y_pred[0])
sns.regplot(x="time", y="pd", data=sim_data)
sns.lineplot(x=x_pred, 
             y=y_pred, 
             color="red")
```
:::

::: {.cell .markdown id="f2014d0a-cbb7-4e39-9a71-cd77c70535e6"}
Do the same for a Gompertz model.
:::

::: {.cell .code id="f57f9d8e-5782-4ec0-aa60-7115c5c86c65"}
``` {.python}
gompertz_popt, gompertz_pcov = curve_fit(gompertz, sim_data["time"], sim_data["cell.count"])
gompertz_popt_str = ["{0:.4g}".format(x) for x in gompertz_popt]
```
:::

::: {.cell .code id="644dd92a-ca26-4129-aa05-4f8c2014d5df" outputId="d5a400c2-2d14-4ccf-82b9-fae9beba2d28"}
``` {.python}
print(f"Optimal parameter values for P0: {gompertz_popt_str[0]}, rate: {gompertz_popt_str[1]}, and K: {gompertz_popt_str[2]}\n")
```
:::

::: {.cell .code id="59b4da50-59c3-4eb8-8445-d2c09b9a4081" outputId="5006551f-0e93-4743-bc6c-795f0879af67"}
``` {.python}
x_pred = np.linspace(min(times_by3),max(times_by3),100)
y_pred = gompertz(x_pred, *gompertz_popt)
y_pred = np.log2(y_pred/y_pred[0])
sns.regplot(x="time", y="pd", data=sim_data)
sns.lineplot(x=x_pred, 
             y=y_pred, 
             color="red")
```
:::

::: {.cell .markdown id="5de323d7-c662-4a00-b5ae-7fe19b3074e2"}
### Limitations of these nonlinear growth models

#### Both logistic and Gompertz models:

-   Are phenomenological (they describe the result, not the cause)
-   Use a carrying capacity parameter ($K$); this may be relevant to
    space available in a culture well and/or average cell size, but how
    would you interpret different values in response to drug?

#### Gompertzian model:

-   Has initial assumptions that do not correspond to a stable,
    exponentially dividing population (infinite rate at time=0, rate is
    continually changing)
:::

::: {.cell .markdown id="411ea7d9-fcf1-44aa-92bd-4ce8fdcb1ddb"}
Assignment task (extra credit)
------------------------------

### How many days would it take for a single tumor cell to grow to a tumor the size of an egg?

**Assumptions:**

-   There are \~ $10^9$ tumor cells in 1 cm$^3$ (\~1 g)
-   Tumor Cell ≈ 1ng
-   Egg ≈ 35g
-   Average time per division (doubling time) ≈ 18h
-   Doubling rate = 1/doubling time
-   Population doubling equation: $P(t) = P_02^{rate*t}$
:::

::: {.cell .markdown id="4cb40054"}
<table>
    <tr>
        <td>
        <img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/cell_division.png?raw=1" style="width: 150px;" align="middle" /> 
        <td>
            $\Longrightarrow$ $\Longrightarrow$
        <td>
            <img src="https://github.com/VU-CSP/quantbio-assignments/blob/main/img/tumor.png?raw=1" style="width: 150px;"  align="middle" />
    </tr>
</table>
:::

::: {.cell .markdown id="c2f24f5b-7c9d-4a8a-b616-2721a4bc434c"}
*Try to write a function that calculates this. Start of function
definition is provided in code block below.*\
NOTE: to calculate log2, use the numpy function `np.log2()`\
NOTE: check the units!
:::

::: {.cell .code id="00a748b9-312d-455b-8e74-516d22d00687"}
``` {.python}
def timeToEgg(P0, egg, DT):
    '''
    P0=initial cell number
    egg=number of tumor cells in an egg-size tumor
    DT=time to double the population size (i.e., the average cell cycle time)
    '''
    # add your code for the function here
    return()
```
:::

::: {.cell .code id="9e33a969-ddac-42c0-b07a-80a45c04dcca"}
``` {.python}
```
:::

::: {.cell .markdown id="fc218c88-2a9f-40d0-9995-1c50e4543890"}
### Calculate time to egg-sized tumor from 100 cells

Do the calculation using the function you made.
:::

::: {.cell .code id="eac88800-2715-41ee-a061-953435113623"}
``` {.python}
# Execute your function with the correct input argument values.
# timeToEgg(P0=100,egg=<egg_val>,DT=<DT_val>)
```
:::

::: {.cell .code id="6aef3847-ab37-42cd-9f62-fd6a512d36a7"}
``` {.python}
```
:::