# enumclass
Smarter compile time ENUM for c++, low memory, high performance, but provides access to iterators, labels, and is Intellisense friendly.

This code provides C++ with an enum construct that is a true (inheritable) class,
while providing the memory footprint, compile time resolution and performance of
the classical enum / enum class construct.

Desiderata:
-) No more memory or CPU overhead than standard enum, for standard enum ops.
-) Compile time resolution of standard enum ops.
-) Full range of integral container types (int, unsigned long, long long)
-) Iterator access to enum values.
-) set / get enum by value, string label or index (into list of defined enums)
-) Initialization via explicit label value, label equivalence, or automatic increment.
-) External dependencies limited to STL (no BOOST).
-) Limit namespace pollution by enum value labels.

All code is in a single header file, with no outside libraries, or dependencies beyond the STL.

To instantiate a simple enum
  enum { first, second, third } foo;
Use the code
  enumclass( foo, first, second, third );

To override the default integral container type of int, with another type
(e.g. unsigned long long) utilize:
  enumclassT( foo, unsigned long long, first, second, third );
  
Autocomplete mechanisms, such as MSVC Intellisense are supported through native
use of class-encapsulated enum, so enum values can be referred to as 
  foo::first, foo::second
To access a list of values, without pollution of enum label name space  use
  foo::val::first
  
Simple enum access is provided buy a class, with a memory footprint equal to the base
container type. More advanced options (like iteration, string label lookup, and reverse lookup)
are accomplished by a singleton class allocated once during static object initialization, using
the Construct on First Use idiom to prevent Static Init Fiasco. Enum values are compile time
constant and do not require initialization to use.

Label values may defined automatically, explicitly or by reference, for example.
  enumclass( foo2, first, second=3,third, last = third );
  the values of each label of foo2 will, respectively, be 0,3,4,4
an instance of foo2 can created, as usual, by
  foo2 val; 
  
The following assignments are identical
  val=foo2:second
  val="second"
  val=3
  
An attempt to set an enum to an invalid value will result in setting the FIRST defined
label, so it is recommended that the first value be an "invalid" element.

If multiple labels have the same value, the LAST name will be returned, for in the previous example
  val=foo2::third
  val.name() == "last"
  val.index() == 3

To preserve compatibility, performance and  memory footprint, enumclass elements are
stored by VALUE not index, so
  foo2::third == foo2::last
Even though
  foo2::name(2) != foo2::name(3) 
Complete list of names, indices and values can be accessed at runtime using the 
.begin()/.end() iterators,  or looping over index values from .first() to .last()

-- Brad
02-Nov-2015.
