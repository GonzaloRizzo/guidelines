Table of Contents
=================

#. `Books`_
#. `Tools`_
#. `Coding Style & Correctness`_


Books
=====

#. `The C++ Programming Language (4th Edition) <http://www.stroustrup.com/4th.html>`__


Tools
=====

#. `doxygen <http://www.stack.nl/~dimitri/doxygen/>`__

   *"Doxygen is the de facto standard tool for generating documentation from annotated C++ sources."*

#. `cmake <https://cmake.org/>`__

   *"CMake is an open-source, cross-platform family of tools designed to build, test and package software. CMake is used to control the software compilation process using simple platform and compiler independent configuration files, and generate native makefiles and workspaces that can be used in the compiler environment of your choice."*

#. `valgrind <http://valgrind.org/>`__

   *"Valgrind is an instrumentation framework for building dynamic analysis tools. There are Valgrind tools that can automatically detect many memory management and threading bugs, and profile your programs in detail. You can also use Valgrind to build new tools."*


Coding Style & Correctness
==========================

Efforts are being made by the `Standard C++ Foundation <https://isocpp.org/about>`__
to come up with a set of guidelines for writing good modern C++ code
`CppCoreGuidelines <https://github.com/isocpp/CppCoreGuidelines/>`__.
Even though the document is still a work in progress there's already a lot of
very useful information in it. Don't forget to read it.


Spaces & Tabs
^^^^^^^^^^^^^

+ Tabs should not be used in any C/C++ source file (configure your editor to
  insert spaces when the ``tab`` key is pressed).
+ Use 4 spaces for indentation.


Headers
^^^^^^^

#. Header files should be named with ``snake_case`` and use the ``.hpp``
   extension for C++ headers, and ``.h`` for plain C headers.

   - ``file_system.hpp``
   - ``array_view.hpp``
   - ``unused.hpp``

#. Header guards should be named as follows:

   .. code-block:: c++

       #ifndef <NAMESPACE>_<PATH>_CLASS_NAME_HPP_
       #define <NAMESPACE>_<PATH>_CLASS_NAME_HPP_

       #endif


   Example:

   ``src/foo/bar/my_class.hpp``

   .. code-block:: c++

      #ifndef NS_FOO_BAR_MY_CLASS_HPP_
      #define NS_FOO_BAR_MY_CLASS_HPP_

      namespace ns {

      class MyClass {
      };

      } /* ns */

      #endif


Includes
^^^^^^^^

The general order for #include's should be as follows:

.. code-block:: text

    header include
    C system headers
    C++ system headers
    third party includes
    your own code's includes.

Example:

.. code-block:: c++

    #include "foo.hpp"

    #include <ctime>
    #include <vector>

    #include <other/library.hpp>

    #include "my/library/example.hpp"


Naming Conventions
^^^^^^^^^^^^^^^^^^

#. **Types** must be in ``UpperCamelCase``.

    .. code-block:: c++

        class FileSystem { };
        struct User { };


#. **Variables** must be in ``snake_case``.

    .. code-block:: c++

        int user_count;
        int balance;

    Member variables should have an ``m_`` prefix

    .. code-block:: c++

        class MyClass {
        public:
            // ...
        protected:
            // ...
        private:
            int m_number;
        };


#. **Functions** must be verbs written in ``snake_case``.

   .. code-block:: c++

       int compute_total();
       void clear();


#. **Namespaces** must be written in ``lowercase``.

   .. code-block:: c++

       namespace io { };
       namespace math { };


Syntax
^^^^^^

#. **Braces**

   Just follow the following examples regarding `brace` and `bracket` placement.

   .. code-block:: c++

       if (condition) {
           // .....
       }

       for (const auto& variable : iterable) {
           // .....
       }

       while (condition) {
           // .....
       }

       switch (something) {
       case Something:
           break;
       }

       void do_something()
       {
           // .....
       }

       class Class {
       public:
           int m_x;
       };

       void do_something()
       {
           // Extra braces on their own line.
           {
               // Another Scope
               int x;
               x++;
           }
       }

       namespace sophi {

       // Stuff inside the namespace block has the same indentation as
       // the block itself;

       class MyClass {
       };

       } /* sophi */


   The final /* sophi \*/ comment is mandatory.



#. **Blank Lines**

   Blank lines should be used sparingly. Inside a function/class body you should
   use at most one blank line between statements. Inbetween function/class
   definitions you should use two blank lines.

   Example:

   .. code-block:: c++

       // Sparate function definitions with two blank lines

       int a_function()
       {
           return 0;
       }


       void another_function()
       {
           // ......
       }

   .. code-block:: c++

       // Separate class declarations with two blank lines.

       namespace ns {

       class A {
       };


       class B {
       };

       } /* ns */


   You should also separate the ``includes`` section from the rest of the code
   by two blank lines

   .. code-block:: c++

       #include "foo.hpp"

       #include <vector>


       void ns::Foo::my_function()
       {
       }
