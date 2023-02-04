+++
title = "Exercism Solutions"
date = '2023-01-28'
description = 'Solutions to the various challenges on the exercism website.'
+++

# Exercism Solutions

These solutions are provided so that you can check your work or get an idea, if you're stuck. If you use them to cheat, understand that you're only cheating yourself. That goes for everything on this website.

## Python

### armstrong numbers
```python
import math

def is_armstrong_number(number):
    """
    A number is an "armstrong" number if the sum of the powers of it's length
    applied to each digit in the number adds up to the number
    
    :param number: int - returns True if number is an amrstrong
    """
    power = len(str(number))
    sums = []
    for i in str(number):
        sums.append(math.pow(int(i), power))
    if number == sum(sums):
        return True
    else:
        return False
```

### Ghost Gobble Arcade Game
```python
"""Functions for implementing the rules of the classic arcade game Pac-Man."""


def eat_ghost(power_pellet_active, touching_ghost):
    """Verify that Pac-Man can eat a ghost if he is empowered by a power pellet.

    :param power_pellet_active: bool - does the player have an active power pellet?
    :param touching_ghost: bool - is the player touching a ghost?
    :return: bool - can the ghost be eaten?
    """
    if (power_pellet_active is True) and (touching_ghost is True):
        return True
    else:
        return False



def score(touching_power_pellet, touching_dot):
    """Verify that Pac-Man has scored when a power pellet or dot has been eaten.

    :param touching_power_pellet: bool - is the player touching a power pellet?
    :param touching_dot: bool - is the player touching a dot?
    :return: bool - has the player scored or not?
    """
    if (touching_power_pellet is True) or (touching_dot is True):
        return True
    else:
        return False


def lose(power_pellet_active, touching_ghost):
    """Trigger the game loop to end (GAME OVER) when Pac-Man touches a ghost without his power pellet.

    :param power_pellet_active: bool - does the player have an active power pellet?
    :param touching_ghost: bool - is the player touching a ghost?
    :return: bool - has the player lost the game?
    """
    if (power_pellet_active is False) and (touching_ghost is True):
        return True
    else:
        return False


def win(has_eaten_all_dots, power_pellet_active, touching_ghost):
    """Trigger the victory event when all dots have been eaten.

    :param has_eaten_all_dots: bool - has the player "eaten" all the dots?
    :param power_pellet_active: bool - does the player have an active power pellet?
    :param touching_ghost: bool - is the player touching a ghost?
    :return: bool - has the player won the game?
    """
    if (has_eaten_all_dots is True) and (power_pellet_active is True) and (touching_ghost is True):
        return True
    elif (has_eaten_all_dots is True) and (power_pellet_active is False) and (touching_ghost is True):
        return False
    elif (has_eaten_all_dots is True) and (touching_ghost is False):
        return True
    else:
        return False
```

### Guido's Gorgeous Lasagna
```python
"""Functions used in preparing Guido's gorgeous lasagna.

Learn about Guido, the creator of the Python language: https://en.wikipedia.org/wiki/Guido_van_Rossum
"""

EXPECTED_BAKE_TIME = 40
PREPARATION_TIME = 2

def bake_time_remaining(elapsed_bake_time):
    """Calculate the bake time remaining.

    :param elapsed_bake_time: int - baking time already elapsed.
    :return: int - remaining bake time (in minutes) derived from 'EXPECTED_BAKE_TIME'.

    Function that takes the actual minutes the lasagna has been in the oven as
    an argument and returns how many minutes the lasagna still needs to bake
    based on the `EXPECTED_BAKE_TIME`.
    """

    return (EXPECTED_BAKE_TIME - elapsed_bake_time)


def preparation_time_in_minutes(layers):
    """Calculate the preparation time in minutes, depeding on the
    amount the desired number of layers.py

    :param layers: int - desired amount of layers
    :return int - preparation time in minutesm derived from 'LAYERS'
    """

    return (layers * PREPARATION_TIME)


def elapsed_time_in_minutes(layers, elapsed_bake_time):
    """
    Return elapsed cooking time.

    This function takes two numbers representing the number of layers & the time already spent
    baking and calculates the total elapsed minutes spent cooking the lasagna.
    """

    return (preparation_time_in_minutes(layers) + elapsed_bake_time)
```

### Currency Exchange
```python
def exchange_money(budget, exchange_rate):
    """

    :param budget: float - amount of money you are planning to exchange.
    :param exchange_rate: float - unit value of the foreign currency.
    :return: float - exchanged value of the foreign currency you can receive.
    """
    return budget / exchange_rate


def get_change(budget, exchanging_value):
    """

    :param budget: float - amount of money you own.
    :param exchanging_value: float - amount of your money you want to exchange now.
    :return: float - amount left of your starting currency after exchanging.
    """
    return budget - exchanging_value


def get_value_of_bills(denomination, number_of_bills):
    """

    :param denomination: int - the value of a bill.
    :param number_of_bills: int - amount of bills you received.
    :return: int - total value of bills you now have.
    """
    return denomination * number_of_bills


def get_number_of_bills(budget, denomination):
    """

    :param budget: float - the amount of money you are planning to exchange.
    :param denomination: int - the value of a single bill.
    :return: int - number of bills after exchanging all your money.
    """
    return budget // denomination


def get_leftover_of_bills(budget, denomination):
    """

    :param budget: float - the amount of money you are planning to exchange.
    :param denomination: int - the value of a single bill.
    :return: float - the leftover amount that cannot be exchanged given the current denomination.
    """
    return budget % denomination


def exchangeable_value(budget, exchange_rate, spread, denomination):
    """

    :param budget: float - the amount of your money you are planning to exchange.
    :param exchange_rate: float - the unit value of the foreign currency.
    :param spread: int - percentage that is taken as an exchange fee.
    :param denomination: int - the value of a single bill.
    :return: int - maximum value you can get.
    """
    return ((budget / (exchange_rate * (1 + spread / 100))) // denomination) * denomination
```

### Hello World!
```python
def hello():
    return 'Hello, World!'
```

### Two Fer
```python
def two_fer(name = "you"):
    return ("One for " + name + ", one for me.")
```

### Leap
```python
def leap_year(year):
    if year % 4 == 0:
        if year % 100 == 0 and year % 400 != 0:
            return False
        else:
            return True

    else:
        return False
```

### Meltdown Mitigation
```python
"""Functions to prevent a nuclear meltdown."""

def is_criticality_balanced(temperature, neutrons_emitted):
    """Verify criticality is balanced.

    :param temperature: int or float - temperature value in kelvin.
    :param neutrons_emitted: int or float - number of neutrons emitted per second.
    :return: bool - is criticality balanced?

    A reactor is said to be critical if it satisfies the following conditions:
    - The temperature is less than 800 K.
    - The number of neutrons emitted per second is greater than 500.
    - The product of temperature and neutrons emitted per second is less than 500000.
    """
    if temperature < 800:
        if neutrons_emitted > 500:
            if temperature * neutrons_emitted < 500000:
                return True
            else:
                return False
        else:
            return False
    else:
        return False


def reactor_efficiency(voltage, current, theoretical_max_power):
    """Assess reactor efficiency zone.

    :param voltage: int or float - voltage value.
    :param current: int or float - current value.
    :param theoretical_max_power: int or float - power that corresponds to a 100% efficiency.
    :return: str - one of ('green', 'orange', 'red', or 'black').

    Efficiency can be grouped into 4 bands:

    1. green -> efficiency of 80% or more,
    2. orange -> efficiency of less than 80% but at least 60%,
    3. red -> efficiency below 60%, but still 30% or more,
    4. black ->  less than 30% efficient.

    The percentage value is calculated as
    (generated power/ theoretical max power)*100
    where generated power = voltage * current
    """
    generated_power = voltage * current / theoretical_max_power * 100
    if generated_power >= 80:
        return "green"
    elif generated_power >= 60:
        return "orange"
    elif generated_power >= 30:
        return "red"
    else:
        return "black"

def fail_safe(temperature, neutrons_produced_per_second, threshold):
    """Assess and return status code for the reactor.

    :param temperature: int or float - value of the temperature in kelvin.
    :param neutrons_produced_per_second: int or float - neutron flux.
    :param threshold: int or float - threshold for category.
    :return: str - one of ('LOW', 'NORMAL', 'DANGER').

    1. 'LOW' -> `temperature * neutrons per second` < 90% of `threshold`
    2. 'NORMAL' -> `temperature * neutrons per second` +/- 10% of `threshold`
    3. 'DANGER' -> `temperature * neutrons per second` is not in the above-stated ranges
    """
    if temperature * neutrons_produced_per_second < threshold * 0.9:
        return 'LOW'
    elif 0.9 * threshold <= temperature * neutrons_produced_per_second <= 1.1 * threshold:
        return 'NORMAL'
    else:
        return 'DANGER'
```

### Triangle
```python
def equilateral(sides):
    if sides[0] == 0 or sides[1] == 0 or sides[2] == 0:
        return False
    return sides[0] == sides[1] == sides[2]


def isosceles(sides):
    if sides[0] == 0 or sides[1] == 0 or sides[2] == 0:
        return False
    if sides[0] == sides[1] == sides[2]:
        return True
    if sides[0] + sides[1] < sides[2]:
        return False
    elif sides[1] + sides[2] < sides[0]:
        return False
    elif sides[0] + sides[2] < sides[1]:
        return False
    else:
        if sides[0] == sides[1] != sides[2]:
            return True
        elif sides[1] == sides[2] != sides[0]:
            return True
        elif sides[0] == sides[2] != sides[1]:
            return True
        else:
            return False


def scalene(sides):
    if sides[0] == 0 or sides[1] == 0 or sides[2] == 0:
        return False
    if sides[0] + sides[1] < sides[2]:
        return False
    elif sides[1] + sides[2] < sides[0]:
        return False
    elif sides[0] + sides[2] < sides[1]:
        return False
    elif sides[0] == sides[2]:
        return False
    else:
        return sides[0] != sides[1] != sides[2]
```

### Collatz Conjecture
```python
def steps(number):
    count = 0
    if number <= 0:
        raise ValueError("Only positive integers are allowed")
    while number != 1:
        if number % 2 == 0:
            number /= 2
            count += 1
        else:
            number *= 3
            number += 1
            count += 1
    return count
```

### Difference of Squares
```python
def square_of_sum(number):
    squares = 0
    for i in range(number + 1):
        squares += i
    return(squares ** 2)


def sum_of_squares(number):
    sums = 0
    for i in range(number + 1):
        sums += i * i
    return(sums)


def difference_of_squares(number):
    return(square_of_sum(number) - sum_of_squares(number))
```


## Kotlin

### Collatz Calculator
```java
object CollatzCalculator {
  fun computeStepCount(start: Int): Int {
  var count = 0
  var run = start
  while (run != 1) {
	 if (run % 2 == 0) {
			count += 1
			run /= 2
		} else {
			count += 1
			run *= 3
			run += 1
      }
	 }
	return count
  }
}
```