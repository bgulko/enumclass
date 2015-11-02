# enumclass
Smarter compile time ENUM for c++, low memory, high performance, but provides access to iterators, labels, and is Intellisense friendly.

This code provides C++ with an enum construct that is a true (inheritable) class,
while providing the memory footprint, compile time resolution and performance of
the classical enum / enum class construct.

<ul>Desiderata:
<li> No more memory or CPU overhead than standard enum, for standard enum ops.
<li> Compile time resolution of standard enum ops.
<li> Full range of integral container types (int, unsigned long, long long)
<li> Iterator access to enum values.
<li> set / get enum by value, string label or index (into list of defined enums)
<li> Initialization via explicit label value, label equivalence, or automatic increment.
<li> External dependencies limited to STL (no BOOST).
<li> Limit namespace pollution by enum value labels.

All code is in a single header file, with no outside libraries, or dependencies beyond the STL.

Define a simple enum, use <tt>enumclass( foo, first, second, third );</tt>
To instantiate the use <tt>enum { first, second, third } foo;</tt>

To override the default integral container type of int, with another type
(e.g. unsigned long long) use <tt>enumclassT( foo, unsigned long long, first, second, third );</tt>
  
Autocomplete mechanisms, such as MSVC Intellisense are supported through native
use of class-encapsulated enum, so enum values can be referred to as 
  <tt>foo::first, foo::second</tt>
To access a list of values, without pollution of enum label name space  use
  <tt>foo::val::first</tt>
  
Simple enum access is provided buy a class, with a memory footprint equal to the base
container type. More advanced options (like iteration, string label lookup, and reverse lookup)
are accomplished by a singleton class allocated once during static object initialization, using
the <a href="https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Construct_On_First_Use"> Construct on First Use</a> idiom to adddress the <a href="https://isocpp.org/wiki/faq/ctors#static-init-order"> Static Init Fiasco</a>. This results in a static object per enum TYPE (<i>not</i> per instance) that is never deallocated. While messy, just consider this  analogous to the memory footprint of a compiled class definition, but allocated at runtime rather than statically in the binary. Enum values are compile time constant and do not require initialization to use.

Label values may defined automatically, explicitly or by reference, for example.<br>
<tt>  enumclass( foo2, first, second=3,third, last = third ); </tt><br>
the values of each label of foo2 will be, respectively, 0,3,4,4  with indicies 0,1,2,3. An instance of foo2 can created, as usual, by<br>
<tt>  foo2 val; </tt>
  
The following assignments are identical<br>
<tt>  val=foo2:second</tt><br>
<tt>  val="second"</tt><br>
<tt>  val=3</tt><br>

An attempt to set an enum to an invalid value will result in setting the FIRST defined
label, so it is recommended that the first value be an "invalid" element.

If multiple labels have the same value, the LAST name will be returned, for in the previous example<br>
<tt>  val=foo2::third</tt><br>
<tt>  val.name() == "last"</tt><br>
<tt>  val.index() == 3</tt><br>

To preserve compatibility, performance and  memory footprint, enumclass elements are
stored by VALUE not index, so<br>
<tt>  foo2::third == foo2::last</tt><br>
Even though<br>
<tt>  foo2::name(2) != foo2::name(3) </tt><br>
Complete list of names, indices and values can be accessed at runtime using the 
<tt>.begin()</tt> and <tt>.end()</tt> iterators,  or looping over index values from <tt>.first()</tt> to <tt>.last()</tt> .

-- Brad
02-Nov-2015.
