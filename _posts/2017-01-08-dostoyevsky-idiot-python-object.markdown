---
layout: post
title:  "Dostoyevsky Idiot — python object"
date:   2017-01-08
categories: dostoyevsky idiot
---
We're going to jsonify and objectify great creation of Fyodor Dostoyevsky -- The Idiot.<br/>
We'll get well structured data ready for further experiments.<br/>
![Image](https://github.com/boorstat/boorstat.github.io/raw/master/images/dostoyevsky-idiot-object.jpg)

We have <a href="https://github.com/boorstat/boorstat-files/raw/master/lit/dostoevsky/The_Idiot.txt">"The Idiot" text</a>.<br/>
That's top level function to objectify this text:


```python
def objectify_idiot(url=TEXT_URL):
    text = requests.get(url).text

    idiot = {
        'title': 'The Idiot',
        'author': 'Fyodor Dostoyevsky',
        'text': text}

    part_seps = ['PART {}'.format(roman.toRoman(i)) for i in range(4, 0, -1)]
    part_seps.append('Copyright')

    parts = re.split('|'.join(part_seps), text)
    idiot['copyright'] = parts[-1].strip()

    parts = [p.strip() for p in parts[1:-1]]

    idiot['parts'] = [objectify_part(p, n + 1) for n, p in enumerate(parts)]

    return idiot
```

`objectify_part()` and others low level functions implementation <a href="https://github.com/boorstat/python-boorstat/blob/master/boorstat/lit/dostoyevsky/idiot/idiot.py">can be found here</a>.<br/>
Run `pip install git+https://github.com/boorstat/python-boorstat.git` to install python package with ready to use functions:


```python
from boorstat.lit.dostoyevsky.idiot import idiot

idiot_as_dict = idiot.objectify_idiot()
idiot_as_dict_from_pregenerated_json = idiot.from_json()
```

Final structure can be understood from <a href="https://github.com/boorstat/boorstat-files/raw/master/lit/dostoevsky/idiot.json">pregenerated json</a>.<br/>
Also this screenshot from debugger can clarify hierarchy inside "The Idiot" object:
![Image](https://github.com/boorstat/boorstat.github.io/raw/master/images/dostoyevsky-idiot-object-structure.png)
