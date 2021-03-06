TomDoc for Python - Version 0.0.1
===================================

Purpose
-------

TomDoc is a code documentation specification that helps you write precise
documentation that is nice to read in plain text, yet structured enough to be
automatically extracted and processed by a machine.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.

Table of Contents
-----------------

* [Method Documentation](#method)
  * [The Description Section](#description)
  * [The Arguments Section](#arguments)
  * [The Examples Section](#examples)
  * [The Returns/Raises Section](#returns)
* [Class/Module Documentation](#class)
* [Constants Documentation](#constants)
* [Special Considerations](#special)
  * [Constructor](#constructor)


<a name="method" />
Method Documentation
--------------------

A quick example will serve to best illustrate the TomDoc method documentation
format:

```python
def multiplex(text, count):
    """Public: Duplicate some text an arbitrary number of times.
 
    text  - The String to be duplicated.
    count - The Integer number of times to duplicate the text.
    
    Examples
    
      multiplex('Tom', 4)
      >>> 'TomTomTomTom'
    
    Returns the duplicated String.

    """
    return text * count
```

TomDoc for a specific method consists of a block of single comment markers (#)
that appears directly above the method. There SHOULD NOT be a blank line
between the comment block and the method definition. A TomDoc method block
consists of four optional sections: a description section, an arguments
section, an examples section, and a returns section. Sections MUST appear in
the order listed above. Lines that contain text MUST be separated from the
comment marker by a single space. Lines that do not contain text SHOULD consist
of just a comment marker (no trailing spaces).

<a name="description" />
### The Description Section

The description section SHOULD be in plain sentences. Each sentence SHOULD end
with a period. Good descriptions explain what the code does at a high level.
Make sure to explain any unexpected behavior that the method may have, or any
pitfalls that the user may experience. Paragraphs SHOULD be separated with
blank lines. Code within the description section should be indented three
spaces from the starting comment symbol. Lines SHOULD be wrapped at 80
characters.

To describe the status of a method, you SHOULD use one of several prefixes:

**Public:** Indicates that the method is part of the project's public API.
This annotation is designed to let developers know which methods are
considered stable. You SHOULD use this to document the public API of your
project. This information can then be used along with [Semantic
Versioning](http://semver.org) to inform decisions on when major, minor, and
patch versions should be incremented.

    # Public: Initialize a new Widget.

**Internal:** Indicates that the method is part of the project's
internal API. These are methods that are intended to be called from other
classes within the project but not intended for public consumption. For
example:

    # Internal: Normalize the filename.

**Deprecated:** Indicates that the method is deprecated and will be removed
in a future version. You SHOULD use this to document methods that were Public
but will be removed at the next major version.

    # Deprecated: Resize an object to the given dimensions.

An example description that includes all of these elements might look
something like the following.

    """Public: Format some data with the given format. Possible format
    identifiers include:
    
    %i   - Output the Integer i.
    %f.n - Output a Float f with n decimal places rounded.
    
    The format String may include any text. To escape a percent sign, prefix
    it with a backslash:
    
      "The sale price was %f.n\% off retail."

    """

<a name="arguments" />
### The Arguments Section

The arguments section consists of a list of arguments. Each list item MUST be
comprised of the name of the argument, a dash, and an explanation of the
argument in plain sentences. The expected type (or types) of each argument
SHOULD be clearly indicated in the explanation. When you specify a type, use
the proper classname of the type (for instance, use 'String' instead of
'string' to refer to a String type). If the argument has other constraints
(e.g. duck-typed method requirements), simply state those requirements. The
dashes following each argument name SHOULD be lined up in a single column.
Lines SHOULD be wrapped at 80 columns.  Wrapped lines MUST be indented, as a
block, under the parent line by at least two spaces, but SHOULD be indented
to match the indentation of the explanation. For example:

    element - The Symbol representation of the element. The Symbol should
              contain only lowercase ASCII alpha characters.

An argument that is String-like might look like this:

    actor - An object that responds to str(). Represents the actor that
            will be output in the log.

All arguments are assumed to be required. If an argument is optional, you MUST
specify the default value:

    host - The String hostname to bind (default: '0.0.0.0').

For hash arguments, you SHOULD enumerate each valid option in a way similar
to how normal arguments are defined:

    options - The Dict options used to refine the selection (default: {}):
              :color  - The String color to restrict by (optional).
              :weight - The Float weight to restrict by. The weight should
                        be specified in grams (optional).

Python allows for some interesting argument capabilities. In those cases, try
to explain what's going on as best as possible. Examples are a good way to
demonstrate how methods should be invoked. For example:

```python
def log(*msgs, func=None):
    """Print a log line to STDOUT. You can customize the output by specifying
    a block.
    
    msgs - Zero or more String messages that will be printed to the log
  	   separated by spaces.
    func - An optional callable that can be used to customize the date format.
           If it is present, it will be sent a datetime object representing
	   the current time. Your function should return a String version of
           the time, formatted however you please.
    
    Examples
    
      log("An error occurred.")
    
      log("No such file", "/var/log/server.log", func=lambda x: str(x))
    
    Returns nothing.

    """
    ...
```

<a name="examples" />
### The Examples Section

The examples section MUST start with the word "Examples" on a line by
itself. The next line SHOULD be blank. The following lines SHOULD be indented
by two spaces (three spaces from the initial comment marker) and contain code
that shows how to call the method and OPTIONAL examples of what it
returns. Everything under the "Examples" line is considered code, so
you SHOULD comment out lines that show return values. For example:

    Examples
    
      multiplex('x', 4)
      >>> 'xxxx'
    
      multiplex('apple', 2)
      >>> 'appleapple'


<a name="returns" />
### The Returns/Raises Section

The returns section should explain in plain sentences what is returned from
the method. The line MUST begin with "Returns". If only a single thing is
returned, state the nature and type of the value. For example:

    Returns the duplicated String.

If several different types may be returned, list all of them. For example:

    Returns the given element Symbol or None if none was found.

If the return value of the method is not intended to be used, then you should
simply state:

    Returns nothing.

If the method raises exceptions that the caller may be interested in, add
additional lines that explain each exception and under what conditions it may
be encountered. The lines MUST begin with "Raises". For example:

    Returns nothing.
    Raises OSError if the file cannot be found.
    Raises OSError if the file cannot be accessed.

Lines SHOULD be wrapped at 80 columns. Wrapped lines MUST be indented, as a
block, under the parent line by at least two spaces. For example:

    Returns the atomic mass of the element as a Float. The value is in
      unified atomic mass units.


<a name="class" />
Class/Module Documentation
--------------------------

TomDoc for classes and modules follows the same form as Method Documentation
but only contains the Description and Examples sections.

```python
class Math(object):
    """Public: Various methods useful for performing mathematical operations.
    All methods are module methods and should be called on the Math module.
    
    Examples
    
      Math.square_root(9)
      >>> 3

    """
    ...
```

Just like methods, classes may be marked as Public, Internal, or Deprecated
depending on their intended use.


<a name="constants" />
Constants Documentation
-----------------------

Constants should be documented with freeform comments. The type of the
constant and any important constraints should be stated.

    # Public: Integer number of seconds to wait before connection timeout.
    CONNECTION_TIMEOUT = 60

Just like methods, constants may be marked as Public, Internal, or Deprecated
depending on their intended use.


<a name="special" />
Special Considerations
----------------------

<a name="constructor" />
### Constructor

A Python class's `__init__` method does not have a significant return value.
You MAY exclude the returns section. A larger description of the purpose of
this class should be done at the Class level.

```python
    def __init__(self, name)
        """Public: Initialize a Widget.
    
        name - A String naming the widget.

        """"
        ...
```
