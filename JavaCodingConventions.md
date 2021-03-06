# Java Code Conventions



----


This document contains the standard conventions that should be followed when
writing Java code for the Spacewalk project.  Everything not specifically
addressed by this document should follow the official [Code Conventions for the Java Programming Language](http://www.oracle.com/technetwork/articles/javase/codeconvtoc-136057.html).

Conventions specific to Spacewalk have been primarily drawn from the
conventions imposed by the various sub-projects of the [Apache Jakarta Project](http://jakarta.apache.org/), or have been invented to accommodate matters of internal style and 
practicality. To help people conform to the document conventions, we have
integrated [checkstyle](http://checkstyle.sourceforge.net/) into our build
tree.  Make sure you run "ant checkstyle" before checking in code.  Failing
checkstyle will cause our integration build system to complain about broken
builds.

The checkstyle configuration file (java/buildconf/checkstyle.xml) is our
definitive style guide source.  The conventions here should be putting
those conventions into english and for things checkstyle can't check.
## Brackets



 All left brackets should be the end of the line and all right brackets should be alone on the line.

 
```java
     // Correct
     public class SomeClass {
         public void someMethod() {
             if (...) {
                 try {
                     // do something
                 }
                 catch (IOException ioe) {
                     // handle exception
                 }
                 catch (SecurityException se) {
                     // handle exception
                 }
             }
             else if (...) {
                 // do something else.
             }
         }
     }

     // Incorrect
     public class SomeClass {
         public void someMethod()
         {
             if (...)
             {
                 try {
                     // do something
                 } catch (IOException ioe) {
                     // handle exception
                 } catch (SecurityException se) {
                     // handle exception
                 }
             } else if (...) {
                 // do something else.
             }
         }
     }
```
    
Brackets are mandatory even for single line statements!

```java
     // Correct
     if (expression) {
         // some code
     }
     // Incorrect
     if (expression)
         // some code
```
    

 
## Directories

Files and Directories should be named such that no case-insensitive duplications occur (i.e.  don't create a directory named "build" in a directory where a file named "BUILD" exists).
## Naming

The following conventions are meant to further refine the conventions described by [section 9 Oracle's code conventions](http://www.oracle.com/technetwork/java/javase/documentation/codeconventions-135099.html#367)
    
* Packages 
        All in house packages should be a subpackage of "com.redhat.rhn"
        Example: package com.redhat.rhn.common
    
* Classes
        Use of '_' in classnames should be avoided but is not strictly 
        prohibited.  An appropriate use of '_' in a class name would be
        to separate multiple back-to-back acronyms (after seriously considering
        whether the chosen classname is appropriate).
        Example:  class SSL_RPCSocket
    
* Interfaces
        Interface names should follow the conventions for class names. They should
        *NOT* be prefixed with an I such as IUser.
        Example:  interface SSL_RPCService
    
* Methods
        The use of '_' should be avoided in method names.
    
* Variables
        The use of '_' should be avoided in variable names.
    
* Constants
        The names of constants should not include a leading '_'.

## Whitespace

* Tab characters are not allowed in the source code.

* No space should appear after the right parenthesis for typecasts

```java
        // Correct
        String myString = (String)list.get(1);
    
    
        // Incorrect
        String myString = (String) list.get(1);
 ```
        

* Whitespace should appear between the following tokens and their subsequent open parenthesis: assert, catch, for, if, synchronized, switch, while in accordance with [section 8 of Oracle's code conventions](http://www.oracle.com/technetwork/java/javase/documentation/codeconventions-141388.html#475)
    
```java
        // Correct
        while (this == that) {
            ...
        }
        
        // Incorrect
        while(this == that) {
             ^
        }
```
    
* The preference is to use a single space rather than a tab to separate the type and the identifier for variable declarations.
       Note: This is **contrary** to the Oracle's code conventions.
 
    
```java
        // Correct
        BigDecimal myNumber
        int level;
    
        // Incorrect
        BigDecimal   myNumber;
        int          level;
```

## Indentations

Indentation should be four spaces - **not** tabs.

    
* For emacs users, the following will produce four space indents rather than tabs:

        (setq-default tab-width 4 indent-tabs-mode nil)

* Vim users can add the following to their .vimrc

        set ts=4
        set expandtab

## Line length



Avoid lines longer than 92 characters.  This is contrary to [section 4.1
of Oracle's code conventions](http://www.oracle.com/technetwork/java/javase/documentation/codeconventions-136091.html#313).
While we allow 92 characters, many developers strive to keep to the 80 character limit.

## Comments

Javadoc comments *SHOULD* exist on all non-private methods and fields.

Comments on methods should indicate any assumptions made about the method's
parameters. Also, if you are working on existing code and the code is missing
javadoc comments, then you should add comments or call it to the attention of
an appropriate developer. Checkstyle looks for "{@inheritDoc}" to determine
that the method inherits its comments from a parent class.

Methods or classes that have been incompletely implemented or for which the
developer has specific ideas for improvement should have a @todo javadoc
comment indicating what work remains, followed by the developer's account
name and a date. Elaborate with further comments interspersed in the code as
necessary.


```java
    /**
     * ...
     * @todo Figure out how best to handle IOException here (kdykeman - 2003/10/08)
     */
```
 
Please refer to 
[[http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html]]
if you are unfamiliar with Javadoc.

Note that while javadocs are encouraged for test classes where
appropriate, checkstyle does not require javadoc comments for test
classes.

## License

The following license message should be placed at the top of each
Spacewalk generated source file:


```java
    /**
     * Copyright (c) 2008 Red Hat, Inc.
     *
     * This software is licensed to you under the GNU General Public License,
     * version 2 (GPLv2). There is NO WARRANTY for this software, express or
     * implied, including the implied warranties of MERCHANTABILITY or FITNESS
     * FOR A PARTICULAR PURPOSE. You should have received a copy of GPLv2
     * along with this software; if not, see
     * http://www.gnu.org/licenses/old-licenses/gpl-2.0.txt.
     * 
     * Red Hat trademarks are not licensed under GPLv2. No permission is
     * granted to use or replicate Red Hat trademarks that are incorporated
     * in this software or its documentation. 
     */
```

Note: The way the license file was written, checkstyle expects a space after the * separating the second and third paragraphs as well as a space after the period at the end of the last line.

## Versioning

All .java files should have a @version Javadoc tag like the one below.


    @version $Revision: 1.2 $

## Qualified imports

All import statements should contain the full class name of classes to 

import and should not use the "*" notation (Eclipse can help do this quickly):
An example:

```java
    // Correct
    import java.net.HttpURLConnection;
    import java.util.Date;
    
    // Incorrect
    import java.util.*;
    import java.net.*;
```

## Logging

Do not use System.out or System.err to log, instead, use the 

[log4j](http://logging.apache.org/log4j/1.2/index.html) API.
Check the logging level prior to logging messages with a level more 
detailed than warn to avoid unnecessary String object creation and 
concatenations.  For example:


```java
    private static final Logger log = Logger.getLogger(MyClass.class);
    
    public void someMethod() {
        // Check to make sure that debug logging is enabled.
        if (log.isDebugEnabled()) {
            log.debug("some debug text" + debugMsg.toString());
        }
        ...
        log.error("something went wrong");
    }
```

Furthermore, care should be taken to log messages at an appropriate level
for the information they present (keeping in mind that the default log
level is INFO).

    trace::
    designates finest-grained informational events that highlight the
    stepwise progress of the application.

    "Entering method foo"
    
    debug::
    designates fine-grained informational events that are most useful
    for debugging an application.

    "Item 'bar' removed from cache."
    
    info::
    designates informational messages that highlight the progress of
    the application at coarse-grained level.

    ''"The service started successfully"''
    
    warn::
    designates potentially harmful situations.

    ''"Property value not set, using default value"''
    
    error::
    designates error events that might still allow the application to
    continue running.

    ''"Failed to contact server, next attempt in 30 seconds."''

    fatal::
    designates very severe error events that will presumably lead the
    application to abort.

    ''"SLL certificate not found on startup."''

## Hidden Fields

Parameter names for a method should not shadow fields defined in the same

class.  Shadowing a field leads to situations that can result in
inadvertently using the wrong variable.  Below is a contrived example of
shadowing and the problems that it can introduce:


```java
    public class Foo {
        private String myString = "bar";
    
        public Foo(String myString) {
            this.myString = myString;
            myString = myString + "baz";  // Oops!
        }
    }
```

## File and Method lengths

Any of the following conditions should be taken as indication that a

class/method should be refactored:
 
 * Source files with a length greater than *2500* lines.
 * Methods with a length greater than *150* lines.
 * Methods taking more than *12* parameters.

## Exception handling

Avoid catching Throwable or Exception and catch specific exceptions instead.
If an exception is caught and rethrown as a different exception type, the new
exception should be constructed with the caught exception as the cause.  This
allows stack trace information for the original exception to be preserved.
If you catch an exception and decide not to rethrow it for whatever reason,
you should log it.  In particular, do not use the printStackTrace()
method because its output goes to stderr rather than to the logging system.

When logging an exception, invoke one of the logging methods taking two
parameters - a message and the exception.  The logging methods will not
complain if you simply pass the exception as a single parameter; however, the
logged message will not contain the valuable stack trace information, it will
just contain the results of calling toString() on the exception.

The following is an example of how to properly log a "handled" exception:

```java
    try {
        doSomethingRisky();
    }
    catch (RiskyException re) {
        log.error("risky failed", re);
    }
```