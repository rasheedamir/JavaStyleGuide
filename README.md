# Java Style Guide

Opinionated Java Style Guide based on [Google](https://google.github.io/styleguide/javaguide.html) and [Spring Framework](https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-Code-Style)

And its a _work in progress_.

## Introduction
---

This document defines the coding standards for Java source files.

Like other programming style guides, the issues covered span not only aesthetic issues of formatting, but other types of conventions or coding standards as well. However, this document focuses primarily on the hard-and-fast rules that we follow universally, and avoids giving advice that isn't clearly enforceable (whether by human or tool).

## Source File Basics
---

### File encoding: UTF-8

Source files must be encoded using `UTF-8`.

### Indentation

* Indentation uses _tabs_ (not spaces)
* Unix (LF), not DOS (CRLF) line endings
* Eliminate all trailing whitespace
  * On Linux, Mac, etc.: `find . -type f -name "*.java" -exec perl -p -i -e "s/[ \t]$//g" {} \;`

## Source file structure
---

A source file consists of the following, in this exact order:

* License
* Package statement
* Import statements
* Exactly one top-level class

Exactly one blank line separates each of the above sections.

### License

Each source file must specify the following license at the very top of the file:

	/*
	 * Copyright 2002-2016 the original author or authors.
	 *
	 * Licensed under the Apache License, Version 2.0 (the "License");
	 * you may not use this file except in compliance with the License.
	 * You may obtain a copy of the License at
	 *
	 *      http://www.apache.org/licenses/LICENSE-2.0
	 *
	 * Unless required by applicable law or agreed to in writing, software
	 * distributed under the License is distributed on an "AS IS" BASIS,
	 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	 * See the License for the specific language governing permissions and
	 * limitations under the License.
	 */

Always check the date range in the license header. For example, if you've modified a file in 2016 whose header still reads:
```   
* Copyright 2002-2013 the original author or authors.
```
Then be sure to update it to 2016 accordingly:
```
* Copyright 2002-2016 the original author or authors.
```

### Import statements

#### No wildcard imports

Wildcard imports, static or otherwise, are not used.

#### No line-wrapping

Import statements are not line-wrapped. The column limit (Section 4.4, Column limit: 100) does not apply to import statements.

#### Ordering and spacing

Imports are ordered as follows:

* All static imports in a single block.
* All non-static imports in a single block.

If there are both static and non-static imports, a single blank line separates the two blocks. There are no other blank lines between import statements.

Within each block the imported names appear in ASCII sort order. (Note: this is not the same as the import statements being in ASCII sort order, since '.' sorts before ';'.)

No static import for classes

Static import is not used for static nested classes. They are imported with normal imports.

### Java source file organization

The following governs how the elements of a source file are organized:

1. static fields
1. normal fields
1. constructors
1. (private) methods called from constructors
1. static factory methods
1. JavaBean properties (i.e., getters and setters)
1. method implementations coming from interfaces
1. private or protected templates that get called from method implementations coming from interfaces
1. other methods
1. `equals`, `hashCode`, and `toString`

Note that private or protected methods called from method implementations should be placed immediately below the methods where they're used. In other words if there 3 interface method implementations with 3 private methods (one used from each), then the order of methods should include 1 interface and 1 private method in sequence, not 3 interface and then 3 private methods at the bottom.

Above all, the organization of the code should feel _natural_.

## Formatting
---

### Braces

#### Block-like constructs: K&R style

Braces mostly follow the _Kernighan and Ritchie style_ (a.k.a., "Egyptian brackets") for nonempty blocks and block-like constructs:

* Line break before the opening brace
* Line break after the opening brace
* Line break before the closing brace
* Line break after the closing brace if that brace terminates a statement or the body of a method, constructor, or named class
* Line break before else, catch and finally statements

Example:

```
return new MyClass() 
{
	@Override 
	public void method() 
	{
		if (condition()) 
		{
			something();
		}
		else 
		{
			try 
			{
				alternative();
			} 
			catch (ProblemException ex) 
			{
				recover();
			}
		}
	}
};
```

### Line wrapping

90 characters is the *preferred* line length we aim for. In some cases the preferred length can be achieved by refactoring code slightly. In other cases it's  just not possible.

90 is not a hard limit. Lines between 90-105 are perfectly acceptable in many cases where it aids readability and where wrapping has the opposite effect of reducing readability. This is a judgement call and it's also important to seek consistency. Many times you can learn by looking how specific situations are handled in other parts of the code.

Lines between 105-120 are allowed but discouraged and should be few.

No lines should exceed 120 characters.

The one big exception to the above line wrapping rules is Javadoc where we aim to wrap around 80 characters for maximum readability in all kinds of contexts, e.g. reading on Github, on your phone, etc.

When wrapping a lengthy expression, 90 characters is the length at which we aim to wrap. Put the separator symbols at the end of the line rather on the next line (comma separated arguments, etc). For instance:

```
if (thisLengthyMethodCall(param1, param2) && anotherCheck() &&
        yetAnotherCheck()) {

    // ....
}
```

### Blank Lines

Add two blank lines before the following elements:

* `static {}` block
* Fields
* Constructors
* Inner classes

Add one blank line after a method signature that is multiline, i.e.

```
@Override
protected Object invoke(FooBarOperationContext context, 
        AnotherSuperLongName name) 
{

    // code here
}
```

## Class declaration

Try as much as possible to put the `implements`, `extends` section of a class declaration on the same line as the class itself. 

Order the classes so that the most important comes first.

## Naming

### Constant names

Constant names use `CONSTANT_CASE`: all uppercase letters, with words separated by underscores. 

Every constant is a `static final` field, but not all `static final` fields are constants. Constant case should therefore be chosen only if the field **is really** a constant.

Example:

```
// Constants
private static final Object NULL_HOLDER = new NullHolder();
public static final int DEFAULT_PORT = -1;

// Not constants
private static final ThreadLocal<Executor> executorHolder = new ThreadLocal<Executor>();
private static final Set<String> internalAnnotationAttributes = new HashSet<String>();
```

### variable names

Avoid using single characters as variable names. For instance prefer `Method method` to `Method m`. 

## Programming Practices

### File history

* A file should look like it was crafted by a single author, not like a history of changes
* Don't artificially spread things out that belong together

### Organization of setter methods

Choose wisely where to add a new setter method; it should not be simply added at the end of the list. Perhaps the setter is related to another setter or relates to a group. In that case it should be placed near related methods.

* Setter order should reflect order of importance, not historical order
* Ordering of _fields_ and _setters_ should be **consistent**

### Ternary Operator

Wrap the ternary operator within parentheses, i.e. `return (foo != null ? foo : "default");`

Also make sure that the _not null_ condition comes first.

### Null Checks

Use the `org.springframework.util.Assert.notNull` static method to check that a method argument is not `null`. Format the exception message so that the name of the parameter comes first with its first character capitalized, followed by "_must not be null_". For instance

```
public void handle(Event event) {
    Assert.notNull(event, "Event must not be null");
    //...
}
```

### Use of @Override

Always add `@Override` on methods overriding or implementing a method declared in a super type.

### Use of @since

* `@since` should be added to every new class with the version of the framework in which it was introduced
* `@since` should be added to any *new* **public** and **protected** methods of an existing class

### Utility classes

A class that is only a collection of static utility methods must be named with a `Utils` suffix, must have a private default constructor, and must be abstract. Making the class abstract and providing a `private` _default_ constructor prevent anyone from instantiating it. For example:

```
public abstract MyUtils 
{

    private MyUtils() 
    {
        /* prevent instantiation */
    }

    // static utility methods
}
```

### Field and method references

A field of a class should *always* be referenced using `this`. A method of class, however, should never be referenced using `this`.

## Javadoc

### Javadoc formatting

The following template summarizes a typical use for the Javadoc of a method.

```
/**
 * Parse the specified {@link Element} and register the resulting
 * {@link BeanDefinition BeanDefinition(s)}.
 * <p>Implementations must return the primary {@link BeanDefinition} that results
 * from the parsing if they will ever be used in a nested fashion (for example as
 * an inner tag in a {@code <property/>} tag). Implementations may return
 * {@code null} if they will <strong>not</strong> be used in a nested fashion.
 * @param element the element that is to be parsed into one or more {@link BeanDefinition BeanDefinitions}
 * @param parserContext the object encapsulating the current state of the parsing process;
 * provides access to a {@link org.springframework.beans.factory.support.BeanDefinitionRegistry}
 * @return the primary {@link BeanDefinition}
 */
BeanDefinition parse(Element element, ParserContext parserContext);
```

In particular, please note:

* Use an imperative style (i.e. _Return_ and not _Returns_) for the first sentence.
* No blank lines between the description and the parameter descriptions.
* If the description is defined with multiple paragraphs, start each of them with `<p>`.
* If a parameter description needs to be wrapped, do not indent subsequent lines (see `parserContext`).

The Javadoc of a class has some extra rules that are illustrated by the sample below:

```
/*
 * Interface used by the {@link DefaultBeanDefinitionDocumentReader} to handle custom,
 * top-level (directly under {@code <beans/>}) tags.
 *
 * <p>Implementations are free to turn the metadata in the custom tag into as many
 * {@link BeanDefinition BeanDefinitions} as required.
 *
 * <p>The parser locates a {@link BeanDefinitionParser} from the associated
 * {@link NamespaceHandler} for the namespace in which the custom tag resides.
 *
 * @author Rob Harrop
 * @since 2.0
 * @see NamespaceHandler
 * @see AbstractBeanDefinitionParser
 */
```

* Each class must have a `@since` tag with the version in which the class was introduced.
* The order of tags for class-level Javadoc is `@author`, `@since` and `@see`.
* In contrast to method-level Javadoc, the paragraphs of a class description *are* separated by blank lines.

The following are additional general rules to apply when writing Javadoc:

* Use `{@code}` to wrap code statements or values such as `null`.
* If a type is only referenced by a `{@link}` element, use the fully qualified name in order to avoid an unnecessary `import` declaration.

## Tests

### Naming

Each test class must end with a `Tests` suffix.

### Mocking

Use the BDD Mockito support.