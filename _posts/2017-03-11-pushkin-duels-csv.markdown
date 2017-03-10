---
layout: post
title:  "Pushkin's Duels CSV"
date:   2017-03-11
categories: pushkin duels
---


There are more than 25 known Pushkin's duels.<br/>
Let's make CSV containing these duels details.<br/>
Here its full content:


```python
import requests

DUELS_CSV_URL = 'https://raw.githubusercontent.com/boorstat/boorstat-files/master/lit/pushkin/duels.csv'

print(requests.get(DUELS_CSV_URL).text)
```

    year,opponent_name,opponent_descr,cause,pushkin_shot,opponent_shot
    1816,Paul Hannibal,uncle,during a ball Paul lugged away Pushkin’s girlfriend miss Loshakova,0,0
    1817,Pyotr Kaverin,friend,Kaverin’s facetious poem,0,0
    1819,Kondratiy Ryleev,poet,Ryleev told a joke about Pushkin at a high society gathering,0,0
    1819,Wilhelm Kiichelbecker,friend,funny verses about Küchelbecker,0,1
    1819,Modest Korf,Ministry of justice worker,Pushkin’s drunk manservant pestered Korf’s servant who finally beat Pushkin’s servant up,0,0
    1819,Denisevich,Major,Pushkin behaved provocatively in theater: he yelled at actors,0,0
    1820,Orlov Fedor and Alexey Alexeev,,Orlov and Alexeev reprimanded Pushkin for being drunk and trying to play pool,0,0
    1821,Deguilly,French military officer,a quarrel under unclear circumstances,0,0
    1822,Semyon Starov,lieutenant colonel,a conflict occurred because of a restaurant orchestra at a casino where both indulged in gambling,1,1
    1822,Ivan Lanov,65-year-old state councilor,a quarrel during a holiday dinner,0,0
    1822,Todor Balsh,Moldavian nobleman,Balsh’s wife Maria responded to Pushkin’s question in an impolite manner,1,1
    1822,Skartla Pruncul,Bessarabian landowner,they were seconds at someone else’s duel and could not agree upon the rules of the duel,0,0
    1822,Seweryn Potocki,Active Privy Councillor,discussion about serfdom at the dinner table,0,0
    1822,Rutkowski,captain,Pushkin did not believe that a hailstone can weigh up to 3 pounds (which is possible) and made fun of the retired captain,0,0
    1822,Inglezi,Chisinau tycoon,Pushkin coveted his wife (a gypsy woman Ludmila Shekora),0,0
    1832,Alexander Zubov,General Staff warrant officer,Pushkin had caught Zubov on cheating during a game of cards,0,1
    1823,Ivan Rousseau,young writer,Pushkin’s personal dislike for this person,0,0
    1826,Nikolay Turgenev,one of the leaders of the Union of Welfare and a member of the Northern Society,Tugrenev did not approve of Pushkin’s poem,0,0
    1827,Vladimir Solomirskiy,artillery officer,the officer’s female friend Sofia to whom Pushkin was personally attracted,0,0
    1828,Alexander Golitsyn,Minister of Education,Pushkin wrote a bold epigram so the Minister arranged a rough interrogation,0,0
    1828,Lagrenée,French Embassy Secretary in St.Petersburg,an unknown girl at a ball,0,0
    1829,Mr. Hvostov,Foreign Office worker,Hvostov was dissatisfied by Pushkin’s epigrams,0,0
    1836,Nikolay Repin,,Repin was dissatisfied with Pushkin’s poems about him,0,0
    1836,Semyon Hlustin,Foreign Office worker,Hlustin did not approve of Pushkin’s poetry,0,0
    1836,Vladimir Sollogub,minor Russian writer,Sologub’s unflattering remarks about the poet’s wife Natalia,0,0
    1836,George d’Anthès,French officer,an anonymous letter which stated that Pushkin’s wife had been cheating on her husband with d’Anthès,0,0
    1837,George d’Anthès,French officer,an anonymous letter which stated that Pushkin’s wife had been cheating on her husband with d’Anthès,1,1


<a href="https://raw.githubusercontent.com/boorstat/boorstat-files/master/lit/pushkin/duels.csv">Pushkin's duels csv</a><br/>

Source for this data:<br/>
<https://rinatim.com/2016/12/03/alexander-pushkins-duels/><br/>
<http://d-push.net><br/>
<https://ru.wikipedia.org/wiki/Пушкин,_Александр_Сергеевич><br/>

And now let's try to use it and plot some visual representation of Pushkin's duels into real shots conversion.<br/>
Starting with imports:


```python
from io import StringIO

import pandas as pd
import plotly.plotly as py
import plotly.graph_objs as go
```

Getting csv as pandas dataframe:


```python
df = pd.DataFrame.from_csv(StringIO(requests.get(DUELS_CSV_URL).text), index_col=None)
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>opponent_name</th>
      <th>opponent_descr</th>
      <th>cause</th>
      <th>pushkin_shot</th>
      <th>opponent_shot</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1816</td>
      <td>Paul Hannibal</td>
      <td>uncle</td>
      <td>during a ball Paul lugged away Pushkin’s girlf...</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1817</td>
      <td>Pyotr Kaverin</td>
      <td>friend</td>
      <td>Kaverin’s facetious poem</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1819</td>
      <td>Kondratiy Ryleev</td>
      <td>poet</td>
      <td>Ryleev told a joke about Pushkin at a high soc...</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1819</td>
      <td>Wilhelm Kiichelbecker</td>
      <td>friend</td>
      <td>funny verses about Küchelbecker</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1819</td>
      <td>Modest Korf</td>
      <td>Ministry of justice worker</td>
      <td>Pushkin’s drunk manservant pestered Korf’s ser...</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



Looks very exciting as for me:)<br/>
Time to plot Pushkin's Duels Histogram.


```python
life_years = {'start': 1799, 'end': 1837, 'size': 1}

nobody_shot_data = go.Histogram(
    x=df[(df['pushkin_shot'] == 0) & (df['opponent_shot'] == 0)]['year'],
    xbins=life_years,
    name='Nobody'
)

only_opponent_shot_data = go.Histogram(
    x=df[(df['pushkin_shot'] == 0) & (df['opponent_shot'] == 1)]['year'],
    xbins=life_years,
    name='Only opponent'
)

both_shot_data = go.Histogram(
    x=df[(df['pushkin_shot'] == 1) & (df['opponent_shot'] == 1)]['year'],
    xbins=life_years,
    name='Both shot'
)

data = [only_opponent_shot_data, nobody_shot_data, both_shot_data]
layout = go.Layout(barmode='stack', title="Pushkin's Duels Histogram")
fig = go.Figure(data=data, layout=layout)

py.iplot(fig, filename='pushkin-duels')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~boorstat/12.embed" height="525px" width="100%"></iframe>
