Title: Reading Notes on C# 5.0 in a Nutshell Ch.3
Tags: Reading-Notes, Code, C#
Slug: csharp-in-a-nutshell-ch-3
Date: 2015-06-06 17:22

1. **readonly** modifier prevents a *field* from being assigned after construction. A **readonly** field can only be assigned in its declaration or in the enclosing constructor. (p68)

2. If *fields* are not initialized, the *fields* are initialized with default value. (p68)

3. **ref** and **out** modifier used in *functions* can also be used in *method*. (p69)

4. A **class** can have multiple *constructors*, and one constructor can call another by by putting `this(/* arguments for the corresponding constructor */)` after a `:`. (p70)

5. Like C++, C# generates a default public parameterless constructor for a class <font color="#DA1D1B">iff</font> no constructor is explictly defined. (p70)

6. When a class is instantiated, the *fields* are initialized in the order the *fields* are declared and then the constructor is called. (p70)

7. Any accessible fileds or properties can be initialized after construction via a *object initializer*. The initializer is defined within braces. (p71)

8. The way initializer works is that it first create a temparory object to initialize, then assign the object back to the original one. The reason for doing this is that if the initializer failed, the origin will not end up in half initialized state. (p71)
