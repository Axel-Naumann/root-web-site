---
title: Coding Conventions
---

*   [Naming conventions](#Naming_conventions)
*   [Class definition conventions](#Class_definition_conventions)
    *   [Inline](#Inline)
    *   [Declaration Order](#Inline)
*   [Avoid raw C types](#Avoid_raw_C_types)
*   [Exception handling](#Exception_handling)
*   [Namespaces](#Namespaces)
*   [Using comments to document the code](#Using_comments_to_document_the_c)
*   [Source file layout](#Source_file_layout)
    *   [Header file layout](#Implementation_file_layout)
    *   [Implementation file layout](#Implementation_file_layout)
*   [Preferred Coding Style](#Preferred_Coding_Style)
    *   [Indentation](#Indentation)
    *   [Placing Braces and Spaces](#Placing_Braces_and_Spaces)
*   [ClangFormat](#ClangFormat)
*   [Astyle](#Astyle)
*   [Where to go from here](#Where_to_go_from_here)

## <a id="Naming_conventions" name="Naming_conventions">Naming conventions</a>

For naming conventions we follow the [Taligent](http://root.cern.ch/TaligentDocs/TaligentOnline/DocumentRoot/1.0/Docs/books/WM/WM_63.html#HEADING77) rules. They have written a very large body of C++ and their rules seem well thought out. No need to invent something new. The only addition/change we made is to append an `_t` to `typedef`s and simple `struct`s, e.g.:

```
typedef int Int_t ; struct Simple_t { ..... } ; 
```

Addherence to the rules is mandatory.  After a while one really gets used to the fact that all class fields start with an `f` followed by a capitalized word, `fEnergy`, or that `TStreamerInfo` is a class. If the convention is sporadically violated debugging becomes a nightmare. The usage of a standard begin letter or token for the different types also makes it easy to parse and search the code using simple tools.

## <a id="Class_definition_conventions" name="Class_definition_conventions">Class definition conventions</a>

Also here the [Taligent guide](http://root.cern.ch/TaligentDocs/TaligentOnline/DocumentRoot/1.0/Docs/books/WM/WM_69.html) is quite reasonable. Of course, no class data member should ever be public. Make the data fields always private. Or protected, if you want to grant an inherited class direct access.

### <a id="Inline" name="Inline">Inline</a>

Add trivial get or setters directly in the class definition. This improves reading time since one does not have to look for it somewhere else. Add more complex inlines (longer than one line) at the bottom of the .h file. Creating separate `.icc` files increases the build time, the complexity of the build system and, more importantly, increases the number of files one possibly has to scan to find a piece of code.

### <a id="Declaration_Order" name="Declaration_Order">Declaration Order</a>

In the class definition we first declare all private data members, followed by the private static members, the private methods and the private static methods. Then the protected members and methods and finally the public methods (no public data members). We put private members first since that is the language default and it gives the developer a quick view on what data members are used in a class.

## <a id="Avoid_raw_C_types" name="Avoid_raw_C_types">Avoid raw C types</a>
Avoid the use of raw C types like `int`, `long`, `float`, `double` when using data that might be written to disk. For example, the sizes of `int` and `long` are machine dependent. On 32 bit machines `int`s and `long`s are 32 bits, but on 64 bit processors an `int` can be either 32 or 64 bits and a `long` 64 bits, depending on the processor. For portability reasons and consistent numerical results use the typedefs provided by ROOT's `Rtypes.h` for the basic raw C types. E.g.: Double_t, Float_t, Int_t etc.

## <a id="Exception_handling" name="Exception_handling">Exception handling</a>
Don't let every method throw an exception when a simple error return code is often enough.

## <a id="Namespaces" name="Namespaces">Namespaces</a>
In ROOT 5 all classes are in the `ROOT` namespace. Some packages will be in a sub-namespace, e.g. `ROOT::Reflex`. For backward compatibility with the previous versions of ROOT, where all classes were in the global namespace, we have by default `using namespace ROOT;` in all headers. However, this can be turned off by defining the `USE_ROOT_NAMESPACE` macro.

## <a id="Using_comments_to_document_the_c" name="Using_comments_to_document_the_c">Using comments to document the code</a>
ROOT chose Doxygen for its code documentation: please refer to [this how-to](https://root.cern.ch/formatting-comments-doxygen) for all the details.

## <a id="Source_file_layout" name="Source_file_layout">Source file layout</a>

Each source file, header or implementation file starts with a module identification line and an author line, e.g.:

<pre class="brush:cpp;">/ @(#)root/net / Author: Fons Rademakers 18/12/96 </pre>

Where in the module identification line the file package is described by `root/package`, in this case the `net` package.

### <a id="Header_file_layout" name="Header_file_layout">Header file layout</a>

Each header file has the following layout:

*   Module identification line
*   Author line
*   Copyright notice
*   Mulitple inclusion protection macro
*   Headers file includes
*   Forward declarations
*   Actual class definition

For a typical example see [TObject.h](http://root.cern.ch/root/htmldoc/TObject.h).

Note the explicit checks to avoid unnecessarily opening already included header files. For large systems this kind of defensive measures can make quite a difference in compile time. Also never include a header file when a forward declaration is enough. On include header files for base classes or classes that are used by value in the class definition.

### <a id="Implementation_file_layout" name="Implementation_file_layout">Implementation file layout</a>

Each implementation file has the following layout:

*   Module identification line
*   Author line
*   Copyright notice
*   Class description comments (see above)
*   Header file includes
*   Actual method implementation

For a typical example see [TObject.cxx](http://root.cern.ch/root/htmldoc/src/TObject.cxx.html).
Note the mandatory method separator line:
```
////////////////////////////////////////
```
exactly 80 characters long.

## <a id="Preferred_Coding_Style" name="Preferred_Coding_Style">Preferred Coding Style</a>
Here we describe our preferred coding style. Coding style is very personal and we don't want to force our views on anybody. But for any contributions to the ROOT system that we have to maintain we would like you to follow our coding style.

### <a id="Indentation" name="Indentation">Indentation</a>
To be able to keep as much code as possible in the visible part of the editor of to avoid over abundant line wrapping we use indentation of 3 spaces. No tabs since they give the code always a different look depending on the tab settings of the original coder. If everything looks nicely lined up with a tab setting of 4 spaces, it does not look so nicely anymore when the tab setting is changed to 3, 5, etc. spaces.

### <a id="Placing_Braces_and_Spaces" name="Placing_Braces_and_Spaces">Placing Braces and Spaces</a>
The other issue that always comes up in C/C++ styling is the placement of braces and spaces. Unlike the indent size, there are few technical reasons to choose one placement strategy over the other, but the preferred way, as shown to us by the prophets Kernighan and Ritchie, is to put the opening brace last on the line, and put the closing brace first, thus:
```
if (x is true) {
   we do y
}
```

However, there is one special case, namely functions: they have the opening brace at the beginning of the next line, thus:
```
int function (int x)
{
   body of function
}
```
Functions are special (you can't nest them in C/C++).
Note that the closing brace is empty on a line of its own, **except** in the cases where it is followed by a continuation of the same statement, ie a `while` in a `do`-statement or an `else` in an `if`-statement, like this:
```
do {
   body of do - loop
} while ( condition ) ;
```
and
```
if (x == y) {
   ...
} else if (x > y) {
   ...
} else {
   ... 
}
```

Note that this brace-placement also minimizes the number of empty (or almost empty) lines, without any loss of readability. Thus, as the supply of new-lines on your screen is not a renewable resource (think 25-line terminal screens here), you have more empty lines to put comments on.

Notice also in the above examples the usage of spaces around keywords, operators and parenthesis/braces. Avoid the following free styles:
```
if (x == y) {
```

or any derivative thereof.

## <a id="ClangFormat" name="ClangFormat">ClangFormat</a>
ClangFormat is a Clang tool which allows you to format your code. This is the configuration file for it:
```
---
Language:        Cpp
# BasedOnStyle:  LLVM
AccessModifierOffset: -3
AlignAfterOpenBracket: Align
AlignConsecutiveAssignments: true
# This would be nice to have but seems to also (mis)align function parameters
AlignConsecutiveDeclarations: true
AlignEscapedNewlinesLeft: true
AlignOperands:   true
AlignTrailingComments: true
AllowAllParametersOfDeclarationOnNextLine: true
AllowShortBlocksOnASingleLine: false
AllowShortCaseLabelsOnASingleLine: true
AllowShortFunctionsOnASingleLine: Inline
AllowShortIfStatementsOnASingleLine: true
AllowShortLoopsOnASingleLine: true
# This option is "deprecated and is retained for backwards compatibility."
# AlwaysBreakAfterDefinitionReturnType: None
AlwaysBreakAfterReturnType: None
AlwaysBreakBeforeMultilineStrings: false
AlwaysBreakTemplateDeclarations: true
BinPackArguments: true
BinPackParameters: true
BraceWrapping:
  AfterClass:      false
  AfterControlStatement: false
  AfterEnum:       false
  AfterFunction:   true
  AfterNamespace:  false
  AfterObjCDeclaration: false
  AfterStruct:     false
  AfterUnion:      false
  BeforeCatch:     false
  BeforeElse:      false
  IndentBraces:    false
BreakBeforeBinaryOperators: None
BreakBeforeBraces: Custom
BreakBeforeTernaryOperators: true
BreakConstructorInitializersBeforeComma: false
ColumnLimit:     120
CommentPragmas:  '^ IWYU pragma:'
ConstructorInitializerAllOnOneLineOrOnePerLine: false
ConstructorInitializerIndentWidth: 3
ContinuationIndentWidth: 3
Cpp11BracedListStyle: true
DerivePointerAlignment: false
DisableFormat:   false
ExperimentalAutoDetectBinPacking: false
ForEachMacros:   [ foreach, Q_FOREACH, BOOST_FOREACH ]
IncludeCategories:
  - Regex:           '^("|<)T'
    Priority:        4
  - Regex:           '^("|<)ROOT/'
    Priority:        5
  - Regex:           '^<.*\.h>'
    Priority:        1
  - Regex:           '^"(llvm|llvm-c|clang|clang-c)/'
    Priority:        2
  - Regex:           '^(<|"(gtest|isl|json)/)'
    Priority:        3
  - Regex:           '.*'
    Priority:        6
IndentCaseLabels: false
IndentWidth:     3
IndentWrappedFunctionNames: false
KeepEmptyLinesAtTheStartOfBlocks: true
MacroBlockBegin: ''
MacroBlockEnd:   ''
MaxEmptyLinesToKeep: 1
NamespaceIndentation: None
ObjCBlockIndentWidth: 3
ObjCSpaceAfterProperty: false
ObjCSpaceBeforeProtocolList: true
PenaltyBreakBeforeFirstCallParameter: 19
PenaltyBreakComment: 300
PenaltyBreakFirstLessLess: 120
PenaltyBreakString: 1000
PenaltyExcessCharacter: 1000000
PenaltyReturnTypeOnItsOwnLine: 60000
PointerAlignment: Right
ReflowComments:  true
SortIncludes: false
SpaceAfterCStyleCast: false
SpaceBeforeAssignmentOperators: true
SpaceBeforeParens: ControlStatements
SpaceInEmptyParentheses: false
SpacesBeforeTrailingComments: 1
SpacesInAngles:  false
SpacesInContainerLiterals: true
SpacesInCStyleCastParentheses: false
SpacesInParentheses: false
SpacesInSquareBrackets: false
Standard:        Cpp11
TabWidth:        3
UseTab:          Never
...
```

## <a id="Astyle" name="Astyle">Astyle</a>
If you don't have access to ClangFormat, [astyle](http://astyle.sourceforge.net/) can be useful. Starting from a code like this:

```
int aap ( int inp ) { 
   if ( inp > 0 ) { 
      return 0 ; 
      int a = 1 ; 
      if ( inp == 0 && a == 1 ) { 
         printf ( >"this is a very long line that is not yet ending" , a, inp, a, inp, a , inp ) ; 
        a + = inp ; return a ; 
     } 
   } else { 
      return 1 ; 
   }   
   if ( inp == 0 ) 
      return - 1 ; 
      return 1 ; 
   }
```

You will find back like this:
```
int aap (int inp) {
   if (inp > 0) { 
      return 0 ; 
      int a = 1 ; 
      if (inp == 0 && a == 1) { 
         printf ("this is a very long line that is not yet ending" , a, inp, a, inp, a, inp ) ; 
         a + = inp ; return a ; 
      } 
   } 
   else { 
      return 1 ; 
   }   
   if (inp == 0) 
      return - 1 ; 
   return 1 ; 
}
```

 Get at least version 2.0 and use the following `~/.astylerc`:

```
# ROOT code formatting style
# Note that the brackets=linux option is not available starting from astyle 2.04
#brackets=linux
style=stroustrup
mode=c
align-pointer=name
indent=spaces=3
indent-switches
indent-cases
indent-namespaces
max-instatement-indent=40
indent-preprocessor
convert-tabs
pad-header
pad-oper
unpad-paren
```

## <a id="Where_to_go_from_here" name="Where_to_go_from_here">Where to go from here</a>
For the rest read the [Taligent Guide](http://root.cern.ch/TaligentDocs/TaligentOnline/DocumentRoot/1.0/Docs/books/WM/WM_1.html) and use common sense.
