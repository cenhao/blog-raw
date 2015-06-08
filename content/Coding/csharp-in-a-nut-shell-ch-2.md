Title: Reading Notes on C# 5.0 in a Nutshell Ch.2
Tags: Reading-Notes, Code, C#
Slug: csharp-in-a-nutshell-ch-2
Date: 2015-06-05 00:17

###To be honest, I was a little reluctant to use C\# at the begining.

But work is work after all, I cannot escape it. After using it for some time, I found C# to be a very powerful language and decided to learn it thoroughtly. I used to write reading note when I read books. And to make to notes more consistent and avoid changing the pattern every now and then, I'm making the commond rules for my notes:

- <font color="#DA1D1B">Red</font> means this is important.
	- Inside <font color="#DA1D1B">red</font> region, <u>underline</u> could be use to stress even higher importance.
- **Bold** is for the language terms, like keywords
- *Italic* indicates concepts, like cast, multi-threading


OK, Though it is going to be boring, reading notes incoming!

1. Function in C#: *Methods*, *operators*, *constructor*, *properties*, *events*, *indexers*, *finalizers*. (p11)
	
2. By convention, parameters, local variables and <font color="#DA1D1B">private fields</font> should be in camel case, and all other identifiers should be in Pascal case. (p12)

3. `@` prefix can be used to stop an identifier from being interpreted as a *keyword*. (p13)

4. Some *keyword* are contextual, meaning that they're *keyword* only in some specific context. But once again, <font color="#DA1D1B">strive to avoid using any keyword as identifier</font>. (p14)

5. C# uses the same comment style as C++. (p14)

6. Conversion can happen between two <font color="#DA1D1B">compatible</font> types. (p18)

7. *Implicit* conversion happens automatically, if
	- The conversion is guaranteed by the compiler that it will succeed.
	- No information is lot in converion
	
8. *Explicit* conversion may lost information and requires a *cast*. To perform a *cast*, the conversion between the two types must be possible. (p18)
	
9. A always fail conversion will be rejected by the compiler. (p18)

10. C# type catagories: *value*, *reference*, *generic type parameters*, *pointer*. (p19)

11. *Value types* comprise most built-in types and custom **struct** and **enum** type. Its content is somply a value. (p19)

12. Reference type comprise of all **class**, **array**, **delegate** and **interface**. (p19)

13. <font color="#DA1D1B">The assignment of value type instance always *copies* the instance.</font> (p19)

14. *Reference type* has two parts: an *object* and a *reference* to the object. The content of a reference type variable is the reference. Refence type assignment copies the reference only. Multiple instances of a reference type can refer to the same object.(p20)

15. **null** can be assigned to a reference, but not a value type. (p21)

16. *Value type* resembles c++'s **struct** or **class**, which has memory alignment and occupies approximatley the same size of memory of the fields. (p21)

17. *Reference type* requires separate allocations of memory for the reference of the object. It take as many bytes as *value type* to store its field, and has an extra administrative overhead for storing some metadata. Each reference will require extra bytes for pointing the data. (p22)

18. Predefined types alias Framework type in **System** namespace. All predefined types except **Decimal** are *primitive type*. (p23)

19. Of the *integral* types, **int** and **long** are favored by C# and runtime. Other integral types are more for space efficiency purpose. (p23)

20. For numeric literals, *numeric suffix* can be used to explictly define the type. E.g `1.0F`, `1U`, `1L` <font color="#DA1D1B">(upper case is preferred)</font>. (p24)

21. Sometimes *numeric suffix* is mandatory for initializing or assigning value to certain numeric types. For example:

		:::C#
		float f = 1.0;

	will fail, according to C#'s implicit type conversion rule. The correct way to do it:

		:::C#
		float f = 1.0F;

	(p25)

22. 8-bit and 16-bit numeric type do not support arithmetic operations (+, -, *, /, %). (p26)

23. Arithmetic operation on integral type can overflow sliently. **checked** can be used to enforce a checking on overflow. But operations at compile time are always checked. (p27)

24. C#'s **char** represents a Unicode character and occupies 2 bytes. **char** literals live inside single quote. (p32)

25. **char** can be converted to *numeric types* that can accommodate an unsigh **short**. (p33)

26. `@` can be used to turn off escape. Remember it can also stop *identifiers* being interpreted as *keyword*. (p33)

27. `@` can also be used for defining multiline string literals. Like \`\`\` in python. (p34)

28. `+` can be used to concatenation **string**s, however as **string**s are immutable, this is not efficent. A better solution would be to use **StringBuilder**. (p34)

29. Unlike C++, **string**s in C# can't be compared by using &gt; or &lt;. The **CompareTo** function is needed. (p34)

30. Creating an *array* always preinitializes the elements with default values. The default value for a type is the result of a bitwise zeroing of memory. (p35)

31. For *reference type* array, all the elements are initialized as **null**. (p35)

32. To define *retangular array*s, put commas to separate each dimension. Like:

		:::C#
		int matrix[,] = new int[2,3];
		char cube[,,] = new char[4,3,5];

	To get the length of each dimension, use **GetLength** with the corresponding index:

		:::C#
		char cube[,,] = new char[5,3,4];
		int len0 = cube.GetLength(0); // 5
		int len1 = cube.GetLength(1); // 3
		int len2 = cube.GetLength(2); // 4

	(p36)

33. *Jagged array*s are declared using successive square brackets to represent each dimension.

		:::C#
		int matrix[][] = new int[3][];

	Each inner array is implicitly initialized to **null** rather than an empty array. Each inner array must be created manually:

		:::C#
		for (int i = 0; i < matrix.Length; i++)
		{
			matrix[i] = new int[3];
			for (int j = 0; j < matrix[i].Length; j++)
			    matrix[i][j] = i * 3 + j;
		}
	
	(p37)

34. The *stack* is a block of memory for storing local variables and parameters. The stack logically grows and shrinks as a function is entered and exited. (p38)

35. The *heap* is a block of memory in which objects (i.e., reference-type instances) reside. Whenever a new object is created, it is allocated on the heap, and a *reference* to that object is returned. <font color="#DA1D1B">Note that the *reference* itself can live in *stack*.</font> (p39)

36. *Value type* instances(and object references) live wherever the variable was declared. (p39)

37. The *heap* also stores static fields and constants. (p39)

38. The **default** keyword can be used to obtain the default value for any types. (p41)

39. By default, the function arguments in C# is *passed by value*. To pass arguments *by reference*, put **ref** before the parameter declaration and the argument. <font color="#DA1D1B">**ref** must be place in both declaration and invocation</font>. (p42)

40. **out** resembles **ref**, but the argument need not to be assigned before going into the function and must be assigned before coming out the function. (p43)

41. The **params** parameter modifier may be specified on the <font color="#DA1D1B">last</font> parameter of a method so that the method accepts any number of parameters of a particular type. <font color="#DA1D1B">The parameter type must be declared as an array</font>. (p44)

42. Besides any number of parameters, an ordinary array can also be supplied to **params** arguments. (p45)

43. *Methods*, *constructors* and *indexers* can declare *optional parameters*. To declare a *optional parameter*, specify a *default value* to its declaration. (p45)

44. The default value of an optional parameter must be specified by a constant expression, or <font color="#DA1D1B"> a <u>parameterless</u> constructor of a *value type*(not *reference type*). </font> Optional parameter can not be marked with **ref** or **out**. (p45)

45. For parameters, *madatory* parameters come first, *optional* parameters come second, and **params** parameter come last. (p45)

46. By default arguments are identified by position. However *named argument* can be use to change the way arguements are identified, put the name of the parameter followed by a semicolon before the argument like this:

		:::C#
		void func(int x, int y);
		func(y:1, x:2);

	*named argument* is very usefully when there're multiple optional parameters but only some of them need to be assigned. (p46)

47. If the compiler is able to infer the type from the initialization expression, you can use the keyword **var** in place of the type declaration. (p46)

48. A variable's scope extends in both direction in both direction, meaning the code like this is not legal:

		:::C#
		{
			{
				int x;
			}
			int x;
		}
	(p52)

49. Besides **break**, **goto case x** and **goto defalut** can also be used in **switch** statement. (p55)

50. The C# jump statements are **break**, **continue**, **goto**, **return** and **throw**, all of them obey the rule of **try** statement:

	- A jump out of the **try** block always excutes the **finally** block before reach the destination.
	- Except **throw**, no jump can be made from inside to outside of the **finally** block.
	(p57)

51. The goto statement transfers execution to another label <font color="#DA1D1B">within a statement block</font>. (p58)

52. Types are not defined in any *namespace* reside in *global namespace*. (p60)

53. Names declared in outer namespaces can be used <font color="#DA1D1B">unqualified</font> within inner namespaces. If you want to refer to a type in a different branch of your namespace hierarchy, you can use a partially qualified name(<font color="#DA1D1B">starting after the common parent</font>). (p60)

54. If the same type name appears in both an inner and an outer namespace, the inner name wins. To refer to the type in the outer namespace, you must (partially) qualify its name. (p61)

55. Namespace declaration can repeated or even in different assemblies, as long as the type names within the namespaces don’t conflict. (p61)

56. Instead of importing a namespace, it's possble to give an *alias* to a namespace or the type within it to get rid of name conflict problem.

		:::C#
		using NS = NS1.NS2.NS3;
		using SomeType = NS1.NS2.SomeType;
	(p62)

57. When there're more than one assembly share the same full qualify name, the **extern** can be used together with compile option to give the assemblies different aliases. (p63)

58. To refer to the *global* namespace, use the **global** keyword. However the `::` token is needed to qualify:

		:::C#
		var a = new global::NS.SomeType();
