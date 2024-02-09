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

## Basic Data Classes

```python
from dataclasses import dataclass

@dataclass
class Position:
    name: str
    lon: float
    lat: float
```

A data class is a regular Python class. The only thing that sets it apart is that it has basic data model methods like `.__init__()`, `.__repr__()`, and `.__eq__()` implemented for you.

### Default Values

```python
from dataclasses import dataclass

@dataclass
class Position:
    name: str
    lon: float = 0.0
    lat: float = 0.0

    
>>> Position('Null Island')
Position(name='Null Island', lon=0.0, lat=0.0)
>>> Position('Greenwich', lat=51.8)
Position(name='Greenwich', lon=0.0, lat=51.8)
>>> Position('Vancouver', -123.1, 49.3)
Position(name='Vancouver', lon=-123.1, lat=49.3)
```

### Type Hints

Adding some kind of type hint is mandatory when defining the fields in your data class. Without a type hint, the field will not be a part of the data class. However, if you do not want to add explicit types to your data class, use typing.Any:

```python
from dataclasses import dataclass
from typing import Any

@dataclass
class WithoutExplicitTypes:
    name: Any
    value: Any = 42
```

### Adding Methods

```python
from dataclasses import dataclass
from math import asin, cos, radians, sin, sqrt

@dataclass
class Position:
    name: str
    lon: float = 0.0
    lat: float = 0.0

    def distance_to(self, other):
        r = 6371  # Earth radius in kilometers
        lam_1, lam_2 = radians(self.lon), radians(other.lon)
        phi_1, phi_2 = radians(self.lat), radians(other.lat)
        h = (sin((phi_2 - phi_1) / 2)**2
             + cos(phi_1) * cos(phi_2) * sin((lam_2 - lam_1) / 2)**2)
        return 2 * r * asin(sqrt(h))
```

## More Flexible Data Classes

```python
from dataclasses import dataclass
from typing import List

@dataclass
class PlayingCard:
    rank: str
    suit: str

@dataclass
class Deck:
    cards: List[PlayingCard]

>>> queen_of_hearts = PlayingCard('Q', 'Hearts')
>>> ace_of_spades = PlayingCard('A', 'Spades')
>>> two_cards = Deck([queen_of_hearts, ace_of_spades])
Deck(cards=[PlayingCard(rank='Q', suit='Hearts'),
            PlayingCard(rank='A', suit='Spades')])
```

```python
RANKS = '2 3 4 5 6 7 8 9 10 J Q K A'.split()
SUITS = '♣ ♢ ♡ ♠'.split()

def make_french_deck():
    return [PlayingCard(r, s) for s in SUITS for r in RANKS]

>>> make_french_deck()
[PlayingCard(rank='2', suit='♣'), PlayingCard(rank='3', suit='♣'), ...
 PlayingCard(rank='K', suit='♠'), PlayingCard(rank='A', suit='♠')]
```

Data classes use something called a default_factory to handle mutable default values. 
To use default_factory (and many other cool features of data classes), you need to use the `field()` specifier:

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Deck:
    cards: List[PlayingCard] = field(default_factory=make_french_deck)
```

## Immutable Data Classes

One of the defining features of the namedtuple you saw earlier is that it is immutable. That is, the value of its fields may never change. For many types of data classes, this is a great idea! To make a data class immutable, set `frozen=True` when you create it. For example, the following is an immutable version of the Position class you saw earlier:

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Position:
    name: str
    lon: float = 0.0
    lat: float = 0.0
```

```python
>>> pos = Position('Oslo', 10.8, 59.9)
>>> pos.name
'Oslo'
>>> pos.name = 'Stockholm'
dataclasses.FrozenInstanceError: cannot assign to field 'name'
```

## Inheritance

```python
from dataclasses import dataclass

@dataclass
class Position:
    name: str
    lon: float
    lat: float

@dataclass
class Capital(Position):
    country: str
```




## Credits

- [Data Classes in Python 3.7+ (Guide)](https://realpython.com/python-data-classes/)
