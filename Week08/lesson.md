# A Little Help From Our Friends (at Python)

![hazel](assets/hazel_pet.jpg)

## Coding is hard

Okay, so we've been coding for a while now. You have the basic tools to take on almost anything!

So, let's flex those muscles a bit. Write me the code to return all the permutations for two items out of a list. This might be useful to pick out all the possible pairings out of a list of fellows. So we want to go from `fellows = ["Chloe","Connor","Janet","Lauren","Natasha"]` to:

```python
[('Chloe', 'Connor'), ('Chloe', 'Janet'), ('Chloe', 'Lauren'), ('Chloe', 'Natasha'), ('Connor', 'Chloe'), ('Connor', 'Janet'), ('Connor', 'Lauren'), ('Connor', 'Natasha'), ('Janet', 'Chloe'), ('Janet', 'Connor'), ('Janet', 'Lauren'), ('Janet', 'Natasha'), ('Lauren', 'Chloe'), ('Lauren', 'Connor'), ('Lauren', 'Janet'), ('Lauren', 'Natasha'), ('Natasha', 'Chloe'), ('Natasha', 'Connor'), ('Natasha', 'Janet'), ('Natasha', 'Lauren')]
```

So, where would we start?

Let's go with one loop on the outside and one on the inside, but we want to make sure that the fellow in the outside isn't the same as the one on the outside...

```python
def permutations(fellows):
    perms = []
    for fellow1 in fellows:
        for fellow2 in fellows:
            if fellow1==fellow2:
                continue
            permutations.append((fellow1,fellow2))
    return perms

fellows = ["Chloe","Connor","Janet","Lauren","Natasha"]
print(permutations(fellows))
```

Easy enough... so far. What if we want to do groups of three instead of two?

```python
def permutations(fellows):
    perms = []
    for fellow1 in fellows:
        for fellow2 in fellows:
            if fellow1==fellow2:
                continue
            p = (fellow1,fellow2)
            for fellow3 in fellows:
                if fellow3 not in p:
                    p = p+(fellow3,)
                    perms.append(p)
    return perms

fellows = ["Chloe","Connor","Janet","Lauren","Natasha"]
print(permutations(fellows))
```

Whew! This is getting a bit rough.

Okay, so what about permutations of 4 fellows? Or a generalizable algorithm that calculates different sizes of permutation sets based on a parameter? Hmmm. I think we're going to have to talk about recursion...

But, wait. Remember when I said (a long time ago) that really novel problems are rare and that it's almost certain that other people have had the same problems as you in the past?

What if we could let some hardworking Python developers do the hard work for us? 

## Imports

```python
from itertools import permutations
praxis = ["Chloe","Connor","Janet","Lauren","Natasha"]
print(permutations(praxis,3))
```

Easy!

We can see in that first line, `from itertools import permutations`, that we're importing a function, `permutations` from the *module* `itertools`, a set of code that provides tools for iterative computation. [Here's the doc.](https://docs.python.org/3/library/itertools.html)

Itertools is one of many modules that come as part of the Python Standard Library. When Python was released, it was known for this robust library that provided many common and fundamental tools.

Let's take a look at another example.

How do we write a function to return a random number? Beats the hell out of me. This is... actually a really hard problem that has important ramifications for, among a lot of tother things, security if not done right. The Python random function is actually probably not good enough for high-level infosec uses, but it's good enough for most things we're liable to do.

![python seucrity warning](assets/security_warning.png)

By nature, randomness is complicated and random number generators are too. There's a lot of stuff in the [Python random module](https://docs.python.org/3/library/random.html), but there's also a few easy functions for us to easily get what we want if we don't care about the nuts and bolts.

We can get a random integer (or a random index for a list):

```python
import random
praxis = ["Chloe","Connor","Janet","Lauren","Natasha"]
print(praxis[random.randint(0,len(praxis)-1)])
```

(I ran this code to test it and I got 4 Chloes out of 5 tries. I don't know what that means.)

If we read the doc, we can see that randint is inclusive (so, randint(0,5) would sometimes return 5), so we want to do len(praxis)-1 because the valid indices for Praxis are `[0,1,2,3,4]`.

In this example, we can see how we can also just import an entire module using `import random` and access its member functions and classes through that name. We could have done this before with `itertools` too:

```python
import itertools
print(itertools.permutations([1,2,3,4,5],2))
```

It just depends on whether or not we just need to use a single function or class from a module or if we want to use more.

Back to random: we can also use `random.shuffle()` or `random.sample` to shuffle the list or to pick out a random sampling from that list.

```python
import random
praxis = ["Chloe","Connor","Janet","Lauren","Natasha"]
print(praxis)
random.shuffle(praxis)
print(praxis)
print(random.sample(praxis,2))
```

We can find the full, glorious index to the Python Standard Library [here](https://docs.python.org/3/library/).


## Importing your own classes

Modules in the Standard Library and from other sources (more on this in a moment) are packaged in a particular way, but we can use imports to bring in our own code too. Let's take the Dog class we worked with last week.

```python
class Dog:
    def __init__(self, name, owner, breed, likes, dislikes):
        self.name = name
        self.owner = owner
        self.breed = breed
        self.likes = likes
        self.dislikes = dislikes
    
    def speak(self):
        print("Bork bork! I'm",self.name)
```

Let's say that we have a few different files that need to refer to Dogs. We don't want to just duplicate this code in all of those, not just because it's messy and a bit of work, but because if we change some part of that Dog code, we want those changes to be the same everywhere so all our code will interoperate with each other. If we added a new data field to our Dog class and constructor, age for example, that new Dog class wouldn't necessarily play well with code that used the old version.

Imports are the solution to this problem. If we save this class to its own file, let's say `dog.py`, we can import this in any other python file in the same directory:

```python
from dog import Dog
hazel = Dog("Hazel","Shane","Beagle",["treats","naps","raccoons"],["thunder"])
hazel.speak()
```

In that first line, `dog` is the name of the file (dog.py) and Dog is the class within that file.


## Outside help

The Python Standard Library provides a pretty wide set of tools. It comes with every Python install and that's one of its main strengths - you don't have to worry about whether someone has itertools or random installed.

But sometimes, the thing we need is a little bit more obscure or specific. We couldn't include all these things with every Python install, because otherwise that install would be enormous. Instead, there's something like an app store for Python code (except that it's all free, because Python is all free) called the [Python Package Index, or PyPI](https://pypi.org/). Not to be confused with PyPy (groan), which is something else.

It's pretty big. Anyone can submit projects to it and so there are 200,000 individual ones.

We can install any one of these packages through [Pipenv](https://pipenv-fork.readthedocs.io/en/latest/), which all of you should already have since we did the environment setup at the begining of the year.

[Here's a decent tutorial](https://thoughtbot.com/blog/how-to-manage-your-python-projects-with-pipenv)

You can browse through PyPI and see if there's anything interesting. Of course, the first thing I did was find this: [getname](https://pypi.org/project/getname/).

Which allows us to do...

```python
from getname import random_name

dogs = []
while len(dogs)<6:
    dogs.append(random_name('dog'))
print(dogs)
```
