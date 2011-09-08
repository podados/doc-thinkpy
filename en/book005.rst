Chapter 4  Case study: interface design
------------------------------------------------------




4.1  TurtleWorld
~~~~~~~~~~~~~~~~~~~~~~~~~~






To accompany this book, I have written a suite of modules called
Swampy. One of these modules is TurtleWorld, which provides
a set of functions for drawing lines by steering
turtles around the screen.



You can download Swampy from http://thinkpython.com/swampy;
follow the instructions there to install Swampy on your system.



Move into the directory that contains TurtleWorld.py,
create a file named polygon.py and type in the following
code:



.. sourcecode:: python

    from TurtleWorld import *
    
    world = TurtleWorld()
    bob = Turtle()
    print bob
    
    wait_for_user()



The first line is a variation of the import statement we saw before;
instead of creating a module object, it imports the functions
from the module directly, so you can access them without using dot
notation.







The next lines create a TurtleWorld assigned to world and
a Turtle assigned to bob. Printing bob yields something
like:



.. sourcecode:: python

    <TurtleWorld.Turtle instance at 0xb7bfbf4c>



This means that bob refers to an instance of a Turtle
as defined in module TurtleWorld. In this context,
“instance” means a member of a set;
this TurtleWorld is one of the set of possible TurtleWorlds.







*wait_for_user* tells TurtleWorld to wait for the user
to do something, although in this case there’s not much for
the user to do except close the window.



TurtleWorld provides several turtle-steering functions: 
*fd* and *bk* for forward and backward, and *lt* and *rt* for left and
right turns. Also, each Turtle is holding a pen, which is
either down or up; if the pen is down, the Turtle leaves
a trail when it moves. The functions *pu* and *pd*
stand for “pen up” and “pen down.”



To draw a right angle, add these lines to the program
(after creating bob and before calling *wait_for_user*):



.. sourcecode:: python

    fd(bob, 100)
    rt(bob)
    fd(bob, 100)



The first line tells bob to take 100 steps
forward. The second line tells him to turn right.



When you run this program, you should see bob move east and then
south, leaving two line segments behind.



Now modify the program to draw a square. Don’t turn the page until
you’ve got it working!

4.2  Simple repetition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






Chances are you wrote something like this (leaving out the code
that creates TurtleWorld and waits for the user):



.. sourcecode:: python

    fd(bob, 100)
    lt(bob)
    
    fd(bob, 100)
    lt(bob)
    
    fd(bob, 100)
    lt(bob)
    
    fd(bob, 100)



We can do the same thing more concisely with a for statement.
Add this example to polygon.py and run it again:







.. sourcecode:: python

    for i in range(4):
        print 'Hello!'



You should see something like this:



.. sourcecode:: python

    Hello!
    Hello!
    Hello!
    Hello!



This is the simplest use of the for statement; we will see
more later. But that should be enough to let you rewrite your
square-drawing program. Don’t turn the page until you do.



Here is a for statement that draws a square:



.. sourcecode:: python

    for i in range(4):
        fd(bob, 100)
        lt(bob)



The syntax of a for statement is similar to a function
definition. It has a header that ends with a colon and an indented
body. The body can contain any number of statements.







A for statement is sometimes called a loop because
the flow of execution runs through the body and then loops back
to the top. In this case, it runs the body four times.



This version is actually a little different from the previous
square-drawing code because it makes another left turn after
drawing the last side of the square. The extra turn takes a little
more time, but it simplifies the code if we do the same thing
every time through the loop. This version also has the effect
of leaving the turtle back in the starting position, facing in
the starting direction.

4.3  Exercises
~~~~~~~~~~~~~~~~~~~~~~~~


The following is a series of exercises using TurtleWorld. They
are meant to be fun, but they have a point, too. While you are
working on them, think about what the point is.



The following sections have solutions to the exercises, so
don’t look until you have finished (or at least tried).



#. Write a function called square that takes a parameter
   named t, which is a turtle. It should use the turtle to draw
   a square.  Write a function call that passes bob as an
   argument to square, and then run the program again.
#. Add another parameter, named length, to square.
   Modify the body so length of the sides is length, and then
   modify the function call to provide a second argument. Run the
   program again. Test your program with a range of values for length.
#. The functions lt and rt make 90-degree turns by
   default, but you can provide a second argument that specifies the
   number of degrees. For example, lt(bob, 45) turns bob 45
   degrees to the left.  Make a copy of square and change the name to polygon. Add
   another parameter named n and modify the body so it draws an
   n-sided regular polygon. Hint: The angles of an n-sided regular
   polygon are 360.0 / n degrees.
#. Write a function called circle that takes a turtle, t, and radius, 
   r, as parameters and that draws an approximate circle by invoking 
   polygon with an appropriate length and number of sides. Test your
   function with a range of values of r.
   
        Hint: figure out the circumference of the circle and make sure that
        length * n = circumference.
        Another hint: if bob is too slow for you, you can speed
        him up by changing bob.delay, which is the time between moves,
        in seconds. bob.delay = 0.01 ought to get him moving.

#. Make a more general version of circle called arc
   that takes an additional parameter angle, which determines
   what fraction of a circle to draw.  angle is in units of
   degrees, so when angle=360, arc should draw a complete
   circle.


4.4  Encapsulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~


The first exercise asks you to put your square-drawing code
into a function definition and then call the function, passing
the turtle as a parameter. Here is a solution:



.. sourcecode:: python

    def square(t):
        for i in range(4):
            fd(t, 100)
            lt(t)
    
    square(bob)



The innermost statements, fd and lt are indented twice to show
that they are inside the for loop,
which is inside the function definition. The next line,
square(bob), is flush with the left margin, so that is the
end of both the for loop and the function definition.



Inside the function, t refers to the same turtle bob
refers to, so lt(t) has the same effect as lt(bob).
So why not call the parameter bob? The idea is that t
can be any turtle, not just bob, so you could create
a second turtle and pass it as an argument to square:



.. sourcecode:: python

    ray = Turtle()
    square(ray)



Wrapping a piece of code up in a function is called encapsulation.
One of the benefits of encapsulation is that it
attaches a name to the code, which serves as a kind of documentation.
Another advantage is that if you re-use the code, it is more concise
to call a function twice than to copy and paste the body!





4.5  Generalization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


The next step is to add a length parameter to square.
Here is a solution:



.. sourcecode:: python

    def square(t, length):
        for i in range(4):
            fd(t, length)
            lt(t)
    
    square(bob, 100)



Adding a parameter to a function is called generalization
because it makes the function more general: in the previous
version, the square is always the same size; in this version
it can be any size.







The next step is also a generalization. Instead of drawing
squares, polygon draws regular polygons with any number of
sides. Here is a solution:



.. sourcecode:: python

    def polygon(t, n, length):
        angle = 360.0 / n
        for i in range(n):
            fd(t, length)
            lt(t, angle)
    
    polygon(bob, 7, 70)



This draws a 7-sided polygon with side length 70. If you have
more than a few numeric arguments, it is easy to forget what they
are, or what order they should be in. It is legal, and sometimes
helpful, to include the names of the parameters in the argument
list:



.. sourcecode:: python

    polygon(bob, n=7, length=70)



These are called keyword arguments because they include
the parameter names as “keywords” (not to be confused with
Python keywords like while and def).







This syntax makes the program more readable. It is also a reminder
about how arguments and parameters work: when you call a function, the
arguments are assigned to the parameters.

4.6  Interface design
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


The next step is to write circle, which takes a radius, *r*, as a
parameter. Here is a simple solution that uses polygon to draw a
50-sided polygon:



.. sourcecode:: python

    def circle(t, r):
        circumference = 2 * math.pi * r
        n = 50
        length = circumference / n
        polygon(t, n, length)



The first line computes the circumference of a circle with radius
*r* using the formula *2 π r*. Since we use *math.pi*, we
have to import *math*. By convention, import statements
are usually at the beginning of the script.



*n* is the number of line segments in our approximation of a circle,
so length is the length of each segment. Thus, polygon
draws a 50-sides polygon that approximates a circle with radius *r*.



One limitation of this solution is that n is a constant, which
means that for very big circles, the line segments are too long, and
for small circles, we waste time drawing very small segments. One
solution would be to generalize the function by taking n as
a parameter. This would give the user (whoever calls circle)
more control, but the interface would be less clean.







The interface of a function is a summary of how it is used: what
are the parameters? What does the function do? And what is the return
value? An interface is “clean” if it is

    “as simple as possible, but not simpler. (Einstein)”







In this example, r belongs in the interface because it
specifies the circle to be drawn. *n* is less appropriate
because it pertains to the details of how the circle should
be rendered.



Rather than clutter up the interface, it is better
to choose an appropriate value of *n*
depending on circumference:



.. sourcecode:: python

    def circle(t, r):
        circumference = 2 * math.pi * r
        n = int(circumference / 3) + 1
        length = circumference / n
        polygon(t, n, length)



Now the number of segments is (approximately) *circumference/3*,
so the length of each segment is (approximately) 3, which is small
enough that the circles look good, but big enough to be efficient,
and appropriate for any size circle.

4.7  Refactoring
~~~~~~~~~~~~~~~~~~~~~~~~~~






When I wrote circle, I was able to re-use polygon
because a many-sided polygon is a good approximation of a circle.
But arc is not as cooperative; we can’t use polygon
or circle to draw an arc.



One alternative is to start with a copy of polygon and transform it
into arc. The result might look like this:



.. sourcecode:: python

    def arc(t, r, angle):
        arc_length = 2 * math.pi * r * angle / 360
        n = int(arc_length / 3) + 1
        step_length = arc_length / n
        step_angle = float(angle) / n
        
        for i in range(n):
            fd(t, step_length)
            lt(t, step_angle)



The second half of this function looks like polygon, but we
can’t re-use polygon without changing the interface. We could
generalize polygon to take an angle as a third argument,
but then polygon would no longer be an appropriate name!
Instead, let’s call the more general function polyline:



.. sourcecode:: python

    def polyline(t, n, length, angle):
        for i in range(n):
            fd(t, length)
            lt(t, angle)



Now we can rewrite polygon and arc to use polyline:



.. sourcecode:: python

    def polygon(t, n, length):
        angle = 360.0 / n
        polyline(t, n, length, angle)
    
    def arc(t, r, angle):
        arc_length = 2 * math.pi * r * angle / 360
        n = int(arc_length / 3) + 1
        step_length = arc_length / n
        step_angle = float(angle) / n
        polyline(t, n, step_length, step_angle)



Finally, we can rewrite circle to use arc:



.. sourcecode:: python

    def circle(t, r):
        arc(t, r, 360)



This process—rearranging a program to improve function
interfaces and facilitate code re-use — is called refactoring.
In this case, we noticed that there was similar code in 
arc andpolygon, so we “factored it out” into polyline.







If we had planned ahead, we might have written polyline first
and avoided refactoring, but often you don’t know enough at the
beginning of a project to design all the interfaces. Once you start
coding, you understand the problem better. Sometimes refactoring is a
sign that you have learned something.

4.8  A development plan
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






A development plan is a process for writing programs.
The process we used in this case study is “encapsulation and
generalization.” The steps of this process are:



#. Start by writing a small program with no function definitions.
#. Once you get the program working, encapsulate it in a function
   and give it a name.
#. Generalize the function by adding appropriate parameters.
#. Repeat steps 1–3 until you have a set of working functions.
   Copy and paste working code to avoid retyping (and re-debugging).
#. Look for opportunities to improve the program by refactoring.
   For example, if you have similar code in several places, consider
   factoring it into an appropriately general function.




This process has some drawbacks—we will see alternatives later—but
it can be useful if you don’t know ahead of time how to divide the
program into functions. This approach lets you design as you go
along.

4.9  docstring
~~~~~~~~~~~~~~~~~~~~~~~~


A docstring is a string at the beginning of a function that
explains the interface (
“doc” is short for “documentation”). Here
is an example:



.. sourcecode:: python

    def polyline(t, length, n, angle):
        """Draw n line segments with the given length and
        angle (in degrees) between them.  t is a turtle.
        """    
        for i in range(n):
            fd(t, length)
            lt(t, angle)



This docstring is a triple-quoted string, also known
as a multiline string because the triple quotes allow the string
to span more than one line.



It is terse, but it contains the essential information
someone would need to use this function. It explains concisely what
the function does (without getting into the details of how it does
it). It explains what effect each parameter has on the behavior of
the function and what type each parameter should be (if it is not
obvious).



Writing this kind of documentation is an important part of interface
design. A well-designed interface should be simple to explain;
if you are having a hard time explaining one of your functions,
that might be a sign that the interface could be improved.

4.10  Debugging
~~~~~~~~~~~~~~~~~~~~~~~~~


An interface is like a contract between a function and a caller.
The caller agrees to provide certain parameters and the function
agrees to do certain work.



For example, polyline requires four arguments. The first
has to be a Turtle (or some other object that works with 
fd and lt). The second has to be a number, and it should
probably be positive, although it turns out that the function
works even if it isn’t. The third argument should be an integer;
range complains otherwise (depending on which version
of Python you are running). The fourth has to be a number,
which is understood to be in degrees.



These requirements are called preconditions because they
are supposed to be true before the function starts executing.
Conversely, conditions at the end of the function are
postconditions. Postconditions include the intended
effect of the function (like drawing line segments) and any
side effects (like moving the Turtle or making other changes
in the World).







Preconditions are the responsibility of the caller. If the caller
violates a (properly documented!) precondition and the function
doesn’t work correctly, the bug is in the caller, not the function.
However, for purposes of debugging it is often a good idea for
functions to check their preconditions rather than assume they are
true. If every function checks its preconditions before starting,
then if something goes wrong, you will know which function to blame.

4.11  Glossary
~~~~~~~~~~~~~~~~~~~~~~~~


:instance: A member of a set. The TurtleWorld in this
  chapter is a member of the set of TurtleWorlds.
:loop: A part of a program that can execute repeatedly.
:encapsulation: The process of transforming a sequence of
  statements into a function definition.
:generalization: The process of replacing something
  unnecessarily specific (like a number) with something appropriately
  general (like a variable or parameter).
:keyword argument: An argument that includes the name of
  the parameter as a “keyword.”
:interface: A description of how to use a function, including
  the name and descriptions of the arguments and return value.
:development plan: A process for writing programs.
:docstring: A string that appears in a function definition
  to document the function’s interface.
:precondition: A requirement that should be satisfied by
  the caller before a function starts.
:postcondition: A requirement that should be satisfied by
  the function before it ends.


4.12  Exercises
~~~~~~~~~~~~~~~~~~~~~~~~~


Exercise 1  
``````````

Download the code in this chapter from http://thinkpython.com/code/polygon.py.



#. Write appropriate docstrings for polygon, arc andcircle.
#. Draw a stack diagram that shows the state of the program
   while executing circle(bob, radius). You can do the
   arithmetic by hand or add print statements to the code.
#. The version of arc in Section 4.7 is not
   very accurate because the linear approximation of the
   circle is always outside the true circle. As a result,
   the turtle ends up a few units away from the correct
   destination. My solution shows a way to reduce
   the effect of this error. Read the code and see if it makes
   sense to you. If you draw a diagram, you might see how it works.






Exercise 2  
``````````

Write an appropriately general set of functions that
can draw flowers like this:


You can download a solution from http://thinkpython.com/code/flower.py.





Exercise 3  
``````````

Write an appropriately general set of functions that
can draw shapes like this:


You can download a solution from http://thinkpython.com/code/pie.py.





Exercise 4  
``````````

The letters of the alphabet can be constructed from a moderate
number of basic elements, like vertical and horizontal lines
and a few curves. Design a font that can be drawn with a
minimal number of basic elements and then write functions
that draw letters of the alphabet.



You should write one function for each letter, with names
draw_a, draw_b, etc., and put your functions in a file named 
letters.py. You can download a “turtle typewriter” from
http://thinkpython.com/code/typewriter.py
to help you test your code.



You can download a solution from http://thinkpython.com/code/letters.py.



