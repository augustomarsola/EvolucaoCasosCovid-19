# Evolução dos casos em escala Global


```python
#Importando bibliotecas
import numpy as np
import pandas as pd
import chart_studio.plotly as py
import plotly.express as px
import plotly.graph_objs as go
from chart_studio.plotly import plot, iplot
from plotly.subplots import make_subplots
```


```python
#Lendo os dados
#Os dados podem ser obtidos em https://www.kaggle.com/sudalairajkumar/novel-corona-virus-2019-dataset
df = pd.read_csv('dataCorona/covid_19_data.csv')
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SNo</th>
      <th>ObservationDate</th>
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Last Update</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>01/22/2020</td>
      <td>Anhui</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>01/22/2020</td>
      <td>Beijing</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>01/22/2020</td>
      <td>Chongqing</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>01/22/2020</td>
      <td>Fujian</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>01/22/2020</td>
      <td>Gansu</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Renomeando as colunas
df = df.rename(columns={'Country/Region':'Country'})
df = df.rename(columns={'ObservationDate':'Date'})
```


```python
#Manipulando o Dataframe
df_countries = df.groupby(['Country', 'Date']).sum().reset_index().sort_values(by='Date', ascending=False)
df_countries = df_countries.drop_duplicates(subset = ['Country'])
df_countries = df_countries[df_countries['Confirmed']>0]
df_countries.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Date</th>
      <th>SNo</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>415</th>
      <td>Azerbaijan</td>
      <td>04/05/2020</td>
      <td>11940</td>
      <td>584.0</td>
      <td>7.0</td>
      <td>32.0</td>
    </tr>
    <tr>
      <th>1218</th>
      <td>Congo (Brazzaville)</td>
      <td>04/05/2020</td>
      <td>11966</td>
      <td>45.0</td>
      <td>5.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4707</th>
      <td>Qatar</td>
      <td>04/05/2020</td>
      <td>12063</td>
      <td>1604.0</td>
      <td>4.0</td>
      <td>123.0</td>
    </tr>
    <tr>
      <th>430</th>
      <td>Bahamas</td>
      <td>04/05/2020</td>
      <td>11941</td>
      <td>28.0</td>
      <td>4.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4137</th>
      <td>New Zealand</td>
      <td>04/05/2020</td>
      <td>12048</td>
      <td>1039.0</td>
      <td>1.0</td>
      <td>156.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Criando o gráfico Choropleth
data = go.Choropleth(
    locations = df_countries['Country'],
    locationmode = 'country names',
    z = df_countries['Confirmed'],
    colorscale = 'Reds',
    marker_line_color = 'black',
    marker_line_width = 0.5
)

fig = go.Figure(data = data)

fig.update_layout(
    title_text = 'Casos Confirmados em 05/04/2020',
    title_x = 0.5,
    geo=dict(
        showframe = False,
        showcoastlines = False,
        projection_type = 'equirectangular'
    )
)

py.iplot(fig)
```


![Gráfico Geral](https://github.com/augustomarsola/EvolucaoCasosCovid-19/blob/master/imgs/graph1.png?raw=true)
<a href="https://plotly.com/~augutoboy/56.embed" target="_blank">Gráfico Geral</a>




# Visualizando os casos com decorrer do tempo


```python
#Manipulando os dados para selecionar todas as datas
df_countrydate = df[df['Confirmed']>0]
df_countrydate = df_countrydate.groupby(['Date','Country']).sum().reset_index()
df_countrydate
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>SNo</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>01/22/2020</td>
      <td>Japan</td>
      <td>36</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>01/22/2020</td>
      <td>Macau</td>
      <td>21</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>01/22/2020</td>
      <td>Mainland China</td>
      <td>373</td>
      <td>547.0</td>
      <td>17.0</td>
      <td>28.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>01/22/2020</td>
      <td>South Korea</td>
      <td>38</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>01/22/2020</td>
      <td>Taiwan</td>
      <td>29</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6377</th>
      <td>04/05/2020</td>
      <td>Vietnam</td>
      <td>12105</td>
      <td>241.0</td>
      <td>0.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>6378</th>
      <td>04/05/2020</td>
      <td>West Bank and Gaza</td>
      <td>12106</td>
      <td>237.0</td>
      <td>1.0</td>
      <td>25.0</td>
    </tr>
    <tr>
      <th>6379</th>
      <td>04/05/2020</td>
      <td>Western Sahara</td>
      <td>12107</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6380</th>
      <td>04/05/2020</td>
      <td>Zambia</td>
      <td>12108</td>
      <td>39.0</td>
      <td>1.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>6381</th>
      <td>04/05/2020</td>
      <td>Zimbabwe</td>
      <td>12109</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>6382 rows × 6 columns</p>
</div>




```python
#Criando a visualização animada
fig = px.choropleth(df_countrydate,
                   locations = 'Country',
                   locationmode = 'country names',
                   color = 'Confirmed',
                   hover_name = 'Country',
                   animation_frame = 'Date'
                   )
fig.update_layout(
    title_text = 'Propagação Grobal do Covid-19',
    title_x = 0.5,
    geo=dict(
        showframe = False,
        showcoastlines = False
    )
)

py.iplot(fig)
```



![Gráfico Evolução](https://github.com/augustomarsola/EvolucaoCasosCovid-19/blob/master/imgs/graph2.png?raw=true)
<a href="https://plotly.com/~augutoboy/58.embed" target="_blank">Gráfico Evolução</a>





# Analisando a propagação nos estados Brasileiros


```python
#Lendo os dados
#Os dados podem ser obtidos em https://covid.saude.gov.br/
df = pd.read_csv('dataCorona/dados_covid_br.csv', sep = ';')
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>regiao</th>
      <th>estado</th>
      <th>data</th>
      <th>casosNovos</th>
      <th>casosAcumulados</th>
      <th>obitosNovos</th>
      <th>obitosAcumulados</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Norte</td>
      <td>RO</td>
      <td>30/01/2020</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Norte</td>
      <td>RO</td>
      <td>31/01/2020</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Norte</td>
      <td>RO</td>
      <td>01/02/2020</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Norte</td>
      <td>RO</td>
      <td>02/02/2020</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Norte</td>
      <td>RO</td>
      <td>03/02/2020</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
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
    </tr>
    <tr>
      <th>1723</th>
      <td>Centro-Oeste</td>
      <td>DF</td>
      <td>29/03/2020</td>
      <td>29</td>
      <td>289</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1724</th>
      <td>Centro-Oeste</td>
      <td>DF</td>
      <td>30/03/2020</td>
      <td>23</td>
      <td>312</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1725</th>
      <td>Centro-Oeste</td>
      <td>DF</td>
      <td>31/03/2020</td>
      <td>20</td>
      <td>332</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1726</th>
      <td>Centro-Oeste</td>
      <td>DF</td>
      <td>01/04/2020</td>
      <td>23</td>
      <td>355</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1727</th>
      <td>Centro-Oeste</td>
      <td>DF</td>
      <td>02/04/2020</td>
      <td>15</td>
      <td>370</td>
      <td>1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>1728 rows × 7 columns</p>
</div>




```python
#Manipulando os dados
df_Brasil = df
df_Brasil = df_Brasil.groupby(['data','estado']).sum().reset_index()
df_Brasil['data'] = pd.to_datetime(df_Brasil['data'], format='%d/%m/%Y')
df_Brasil.sort_values(by='data', inplace = True)
df_Brasil = df_Brasil[df_Brasil['data']>'2020-02-24']
df_Brasil['data'] = df_Brasil['data'].astype('str')
df_Brasil
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>data</th>
      <th>estado</th>
      <th>casosNovos</th>
      <th>casosAcumulados</th>
      <th>obitosNovos</th>
      <th>obitosAcumulados</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1373</th>
      <td>2020-02-25</td>
      <td>SC</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1374</th>
      <td>2020-02-25</td>
      <td>SE</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1350</th>
      <td>2020-02-25</td>
      <td>AC</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1351</th>
      <td>2020-02-25</td>
      <td>AL</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1352</th>
      <td>2020-02-25</td>
      <td>AM</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>138</th>
      <td>2020-04-02</td>
      <td>AP</td>
      <td>0</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>137</th>
      <td>2020-04-02</td>
      <td>AM</td>
      <td>29</td>
      <td>229</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>135</th>
      <td>2020-04-02</td>
      <td>AC</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>149</th>
      <td>2020-04-02</td>
      <td>PB</td>
      <td>1</td>
      <td>21</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>136</th>
      <td>2020-04-02</td>
      <td>AL</td>
      <td>0</td>
      <td>18</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1026 rows × 6 columns</p>
</div>




```python
#Criando os gráficos com visualização global e animada
#Criando a visualização animada
fig = px.line(df_Brasil,
              x = 'data',
              y = 'casosAcumulados',
              color = 'estado',
              )
fig.update_layout(
    title_text = 'Propagação do Covid-19 nos estados brasileiros',
    title_x = 0.5,
)

py.iplot(fig)
```



![Gráfico Estados Brasileiros](https://github.com/augustomarsola/EvolucaoCasosCovid-19/blob/master/imgs/graph3.png?raw=true)
<a href="https://plotly.com/~augutoboy/62.embed" target="_blank">Gráfico Estados Brasileiros</a>

