
# Effect Size, P-Values and Power - Lab

## Introduction 

In this lab, you'll run simulations to continue to investigate the relationship between effect size, p-values, power and sample size. To do this, you'll   

## Objectives
You will be able to:
* Describe how effect size, sample size and p_value are related to each other. 
* Use Welch's t-test for distributions that do not have a normality assumption.
* Plot visualisations to confirm the calculations and intuitions towards p_value and effect size. 
* Explain how with a same effect size, we may see different p_values with increasing number of simulations. 

## Philosophical Review

Remember that the underlying question behind all hypothesis tests is:

>"What is the probability I would see this effect due to random fluctuations if there was actually no effect?" 

This is exactly what a p-value represents: the chance that the observed data would satisfy the null hypothesis. As such, if the p-value is sufficiently low, you can declare the results statistically significant and reject the null hypothesis. Recall that this threshold is defined as $\alpha$, and is also the rate of type I errors. In this lab, you'll investigate the robustness of p-values and their relation with effect-size and sample-size. 

# Import Starter Functions

To start, import the functions stored in the `flatiron_stats.py` file. It contains the stats functions that you previously coded in the last lab: `welch_t(a,b)`, `welch_df(a, b)` and `p_value(a, b, two_sided=False)`. You'll then use these functions below to further investigate the relationship between p-values and sample size.


```python
#Your code here; import the contents from flatiron_stats.py
#You may also wish to open up flatiron_stats.py in a text editor to preview its contents.
from flatiron_stats import *
```

## Generating Random Samples

Before you start running simulations, it will be useful to have a helper function that will generate random samples. Write such a function below which generates 2 random samples from 2 normal distributions. The function should take 6 input parameters:

* m1 - The underlying population mean for sample 1
* s1 - The underlying population standard deviation for sample 1
* n1 - The sample size for sample 1

* m2 - The underlying population mean for sample 2
* s2 - The underlying population standard deviation for sample 2
* n2 - The sample size for sample 2


```python
import numpy as np
def generate_samples(m1,s1,n1,m2,s2,n2):
    #Your code here
    sample1 = np.random.normal(loc=m1, scale=s1, size=n1)
    sample2 = np.random.normal(loc=m2, scale=s2, size=n2)
    return sample1, sample2
```

## Running a Simulation

For your first simulation, you're going to investigate how the p-value of an experiment relates to sample size when both samples are from identical underlying distributions. To do this, use your `generate_samples()` function along with the `p_value_welch_ttest()` function defined in the flatiron_stats file. Use sample sizes from 5 to 5000. For each sample size, simulate 10,000 experiments. For each of these experiments, generate 2 samples of the given sample size, each with a mean of 5 and a standard deviation of 1. Calculate the corresponding p-values for a Welch's t-test for each of these sample-pairs. Finally, store all 10,000 of these p-values along with their corresponding sample size. 


```python
import pandas as pd
```


```python
n_vs_p = {}
for n in range(5,5005,5):
    temp = []
    for i in range(10**4):
        sample1, sample2 = generate_samples(m1=5, s1=1, n1=n, m2=5, s2=1, n2=n)
        p = p_value_welch_ttest(sample1, sample2)
        temp.append(p)
    n_vs_p[n] = temp
df = pd.DataFrame.from_dict(n_vs_p) 
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>5</th>
      <th>10</th>
      <th>15</th>
      <th>20</th>
      <th>25</th>
      <th>30</th>
      <th>35</th>
      <th>40</th>
      <th>45</th>
      <th>50</th>
      <th>...</th>
      <th>4955</th>
      <th>4960</th>
      <th>4965</th>
      <th>4970</th>
      <th>4975</th>
      <th>4980</th>
      <th>4985</th>
      <th>4990</th>
      <th>4995</th>
      <th>5000</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.114858</td>
      <td>0.225780</td>
      <td>0.184541</td>
      <td>0.023004</td>
      <td>0.136535</td>
      <td>0.474732</td>
      <td>0.246251</td>
      <td>0.008014</td>
      <td>0.045742</td>
      <td>0.276221</td>
      <td>...</td>
      <td>0.320563</td>
      <td>0.093427</td>
      <td>0.359169</td>
      <td>0.057506</td>
      <td>0.295422</td>
      <td>0.419694</td>
      <td>0.312001</td>
      <td>0.161778</td>
      <td>0.146389</td>
      <td>0.243631</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.107518</td>
      <td>0.289804</td>
      <td>0.166036</td>
      <td>0.168630</td>
      <td>0.425122</td>
      <td>0.361368</td>
      <td>0.444858</td>
      <td>0.285630</td>
      <td>0.341772</td>
      <td>0.214734</td>
      <td>...</td>
      <td>0.069012</td>
      <td>0.183049</td>
      <td>0.324486</td>
      <td>0.289466</td>
      <td>0.432983</td>
      <td>0.145880</td>
      <td>0.171273</td>
      <td>0.231472</td>
      <td>0.089274</td>
      <td>0.034726</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.101484</td>
      <td>0.154497</td>
      <td>0.296421</td>
      <td>0.108977</td>
      <td>0.180490</td>
      <td>0.121559</td>
      <td>0.078134</td>
      <td>0.058508</td>
      <td>0.371425</td>
      <td>0.156752</td>
      <td>...</td>
      <td>0.223777</td>
      <td>0.342147</td>
      <td>0.287711</td>
      <td>0.392367</td>
      <td>0.008451</td>
      <td>0.391506</td>
      <td>0.234037</td>
      <td>0.100587</td>
      <td>0.182217</td>
      <td>0.458477</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.150230</td>
      <td>0.207533</td>
      <td>0.486753</td>
      <td>0.116937</td>
      <td>0.309638</td>
      <td>0.239450</td>
      <td>0.308371</td>
      <td>0.333574</td>
      <td>0.349605</td>
      <td>0.115435</td>
      <td>...</td>
      <td>0.193887</td>
      <td>0.164900</td>
      <td>0.255469</td>
      <td>0.294494</td>
      <td>0.151002</td>
      <td>0.148370</td>
      <td>0.261621</td>
      <td>0.215301</td>
      <td>0.468064</td>
      <td>0.388551</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.440121</td>
      <td>0.280969</td>
      <td>0.162513</td>
      <td>0.144875</td>
      <td>0.276414</td>
      <td>0.213804</td>
      <td>0.375926</td>
      <td>0.330105</td>
      <td>0.370874</td>
      <td>0.328820</td>
      <td>...</td>
      <td>0.115081</td>
      <td>0.074749</td>
      <td>0.294784</td>
      <td>0.123185</td>
      <td>0.368410</td>
      <td>0.494228</td>
      <td>0.099280</td>
      <td>0.497153</td>
      <td>0.301893</td>
      <td>0.388507</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.469730</td>
      <td>0.135150</td>
      <td>0.149539</td>
      <td>0.313661</td>
      <td>0.399098</td>
      <td>0.182035</td>
      <td>0.429303</td>
      <td>0.461018</td>
      <td>0.399955</td>
      <td>0.080824</td>
      <td>...</td>
      <td>0.396749</td>
      <td>0.167838</td>
      <td>0.421600</td>
      <td>0.217456</td>
      <td>0.367965</td>
      <td>0.329645</td>
      <td>0.309522</td>
      <td>0.082679</td>
      <td>0.010959</td>
      <td>0.496828</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.066777</td>
      <td>0.453294</td>
      <td>0.398427</td>
      <td>0.099709</td>
      <td>0.326128</td>
      <td>0.232553</td>
      <td>0.251274</td>
      <td>0.302913</td>
      <td>0.252934</td>
      <td>0.184063</td>
      <td>...</td>
      <td>0.067361</td>
      <td>0.099187</td>
      <td>0.248965</td>
      <td>0.360580</td>
      <td>0.267872</td>
      <td>0.136599</td>
      <td>0.187469</td>
      <td>0.467630</td>
      <td>0.442827</td>
      <td>0.270327</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.220418</td>
      <td>0.175790</td>
      <td>0.084153</td>
      <td>0.053748</td>
      <td>0.002785</td>
      <td>0.313984</td>
      <td>0.036666</td>
      <td>0.341114</td>
      <td>0.319663</td>
      <td>0.365298</td>
      <td>...</td>
      <td>0.117277</td>
      <td>0.365157</td>
      <td>0.088565</td>
      <td>0.011923</td>
      <td>0.290223</td>
      <td>0.114080</td>
      <td>0.382380</td>
      <td>0.490774</td>
      <td>0.426432</td>
      <td>0.377895</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.322582</td>
      <td>0.020165</td>
      <td>0.127246</td>
      <td>0.011151</td>
      <td>0.180162</td>
      <td>0.050520</td>
      <td>0.479191</td>
      <td>0.121253</td>
      <td>0.083359</td>
      <td>0.247707</td>
      <td>...</td>
      <td>0.093393</td>
      <td>0.024292</td>
      <td>0.271352</td>
      <td>0.045134</td>
      <td>0.192573</td>
      <td>0.085973</td>
      <td>0.482460</td>
      <td>0.366383</td>
      <td>0.269380</td>
      <td>0.488725</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.096211</td>
      <td>0.008377</td>
      <td>0.098663</td>
      <td>0.374167</td>
      <td>0.228272</td>
      <td>0.463920</td>
      <td>0.451042</td>
      <td>0.291125</td>
      <td>0.397267</td>
      <td>0.125357</td>
      <td>...</td>
      <td>0.353278</td>
      <td>0.181403</td>
      <td>0.366467</td>
      <td>0.394625</td>
      <td>0.222295</td>
      <td>0.393405</td>
      <td>0.027650</td>
      <td>0.267931</td>
      <td>0.115542</td>
      <td>0.497577</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.173390</td>
      <td>0.081111</td>
      <td>0.308331</td>
      <td>0.189311</td>
      <td>0.062138</td>
      <td>0.171704</td>
      <td>0.181996</td>
      <td>0.173733</td>
      <td>0.402688</td>
      <td>0.116832</td>
      <td>...</td>
      <td>0.490524</td>
      <td>0.042358</td>
      <td>0.213435</td>
      <td>0.359513</td>
      <td>0.257911</td>
      <td>0.181510</td>
      <td>0.332805</td>
      <td>0.186563</td>
      <td>0.458938</td>
      <td>0.157789</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.095957</td>
      <td>0.175838</td>
      <td>0.272199</td>
      <td>0.203713</td>
      <td>0.225770</td>
      <td>0.327386</td>
      <td>0.123449</td>
      <td>0.341170</td>
      <td>0.439166</td>
      <td>0.135180</td>
      <td>...</td>
      <td>0.483843</td>
      <td>0.079416</td>
      <td>0.249924</td>
      <td>0.405046</td>
      <td>0.171859</td>
      <td>0.493507</td>
      <td>0.288509</td>
      <td>0.403715</td>
      <td>0.077774</td>
      <td>0.318710</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.446420</td>
      <td>0.100984</td>
      <td>0.057295</td>
      <td>0.073321</td>
      <td>0.046772</td>
      <td>0.326636</td>
      <td>0.127909</td>
      <td>0.313433</td>
      <td>0.345526</td>
      <td>0.439955</td>
      <td>...</td>
      <td>0.141569</td>
      <td>0.007565</td>
      <td>0.438984</td>
      <td>0.088783</td>
      <td>0.023223</td>
      <td>0.267818</td>
      <td>0.086675</td>
      <td>0.001053</td>
      <td>0.301147</td>
      <td>0.082975</td>
    </tr>
    <tr>
      <th>13</th>
      <td>0.347055</td>
      <td>0.219059</td>
      <td>0.165646</td>
      <td>0.293715</td>
      <td>0.274022</td>
      <td>0.237832</td>
      <td>0.169376</td>
      <td>0.173357</td>
      <td>0.157696</td>
      <td>0.181621</td>
      <td>...</td>
      <td>0.045965</td>
      <td>0.087647</td>
      <td>0.064891</td>
      <td>0.325822</td>
      <td>0.280250</td>
      <td>0.191379</td>
      <td>0.078912</td>
      <td>0.259216</td>
      <td>0.024290</td>
      <td>0.135715</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0.400065</td>
      <td>0.366765</td>
      <td>0.379373</td>
      <td>0.220407</td>
      <td>0.078728</td>
      <td>0.007596</td>
      <td>0.149321</td>
      <td>0.443158</td>
      <td>0.101539</td>
      <td>0.490303</td>
      <td>...</td>
      <td>0.349164</td>
      <td>0.222257</td>
      <td>0.376252</td>
      <td>0.324297</td>
      <td>0.057179</td>
      <td>0.182031</td>
      <td>0.382273</td>
      <td>0.245954</td>
      <td>0.216999</td>
      <td>0.198136</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.402358</td>
      <td>0.167275</td>
      <td>0.065970</td>
      <td>0.153421</td>
      <td>0.281591</td>
      <td>0.483880</td>
      <td>0.471942</td>
      <td>0.230682</td>
      <td>0.088438</td>
      <td>0.392019</td>
      <td>...</td>
      <td>0.301670</td>
      <td>0.185681</td>
      <td>0.042195</td>
      <td>0.008105</td>
      <td>0.434191</td>
      <td>0.405225</td>
      <td>0.278360</td>
      <td>0.147777</td>
      <td>0.277254</td>
      <td>0.111873</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0.461541</td>
      <td>0.456212</td>
      <td>0.167693</td>
      <td>0.359306</td>
      <td>0.037956</td>
      <td>0.224919</td>
      <td>0.112298</td>
      <td>0.030708</td>
      <td>0.279974</td>
      <td>0.482689</td>
      <td>...</td>
      <td>0.198375</td>
      <td>0.362349</td>
      <td>0.442452</td>
      <td>0.122648</td>
      <td>0.270290</td>
      <td>0.303408</td>
      <td>0.082588</td>
      <td>0.157595</td>
      <td>0.492906</td>
      <td>0.096136</td>
    </tr>
    <tr>
      <th>17</th>
      <td>0.100123</td>
      <td>0.394723</td>
      <td>0.183735</td>
      <td>0.401194</td>
      <td>0.256124</td>
      <td>0.436056</td>
      <td>0.136146</td>
      <td>0.483834</td>
      <td>0.362703</td>
      <td>0.173519</td>
      <td>...</td>
      <td>0.174438</td>
      <td>0.482636</td>
      <td>0.004005</td>
      <td>0.497832</td>
      <td>0.472115</td>
      <td>0.340216</td>
      <td>0.175746</td>
      <td>0.387998</td>
      <td>0.236260</td>
      <td>0.198267</td>
    </tr>
    <tr>
      <th>18</th>
      <td>0.375695</td>
      <td>0.102156</td>
      <td>0.254315</td>
      <td>0.444419</td>
      <td>0.338202</td>
      <td>0.193703</td>
      <td>0.244664</td>
      <td>0.207302</td>
      <td>0.462349</td>
      <td>0.063194</td>
      <td>...</td>
      <td>0.277022</td>
      <td>0.315328</td>
      <td>0.356932</td>
      <td>0.290431</td>
      <td>0.036800</td>
      <td>0.395636</td>
      <td>0.347917</td>
      <td>0.124616</td>
      <td>0.243283</td>
      <td>0.463797</td>
    </tr>
    <tr>
      <th>19</th>
      <td>0.021973</td>
      <td>0.171665</td>
      <td>0.358030</td>
      <td>0.230028</td>
      <td>0.222996</td>
      <td>0.405406</td>
      <td>0.283871</td>
      <td>0.334117</td>
      <td>0.313589</td>
      <td>0.170684</td>
      <td>...</td>
      <td>0.134321</td>
      <td>0.015509</td>
      <td>0.274366</td>
      <td>0.284940</td>
      <td>0.061096</td>
      <td>0.431984</td>
      <td>0.117862</td>
      <td>0.029091</td>
      <td>0.059181</td>
      <td>0.005263</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.362812</td>
      <td>0.367847</td>
      <td>0.065734</td>
      <td>0.248488</td>
      <td>0.487747</td>
      <td>0.191273</td>
      <td>0.103744</td>
      <td>0.465940</td>
      <td>0.125964</td>
      <td>0.404875</td>
      <td>...</td>
      <td>0.349848</td>
      <td>0.132376</td>
      <td>0.175315</td>
      <td>0.021150</td>
      <td>0.360536</td>
      <td>0.379163</td>
      <td>0.476041</td>
      <td>0.175812</td>
      <td>0.089571</td>
      <td>0.058657</td>
    </tr>
    <tr>
      <th>21</th>
      <td>0.065773</td>
      <td>0.041402</td>
      <td>0.443875</td>
      <td>0.459972</td>
      <td>0.265310</td>
      <td>0.343614</td>
      <td>0.099022</td>
      <td>0.300096</td>
      <td>0.410990</td>
      <td>0.289101</td>
      <td>...</td>
      <td>0.119483</td>
      <td>0.087328</td>
      <td>0.056896</td>
      <td>0.382882</td>
      <td>0.381044</td>
      <td>0.149278</td>
      <td>0.420791</td>
      <td>0.431331</td>
      <td>0.000304</td>
      <td>0.090227</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.389000</td>
      <td>0.213872</td>
      <td>0.011858</td>
      <td>0.199264</td>
      <td>0.424970</td>
      <td>0.363937</td>
      <td>0.387684</td>
      <td>0.152882</td>
      <td>0.317260</td>
      <td>0.034410</td>
      <td>...</td>
      <td>0.401517</td>
      <td>0.338197</td>
      <td>0.488668</td>
      <td>0.066359</td>
      <td>0.168723</td>
      <td>0.492004</td>
      <td>0.384085</td>
      <td>0.221343</td>
      <td>0.214281</td>
      <td>0.370681</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0.152391</td>
      <td>0.034580</td>
      <td>0.287936</td>
      <td>0.235822</td>
      <td>0.392321</td>
      <td>0.372435</td>
      <td>0.302966</td>
      <td>0.263409</td>
      <td>0.385705</td>
      <td>0.191967</td>
      <td>...</td>
      <td>0.414392</td>
      <td>0.406956</td>
      <td>0.241404</td>
      <td>0.344597</td>
      <td>0.014448</td>
      <td>0.394439</td>
      <td>0.180444</td>
      <td>0.008280</td>
      <td>0.154871</td>
      <td>0.464906</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0.114388</td>
      <td>0.383958</td>
      <td>0.464528</td>
      <td>0.195476</td>
      <td>0.212356</td>
      <td>0.217077</td>
      <td>0.026005</td>
      <td>0.269259</td>
      <td>0.302763</td>
      <td>0.258516</td>
      <td>...</td>
      <td>0.337148</td>
      <td>0.409577</td>
      <td>0.423439</td>
      <td>0.438330</td>
      <td>0.324397</td>
      <td>0.405230</td>
      <td>0.009807</td>
      <td>0.426356</td>
      <td>0.385410</td>
      <td>0.255353</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.192621</td>
      <td>0.394277</td>
      <td>0.113474</td>
      <td>0.385585</td>
      <td>0.184270</td>
      <td>0.357962</td>
      <td>0.162295</td>
      <td>0.240698</td>
      <td>0.377824</td>
      <td>0.329874</td>
      <td>...</td>
      <td>0.302505</td>
      <td>0.413257</td>
      <td>0.159053</td>
      <td>0.432838</td>
      <td>0.124259</td>
      <td>0.001669</td>
      <td>0.280146</td>
      <td>0.054214</td>
      <td>0.305896</td>
      <td>0.385738</td>
    </tr>
    <tr>
      <th>26</th>
      <td>0.253945</td>
      <td>0.266481</td>
      <td>0.166615</td>
      <td>0.210071</td>
      <td>0.063017</td>
      <td>0.091559</td>
      <td>0.489944</td>
      <td>0.283884</td>
      <td>0.439629</td>
      <td>0.219596</td>
      <td>...</td>
      <td>0.322612</td>
      <td>0.125837</td>
      <td>0.060877</td>
      <td>0.255234</td>
      <td>0.466884</td>
      <td>0.442057</td>
      <td>0.345347</td>
      <td>0.001604</td>
      <td>0.124514</td>
      <td>0.116912</td>
    </tr>
    <tr>
      <th>27</th>
      <td>0.067262</td>
      <td>0.361673</td>
      <td>0.339834</td>
      <td>0.484065</td>
      <td>0.268390</td>
      <td>0.019164</td>
      <td>0.476575</td>
      <td>0.228747</td>
      <td>0.094414</td>
      <td>0.475192</td>
      <td>...</td>
      <td>0.283164</td>
      <td>0.308056</td>
      <td>0.436450</td>
      <td>0.418206</td>
      <td>0.336869</td>
      <td>0.303914</td>
      <td>0.460402</td>
      <td>0.346550</td>
      <td>0.049530</td>
      <td>0.482959</td>
    </tr>
    <tr>
      <th>28</th>
      <td>0.088485</td>
      <td>0.285565</td>
      <td>0.375786</td>
      <td>0.017040</td>
      <td>0.351521</td>
      <td>0.411464</td>
      <td>0.104834</td>
      <td>0.496914</td>
      <td>0.216813</td>
      <td>0.414782</td>
      <td>...</td>
      <td>0.189884</td>
      <td>0.394636</td>
      <td>0.379351</td>
      <td>0.039660</td>
      <td>0.491557</td>
      <td>0.462713</td>
      <td>0.357981</td>
      <td>0.420502</td>
      <td>0.089534</td>
      <td>0.204605</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.046500</td>
      <td>0.297794</td>
      <td>0.162158</td>
      <td>0.337316</td>
      <td>0.045865</td>
      <td>0.334208</td>
      <td>0.254868</td>
      <td>0.078950</td>
      <td>0.307363</td>
      <td>0.140896</td>
      <td>...</td>
      <td>0.493319</td>
      <td>0.208193</td>
      <td>0.144256</td>
      <td>0.215770</td>
      <td>0.043670</td>
      <td>0.402825</td>
      <td>0.357016</td>
      <td>0.293870</td>
      <td>0.470956</td>
      <td>0.113387</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9970</th>
      <td>0.421777</td>
      <td>0.197788</td>
      <td>0.342480</td>
      <td>0.190154</td>
      <td>0.419057</td>
      <td>0.079552</td>
      <td>0.105877</td>
      <td>0.267792</td>
      <td>0.429790</td>
      <td>0.228643</td>
      <td>...</td>
      <td>0.150307</td>
      <td>0.025608</td>
      <td>0.475648</td>
      <td>0.366040</td>
      <td>0.475914</td>
      <td>0.368476</td>
      <td>0.320794</td>
      <td>0.160045</td>
      <td>0.356061</td>
      <td>0.227253</td>
    </tr>
    <tr>
      <th>9971</th>
      <td>0.194083</td>
      <td>0.386469</td>
      <td>0.124656</td>
      <td>0.393002</td>
      <td>0.031543</td>
      <td>0.241263</td>
      <td>0.435171</td>
      <td>0.170421</td>
      <td>0.007996</td>
      <td>0.146411</td>
      <td>...</td>
      <td>0.320249</td>
      <td>0.238016</td>
      <td>0.234846</td>
      <td>0.339436</td>
      <td>0.307147</td>
      <td>0.396868</td>
      <td>0.244181</td>
      <td>0.460319</td>
      <td>0.044039</td>
      <td>0.438333</td>
    </tr>
    <tr>
      <th>9972</th>
      <td>0.122401</td>
      <td>0.498527</td>
      <td>0.097859</td>
      <td>0.187011</td>
      <td>0.491156</td>
      <td>0.286688</td>
      <td>0.105165</td>
      <td>0.035895</td>
      <td>0.315769</td>
      <td>0.481132</td>
      <td>...</td>
      <td>0.020895</td>
      <td>0.210227</td>
      <td>0.431365</td>
      <td>0.192556</td>
      <td>0.177146</td>
      <td>0.382448</td>
      <td>0.022529</td>
      <td>0.321375</td>
      <td>0.474372</td>
      <td>0.415878</td>
    </tr>
    <tr>
      <th>9973</th>
      <td>0.125220</td>
      <td>0.046373</td>
      <td>0.469612</td>
      <td>0.233500</td>
      <td>0.471260</td>
      <td>0.488931</td>
      <td>0.436293</td>
      <td>0.120631</td>
      <td>0.024468</td>
      <td>0.208650</td>
      <td>...</td>
      <td>0.236756</td>
      <td>0.448358</td>
      <td>0.314666</td>
      <td>0.123122</td>
      <td>0.248342</td>
      <td>0.426594</td>
      <td>0.075141</td>
      <td>0.189125</td>
      <td>0.144008</td>
      <td>0.300202</td>
    </tr>
    <tr>
      <th>9974</th>
      <td>0.479035</td>
      <td>0.396865</td>
      <td>0.471994</td>
      <td>0.342487</td>
      <td>0.238557</td>
      <td>0.174738</td>
      <td>0.496929</td>
      <td>0.327788</td>
      <td>0.181234</td>
      <td>0.358178</td>
      <td>...</td>
      <td>0.279331</td>
      <td>0.173524</td>
      <td>0.373169</td>
      <td>0.057867</td>
      <td>0.012612</td>
      <td>0.466653</td>
      <td>0.187223</td>
      <td>0.498742</td>
      <td>0.483324</td>
      <td>0.001391</td>
    </tr>
    <tr>
      <th>9975</th>
      <td>0.448164</td>
      <td>0.039431</td>
      <td>0.385362</td>
      <td>0.420627</td>
      <td>0.297886</td>
      <td>0.178272</td>
      <td>0.424014</td>
      <td>0.123949</td>
      <td>0.230289</td>
      <td>0.051300</td>
      <td>...</td>
      <td>0.051572</td>
      <td>0.209664</td>
      <td>0.243451</td>
      <td>0.108758</td>
      <td>0.426466</td>
      <td>0.484438</td>
      <td>0.287424</td>
      <td>0.212167</td>
      <td>0.154904</td>
      <td>0.012431</td>
    </tr>
    <tr>
      <th>9976</th>
      <td>0.276802</td>
      <td>0.021515</td>
      <td>0.108223</td>
      <td>0.093967</td>
      <td>0.085017</td>
      <td>0.324663</td>
      <td>0.239167</td>
      <td>0.214243</td>
      <td>0.267051</td>
      <td>0.161560</td>
      <td>...</td>
      <td>0.398124</td>
      <td>0.315688</td>
      <td>0.087453</td>
      <td>0.194246</td>
      <td>0.349099</td>
      <td>0.283374</td>
      <td>0.150703</td>
      <td>0.307835</td>
      <td>0.029627</td>
      <td>0.376910</td>
    </tr>
    <tr>
      <th>9977</th>
      <td>0.380414</td>
      <td>0.107782</td>
      <td>0.126926</td>
      <td>0.224488</td>
      <td>0.316124</td>
      <td>0.473790</td>
      <td>0.114210</td>
      <td>0.173588</td>
      <td>0.374195</td>
      <td>0.174987</td>
      <td>...</td>
      <td>0.479186</td>
      <td>0.183820</td>
      <td>0.252347</td>
      <td>0.272420</td>
      <td>0.124066</td>
      <td>0.026630</td>
      <td>0.145043</td>
      <td>0.228385</td>
      <td>0.118422</td>
      <td>0.337801</td>
    </tr>
    <tr>
      <th>9978</th>
      <td>0.020373</td>
      <td>0.080142</td>
      <td>0.159890</td>
      <td>0.237249</td>
      <td>0.303855</td>
      <td>0.312073</td>
      <td>0.131563</td>
      <td>0.455866</td>
      <td>0.049948</td>
      <td>0.372795</td>
      <td>...</td>
      <td>0.026782</td>
      <td>0.377678</td>
      <td>0.087261</td>
      <td>0.078150</td>
      <td>0.242953</td>
      <td>0.105519</td>
      <td>0.246612</td>
      <td>0.335041</td>
      <td>0.084376</td>
      <td>0.097436</td>
    </tr>
    <tr>
      <th>9979</th>
      <td>0.496838</td>
      <td>0.088771</td>
      <td>0.124215</td>
      <td>0.084156</td>
      <td>0.201238</td>
      <td>0.432589</td>
      <td>0.465185</td>
      <td>0.140485</td>
      <td>0.210564</td>
      <td>0.089429</td>
      <td>...</td>
      <td>0.260341</td>
      <td>0.302665</td>
      <td>0.042538</td>
      <td>0.273971</td>
      <td>0.442811</td>
      <td>0.497866</td>
      <td>0.144640</td>
      <td>0.353843</td>
      <td>0.028688</td>
      <td>0.424673</td>
    </tr>
    <tr>
      <th>9980</th>
      <td>0.260819</td>
      <td>0.097664</td>
      <td>0.358567</td>
      <td>0.219893</td>
      <td>0.417853</td>
      <td>0.023272</td>
      <td>0.189344</td>
      <td>0.359629</td>
      <td>0.089450</td>
      <td>0.001011</td>
      <td>...</td>
      <td>0.411112</td>
      <td>0.094030</td>
      <td>0.223648</td>
      <td>0.395327</td>
      <td>0.351523</td>
      <td>0.363728</td>
      <td>0.180410</td>
      <td>0.186471</td>
      <td>0.088211</td>
      <td>0.141038</td>
    </tr>
    <tr>
      <th>9981</th>
      <td>0.113869</td>
      <td>0.479190</td>
      <td>0.241230</td>
      <td>0.445938</td>
      <td>0.149355</td>
      <td>0.015573</td>
      <td>0.490403</td>
      <td>0.102382</td>
      <td>0.378935</td>
      <td>0.013436</td>
      <td>...</td>
      <td>0.293594</td>
      <td>0.274015</td>
      <td>0.197209</td>
      <td>0.050314</td>
      <td>0.410300</td>
      <td>0.221550</td>
      <td>0.387925</td>
      <td>0.433204</td>
      <td>0.143887</td>
      <td>0.123024</td>
    </tr>
    <tr>
      <th>9982</th>
      <td>0.173693</td>
      <td>0.080731</td>
      <td>0.032250</td>
      <td>0.258666</td>
      <td>0.278157</td>
      <td>0.423108</td>
      <td>0.123208</td>
      <td>0.326117</td>
      <td>0.238312</td>
      <td>0.107292</td>
      <td>...</td>
      <td>0.250731</td>
      <td>0.103941</td>
      <td>0.222765</td>
      <td>0.326553</td>
      <td>0.081726</td>
      <td>0.238343</td>
      <td>0.035721</td>
      <td>0.441485</td>
      <td>0.078523</td>
      <td>0.245655</td>
    </tr>
    <tr>
      <th>9983</th>
      <td>0.342764</td>
      <td>0.498982</td>
      <td>0.026614</td>
      <td>0.097025</td>
      <td>0.031486</td>
      <td>0.413697</td>
      <td>0.318141</td>
      <td>0.150750</td>
      <td>0.107830</td>
      <td>0.430179</td>
      <td>...</td>
      <td>0.353522</td>
      <td>0.299122</td>
      <td>0.276907</td>
      <td>0.390874</td>
      <td>0.488740</td>
      <td>0.194855</td>
      <td>0.266092</td>
      <td>0.181714</td>
      <td>0.431401</td>
      <td>0.101759</td>
    </tr>
    <tr>
      <th>9984</th>
      <td>0.490368</td>
      <td>0.301021</td>
      <td>0.007331</td>
      <td>0.327328</td>
      <td>0.112748</td>
      <td>0.285654</td>
      <td>0.218703</td>
      <td>0.395095</td>
      <td>0.328898</td>
      <td>0.135221</td>
      <td>...</td>
      <td>0.255161</td>
      <td>0.396824</td>
      <td>0.394282</td>
      <td>0.092282</td>
      <td>0.108201</td>
      <td>0.068116</td>
      <td>0.310281</td>
      <td>0.354020</td>
      <td>0.383353</td>
      <td>0.216653</td>
    </tr>
    <tr>
      <th>9985</th>
      <td>0.083413</td>
      <td>0.284136</td>
      <td>0.276989</td>
      <td>0.104382</td>
      <td>0.006351</td>
      <td>0.348365</td>
      <td>0.165746</td>
      <td>0.307051</td>
      <td>0.262000</td>
      <td>0.410970</td>
      <td>...</td>
      <td>0.059289</td>
      <td>0.361621</td>
      <td>0.352426</td>
      <td>0.001870</td>
      <td>0.120462</td>
      <td>0.436615</td>
      <td>0.011705</td>
      <td>0.191411</td>
      <td>0.349573</td>
      <td>0.434136</td>
    </tr>
    <tr>
      <th>9986</th>
      <td>0.474007</td>
      <td>0.289810</td>
      <td>0.126150</td>
      <td>0.215040</td>
      <td>0.060790</td>
      <td>0.202084</td>
      <td>0.430676</td>
      <td>0.412997</td>
      <td>0.029689</td>
      <td>0.105724</td>
      <td>...</td>
      <td>0.105612</td>
      <td>0.151222</td>
      <td>0.387238</td>
      <td>0.324506</td>
      <td>0.493294</td>
      <td>0.061298</td>
      <td>0.225786</td>
      <td>0.030533</td>
      <td>0.381029</td>
      <td>0.069896</td>
    </tr>
    <tr>
      <th>9987</th>
      <td>0.309029</td>
      <td>0.471691</td>
      <td>0.382329</td>
      <td>0.333671</td>
      <td>0.096527</td>
      <td>0.112359</td>
      <td>0.345892</td>
      <td>0.412329</td>
      <td>0.090464</td>
      <td>0.042739</td>
      <td>...</td>
      <td>0.017616</td>
      <td>0.377720</td>
      <td>0.296843</td>
      <td>0.475707</td>
      <td>0.495593</td>
      <td>0.350906</td>
      <td>0.155304</td>
      <td>0.011144</td>
      <td>0.079982</td>
      <td>0.139057</td>
    </tr>
    <tr>
      <th>9988</th>
      <td>0.320448</td>
      <td>0.381645</td>
      <td>0.138976</td>
      <td>0.245411</td>
      <td>0.180654</td>
      <td>0.334628</td>
      <td>0.211913</td>
      <td>0.158320</td>
      <td>0.224862</td>
      <td>0.294847</td>
      <td>...</td>
      <td>0.400185</td>
      <td>0.394456</td>
      <td>0.165990</td>
      <td>0.192242</td>
      <td>0.437272</td>
      <td>0.280530</td>
      <td>0.298746</td>
      <td>0.443987</td>
      <td>0.329689</td>
      <td>0.048823</td>
    </tr>
    <tr>
      <th>9989</th>
      <td>0.232781</td>
      <td>0.208661</td>
      <td>0.479462</td>
      <td>0.308589</td>
      <td>0.375247</td>
      <td>0.181381</td>
      <td>0.370008</td>
      <td>0.437716</td>
      <td>0.017738</td>
      <td>0.009142</td>
      <td>...</td>
      <td>0.333545</td>
      <td>0.288636</td>
      <td>0.325717</td>
      <td>0.477608</td>
      <td>0.284585</td>
      <td>0.080885</td>
      <td>0.264828</td>
      <td>0.465079</td>
      <td>0.416041</td>
      <td>0.338426</td>
    </tr>
    <tr>
      <th>9990</th>
      <td>0.081908</td>
      <td>0.494099</td>
      <td>0.069223</td>
      <td>0.442342</td>
      <td>0.047608</td>
      <td>0.106896</td>
      <td>0.048109</td>
      <td>0.206825</td>
      <td>0.375535</td>
      <td>0.250625</td>
      <td>...</td>
      <td>0.219865</td>
      <td>0.127863</td>
      <td>0.308730</td>
      <td>0.376680</td>
      <td>0.381468</td>
      <td>0.430515</td>
      <td>0.410196</td>
      <td>0.355801</td>
      <td>0.197529</td>
      <td>0.246231</td>
    </tr>
    <tr>
      <th>9991</th>
      <td>0.187691</td>
      <td>0.484102</td>
      <td>0.132478</td>
      <td>0.369238</td>
      <td>0.273251</td>
      <td>0.472948</td>
      <td>0.013074</td>
      <td>0.375856</td>
      <td>0.423856</td>
      <td>0.470292</td>
      <td>...</td>
      <td>0.076738</td>
      <td>0.278236</td>
      <td>0.463720</td>
      <td>0.438520</td>
      <td>0.125812</td>
      <td>0.414538</td>
      <td>0.487643</td>
      <td>0.041705</td>
      <td>0.042398</td>
      <td>0.212985</td>
    </tr>
    <tr>
      <th>9992</th>
      <td>0.233611</td>
      <td>0.242455</td>
      <td>0.181436</td>
      <td>0.388481</td>
      <td>0.330482</td>
      <td>0.392666</td>
      <td>0.402934</td>
      <td>0.104291</td>
      <td>0.123549</td>
      <td>0.475594</td>
      <td>...</td>
      <td>0.057227</td>
      <td>0.374369</td>
      <td>0.254078</td>
      <td>0.112661</td>
      <td>0.286919</td>
      <td>0.244806</td>
      <td>0.491183</td>
      <td>0.181350</td>
      <td>0.195787</td>
      <td>0.147899</td>
    </tr>
    <tr>
      <th>9993</th>
      <td>0.347327</td>
      <td>0.266120</td>
      <td>0.148945</td>
      <td>0.137808</td>
      <td>0.140156</td>
      <td>0.381718</td>
      <td>0.366752</td>
      <td>0.466680</td>
      <td>0.372542</td>
      <td>0.329933</td>
      <td>...</td>
      <td>0.186157</td>
      <td>0.055750</td>
      <td>0.488074</td>
      <td>0.078476</td>
      <td>0.233228</td>
      <td>0.263700</td>
      <td>0.090543</td>
      <td>0.427950</td>
      <td>0.293155</td>
      <td>0.459914</td>
    </tr>
    <tr>
      <th>9994</th>
      <td>0.111958</td>
      <td>0.214487</td>
      <td>0.499568</td>
      <td>0.422591</td>
      <td>0.443897</td>
      <td>0.411890</td>
      <td>0.298276</td>
      <td>0.040425</td>
      <td>0.481301</td>
      <td>0.049017</td>
      <td>...</td>
      <td>0.498052</td>
      <td>0.152652</td>
      <td>0.315106</td>
      <td>0.432988</td>
      <td>0.147282</td>
      <td>0.366858</td>
      <td>0.319092</td>
      <td>0.035655</td>
      <td>0.424663</td>
      <td>0.264431</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>0.205040</td>
      <td>0.423736</td>
      <td>0.081348</td>
      <td>0.232906</td>
      <td>0.202543</td>
      <td>0.326670</td>
      <td>0.013602</td>
      <td>0.479436</td>
      <td>0.275898</td>
      <td>0.138233</td>
      <td>...</td>
      <td>0.393088</td>
      <td>0.064353</td>
      <td>0.174400</td>
      <td>0.064240</td>
      <td>0.072438</td>
      <td>0.172798</td>
      <td>0.105130</td>
      <td>0.265267</td>
      <td>0.441779</td>
      <td>0.227116</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>0.203148</td>
      <td>0.157499</td>
      <td>0.124443</td>
      <td>0.436957</td>
      <td>0.260118</td>
      <td>0.181161</td>
      <td>0.025483</td>
      <td>0.252997</td>
      <td>0.440109</td>
      <td>0.442007</td>
      <td>...</td>
      <td>0.415278</td>
      <td>0.367850</td>
      <td>0.094299</td>
      <td>0.040705</td>
      <td>0.325592</td>
      <td>0.236256</td>
      <td>0.467281</td>
      <td>0.094071</td>
      <td>0.155048</td>
      <td>0.055361</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>0.447512</td>
      <td>0.009580</td>
      <td>0.399273</td>
      <td>0.045666</td>
      <td>0.455055</td>
      <td>0.017429</td>
      <td>0.412421</td>
      <td>0.455696</td>
      <td>0.446386</td>
      <td>0.159728</td>
      <td>...</td>
      <td>0.120003</td>
      <td>0.346098</td>
      <td>0.245088</td>
      <td>0.126565</td>
      <td>0.132780</td>
      <td>0.183866</td>
      <td>0.281442</td>
      <td>0.434186</td>
      <td>0.374102</td>
      <td>0.194862</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>0.332412</td>
      <td>0.033429</td>
      <td>0.230259</td>
      <td>0.380048</td>
      <td>0.021626</td>
      <td>0.404370</td>
      <td>0.273285</td>
      <td>0.230652</td>
      <td>0.311733</td>
      <td>0.281851</td>
      <td>...</td>
      <td>0.285046</td>
      <td>0.196618</td>
      <td>0.151564</td>
      <td>0.467636</td>
      <td>0.104867</td>
      <td>0.060493</td>
      <td>0.495861</td>
      <td>0.133626</td>
      <td>0.036835</td>
      <td>0.093109</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>0.084849</td>
      <td>0.211830</td>
      <td>0.038264</td>
      <td>0.352653</td>
      <td>0.083030</td>
      <td>0.128151</td>
      <td>0.329528</td>
      <td>0.178403</td>
      <td>0.426508</td>
      <td>0.033863</td>
      <td>...</td>
      <td>0.041084</td>
      <td>0.091983</td>
      <td>0.248192</td>
      <td>0.424153</td>
      <td>0.079861</td>
      <td>0.421661</td>
      <td>0.230583</td>
      <td>0.103274</td>
      <td>0.332849</td>
      <td>0.232703</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 1000 columns</p>
</div>




```python
df.to_csv('p-value-simulations-for-identical-normal-distributions-and-varying-sample-size.csv', index=False)
```


```python
%matplotlib inline
```


```python
df.mean().plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1ce0cda0>




![png](index_files/index_11_1.png)



```python
p_vs_e_n = {}
for effect_size in [0.01,.1,.2, .5, 1, 2]:
    n_vs_p = {}
    for n in range(5,750,5):
        p_vals = []
        for i in range(100):
            sample1, sample2 = generate_samples(m1=5, s1=1, n1=n, m2=5+effect_size, s2=1, n2=n)
            p = p_value_welch_ttest(sample1, sample2)
            p_vals.append(p)
        n_vs_p[n] = np.mean(p_vals)
    p_vs_e_n[effect_size] = n_vs_p
```


```python
df = pd.DataFrame.from_dict(p_vs_e_n)
df.plot()
plt.title('Power vs Sample Size for Varying Effect sizes')
plt.xlabel('Sample Size')
ply.ylabel('Power')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a173c04a8>




![png](index_files/index_13_1.png)



```python
df.head(150)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0.2</th>
      <th>0.5</th>
      <th>1.0</th>
      <th>2.0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>0.242891</td>
      <td>2.232280e-01</td>
      <td>1.272707e-01</td>
      <td>2.561347e-02</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.255934</td>
      <td>1.916927e-01</td>
      <td>6.961595e-02</td>
      <td>2.050308e-03</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.237548</td>
      <td>1.782123e-01</td>
      <td>1.966554e-02</td>
      <td>1.110364e-04</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.206330</td>
      <td>1.282212e-01</td>
      <td>1.268444e-02</td>
      <td>8.968534e-06</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.225637</td>
      <td>9.990318e-02</td>
      <td>9.214690e-03</td>
      <td>2.308070e-06</td>
    </tr>
    <tr>
      <th>30</th>
      <td>0.192584</td>
      <td>6.582977e-02</td>
      <td>2.691906e-03</td>
      <td>1.788669e-08</td>
    </tr>
    <tr>
      <th>35</th>
      <td>0.178504</td>
      <td>5.757936e-02</td>
      <td>2.803232e-03</td>
      <td>1.279791e-08</td>
    </tr>
    <tr>
      <th>40</th>
      <td>0.188674</td>
      <td>7.779345e-02</td>
      <td>4.303528e-04</td>
      <td>1.030092e-10</td>
    </tr>
    <tr>
      <th>45</th>
      <td>0.161429</td>
      <td>4.261321e-02</td>
      <td>2.845029e-04</td>
      <td>3.876338e-12</td>
    </tr>
    <tr>
      <th>50</th>
      <td>0.194561</td>
      <td>3.531266e-02</td>
      <td>2.382991e-04</td>
      <td>4.241405e-12</td>
    </tr>
    <tr>
      <th>55</th>
      <td>0.171894</td>
      <td>1.990790e-02</td>
      <td>3.926199e-05</td>
      <td>1.181277e-15</td>
    </tr>
    <tr>
      <th>60</th>
      <td>0.170686</td>
      <td>2.370354e-02</td>
      <td>3.563495e-05</td>
      <td>6.278833e-13</td>
    </tr>
    <tr>
      <th>65</th>
      <td>0.186623</td>
      <td>3.051535e-02</td>
      <td>3.123601e-05</td>
      <td>3.774758e-17</td>
    </tr>
    <tr>
      <th>70</th>
      <td>0.142966</td>
      <td>2.558571e-02</td>
      <td>1.750537e-06</td>
      <td>5.551115e-18</td>
    </tr>
    <tr>
      <th>75</th>
      <td>0.186482</td>
      <td>1.365226e-02</td>
      <td>1.710313e-06</td>
      <td>3.330669e-18</td>
    </tr>
    <tr>
      <th>80</th>
      <td>0.141668</td>
      <td>1.920600e-02</td>
      <td>1.002841e-06</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>85</th>
      <td>0.134315</td>
      <td>9.179545e-03</td>
      <td>5.551854e-07</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>90</th>
      <td>0.127883</td>
      <td>1.397953e-02</td>
      <td>1.025645e-06</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>95</th>
      <td>0.142164</td>
      <td>6.485885e-03</td>
      <td>1.176361e-06</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>100</th>
      <td>0.122976</td>
      <td>1.492279e-02</td>
      <td>2.349519e-08</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>105</th>
      <td>0.125400</td>
      <td>4.927529e-03</td>
      <td>4.304047e-08</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>110</th>
      <td>0.130842</td>
      <td>3.887551e-03</td>
      <td>7.910607e-09</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>115</th>
      <td>0.114406</td>
      <td>4.587942e-03</td>
      <td>1.351068e-05</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>120</th>
      <td>0.120477</td>
      <td>3.774161e-03</td>
      <td>3.898085e-09</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>125</th>
      <td>0.123318</td>
      <td>2.076810e-03</td>
      <td>9.872251e-11</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>130</th>
      <td>0.135841</td>
      <td>1.555596e-03</td>
      <td>9.219681e-10</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>135</th>
      <td>0.100706</td>
      <td>2.702441e-03</td>
      <td>1.173044e-09</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>140</th>
      <td>0.103652</td>
      <td>8.021036e-04</td>
      <td>3.147598e-10</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>145</th>
      <td>0.110782</td>
      <td>1.760959e-03</td>
      <td>1.426623e-11</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>150</th>
      <td>0.096373</td>
      <td>6.071000e-04</td>
      <td>3.102007e-11</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>605</th>
      <td>0.008223</td>
      <td>2.306720e-11</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>610</th>
      <td>0.003126</td>
      <td>5.319345e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>615</th>
      <td>0.008000</td>
      <td>1.140420e-12</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>620</th>
      <td>0.005746</td>
      <td>1.539300e-10</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>625</th>
      <td>0.005313</td>
      <td>3.162248e-14</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>630</th>
      <td>0.006789</td>
      <td>2.435674e-12</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>635</th>
      <td>0.001136</td>
      <td>2.526590e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>640</th>
      <td>0.005208</td>
      <td>6.487144e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>645</th>
      <td>0.002022</td>
      <td>6.122647e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>650</th>
      <td>0.003225</td>
      <td>3.092306e-12</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>655</th>
      <td>0.004371</td>
      <td>4.657541e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>660</th>
      <td>0.002926</td>
      <td>1.448851e-12</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>665</th>
      <td>0.004179</td>
      <td>1.194649e-09</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>670</th>
      <td>0.007469</td>
      <td>1.423717e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>675</th>
      <td>0.004178</td>
      <td>1.210769e-12</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>680</th>
      <td>0.003379</td>
      <td>1.334044e-14</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>685</th>
      <td>0.007098</td>
      <td>3.376333e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>690</th>
      <td>0.004039</td>
      <td>5.624834e-14</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>695</th>
      <td>0.002819</td>
      <td>1.239442e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>700</th>
      <td>0.001305</td>
      <td>1.764699e-14</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>705</th>
      <td>0.003401</td>
      <td>1.052158e-14</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>710</th>
      <td>0.001175</td>
      <td>2.486892e-12</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>715</th>
      <td>0.003908</td>
      <td>4.993783e-15</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>720</th>
      <td>0.007167</td>
      <td>1.801892e-15</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>725</th>
      <td>0.002230</td>
      <td>7.498446e-15</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>730</th>
      <td>0.004353</td>
      <td>6.111778e-15</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>735</th>
      <td>0.005615</td>
      <td>6.896705e-15</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>740</th>
      <td>0.009238</td>
      <td>2.056011e-13</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>745</th>
      <td>0.001927</td>
      <td>5.084821e-15</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>750</th>
      <td>0.004880</td>
      <td>6.870948e-14</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 4 columns</p>
</div>




```python
df.to_csv('p_value_vs_effect_size.csv')
```

### P_Value and Sample Size

Let's now explore how the p-value depeands on sample size and effect size. AWe will take the effect size as the difference in means between two samples from normal distributions with variances of one. 

Let's write a function to run an experiment with N (sample size), effect size (difference in means) and return the p_value using functions created earlier. 


```python
def p_experiment(N, effect=1):
    
    control = np.random.randn(N)
    treatment = np.random.randn(N) + effect # Add effect to treatment group
    
    t, p = p_value(control, treatment)

    return p

```

Using the given values of M and N below, run the above function for effect sizes [0.2, 0.5, 1]. Store the values in an array using formula 

`ps = np.array([sum(simulate_experiment(N, effect_size) for m in range(M))/M for N in Ns])`


```python
nobs = np.linspace(2,300).astype(int) # A range of sample sizes to iterate over
M = 1000 # Number of simulations; run 1000 simulations for each sample size/effect size combination in order to converge on an average p-value

for e_size in [0.2,0.5,1]:
    
    p1 = np.array([sum(p_experiment(N, 0.2) for m in range(M))/M for N in Ns])
    p2 = np.array([sum(p_experiment(N, 0.5) for m in range(M))/M for N in Ns])
    p3 = np.array([sum(p_experiment(N, 1) for m in range(M))/M for N in Ns])
```

For each chosen effect size i.e. .2, .5 and 1, show the effect of sample size on averaged p_value calculated above. An example plot may look like:
![](images/p-sample-eff.png)


```python
# Plot the graph similar to one shown above
plt.plot(Ns, p1, label="Effect = 0.2")
plt.plot(Ns, p2, label="Effect = 0.5")
plt.plot(Ns, p3, label="Effect = 1")
plt.hlines(0.05, 0, 300, linestyles='--', color='k')
plt.ylabel("Average p-value")
plt.xlabel("Sample size")
plt.legend()

```




    <matplotlib.legend.Legend at 0x1a148cdb70>




![png](index_files/index_21_1.png)


What we see here is that the p-value is a function of the sample size. This means that regardless of effect size, if you have a large amount of data, you will get a significant p-value. It also means that if you don't have a significant p-value, an effect isn't rejected, you just can't see it through the noise.

### P_Value and Effect Size

We shall now look at how the p-values depend on effect size. We shall simulate experiments to see the distribution of p-values we get with changing effect sizes (as compared to fixed effect size previously).

Use the effect sizes [0.1, 0.25, 0.5, 0.75] with a sample size N = 100 and number of simulations -  M = 10000. Plot a hoistogram of p_values calculated for each effect size. The output may look similar to:
![](images/p_eff.png)



```python
fig, axes = plt.subplots(figsize=(12,3), ncols=4, sharey=True)
effect_sizes = [0.1, 0.25, 0.5, 0.75]

effects = [[p_experiment(100, effect=e) for m in range(10000)] for e in effect_sizes]

for i, ps in enumerate(effects):
    ax = axes[i]
    ax.hist(ps, range=(0, 0.5), bins=40, normed=True, alpha=0.7)
    ax.vlines(0.05, 0, 100, color='k', linestyles='--')
    ax.set_title('Effect = {}'.format(effect_sizes[i]))
    ax.set_xlabel('p')
    ax.set_ylim(0, 10)
```


![png](index_files/index_24_0.png)


It can be seen from the second set of simulations with an effect of 0.25, that If this same experiment were replicated in multiple labs in multiple locations, the chance that one particular experiment would find a statistically significant effect is about the same as getting heads from a coin flip. 


```python
for i, each in enumerate(effects):
    print('Effect = {}, P(p < 0.05): {}'.\
          format(effect_sizes[i], (np.array(each) < 0.05).mean()))

```

    Effect = 0.1, P(p < 0.05): 0.1114
    Effect = 0.25, P(p < 0.05): 0.4225
    Effect = 0.5, P(p < 0.05): 0.9396
    Effect = 0.75, P(p < 0.05): 0.9995


## Summary

This lesson summarizes and further builds upon the ideas that we saw in the previous labs. We learnt how p_value can be described as a function of effect size and for a given effect size, the p_value may get lower if we increase the sample size considerably. We also saw how p_value alone can not be used in order to identify some results as truly siginifcant, as this can be achieved when there is not a significant effect size. 
