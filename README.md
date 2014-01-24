Overview
--------

[![Build Status](https://secure.travis-ci.org/edsu/pymarc.png)](http://travis-ci.org/edsu/pymarc)

pymarc is a python library for working with bibliographic data encoded in 
[MARC21](http://en.wikipedia.org/wiki/MARC_standards). It provides an API 
for reading, writing and modifying MARC records. It was mostly designed to 
be an emergency eject seat, for getting your data assets out of MARC and into
some kind of saner representation. However over the years it has been used 
to create and modify MARC records, since despite [repeated
calls](http://marc-must-die.info/index.php/Main_Page) for it to die as a
format, it seems to be living quite happily as a zombie. 

Below are some common examples of how you might want to use pymarc. If 
you run across an example that you think should be here please send a 
pull request.

### Reading

Most often you will have some MARC data and will want to extract data
from it. Here's an example of reading a batch of records and printing out 
the title. If you are curious this example uses the batch file 
available here in pymarc repository:

```python  
>>> from pymarc import MARCReader
>>> reader = MARCReader(open('test/marc.dat'))
>>> for record in reader: 
...    print record.title()
The pragmatic programmer :
Programming Python /
Learning Python /
Python cookbook /
Python programming for the absolute beginner /
Web programming :
Python programming on Win32 /
Python programming :
Python Web programming /
Core python programming /
Python and Tkinter programming /
Game programming with Python, Lua, and Ruby /
Python programming patterns /
Python programming with the Java class libraries :
Learn to program using Python :
Programming with Python /
BSD Sockets programming from a multi-language perspective /
Design patterns :
Introduction to algorithms /
ANSI Common Lisp /
```

A `pymarc.Record` has a few handy methods like `title` for getting at bits
of a bibliographic record, others include: `author`, `isbn`, `subjects`, 
`location`, `notes`, `physicaldescription`, `publisher`, `pubyear`. But 
really, to work with MARC data you need to understand the numeric field tags 
and subfield codes that are used to designate various bits of information. There
is a lot more hiding in a MARC record than these methods provide access to.
For example the `title` method extracts the information from the `245` field, 
subfield `a`. If you are new to MARC fields [Understanding
MARC](http://www.loc.gov/marc/umb/) is a pretty good primer, and the [MARC 21
Formats](http://www.loc.gov/marc/marcdocz.html) page at the Library of Congress is a good reference once you understand the basics.

### Writing

Here's an example of creating a record and writing it out to a file.

```python
>>> from pymarc import Record, Field
>>> record = Record()
>>> record.addField(
...     Field(
...         tag = '245', 
...         indicators = ['0','1'],
...         subfields = [
...             'a', 'The pragmatic programmer : ',
...             'b', 'from journeyman to master /', 
...             'c', 'Andrew Hunt, David Thomas.'
...         ]))
>>> out = open('file.dat', 'w')
>>> out.write(record.asMARC21())
>>> out.close()
```

### Updating

Modifying works the same way, you read it in, modify it, and then write it out
again:

```python
>>> from pymarc import MARCReader
>>> reader = MARCReader(open('test/marc.dat'))
>>> record = reader.next()
>>> record['245']['a'] = 'The Zombie Programmer'
>>> out = open('file.dat', 'w')
>>> out.write(record.asMARC21())
>>> out.close()
```


### JSON and XML

If you find yourself using MARC data a fair bit, and distributing it, you may 
make other developers a bit happier by using the JSON or XML serializations. 
pymarc has support for both. The main benefit here is that the UTF8 character
encoding is used, rather than the frustratingly archaic MARC8 encoding.

Installation
------------

You'll probably just want to use pip to install pymarc:

    pip install pymarc

If you'd like to download and install the latest source you'll need git:

    git clone git://github.com/edsu/pymarc.git

You'll also need [setuptools](https://pypi.python.org/pypi/setuptools#installation-instructions). Once you have the source and setuptools run the pymarc test 
suite to make sure things are in order with the distribution:

    python setup.py test

And then install:

    python setup.py install

Support
-------

The pymarc developers encourage you to join the [pymarc Google Group](http://groups.google.com/group/pymarc) if you need help.  Also, please feel free to use [issue tracking](https://github.com/edsu/pymarc/issues) on Github to to submit feature requests or bug reports. If you've got an itch to scratch, please scratch it, and send merge requests on [Github](http://github.com/edsu/pymarc).

Copyright
---------

Copyright (c) 2005-2012 Gabriel Farrell, Mark Matienzo, Ed Summers

License
-------

[BSD](http://www.opensource.org/licenses/bsd-license.php)
