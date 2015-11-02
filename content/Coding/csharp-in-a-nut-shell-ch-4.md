Title: Reading Notes on C# 5.0 in a Nutshell Ch.4
Tags: Reading-Notes, Code, C#
Slug: csharp-in-a-nutshell-ch-4
Date: 2015-11-01 10:59

1. A **delegate** is an object that knows how to call a method. (p119)

2. A **delegate** instance literally acts as a *delegate* for the caller: the caller invokes the delegate, and then the delegate calls the target method. This indirection decouples the caller from the target method. (p120)

3. All delegate instances have *multicast* capability. This means that a delegate instance can reference <font color="#DA1D1B">not just a single target method, but also a list of target methods</font>. (p121)

4. `+` and `+=` can be used to combine delegate instances, while `-` and `-=` are used to remove delegate instances. Delegates are invoked in the order they are added. (p121)

5. All the methods in a multicast delegate are invoked, however only the return value of the last delegate is returned, the others are discarded. A good practice is only assign methods which returns **void** to a multicast delegate. (p121)

6. Both *instance* method(non-static member function) and **static** method can be assigned to a delegate. The delegate's **Target** property will keep a reference to that instance. In the case of **static** method is assigned, the **Target** is **null**. (p122)

7. A **delegate** type may contain generic type parameters. Like:

		:::C#
		public static int Inc(int a)
		{
			return ++a;
		}

		public delegate T DoSomething&lt;T&gt; (T arg);
		DoSomething d = Inc;

	(p123)

8. **Func** and **Action** are predefined generic delegates.Both **Func** and **Action** support parameter number from 0 to 16.

		:::C#
		public delegate TResult Func<[in T1, [in T2, [..., [in T16]]]], out TResult>
			([T1 arg1, [T2 arg2, [..., [T16 arg16]]]]);

		public delegate void Action<[in T1, [in T2, [..., [in T16]]]]>
			([T1 arg1, [T2 arg2, [..., [T16 arg16]]]]);

	**ref**/**out** and *pointer* parameters are not supported for **Func** and **Action**. (p124)

9. Delegate types are all <font color="#DA1D1B">incompatible</font> with each other, even if their signatures are the same. (p126)

10. Delegate *instances* are considered equal if they have the same method targets. Multicast delegates are considered equal if they reference the same methods in the same order. (p126)

11. A delegate can have more specific parameter types than its method target, and more general return type than its method target. (p127)

12. When defining generic delegates, a good practice is to:
	- mark a type parameter used only on the return value as covariant (**out**).
	- Mark any type parameters used only on parameters as contravariant (**in**).
(p127)

13. **event** is a language feature designed specifically for the *broadcaster/subscriber* model, it's used as a modifier for the delegate. With **event**, when accessing outside the class, subscriber can only use `+=` or `-=` to modify the delegate(while `=` or invoking the delegate is not allowed), and the delegate can be invoke inside the class only. (p128)

14. There's a standard way of defining a event delegate:
	- It must have a void return type.
	- It must accept two arguments: the first of type **object**, and the second a <font color="#DA1D1B">subclass</font> of **EventArgs**. The first argument indicates the event broadcaster, and the second argument contains the extra information to convey.
	- Its name must end with `EventHandler`.
(p131)

15. The `System.EventHandler<>` and `System.EvenHandler` are predefined delegate that satisfy the rules mentioned in #14.

		:::C#
		public delegate void EventHandler<TEventArgs>
			(object source, TEventArgs e) where TEventArgs : EventArgs;
		public delegate void EventHandler(object source, EventArgs e);
(p131)

16. The standard to fire an event is to wrap the invocation of the event delegate with a **protected virtual** method, whose name starts with `On`. And in multithreaded scenarios, you need to assign the delegate to a temporary variable before testing and invoking it, to avoid an obvious thread-safety error:

		:::C#
		protected virtual void OnSomethingChanged (SomeEventArgs e)
		{
			var temp = EventDelegate;
			if (temp != null) temp(this, e);
		}
(p132)

17. The **EventArgs.Empty** property can be used to avoid unnecessarily instantiating an instance of **EventArgs** when passing **EvenArgs** is not necessary. (p133)

18. Besides auto-generating event *accessors*, C# also supports manually defining *explic* accessor by using **add** and **remove**. This is useful for:

	- Delegating the event handler to another class
		
			:::C#
			public event EventHandler Eh
			{
				add { someOtherClass.Eh += value; }
				remove { someOtherClass.Eh -= value; }
			}

	- Hiding the actual event handler storage, if there're way more event handlers than event listener

			:::C#
			private Dictionary<int, EventHandler> _ehDict;
			public event EventHandler Eh1
			{
				add { _ehDict[1] += value; } // Ignore null cases
			}
			
	- Explicitly implementint an interface that declares an event. 
(p134)

19. Like methods, events can be **virtual**, **overridden**, **abstract**, **seal** and **static**. (p135)

20. A *lambda expression* is an unnamed method writen in place of a delegate instance. A lambda expression is converted to either of these two forms: a delegate instance or an *expression tree*, of type **Expression<TDelegate>** (not discussed at this point). (p135)

21. A lambda expression has this form: `(paramters) => expression-or-statement-block`. (Note that lambda exprssion supports statement block). For convenience, <font color="#DA1D1B">iff</font> there's only one parameter, the parentheses can be omitted. (p135)

22. Usually the type of a lambda paramter is inferred by the compiler according to the context. But the type can also be explictly specified.

		:::C#
		Func<int, int> pow2 = (int x) => x * x;
(p136)

23. A lambda expression can reference the local variables and parameters of the method in which it’s defined (*outer variables*). Outer variables referenced by a lambda expression are called *captured variables*. A lambda expression that captures variables is called a <font color="#DA1D1B">*closure*</font>. (p136)

24. <font color="#DA1D1B">Captured variables are evaluated when the delegate is actually invoked, not when the variables were captured</font>. So this postponed evaluation requires captured variables to have their lifetimes extended to that of the delegate. (p137)

25. A local variable instantiated within a lambda expression is just like a local variable in a method, it's unique per invocation of the delegate instance. (p137)

26. Be careful when capturing iteration variable, because of postponed evaluation, when the delegate is invoke, the captured variable might have been modified. The iteration variable in **foreach** is immutable, so it doesn't suffer this problem as the other iteration variables(After C# 5.0). (p139)

27. Regardless if the exception is caught/handled/rethrown or not, the **finally** block is always executed. (p141)

28. The type of an exception must be either **System.Exception** or a subclass of this. (p142)

29. If there're multiple possible exceptions will be caught, put the specific ones first. (p142)

30. An exception can be caught without specifying a variable, if you don’t need to access its properties. If the type of the exception does not matter, the type can be omitted as well. (p143)

31. A **finally** block excutes either:

	- After a **catch** block finishes
	- After the control leaves the **try** block because of a **jump** statement (e.g., **return** or **goto**)
	- After the **try** block ends

	The only things that can defeat a finally block are an infinite loop, or the process ending abruptly. (p143)

32. The **using** statement provides a elegant wait of calling **Dispose** on an **IDisposable** by using **finally** block. (p144)

33. When in a catch block, the exception can be rethrown without changes by simply calling `throw;`. Alternatively, a new exception can be thrown as well. (p145)

34. The **StackTrace**(string), **Message**(string) and **InnerException** are the most important properties of **System.Exception**. (p146)

35. It's a common practice to implement a nothrow function with name TryXXX where XXX is the one that will throw. (p147)

36. An *enumerator* is a read-only, forward-only cursor over a *sequence of values*. (p148)

37. A enumerable object is not a cursor itself, but can produce cursors over itself. It either implements **IEnumerable** / **IEnumerable&lt;T&gt;**, or has a method named **GetEnumerator** that returns an *enumerator*. The **foreach** statement can iterate over a enumerable object. (p148)

38. The *collection initializor* is implemented using the **Add** method in from the **IEnumerable** interface, hence to use collection initializor the class must implement the **IEnumerable** interface. (p149)

39. An *iterator* is a method, property, or indexer that contains one or more **yield** statements. An **iterator** must return a **IEnumerable**/**IEnumerable&lt;T&gt;**/**IEnumerator**/**IEnumerator&lt;T&gt;**, and depending on the type of the interface returned, the *itertor* has different sematics. (p150)

40. To exit early, the *iterator* can only use **yield break**, **return** is not allowed in an *iterator*. (p151)

41. **yield** can't be used in a **catch** block or a **finally** block, and it can be used in the **try** block only when the **try** block has only a **finally** block. (p151)

42. When using enumerator explicitly, remember to dispose them. A good practice would be protect the enumerator with a **using** clause. **foreach** statement dispose the enumerator implicitly. (p152)

43. **T?** is translated into **Nullable&lt;T&gt;**, which is a <font color="DA1D1B">immutable sturcture</font>. However **T?** can be used together with **null** as the compiler will automatically convert it to the *Nullable&lt;T&gt;* sematic. (p154)

44. When **T?** is boxed, the boxed value on the heap contains **T**, not **T?**, since reference type can already express **null**. And when unboxed back to **Nullable&lt;T&gt;**, the value will be **null** (**HasValue** is false) if cast fails. (p154)

45. Calling the **Value** of a nullable type will throw an **InvalidOperationException** if **HasValue** is false. (p154)

46. Though nullable type does not define any operator, the compiler will *lift* the operator from the underlying type. Mixing nullable and non-nullable operator will convert the non-nullable to a temperary nullable to perform the operation. When two nullables are operating together, if none of them is null, then they work as the non-nullable type. (p156)

47. The **??** is the null coalescing operator, it can be used with both *reference type* and *nullable type*. It returns the left operand if it's no null, otherwise the right operand. (p156)

48. The null coalescing operator is equivalent to calling **GetValueOrDefault**, except the expressiong for default value is not evaluated if the left operand is not null. (p157)

49. Lots of operators can be overloaded. In addition to the normal operator overloading, C# has **implicit** and **explicit** conversions, and **true** and **fallse** can be overloaded as well. (p158)

50. The *compound assignment operators* (e.g., **+=**, **/=**) are implicitly overridden by overriding the noncompound operators (e.g., **+**, **/**). The conditional operators **&amp;&amp;** and **||** are implicitly overridden by overriding the bitwise operators **&amp;** and **|**. (p159)

51. An operator is overloaded by declaring an operator function. (p159)

52. For C#, it's more natural to implement the **IComparable** or **IComparable&lt;T&gt;** instead of overloading the *equality* and *comparision* operators(more common for **struct**). (p160)

53. To convert between weakly related types, the following strategies are more suitable:

	- Write a constructor that has a parameter of the type to convert from.
	- Write **ToXXX** and (static) **FromXXX** methods to convert between types.

	(p160)

54. *Extension methods* allow an existing type to be extended with new methods without altering the definition of the original type. An extension method is a **static** method of a **static** class, where the this modifier is applied to the first parameter. (p162)

55. Any compatible instance method will always take precedence over an extension method. If two extension methods have the same signature, the extension method must be called as an ordinary static method to disambiguate the method to call. If one extension method has more <font color="#DA1D1B">specific</font> arguments, however, the more specific method takes precedence. (p163)

56. An *anonymous type* is a simple class created by the compiler on the fly to store a set of values. To create an anonymous type, use the new keyword followed by an object initializer, specifying the properties and values the type will contain. For example:

		:::C#
		var dude = new { Name = "Bob", Age = 23 };

	(p164)

57. You must use the var keyword to reference an *anonymous type*, because it doesn’t have a name. The property name of an anonymous type can be inferred from an expression that is itself an *identifier* (or ends with one). the **Equals** method of a *anonymous type* is overridden to perform equality comparisons. (p165)

58. In short, **dynamic** type works as other weak type language's variable, that is, it can be assign any type of value, the *bingding* will be deferred to runtime. When something went wrong, a **RuntimeBinderException** will be thrown most of time. (p165)

59. A class implement **IDynamicMetaObjectProvider** to achieve *custom binding*. If not, the default *language binding* will be performed. (p167)

60. An attribute is defined by a class that inherits (directly or indirectly) from the abstract class **System.Attribute**. To attach an attribute to a code element(assemblies, types, members, return values, parameters, and generic type parameters), specify the attribute’s type name in square brackets, before the code element. (p174)

61. Attributes may have *named* or *positional* parameters. When specifying an attribute, you must include *positional parameters* that correspond to one of the attribute’s constructors. *Named parameters* are optional. (p174)

62. An attribute can also be attached an assembly, while usually it's targeting the code element it immediately precedes. To attach an attribute to an assembly requires explicitly specify the attribute’s target.

		:::C#
		[assembly:AttributeName]
	(p175)

63. Multiple attributes can be specified for a single code element. Each attribute can be listed either within the same pair of square brackets (separated by a comma) or in separate pairs of square brackets (or a combination of the two). (p175)

64. I'm skipping the unsafe pointer, preprocess directives and the xml documentation in this reading note, as I feel they're either less used or very easy to use in visual studio.
