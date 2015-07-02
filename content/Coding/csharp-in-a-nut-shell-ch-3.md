Title: Reading Notes on C# 5.0 in a Nutshell Ch.3
Tags: Reading-Notes, Code, C#
Slug: csharp-in-a-nutshell-ch-3
Date: 2015-07-01 23:22

###Can't believe it took another month to finish the next chapter.

1. **readonly** modifier prevents a *field* from being assigned after construction. A **readonly** field can only be assigned in its declaration or in the enclosing constructor. (p68)

2. If *fields* are not initialized, the *fields* are initialized with default value. (p68)

3. **ref** and **out** modifier used in *functions* can also be used in *method*. (p69)

4. A **class** can have multiple *constructors*, and one constructor can call another by by putting `this(/* arguments for the corresponding constructor */)` after a `:`. (p70)

5. Like C++, C# generates a default public parameterless constructor for a class <font color="#DA1D1B">iff</font> no constructor is explicitly defined. (p70)

6. When a class is instantiated, the *fields* are initialized in the order the *fields* are declared and then the constructor is called. (p70)

7. Any accessible fields or properties can be initialized after construction via a *object initializer*. The initializer is defined within braces. (p71)

8. The way *initializer* works is that it first create a temporary object to initialize, then assign the object back to the original one. The reason for doing this is that if the initializer failed, the origin will not end up in half initialized state. (p71)

9. One more thing to be noted about the *object initializer* is that the object is first initialized with the corresponding constructor, and then assigned the fields or properties to the given values one by one. Hence those fields or properties cannot be **readonly**. (p72)

10. Besides referencing another constructor (entry 4), **this** can also be used to reference the instance itself. (p73)

11. a *property* looks like a *field* from the outside, but internally it can contain logic and access control can be applied to its accessor (**get** and **set**). (p73)

12. The **get** accessor must return a value of the property's type, and within the **set** accessor a special parameter named **value** can be used to get the value that's assigned to the property. (p73)

13. A property can be made read-only or write-only by specifying only the **get** accessor or **set** accessor. (p74)

14. A property typically has a dedicated backing field to store the underlying data. However, a property can also be computed from other data. (p74)

15. *automatic properties* can be used to instruct the compiler to prepare a private backing field for the property:

		:::C++
		public class Stock
		{
			public decimal CurrentPrice { get; set; }
		}
	(p74)

16. *Accessors* must not more accessible that the declaration of the property. I.e., if the property is **protected**, then its accessor(s) can only be **protected** or **private**. (p75)

17. Simple <font color="#DA1D1B">non-virtual</font> accessors can *inlined* by JIT compiler, eliminating any performance difference between accessing a property and a field. (75)

18. To defined an *indexer*, define a <font color="#DA1D1B">property</font> called **this**(the **this** is reused again), specifying the arguments in square brackets. For instance:

		:::C++
		public string this [int wordNum]      // indexer
		{
			get { return words [wordNum];  }
			set { words [wordNum] = value; }
		}

	*indexer* is essentially a special *property*. (p75)

19. A type may declare multiple indexers, each with parameters of different types. An indexer can also take more than one parameter. It can also be read-only or write-only by omitting the corresponding accessor. (p76)

20. Use **const** to declare a *constant*. A *constant* can <font color="#DA1D1B"><u>only</u></font> be any of the built-in numeric types, **bool**, **char**, **string** or an *enum* type. To declare a "constant" reference type, use **static readonly**. (p76)

21. **const** is more restrictive than **static readonly** as it's determined at compile time. **static readonly**  is determined at run time. (p76)

22. Besides normal constructors, C# has something called *static constructor* (what's the meaning of this?? Just something that makes the language even more complex..). This constructor is run once per type, when this type is being used:

	- Instantiting the type
	- Accessing a static member in the type
	(p77)

23. A class can be marked **static**, indicating that it must be composed solely of **static** members and <font color="#DA1D1B">cannot be subclassed</font>. The **System.Console** and **System.Math** classes are good examples of static classes. (p78)

24. *Finalizer* in C# can be defined like destructor in C++, only that no accessibility modifier is needed. (Finalizer is not discussed in detail here.) (p79)

25. The definition of a class can be split by using **partial**. <font color="#DA1D1B">**partial** must be specified in all the participants</font>, however the *base class* of the class can be specified in only some of the participants. Each participants can specify different *interface* to implement.

	To make the code easier to maintain, the *base class* should be declared in all the participants, but for the purpose of spliting implementation, *interface* can be declared only in the participants that's implementing it.

		:::C++
		partial class PartialClass { /* Partial implementation A */ };
		class PartialClass { /* Partial implementation B */ }; // Compile time error

		partial class PartialClass { /* Partial implementation A */ };
		partial class PartialClass { /* Partial implementation B */ }; // OK

		partial class PartialWithBase : Base { /* Partial implementation A */ };
		partial class PartialWithBase { /* Partial implementation B */ }; // OK

		partial class PartialWithBase : Base { /* Partial implementation A */ };
		partial class PartialWithBase : Base { /* Partial implementation B */ }; // Preferred

		partial class PartialWithItf : ItfA { /* Partial implementation A */ };
		partial class PartialWithItf : ItfB { /* Partial implementation B */ }; // Preferred

26. **partial** method can be declare within **partial** classes. A **partial** method consists of two parts: *definition* and *implementation* (implying that a **partial** method can only have at most two participants). If the *implementation* is not provided, the *definition* as well as the codes that call it is compiled away. (p80)

27. **partial** methods <font color="#DA1D1B">must be void and are implicitly private.</font> (p80)

28. C# adopts single inheritance. (p81)

29. Upcasting is implicit and will always succeed. Downcasting requires explicit operation and depends on whether the object is suitably typed (if downcast fails, an **InvalidCastException** is thrown). (P82)

30. Instead of using explicit downcasting, **as** operator can be used to perform downcasting as well, but it evaluates to **null** if casting fails. (p83)

31. The **as** operator cannot perform *custom conversions* and <font color="#DA1D1B">it cannot do numeric conversion.</font>

		:::C++
		long a = 3 as long; // compilation error
	(p83)

32. The **is** operator tests whether a reference conversion would succeed. (p83)

33. The signatures, return types, and accessibility of the **virtual** and *overridden* methods must be identical. (p84)

34. Calling **virtual** method in constructor is dangerous as the derived class might **override** the method and accessing uninitialized fields in it. (p84)

35. A class declared as **abstract** can never be instantiated. Instead, only its *concrete* subclasses can be instantiated. (p84)

36. Abstract classes are able to define *abstract members*. Abstract members are like virtual members, except they don’t provide a default implementation. (p85)

37. Subclasses of **abstract** classes can be **abstract** as well. (p85)

38. A subclass can define identical member with the **new** modifier to *hide* the member in base class. <font color="#DA1D1B">Hiding means the member is hidden from the base class, i.e., if the instance of a subclass is upcasted into base class, it cannot access the hidden member.</font> (p85)

39. The **sealed** modifier in a function member seals the overridden implementation of a **virtual**/**abstract** method, so that its subclass cannot override this method. Putting the **sealed** modifier in the class will seal all the overridden methods in that class. (p86)

40. A **sealed** method cannot be overridden in subclass, but can be hidden by subclasses. (p86)

41. The **base** keyword is like the **this** keyword, it serves two essential purposes: to access the overridden method in base class, and to call base class constructor. (pp86)

42. Unlike C++, subclass in C# must declare its own constructors. (p87)

43. If a constructor in subclass omits the **base** keyword to call its base class' constructor, the parameterless constructor in base class is called implicitly. If the base class has no parameterless constructor, an error will occur at compile time. (p87)

44. Order of constructor and field initialization: subclass fields -> base class constructor parameters -> base class field -> base class constructor -> subclass constructor. (p88)

45. The resolution of calling overloaded functions is done at compile time regardless the actual runtime type:

		:::C++
		int Func(Base obj) { /* ... */ }
		int Func(Derived obj) { /* ... */ }

		Derived d = new Derived();
		Base b = d;

		Func(d); // Call Func(Derived)
		Func(b); // Call Func(Base)

	To postpone the resolution to run time, cast the argument to **dynamic**:

		:::C++
		Func((dynamic)b); // Call Func(Derived)
	(p89)

46. **object** is the ultimate base class for all types. **object** itself is a reference type, but value type can also be cast to **object** as well. Casting value type to **object** is called *type unification*. (p90)

47. Casting from value type to **object**, a.k.a *boxing*, is implicit, while *unboxing*, casting **object** back to value type requires explicit casting. (p90)

48. *Boxing* <font color="#DA1D1B">copies</font> the value-type instance into the new object, and *unboxing* <font color="#DA1D1B">copies</font> the contents of the object back into a value-type instance. The operation is "pass by value". (p91)

49. All types in C# are represented at runtime with an instance of **System.Type**. There are two basic ways to get a **System.Type** object:

	- Call **GetType** on the instance. (runtime)
	- Use the **typeof** operator on a type name. (complie time)
	(p91)

50. *Boxing/unboxing* only happens when casting is performed. (p92)

51. **struct** is a **System.ValueType** and does not support inheritance (hence no **virtual** members). (p93)

52. **struct** has an implicit parameterless constructor which will bitwise-zeroing the **struct**, this constructor cannot be replaced. (p94)

53. A constructor of a **struct** must initialize all the fields. (p94)

54. A **struct** has no finalizer. (p93)

55. Default: **public** -> **interface**, **internal** -> non-nested type(**class** without access modifiers), **protected** -> NO DEFAULT, **private** -> member. (p94)

56. **internal** and **protected** is not more accessible than each other. **internal** is visible to containing and friend assemblies, **protected** is visible to containing class and subclass. There's a **protected internal**, which is a union of the **protected** and **internal**. (p95)

57. **internal** members can be exposed to *friend* assemblies by using the **System.Runtime.CompilerServices.InternalsVisibleTo** attribute like:

		[assembly: InternalsVisibleTo ("Friend")]
	(p95)

58. The inner accessibility is controlled by the outter accessibility. (p96)

59. The accessibility of a overridden member function must be consistent/identical between base class and derived class. A subclass can be less accessible than the base, but not more. (p96)

60. If overriding a **protected internal** method <font color="#DA1D1B">in another assembly</font>, the accessibility modifier must be simplified to **protected**. It makes sense as the function is access using **protected** and that function should not be accessed by others in the assembly. (p96)

61. **interface** members(methods, properties, events, indexers) are always implicitly **abstract** and **public** and <font color="#DA1D1B">cannot declare an access modifier</font>. Implementing an interface means providing a public implementation for all its members(must be public as the accessibility must be identical). (p97)

62. For an **internal** implementation of a **public** **interface**, the methods of it can be accessed by upcasting the instance to the **interface**. (p97)

63. **interface** can derive from **interface**. (p97)

64. If two(or more) interfaces have same member signatures, implementing them would result in a collision. *Explicitly implementing* an interface can resolve this.

		:::C++
		interface I1 { void Foo(); }
		interface I2 { int Foo(); } // Note that the signature of a function is its parameters.

		public class Bar: I1, I2 // Order of interface does not matter
		{
			// This will be the default one
			public void Foo() { /* implementation */ }

			// Need to cast Bar to I2 to call this
			public int I2.Foo() { /* implementation */ }
		}

		Bar bar = new Bar();
		bar.Foo(); // Default one, I1.Foo
		((I1)bar).Foo(); // I1.Foo
		((I2)bar).Foo(); // I2.Foo

	It's a good practice that you want user to call the "save" one by default and call other ones explicitly. (p98)

65. An *implicitly implemented* interface member is <font color="#DA1D1B">by default sealed</font>. It <font color="#DA1D1B">must</font> be marked **virtual** or **abstract** in the <font color="#DA1D1B">base class(virtual can only be marked at base class)</font> in order to be overridden. (p98)

66. A *explicitly implemented* interface member can not be marked **virtual** nor be *overridden*. A subclass can *reimplement* a method *explicitly implemented* in base class by using the **new** modifier. Reimplementation is a special kind of *hiding*, but the interface itself is able to access the method. Reimplementation has quite some drawbacks. (p99)

67. **struct** can also implement **interface** as well, cast between **struct** and **interface** requires boxing. (p101)

68. **enum** is essentially a group of integers with names. It can be casted to **int** or casted back to **enum**. It's basically the same as enum in C++.

69. *Flags* enum is a special type of enum. It's still integers but the `[Flags]` attribute forces the enum to be assigned explicitly, typically in power of 2. (p103)

70. By convention, the *Flags* attribute should always be applied to an enum type when its members are combinable. If you declare such an enum without the Flags attribute, you can still combine members, but calling **ToString** on an enum instance will emit a number rather than a series of names(**ToString** on **enum** will return the name of it). (p103)

71. Casting between different **enum** is like assigned the integral value. (p102)

72. If two(or more) **interface** have completely identical method, i.e., same parameter list and return type, it's not necessary to use *explicit implementation* to implement both, implement only one will satisfies those interfaces. However, *implicit implementation* can still be used to "hide" the implementation of a specific interface from other interfaces so that only that interface is able to access it. (out of book)

73. As there's no limitation on casting integer to **enum** or casting between **enums**, it's usually a bad idea to use cast on **enum** casually. (p104)

74. *Nested class* is by default **private**, but can be made more accessible. *Nested class* can access the private members of its enclosing class. To access a *nested class* from outside the enclosing type requires qualification with the enclosing type’s name. (p105)

75. There're *generic types* and *generic method*, in declaration they both have *type parameters*, but usually only *generic types* require *type arguments* as the compiler is able to infer the type of *generic method*. (p109)

76. Properties, indexers, events, fields, constructors, operators, and so on <font color="#DA1D1B">cannot declare type parameters</font>, although they can <font color="#DA1D1B">partake</font> in any type parameters already declared by their enclosing type. Similarly, constructors can partake in existing type parameters, but not introduce them. (p109)

77. Type parameters can be introduced in the declaration of **classes**, **structs**, **interfaces**, **delegates** (covered in Chapter 4), and *methods*. Other constructs, such as *properties*, cannot introduce a type parameter, but can *use* one. (p109)

78. A generic type or method can have multiple type parameters, and as long as the number of type parameters is different, the generic class name can be overloaded. (p110)

79. There's no *open* generic type at runtime, all *open* generic type are closed as part of the compilation. However the **Type** of the *open* type can exist at runtime.

		:::C++
		Type t1 = typeof(A<>);
		Type t2 = typeof(A<,>); // Unbound class
		Type t3 = typeof(T); // This is acutally closed
	(p110)

80. Constrains can be placed on generic type. There're possible constrains:

	where T : *base-class* // T has to be a subclass of or is a *base-class*
	where T : *interface* // T must implements the *interface*
	where T : **class** // T must be a reference type
	where T : **struct** // T must be a value type
	where T : new() // T must has a accessible parameterless constructor
	where T : U // T must be a subclass of or is a U
	(p111)

81. When sub-classing a generic type, the subclass can choose to close none/some/all of the type parameters of the generic type:

		:::C++
		class NoneClose<T, U> : Interface<T, U>
		class SomeClose<U> : Interface<int, U>
		class AllClose : Interface<int, string>

	(p111)

82. A type can name itself as the concrete type when closing a type argument: 

		:::C++
		public class Balloon : IEquatable<Balloon> { ... }
		public class Foo<T> where T : IComparable<T> { ... }
		class Bar<T> where T : Bar<T> { ... } // Decorator
	(p112)

83. **static** data is unique for each closed type. (p113)

84. Given a *base* class and a *derived* class, **X** is *convariant* if **X&lt;derived&gt;** is convertible to **X&lt;base&gt;**. **Y** is *contravariant* if **Y&lt;base&gt;** is convertible to **Y&lt;derived&gt;**. By default those conversion is illegal and will result in compile time error. (p115)

85. Arrays are covariant. (p116)

87. Generic interfaces support *covariance* for type parameters marked with the **out** modifier, and support *contravariance* for type parameters marked with **in** modifier. (p116)
