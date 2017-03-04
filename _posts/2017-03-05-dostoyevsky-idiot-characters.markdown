---
layout: post
title:  "Visualised Dostoyevsky Idiot characters activity rate"
date:   2017-03-05
categories: dostoyevsky idiot
---


In <a href="/dostoyevsky/idiot/2017/01/08/dostoyevsky-idiot-python-object.html">previous post</a> we've generated <a href="https://github.com/boorstat/boorstat-files/raw/master/lit/dostoevsky/idiot.json">json</a> based on <a href="https://github.com/boorstat/boorstat-files/raw/master/lit/dostoevsky/The_Idiot.txt">Idiot text</a>.

In this post we're going to use this data and visualise characters per chapter activity rate.<br/>
First of all we need dict of lists how characters can be called or named in text:


```python
CHARACTERS = {
    'Prince Myshkin': ['Lev Nikolayevich', 'Lef Nicolayevitch', 'Myshkin', r'prince(?! S\.)'],
    'Nastasya Philipovna': ['Nastasia Philipovna', 'Barashkova'],
    'Parfyon Semyonovich Rogozhin': ['Parfyon', 'Rogozhin', 'Rogojin'],
    'General Ivan Fyodorovich Yepanchin': ['general', 'Ivan Fyodorovich'],
    'Elizaveta Prokofyevna': ['Elizabetha', 'Prokofievna', r'Mrs\. Epanchin'],
    'Alexandra Ivanovna': ['Alexandra'],
    'Adelaida Ivanovna': ['Adelaida'],
    'Aglaya Ivanovna': ['Aglaya'],
    'General Ardalion Alexandrovich Ivolgin': ['general', 'Ivolgin', 'Ardalion'],
    'Nina Alexandrovna': ['Nina'],
    'Gavrila Ardalionovich': ['Gavrila', 'Ganya', 'Ganechka', 'Ganka'],
    'Varvara Ardalionovna': ['Varvara'],
    'Lukyan Timofeevich Lebedev': ['Lukyan', 'Lebedeff'],
    'Vera Lukyanovna': ['Vera'],
    'Ippolit Terentyev': ['Ippolit'],
    'Ivan Petrovich Ptitsyn': ['Ivan Petrovich', 'Ptitsin'],
    'Evgeny Pavlovich Radomsky': ['Pavlovitch', 'Radomski'],
    'Prince S.': ['prince S.'],
    'Afanasy Ivanovich Totsky': ['Afanasy Ivanovitch', 'Totski'],
    'Ferdyshchenko': ['Ferdishenko'],
    'Keller': ['Keller'],
    'Antip Burdovsky': ['Antip', 'Burdovsky']
}
```

<a href="https://plot.ly">Plotly</a> and its python API is used to visualise data at the final stage.<br/>
Se we need to do some imports for it:


```python
import plotly.plotly as py
import plotly.graph_objs as go
```

Also we need to import our own python package: python-boorstat.<br/>
It can be easily installed using pip.<br/>
Read <a href="/setup/">how</a>.


```python
from boorstat.lit.dostoyevsky.idiot import idiot
```

This is the most top level of our visualization script:


```python
roman = idiot.from_json()

data = parse_parts(roman)
traces = prepare_data(data)
plot(traces)
```

Final plot function:


```python
def plot(traces):
    layout = go.Layout(title='Idiot Characters')
    fig = go.Figure(data=traces, layout=layout)

    return py.iplot(
        fig,
        filename='idiot-characters',
        sharing='public')
```

Couple of functions where idiot parts and chapters parsed and charactes rates are set:


```python
def rate_characters(chapter):
    characters = {}

    for char, regexps in CHARACTERS.items():
        characters[char] = sum([len(re.findall(regex, chapter['text'], re.U)) for regex in regexps])

    return characters


def parse_parts(roman):
    data = []

    for part in roman['parts']:
        for chapter in part['chapters']:
            data.append({
                'chapter': '{} - {}'.format(part['title'], chapter['title']),
                'rates': rate_characters(chapter)})

    return data
```

And huge code to convert native python data to plotly objects ready for plotting:


```python
def prepare_data(data):
    data = data

    traces = []

    for character in reversed(sorted(data[0]['rates'])):
        traces.append(go.Scatter(
            x=[],
            y=[],
            text=[],
            fill='tonexty',
            mode='none',
            line={'shape': 'spline'},
            hoverinfo='text',
            name=character))

    sums = {}
    for trace in traces:
        for chapter in data:
            sums[chapter['chapter']] = sums.get(chapter['chapter'], 0) + chapter['rates'][trace['name']]
            trace['x'].append(chapter['chapter'])
            trace['y'].append(sums[chapter['chapter']])
            trace['text'].append(
                '{} - {}'.format(chapter['rates'][trace['name']], trace['name'])
                if chapter['rates'][trace['name']] else '')

    traces_with_liners = []
    for trace in traces:
        traces_with_liners.append(trace)
        traces_with_liners.append(go.Scatter(
            x=trace['x'],
            y=trace['y'],
            fill='tonexty',
            showlegend=False,
            line={'shape': 'spline'},
            hoverinfo='none',
            mode='none',
            fillcolor='#ffffff'
        ))

    return traces_with_liners

```

And returning to code from which we started — let's see the plot result:


```python
roman = idiot.from_json()

data = parse_parts(roman)
traces = prepare_data(data)
plot(traces)
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~boorstat/10.embed" height="525px" width="100%"></iframe>

Click on character in legend — and you'll see where it is on graph.