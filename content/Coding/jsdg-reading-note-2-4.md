Title: JavaScript Definite Guide V6 Reading Note: Chapter 2 - 4
Tag: reading note, javascript
Slug: js-definite-guide-ch-2-4

###Chapter 2

1. JavaScript is **case-sensitive**, while HTML is not(though XHTML is). [2.1.1]2. 
2. Unicode escapes can be used in JavaScript string literals, regular expressions and identifiers. Comments are okay as well, however those escapes will not be interpreted as comments are ignored. [2.1.3]
3. `$`(dollar sign) can be used in identifiers. [2.4]
4. Key words cannot be used as identifiers. [2.4.1]
5. In general, the semicolon(`;`) at the end of a statement can be omitted. However sometime the code may be interpreted in a way that is not the intention of the author. For safety and consistence consideration, it's better to write JavaScript with a semicolon at the end of every statement. [2.5]


###Chapter 3

1. JavaScript types: **primitive**(number, string, boolean, null, undefined) and **object**(anything else). [3]
2. Objects are unordered collections of named values. Two special kinds of object are arrays and functions. [3]
3. JavaScript variables are **untyped**. [3]
4. JavaScript **does not distinct integers and float point numbers**. For integers, JavaScript can represent all integers from -2^53 to 2^53. For float point numbers, JavaScript uses IEEE 754 standard to represent them. However indexing and bitwise operations are performed with 32-bit integers. [3.1]
5. JavaScript officially supports base-10 integers and hexadecimals. [3.1.1]
6. Arithmetic in JavaScript does not raise errors in case of overflow, underflow or division by zero.
7. When overflow occurs, JS set the result to a special value, `Infinity` or `-Infiity`, instead. Arithmetic operation to infinity value are still infinity, though the sign might be reversed; for underflow situation, JS simply returns `0`. And when a number is divided by 0, the result would be `NaN`(Not a Number). [3.1.3]
8. In JavaScript, `Infinity` and `NaN` are predifined global variable. Under different standards, those two variable might be writable, but we should never try to write a new value to them. [3.1.3]
9. `NaN` **does not compare equal to any other value including `NaN` itself**. To test if a value is `NaN`, try x != x (this expression will only evaluate to true when x is `NaN`) or use a predefined function `isNaN`. There's another similar function, `isFinite`, which returns true if its argument is a number other than `Infinity`, `-Infinity` and `NaN`. [3.1.3]
10. Rounding errors are inevitable in JavaScript. (Not every real number is representable even for simple number like 0.1) [3.1.4]
11. Strings in JavaScript are immutable. [3.2]
12. There's no type for representing a single character, just simply use a string of length 1. [3.2]
13. The length of a string in JavaScript is **the number of 16-bit values it contains**. JavaScript uses UTF-16 encoding, a codepoint may not fit into one 16-bit and hence will take up 2. For such codepoint its length will be 2. [3.2]
14. Single quotes and double quotes are the same in JavaScript. [3.2.1]
15. JavaScript strings have lots of method like replace and substring, but string itself is immutable so that those methods return a new string. [3.2.3]
16. From ECMAScript 5, JavaScript strings can be treated as read-only arrays, i.e., string elements can be accessed by index as well as charAt method. [3.2.3]
17. `undifined`, `null`, `0`, `-0`, `NaN` and `""` can be converted to `false`, for anything else is converted to `true`. Therefore `if (sth) ...` is less strict than `if (sth !== null) ...`. [3.3]
18. `null` means no object thought `typeof(null)` returns a string "`object`". `undefined` means the value of a variable is not initialized. `typeof(undefined)` returns `undefined`. Although `null` and `undefined` represent different level of *absence of value*, the equlity operator `==` considers them to be equal. [3.4]
19. If a funcion has no return value, it returns `undefined`. [3.4]
20. `undefined` can be considered as a system-level, unexpected, error-like absence of value, and `null` represents a program-level, expected, normal absence of value. [3.4]
21. In the top-level-non-function code, `this` keyword can be used to refer to the global object, which holds all global properties, global functions and global objects. Program-defined globals are also held here. [3.5]
22. JavaScript object are a collection of porperties. For primative types, `number`, `string` & `boolean`, by design they're converted into corresponding wrapper object so that they have porperties and methods. [3.6]
23. There's no wrapper object for `null` and `undefined`. [3.6]
24. The properties of primitive type variable are only readable. Attempt to set the properties will be silently ignored, as the properties belongs to the underlying temporary warpper object. [3.6]
25. Primitives are immutable and are compared by **value**. Objects are mutable and are compared by references, i.e., two objects are the same iff they're refering to the same object. [3.7]
26. Assigning a object to a variable is assiging the reference of the object to the variable. [3.7]
27. Conversion within operation between `Number` and `String`: if it's a numberic operator, `String` will be converted into `Number`, otherwise `Number` will be converted into `String`. [3.8]
28. For the `==` operator, auto-conversion is performed before the comparison. There're another strict comparison operator `===` which does not perform auto-conversion. [3.8.1]
29. 



