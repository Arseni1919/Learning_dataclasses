# Data Classes in Python

```python
from dataclasses import dataclass

@dataclass
class DataClassCard:
    rank: str
    suit: str
```

You can instantiate, print, and compare data class instances straight out of the box:
```python
>>> queen_of_hearts = DataClassCard('Q', 'Hearts')
>>> queen_of_hearts.rank
'Q'
>>> queen_of_hearts
DataClassCard(rank='Q', suit='Hearts')
>>> queen_of_hearts == DataClassCard('Q', 'Hearts')
True
```


## Credits

- [Data Classes in Python 3.7+ (Guide)](https://realpython.com/python-data-classes/)
