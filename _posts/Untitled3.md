
# Percentage of Profitable Public Companies

financialmodelingprep.com has an API that allows users to query the financial data of a little over 5000 publicly traded companies. I wrote a script to pull the net income for 2018 of all the companies they provide data for. The data 1781 out of 5060 companies I pulled data for had negative net income.  

### Importing Libraries


```python
from bs4 import BeautifulSoup as bs
import json
import requests
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
%matplotlib inline
```

### The url below lists all the companies the Financial Modeling Prep has data for


```python
list_url = "https://financialmodelingprep.com/api/v3/company/stock/list"
```


```python
list_request = requests.get(list_url)
list_html = bs(list_request.text, 'html.parser')
list_data = json.loads(list_html.text)
```


```python
financial_url = "https://financialmodelingprep.com/api/v3/financials/income-statement/"
```

### The code snippet below creates a data frame with 3 columns (ticker symbol, net income, and name of company). It took about an hour to run.


```python
symbols = []
incomes = []
name = []
for i in range(len(list_data['symbolsList'])):
    symbol = list_data['symbolsList'][i]['symbol']
    fin_request = requests.get(financial_url + symbol)
    fin_html = bs(fin_request.text, 'html.parser')
    fin_data = json.loads(fin_html.text)
    if len(fin_data['financials']) > 0:

        symbols.append(symbol)
        name.append(list_data['symbolsList'][i]['name'])
        incomes.append(fin_data['financials'][0]['Consolidated Income'])

        

        
dictionary = {'symbol': symbols, 'incomes': incomes, 'name': name}
data = pd.DataFrame(dictionary)
```


```python
data
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
      <th>symbol</th>
      <th>incomes</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CMCSA</td>
      <td>11862000000.0</td>
      <td>Comcast Corporation Class A Common Stock</td>
    </tr>
    <tr>
      <th>1</th>
      <td>KMI</td>
      <td>1919000000.0</td>
      <td>Kinder Morgan Inc.</td>
    </tr>
    <tr>
      <th>2</th>
      <td>INTC</td>
      <td>21053000000.0</td>
      <td>Intel Corporation</td>
    </tr>
    <tr>
      <th>3</th>
      <td>MU</td>
      <td>14138000000.0</td>
      <td>Micron Technology Inc.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GE</td>
      <td>-22443000000.0</td>
      <td>General Electric Company</td>
    </tr>
    <tr>
      <th>5</th>
      <td>BAC</td>
      <td>28147000000.0</td>
      <td>Bank of America Corporation</td>
    </tr>
    <tr>
      <th>6</th>
      <td>AAPL</td>
      <td>59531000000.0</td>
      <td>Apple Inc.</td>
    </tr>
    <tr>
      <th>7</th>
      <td>MSFT</td>
      <td>16571000000.0</td>
      <td>Microsoft Corporation</td>
    </tr>
    <tr>
      <th>8</th>
      <td>SIRI</td>
      <td>1175893000.0</td>
      <td>Sirius XM Holdings Inc.</td>
    </tr>
    <tr>
      <th>9</th>
      <td>HPQ</td>
      <td>5327000000.0</td>
      <td>HP Inc.</td>
    </tr>
    <tr>
      <th>10</th>
      <td>CX</td>
      <td>574285714.2857</td>
      <td>Cemex S.A.B. de C.V. Sponsored ADR</td>
    </tr>
    <tr>
      <th>11</th>
      <td>CZR</td>
      <td>304000000.0</td>
      <td>Caesars Entertainment Corporation</td>
    </tr>
    <tr>
      <th>12</th>
      <td>F</td>
      <td>3695000000.0</td>
      <td>Ford Motor Company</td>
    </tr>
    <tr>
      <th>13</th>
      <td>AMD</td>
      <td>337000000.0</td>
      <td>Advanced Micro Devices Inc.</td>
    </tr>
    <tr>
      <th>14</th>
      <td>SNAP</td>
      <td>-1255911000.0</td>
      <td>Snap Inc. Class A</td>
    </tr>
    <tr>
      <th>15</th>
      <td>FB</td>
      <td>22112000000.0</td>
      <td>Facebook Inc.</td>
    </tr>
    <tr>
      <th>16</th>
      <td>WFC</td>
      <td>22876000000.0</td>
      <td>Wells Fargo &amp; Company</td>
    </tr>
    <tr>
      <th>17</th>
      <td>AIG</td>
      <td>61000000.0</td>
      <td>American International Group Inc.</td>
    </tr>
    <tr>
      <th>18</th>
      <td>T</td>
      <td>19953000000.0</td>
      <td>AT&amp;T; Inc.</td>
    </tr>
    <tr>
      <th>19</th>
      <td>C</td>
      <td>18080000000.0</td>
      <td>Citigroup Inc.</td>
    </tr>
    <tr>
      <th>20</th>
      <td>VALE</td>
      <td>6896000000.0</td>
      <td>VALE S.A. American Depositary Shares Each Repr...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>MS</td>
      <td>8883000000.0</td>
      <td>Morgan Stanley</td>
    </tr>
    <tr>
      <th>22</th>
      <td>AKS</td>
      <td>244100000.0</td>
      <td>AK Steel Holding Corporation</td>
    </tr>
    <tr>
      <th>23</th>
      <td>JPM</td>
      <td>32474000000.0</td>
      <td>JP Morgan Chase &amp; Co.</td>
    </tr>
    <tr>
      <th>24</th>
      <td>ORCL</td>
      <td>3825000000.0</td>
      <td>Oracle Corporation</td>
    </tr>
    <tr>
      <th>25</th>
      <td>NKE</td>
      <td>1933000000.0</td>
      <td>Nike Inc.</td>
    </tr>
    <tr>
      <th>26</th>
      <td>PG</td>
      <td>9861000000.0</td>
      <td>Procter &amp; Gamble Company (The)</td>
    </tr>
    <tr>
      <th>27</th>
      <td>GSM</td>
      <td>24573000.0</td>
      <td>Ferroglobe PLC</td>
    </tr>
    <tr>
      <th>28</th>
      <td>HK</td>
      <td>45959000.0</td>
      <td>Halcon Resources Corporation</td>
    </tr>
    <tr>
      <th>29</th>
      <td>BBD</td>
      <td>4316607989.6907</td>
      <td>Banco Bradesco Sa American Depositary Shares</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5030</th>
      <td>VXRT</td>
      <td>-18007000.0</td>
      <td>Vaxart Inc.</td>
    </tr>
    <tr>
      <th>5031</th>
      <td>VZA</td>
      <td>15528000000.0</td>
      <td>Verizon Communications Inc. 5.90% Notes due 2054</td>
    </tr>
    <tr>
      <th>5032</th>
      <td>WCFB</td>
      <td>226407.0</td>
      <td>WCF Bancorp Inc.</td>
    </tr>
    <tr>
      <th>5033</th>
      <td>WEAR</td>
      <td>1000000.0</td>
      <td>Exchange Listed Funds Trust ETF The WEAR</td>
    </tr>
    <tr>
      <th>5034</th>
      <td>WEBK</td>
      <td>5991000.0</td>
      <td>Wellesley Bancorp Inc.</td>
    </tr>
    <tr>
      <th>5035</th>
      <td>WHLM</td>
      <td>856000.0</td>
      <td>Wilhelmina International Inc.</td>
    </tr>
    <tr>
      <th>5036</th>
      <td>WHLRD</td>
      <td>-16000000.0</td>
      <td>Wheeler Real Estate Investment Trust Inc. Seri...</td>
    </tr>
    <tr>
      <th>5037</th>
      <td>WILC</td>
      <td>6640159.5745</td>
      <td>G. Willi-Food International  Ltd.</td>
    </tr>
    <tr>
      <th>5038</th>
      <td>WINA</td>
      <td>30125500.0</td>
      <td>Winmark Corporation</td>
    </tr>
    <tr>
      <th>5039</th>
      <td>WINS</td>
      <td>10499876.0</td>
      <td>Wins Finance Holdings Inc.</td>
    </tr>
    <tr>
      <th>5040</th>
      <td>WMLP</td>
      <td>-32000000.0</td>
      <td>Westmoreland Resource Partners LP representing...</td>
    </tr>
    <tr>
      <th>5041</th>
      <td>WRLS</td>
      <td>-3939349.0</td>
      <td>Pensare Acquisition Corp.</td>
    </tr>
    <tr>
      <th>5042</th>
      <td>WRN</td>
      <td>-2115674.0741</td>
      <td>Western Copper and Gold Corporation</td>
    </tr>
    <tr>
      <th>5043</th>
      <td>WSCI</td>
      <td>-1000000.0</td>
      <td>WSI Industries Inc.</td>
    </tr>
    <tr>
      <th>5044</th>
      <td>WSTG</td>
      <td>3538000.0</td>
      <td>Wayside Technology Group Inc.</td>
    </tr>
    <tr>
      <th>5045</th>
      <td>WTT</td>
      <td>35000.0</td>
      <td>Wireless Telecom Group Inc.</td>
    </tr>
    <tr>
      <th>5046</th>
      <td>WVFC</td>
      <td>2125000.0</td>
      <td>WVS Financial Corp.</td>
    </tr>
    <tr>
      <th>5047</th>
      <td>WVVI</td>
      <td>2858580.0</td>
      <td>Willamette Valley Vineyards Inc.</td>
    </tr>
    <tr>
      <th>5048</th>
      <td>WVVIP</td>
      <td>3000000.0</td>
      <td>Willamette Valley Vineyards Inc. Series A Rede...</td>
    </tr>
    <tr>
      <th>5049</th>
      <td>XBIO</td>
      <td>-7300458.0</td>
      <td>Xenetic Biosciences Inc.</td>
    </tr>
    <tr>
      <th>5050</th>
      <td>XBIT</td>
      <td>-21138000.0</td>
      <td>XBiotech Inc.</td>
    </tr>
    <tr>
      <th>5051</th>
      <td>XELB</td>
      <td>1088000.0</td>
      <td>Xcel Brands Inc</td>
    </tr>
    <tr>
      <th>5052</th>
      <td>XGTI</td>
      <td>-11000000.0</td>
      <td>XG Technology Inc</td>
    </tr>
    <tr>
      <th>5053</th>
      <td>XTLB</td>
      <td>2986000.0</td>
      <td>XTL Biopharmaceuticals Ltd.</td>
    </tr>
    <tr>
      <th>5054</th>
      <td>YRIV</td>
      <td>-13716485.0</td>
      <td>Yangtze River Port and Logistics Limited</td>
    </tr>
    <tr>
      <th>5055</th>
      <td>YTEN</td>
      <td>-9170000.0</td>
      <td>Yield10 Bioscience Inc.</td>
    </tr>
    <tr>
      <th>5056</th>
      <td>ZAIS</td>
      <td>-4000000.0</td>
      <td>ZAIS Group Holdings Inc.</td>
    </tr>
    <tr>
      <th>5057</th>
      <td>ZKIN</td>
      <td>7103057.0</td>
      <td>ZK International Group Co. Ltd</td>
    </tr>
    <tr>
      <th>5058</th>
      <td>ZOM</td>
      <td>-16647687.0</td>
      <td>Zomedica Pharmaceuticals Corp.</td>
    </tr>
    <tr>
      <th>5059</th>
      <td>ZYME</td>
      <td>-36556000.0</td>
      <td>Zymeworks Inc.</td>
    </tr>
  </tbody>
</table>
<p>5060 rows × 3 columns</p>
</div>




```python
data.incomes = pd.to_numeric(data.incomes)
```


```python
data[data.incomes < 0]
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
      <th>symbol</th>
      <th>incomes</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>GE</td>
      <td>-2.244300e+10</td>
      <td>General Electric Company</td>
    </tr>
    <tr>
      <th>14</th>
      <td>SNAP</td>
      <td>-1.255911e+09</td>
      <td>Snap Inc. Class A</td>
    </tr>
    <tr>
      <th>31</th>
      <td>JD</td>
      <td>-4.052894e+08</td>
      <td>JD.com Inc.</td>
    </tr>
    <tr>
      <th>32</th>
      <td>NOK</td>
      <td>-6.896552e+04</td>
      <td>Nokia Corporation Sponsored American Depositar...</td>
    </tr>
    <tr>
      <th>36</th>
      <td>WFT</td>
      <td>-2.811000e+09</td>
      <td>Weatherford International plc (Ireland)</td>
    </tr>
    <tr>
      <th>40</th>
      <td>QCOM</td>
      <td>-4.864000e+09</td>
      <td>QUALCOMM Incorporated</td>
    </tr>
    <tr>
      <th>49</th>
      <td>GERN</td>
      <td>-2.701700e+07</td>
      <td>Geron Corporation</td>
    </tr>
    <tr>
      <th>56</th>
      <td>CLNS</td>
      <td>-6.461300e+07</td>
      <td>Colony NorthStar Inc.</td>
    </tr>
    <tr>
      <th>62</th>
      <td>SQ</td>
      <td>-3.845300e+07</td>
      <td>Square Inc. Class A</td>
    </tr>
    <tr>
      <th>67</th>
      <td>RAD</td>
      <td>-4.222130e+08</td>
      <td>Rite Aid Corporation</td>
    </tr>
    <tr>
      <th>70</th>
      <td>ESV</td>
      <td>-6.366000e+08</td>
      <td>Ensco plc Class A</td>
    </tr>
    <tr>
      <th>80</th>
      <td>MAT</td>
      <td>-5.309930e+08</td>
      <td>Mattel Inc.</td>
    </tr>
    <tr>
      <th>81</th>
      <td>CTL</td>
      <td>-1.733000e+09</td>
      <td>CenturyLink Inc.</td>
    </tr>
    <tr>
      <th>90</th>
      <td>AABA</td>
      <td>-2.094630e+08</td>
      <td>Altaba Inc.</td>
    </tr>
    <tr>
      <th>100</th>
      <td>AUY</td>
      <td>-2.977000e+08</td>
      <td>Yamana Gold Inc. (Canada)</td>
    </tr>
    <tr>
      <th>108</th>
      <td>GFI</td>
      <td>-3.448000e+08</td>
      <td>Gold Fields Limited American Depositary Shares</td>
    </tr>
    <tr>
      <th>109</th>
      <td>KGC</td>
      <td>-2.560000e+07</td>
      <td>Kinross Gold Corporation</td>
    </tr>
    <tr>
      <th>111</th>
      <td>NBR</td>
      <td>-6.127260e+08</td>
      <td>Nabors Industries Ltd.</td>
    </tr>
    <tr>
      <th>122</th>
      <td>HMY</td>
      <td>-3.210000e+08</td>
      <td>Harmony Gold Mining Company Limited</td>
    </tr>
    <tr>
      <th>125</th>
      <td>NGD</td>
      <td>-1.225700e+09</td>
      <td>New Gold Inc.</td>
    </tr>
    <tr>
      <th>126</th>
      <td>KOS</td>
      <td>-9.399100e+07</td>
      <td>Kosmos Energy Ltd.</td>
    </tr>
    <tr>
      <th>137</th>
      <td>PTEN</td>
      <td>-3.214210e+08</td>
      <td>Patterson-UTI Energy Inc.</td>
    </tr>
    <tr>
      <th>142</th>
      <td>QEP</td>
      <td>-1.011600e+09</td>
      <td>QEP Resources Inc.</td>
    </tr>
    <tr>
      <th>146</th>
      <td>MRVL</td>
      <td>-1.790940e+08</td>
      <td>Marvell Technology Group Ltd.</td>
    </tr>
    <tr>
      <th>149</th>
      <td>CDNA</td>
      <td>-4.678100e+07</td>
      <td>CareDx Inc.</td>
    </tr>
    <tr>
      <th>157</th>
      <td>RIG</td>
      <td>-2.003000e+09</td>
      <td>Transocean Ltd (Switzerland)</td>
    </tr>
    <tr>
      <th>164</th>
      <td>OAS</td>
      <td>-1.950000e+07</td>
      <td>Oasis Petroleum Inc.</td>
    </tr>
    <tr>
      <th>166</th>
      <td>CVS</td>
      <td>-5.960000e+08</td>
      <td>CVS Health Corporation</td>
    </tr>
    <tr>
      <th>167</th>
      <td>HES</td>
      <td>-1.150000e+08</td>
      <td>Hess Corporation</td>
    </tr>
    <tr>
      <th>170</th>
      <td>NLSN</td>
      <td>-7.000000e+08</td>
      <td>Nielsen N.V.</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4993</th>
      <td>TRMT</td>
      <td>-1.608000e+06</td>
      <td>Tremont Mortgage Trust</td>
    </tr>
    <tr>
      <th>4995</th>
      <td>TRPX</td>
      <td>-2.380053e+06</td>
      <td>Therapix Biosciences Ltd.</td>
    </tr>
    <tr>
      <th>4996</th>
      <td>TRX</td>
      <td>-5.225301e+06</td>
      <td>Tanzanian Royalty Exploration Corporation</td>
    </tr>
    <tr>
      <th>4998</th>
      <td>TURN</td>
      <td>-1.636636e+07</td>
      <td>180 Degree Capital Corp.</td>
    </tr>
    <tr>
      <th>5002</th>
      <td>UBC</td>
      <td>-7.100000e+07</td>
      <td>E-TRACS USB Bloomberg Commodity Index Exchange...</td>
    </tr>
    <tr>
      <th>5005</th>
      <td>UBN</td>
      <td>-2.800000e+07</td>
      <td>E-TRACS USB Bloomberg Commodity Index Exchange...</td>
    </tr>
    <tr>
      <th>5009</th>
      <td>UNAM</td>
      <td>-3.169559e+06</td>
      <td>Unico American Corporation</td>
    </tr>
    <tr>
      <th>5015</th>
      <td>USAS</td>
      <td>-1.067800e+07</td>
      <td>Americas Silver Corporation no par value</td>
    </tr>
    <tr>
      <th>5016</th>
      <td>USATP</td>
      <td>-2.000000e+06</td>
      <td>USA Technologies Inc. Preferred Stock</td>
    </tr>
    <tr>
      <th>5022</th>
      <td>VII</td>
      <td>-5.000000e+06</td>
      <td>Vicon Industries Inc</td>
    </tr>
    <tr>
      <th>5023</th>
      <td>VIRC</td>
      <td>-1.614000e+06</td>
      <td>Virco Manufacturing Corporation</td>
    </tr>
    <tr>
      <th>5024</th>
      <td>VNCE</td>
      <td>-2.022000e+06</td>
      <td>Vince Holding Corp.</td>
    </tr>
    <tr>
      <th>5025</th>
      <td>VRNA</td>
      <td>-1.990100e+07</td>
      <td>Verona Pharma plc</td>
    </tr>
    <tr>
      <th>5027</th>
      <td>VTGN</td>
      <td>-1.434590e+07</td>
      <td>VistaGen Therapeutics Inc.</td>
    </tr>
    <tr>
      <th>5028</th>
      <td>VTNR</td>
      <td>-1.983579e+06</td>
      <td>Vertex Energy Inc</td>
    </tr>
    <tr>
      <th>5029</th>
      <td>VVUS</td>
      <td>-3.695000e+07</td>
      <td>VIVUS Inc.</td>
    </tr>
    <tr>
      <th>5030</th>
      <td>VXRT</td>
      <td>-1.800700e+07</td>
      <td>Vaxart Inc.</td>
    </tr>
    <tr>
      <th>5036</th>
      <td>WHLRD</td>
      <td>-1.600000e+07</td>
      <td>Wheeler Real Estate Investment Trust Inc. Seri...</td>
    </tr>
    <tr>
      <th>5040</th>
      <td>WMLP</td>
      <td>-3.200000e+07</td>
      <td>Westmoreland Resource Partners LP representing...</td>
    </tr>
    <tr>
      <th>5041</th>
      <td>WRLS</td>
      <td>-3.939349e+06</td>
      <td>Pensare Acquisition Corp.</td>
    </tr>
    <tr>
      <th>5042</th>
      <td>WRN</td>
      <td>-2.115674e+06</td>
      <td>Western Copper and Gold Corporation</td>
    </tr>
    <tr>
      <th>5043</th>
      <td>WSCI</td>
      <td>-1.000000e+06</td>
      <td>WSI Industries Inc.</td>
    </tr>
    <tr>
      <th>5049</th>
      <td>XBIO</td>
      <td>-7.300458e+06</td>
      <td>Xenetic Biosciences Inc.</td>
    </tr>
    <tr>
      <th>5050</th>
      <td>XBIT</td>
      <td>-2.113800e+07</td>
      <td>XBiotech Inc.</td>
    </tr>
    <tr>
      <th>5052</th>
      <td>XGTI</td>
      <td>-1.100000e+07</td>
      <td>XG Technology Inc</td>
    </tr>
    <tr>
      <th>5054</th>
      <td>YRIV</td>
      <td>-1.371648e+07</td>
      <td>Yangtze River Port and Logistics Limited</td>
    </tr>
    <tr>
      <th>5055</th>
      <td>YTEN</td>
      <td>-9.170000e+06</td>
      <td>Yield10 Bioscience Inc.</td>
    </tr>
    <tr>
      <th>5056</th>
      <td>ZAIS</td>
      <td>-4.000000e+06</td>
      <td>ZAIS Group Holdings Inc.</td>
    </tr>
    <tr>
      <th>5058</th>
      <td>ZOM</td>
      <td>-1.664769e+07</td>
      <td>Zomedica Pharmaceuticals Corp.</td>
    </tr>
    <tr>
      <th>5059</th>
      <td>ZYME</td>
      <td>-3.655600e+07</td>
      <td>Zymeworks Inc.</td>
    </tr>
  </tbody>
</table>
<p>1781 rows × 3 columns</p>
</div>



### Number of companies with negative net income (first number in the parantheses)


```python
data[data.incomes < 0].shape
```




    (1781, 3)



### Number of companies with positive net income (first number in the parantheses)


```python
data[data.incomes > 0].shape
```




    (3235, 3)



### Company with the most income


```python
data[data.incomes == max(data.incomes)]
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
      <th>symbol</th>
      <th>incomes</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>478</th>
      <td>CCE</td>
      <td>3.965092e+18</td>
      <td>Coca-Cola European Partners plc</td>
    </tr>
  </tbody>
</table>
</div>



### Company that lost the most money


```python
data[data.incomes == min(data.incomes)]
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
      <th>symbol</th>
      <th>incomes</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>GE</td>
      <td>-2.244300e+10</td>
      <td>General Electric Company</td>
    </tr>
  </tbody>
</table>
</div>


