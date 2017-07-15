# Guide for Contributing Code

The purpose of this guide is to set some standards for how anyone can write IDL code and contribute to repositories for IDL and ENVI. These are meant as guidelines and some of the formatting requests below are not required, but they will help everything be consistent and easier for beginners to follow along with.

In general, the rule is to write code **with readability in mind** so that others can easily understand what you are doing and be able to follow your logic. The first and most important piece of that is comments!


## Comments

Comments are a critical part of any programming project especially when users that are not as advanced might be using the code. Please do your best to comment everywhere and make it clear what you are doing.

### General Comments

To make sure that it is clear to any users what any code is doing, it is important to frequently comment your code. Typically this should look like a few words for each chunk of code that is written. Here is an example:

```
;check for bad data values
idx = where(someArr eq 0, countArr)
if (countArr gt 0) then begin
  ;remove bad values from data
  someArr[idx] = 1
endif

;save the data to disk
openw, lun, file, /GET_LUN
printf, lun, someArr
free_lun, lun
```

Note that for experienced programmers the steps may be apparent, but new programmers might not understand all that the code is doing. Plenty of comments make it easy for everyone to understand the code so that it can be re-used in other places.


### Routine Comments

When using the IDL workbench, you can very easily add a comment template for any selected routine. To do so, you simply need to: save your work, highlight the name of the function or procedure, right click, and select add routine comments. 

For this, IDL will add a template for comments with the arguments and keywords already started for you. When filling out the parameter description, you should use follow the format:

```
;+
; :Description:
;   This function/procedure does blah.
;
; :Params:
;   some_param: direction, required, type=IDL_Type
;     This parameter does blah.
;
; :Keywords:
;   SOME_KEYWORD: direction, required, type=IDL_Type, default=defaultValue
;     Some short description of what the keyword does.
;
; :Author: Zachary Norman - Github: znorman17
;
;-
```

Where the values represent:

- The direction of the keyword/argument (i.e. input or output)

- Whether the parameter needs to be provided or not

- The data type that is expected (i.e float, double, structure, ENVIRaster)

- If relevant, the default value for the parameter. Not a required addition.

The format for the keywords above follows that of IDL doc (a neat package developed by Mike Galloy).


### Header Comments (optional)

In addition to adding comments for each separate routine you should also add an overall comment section to the top of each PRO code file. This should just have a basic description of the contents of the file and possibly a short example for how to run the routine.

If you do not add comments at the beginning of each file, then just make sure that there is a place somewhere that describes the purpose of the code that is easy to find.



## Naming Conventions and Formatting

Here are some general rules to follow for how syntax should be formatted.


### Generic Variables

Underscores or camel case are each acceptable. Try to make names descriptive and have a short comment when the variable is created describing what it is for. For example:

```
;flag for type of processing being performed
datFlag = 1
```


### Procedures and Functions

The names of routines can have underscores and be camel case.

The most important routine in the PRO code **MUST** be at the bottom of the file and have the same name as the PRO file. In other words, a file called "some_routine.pro" should have a procedure or function called "some_routine" as the last routine in the file.

The file name should **ALWAYS** be lower case (important for non-Windows operating systems). The routine name in the PRO code can have capital letters.

When you have multiple routines that are all used for the same PRO code, then the naming convention should be similar to the following example (to make sure there are no name space issues in IDL). For example, I have a PRO code file called "awesome_classification.pro" and my routines are named (in order from the top to the bottom of the file):

```
awesome_classification_calculateValues
awesome_classification_preprocessData
awesome_classification
```

While this makes the routine names be a little bit longer, they will be more descriptive as to where they come from and they will make sure that there are no collisions with file naming.



### Keywords

**ALL** keywords should be written such that the left hand side and the right hand side have the same name. For beginner programmers this concept can be challenging so it helps things be easier to understand. 

The keywords should also be capitalized on the left and lower case (or camel case) on the right. Here is an example of how you should define them:

```
pro someProcedure, FIRST_KEYWORD = first_keyword, SECOND_KEYWORD = second_keyword
  compile_opt idl2
  ;do something...
end
```

Here is an example of how you should would format the call to this procedure when using it:

```
someProcedure, FIRST_KEYWORD = fooBar, SECOND_KEYWORD = 42
```

If your keywords are for outputs, then name them appropriately such as:

```
pro someProcedure, FIRST_KEYWORD = first_keyword, OUTPUT_DATA = output_data
  compile_opt idl2
  ;do something...
end
```

### Arguments

Don't use arguments for returning results, instead use keywords. Arguments should **ONLY** be used for required information that is needed for a function or procedure and not for optional information. 


### Keywords vs Arguments

Similar to functions and procedures, keywords and arguments can be used interchangeably. The rule of thumb is that arguments are **ONLY** for required inputs and **NOT** outputs. Keywords are traditionally for optional inputs and additional outputs, but can also be used for inputs to routines.

This is also based on personal preference and the type of code that is being developed. For example, if you are writing a task for ENVI or IDL, then all of your parameters need to be used as keywords so this may be easier (and habit) when writing code. This is just fine as long as it makes sense what the code is doing.


## Control Statements

In IDL the control statements (i.e. if, for, while) should all be written as multi-line statements to make them easier to read. This means that they should be written in the form:

```
if (this eq 1) then begin
  ;do something
endif
```

Some exceptions are if you have a simple statement such as:

```
if (1 eq var) then continue
```

This does not need a multi-line code block because it is easy to read as is.


## Functions vs. Procedures

Because IDL has two different types of routines (i.e. functions and procedures), there are different times when it is appropriate to use one over the other. However, this is also based on personal preference so just think about if it would be easy for others to understand your code or not.

The general rule is that functions are used when you are returning values and procedures are not. **However**, if more than one variable needs to be returned then procedures are a great alternative with multiple keywords.


## Compile-opt idl2

Every routine **MUST** have ```compile_opt idl2``` at the beginning of each procedure or function. It forces programmers to use square brackets for indexing arrays and changes the default data type to long instead of unsigned integer. For example this would look like:

```
pro someProcedure, FIRST_KEYWORD = first_keyword, SECOND_KEYWORD = second_keyword
  compile_opt idl2
  ;do something...
end
```
