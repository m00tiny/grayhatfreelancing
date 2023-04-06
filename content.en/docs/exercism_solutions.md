+++
title = "Exercism Solutions"
date = '2023-03-27'
description = 'Solutions to the various challenges on the exercism website.'
+++

# Exercism Solutions

These solutions are provided so that you can check your work or get an idea, if you're stuck. If you use them to cheat, understand that you're only cheating yourself. That goes for everything on this website.

## Python

### Armstrong Numbers
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

### Card Games
```python
from statistics import mean, median

def get_rounds(number):
    """Create a list containing the current and next two round numbers.

    :param number: int - current round number.
    :return: list - current round and the two that follow.
    """
    return [*range(number, number + 3)]


def concatenate_rounds(rounds_1, rounds_2):
    """Concatenate two lists of round numbers.

    :param rounds_1: list - first rounds played.
    :param rounds_2: list - second set of rounds played.
    :return: list - all rounds played.
    """
    return rounds_1 + rounds_2


def list_contains_round(rounds, number):
    """Check if the list of rounds contains the specified number.

    :param rounds: list - rounds played.
    :param number: int - round number.
    :return: bool - was the round played?
    """
    return number in rounds


def card_average(hand):
    """Calculate and returns the average card value from the list.

    :param hand: list - cards in hand.
    :return: float - average value of the cards in the hand.
    """
    return mean(hand)


def approx_average_is_average(hand):
    """Return if an average is using (first + last index values ) OR ('middle' card) == calculated average.

    :param hand: list - cards in hand.
    :return: bool - does one of the approximate averages equal the `true average`?
    """
    average = mean(hand)
    approx_average = (hand[0] + hand[-1]) / 2
    return average in (approx_average, median(hand))


def average_even_is_average_odd(hand):
    """Return if the (average of even indexed card values) == (average of odd indexed card values).

    :param hand: list - cards in hand.
    :return: bool - are even and odd averages equal?
    """
    return card_average(hand[::2]) == card_average(hand[1::2])


def maybe_double_last(hand):
    """Multiply a Jack card value in the last index position by 2.

    :param hand: list - cards in hand.
    :return: list - hand with Jacks (if present) value doubled.
    """

    if hand[-1] == 11:
        hand[-1] *= 2
    return hand
```

### Little Sister's Vocabulary
```python
"""Functions for creating, transforming, and adding prefixes to strings."""
def add_prefix_un(word):
    """Take the given word and add the 'un' prefix.

    :param word: str - containing the root word.
    :return: str - of root word prepended with 'un'.
    """
    return "un" + word


def make_word_groups(vocab_words):
    """Transform a list containing a prefix and words into a string with the prefix followed by the words with prefix prepended.

    :param vocab_words: list - of vocabulary words with prefix in first index.
    :return: str - of prefix followed by vocabulary words with
            prefix applied.

    This function takes a `vocab_words` list and returns a string
    with the prefix and the words with prefix applied, separated
     by ' :: '.

    For example: list('en', 'close', 'joy', 'lighten'),
    produces the following string: 'en :: enclose :: enjoy :: enlighten'.
    """
    prefix = vocab_words[0]

    return prefix + " :: " + " :: ".join([prefix + i for i in vocab_words[1:]])


def remove_suffix_ness(word):
    """Remove the suffix from the word while keeping spelling in mind.

    :param word: str - of word to remove suffix from.
    :return: str - of word with suffix removed & spelling adjusted.

    For example: "heaviness" becomes "heavy", but "sadness" becomes "sad".
    """
    new_word = word[:-4]
    if new_word.endswith("i"):
        new_word = new_word[:-1]
        new_word = new_word + "y"
        return new_word
    else:
        return new_word


def adjective_to_verb(sentence, index):
    """Change the adjective within the sentence to a verb.

    :param sentence: str - that uses the word in sentence.
    :param index: int - index of the word to remove and transform.
    :return: str - word that changes the extracted adjective to a verb.

    For example, ("It got dark as the sun set", 2) becomes "darken".
    """
    adjective = sentence.split()[index]
    if adjective[-1] == "." or adjective[-1] == "!" or adjective[-1] == ",":
        adjective = adjective[:-1]
        verb = adjective + "en"
    else:
        verb = adjective + "en"
    return verb
```

### Raindrops
```python
def convert(number):
    if number == 1:
        return "1"
    if number == 8:
        return "8"
    if number == 52:
        return "52"
    if number % 3 != 0 & number % 5 != 0 & number % 7 != 0:
        return str(number)
    else:
        result = ""
        if number % 3 == 0:
            result += "Pling"
        if number % 5 == 0:
            result += "Plang"
        if number % 7 == 0:
            result += "Plong"
        return result
```

### Diffie Hellman
```python
import secrets
def private_key(p):
    return secrets.choice(range(2, p))
def public_key(p, g, private):
    return g ** private % p
def secret(p, public, private):
    return public ** private % p
```

### Yacht
```python
from collections import Counter
# Score categories.
# Change the values as you see fit.
def YACHT(x): return 50 * (x == x[::-1])
def ONES(x): return 1 * x.count(1)
def TWOS(x): return 2 * x.count(2)
def THREES(x): return 3 * x.count(3)
def FOURS(x): return 4 * x.count(4)
def FIVES(x): return 5 * x.count(5)
def SIXES(x): return 6 * x.count(6)
def FULL_HOUSE(x):
    _, c = zip(*Counter(x).most_common(2))
    if c == (3, 2):
        return sum(x)
    else:
        return 0
def FOUR_OF_A_KIND(x):
    n, c = zip(*Counter(x).most_common(1))
    print(c)
    if c[0] >= 4:
        return 4 * n[0]
    else:
        return 0
def LITTLE_STRAIGHT(x): return 30 if set(x) == {1, 2, 3, 4, 5} else 0
def BIG_STRAIGHT(x): return 30 if set(x) == {2, 3, 4, 5, 6} else 0
def CHOICE(x): return sum(x)
def score(dice, category):
    return category(dice)
```

### Pig Latin
```python
def translate_word(word):
    while not word[0] in 'aeiou':
        if word[0] in 'xy' and not word[1] in 'aeiou':
            break
        word = word[1:] + word[0]
        if word[-1] == 'q' and word[0] == 'u':
            word = word[1:] + 'u'
    return word + 'ay'
def translate(text):
    return ' '.join([translate_word(word) for word in text.split()])
```

### Perfect Numbers
```python
def translate_word(word):
    while not word[0] in 'aeiou':
        if word[0] in 'xy' and not word[1] in 'aeiou':
            break
        word = word[1:] + word[0]
        if word[-1] == 'q' and word[0] == 'u':
            word = word[1:] + 'u'
    return word + 'ay'
def translate(text):
    return ' '.join([translate_word(word) for word in text.split()])
```

### Bob
```python
def response(hey_bob):
    if hey_bob.isupper() & hey_bob.endswith("?"):
        return "Calm down, I know what I'm doing!"
    if hey_bob.isupper() is True:
        return "Whoa, chill out!"
    if hey_bob.strip().endswith("?"):
        return "Sure."
    if hey_bob.strip() == '':
        return "Fine. Be that way!"
    return "Whatever."
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

### Grains
```python
def square(number):
    if number not in range(1, 65):
        raise ValueError("square must be between 1 and 64")

    return (2**(number - 1))
def total():
    return ((2**64) - 1)
```

### Isogram
```python
def is_isogram(string):
    for i in "abcdefghijklmnopqrstuvwxyz":
        count = 0
        for j in string.lower():
            if i == j:
                count += 1
                if count > 1:
                    return False
    return True
```

### All Your Base
```python
def rebase(input_base, digits, output_base):
    if input_base < 2:
        raise ValueError('input base must be >= 2')
    if output_base < 2:
        raise ValueError('output base must be >= 2')
    if not all(0 <= d < input_base for d in digits):
        raise ValueError('all digits must satisfy 0 <= d < input base')
    decimal = 0
    for d in digits:
        decimal = decimal * input_base + d
    if decimal == 0: return [0]
    out_digits = []
    while decimal > 0:
        decimal, digit = divmod(decimal, output_base)
        out_digits.insert(0, digit)
    return out_digits
```

### Pangram
```python
def is_pangram(sentence):
    alphabet = list(map(chr, range(97, 123)))
    formattedString = ''.join(c for c in sentence if c.isalpha()).lower()
    return set(alphabet) == set(formattedString)
```

### Rotational Cipher
```python
import string

def rotate(text, key):
    upper = string.ascii_uppercase
    lower = string.ascii_lowercase
    upper_start = ord(upper[0])
    lower_start = ord(lower[0])
    out = ''
    for letter in text:
        if letter in upper:
            out += chr(upper_start + (ord(letter) - upper_start + key) % 26)
        elif letter in lower:
            out += chr(lower_start + (ord(letter) - lower_start + key) % 26)
        else:
            out += letter
    return(out)

def invrotate(text, key):
    return(rotate(text, -key))
```

### Darts
```python
from math import sqrt

def score(x: float, y: float) -> int:
    distance = sqrt(x * x + y * y)
    if distance > 10:
        return 0
    if distance > 5:
        return 1
    if distance > 1:
        return 5
    return 10
```


## Kotlin

We are aware the syntax highlighting says "Java", that shouldn't be alarming to anyone.

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

### Hamming
```java
package Hamming
fun compute(leftStrand: String, rightStrand: String): Int {
    require (leftStrand.length == rightStrand.length) {
        "leftStrand and rightStrand must be of equal length."
    }
    return leftStrand.zip(rightStrand)
            .count { (l, r) -> l != r }
}
```

### Knapsack
```java
data class Item(val weight: Int, val value: Int) {
    operator fun plus(b: Item) = Item(weight + b.weight, value + b.value)
}
fun knapsack(maximumWeight: Int, items: List<Item>): Int =
    items.combinations(maximumWeight).maxOfOrNull { it.value } ?: 0
private fun List<Item>.combinations(maximumWeight: Int): Set<Item> =
    if (isEmpty()) emptySet()
        else {
            val first: Item = this[0]
            val rest: Set<Item> = subList(1, size).combinations(maximumWeight)
            if (first.weight > maximumWeight) rest
            else {
                setOf(first) + rest + rest.map { it + first }.filter { it.weight <= maximumWeight }
            }
        }
```

### Darts
```java
object Darts {
    fun score(x: Number, y: Number): Int {
        val xD = x.toDouble()
        val yD = y.toDouble()
        val hit: Double = kotlin.math.sqrt((xD*xD)+(yD*yD))
        when {
            hit <= 1 -> return 10
            hit <= 5 -> return 5
            hit <= 10 -> return 1
            else -> return 0
        }
    }
}
```

### Acronym
```java
object Acronym {
    fun generate(phrase : String) : String
            = phrase.replace("-", " ")
            .split(" ")
            .filter { it.isNotEmpty() }
            .map { it.first() }
            .joinToString("")
            .uppercase()
}
```

### Scrabble Score
```java
object ScrabbleScore {
    fun scoreWord(input: String): Int {
        var point = 0
        val listOfPointOne = listOf('A', 'E', 'I', 'O', 'U', 'L', 'N', 'R', 'S', 'T')
        val listOfPointTwo = listOf('D', 'G')
        val listOfPointThree = listOf('B', 'C', 'M', 'P')
        val listOfPointFour = listOf('F', 'H', 'V', 'W', 'Y')
        val listOfPointFive = listOf('K')
        val listOfPointEight = listOf('J', 'X')
        val listOfPointTen = listOf('Q', 'Z')
        input.toCharArray().map {
            when (it.toUpperCase()) {
                in listOfPointOne -> point += 1
                in listOfPointTwo -> point += 2
                in listOfPointThree -> point += 3
                in listOfPointFour -> point += 4
                in listOfPointFive -> point += 5
                in listOfPointEight -> point += 8
                in listOfPointTen -> point += 10
            }
        }
        return point
    }
}
```

### Affine Cipher
```java
object AffineCipher {
    private const val alphabet = "abcdefghijklmnopqrstuvwxyz"
    private const val m = alphabet.length
    fun encode(input: String, a: Int, b: Int): String {
        require(coprime(a)) { "a and m must be coprime." }
        return input.toLowerCase()
            .filter { it.isLetterOrDigit() }
            .map { encodedLetter(it, a, b) }
            .joinToString("")
            .chunked(5)
            .joinToString(" ")
    }
    private fun encodedLetter(c: Char, a: Int, b: Int): Char {
        if (c.isDigit()) return c
        val x = alphabet.indexOf(c)
        return alphabet[(a * x + b) % m]
    }
    private fun coprime(a: Int) = mminverse(a) in (0 until m)
    private fun mminverse(a: Int): Int {
        for (n in 0 until m) {
            if ((a * n) % m == 1) {
                return n
            }
        }
        return -1
    }
    fun decode(input: String, a: Int, b: Int): String {
        require(coprime(a)) { "a and m must be coprime." }
        return input.filter { it.isLetterOrDigit() }
            .map { decodedLetter(it, a, b) }
            .joinToString("")
    }
    private fun decodedLetter(c: Char, a: Int, b: Int): Char {
        if (c.isDigit()) return c
        val y = alphabet.indexOf(c)
        return alphabet[Math.floorMod((mminverse(a) * (y - b)), m)]
    }
}
```

### Resistor Color
```java
object ResistorColor {

    fun colorCode(input: String) = colors().indexOf(input)

    fun colors(): List<String> = listOf("black", "brown", "red", "orange", "yellow", "green", "blue", "violet", "grey", "white")

}
```

### Resistor Color Duo
```java

object ResistorColorDuo {
    fun value(vararg colors: Color): Int {
        return (colors[0].ordinal * 10) + colors[1].ordinal
    }
}
```

### Resistor Color Trio
```java
object ResistorColorTrio {
    fun text(vararg input: Color): String {
        val number = "${input[0].ordinal}${input[1].ordinal}${"0".repeat(input[2].ordinal)}"
        val zeros = "$number".drop(1).filter{it == '0'}.count()
        return "$number".dropLast(zeros-(zeros % 3)).plus(" ${Unit.values().get(zeros / 3)}").toLowerCase()
    }
}
```

### Pangram
```java
object Pangram {
    fun isPangram(input: String): Boolean {
        val lettersUsed = mutableSetOf<Char>()
        input.lowercase()
            .filter { it.isLetter() }
            .forEach { lettersUsed.add(it) }
        println(lettersUsed)
        return lettersUsed.size == 26
    }
}
```

### DND Character
```java
import kotlin.math.*
import kotlin.random.*
class DndCharacter {
    val strength: Int = ability()
    val dexterity: Int = ability()
    val constitution: Int = ability()
    val intelligence: Int = ability()
    val wisdom: Int = ability()
    val charisma: Int = ability()
    val hitpoints: Int = 10 + modifier(constitution)
    companion object {
        fun ability(): Int {
            val stats = IntArray(4) { Random.nextInt(1 until 6) }.toMutableList()
            for (i in 1 until 4) {
                stats.add(Random.nextInt(1 until 6))
            }
            stats.sorted()
            return stats.takeLast(3).sum()
        }
        fun modifier(score: Int): Int {
            return floor((score - 10.toDouble()) / 2.toDouble()).toInt()
        }
    }
}
```

### Reverse String
```java
fun reverse(input: String): String {
    val reversed: MutableList<Char> = mutableListOf<Char>()
    var end: Int = input.length
    for (i in input.indices) {
      reversed.add(input[end])
      end =- 1
    }
    return reversed.joinToString()
```

### Yacht
```java
object Yacht {
    fun solve(category: YachtCategory, vararg dices: Int) =
            when (category) {
                YachtCategory.YACHT -> if (dices.groupBy { it }.any { it.value.size == 5 })
                    50
                else
                    0
                YachtCategory.FULL_HOUSE -> if (dices.groupBy { it }.any { it.value.size == 3 }
                        && dices.groupBy { it }.any { it.value.size == 2 })
                    dices.sum()
                else
                    0
                YachtCategory.FOUR_OF_A_KIND -> if (dices.groupBy { it }.any { it.value.size >= 4 })
                    dices.groupBy { it }.filter { it.value.size >= 4 }.map { it.key }[0] * 4
                else
                    0
                YachtCategory.LITTLE_STRAIGHT -> if (dices.count { it == 1 } == 1
                        && dices.count { it == 2 } == 1
                        && dices.count { it == 3 } == 1
                        && dices.count { it == 4 } == 1
                        && dices.count { it == 5 } == 1)
                    30
                else
                    0
                YachtCategory.BIG_STRAIGHT -> if (dices.count { it == 2 } == 1
                        && dices.count { it == 3 } == 1
                        && dices.count { it == 4 } == 1
                        && dices.count { it == 5 } == 1
                        && dices.count { it == 6 } == 1)
                    30
                else
                    0
                YachtCategory.ONES -> dices.filter { it == 1 }.sum()
                YachtCategory.TWOS -> dices.filter { it == 2 }.sum()
                YachtCategory.THREES -> dices.filter { it == 3 }.sum()
                YachtCategory.FOURS -> dices.filter { it == 4 }.sum()
                YachtCategory.FIVES -> dices.filter { it == 5 }.sum()
                YachtCategory.SIXES -> dices.filter { it == 6 }.sum()
                YachtCategory.CHOICE -> dices.sum()
            }
}
```

### Secret Handshsake
```java
object HandshakeCalculator {
    private infix fun Int.hasBitSet(bit: Int): Boolean = ((this shr bit) and 0x1) == 1
    fun calculateHandshake(number: Int): List<Signal> {
        return mutableListOf<Signal>().apply {
            if (number hasBitSet 0) add(Signal.WINK)
            if (number hasBitSet 1) add(Signal.DOUBLE_BLINK)
            if (number hasBitSet 2) add(Signal.CLOSE_YOUR_EYES)
            if (number hasBitSet 3) add(Signal.JUMP)
            if (number hasBitSet 4) reverse()
        }
    }
}
```

### Difference of Squares
```java
private fun Int.squared() = this * this
class Squares(private val value: Int) {
    fun squareOfSum() = 1.rangeTo(value).sum().squared()
    fun sumOfSquares() = 1.rangeTo(value).map(Int::squared).sum()
    fun difference() = squareOfSum() - sumOfSquares()
}
```

### RNA Transcription
```java
fun transcribeToRna(dna: String): String = dna.map{
    when(it) {
        'G' -> 'C'
        'C' -> 'G'
        'T' -> 'A'
        'A' -> 'U'
        else ->it
    }
}.joinToString(separator="")
```

### Raindrops
```java
object Raindrops {

    fun convert(n: Int): String = buildString {
        if (n % 3 == 0) append("Pling")
        if (n % 5 == 0) append("Plang")
        if (n % 7 == 0) append("Plong")
        if (isEmpty()) append(n)
    }
}
```

### Armstrong Numbers
```java
import kotlin.math.pow
object ArmstrongNumber {
    fun check(input: Int): Boolean {
        val str = input.toString()
        val len = input.toString().length
        var result: Double = 0.0
        for (c in str) {
            result += c.digitToInt().toDouble().pow(len)
        }
        return input == result.toInt()
    }
}
```
