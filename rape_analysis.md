# Rapes in Italy


Little analysis using official [INSTAT data](http://dati.istat.it/Index.aspx?DataSetCode=DCCV_AUTVITTPS) on sexual violence showing that regular foreigners are 7.5 times more like to commit sexual violence

Let's start by opening the INSTAT data


```python
import pandas as pd

df = pd.read_csv('./DCCV_AUTVITTPS_02022021182031375.csv', index_col=0)
```

Let's check the crimes types


```python
df['Tipo di delitto'].unique()
```




    array(['strage', 'omicidi volontari consumati',
           'omicidi volontari consumati a scopo di furto o rapina',
           'omicidi volontari consumati di tipo mafioso',
           'omicidi volontari consumati a scopo terroristico',
           'tentati omicidi', 'infanticidi', 'omicidi preterintenzionali',
           'omicidi colposi', 'omicidi colposi da incidente stradale',
           'percosse', 'lesioni dolose', 'minacce', 'stalking',
           'sequestri di persona', 'violenze sessuali',
           'atti sessuali con minorenne', 'corruzione di minorenne',
           'sfruttamento e favoreggiamento della prostituzione',
           'pornografia minorile e detenzione di materiale pedopornografico',
           'furti', 'furti con strappo', 'furti con destrezza',
           'furti in abitazioni', 'furti in esercizi commerciali',
           'furti in auto in sosta',
           "furti di opere d'arte e materiale archeologico",
           'furti di automezzi pesanti trasportanti merci',
           'furti di ciclomotori', 'furti di motocicli',
           'furti di autovetture', 'rapine', 'rapine in abitazione',
           'rapine in banca', 'rapine in uffici postali',
           'rapine in esercizi commerciali', 'rapine in pubblica via',
           'estorsioni', 'truffe e frodi informatiche', 'delitti informatici',
           'contraffazione di marchi e prodotti industriali',
           'violazione della proprietà intellettuale', 'ricettazione',
           'riciclaggio e impiego di denaro, beni o utilità di provenienza illecita',
           'usura', 'danneggiamenti', 'incendi', 'incendi boschivi',
           'danneggiamento seguito da incendio',
           'normativa sugli stupefacenti', 'attentati',
           'associazione per delinquere', 'associazione di tipo mafioso',
           'contrabbando', 'altri delitti', 'totale'], dtype=object)



## Rapes %

Okay, now we have find out the correct ratio for sexual violence between italians and foreigners


```python
df_rape = df[(df['Tipo di delitto'] == 'violenze sessuali') & (df['TIPO_DATO35'] == 'OFFEND')]

rapes_it = df_rape[df_rape['Cittadinanza'] == 'italiano-a']['Value'].sum()

rapes_fo = df_rape[df_rape['Cittadinanza'] == 'straniero-a']['Value'].sum()

rapes_tot = df_rape[df_rape['Cittadinanza'] == 'totale']['Value'].sum()

rapes_it, rapes_fo, rapes_tot

f'Rapes it = {rapes_it / rapes_tot:.2f}%, rapes foreigner = {rapes_fo / rapes_tot:.2f}%'
```




    'Rapes it = 0.58%, rapes foreigner = 0.42%'



### Normalize per population


```python
tot_pop = 60360000
pop_fo = 5255503
pop_it = tot_pop - pop_fo

pop_it_per = pop_it / tot_pop 
pop_pop_per = pop_fo / tot_pop 

rapes_it_per = rapes_it / rapes_tot
rapes_fo_per = rapes_fo / rapes_tot

# compute ratio rape/person for italians
rapes_ratio_it = rapes_it_per / pop_it_per 

# compute ratio rape/person for foreigners
rapes_ratio_fo = rapes_fo_per / pop_pop_per

rapes_ratio_it, rapes_ratio_fo

f'Foreigners rapes {rapes_ratio_fo / rapes_ratio_it:.2f} times more than Italian'
```




    'Foreigners rapes 7.50 times more than Italian'




```python
df_rape
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
      <th>Territorio</th>
      <th>TIPO_DATO35</th>
      <th>Tipo dato</th>
      <th>REATI_PS</th>
      <th>Tipo di delitto</th>
      <th>SEXISTAT1</th>
      <th>Sesso</th>
      <th>ETA1</th>
      <th>Classe di età</th>
      <th>CITTADINANZA</th>
      <th>Cittadinanza</th>
      <th>TIME</th>
      <th>Seleziona periodo</th>
      <th>Value</th>
      <th>Flag Codes</th>
      <th>Flags</th>
    </tr>
    <tr>
      <th>ITTER107</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y18-24</td>
      <td>18-24 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>308</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y18-24</td>
      <td>18-24 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y25-34</td>
      <td>25-34 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>420</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y25-34</td>
      <td>25-34 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>11</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y35-44</td>
      <td>35-44 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>640</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y35-44</td>
      <td>35-44 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>17</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y45-54</td>
      <td>45-54 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>608</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y45-54</td>
      <td>45-54 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>13</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y55-64</td>
      <td>55-64 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>396</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y55-64</td>
      <td>55-64 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y_GE65</td>
      <td>65 anni e più</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>344</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y_GE65</td>
      <td>65 anni e più</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y14-17</td>
      <td>14-17 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>123</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y14-17</td>
      <td>14-17 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y_UN13</td>
      <td>fino a 13 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>24</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y_UN13</td>
      <td>fino a 13 anni</td>
      <td>ITL</td>
      <td>italiano-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y18-24</td>
      <td>18-24 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>425</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y18-24</td>
      <td>18-24 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y25-34</td>
      <td>25-34 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>653</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y25-34</td>
      <td>25-34 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y35-44</td>
      <td>35-44 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>522</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y35-44</td>
      <td>35-44 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>15</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y45-54</td>
      <td>45-54 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>243</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y45-54</td>
      <td>45-54 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y55-64</td>
      <td>55-64 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y_GE65</td>
      <td>65 anni e più</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>30</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y_GE65</td>
      <td>65 anni e più</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y14-17</td>
      <td>14-17 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>96</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y14-17</td>
      <td>14-17 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y_UN13</td>
      <td>fino a 13 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>16</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y_UN13</td>
      <td>fino a 13 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y18-24</td>
      <td>18-24 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>733</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y18-24</td>
      <td>18-24 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y35-44</td>
      <td>35-44 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>1162</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y55-64</td>
      <td>55-64 anni</td>
      <td>FRG</td>
      <td>straniero-a</td>
      <td>2019</td>
      <td>2019</td>
      <td>75</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y45-54</td>
      <td>45-54 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>851</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y35-44</td>
      <td>35-44 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>32</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y45-54</td>
      <td>45-54 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>17</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y25-34</td>
      <td>25-34 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>1073</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y55-64</td>
      <td>55-64 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>471</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y25-34</td>
      <td>25-34 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>20</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y55-64</td>
      <td>55-64 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y_GE65</td>
      <td>65 anni e più</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>374</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y_GE65</td>
      <td>65 anni e più</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y14-17</td>
      <td>14-17 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y14-17</td>
      <td>14-17 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>219</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>1</td>
      <td>maschi</td>
      <td>Y_UN13</td>
      <td>fino a 13 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>40</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>IT</th>
      <td>Italia</td>
      <td>OFFEND</td>
      <td>numero di autori di delitto denunciati/arresta...</td>
      <td>RAPE</td>
      <td>violenze sessuali</td>
      <td>2</td>
      <td>femmine</td>
      <td>Y_UN13</td>
      <td>fino a 13 anni</td>
      <td>TOTAL</td>
      <td>totale</td>
      <td>2019</td>
      <td>2019</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Meaning if you are women and you want to go out, you are 7.5 less like to be sexually attacked by an Italian
