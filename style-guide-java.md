
# Table of Contents  
  

 - [Introduction](#introduction)
	 - [Terminology notes](#terminology-notes)
	 - [Guide notes](#guide-notes)  
 - [Source file basics](#source-file-basics)
	 - [File name](#file-name)
	 - [File encoding: UTF-8](#file-encoding-utf-8)
	 - [Special characters](#special-characters)  
 - [Source file structure](#source-file-structure)
	 - [License or copyright information, if present](#license-or-copyright-information-if-present)
	 - [Package statement](#package-statement)
	 - [Import statements](#import-statements)
	 - [Class declaration](#class-declaration) 
 - [Formatting](#formatting)
	 - [Braces](#braces)
	 - [Block indentation: +2 spaces](#block-indentation-2-spaces)
	 - [One statement per line](#one-statement-per-line)
	 - [Column limit: 100](#column-limit-100)
	 - [Line-wrapping](#line-wrapping)
	 - [Whitespace](#whitespace)
	 - [Grouping parentheses: recommended](#grouping-parentheses-recommended)
	 - [Specific constructs](#specific-constructs) 
 - [Naming](#naming)
	 - [Rules common to all identifiers](#rules-common-to-all-identifiers)
	 - [Rules by identifier type](#rules-by-identifier-type)
	 - [Camel case: defined](#camel-case-defined) 
 - [Programming Practices](#programming-practices)
	 - [@Override: always used](#override-always-used)
	 - [Caught exceptions: not ignored](#caught-exceptions-not-ignored)
	 - [Static members: qualified using class](#static-members-qualified-using-class)
	 - [Finalizers: not used](#finalizers-not-used)  
 - [Javadoc](#javadoc)
	 - [Formatting](#formatting)
	 - [The summary fragment](#the-summary-fragment)
	 - [Where Javadoc is used](#where-javadoc-is-used)
	 
## Introduction

This document serves as the  **complete**  definition of Google's coding standards for source code in the Java™ Programming Language. A Java source file is described as being  _in Google Style_  if and only if it adheres to the rules herein.

Like other programming style guides, the issues covered span not only aesthetic issues of formatting, but other types of conventions or coding standards as well. However, this document focuses primarily on the  **hard-and-fast rules**  that we follow universally, and avoids giving  _advice_  that isn't clearly enforceable (whether by human or tool).

### Terminology notes

In this document, unless otherwise clarified:

1.  The term  _class_  is used inclusively to mean an "ordinary" class, enum class, interface or annotation type (`@interface`).
2.  The term  _member_  (of a class) is used inclusively to mean a nested class, field, method,  _or constructor_; that is, all top-level contents of a class except initializers and comments.
3.  The term  _comment_  always refers to  _implementation_  comments. We do not use the phrase "documentation comments", instead using the common term "Javadoc."

Other "terminology notes" will appear occasionally throughout the document.
### Guide notes

Example code in this document is  **non-normative**. That is, while the examples are in Google Style, they may not illustrate the  _only_  stylish way to represent the code. Optional formatting choices made in examples should not be enforced as rules.

## Source file basics

### File name
The source file name consists of the case-sensitive name of the top-level class it contains (of which there is  [exactly one](https://google.github.io/styleguide/javaguide.html#s3.4.1-one-top-level-class)), plus the  `.java`  extension.

### File encoding: UTF-8

Source files are encoded in  **UTF-8**.

### Special characters

#### Whitespace characters
Aside from the line terminator sequence, the  **ASCII horizontal space character**  (**0x20**) is the only whitespace character that appears anywhere in a source file. This implies that:

1.  All other whitespace characters in string and character literals are escaped.
2.  Tab characters are  **not**  used for indentation.

#### Special escape sequences

For any character that has a  [special escape sequence](http://docs.oracle.com/javase/tutorial/java/data/characters.html)  (`\b`,  `\t`,  `\n`,  `\f`,  `\r`,  `\"`,  `\'`  and  `\\`), that sequence is used rather than the corresponding octal (e.g. `\012`) or Unicode (e.g. `\u000a`) escape.

#### Non-ASCII characters

For the remaining non-ASCII characters, either the actual Unicode character (e.g. `∞`) or the equivalent Unicode escape (e.g. `\u221e`) is used. The choice depends only on which makes the code  **easier to read and understand**, although Unicode escapes outside string literals and comments are strongly discouraged.

**Tip:**  In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful.

Examples:
| Example | Discussion |
|--|--|
|`String unitAbbrev =  "μs";`  |Best: perfectly clear even without a comment.  |
|`String unitAbbrev =  "\u03bcs";  // "μs"`  |Allowed, but there's no reason to do this.|
|`String unitAbbrev =  "\u03bcs";  // Greek letter mu, "s"`  |Allowed, but awkward and prone to mistakes. |
|`String unitAbbrev = "\u03bcs";`  |Poor: the reader has no idea what this is. |
|`return  '\ufeff'  + content;  // byte order mark`  |Good: use escapes for non-printable characters, and comment if necessary. |

**Tip:**  Never make your code less readable simply out of fear that some programs might not handle non-ASCII characters properly. If that should happen, those programs are  **broken**  and they must be  **fixed**.

## Source file structure

A source file consists of,  **in order**:

1.  License or copyright information, if present
2.  Package statement
3.  Import statements
4.  Exactly one top-level class

**Exactly one blank line**  separates each section that is present.

### License or copyright information, if present

If license or copyright information belongs in a file, it belongs here.

### Package statement

The package statement is  **not line-wrapped**. The column limit ([Column limit: 100](#column-limit-100)) does not apply to package statements.

### Import statements

#### No wildcard imports

**Wildcard imports**, static or otherwise,  **are not used**.

#### No line-wrapping
Import statements are  **not line-wrapped**. The column limit ([Column limit: 100](#column-limit-100)) does not apply to import statements.

#### Ordering and spacing

Imports are ordered as follows:

1.  All static imports in a single block.
2.  All non-static imports in a single block.

If there are both static and non-static imports, a single blank line separates the two blocks. There are no other blank lines between import statements.

Within each block the imported names appear in ASCII sort order. (**Note:**  this is not the same as the import  _statements_  being in ASCII sort order, since '.' sorts before ';'.)

#### No static import for classes

Static import is not used for static nested classes. They are imported with normal imports.

### Class declaration

#### Exactly one top-level class declaration

Each top-level class resides in a source file of its own.

#### Ordering of class contents

The order you choose for the members and initializers of your class can have a great effect on learnability. However, there's no single correct recipe for how to do it; different classes may order their contents in different ways.

What is important is that each class uses  **_some_  logical order**, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the class, as that would yield "chronological by date added" ordering, which is not a logical ordering.

##### Overloads: never split

When a class has multiple constructors, or multiple methods with the same name, these appear sequentially, with no other code in between (not even private members).

## Formatting

**Terminology Note:**  _block-like construct_  refers to the body of a class, method or constructor. Note that, by Section on  [array initializers](#array-initializers), any array initializer  _may_  optionally be treated as if it were a block-like construct.

### Braces

#### Braces are used where optional

Braces are used with  `if`,  `else`,  `for`,  `do`  and  `while`  statements, even when the body is empty or contains only a single statement.

#### Nonempty blocks: K & R style

Braces follow the Kernighan and Ritchie style ("[Egyptian brackets](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)") for  _nonempty_  blocks and block-like constructs:

-   No line break before the opening brace.
-   Line break after the opening brace.
-   Line break before the closing brace.
-   Line break after the closing brace,  _only if_  that brace terminates a statement or terminates the body of a method, constructor, or  _named_  class. For example, there is  _no_  line break after the brace if it is followed by  `else`  or a comma.

Examples:
```java
      return () -> {
      while (condition()) {
        method();
      }
    };
    
    return new MyClass() {
      @Override public void method() {
        if (condition()) {
          try {
            something();
          } catch (ProblemException e) {
            recover();
          }
        } else if (otherCondition()) {
          somethingElse();
        } else {
          lastThing();
        }
      }
    };
```
A few exceptions for enum classes are given in,  [Enum classes](#enum-classes).

#### Empty blocks: may be concise
An empty block or block-like construct may be in K & R style (as described in  [Section 4.1.2](https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style)). Alternatively, it may be closed immediately after it is opened, with no characters or line break in between (`{}`),  **unless**  it is part of a  _multi-block statement_  (one that directly contains multiple blocks:  `if/else`  or  `try/catch/finally`).

Examples:

  // This is acceptable  void doNothing()  {}  // This is equally acceptable  void doNothingElse()  {  }

  // This is not acceptable: No concise empty blocks in a multi-block statement  try  { doSomething();  }  catch  (Exception e)  {}

### Block indentation: +2 spaces

Each time a new block or block-like construct is opened, the indent increases by two spaces. When the block ends, the indent returns to the previous indent level. The indent level applies to both code and comments throughout the block. (See the example in Section 4.1.2,  [Nonempty blocks: K & R Style](https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style).)

### One statement per line

Each statement is followed by a line break.

### Column limit: 100

Java code has a column limit of 100 characters. A "character" means any Unicode code point. Except as noted below, any line that would exceed this limit must be line-wrapped, as explained in Section 4.5,  [Line-wrapping](https://google.github.io/styleguide/javaguide.html#s4.5-line-wrapping).

Each Unicode code point counts as one character, even if its display width is greater or less. For example, if using  [fullwidth characters](https://en.wikipedia.org/wiki/Halfwidth_and_fullwidth_forms), you may choose to wrap the line earlier than where this rule strictly requires.

**Exceptions:**

1.  Lines where obeying the column limit is not possible (for example, a long URL in Javadoc, or a long JSNI method reference).
2.  `package`  and  `import`  statements (see Sections 3.2  [Package statement](https://google.github.io/styleguide/javaguide.html#s3.2-package-statement)  and 3.3  [Import statements](https://google.github.io/styleguide/javaguide.html#s3.3-import-statements)).
3.  Command lines in a comment that may be cut-and-pasted into a shell.

### Line-wrapping

**Terminology Note:**  When code that might otherwise legally occupy a single line is divided into multiple lines, this activity is called  _line-wrapping_.

There is no comprehensive, deterministic formula showing  _exactly_  how to line-wrap in every situation. Very often there are several valid ways to line-wrap the same piece of code.

**Note:**  While the typical reason for line-wrapping is to avoid overflowing the column limit, even code that would in fact fit within the column limit  _may_  be line-wrapped at the author's discretion.

**Tip:**  Extracting a method or local variable may solve the problem without the need to line-wrap.

#### Where to break
The prime directive of line-wrapping is: prefer to break at a  **higher syntactic level**. Also:

1.  When a line is broken at a  _non-assignment_  operator the break comes  _before_  the symbol. (Note that this is not the same practice used in Google style for other languages, such as C++ and JavaScript.)
    -   This also applies to the following "operator-like" symbols:
        -   the dot separator (`.`)
        -   the two colons of a method reference (`::`)
        -   an ampersand in a type bound (`<T extends  Foo  &  Bar>`)
        -   a pipe in a catch block (`catch  (FooException  |  BarException e)`).
2.  When a line is broken at an  _assignment_  operator the break typically comes  _after_  the symbol, but either way is acceptable.
    -   This also applies to the "assignment-operator-like" colon in an enhanced  `for`  ("foreach") statement.
3.  A method or constructor name stays attached to the open parenthesis (`(`) that follows it.
4.  A comma (`,`) stays attached to the token that precedes it.
5.  A line is never broken adjacent to the arrow in a lambda, except that a break may come immediately after the arrow if the body of the lambda consists of a single unbraced expression. Examples:
    
    MyLambda<String,  Long,  Object> lambda =  (String label,  Long value,  Object obj)  ->  {  ...  };  Predicate<String> predicate = str -> longExpressionInvolving(str);
    

**Note:**  The primary goal for line wrapping is to have clear code,  _not necessarily_  code that fits in the smallest number of lines.

#### Indent continuation lines at least +4 spaces

When line-wrapping, each line after the first (each  _continuation line_) is indented at least +4 from the original line.

When there are multiple continuation lines, indentation may be varied beyond +4 as desired. In general, two continuation lines use the same indentation level if and only if they begin with syntactically parallel elements.

Section 4.6.3 on  [Horizontal alignment](https://google.github.io/styleguide/javaguide.html#s4.6.3-horizontal-alignment)  addresses the discouraged practice of using a variable number of spaces to align certain tokens with previous lines.

### Whitespace

#### Vertical Whitespace

A single blank line always appears:

1.  _Between_  consecutive members or initializers of a class: fields, constructors, methods, nested classes, static initializers, and instance initializers.
    -   **Exception:**  A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create  _logical groupings_  of fields.
    -   **Exception:**  Blank lines between enum constants are covered in  [Section 4.8.1](https://google.github.io/styleguide/javaguide.html#s4.8.1-enum-classes).
2.  As required by other sections of this document (such as Section 3,  [Source file structure](https://google.github.io/styleguide/javaguide.html#s3-source-file-structure), and Section 3.3,  [Import statements](https://google.github.io/styleguide/javaguide.html#s3.3-import-statements)).

A single blank line may also appear anywhere it improves readability, for example between statements to organize the code into logical subsections. A blank line before the first member or initializer, or after the last member or initializer of the class, is neither encouraged nor discouraged.

_Multiple_  consecutive blank lines are permitted, but never required (or encouraged).

#### Horizontal whitespace

Beyond where required by the language or other style rules, and apart from literals, comments and Javadoc, a single ASCII space also appears in the following places  **only**.

1.  Separating any reserved word, such as  `if`,  `for`  or  `catch`, from an open parenthesis (`(`) that follows it on that line
2.  Separating any reserved word, such as  `else`  or  `catch`, from a closing curly brace (`}`) that precedes it on that line
3.  Before any open curly brace (`{`), with two exceptions:
    -   `@SomeAnnotation({a, b})`  (no space is used)
    -   `String[][] x =  {{"foo"}};`  (no space is required between  `{{`, by item 8 below)
4.  On both sides of any binary or ternary operator. This also applies to the following "operator-like" symbols:
    
    -   the ampersand in a conjunctive type bound:  `<T extends  Foo  &  Bar>`
    -   the pipe for a catch block that handles multiple exceptions:  `catch  (FooException  |  BarException e)`
    -   the colon (`:`) in an enhanced  `for`  ("foreach") statement
    -   the arrow in a lambda expression:  `(String str)  -> str.length()`
    
    but not
    -   the two colons (`::`) of a method reference, which is written like  `Object::toString`
    -   the dot separator (`.`), which is written like  `object.toString()`
5.  After  `,:;`  or the closing parenthesis (`)`) of a cast
6.  On both sides of the double slash (`//`) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.
7.  Between the type and variable of a declaration:  `List<String> list`
8.  _Optional_  just inside both braces of an array initializer
    -   `new  int[]  {5,  6}`  and  `new  int[]  {  5,  6  }`  are both valid
9.  Between a type annotation and  `[]`  or  `...`.

This rule is never interpreted as requiring or forbidding additional space at the start or end of a line; it addresses only  _interior_  space.

#### Horizontal alignment: never required

**Terminology Note:**  _Horizontal alignment_  is the practice of adding a variable number of additional spaces in your code with the goal of making certain tokens appear directly below certain other tokens on previous lines.

This practice is permitted, but is  **never required**  by Google Style. It is not even required to  _maintain_  horizontal alignment in places where it was already used.

Here is an example without alignment, then using alignment:

private  int x;  // this is fine  private  Color color;  // this too  private  int x;  // permitted, but future edits  private  Color color;  // may leave it unaligned

**Tip:**  Alignment can aid readability, but it creates problems for future maintenance. Consider a future change that needs to touch just one line. This change may leave the formerly-pleasing formatting mangled, and that is  **allowed**. More often it prompts the coder (perhaps you) to adjust whitespace on nearby lines as well, possibly triggering a cascading series of reformattings. That one-line change now has a "blast radius." This can at worst result in pointless busywork, but at best it still corrupts version history information, slows down reviewers and exacerbates merge conflicts.

### Grouping parentheses: recommended

Optional grouping parentheses are omitted only when author and reviewer agree that there is no reasonable chance the code will be misinterpreted without them, nor would they have made the code easier to read. It is  _not_  reasonable to assume that every reader has the entire Java operator precedence table memorized.

### Specific constructs

#### Enum classes

After each comma that follows an enum constant, a line break is optional. Additional blank lines (usually just one) are also allowed. This is one possibility:

private  enum  Answer  { YES {  @Override  public  String toString()  {  return  "yes";  }  }, NO, MAYBE }

An enum class with no methods and no documentation on its constants may optionally be formatted as if it were an array initializer (see Section 4.8.3.1 on  [array initializers](https://google.github.io/styleguide/javaguide.html#s4.8.3.1-array-initializers)).

private  enum  Suit  { CLUBS, HEARTS, SPADES, DIAMONDS }

Since enum classes  _are classes_, all other rules for formatting classes apply.

#### Variable declaration

#####  One variable per declaration

Every variable declaration (field or local) declares only one variable: declarations such as  `int a, b;`  are not used.

**Exception:**  Multiple variable declarations are acceptable in the header of a  `for`  loop.

##### Declared when needed

Local variables are  **not**  habitually declared at the start of their containing block or block-like construct. Instead, local variables are declared close to the point they are first used (within reason), to minimize their scope. Local variable declarations typically have initializers, or are initialized immediately after declaration.

#### Arrays

##### Array initializers: can be "block-like"

Any array initializer may  _optionally_  be formatted as if it were a "block-like construct." For example, the following are all valid (**not**  an exhaustive list):

new  int[]  {  new  int[]  {  0,  1,  2,  3  0,  }  1,  2,  new  int[]  {  3,  0,  1,  }  2,  3  }  new  int[]  {0,  1,  2,  3}

##### 4.8.3.2 No C-style array declarations

The square brackets form a part of the  _type_, not the variable:  `String[] args`, not  `String args[]`.

#### Switch statements

**Terminology Note:**  Inside the braces of a  _switch block_  are one or more  _statement groups_. Each statement group consists of one or more  _switch labels_  (either  `case FOO:`  or  `default:`), followed by one or more statements (or, for the  _last_  statement group,  _zero_  or more statements).

##### Indentation

As with any other block, the contents of a switch block are indented +2.

After a switch label, there is a line break, and the indentation level is increased +2, exactly as if a block were being opened. The following switch label returns to the previous indentation level, as if a block had been closed.

##### Fall-through: commented

Within a switch block, each statement group either terminates abruptly (with a  `break`,  `continue`,  `return`  or thrown exception), or is marked with a comment to indicate that execution will or  _might_  continue into the next statement group. Any comment that communicates the idea of fall-through is sufficient (typically  `// fall through`). This special comment is not required in the last statement group of the switch block. Example:

switch  (input)  {  case  1:  case  2: prepareOneOrTwo();  // fall through  case  3: handleOneTwoOrThree();  break;  default: handleLargeNumber(input);  }

Notice that no comment is needed after  `case  1:`, only at the end of the statement group.

##### The  `default`  case is present

Each switch statement includes a  `default`  statement group, even if it contains no code.

**Exception:**  A switch statement for an  `enum`  type  _may_  omit the  `default`  statement group,  _if_  it includes explicit cases covering  _all_  possible values of that type. This enables IDEs or other static analysis tools to issue a warning if any cases were missed.

#### Annotations

Annotations applying to a class, method or constructor appear immediately after the documentation block, and each annotation is listed on a line of its own (that is, one annotation per line). These line breaks do not constitute line-wrapping (Section 4.5,  [Line-wrapping](https://google.github.io/styleguide/javaguide.html#s4.5-line-wrapping)), so the indentation level is not increased. Example:

@Override  @Nullable  public  String getNameIfPresent()  {  ...  }

**Exception:**  A  _single_  parameterless annotation  _may_  instead appear together with the first line of the signature, for example:

@Override  public  int hashCode()  {  ...  }

Annotations applying to a field also appear immediately after the documentation block, but in this case,  _multiple_  annotations (possibly parameterized) may be listed on the same line; for example:

@Partial  @Mock  DataLoader loader;

There are no specific rules for formatting annotations on parameters, local variables, or types.

#### Comments

This section addresses  _implementation comments_. Javadoc is addressed separately in Section 7,  [Javadoc](https://google.github.io/styleguide/javaguide.html#s7-javadoc).

Any line break may be preceded by arbitrary whitespace followed by an implementation comment. Such a comment renders the line non-blank.

##### Block comment style

Block comments are indented at the same level as the surrounding code. They may be in  `/* ... */`  style or  `// ...`  style. For multi-line  `/* ... */`  comments, subsequent lines must start with  `*`  aligned with the  `*`  on the previous line.

/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */  */

Comments are not enclosed in boxes drawn with asterisks or other characters.

**Tip:**  When writing multi-line comments, use the  `/* ... */`  style if you want automatic code formatters to re-wrap the lines when necessary (paragraph-style). Most formatters don't re-wrap lines in  `// ...`  style comment blocks.

#### Modifiers

Class and member modifiers, when present, appear in the order recommended by the Java Language Specification:

public protected private abstract default static final transient volatile synchronized native strictfp

#### Numeric Literals

`long`-valued integer literals use an uppercase  `L`  suffix, never lowercase (to avoid confusion with the digit  `1`). For example,  `3000000000L`  rather than  `3000000000l`.

## Naming
### Rules common to all identifiers

Identifiers use only ASCII letters and digits, and, in a small number of cases noted below, underscores. Thus each valid identifier name is matched by the regular expression  `\w+`  .

In Google Style, special prefixes or suffixes are  **not**  used. For example, these names are not Google Style:  `name_`,  `mName`,  `s_name`  and  `kName`.

### Rules by identifier type

#### Package names

Package names are all lowercase, with consecutive words simply concatenated together (no underscores). For example,  `com.example.deepspace`, not  `com.example.deepSpace`  or  `com.example.deep_space`.

#### Class names

Class names are written in  [UpperCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).

Class names are typically nouns or noun phrases. For example,  `Character`  or  `ImmutableList`. Interface names may also be nouns or noun phrases (for example,  `List`), but may sometimes be adjectives or adjective phrases instead (for example,`Readable`).

There are no specific rules or even well-established conventions for naming annotation types.

_Test_  classes are named starting with the name of the class they are testing, and ending with  `Test`. For example,  `HashTest`  or  `HashIntegrationTest`.

#### Method names

Method names are written in  [lowerCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).

Method names are typically verbs or verb phrases. For example,  `sendMessage`  or  `stop`.

Underscores may appear in JUnit  _test_  method names to separate logical components of the name, with  _each_  component written in  [lowerCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case). One typical pattern is  `_<methodUnderTest>___<state>_`, for example  `pop_emptyStack`. There is no One Correct Way to name test methods.

#### Constant names

Constant names use  `CONSTANT_CASE`: all uppercase letters, with each word separated from the next by a single underscore. But what  _is_  a constant, exactly?

Constants are static final fields whose contents are deeply immutable and whose methods have no detectable side effects. This includes primitives, Strings, immutable types, and immutable collections of immutable types. If any of the instance's observable state can change, it is not a constant. Merely  _intending_  to never mutate the object is not enough. Examples:

// Constants  static  final  int NUMBER =  5;  static  final  ImmutableList<String> NAMES =  ImmutableList.of("Ed",  "Ann");  static  final  ImmutableMap<String,  Integer> AGES =  ImmutableMap.of("Ed",  35,  "Ann",  32);  static  final  Joiner COMMA_JOINER =  Joiner.on(',');  // because Joiner is immutable  static  final  SomeMutableType[] EMPTY_ARRAY =  {};  enum  SomeEnum  { ENUM_CONSTANT }  // Not constants  static  String nonFinal =  "non-final";  final  String nonStatic =  "non-static";  static  final  Set<String> mutableCollection =  new  HashSet<String>();  static  final  ImmutableSet<SomeMutableType> mutableElements =  ImmutableSet.of(mutable);  static  final  ImmutableMap<String,  SomeMutableType> mutableValues =  ImmutableMap.of("Ed", mutableInstance,  "Ann", mutableInstance2);  static  final  Logger logger =  Logger.getLogger(MyClass.getName());  static  final  String[] nonEmptyArray =  {"these",  "can",  "change"};

These names are typically nouns or noun phrases.

#### Non-constant field names

Non-constant field names (static or otherwise) are written in  [lowerCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).

These names are typically nouns or noun phrases. For example,  `computedValues`  or  `index`.

#### Parameter names

Parameter names are written in  [lowerCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).

One-character parameter names in public methods should be avoided.

#### Local variable names

Local variable names are written in  [lowerCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).

Even when final and immutable, local variables are not considered to be constants, and should not be styled as constants.

#### Type variable names

Each type variable is named in one of two styles:

-   A single capital letter, optionally followed by a single numeral (such as  `E`,  `T`,  `X`,  `T2`)
-   A name in the form used for classes (see Section 5.2.2,  [Class names](https://google.github.io/styleguide/javaguide.html#s5.2.2-class-names)), followed by the capital letter  `T`  (examples:  `RequestT`,  `FooBarT`).

### Camel case: defined

Sometimes there is more than one reasonable way to convert an English phrase into camel case, such as when acronyms or unusual constructs like "IPv6" or "iOS" are present. To improve predictability, Google Style specifies the following (nearly) deterministic scheme.

Beginning with the prose form of the name:

1.  Convert the phrase to plain ASCII and remove any apostrophes. For example, "Müller's algorithm" might become "Muellers algorithm".
2.  Divide this result into words, splitting on spaces and any remaining punctuation (typically hyphens).
    -   _Recommended:_  if any word already has a conventional camel-case appearance in common usage, split this into its constituent parts (e.g., "AdWords" becomes "ad words"). Note that a word such as "iOS" is not really in camel case  _per se_; it defies  _any_  convention, so this recommendation does not apply.
3.  Now lowercase  _everything_  (including acronyms), then uppercase only the first character of:
    -   ... each word, to yield  _upper camel case_, or
    -   ... each word except the first, to yield  _lower camel case_
4.  Finally, join all the words into a single identifier.

Note that the casing of the original words is almost entirely disregarded. Examples:

Prose form

Correct

Incorrect

"XML HTTP request"

`XmlHttpRequest`

`XMLHTTPRequest`

"new customer ID"

`newCustomerId`

`newCustomerID`

"inner stopwatch"

`innerStopwatch`

`innerStopWatch`

"supports IPv6 on iOS?"

`supportsIpv6OnIos`

`supportsIPv6OnIOS`

"YouTube importer"

`YouTubeImporter`  
`YoutubeImporter`*

*Acceptable, but not recommended.

**Note:**  Some words are ambiguously hyphenated in the English language: for example "nonempty" and "non-empty" are both correct, so the method names  `checkNonempty`  and  `checkNonEmpty`  are likewise both correct.

## Programming Practices

### `@Override`: always used
A method is marked with the  `@Override`  annotation whenever it is legal. This includes a class method overriding a superclass method, a class method implementing an interface method, and an interface method respecifying a superinterface method.

**Exception:**  `@Override`  may be omitted when the parent method is  `@Deprecated`.

### Caught exceptions: not ignored

Except as noted below, it is very rarely correct to do nothing in response to a caught exception. (Typical responses are to log it, or if it is considered "impossible", rethrow it as an  `AssertionError`.)

When it truly is appropriate to take no action whatsoever in a catch block, the reason this is justified is explained in a comment.

try  {  int i =  Integer.parseInt(response);  return handleNumericResponse(i);  }  catch  (NumberFormatException ok)  {  // it's not numeric; that's fine, just continue  }  return handleTextResponse(response);

**Exception:**  In tests, a caught exception may be ignored without comment  _if_  its name is or begins with  `expected`. The following is a very common idiom for ensuring that the code under test  _does_  throw an exception of the expected type, so a comment is unnecessary here.

try  { emptyStack.pop(); fail();  }  catch  (NoSuchElementException expected)  {  }

### Static members: qualified using class

When a reference to a static class member must be qualified, it is qualified with that class's name, not with a reference or expression of that class's type.

Foo aFoo =  ...;  Foo.aStaticMethod();  // good  aFoo.aStaticMethod();  // bad  somethingThatYieldsAFoo().aStaticMethod();  // very bad

### Finalizers: not used

It is  **extremely rare**  to override  `Object.finalize`.

**Tip:**  Don't do it. If you absolutely must, first read and understand  [_Effective Java_  Item 7,](http://books.google.com/books?isbn=8131726592)  "Avoid Finalizers," very carefully, and  _then_  don't do it.

## Javadoc

### Formatting

#### General form

The  _basic_  formatting of Javadoc blocks is as seen in this example:

/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */  public  int method(String p1)  {  ...  }

... or in this single-line example:

/** An especially short bit of Javadoc. */

The basic form is always acceptable. The single-line form may be substituted when the entirety of the Javadoc block (including comment markers) can fit on a single line. Note that this only applies when there are no block tags such as  `@return`.

#### Paragraphs

One blank line—that is, a line containing only the aligned leading asterisk (`*`)—appears between paragraphs, and before the group of block tags if present. Each paragraph but the first has  `<p>`  immediately before the first word, with no space after.

#### Block tags

Any of the standard "block tags" that are used appear in the order  `@param`,  `@return`,  `@throws`,  `@deprecated`, and these four types never appear with an empty description. When a block tag doesn't fit on a single line, continuation lines are indented four (or more) spaces from the position of the  `@`.

### The summary fragment

Each Javadoc block begins with a brief  **summary fragment**. This fragment is very important: it is the only part of the text that appears in certain contexts such as class and method indexes.

This is a fragment—a noun phrase or verb phrase, not a complete sentence. It does  **not**  begin with  `A {@code Foo} is a...`, or  `This method returns...`, nor does it form a complete imperative sentence like  `Save the record.`. However, the fragment is capitalized and punctuated as if it were a complete sentence.

**Tip:**  A common mistake is to write simple Javadoc in the form  `/** @return the customer ID */`. This is incorrect, and should be changed to  `/** Returns the customer ID. */`.

### Where Javadoc is used

At the  _minimum_, Javadoc is present for every  `public`  class, and every  `public`  or  `protected`  member of such a class, with a few exceptions noted below.

Additional Javadoc content may also be present, as explained in Section 7.3.4,  [Non-required Javadoc](https://google.github.io/styleguide/javaguide.html#s7.3.4-javadoc-non-required).

#### Exception: self-explanatory methods

Javadoc is optional for "simple, obvious" methods like  `getFoo`, in cases where there  _really and truly_  is nothing else worthwhile to say but "Returns the foo".

**Important:**  it is not appropriate to cite this exception to justify omitting relevant information that a typical reader might need to know. For example, for a method named  `getCanonicalName`, don't omit its documentation (with the rationale that it would say only  `/** Returns the canonical name. */`) if a typical reader may have no idea what the term "canonical name" means!

#### Exception: overrides

Javadoc is not always present on a method that overrides a supertype method.

#### Non-required Javadoc

Other classes and members have Javadoc  _as needed or desired_.

Whenever an implementation comment would be used to define the overall purpose or behavior of a class or member, that comment is written as Javadoc instead (using  `/**`).

Non-required Javadoc is not strictly required to follow the formatting rules of Sections 7.1.2, 7.1.3, and 7.2, though it is of course recommended.