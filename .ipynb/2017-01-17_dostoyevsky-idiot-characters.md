
### boor-system


```python
import sys
sys.path.append('/Users/baza/dev/python-boorstat/')

#from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
#init_notebook_mode(connected=True)
import plotly.plotly as py

import boorstat
```

### boor-title

Visualised Dostoyevsky Idiot characters activity

### boor-short-content

Let's jsonify and objectify great creation of Fyodor Dostoyevsky -- The Idiot.<br/>
Among the images there is example of the obtained structure usage.<br/>
![Image](https://github.com/boorstat/boorstat.github.io/raw/master/images/dostoyevsky-idiot-object.jpg)

More about the process http://example.com




### boor-full-content

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
Se we need to make some imports for it:


```python
import plotly.plotly as py
```

Also we need to import our own python package: python-boorstat.<br/>
It can be easily installed using pip.<br/>



```python
py.iplot({                      # use `py.iplot` inside the ipython notebook
    "data": [{
        "x": [1, 2, 3],
        "y": [4, 2, 5]
    }],
    "layout": {
        "title": "hello world"
    }
    }, filename='hello world',      # name of the file as saved in your plotly account
    sharing='public')            # 'public' | 'private' | 'secret': Learn more: https://plot.ly/python/privacy
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~boorstat/8.embed" height="525px" width="100%"></iframe>




```python

```
