+++
title = "Exercism Solutions"
date = '2023-06-25'
description = 'Solutions to the various challenges on the exercism website.'
+++

# Exercism Solutions

These solutions are provided so that you can check your work or get an idea, if you're stuck. If you use them to cheat, understand that you're only cheating yourself. That goes for everything on this website.

## Python

### Pascal's Triangle
```python
def rows(row_count, previous_row=[1]):
    if row_count < 0:
        raise ValueError("number of rows is negative")
    elif row_count == 0:
        return []
    temp_row = previous_row + [0]
    new_row = list(map(sum, zip(temp_row, temp_row[::-1])))
    return [previous_row] + rows(row_count - 1, new_row)
```

### Resistor Colors
```python
COLORS = ["black", "brown", "red", "orange", "yellow", "green", "blue", "violet", "grey", "white"]
def color_code(color):
    return COLORS.index(color)


def colors():
    return COLORS
```

### Resistor Colors Trio
```python
import math

COLORS = ["black", "brown", "red", "orange", "yellow", "green", "blue", "violet", "grey", "white"]
PREFIX = [" ", " kilo", " mega", " giga"]

def label(colors):
    zeros = str(10 ** COLORS.index(colors[2]))[1:]
    ohms = int(str(COLORS.index(colors[0])) + str(COLORS.index(colors[1])) + zeros)
    thousands = 0 if ohms == 0 else int(math.log(ohms, 10)) // 3
    prefix = PREFIX[thousands]

    if thousands == 0:
        return  str(ohms) + prefix + "ohms"
    else:
        return  str(ohms)[:-thousands*3] + prefix + "ohms"
```

### Sublist
```python
SUBLIST = 1
SUPERLIST = 2
EQUAL = 3
UNEQUAL = 4
def sublist(list_one, list_two):
    if list_one == list_two:
        return EQUAL
    for i in range(len(list_one) - len(list_two) + 1):
        if list_one[i:i + len(list_two)] == list_two:
            return SUPERLIST
    for i in range(len(list_two) - len(list_one) + 1):
        if list_two[i:i + len(list_one)] == list_one:
            return SUBLIST
    return UNEQUAL
```

### Little Sister's Essay
```python
"""Functions to help edit essay homework using string manipulation."""
def capitalize_title(title):
    """Convert the first letter of each word in the title to uppercase if needed.

    :param title: str - title string that needs title casing.
    :return: str - title string in title case (first letters capitalized).
    """

    return title.title()


def check_sentence_ending(sentence):
    """Check the ending of the sentence to verify that a period is present.

    :param sentence: str - a sentence to check.
    :return: bool - return True if punctuated correctly with period, False otherwise.
    """

    return sentence[-1]=='.'


def clean_up_spacing(sentence):
    """Verify that there isn't any whitespace at the start and end of the sentence.

    :param sentence: str - a sentence to clean of leading and trailing space characters.
    :return: str - a sentence that has been cleaned of leading and trailing space characters.
    """

    return sentence.strip(' ')


def replace_word_choice(sentence, old_word, new_word):
    """Replace a word in the provided sentence with a new one.

    :param sentence: str - a sentence to replace words in.
    :param old_word: str - word to replace.
    :param new_word: str - replacement word.
    :return: str - input sentence with new words in place of old words.
    """

    return sentence.replace(old_word,new_word)
```

### ISBN Verifier
```python
def is_valid(isbn):
    nums = list(isbn.replace("-", ""))
    if len(nums)!=10:
        return False
    if nums[-1] == "X":
        nums[-1] = "10"
    if not all([c.isdigit() for c in nums]):
        return False
    return sum(int(x)*y for x,y in zip(nums, range(10, 0, -1))) % 11 == 0
```

### Chaitana's Colossal Coaster
```python
"""Functions to manage and organize queues at Chaitana's roller coaster."""

def add_me_to_the_queue(express_queue, normal_queue, ticket_type, person_name):
    """Add a person to the 'express' or 'normal' queue depending on the ticket number.

    :param express_queue: list - names in the Fast-track queue.
    :param normal_queue: list - names in the normal queue.
    :param ticket_type: int - type of ticket. 1 = express, 0 = normal.
    :param person_name: str - name of person to add to a queue.
    :return: list - the (updated) queue the name was added to.
    """
    

    if ticket_type == 1:
        express_queue.append(person_name)
        return express_queue
    else:
        normal_queue.append(person_name)
        return normal_queue


def find_my_friend(queue, friend_name):
    """Search the queue for a name and return their queue position (index).

    :param queue: list - names in the queue.
    :param friend_name: str - name of friend to find.
    :return: int - index at which the friends name was found.
    """

    return queue.index(friend_name)


def add_me_with_my_friends(queue, index, person_name):
    """Insert the late arrival's name at a specific index of the queue.

    :param queue: list - names in the queue.
    :param index: int - the index at which to add the new name.
    :param person_name: str - the name to add.
    :return: list - queue updated with new name.
    """
    
    queue.insert(index, person_name)
    return queue


def remove_the_mean_person(queue, person_name):
    """Remove the mean person from the queue by the provided name.

    :param queue: list - names in the queue.
    :param person_name: str - name of mean person.
    :return: list - queue update with the mean persons name removed.
    """

    queue.remove(person_name)
    return queue


def how_many_namefellows(queue, person_name):
    """Count how many times the provided name appears in the queue.

    :param queue: list - names in the queue.
    :param person_name: str - name you wish to count or track.
    :return: int - the number of times the name appears in the queue.
    
    """

    return queue.count(person_name)


def remove_the_last_person(queue):
    """Remove the person in the last index from the queue and return their name.

    :param queue: list - names in the queue.
    :return: str - name that has been removed from the end of the queue.
    """

    return queue.pop()


def sorted_names(queue):
    """Sort the names in the queue in alphabetical order and return the result.

    :param queue: list - names in the queue.
    :return: list - copy of the queue in alphabetical order.
    """

    queue.sort()
    return queue
```

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

### List Ops
```java
fun <T> List<T>.customAppend(list: List<T>): List<T> {
    return list.customFoldLeft(this) { newList, element -> newList + element }
}
fun List<Any>.customConcat(): List<Any> {
    return customFoldLeft(listOf()) { acc, element ->
        when (element) {
            is List<*> -> acc + (element as List<Any>).customConcat()
            else -> acc + element
        }
    }
}
fun <T> List<T>.customFilter(predicate: (T) -> Boolean): List<T> {
    return customFoldLeft(this) { newList, element ->
        if (predicate(element)) {
            newList
        } else newList - element
    }
}
val List<Any>.customSize: Int get() = this.customFoldLeft(0) { acc, _ -> acc + 1 }
fun <T, U> List<T>.customMap(transform: (T) -> U): List<U> {
    return customFoldLeft(listOf()) { newList, element -> newList + transform(element) }
}
fun <T, U> List<T>.customFoldLeft(initial: U, f: (U, T) -> U): U {
    if (this.isEmpty()) {
        return initial
    } else {
        return drop(1).customFoldLeft(f(initial, this.first()), f)
    }
}
fun <T, U> List<T>.customFoldRight(initial: U, f: (T, U) -> U): U {
    if (this.isEmpty()) {
        return initial
    } else {
        return f(this.first(), drop(1).customFoldRight(initial, f))
    }
}
fun <T> List<T>.customReverse(): List<T> {
    if (this.isEmpty()) {
        return emptyList()
    } else {
        val initial: List<T> = listOf()
        return customFoldLeft(initial) { newList, element -> listOf(element) + newList }
    }
}
```

### Diffie Hellman
```java
import java.math.BigInteger
import kotlin.random.Random
object DiffieHellman {
    fun privateKey(prime: BigInteger): BigInteger =
        BigInteger.valueOf(1L + Random.nextLong(prime.longValueExact() - 1))
    fun publicKey(p: BigInteger, g: BigInteger, privateKey: BigInteger): BigInteger =
        g.modPow(privateKey, p)
    fun secret(p: BigInteger, publicKey: BigInteger, privateKey: BigInteger): BigInteger =
        publicKey.modPow(privateKey, p)
}
```

### Sublist
```java
fun <T> List<T>.relationshipTo(list: List<T>): Relationship {
    return if (this.size < list.size) {
        if (isSublist(list, this)) Relationship.SUBLIST else Relationship.UNEQUAL
    } else if (this.size > list.size) {
        if (isSublist(this, list)) Relationship.SUPERLIST else Relationship.UNEQUAL
    } else {
        if (equals(this, list)) Relationship.EQUAL else Relationship.UNEQUAL
    }
}
private fun <T> isSublist(list: List<T>, subList: List<T>): Boolean {
    var fromIndex = 0
    while (fromIndex <= list.size - subList.size) {
        if (isSublist(fromIndex, list, subList)){
            return true
        }
        fromIndex++
    }
    return false
}
private fun <T> isSublist(fromIndex: Int, list: List<T>, subList: List<T>): Boolean {
    for (i in subList.indices) {
        if (list[i + fromIndex] != subList[i]) {
            return false
        }
    }
    return true
}
private fun <T> equals(l1: List<T>, l2: List<T>): Boolean {
    for (i in l1.indices) {
        if (l1[i]!! != l2[i]) {
            return false
        }
    }
    return true
}
enum class Relationship {
    EQUAL, SUBLIST, SUPERLIST, UNEQUAL
}
```

### Simple Cipher
```java
private val letters = ('a'..'z').map { it }
private fun randomString() = (1..100).let { nums ->
  val rand = java.util.Random()
  nums.map { letters[0] + rand.nextInt(letters.size) }.joinToString("")
}
class Cipher(val key: String = randomString()) {
  private val shift: Array<Int>
  init {
    require(key.isNotEmpty() && key.all { it in letters })
    shift = key.fold(emptyArray<Int>()) { acc, v ->
      acc + (v - letters[0])
    }
  }
  fun translate(text: String, f: (Int) -> Int): String {
    require(text.all { it in letters })
    return text.foldIndexed(StringBuffer()) { idx, acc, c ->
      acc.append(letters[(c - letters[0] + f(shift[idx])) % letters.size])
    }.toString()
  }
  fun encode(plainText: String) = translate(plainText) { it }
  fun decode(cipherText: String) = translate(cipherText) { letters.size - it }
}
```

### Say
```java
class NumberSpeller {
    fun say(input: Long): String {
        require(input in 0..999_999_999_999)
        if (input == 0L) return "zero"
        val sliced = input.slice()
        val len = sliced.size
        return sliced.withIndex().asSequence().flatMap { (i, n) ->
            val eng = n.toEnglish().asSequence()
            when {
                n == 0L -> sequenceOf()
                i == len - 1 -> eng
                else -> eng + phrases[len - i - 2]
            }
        }.filter { it.isNotEmpty() }.joinToString(" ")
    }
    companion object {
        val phrases = listOf("thousand", "million", "billion")
        val smalls = listOf(
            "one",
            "two",
            "three",
            "four",
            "five",
            "six",
            "seven",
            "eight",
            "nine",
            "ten",
            "eleven",
            "twelve"
        )
        val tens = listOf(
            "twenty",
            "thirty",
            "forty",
            "fifty",
            "sixty",
            "seventy",
            "eighty",
            "ninety"
        )
    }
    private fun Long.slice(): List<Long> {
        var i = this
        val r = mutableListOf<Long>()
        while (i > 0) {
            r.add(0, i % 1000)
            i /= 1000
        }
        return r
    }
    private fun Long.toEnglish(): List<String> {
        require(this < 1000)
        val r = mutableListOf<String>()
        var n = this
        if (n >= 100) {
            r.add("${(this / 100).mapSmall()} hundred")
            n %= 100
        }
        r.add(
            when {
                n <= 12 -> n.mapSmall()
                n in (13..19) -> "${(n % 10).mapSmall()}teen"
                n % 10 == 0L -> tens[n.toInt() / 10 - 2]
                else -> "${(tens[n.toInt() / 10 - 2])}-${(n % 10).mapSmall()}"
            }
        )
        return r
    }
    private fun Long.mapSmall(): String {
        require(this < 10)
        return if (this == 0L) "" else smalls[this.toInt() - 1]
    }
}
```

### Rail Fence Cipher
```java
typealias Point = Pair<Int,Int>
class RailFenceCipher(private val rails: Int) {
  private fun mkPairs(len: Int): List<Point> {
    val xs = List(len) { it }
    val ys = List(rails) { it }
        .let { it + it.reversed().drop(1).dropLast(1) }
        .let { generateSequence { it } }
        .flatten()
    return xs zip ys.take(xs.size).toList()
  }
  private val fst = { p: Point -> p.first }
  private val snd = { p: Point -> p.second }
  private fun crypt(text: String, sort1: (Point) -> Int, sort2: (Point) -> Int): String {
    val cs = text.replace("\\s".toRegex(), "").toList()
    val zig = mkPairs(cs.size).sortedBy(sort1) zip cs
    return zig.sortedBy { (v,_) -> sort2(v) }
        .map { it.second }
        .joinToString("")
  }
  fun getEncryptedData(plainText: String) = crypt(plainText, fst, snd)
  fun getDecryptedData(cipherText: String) = crypt(cipherText, snd, fst)
}
```

### Atbash Cipher
```java
object Atbash {
    private val alphabet = ('a'..'z').toList().joinToString("")
    private val numbers = (0..9).toList().joinToString("")
    private val e = alphabet + numbers
    private val er = alphabet.reversed() + numbers
    private var map: Map<Char, Char> = e.mapIndexed { i, c ->  c to er[i] }.toMap()
    private val reversedMap = map.entries.associateBy({ it.value }) { it.key }
    private fun cifra(input: String, map: Map<Char,Char>): String = input
            .filter { it.isLetterOrDigit() }
            .toLowerCase()
            .map { map[it] }.joinToString("")
    fun encode(input: String): String = cifra(input, map).chunked(5).joinToString(" ")
    fun decode(input: String): String = cifra(input, reversedMap)
}
```

### Grade School
```java
class School {
    private val students = mutableMapOf<Int, MutableList<String>>()
    fun add(student: String, grade: Int) = students.getOrPut(grade) { mutableListOf() }.add(student)
    fun grade(grade: Int): List<String> = students[grade]?.sorted() ?: emptyList()
    fun roster(): List<String> = students.toList().sortedBy { it.first }.flatMap { it.second.sorted() }
}
```

### ETL
```java
object ETL {
    fun transform(source: Map<Int, Collection<Char>>): Map<Char, Int> {
        val result = mutableMapOf<Char, Int>()
        source.forEach { (score, chars) ->
            chars.forEach { c -> result[c.lowercaseChar()] = score }
        }
        return result
    }
}
```

### Binary Search Tree
```java
class BinarySearchTree<T : Comparable<T>> {
    data class Node<T>(val data: T, var left: Node<T>?, var right: Node<T>?)
    var root: Node<T>? = null
    fun insert(value: T) {
        root = insertHelper(value, root)
    }
    private fun insertHelper(value: T, node: Node<T>?): Node<T> {
        when {
            node == null -> return Node(value, null, null)
            value > node.data -> node.right = insertHelper(value, node.right)
            else -> node.left = insertHelper(value, node.left)
        }
        return node
    }
    fun asSortedList(): List<T> {
        return sortedHelper(root!!)
    }
    private fun sortedHelper(node: Node<T>?): List<T> {
        return if (node == null) {
            emptyList()
        } else {
            sortedHelper(node.left) + listOf(node.data) + sortedHelper(node.right)
        }
    }
    fun asLevelOrderList(): List<T> {
        val nodes = mutableListOf(root)
        var i = 0
        while (i < nodes.size) {
            if (nodes[i]?.left != null) nodes.add(nodes[i]?.left)
            if (nodes[i]?.right != null) nodes.add(nodes[i]?.right)
            i++
        }
        return nodes.filterNotNull().map { it.data }
    }
}
```

### Change
```java
class ChangeCalculator(val coins: List<Int>) {
    private val sorted = coins.sorted()
    fun computeMostEfficientChange(grandTotal: Int): List<Int> {
        require(grandTotal >= 0) { "Negative totals are not allowed." }
        var changes = (1..grandTotal).fold(listOf<List<Int>?>(listOf<Int>())){ result, amount ->
            coins
                .filter { result.getOrNull(amount - it) != null }
                .map { listOf(it) + result.get(amount - it)!!  }
                .sortedBy { it.size }
                .firstOrNull()
                .let { result.plusElement(it) }
        }
        return requireNotNull(changes.lastOrNull()) {
            "The total $grandTotal cannot be represented in the given currency.."
        }
    }
}
```

### Spiral Matrix
```java
private class Spiral(private val size: Int) {
    val matrix = Array(size) { Array(size) { 0 } }
    var value = 1
    var verticalOffset = 0
    var horizontalOffset = 0
    init {
        fillArray()
    }
    private fun upwards() {
        for (index in matrix.size - 1 downTo 0) {
            insertAtPosition(index, horizontalOffset)
        }
    }
    private fun backward() {
        val backwardsIndex = size - 1 - verticalOffset
        for (index in matrix[backwardsIndex].size - 1 downTo 0) {
            insertAtPosition(backwardsIndex, index)
        }
    }
    private fun downward() {
        matrix.forEachIndexed { index, _ ->
            insertAtPosition(index, size - 1 - horizontalOffset)
        }
    }
    private fun forward() {
        matrix[verticalOffset].forEachIndexed { index, _ ->
            insertAtPosition(verticalOffset, index)
        }
    }
    private fun insertAtPosition(index1: Int, horizontalOffset1: Int) {
        if (matrix[index1][horizontalOffset1] == 0) {
            matrix[index1][horizontalOffset1] = value
            value++
        }
    }
    private fun fillArray() {
        while (value <= (size * size)) {
            forward()
            downward()
            backward()
            verticalOffset++
            upwards()
            horizontalOffset++
        }
    }
}
object SpiralMatrix {
    fun ofSize(size: Int): Array<Array<Int>> {
        if (size == 1)
            return arrayOf(arrayOf(1))
        if (size == 0)
            return emptyArray()
        return Spiral(size).matrix
    }
}
```

### Matching Brackets
```java
import java.util.*
object MatchingBrackets {
    fun isValid(input: String): Boolean {
        val stack = Stack<Char>()
        try {
            input.forEach { c ->
                if (c in listOf('[', '{', '('))
                    stack.push(c)
                if (c in listOf(']', '}', ')')) {
                    val top = stack.pop()
                    if (top == '[' && c != ']')
                        return false
                    else if (top == '(' && c != ')')
                        return false
                    else if (top == '{' && c != '}')
                        return false
                }
            }
        }catch (_: Exception){
            return false
        }
        return stack.isEmpty()
    }
}
```


### All Your Base
```java
class BaseConverter(val base: Int, val digits: IntArray) {
    val number: Int
    init {
        require(base > 1) { "Bases must be at least 2." }
        require(digits.isNotEmpty()) { "You must supply at least one digit." }
        require(digits[0] > 0 || digits.size < 2) { "Digits may not contain leading zeros." }
        require(digits.all { it >= 0 }) { "Digits may not be negative." }
        require(digits.all { it < base }) { "All digits must be strictly less than the base." }
        number = digits.fold(0) { sum: Int, d: Int -> sum * base + d }
    }
    fun convertToBase(nb: Int): IntArray {
        require(nb > 1) { "Bases must be at least 2." }
        if (number == 0) return IntArray(1)
        var current = number
        return generateSequence { current % nb }.takeWhile {
            current /= nb; current > 0 || it > 0
        }.toList().reversed().toIntArray()
    }
}
```

### Complex Numbers
```java
import kotlin.math.*
data class ComplexNumber(val real: Double = 0.0, val imag: Double = 0.0) {
  val abs = sqrt(real * real + imag * imag)
}
fun ComplexNumber.conjugate() = ComplexNumber(this.real, -this.imag)
fun exponential(c: ComplexNumber): ComplexNumber = c.let { (a, b) ->
  if (a == 0.0)
    ComplexNumber(cos(b), sin(b))
  else
    ComplexNumber(E.pow(a)) * exponential(ComplexNumber(imag=b))
}
operator fun ComplexNumber.times(other: ComplexNumber): ComplexNumber {
  val (a, b, c, d) = t4(this, other)
  return ComplexNumber(a * c - b * d, b * c + a * d)
}
operator fun ComplexNumber.plus(other: ComplexNumber): ComplexNumber {
  val (a, b, c, d) = t4(this, other)
  return ComplexNumber(a + c, b + d)
}
operator fun ComplexNumber.minus(other: ComplexNumber): ComplexNumber {
  val (a, b, c, d) = t4(this, other)
  return ComplexNumber(a - c, b - d)
}
operator fun ComplexNumber.div(other: ComplexNumber): ComplexNumber {
  val (a, b, c, d) = t4(this, other)
  return ComplexNumber(
      (a * c + b * d) / (c * c + d * d),
      (b * c - a * d) / (c * c + d * d))
}
private data class Tuple4(
    val a: Double,
    val b: Double,
    val c: Double,
    val d: Double)
private fun t4(x: ComplexNumber, y: ComplexNumber): Tuple4 {
  val (a, b) = x
  val (c, d) = y
  return Tuple4(a, b, c, d)
}
```

### Prime Factors
```java
object PrimeFactorCalculator {
    fun <T> primeFactors(num: T): List<T> where T : Number {
        val list = mutableListOf<T>()
        var number = num.toLong()
        val primeSequence = (2..Long.MAX_VALUE).asSequence().filter { x -> !(2..x / 2).any { x.rem(it) == 0L } }.iterator()
        while (number > 1L) {
            val primeNumber = primeSequence.next()
            while (number.rem(primeNumber) == 0L) {
                list += primeNumber.convert(num)
                number = number.div(primeNumber)
            }
        }
        return list
    }
    private fun <T> Long.convert(input: T): T {
        return when (input) {
            is Int -> this.toInt()
            is Double -> this.toDouble()
            else -> this
        } as T
    }
}
```

### Pascal's Triangle
```java
object PascalsTriangle {
  fun computeTriangle(rows: Int): List<List<Int>> {
    require(rows >= 0) { "Rows can't be negative!" }
    return generateSequence(listOf(1)) { prev ->
      listOf(1) + prev.windowed(2).map { it.sum() } + listOf(1)
    }.take(rows).toList()
  }
}
```

### Nth Prime
```java
private fun Int.sqrt() = Math.sqrt(this.toDouble()).toInt()
object Prime {
    fun nth(n: Int): Int {
        require(n > 0) { "There is no zeroth prime." }
        return generateSequence(1, { it + 1 })
            .filterNot { value -> (2..value.sqrt()).any { value % it == 0 } }
            .elementAt(n)
    }
}
```

### Kindergarten Garden
```java
class KindergartenGarden(private val diagram: String) {
    private val children = """
        Alice, Bob, Charlie, David,
        Eve, Fred, Ginny, Harriet,
        Ileana, Joseph, Kincaid, Larry
    """.trimIndent()
        .split(Regex("\\W+"))
        .sorted()
    private val ltrToPlant = mapOf(
        'V' to "violets",
        'R' to "radishes",
        'C' to "clover",
        'G' to "grass"
    )
    private val plants = diagram
        .lines()
        .map { it.map(ltrToPlant::get).filterNotNull().chunked(2) }
        .let { (fst, snd) -> fst.zip(snd) { a, b -> a + b }  }
    fun getPlantsOfStudent(student: String): List<String> {
        return children
            .indexOf(student)
            .let { plants[it] }
    }
}
```

### Robot Simulator
Robot.kt
```java
import Orientation.*
class Robot (var gridPosition: GridPosition = GridPosition(0,0),
             var orientation: Orientation = NORTH){
    fun turnRight() {
        orientation  = values()[(orientation.ordinal + 1) % 4]
    }
    fun turnLeft() {
        val new  =
                when (orientation) {
                    NORTH -> WEST.ordinal
                    else -> orientation.ordinal - 1
                }
        orientation = values()[new]
    }
    fun advance() {
        gridPosition =
                when (orientation) {
                    NORTH -> GridPosition(gridPosition.x, gridPosition.y + 1)
                    EAST -> GridPosition(gridPosition.x + 1, gridPosition.y)
                    SOUTH -> GridPosition(gridPosition.x, gridPosition.y - 1)
                    WEST -> GridPosition(gridPosition.x - 1, gridPosition.y)
                }
    }
    fun simulate(s: String) {
        s.forEach {
            when (it) {
                'L' -> turnLeft()
                'R' -> turnRight()
                'A' -> advance()
            }
        }
    }
}
```

### Grains
```java
package Board
import java.math.BigInteger
const val FIRST_SQUARE = 1
const val LAST_SQUARE = 64
fun requireValidSquare(n: Int) =
    require(n in FIRST_SQUARE .. LAST_SQUARE) { "Only integers between 1 and 64 (inclusive) are allowed" }
fun getGrainCountForSquare(n: Int): BigInteger {
    requireValidSquare(n)
    return getGrainCountForValidSquare(n)
}
private fun getGrainCountForValidSquare(n: Int) = BigInteger.valueOf(2).pow(n - 1)
fun getTotalGrainCount() =
    (FIRST_SQUARE .. LAST_SQUARE)
    .fold(BigInteger.ZERO) { sum, n ->
        sum.add(getGrainCountForValidSquare(n))
    }
```

### Forth
```java
class Forth {
    private val words = mutableListOf<String>()
    private val stack = mutableListOf<Int>()
    private val procedureTitles = mutableListOf<String>()
    private val procedureOps = mutableListOf<String>()
    private var recursiveCall = false
    private var recursiveIndex = 0
    fun evaluate(vararg line: String): List<Int> {
        var newWordFound = false
        var titleFound = false
        var stringToAdd = ""
        line.forEach {
            val wordLine = it.split(' ')
            val newLine = wordLine.map {x ->
                x.lowercase()
            }
            words.addAll(newLine)
        }
        for (command in words) {
            if (command.all { it.isDigit() } && !newWordFound) {
                stack.add(command.toInt())
            }
            else {
                if (command.length == 1 && command != ":" && command != ";"
                    && !command.all { it.isDigit() } && !newWordFound) {
                    operation(command)
                }
                else if (command == ":" && !newWordFound){
                    newWordFound = true
                }
                else if (newWordFound){
                    if (!titleFound){
                        if (command.all { it.isDigit() }){
                            throw Exception("illegal operation")
                        }
                        else {
                            procedureTitles.add(command)
                            titleFound = true
                        }
                    }
                    else{
                        if (command != ";")
                           stringToAdd += " $command"
                        else{
                            procedureOps.add(stringToAdd.removePrefix(" "))
                            stringToAdd = ""
                            titleFound = false
                            newWordFound = false
                        }
                    }
                }
                else if (command.length > 1) {
                    stackOperation(command)
                }
            }
        }
        return stack
    }
    private fun operation (action: String) {
        if (procedureTitles.contains(action))
            otherOpFound(action)
        else {
            when (action) {
                "+" -> {
                    if (stack.size >= 2) {
                        val newValue = stack[0] + stack[1]
                        stack.removeAt(0)
                        stack.removeAt(0)
                        stack.add(0, newValue)
                    } else {
                        if (stack.size == 0)
                            throw Exception("empty stack")
                        else
                            throw Exception("only one value on the stack")
                    }
                }
                "-" -> {
                    if (stack.size >= 2) {
                        val newValue = stack[0] - stack[1]
                        stack.removeAt(0)
                        stack.removeAt(0)
                        stack.add(0, newValue)
                    } else {
                        if (stack.size == 0)
                            throw Exception("empty stack")
                        else
                            throw Exception("only one value on the stack")
                    }
                }
                "*" -> {
                    if (stack.size >= 2) {
                        val newValue = stack[0] * stack[1]
                        stack.removeAt(0)
                        stack.removeAt(0)
                        stack.add(0, newValue)
                    } else {
                        if (stack.size == 0)
                            throw Exception("empty stack")
                        else
                            throw Exception("only one value on the stack")
                    }
                }
                "/" -> {
                    if (stack.size >= 2) {
                        if (stack[1] != 0) {
                            val newValue = stack[0] / stack[1]
                            stack.removeAt(0)
                            stack.removeAt(0)
                            stack.add(0, newValue)
                        } else
                            throw Exception("divide by zero")
                    } else {
                        if (stack.size == 0)
                            throw Exception("empty stack")
                        else
                            throw Exception("only one value on the stack")
                    }
                }
                else -> println("Error! No operation sent!")
            }
        }
    }
    private fun stackOperation (action: String) {
        if (procedureTitles.contains(action))
            otherOpFound(action)
        else{
        when (action) {
            "dup" -> {
                if (stack.size >= 1) {
                    stack.add(stack.last())
                } else
                    throw Exception("empty stack")
            }
            "drop" -> {
                if (stack.size >= 1) {
                    stack.removeAt(stack.lastIndex)
                } else
                    throw Exception("empty stack")
            }
            "swap" -> {
                if (stack.size >= 2) {
                    val newValues = mutableListOf(stack.removeAt(stack.lastIndex))
                    newValues.add(stack.removeAt(stack.lastIndex))
                    stack.addAll(newValues)
                } else {
                    if (stack.size == 0)
                        throw Exception("empty stack")
                    else
                        throw Exception("only one value on the stack")
                }
            }
            "over" -> {
                if (stack.size >= 2) {
                    stack.add(stack[stack.lastIndex - 1])
                } else {
                    if (stack.size == 0)
                        throw Exception("empty stack")
                    else
                        throw Exception("only one value on the stack")
                }
            }
            else -> {
                if (procedureTitles.contains(action))
                    otherOpFound(action)
                else
                    throw Exception("undefined operation")
            }
        }
        }
    }
    private fun otherOpFound (word: String) {
        val indexToUse: Int
        if (!recursiveCall) {
            if (procedureTitles.contains(word))
                indexToUse = procedureTitles.lastIndexOf(word)
            else {
                throw Exception(
                    "Procedure not found!!! word is $word"
                )
            }
        }
        else{
            if (procedureTitles.slice(0..recursiveIndex).contains(word)) {
                recursiveCall = false
                indexToUse = procedureTitles.slice(0..recursiveIndex).lastIndexOf(word)
            }
            else {
                throw Exception(
                    "Procedure not found!!! Word $word has not been defined before ${procedureTitles[recursiveIndex]}"
                )
            }
        }
        val procedureWords = procedureOps[indexToUse].split(" ")
        for (command in procedureWords){
            if (command.all { it.isDigit() }) {
                    stack.add(command.toInt())
                }
            else {
                if (command.length == 1 && command != "!") {
                    operation(command)
                }
                else if (procedureTitles.contains(command)){
                    recursiveCall = true
                    stackOperation(command)
                }
                else {
                    stackOperation(command)
                }
            }
        }
    }
}
```

### Sum of Multiples
```java
object SumOfMultiples {
    fun sum(divisors: Set<Int>, limit: Int) = (1 until limit).filter { n ->
        n.isDivisible(divisors.filter { d -> d > 0 }.toSet()) }.sum()
    private fun Int.isDivisible(divisors: Set<Int>): Boolean = divisors.any { this % it == 0 }
}
```

### Sieve
```java
import kotlin.math.sqrt
object Sieve {
    fun primesUpTo(max: Int): List<Int> {
        var numbers = (2..max).toList()
        val primes = mutableListOf<Int>()
        while (numbers.isNotEmpty()) {
            if (numbers.first() > sqrt(max.toDouble())) {
                primes.addAll(numbers)
                break
            }
            primes.add(numbers.first())
            numbers = numbers.filter { it % numbers.first() != 0 }
        }
        return primes
    }
}
```

### Perfect Numbers
```java
enum class Classification { DEFICIENT, PERFECT, ABUNDANT }
fun classify(naturalNumber: Int) = naturalNumber
        .let { require(it > 0); it }
        .compareTo(naturalNumber.factors.sum())
        .let {
            when {
                it < 0 -> Classification.ABUNDANT
                it == 0 -> Classification.PERFECT
                else -> Classification.DEFICIENT
            }
        }
private val Int.factors: List<Int>
    get() = (1..this / 2).fold(emptyList()) { factors, i -> if (this % i == 0) factors.plus(i) else factors }
```

### Dominoes
```java
class ChainNotFoundException : RuntimeException()
data class Domino(val left: Int, val right: Int)
object Dominoes {
    fun formChain(vararg xs: Domino) = formChain(xs.toList())
    fun formChain(xs: List<Domino>): List<Domino> =
        if (xs.isEmpty())
            emptyList()
        else {
            val ys = mutableListOf(listOf(xs.first()))
            go(ys, xs.drop(1))
            ys.removeIf { it.size != xs.size || it.first().left != it.last().right }
            if (ys.isEmpty()) throw ChainNotFoundException() else ys.first()
        }
    private fun go(acc: MutableList<List<Domino>>, pool: List<Domino>) {
        if (pool.isEmpty()) return
        val xs = acc.last()
        val last = xs.last()
        pool.filter { last.right == it.left || last.right == it.right }
            .forEach {
                acc.add(xs.plus(if (last.right == it.left) it else Domino(it.right, it.left)))
                go(acc, pool.minus(it))
            }
    }
}
```

### Minesweeper
```java
class MinesweeperBoard(val inputBoard: List<String>) {
  fun withNumbers(): List<String> =
    inputBoard.mapIndexed { row, str ->
      str.mapIndexed { col, cell -> when (cell) {
          ' ' -> adjacentMines(row, col)
          else -> cell
        }
      }.joinToString("")
    }
  private fun adjacentMines(row: Int, col: Int): Char {
    val count =
      (Math.max(0, row - 1)..Math.min(inputBoard.size - 1, row + 1))
        .flatMap { i ->
          (Math.max(0, col - 1)..Math.min(inputBoard[i].length - 1, col + 1))
            .map { j ->
              if (inputBoard[i][j] == '*') 1 else 0
            }
        }
        .sum()
    return if (count > 0) (count + 48).toChar() else ' '
  }
}
```

### Scale Generator
```java
class Scale(private val tonic: String) {
    private val chromatic = if (tonic.getOrNull(1) == '#') {
        listOf("C", "C#", "D", "D#", "E", "F", "F#", "G", "G#", "A", "A#", "B")
    } else {
        listOf("F", "Gb", "G", "Ab", "A", "Bb", "B", "C", "Db", "D", "Eb", "E")
    }.run { dropWhile { it.toUpperCase() != tonic.toUpperCase() } + takeWhile { it.toUpperCase() != tonic.toUpperCase() } }
    fun chromatic(): List<String> = chromatic
    fun interval(intervals: String): List<String> =
        intervals.scan(0) { x, y -> x + "mMA".indexOf(y) + 1 }.mapNotNull { chromatic.getOrNull(it) }
}
```

### Run Length Encoding
```java
object RunLengthEncoding {
    fun encode(input: String): String =
        input.replace(Regex("(.)\\1+")) {
            String.format("%d%s", it.value.length, it.groupValues[1])
        }
    fun decode(input: String): String =
        input.replace(Regex("(\\d+)(.)")) {
            it.groupValues[2].repeat(it.groupValues[1].toInt())
        }
}
```

### Crypto Square
```java
import kotlin.math.ceil
import kotlin.math.sqrt
object CryptoSquare {
    fun ciphertext(plaintext: String): String = plaintext
            .normalize()
            .toRectangleMatrix()
            .transpose()
            .joinToString(" ")
    private fun String.normalize() = this.toLowerCase().filter(Char::isLetterOrDigit)
    private fun String.toRectangleMatrix(): List<String> {
        val c = numberOfColumns(this)
        return this.chunked(c) { it.toString().padEnd(c, ' ') }
    }
    // Using max(x, 1) in case the string is empty
    private fun numberOfColumns(s: String) = maxOf(ceil(sqrt(s.length.toDouble())).toInt(), 1)
    private fun List<String>.transpose(): List<String> =
            (this.elementAtOrElse(0) { "" }.indices).map { col ->
                (this.indices).map { row ->
                    this[row][col]
                }.joinToString("")
            }
}
```

### Roman Numerals
```java
object RomanNumerals {
    fun value(number: Int): String {
        require(number < 4000) { "Unsupported Value $number" }
        return ROMAN.fold(Pair(number, "")) { (arabic, result) , (rem, roman) ->
            val times = arabic / rem
            Pair(arabic - rem * times, result + roman.repeat(times))
        }.second
    }
    val ROMAN = listOf(
        1000 to "M", 900 to "CM", 600 to "CM", 500 to "D", 400 to "CD",
        100 to "C", 90 to "XC", 50 to "L", 40 to "XL",
        10 to "X", 9 to "IX", 5 to "V", 4 to "IV", 1 to "I"
    )
}
```

### Series
```java
object Series {
    fun slices(n: Int, s: String): List<List<Int>> {
        require(n <= s.length && s.isNotBlank())
        return s.windowed(n) { it.map { c -> "$c".toInt() } }
    }
}
```

### Phone Number
```java
class PhoneNumber(input: String) {
    var number: String? = null
    init {
        number = input
                .replace(Regex("[^0-9]"), "")
                .let {
                    when {
                        it.length == 10
                                && it.substring(0, 1).matches(Regex("[2-9]"))
                                && it.substring(3, 4).matches(Regex("[2-9]")) -> it
                        it.length == 11
                                && it.first().equals('1') -> it.drop(1)
                        else -> null
                    }
                }
    }
}
```

### Nucleotide Count
```java
class Dna (sequence: String) {
    init {
        require(sequence.all { c -> c in "ACGT" })
    }
    val nucleotideCounts: Map<Char, Int> = sequence.groupBy { it }
            .mapValuesTo(mutableMapOf('A' to 0,'C' to 0, 'G' to 0, 'T' to 0)) { it.value.size }
}
```

### Luhn
```java
object Luhn {
  fun isValid(number: String): Boolean {
    val (digits, others) = number
      .filterNot(Char::isWhitespace)
      .partition(Char::isDigit)
    if (digits.length <= 1 || others.isNotEmpty()) {
      return false
    }
    val checksum = digits
      .map { it.toInt() - '0'.toInt() }
      .reversed()
      .mapIndexed { index, value ->
        if (index % 2 == 1 && value < 9) value * 2 % 9 else value
      }
      .sum()
    return checksum % 10 == 0
  }
}
```

### Largest Series Product
```java
class Series(input: String) {
    private val digits = input.map { it.digitToInt() }
    fun getLargestProduct(span: Int): Long {
        require(span >= 0 && span <= digits.size) { "span $span, outside of range 0..${digits.size}" }
        if (span == 0) return 1
        return digits.windowed(span).maxOf { it.product() }
    }
    private fun Iterable<Int>.product() = fold(1L) { acc, e -> acc * e }
}
```

### ISBN Verifier
```java
class IsbnVerifier {
    fun isValid(number: String): Boolean =
        with (number.replace("-", "")) {
            matches(Regex("\\d{9}[\\dX]")) &&
            mapIndexed { i, c -> c.asInt() * (10 - i) }.sum() % 11 == 0
        }
}
fun Char.asInt(): Int = if (this.isDigit()) this - '0' else 10
```

### Beer Song
```java
object   BeerSong {
    val lyrics = verses(99, 0)
    fun verses(start: Int, end: Int): String {
        return (start downTo end).map { verse(it) }.joinToString("\n")
    }
    fun verse(n: Int): String {
        return when {
            n in 2..99 -> "${n} bottles of beer on the wall, ${n} bottles of beer.\nTake one down and pass it around, ${n - 1} ${if (n == 2) "bottle" else "bottles"} of beer on the wall.\n"
            n == 1 -> "1 bottle of beer on the wall, 1 bottle of beer.\nTake it down and pass it around, no more bottles of beer on the wall.\n"
            n == 0 -> "No more bottles of beer on the wall, no more bottles of beer.\nGo to the store and buy some more, 99 bottles of beer on the wall.\n"
            n < 0  -> throw IllegalArgumentException("Beer song verse can't be negative")
            n > 99 -> throw IllegalArgumentException("Beer song only goes up to verse 99")
            else   -> ""
        }
    }
}
```

### Bob
```java
enum class Request {
    QUESTION, YELL, ADDRESS, ELSE;
    companion object {
        fun parseRequest(request: String) =
                when {
                    request.isBlank() -> ADDRESS
                    request.filter { it.isLetter() }.let { it.isNotEmpty() && it.none { it.isLowerCase() } } -> YELL
                    request.trimEnd().endsWith('?') -> QUESTION
                    else -> ELSE
                }
    }
}
class Bob {
    companion object {
        fun hey(request: String) =
                when (Request.parseRequest(request)) {
                    Request.QUESTION -> "Sure."
                    Request.YELL -> "Whoa, chill out!"
                    Request.ADDRESS -> "Fine. Be that way!"
                    Request.ELSE -> "Whatever."
                }
    }
}
```

### Diamond
```java
class DiamondPrinter {
  fun printToList(c : Char): List<String> {
    require(c in 'A'..'Z')
    val vp = ('A' until c) + (c downTo 'A')
    val hp = (c downTo 'A') + ('B'..c)
    return vp.map { y -> hp.map { x -> if (x == y) y else ' ' }.joinToString("") }
  }
}
```

### Anagram
```java
import java.util.stream.Collectors.toSet
class Anagram(private val s: String) {
    private val toSortedList: (String) -> List<Char> = { it.toLowerCase().toList().sorted() }
    fun match(anagrams: Collection<String>): Set<String> {
        return anagrams.stream()
                .filter{ it.length == s.length }
                .filter{ it.toLowerCase() != s.toLowerCase() }
                .filter{ toSortedList(it) == toSortedList(s) }
                .collect(toSet())
    }
}
```

### Pig Latin
```java
package PigLatin
fun translate(phrase: String) =
    Regex(
        """
        (?:
          (?<vowel> [aeiou] | xr | yt )
        | (?<consonant> ch | \w?qu | rh | thr? | sch | \w )
        )(?<body> \w+ )
        """, RegexOption.COMMENTS)
    .replace(phrase, "\${vowel}\${body}\${consonant}ay")
```

### Isogram
```java
class Isogram {
    companion object {
       fun isIsogram(input: String): Boolean{
           val clean = input.replace(Regex("[ -]"), "")
           return (clean.length == clean.toLowerCase().toCharArray().distinct().size)
       }
    }
}
```

### Binary Search
```java
object BinarySearch {
    fun search(list: List<Int>, item: Int): Int =
            (list.size / 2).let {
                when {
                    list.isEmpty() -> throw NoSuchElementException()
                    list[it] < item -> (it + 1) + search(list.subList(it + 1, list.size), item)
                    list[it] > item -> search(list.subList(0, it), item)
                    else -> it
                }
            }
}
```

### Linked List
```java
class Deque<T> {
    private var first: Node<T>? = null
    private var last: Node<T>? = null
    fun push(value: T) {
        if (first == null) {
            first = Node(value)
        } else {
            if (last == null) {
                last = first
            }
            first = Node(value, right = first)
            first?.right?.left = first
        }
    }
    fun pop(): T? {
        val toReturn = first?.value
        first?.right?.let {
            first = first!!.right
            first?.left = null
            if (first == last) {
                last = null
            }
        } ?: run { first = null }
        return toReturn
    }
    fun unshift(value: T) {
        if (first == null)
            push(value)
        else if (last == null) {
            last = Node(value, left = first)
            first?.right = last
        } else {
            val add = Node(value, left = last)
            last?.right = add
            last = add
        }
    }
    fun shift(): T? {
        if (last == null) {
            val ret = first?.value
            first = null
            return ret
        } else {
            val ret = last?.value
            last?.left?.right = null
            if (last?.left == first)
                last = null
            else
                last = last?.left
            return ret
        }
    }
}
class Node<T>(var value: T, var left: Node<T>? = null, var right: Node<T>? = null)
```

### Bank Account
```java
import java.util.concurrent.atomic.AtomicBoolean
import java.util.concurrent.atomic.AtomicInteger
class BankAccount {
    private val _balance: AtomicInteger = AtomicInteger(0)
    private val closed: AtomicBoolean = AtomicBoolean(false)
    val balance: Int
        get() = ifOpen {
            _balance.get()
        }
    fun adjustBalance(amount: Int) = ifOpen {
        _balance.addAndGet(amount)
    }
    fun close() {
        closed.set(true)
    }
    private inline fun <T> ifOpen(action: () -> T): T {
        if (closed.get()) throw IllegalStateException()
        return action()
    }
}
```

### Rotational Cipher
```java
import java.util.Random
private val random = Random()
private fun List<*>.randomSample(n: Int) = (0 until n).map { this[random.nextInt(this.size)] }
private val names = mutableSetOf<String>()
private val letters = ('A'..'Z').toList()
private val numbers = ('0'..'9').toList()
class Robot {
    var name: String
    init {
        name = generateName()
    }
    fun reset() {
        name = generateName()
    }
    private tailrec fun generateName(): String {
        val name = letters.randomSample(2).plus(numbers.randomSample(3)).joinToString("")
        return when {
            names.add(name) -> name
            else -> generateName()
        }
    }
}
```

### Robot Name
```java
import java.util.Random
private val random = Random()
private fun List<*>.randomSample(n: Int) = (0 until n).map { this[random.nextInt(this.size)] }
private val names = mutableSetOf<String>()
private val letters = ('A'..'Z').toList()
private val numbers = ('0'..'9').toList()
class Robot {
    var name: String
    init {
        name = generateName()
    }
    fun reset() {
        name = generateName()
    }
    private tailrec fun generateName(): String {
        val name = letters.randomSample(2).plus(numbers.randomSample(3)).joinToString("")
        return when {
            names.add(name) -> name
            else -> generateName()
        }
    }
}
```

### Word Count
```java
object WordCount {
    fun phrase(text: String): Map<String, Int> =
            text.toLowerCase()
                .split(Regex("[^a-zA-Z0-9']+"))
                .filter(String::isNotEmpty)
                .groupBy { it }.mapValues { it.value.size }
}
```

### Flatten-Array
```java
object Flattener {
   fun flatten(nestedList: List<Any?>): List<Any> {
      return nestedList.flatMap { when(it) {
         is List<*> -> flatten(it)
         else -> listOf(it)
      } }.filterNotNull()
   }
}
```

### Triangle
```java
class Triangle<out T : Number>(val a: T, val b: T, val c: T) {
    init {
        val sides = listOf(a, b, c).map(Number::toDouble).sorted()
        require(sides[0] > 0 && sides[0] + sides[1] > sides[2]) {
            "not a triangle"
        }
    }
    private val numSides = setOf(a, b, c).size
    val isEquilateral: Boolean = numSides == 1
    val isIsosceles:   Boolean = numSides <= 2
    val isScalene:     Boolean = numSides == 3
}
```

### Wordy
```java
import kotlin.math.pow
object Wordy {
    fun answer(input: String): Int {
        require(input.startsWith("What is ") && input.endsWith("?"))
        val expression = input.substring("What is ".length, input.length - 1)
                .replace("by ", "")
                .replace("th power", "")
                .replace("raised to the ", "raised ")
                .split(" ")
        var result = 0
        var operation = ""
        for ((index, value) in expression.withIndex()) {
            if (index % 2 == 0) {
                val intValue = value.toInt()
                when (operation) {
                    "plus" -> result += intValue
                    "minus" -> result -= intValue
                    "multiplied" -> result *= intValue
                    "divided" -> result /= intValue
                    "raised" -> result = result.pow(intValue)
                    "" -> result = intValue
                }
                operation = ""
            } else {
                operation = value
            }
        }
        if (operation.isNotEmpty()) throw IllegalArgumentException()
        return result
    }
}
private fun Int.pow(exp: Int) = this.toDouble().pow(exp).toInt()
```

### Zebra Puzzle
```java
import kotlin.math.absoluteValue

enum class Color { Red, Green, Ivory, Yellow, Blue }
enum class Resident { Englishman, Spaniard, Ukrainian, Norwegian, Japanese }
enum class Pet { Dog, Snails, Fox, Horse, Zebra }
enum class Drink { Coffee, Tea, Milk, OrangeJuice, Water }
enum class Smoke { OldGold, Kools, Chesterfields, LuckyStrike, Parliaments }

data class Solution(
    val colors: List<Color>,
    val residents: List<Resident>,
    val pets: List<Pet>,
    val drinks: List<Drink>,
    val smokes: List<Smoke>
)

class ZebraPuzzle {
    fun drinksWater() = solution.residents[solution.drinks.indexOf(Drink.Water)].name
    fun ownsZebra() = solution.residents[solution.pets.indexOf(Pet.Zebra)].name

    private val solution = calculateSolution()
    private fun calculateSolution(): Solution {
        for (colors in permutations<Color>().filter { it.matchesColorRules() }) {
            for (residents in permutations<Resident>().filter { it.matchesResidentRules(colors) }) {
                for (pets in permutations<Pet>().filter { it.matchesPetRules(residents) }) {
                    for (drinks in permutations<Drink>().filter { it.matchesDrinkRules(colors, residents) }) {
                        for (smokes in permutations<Smoke>().filter { it.matchesSmokeRules(colors, residents, drinks, pets) }) {
                            return Solution(colors, residents, pets, drinks, smokes)
                        }
                    }
                }
            }
        }
        throw Exception("No solution could be found")
    }

    private fun List<Color>.matchesColorRules() =
        indexOf(Color.Green) == indexOf(Color.Ivory) + 1

    private fun List<Resident>.matchesResidentRules(colors: List<Color>) =
        indexOf(Resident.Norwegian) == 0 &&
                indexOf(Resident.Englishman) == colors.indexOf(Color.Red) &&
                (indexOf(Resident.Norwegian) - colors.indexOf(Color.Blue)).absoluteValue == 1

    private fun List<Pet>.matchesPetRules(residents: List<Resident>) =
        indexOf(Pet.Dog) == residents.indexOf(Resident.Spaniard)

    private fun List<Drink>.matchesDrinkRules(colors: List<Color>, residents: List<Resident>) =
        indexOf(Drink.Coffee) == colors.indexOf(Color.Green) &&
                indexOf(Drink.Tea) == residents.indexOf(Resident.Ukrainian) &&
                indexOf(Drink.Milk) == 2

    private fun List<Smoke>.matchesSmokeRules(
        colors: List<Color>,
        residents: List<Resident>,
        drinks: List<Drink>,
        pets: List<Pet>
    ) =
        indexOf(Smoke.OldGold) == pets.indexOf(Pet.Snails) &&
                indexOf(Smoke.Kools) == colors.indexOf(Color.Yellow) &&
                (indexOf(Smoke.Chesterfields) - pets.indexOf(Pet.Fox)).absoluteValue == 1 &&
                (indexOf(Smoke.Kools) - pets.indexOf(Pet.Horse)).absoluteValue == 1 &&
                indexOf(Smoke.LuckyStrike) == drinks.indexOf(Drink.OrangeJuice) &&
                indexOf(Smoke.Parliaments) == residents.indexOf(Resident.Japanese)
}

private inline fun <reified T : Enum<T>> permutations() = enumValues<T>().toList().permutations()

private fun <T> List<T>.permutations(): Sequence<List<T>> =
    if (size <= 1) sequenceOf(this)
    else asSequence().flatMap { seq -> (this - seq).permutations().map { listOf(seq) + it } }
```

### Meetup
Meetup.kt
```java
import java.time.DayOfWeek
import java.time.LocalDate

import MeetupSchedule.*

class Meetup(_month: Int, _year: Int) {
    private val year = _year
    private val month = _month
    private val monthLength = LocalDate.of(_year, _month, 1).lengthOfMonth()
    fun day(dayOfWeek: DayOfWeek, schedule: MeetupSchedule): LocalDate {
        val possibleDays = (1..monthLength).toList()
            .groupBy { LocalDate.of(year, month, it).dayOfWeek }
            .getValue(dayOfWeek)
            .sorted()
        val meetupDay = when (schedule) {
            in (FIRST..FOURTH)  -> possibleDays[schedule.ordinal]
            LAST                -> possibleDays[possibleDays.lastIndex]
            else                -> possibleDays.first { it in (13..19) }
        }
        return LocalDate.of(year, month, meetupDay)
    }
}
```

MeetupSchedule.kt
```java
import java.time.DayOfWeek
import java.time.LocalDate

enum class MeetupSchedule {

    FIRST, SECOND, THIRD, FOURTH, LAST, TEENTH

}
```
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

### Clock
```java
class Clock(hh: Int, mm: Int) {
    private var hh = 0
    private var mm = 0

    init {
        setup(hh, mm)
    }

    fun subtract(minutes: Int) {
        add(-minutes)
    }

    fun add(minutes: Int) {
        setup(hh, mm + minutes)
    }

    private fun setup(hh: Int, mm: Int) {
        this.hh = ((hh + mm / 60) % 24)
        .let { (if (mm % 60 < 0) it - 1 else it) }
        .let { if (it < 0) it + 24 else it }
        this.mm = (mm % 60)
        .let { if (it < 0) it + 60 else it }
    }

    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (other is Clock) return hh == other.hh && mm == other.mm
        return false
    }

    override fun toString(): String = "%02d:%02d".format(hh, mm)
}
```

### Saddle Points
```java
data class MatrixCoordinate(val row: Int, val col: Int)
class Matrix(private val matrix: List<List<Int>>) {

    private val columns by lazy {
        (0 until matrix[0].size).map { i -> matrix.map { it[i] } }
    }

    private val maxInRow by lazy {
        matrix.map { it.maxOrNull() }
    }

    private val minInCol by lazy {
        columns.map { it.minOrNull() }
    }

    val saddlePoints: Set<MatrixCoordinate> = matrix.mapIndexed { x, row -> row.mapIndexedNotNull { y, it -> if (it == maxInRow[x] && it == minInCol[y]) { MatrixCoordinate(x, y) } else null } }.flatten().toSet()
 }
 ```

 ### React
 ```java
 class Reactor<T> {
     interface Subscription {
         fun cancel()
     }
     abstract inner class Cell(
     internal var usedBy: MutableSet<ComputeCell> = mutableSetOf()
         ) {
             abstract var value: T
         }
         inner class InputCell(initialValue: T) : Cell() {
             override var value: T = initialValue
             set(value) {
                 field = value
                 val map = LinkedHashMap<ComputeCell, Int>()
                 var i = 0
                 val searchIn = usedBy.toMutableList()
                 while (searchIn.any()) {
                     val e = searchIn.removeFirst()
                     map[e] = i++
                     searchIn.addAll(e.usedBy)
                 }
                 map.toList()
                 .sortedBy { (_, v) -> v }
                 .forEach { (k, _) -> k.value = k.updateValue() }
             }
         }
         inner class ComputeCell(
         private vararg val inputs: Cell,
         private val calc: (List<T>) -> T,
             ) : Cell() {
                 private val callbacks: MutableList<(T) -> Unit> = mutableListOf()
                 override var value: T = updateValue()
                 set(value) {
                     if (field != value) {
                         field = value
                         callbacks.forEach { c -> c(value) }
                     }
                 }
                 init {
                     inputs.forEach { input -> input.usedBy.add(this) }
                 }
                 internal fun updateValue(): T {
                     return calc(inputs.map { cell -> cell.value })
                 }
                 fun addCallback(callback: (T) -> Unit): Subscription {
                     callbacks.add(callback)
                     return object : Subscription {
                         override fun cancel() {
                             callbacks.remove(callback)
                         }
                     }
                 }
             }
         }
```

### Matrix
```java
class Matrix(private val matrixAsString: String) {
    fun column(colNr: Int): List<Int> {
        val matrixWhole: List<String> = matrixAsString.split("\n")
            val matrixCol : MutableList<Int> = mutableListOf()
                for (i in matrixWhole) {
                    matrixCol.add(i.split(" ")[colNr-1].toInt())
                }
                return matrixCol
            }
            fun row(rowNr: Int): List<Int> {
                val matrixWhole: List<String> = matrixAsString.split("\n")
                    return matrixWhole[rowNr - 1].split(" ").map{i -> i.toInt()}
                }
            }
```

### Transpose
```java
object Transpose {

    fun transpose(input: List<String>): List<String> {
        if (input.isEmpty()) return emptyList()

        val max = input.maxOf { it.length }
        var transposed = mutableListOf<String>()
            for (i in 0 until max) {
                val line = input.map { it.getOrNull(i)?: 'X' }.joinToString("")
                transposed.add(line.trimEnd('X').replace('X', ' '))
            }
            return transposed
        }
    }
```

### Leap
```java
class Year(private val year: Int) {
    val isLeap: Boolean = when {
        isDivisibleBy(400) -> true
        isDivisibleBy(100) -> false
        isDivisibleBy(4)   -> true
        else -> false
    }
    private fun isDivisibleBy(divisor: Int) = (year % divisor == 0)
}
```

### custom-set
```java
class CustomSet(vararg param: Int) {
    private val items :MutableList<Int> = param.toMutableList()
        fun isEmpty() = this.items.size == 0
        fun contains(other: Int): Boolean = items.contains(other)
        fun isSubset(other: CustomSet): Boolean = this.items.size <= other.items.size && this.items.all { other.contains(it) }
        fun isDisjoint(other: CustomSet): Boolean = this.isEmpty() || other.isEmpty() ||!this.items.any { other.contains(it)}
        fun intersection(other: CustomSet): CustomSet = CustomSet(*(this.items.filter { other.contains(it)}.toIntArray()))
        fun add(other: Int) { if (other !in this.items) this.items.add(other)}
        override fun equals(other: Any?): Boolean = other is CustomSet && this.isSubset(other) && other.isSubset(this)
        operator fun plus(other: CustomSet): CustomSet = CustomSet(*(other.items+(this.items.filter { !other.contains(it)})).toIntArray())
        operator fun minus(other: CustomSet): CustomSet = CustomSet(*(this.items.filter { !other.contains(it)}.toIntArray()))
    }
 ```

### Space Age
```java
class SpaceAge(private val ageInSeconds: Long) {
    fun onEarth(): Double {
        return formatRounding(baseAge())
    }
    fun onMercury(): Double {
        return formatRounding(baseAge().div( 0.2408467))
    }
    fun onVenus(): Double {
        return formatRounding(baseAge().div( 0.61519726))
    }
    fun onMars(): Double {
        return formatRounding(baseAge().div( 1.8808158))
    }
    fun onJupiter(): Double {
        return formatRounding(baseAge().div( 11.862615))
    }
    fun onSaturn(): Double {
        return formatRounding(baseAge().div( 29.447498))
    }
    fun onUranus(): Double {
        return formatRounding(baseAge().div( 84.016846))
    }
    fun onNeptune(): Double {
        return formatRounding(baseAge().div( 164.79132))
    }
    private fun baseAge(): Double {
        return ageInSeconds.div(31557600.toDouble())
    }
    private fun formatRounding(beforeRound: Double): Double {
        return "%.2f".format(beforeRound).toDouble()
    }
}
```

### CrptoSquare
```java
import kotlin.math.ceil
import kotlin.math.sqrt
object CryptoSquare {
    fun ciphertext(plaintext: String): String {
        val normalized = plaintext.filter { it.isLetterOrDigit() }.toLowerCase().also { if (it.isEmpty()) return "" }
        val numCols = ceil(sqrt(normalized.length.toDouble())).toInt()
        return normalized.chunked(numCols).let { sq ->
            (0 until numCols).joinToString(" ") { i -> sq.map { it.getOrNull(i) ?: ' ' }.joinToString("")  }
        }
    }
}
```.


## AWK

### Hello World
```awk
BEGIN {print "Hello World!"}
```
