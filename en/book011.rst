Chapter 10  Lists
--------------------------------




10.1  A list is a sequence
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Like a string, a list is a sequence of values. In a string, the
values are characters; in a list, they can be any type. The values in
list are called elements or sometimes items.







There are several ways to create a new list; the simplest is to
enclose the elements in square brackets ([ and ]):



.. sourcecode:: python

    [10, 20, 30, 40]
    ['crunchy frog', 'ram bladder', 'lark vomit']



The first example is a list of four integers. The second is a list of
three strings. The elements of a list don
’t have to be the same type.
The following list contains a string, a float, an integer, and
(lo!) another list:



.. sourcecode:: python

    ['spam', 2.0, 5, [10, 20]]



A list within another list is nested.







A list that contains no elements is
called an empty list; you can create one with empty
brackets, [].







As you might expect, you can assign list values to variables:



.. sourcecode:: python

    >>> cheeses = ['Cheddar', 'Edam', 'Gouda']
    >>> numbers = [17, 123]
    >>> empty = []
    >>> print cheeses, numbers, empty
    ['Cheddar', 'Edam', 'Gouda'] [17, 123] []





10.2  Lists are mutable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






The syntax for accessing the elements of a list is the same as for
accessing the characters of a string
—the bracket operator. The
expression inside the brackets specifies the index. Remember that the
indices start at 0:



.. sourcecode:: python

    >>> print cheeses[0]
    Cheddar



Unlike strings, lists are mutable. When the bracket operator appears
on the left side of an assignment, it identifies the element of the
list that will be assigned.







.. sourcecode:: python

    >>> numbers = [17, 123]
    >>> numbers[1] = 5
    >>> print numbers
    [17, 5]



The one-eth element of numbers, which
used to be 123, is now 5.







You can think of a list as a relationship between indices and
elements. This relationship is called a 
mapping; each index“maps to” one of the elements. Here is a state diagram showing cheeses, numbers and empty:











Lists are represented by boxes with the word “list” outside
and the elements of the list inside. 
cheeses refers to
a list with three elements indexed 0, 1 and 2.
numbers contains two elements; the diagram shows that the
value of the second element has been reassigned from 123 to 5.empty refers to a list with no elements.







List indices work the same way as string indices:



- Any integer expression can be used as an index.
- If you try to read or write an element that does not exist, you
  get an IndexError.
- If an index has a negative value, it counts backward from the
  end of the list.












The in operator also works on lists.



.. sourcecode:: python

    >>> cheeses = ['Cheddar', 'Edam', 'Gouda']
    >>> 'Edam' in cheeses
    True
    >>> 'Brie' in cheeses
    False

10.3  Traversing a list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






The most common way to traverse the elements of a list is
with a for loop. The syntax is the same as for strings:



.. sourcecode:: python

    for cheese in cheeses:
        print cheese



This works well if you only need to read the elements of the
list. But if you want to write or update the elements, you
need the indices. A common way to do that is to combine
the functions range and len:







.. sourcecode:: python

    for i in range(len(numbers)):
        numbers[i] = numbers[i] * 2



This loop traverses the list and updates each element. len
returns the number of elements in the list. 
range returns
a list of indices from 0 to 
n−1, where n is the length of
the list. Each time through the loop 
i gets the index
of the next element. The assignment statement in the body uses
i to read the old value of the element and to assign the
new value.







A for loop over an empty list never executes the body:



.. sourcecode:: python

    for x in empty:
        print 'This never happens.'



Although a list can contain another list, the nested
list still counts as a single element. The length of this list is
four:







.. sourcecode:: python

    ['spam', 1, ['Brie', 'Roquefort', 'Pol le Veq'], [1, 2, 3]]

10.4  List operations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






The + operator concatenates lists:







.. sourcecode:: python

    >>> a = [1, 2, 3]
    >>> b = [4, 5, 6]
    >>> c = a + b
    >>> print c
    [1, 2, 3, 4, 5, 6]



Similarly, the * operator repeats a list a given number of times:







.. sourcecode:: python

    >>> [0] * 4
    [0, 0, 0, 0]
    >>> [1, 2, 3] * 3
    [1, 2, 3, 1, 2, 3, 1, 2, 3]



The first example repeats [0] four times. The second example
repeats the list [1, 2, 3] three times.

10.5  List slices
~~~~~~~~~~~~~~~~~~~~~~~~~~~






The slice operator also works on lists:



.. sourcecode:: python

    >>> t = ['a', 'b', 'c', 'd', 'e', 'f']
    >>> t[1:3]
    ['b', 'c']
    >>> t[:4]
    ['a', 'b', 'c', 'd']
    >>> t[3:]
    ['d', 'e', 'f']



If you omit the first index, the slice starts at the beginning.
If you omit the second, the slice goes to the end. So if you
omit both, the slice is a copy of the whole list.







.. sourcecode:: python

    >>> t[:]
    ['a', 'b', 'c', 'd', 'e', 'f']



Since lists are mutable, it is often useful to make a copy
before performing operations that fold, spindle or mutilate
lists.







A slice operator on the left side of an assignment
can update multiple elements:







.. sourcecode:: python

    >>> t = ['a', 'b', 'c', 'd', 'e', 'f']
    >>> t[1:3] = ['x', 'y']
    >>> print t
    ['a', 'x', 'y', 'd', 'e', 'f']

10.6  List methods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~






Python provides methods that operate on lists. For example,append adds a new element to the end of a list:







.. sourcecode:: python

    >>> t = ['a', 'b', 'c']
    >>> t.append('d')
    >>> print t
    ['a', 'b', 'c', 'd']



extend takes a list as an argument and appends all of
the elements:







.. sourcecode:: python

    >>> t1 = ['a', 'b', 'c']
    >>> t2 = ['d', 'e']
    >>> t1.extend(t2)
    >>> print t1
    ['a', 'b', 'c', 'd', 'e']



This example leaves t2 unmodified.



sort arranges the elements of the list from low to high:







.. sourcecode:: python

    >>> t = ['d', 'c', 'e', 'b', 'a']
    >>> t.sort()
    >>> print t
    ['a', 'b', 'c', 'd', 'e']



List methods are all void; they modify the list and return None.
If you accidentally write 
t = t.sort(), you will be disappointed
with the result.





10.7  Map, filter and reduce
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


To add up all the numbers in a list, you can use a loop like this:



.. sourcecode:: python

    def add_all(t):
        total = 0
        for x in t:
            total += x
        return total



total is initialized to 0. Each time through the loop,
x gets one element from the list. The += operator
provides a short way to update a variable:







.. sourcecode:: python

        total += x



is equivalent to:



.. sourcecode:: python

        total = total + x



As the loop executes, total accumulates the sum of the
elements; a variable used this way is sometimes called anaccumulator.







Adding up the elements of a list is such a common operation
that Python provides it as a built-in function, sum:



.. sourcecode:: python

    >>> t = [1, 2, 3]
    >>> sum(t)
    6



An operation like this that combines a sequence of elements into
a single value is sometimes called reduce.







Sometimes you want to traverse one list while building
another. For example, the following function takes a list of strings
and returns a new list that contains capitalized strings:



.. sourcecode:: python

    def capitalize_all(t):
        res = []
        for s in t:
            res.append(s.capitalize())
        return res



res is initialized with an empty list; each time through
the loop, we append the next element. So 
res is another
kind of accumulator.







An operation like capitalize_all is sometimes called a map because it “maps” a function (in this case the method capitalize) onto each of the elements in a sequence.







Another common operation is to select some of the elements from
a list and return a sublist. For example, the following
function takes a list of strings and returns a list that contains
only the uppercase strings:



.. sourcecode:: python

    def only_upper(t):
        res = []
        for s in t:
            if s.isupper():
                res.append(s)
        return res



isupper is a string method that returns True if
the string contains only upper case letters.



An operation like only_upper is called a filter because
it selects some of the elements and filters out the others.



Most common list operations can be expressed as a combination
of map, filter and reduce. Because these operations are
so common, Python provides language features to support them,
including the built-in function 
map and an operator
called a “list comprehension.”







Exercise 1  

Write a function that takes a list of numbers and returns the
cumulative sum; that is, a new list where the 
ith element
is the sum of the first 
i+1 elements from the original list.
For example, the cumulative sum of 
[1, 2, 3] is
[1, 3, 6]. 



10.8  Deleting elements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






There are several ways to delete elements from a list. If you
know the index of the element you want, you can usepop:







.. sourcecode:: python

    >>> t = ['a', 'b', 'c']
    >>> x = t.pop(1)
    >>> print t
    ['a', 'c']
    >>> print x
    b



pop modifies the list and returns the element that was removed.
If you don
’t provide an index, it deletes and returns the
last element.



If you don’t need the removed value, you can use the del
operator:







.. sourcecode:: python

    >>> t = ['a', 'b', 'c']
    >>> del t[1]
    >>> print t
    ['a', 'c']



If you know the element you want to remove (but not the index), you
can use remove:







.. sourcecode:: python

    >>> t = ['a', 'b', 'c']
    >>> t.remove('b')
    >>> print t
    ['a', 'c']



The return value from remove is None.







To remove more than one element, you can use del with
a slice index:



.. sourcecode:: python

    >>> t = ['a', 'b', 'c', 'd', 'e', 'f']
    >>> del t[1:5]
    >>> print t
    ['a', 'f']



As usual, the slice selects all the elements up to, but not
including, the second index.

10.9  Lists and strings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






A string is a sequence of characters and a list is a sequence
of values, but a list of characters is not the same as a
string. To convert from a string to a list of characters,
you can use list:







.. sourcecode:: python

    >>> s = 'spam'
    >>> t = list(s)
    >>> print t
    ['s', 'p', 'a', 'm']



Because list is the name of a built-in function, you should
avoid using it as a variable name. I also avoid 
l because
it looks too much like 1. So that’s why I use t.



The list function breaks a string into individual letters. If
you want to break a string into words, you can use the 
split
method:







.. sourcecode:: python

    >>> s = 'pining for the fjords'
    >>> t = s.split()
    >>> print t
    ['pining', 'for', 'the', 'fjords']



An optional argument called a delimiter specifies which
characters to use as word boundaries.
The following example
uses a hyphen as a delimiter:







.. sourcecode:: python

    >>> s = 'spam-spam-spam'
    >>> delimiter = '-'
    >>> s.split(delimiter)
    ['spam', 'spam', 'spam']



join is the inverse of split. It
takes a list of strings and
concatenates the elements. 
join is a string method,
so you have to invoke it on the delimiter and pass the
list as a parameter:







.. sourcecode:: python

    >>> t = ['pining', 'for', 'the', 'fjords']
    >>> delimiter = ' '
    >>> delimiter.join(t)
    'pining for the fjords'



In this case the delimiter is a space character, so
join puts a space between words. To concatenate
strings without spaces, you can use the empty string,’’ as a delimiter. 





10.10  Objects and values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






If we execute these assignment statements:



.. sourcecode:: python

    a = 'banana'
    b = 'banana'



We know that a and b both refer to a
string, but we don
’t
know whether they refer to the 
same string.
There are two possible states:











In one case, a and b refer to two different objects that
have the same value. In the second case, they refer to the same
object.







To check whether two variables refer to the same object, you can
use the is operator.



.. sourcecode:: python

    >>> a = 'banana'
    >>> b = 'banana'
    >>> a is b
    True



In this example, Python only created one string object,
and both a and b refer to it.



But when you create two lists, you get two objects:



.. sourcecode:: python

    >>> a = [1, 2, 3]
    >>> b = [1, 2, 3]
    >>> a is b
    False



So the state diagram looks like this:











In this case we would say that the two lists are equivalent,
because they have the same elements, but not 
identical, because
they are not the same object. If two objects are identical, they are
also equivalent, but if they are equivalent, they are not necessarily
identical.







Until now, we have been using “object” and “value”
interchangeably, but it is more precise to say that an object has a
value. If you execute 
a = [1,2,3], a refers to a list
object whose value is a particular sequence of elements. If another
list has the same elements, we would say it has the same value.





10.11  Aliasing
~~~~~~~~~~~~~~~~~~~~~~~~~






If a refers to an object and you assign b = a,
then both variables refer to the same object:



.. sourcecode:: python

    >>> a = [1, 2, 3]
    >>> b = a
    >>> b is a
    True



The state diagram looks like this:











The association of a variable with an object is called a reference. In this example, there are two references to the same
object.







An object with more than one reference has more
than one name, so we say that the object is aliased.







If the aliased object is mutable, 
changes made with one alias affect
the other:



.. sourcecode:: python

    >>> b[0] = 17
    >>> print a
    [17, 2, 3]



Although this behavior can be useful, it is error-prone. In general,
it is safer to avoid aliasing when you are working with mutable
objects.







For immutable objects like strings, aliasing is not as much of a
problem. In this example:



.. sourcecode:: python

    a = 'banana'
    b = 'banana'



It almost never makes a difference whether a and b refer
to the same string or not.

10.12  List arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






When you pass a list to a function, the function gets a reference
to the list.
If the function modifies a list parameter, the caller sees the change.
For example, delete_head removes the first element from a list:



.. sourcecode:: python

    def delete_head(t):
        del t[0]



Here’s how it is used:



.. sourcecode:: python

    >>> letters = ['a', 'b', 'c']
    >>> delete_head(letters)
    >>> print letters
    ['b', 'c']



The parameter t and the variable letters are
aliases for the same object. The stack diagram looks like
this:











Since the list is shared by two frames, I drew
it between them.



It is important to distinguish between operations that
modify lists and operations that create new lists. For
example, the 
append method modifies a list, but the+ operator creates a new list:







.. sourcecode:: python

    >>> t1 = [1, 2]
    >>> t2 = t1.append(3)
    >>> print t1
    [1, 2, 3]
    >>> print t2
    None
    
    >>> t3 = t1 + [3]
    >>> print t3
    [1, 2, 3]
    >>> t2 is t3
    False



This difference is important when you write functions that
are supposed to modify lists. For example, this functiondoes not delete the head of a list:



.. sourcecode:: python

    def bad_delete_head(t):
        t = t[1:]              # WRONG!



The slice operator creates a new list and the assignment
makes 
t refer to it, but none of that has any effect
on the list that was passed as an argument.







An alternative is to write a function that creates and
returns a new list. For
example, 
tail returns all but the first
element of a list:



.. sourcecode:: python

    def tail(t):
        return t[1:]



This function leaves the original list unmodified.
Here’s how it is used:



.. sourcecode:: python

    >>> letters = ['a', 'b', 'c']
    >>> rest = tail(letters)
    >>> print rest
    ['b', 'c']



Exercise 2  

Write a function called chop that takes a list and modifies
it, removing the first and last elements, and returns None.



Then write function called middle that takes a list and
returns a new list that contains all but the first and last
elements.



10.13  Debugging
~~~~~~~~~~~~~~~~~~~~~~~~~~






Careless use of lists (and other mutable objects)
can lead to long hours of debugging. Here are some common
pitfalls and ways to avoid them:



# Don’t forget that most list methods modify the argument and
return 
None. This is the opposite of the string methods,
which return a new string and leave the original alone.If you are used to writing string code like this:

.. sourcecode:: python

    word = word.strip()

It is tempting to write list code like this:

.. sourcecode:: python

    t = t.sort()           # WRONG!

Because sort returns None, the
  next operation you perform with 
  t is likely to fail.Before using list methods and operators, you should read the
  documentation carefully and then test them in interactive mode. The
  methods and operators that lists share with other sequences (like
  strings) are documented at
  docs.python.org/lib/typesseq.html. The
  methods and operators that only apply to mutable sequences
  are documented at docs.python.org/lib/typesseq-mutable.html.
# Pick an idiom and stick with it.Part of the problem with lists is that there are too many
ways to do things. For example, to remove an element from
a list, you can use 
pop, remove, del,
or even a slice assignment.
To add an element, you can use the append method or
the + operator. But don’t forget that these are right: 

.. sourcecode:: python

    t.append(x)
    t = t + [x]

And these are wrong:

.. sourcecode:: python

    t.append([x])          # WRONG!
    t = t.append(x)        # WRONG!
    t + [x]                # WRONG!
    t = t + x              # WRONG!

Try out each of these examples in interactive mode to make sure
  you understand what they do. Notice that only the last
  one causes a runtime error; the other three are legal, but they
  do the wrong thing.
# Make copies to avoid aliasing.If you want to use a method like sort that modifies
the argument, but you need to keep the original list as
well, you can make a copy.

.. sourcecode:: python

    orig = t[:]
    t.sort()

In this example you could also use the built-in function sorted,
  which returns a new, sorted list and leaves the original alone.
  But in that case you should avoid using 
  sorted as a variable
  name!


10.14  Glossary
~~~~~~~~~~~~~~~~~~~~~~~~~


:list: A sequence of values.
:element: One of the values in a list (or other sequence),
  also called items.
:index: An integer value that indicates an element in a list.
:nested list: A list that is an element of another list.
:list traversal: The sequential accessing of each element in a list.
:mapping: A relationship in which each element of one set
  corresponds to an element of another set. For example, a list is
  a mapping from indices to elements.
:accumulator: A variable used in a loop to add up or
accumulate a result.




:reduce: A processing pattern that traverses a sequence 
  and accumulates the elements into a single result.
:map: A processing pattern that traverses a sequence and
  performs an operation on each element.
:filter: A processing pattern that traverses a list and
  selects the elements that satisfy some criterion.
:object: Something a variable can refer to. An object
  has a type and a value.
:equivalent: Having the same value.
:identical: Being the same object (which implies equivalence).
:reference: The association between a variable and its value.
:aliasing: A circumstance where two variables refer to the same
  object.
:delimiter: A character or string used to indicate where a
  string should be split.


10.15  Exercises
~~~~~~~~~~~~~~~~~~~~~~~~~~


Exercise 3  
Write a function called 
is_sorted that takes a list as a
parameter and returns 
True if the list is sorted in ascending
order and 
False otherwise. You can assume (as a precondition)
that the elements of the list can be compared with the comparison
operators <, >, etc.





For example, is_sorted([1,2,2]) should return True
and 
is_sorted(['b','a']) should return False.





Exercise 4  





Two words are anagrams if you can rearrange the letters from one
to spell the other. Write a function called 
is_anagram
that takes two strings and returns 
True if they are anagrams.





Exercise 5  

The (so-called) Birthday Paradox:



# Write a function called has_duplicates that takes
  a list and returns 
  True if there is any element that
  appears more than once. It should not modify the original
  list.
# If there are 23 students in your class, what are the chances
  that two of you have the same birthday? You can estimate this
  probability by generating random samples of twelve birthdays
  and checking for matches. Hint: you can generate random birthdays
  with the randint function in the random module.




You can read about this problem at
wikipedia.org/wiki/Birthday_paradox, and you can see my solution
at thinkpython.com/code/birthday.py.





Exercise 6  





Write a function called remove_duplicates that takes
a list and returns a new list with only the unique elements from
the original. Hint: they don
’t have to be in the same order.





Exercise 7  

Write a function that reads the file words.txt and builds
a list with one element per word. Write two versions of
this function, one using the 
append method and the
other using the idiom 
t = t + [x]. Which one takes
longer to run? Why?



You can see my solution at thinkpython.com/code/wordlist.py.





Exercise 8  





To check whether a word is in the word list, you could use
the 
in operator, but it would be slow because it searches
through the words in order.



Because the words are in alphabetical order, we can speed things up
with a bisection search, which is similar to what you do when you look
a word up in the dictionary. You start in the middle and check to see
whether the word you are looking for comes before the word in the
middle of the list. If so, then you search the first half of the list
the same way. Otherwise you search the second half.



Either way, you cut the remaining search space in half. If the
word list has 113,809 words, it will take about 17 steps to
find the word or conclude that it’s not there.



Write a function called bisect that takes a sorted list
and a target value and returns the index of the value
in the list, if it’s there, or None if it’s not.







Or you could read the documentation of the bisect module
and use that!





Exercise 9  

Two words are a “reverse pair” if each is the reverse of the
other. Write a program that finds all the reverse pairs in the
word list. 





Exercise 10  

Two words “interlock” if taking alternating letters from each forms
a new word
1. For example, “shoe” and “cold”
interlock to form “schooled.”



# Write a program that finds all pairs of words that interlock.
  Hint: don’t enumerate all pairs!
# Can you find any words that are three-way interlocked; that is,
  every third letter forms a word, starting from the first, second or
  third?






:1This exercise is inspired by an example at
  puzzlers.org.


