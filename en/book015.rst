Chapter 14  Files
--------------------------------




14.1  Persistence
~~~~~~~~~~~~~~~~~~~~~~~~~~~






Most of the programs we have seen so far are transient in the
sense that they run for a short time and produce some output,
but when they end, their data disappears. If you run the program
again, it starts with a clean slate.



Other programs are persistent: they run for a long time
(or all the time); they keep at least some of their data
in permanent storage (a hard drive, for example); and
if they shut down and restart, they pick up where they left off.



Examples of persistent programs are operating systems, which
run pretty much whenever a computer is on, and web servers,
which run all the time, waiting for requests to come in on
the network.



One of the simplest ways for programs to maintain their data
is by reading and writing text files. We have already seen
programs that read text files; in this chapters we will see programs
that write them.



An alternative is to store the state of the program in a database.
In this chapter I will present a simple database and a module,pickle, that makes it easy to store program data.





14.2  Reading and writing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






A text file is a sequence of characters stored on a permanent
medium like a hard drive, flash memory, or CD-ROM. We saw how
to open and read a file in Section 9.1.







To write a file, you have to open it with mode’w’ as a second parameter:



.. sourcecode:: python

    >>> fout = open('output.txt', 'w')
    >>> print fout<open file 'output.txt', mode 'w' at 0xb7eb2410>



If the file already exists, opening it in write mode clears out
the old data and starts fresh, so be careful!
If the file doesn’t exist, a new one is created.



The write method puts data into the file.



.. sourcecode:: python

    >>> line1 = "This here's the wattle,\n"
    >>> fout.write(line1)



Again, the file object keeps track of where it is, so if
you call write again, it adds the new data to the end.



.. sourcecode:: python

    >>> line2 = "the emblem of our land.\n"
    >>> fout.write(line2)



When you are done writing, you have to close the file.



.. sourcecode:: python

    >>> fout.close()





14.3  Format operator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






The argument of write has to be a string, so if we want
to put other values in a file, we have to convert them to
strings. The easiest way to do that is with str:



.. sourcecode:: python

    >>> x = 52
    >>> f.write(str(x))



An alternative is to use the format operator, %. When
applied to integers, 
% is the modulus operator. But
when the first operand is a string, % is the format operator.







The first operand is the format string, and the second operand
is a tuple of expressions. The result is a string that contains
the values of the expressions, formatted according to the format
string.







As an example, the format sequence’%d’ means that
the first expression in the tuple should be formatted as an
integer (d stands for “decimal”):



.. sourcecode:: python

    >>> camels = 42
    >>> '%d' % camels
    '42'



The result is the string ’42’, which is not to be confused
with the integer value 42.



A format sequence can appear anywhere in the format string,
so you can embed a value in a sentence:



.. sourcecode:: python

    >>> camels = 42
    >>> 'I have spotted %d camels.' % camels
    'I have spotted 42 camels.'



The format sequence ’%g’ formats the next element in the tuple
as a floating-point number (don
’t ask why), and ’%s’ formats
the next item as a string:



.. sourcecode:: python

    >>> 'In %d years I have spotted %g %s.' % (3, 0.1, 'camels')
    'In 3 years I have spotted 0.1 camels.'



The number of elements in the tuple has to match the number
of format sequences in the string. Also, the types of the
elements have to match the format sequences:







.. sourcecode:: python

    >>> '%d %d %d' % (1, 2)
    TypeError: not enough arguments for format string
    >>> '%d' % 'dollars'
    TypeError: illegal argument type for built-in operation



In the first example, there aren’t enough elements; in the
second, the element is the wrong type.



The format operator is powerful but difficult to use. You can
read more about it at docs.python.org/lib/typesseq-strings.html.

14.4  Filenames and paths
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~










Files are organized into directories (also called “folders”).
Every running program has a 
“current directory,” which is the
default directory for most operations. 
For example, when you open a file for reading, Python looks for it in the
current directory.







The os module provides functions for working with files and
directories (
“os” stands for “operating system”). os.getcwd
returns the name of the current directory:







.. sourcecode:: python

    >>> import os
    >>> cwd = os.getcwd()
    >>> print cwd
    /home/dinsdale



cwd stands for “current working directory.” The result in
this example is 
/home/dinsdale, which is the home directory of a
user named dinsdale.







A string like cwd that identifies a file is called a path.
A 
relative path starts from the current directory;
an 
absolute path starts from the topmost directory in the
file system.







The paths we have seen so far are simple filenames, so they are
relative to the current directory. To find the absolute path to
a file, you can use os.path.abspath:



.. sourcecode:: python

    >>> os.path.abspath('memo.txt')
    '/home/dinsdale/memo.txt'



os.path.exists checks
whether a file or directory exists:







.. sourcecode:: python

    >>> os.path.exists('memo.txt')
    True



If it exists, os.path.isdir checks whether it’s a directory:



.. sourcecode:: python

    >>> os.path.isdir('memo.txt')
    False
    >>> os.path.isdir('music')
    True



Similarly, os.path.isfile checks whether it’s a file.



os.listdir returns a list of the files (and other directories)
in the given directory:



.. sourcecode:: python

    >>> os.listdir(cwd)
    ['music', 'photos', 'memo.txt']



To demonstrate these functions, the following example
“walks” through a directory, prints
the names of all the files, and calls itself recursively on
all the directories.







.. sourcecode:: python

    def walk(dir):
        for name in os.listdir(dir):
            path = os.path.join(dir, name)
    
            if os.path.isfile(path):
                print path
            else:
                walk(path)



os.path.join takes a directory and a file name and joins
them into a complete path. 



Exercise 1  
Modify 
walk so that instead of printing the names of
the files, it returns a list of names.



Exercise 2  
The 
os module provides a function called walk
that is similar to this one but more versatile. Read
the documentation and use it to print the names of the
files in a given directory and its subdirectories.

14.5  Catching exceptions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






A lot of things can go wrong when you try to read and write
files. If you try to open a file that doesn
’t exist, you get anIOError:







.. sourcecode:: python

    >>> fin = open('bad_file')
    IOError: [Errno 2] No such file or directory: 'bad_file'



If you don’t have permission to access a file:







.. sourcecode:: python

    >>> fout = open('/etc/passwd', 'w')
    IOError: [Errno 13] Permission denied: '/etc/passwd'



And if you try to open a directory for reading, you get



.. sourcecode:: python

    >>> fin = open('/home')
    IOError: [Errno 21] Is a directory



To avoid these errors, you could use functions like os.path.exists
and 
os.path.isfile, but it would take a lot of time and code
to check all the possibilities (if 
“Errno 21” is any
indication, there are at least 21 things that can go wrong).







It is better to go ahead and try, and deal with problems if they
happen, which is exactly what the 
try statement does. The
syntax is similar to an if statement:



.. sourcecode:: python

    try:    
        fin = open('bad_file')
        for line in fin:
            print line
        fin.close()
    except:
        print 'Something went wrong.'



Python starts by executing the try clause. If all goes
well, it skips the 
except clause and proceeds. If an
exception occurs, it jumps out of the 
try clause and
executes the except clause.



Handling an exception with a try statement is called catching an exception. In this example, the except clause
prints an error message that is not very helpful. In general,
catching an exception gives you a chance to fix the problem, or try
again, or at least end the program gracefully.

14.6  Databases
~~~~~~~~~~~~~~~~~~~~~~~~~






A database is a file that is organized for storing data.
Most databases are organized like a dictionary in the sense
that they map from keys to values. The biggest difference
is that the database is on disk (or other permanent storage),
so it persists after the program ends.







The module anydbm provides an interface for creating
and updating database files. As an example, I
’ll create a database
that contains captions for image files.







Opening a database is similar
to opening other files:



.. sourcecode:: python

    >>> import anydbm
    >>> db = anydbm.open('captions.db', 'c')



The mode ’c’ means that the database should be created if
it doesn
’t already exist. The result is a database object
that can be used (for most operations) like a dictionary.
If you create a new item, anydbm updates the database file.







.. sourcecode:: python

    >>> db['cleese.png'] = 'Photo of John Cleese.'



When you access one of the items, anydbm reads the file:



.. sourcecode:: python

    >>> print db['cleese.png']
    Photo of John Cleese.



If you make another assignment to an existing key, anydbm replaces
the old value:



.. sourcecode:: python

    >>> db['cleese.png'] = 'Photo of John Cleese doing a silly walk.'
    >>> print db['cleese.png']
    Photo of John Cleese doing a silly walk.



Many dictionary methods, like keys and items, also
work with database objects. So does iteration with a 
for
statement.







.. sourcecode:: python

    for key in db:
         print key



As with other files, you should close the database when you are
done:



.. sourcecode:: python

    >>> db.close()





14.7  Pickling
~~~~~~~~~~~~~~~~~~~~~~~~






A limitation of anydbm is that the keys and values have
to be strings. If you try to use any other type, you get an
error.







The pickle module can help. It translates
almost any type of object into a string suitable for storage in a
database, and then translates strings back into objects.



pickle.dumps takes an object as a parameter and returns
a string representation (dumps is short for “dump string”):



.. sourcecode:: python

    >>> import pickle
    >>> t = [1, 2, 3]
    >>> pickle.dumps(t)
    '(lp0\nI1\naI2\naI3\na.'



The format isn’t obvious to human readers; it is meant to be
easy for 
pickle to interpret. pickle.loads
(“load string”) reconstitutes the object:



.. sourcecode:: python

    >>> t1 = [1, 2, 3]
    >>> s = pickle.dumps(t1)
    >>> t2 = pickle.loads(s)
    >>> print t2
    [1, 2, 3]



Although the new object has the same value as the old, it is
not (in general) the same object:



.. sourcecode:: python

    >>> t == t2
    True
    >>> t is t2
    False



In other words, pickling and then unpickling has the same effect
as copying the object.



You can use pickle to store non-strings in a database.
In fact, this combination is so common that it has been
encapsulated in a module called shelve. 







Exercise 3  





If you did Exercise 12.4, modify your solution so that
it creates a database that maps from each word in the list to
a list of words that use the same set of letters.



Write a different program that opens the database and prints
the contents in a human-readable format.



14.8  Pipes
~~~~~~~~~~~~~~~~~~~~~






Most operating systems provide a command-line interface,
also known as a 
shell. Shells usually provide commands
to navigate the file system and launch applications. For
example, in Unix, you can change directories with 
cd,
display the contents of a directory with 
ls, and launch
a web browser by typing (for example) firefox.







Any program that you can launch from the shell can also be
launched from Python using a 
pipe. A pipe is an object
that represents a running process.



For example, the Unix command ls -l normally displays the
contents of the current directory (in long format). You can
launch ls with os.popen:







.. sourcecode:: python

    >>> cmd = 'ls -l'
    >>> fp = os.popen(cmd)



The argument is a string that contains a shell command. The
return value is a file pointer that behaves just like an open
file. You can read the output from the 
ls process one
line at a time with 
readline or get the whole thing at
once with read:







.. sourcecode:: python

    >>> res = fp.read()



When you are done, you close the pipe like a file:







.. sourcecode:: python

    >>> stat = fp.close()
    >>> print stat
    None



The return value is the final status of the ls process;None means that it ended normally (with no errors).







A common use of pipes is to read a compressed file incrementally;
that is, without uncompressing the whole thing at once. The
following function takes the name of a compressed file as a
parameter and returns a pipe that uses 
gzip to decompress
the contents:



.. sourcecode:: python

    def open_gzip(filename):
        cmd = 'gunzip -c ' + filename
        fp = os.popen(cmd)
        return fp



If you read lines from fp one at a time, you never have
to store the uncompressed file in memory or on disk.

14.9  Writing modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~










Any file that contains Python code can be imported as a module.
For example, suppose you have a file named 
wc.py with the following
code:



.. sourcecode:: python

    def linecount(filename):
        count = 0
        for line in open(filename):
            count += 1
        return count
    
    print linecount('wc.py')



If you run this program, it reads itself and prints the number
of lines in the file, which is 7.
You can also import it like this:



.. sourcecode:: python

    >>> import wc
    7



Now you have a module object wc:







.. sourcecode:: python

    >>> print wc<module 'wc' from 'wc.py'>



That provides a function called linecount:



.. sourcecode:: python

    >>> wc.linecount('wc.py')
    7



So that’s how you write modules in Python.



The only problem with this example is that when you import
the module it executes the test code at the bottom. Normally
when you import a module, it defines new functions but it
doesn’t execute them.







Programs that will be imported as modules often
use the following idiom:



.. sourcecode:: python

    if __name__ == '__main__':
        print linecount('wc.py')



__name__ is a built-in variable that is set when the
program starts. If the program is running as a script,
__name__ has the value __main__; in that
case, the test code is executed. Otherwise,
if the module is being imported, the test code is skipped.



Exercise 4  
Type this example into a file named 
wc.py and run
it as a script. Then run the Python interpreter and
import wc. What is the value of __name__
when the module is being imported?

Warning: If you import a module that has already been imported,
Python does nothing. It does not re-read the file, even if it has
changed.







If you want to reload a module, you can use the built-in function 
reload, but it can be tricky, so the safest thing to do is
restart the interpreter and then import the module again.



14.10  Debugging
~~~~~~~~~~~~~~~~~~~~~~~~~~






When you are reading and writing files, you might run into problems
with whitespace. These errors can be hard to debug because spaces,
tabs and newlines are normally invisible:



.. sourcecode:: python

    >>> s = '1 2\t 3\n 4'
    >>> print s
    1 2  3
     4







The built-in function repr can help. It takes any object as an
argument and returns a string representation of the object. For
strings, it represents whitespace
characters with backslash sequences:



.. sourcecode:: python

    >>> print repr(s)
    '1 2\t 3\n 4'



This can be helpful for debugging.



One other problem you might run into is that different systems
use different characters to indicate the end of a line. Some
systems use a newline, represented 
\n. Others use
a return character, represented 
\r. Some use both.
If you move files between different systems, these inconsistencies
might cause problems.







For most systems, there are applications to convert from one
format to another. You can find them (and read more about this
issue) at 
wikipedia.org/wiki/Newline. Or, of course, you
could write one yourself.

14.11  Glossary
~~~~~~~~~~~~~~~~~~~~~~~~~


:persistent: Pertaining to a program that runs indefinitely
  and keeps at least some of its data in permanent storage.
:format operator: An operator, %, that takes a format
  string and a tuple and generates a string that includes
  the elements of the tuple formatted as specified by the format string.
:format string: A string, used with the format operator, that
  contains format sequences. 
:format sequence: A sequence of characters in a format string,
  like 
  %d, that specifies how a value should be formatted.
:text file: A sequence of characters stored in permanent
  storage like a hard drive.
:directory: A named collection of files, also called a folder.
:path: A string that identifies a file.
:relative path: A path that starts from the current directory.
:absolute path: A path that starts from the topmost directory
  in the file system.
:catch: To prevent an exception from terminating
  a program using the 
  try
  and 
  except statements.
:database: A file whose contents are organized like a dictionary
  with keys that correspond to values.


14.12  Exercises
~~~~~~~~~~~~~~~~~~~~~~~~~~


Exercise 5  





The urllib module provides methods for manipulating URLs
and downloading information from the web. The following example
downloads and prints a secret message from thinkpython.com:



.. sourcecode:: python

    import urllib
    
    conn = urllib.urlopen('http://thinkpython.com/secret.html')
    for line in conn.fp:
        print line.strip()



Run this code and follow the instructions you see there.









Exercise 6  





In a large collection of MP3 files, there may be more than one
copy of the same song, stored in different directories or with
different file names. The goal of this exercise is to search for
these duplicates.



# Write a program that searches a directory and all of its
  subdirectories, recursively, and returns a list of complete paths
  for all files with a given suffix (like 
  .mp3).
  Hint: 
  os.path provides several useful functions for
  manipulating file and path names.
# To recognize duplicates, you can use a hash function that
  reads the file and generates a short summary
  of the contents. For example,
  MD5 (Message-Digest algorithm 5) takes an arbitrarily-long
  “message” and returns a 128-bit “checksum.” The probability
  is very small that two files with different contents will
  return the same checksum.
  You can read about MD5 at wikipedia.org/wiki/Md5. On
  a Unix system you can use the program 
  md5sum and a pipe to
  compute checksums from Python.






Exercise 7  





The Internet Movie Database (IMDb) is an online collection of
information about movies. Their database is available
in plain text format, so it is reasonably easy to read from
Python. For this exercise, the files you need
are 
actors.list.gz and actresses.list.gz; you
can download them from www.imdb.com/interfaces#plain.







I have written a program that parses these files and
splits them into actor names, movie titles, etc. You can
download it from thinkpython.com/code/imdb.py.



If you run imdb.py as a script, it reads actors.list.gz
and prints one actor-movie pair per line. Or, if you 
import
imdb
 you can use the function process_file to, well,
process the file. The arguments are a filename, a function
object and an optional number of lines to process. Here is
an example:



.. sourcecode:: python

    import imdb
    
    def print_info(actor, date, title, role):
        print actor, date, title, role
    
    imdb.process_file('actors.list.gz', print_info)



When you call process_file, it opens filename, reads the
contents, and calls 
print_info once for each line in the file.
print_info takes an actor, date, movie title and role as
arguments and prints them.



# Write a program that reads actors.list.gz and actresses.list.gz and uses shelve to build a database
  that maps from each actor to a list of his or her films.
# Two actors are “costars” if they have been in at least one
  movie together. Process the database you built in the previous step
  and build a second database that maps from each actor to a list of
  his or her costars.
# Write a program that can play the “Six Degrees of Kevin
  Bacon,
  ” which you can read about at
  wikipedia.org/wiki/Six_Degrees_of_Kevin_Bacon. This
  problem is challenging because it requires you to find the shortest
  path in a graph. You can read about shortest path algorithms
  at wikipedia.org/wiki/Shortest_path_problem.




