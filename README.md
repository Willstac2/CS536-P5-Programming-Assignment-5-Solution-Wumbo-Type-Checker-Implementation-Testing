Download link :https://programmingsolver.com/questions-and-answers/cs536-p5-programming-assignment-5-solution-wumbo-type-checker-implementation-testing/

For this assignment you will write a type checker for Wumbo programs represented as abstract-syntax trees. Your main task will be to write type checking methods for the nodes of the AST. In addition you will need to:

1. Modify P5.java.

2. Write two test inputs: typeErrors.wumbo and test.wumbo to test your new code.

Getting Started

Download zip file

Start the project by downloading p5.zip

The files:

ast.java

Wumbo.cup

Wumbo.jlex

DuplicateSymException.java

EmpytSymTableException.java

ErrMsg.java

Sym.java

SymTable.java

Type.java

Makefile

P5.java

typeErrors.wumbo

test.wumbo

Specifications

Type Checking

Preventing Cascading Errors

Other Tasks

P5.java

Writing test Inputs

Some Advice

Type Checking

The type checker will determine the type of every expression represented in the abstract-syntax tree and will use that information to identify type errors. In the Wumbo language we have the following types:
int, bool, void (as function return types only), struct types, and function types.

http://pages.cs.wisc.edu/~loris/cs536/asn/p5/p5.html Page 1 of 7
CS536 P5 2020/4/11, 21:00

A struct type includes the name of the struct (i.e., when it was declared/defined). A function type includes the types of the parameters and the return type.

The operators in the Wumbo language are divided into the following categories:

logical: not, and, or

arithmetic: plus, minus, times, divide, unary minus

equality: equals, not equals

relational: less than (<), greater than (>), less than or equals (<=), greater than or equals (>=)

assignment: assign

The type rules of the Wumbo language are as follows:

logical operators and conditions: Only boolean expressions can be used as operands of logical operators or in the condition of an if or while statement. The result of applying a logical operator to bool operands is

bool.

repeat loop: Only integer expressions can be used in the times clause of an repeat statement

arithmetic and relational operators: Only integer expressions can be used as operands of these operators. The result of applying an arithmetic operator to int operand(s) is int. The result of applying a relational operator to int operands is bool.

equality operators: Only integer or boolean expressions can be used as operands of these operators. Furthermore, the types of both operands must be the same. The result of applying an equality operator is

bool.

Note: You don’t need to worry about equality operators between string literals. Either accepting it or declining it will be accepted in this assignment.

assignment operator: Only integer or boolean expressions can be used as operands of an assignment operator. Furthermore, the types of the left-hand side and right-hand side must be the same. The type of the result of applying the assignment operator is the type of the right-hand side.

cout and cin:

Only an int or bool expression or a string literal can be printed by cout. Only an int or bool identifer can be read by cin. Note that the identifier can be a field of a struct type (accessed using . ) as long as the field is an int or a bool.

function calls: A function call can be made only using an identifier with function type (i.e., an identifier that is the name of a function). The number of actuals must match the number of formals. The type of each actual must match the type of the corresponding formal.

function returns:

A void function may not return a value.
A non-void function may not have a return statement without a value.
A function whose return type is int may only return an int; a function whose return type is bool may only return a bool.

Note: some compilers give error messages for non-void functions that have paths from function start to function end with no return statement. For example, this code would cause such an error:

int f() {

cout << “hello”;

}

However, finding such paths is beyond the capabilities of our Wumbo compiler, so don’t worry about this

http://pages.cs.wisc.edu/~loris/cs536/asn/p5/p5.html Page 2 of 7
CS536 P5 2020/4/11, 21:00

kind of error.

You must implement your type checker by writing appropriate member methods for the different subclasses of ASTnode. Your type checker should find all of the type errors described in the following table; it must report the specified position of the error, and it must give exactly the specified error message. (Each message should appear on a single line, rather than how it is formatted in the following table.)

Type of Error Error Message Position to Report

Attempt to

Writing a function; e.g., “cout << f”, where f is a function name. write a
function

1st character of the function name.

Writing a struct name; e.g., “cout << P”, where P is the name of a

struct type.

Writing a struct variable; e.g., “cout << p”, where p is a variable declared to be of a struct type.

Attempt to write a struct name

Attempt to write a struct variable

1st character of the

struct name.

1st character of the struct variable.

Writing a void value (note: this can only happen if there is an Attempt to
attempt to write the return value from a void function); e.g., “cout
<< f()”, where f is a void function. write void

Reading a function: e.g., “cin >> f”, where f is a function name. Attempt to
read a
function

1st character of the function name.

1st character of the function name.

Reading a struct name; e.g., “cin >> P”, where P is the name of a

struct type.

Reading a struct variable; e.g., “cin >> p”, where p is a variable declared to be of a struct type.

Calling something other than a function; e.g., “x();”, where x is not a function name. Note: In this case, you should not type-check the actual parameters.

Calling a function with the wrong number of arguments. Note: In this case, you should not type-check the actual parameters.

Attempt to read a struct name

Attempt to read a struct variable

Attempt to call a non-function

Function call with wrong number of args

1st character of the

struct name.

1st character of the struct variable.

1st character of the variable name.

1st character of the function name.

Calling a function with an argument of the wrong type. Note: you should only check for this error if the number of arguments is correct. If there are several arguments with the wrong type, you must give an error message for each such argument.

Returning from a non-void function with a plain return statement (i.e., one that does not return a value).

Returning a value from a void function.

 

Returning a value of the wrong type from a non-void function.

 

 

Applying an arithmetic operator (+, -, *, /) to an operand with type other than int. Note: this includes the ++ and — operators.

Type of actual does not match type of formal

Missing return value

Return with a value in a void function

Bad return value

Arithmetic operator applied to non-numeric

1st character of the first identifier or literal in the actual parameter.

0,0
1st character of the first identifier or literal in the returned expression.

1st character of the first identifier or literal in the returned expression.

1st character of the first identifier or literal in an operand that is an expression of the wrong

http://pages.cs.wisc.edu/~loris/cs536/asn/p5/p5.html Page 3 of 7
CS536 P5 2020/4/11, 21:00

 

 

Applying a relational operator (<, >, <=, >=) to an operand with type other than int.

 

Applying a logical operator (!, &&, ||) to an operand with type other than bool.

 

Using a non-bool expression as the condition of an if.

 

Using a non-bool expression as the condition of a while.

 

Using a non-integer expression as the times clause of a repeat.

Applying an equality operator (==, !=) to operands of two different types (e.g., “j == true”, where j is of type int), or assigning a value of one type to a variable of another type (e.g., “j = true”, where j is of type int).

operand

 

Relational operator applied to non-numeric operand

Logical operator applied to non-bool operand

Non-bool expression used as an if condition

Non-bool expression used as a while condition

Non-integer expression used as a repeat clause

Type mismatch

type.

1st character of the first identifier or literal in an operand that is an expression of the wrong type.

1st character of the first identifier or literal in an operand that is an expression of the wrong type.

1st character of the first identifier or literal in the condition.

1st character of the first identifier or literal in the condition.

1st character of the first identifier or literal in the condition.

1st character of the first identifier or literal in the left-hand operand.

Applying an equality operator (==, !=) to void function operands (e.g., “f() == g()”, where f and g are functions whose return type is void).

Equality
operator 1st character of the first
applied to
function name.
void
functions

Comparing two functions for equality, e.g., “f == g” or “f != g”, where f and g are function names.

Comparing two struct names for equality, e.g., “A == B” or “A != B”, where A and B are the names of struct types.

Comparing two struct variables for equality, e.g., “a == b” or “a != b”, where a and a are variables declared to be of struct types.

Equality operator applied to functions

Equality operator applied to struct names

Equality operator applied to struct variables

1st character of the first function name.

1st character of the first
struct name.

1st character of the first struct variable.

Assigning a function to a function; e.g., “f = g;”, where f and g Function
are function names. assignment

Assigning a struct name to a struct name; e.g., “A = B;”, where Struct name
A and B are the names of struct types. assignment

Assigning a struct variable to a struct variable; e.g., “a = b;”, Struct
where a and b are variables declared to be of struct types. variable
assignment

1st character of the first function name.

1st character of the first

struct name.

1st character of the first struct variable.

http://pages.cs.wisc.edu/~loris/cs536/asn/p5/p5.html Page 4 of 7
CS536 P5 2020/4/11, 21:00

Preventing Cascading Errors

A single type error in an expression or statement should not trigger multiple error messages. For example, assume that P is the name of a struct type, p is a variable declared to be of struct type P, and f is a function that has one integer parameter and returns a bool. Each of the following should cause only one error message:

cout << P + 1 // P + 1 is an error; the write is OK
(true + 3) * 4 // true + 3 is an error; the * is OK
true && (false || 3) // false || 3 is an error; the && is OK
f(“a” * 4); // “a” * 4 is an error; the call is OK
1 + p(); 3) == x // p() is an error; the + is OK
(true + // true + 3 is an error; the == is OK
// regardless of the type of x

One way to accomplish this is to use a special ErrorType for expressions that contain type errors. In the second example above, the type given to (true + 3) should be ErrorType, and the type-check method for the multiplication node should not report “Arithmetic operator applied to non-numeric operand” for the first
operand. But note that the following should each cause two error messages (assuming the same declarations of f as above):

true + “hello” // one error for each of the non-int operands of the +
1 + f(true) // one for the bad arg type and one for the 2nd operand of the +
1 + f(1, 2) // one for the wrong number of args and one for the 2nd operand of the +
return 3+true; // in a void function: one error for the 2nd operand to +
// and one for returning a value

To provide some help with this issue, here is an example input file , along with the corresponding error messages. (Note: This is not meant to be a complete test of the type checker; it is provided merely to help you understand some of the messages you need to report, and to help you find small typos in your error messages. If you run your program on the example file and put the output into a new file, you can use the Linux utility diff to compare your file of error messages with the one supplied here. This will help both to make sure that your code finds the errors it is supposed to find, and to uncover small typos you may have made in the error messages.)

Other Tasks

P5.java

You will need to modify P5.java.

The main program, P5.java, will be similar to P4.java, except that if it calls the name analyzer and there are no errors, it will then call the type checker.

Writing Test Inputs

You will need to write two input files to test your code:

1. typeErrors.wumbo should contain code with errors detected by the type checker. For every type error listed in the table above, you should include an instance of that error for each of the relevant operators, and in each part of a program where the error can occur (e.g., in a top-level statement, in a statement inside a while loop, etc).

2. test.wumbo should contain code with no errors that exercises all of the type-check methods that you wrote for the different AST nodes. This means that it should include (good) examples of every kind of statement and expression.

Note that your typeErrors.wumbo should cause error messages to be output, so to know whether your type

http://pages.cs.wisc.edu/~loris/cs536/asn/p5/p5.html Page 5 of 7
CS536 P5 2020/4/11, 21:00

checker behaves correctly, you will need to know what output to expect.

Part of the grade depends on how thoroughly the input files you used, test the program. Make sure that you submit the input files you used to test your program.
Some Advice

Here are few words of advice about various issues that come up in the assignment:

For this assignment you are free to make any changes you want to the code in ast.java. For example, you may find it helpful to make small changes to the class hierarchy, or to add new fields and/or methods to some classes.

As for name analysis, think about which AST nodes need to have type-check methods. For example, for type checking, you do not need to visit nodes that represent declarations, only those that represent statements.

Handing in

Please read the following handing in instructions carefully. You will be needed to submit the entire working folder as a compressed file as given below.

lastname.firstname.lastname.firstname.P5.zip

+—+ deps/
+—+ ast.java
+—+ Wumbo.cup
+—+ Wumbo.jlex
+—+ DuplicateSymException.java
+—+ EmptySymTableException.java
+—+ ErrMsg.java
+—+ Makefile
+—+ P5.java
+—+ Sym.java
+—+ SymTable.java
+—+ Type.java
+—+ typeErrors.wumbo
+—+ test.wumbo

+—+ lastname.firstname.lastname.firstname.P5.pdf

Please ensure that you do not include any extra sub-directories. Do not turn in any .class files. If you accidentally turn in (or create) extra files or subdirectories, please remove them from your submission zip file.

Joining a Canvas group with your partner (also required for those who work alone)

To faciliate grading, before you submit the compressed file, please join a Canvas group with your partner (guide) so you can both receive grades for the same submission. Since you’re working on P5, please join an empty group whose name is in the form of “P5-Pair #” (type “P5” in the search bar). Note that we have created the groups for you so you only need to join an empty one with your partner. If you work alone, please join an empty group as well.

 

 

http://pages.cs.wisc.edu/~loris/cs536/asn/p5/p5.html Page 6 of 7
CS536 P5 2020/4/11, 21:00

If you are working in a pair, have only one member submit the program. Include both persons’ name as given above. Also, mention the teammate’s name as a comment while submitting the assignment on canvas.

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

http://pages.cs.wisc.edu/~loris/cs536/asn/p5/p5.html Page 7 of 7

加QQ codinghelp Email: programminghelp1@proton.me
