Code Standards
==============

Classes
-------

Classes SHOULD NOT be declared package-private.

Anonymous classes SHOULD NOT be used. Inner classes MUST NOT be used.

Constructors MUST NOT invoke overridable methods.

Class Constants, Properties, and Methods
----------------------------------------

### Constants

Constants MUST be declared static and final. Constants MAY be public, private
or protected.

Constants SHOULD NOT be used to declare a set of values Enums are preferable
to this.

### Properties

Properties SHOULD NOT be declared public unless they are considered Constants.
Properties SHOULD NOT be declared protected, or package-private.

### Methods

Methods SHOULD NOT be declared package-private.

Methods MUST include the `@Override` Annotation when overriding a non-deprecated
parent method.
