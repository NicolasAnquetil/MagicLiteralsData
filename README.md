# Magic Literals Data

Data for the paper: "What Do Developers Consider Magic literals? -- A Smalltalk Perspective"


For more information please contact nicolas.anquetil@inria.fr .


The file 'allLiteralData.csv' contains a classification of cerca 24K literals performed by 26 different participants (P1 to P26)


# Data

Columns are:

- literal -- the value of the literal (eg: "1")
- kind -- the type among: Integer, String, Symbol, Boolean, Array, UndefinedObject (=nil), Float, and Character
- astNode -- the AST Role of the literal among: Argument (of a message), Operand (of an operation), argument of a Pragma, value Returned, value Assigned to a variable, Receiver of a message, member of a Sequence (ie more or less an expressionStatement with the literal as expression), member of an array.
- specialArg -- whether the literal appears in one of the specific context that are linked to language conventions: "loopFrom1" (literal "1" is start of a loop), "increment1" (literal "1" used as increment or decrement), "streamOutput" (string or character literal output on a stream), "nameArg" (string literal argument of a "name:" message), "other" (none of the previous cases)
- status -- whether the literal was classified "Magic", "NotMagic", or "Undecided"
- idInMethod -- the position of the literal in the list of all literals that its parent method contains (whether it is the 1st, 2nd, ... of its parent method). Will always be >=1 and <= "nbLiteralsInMethod" (see below)
- isConstantMethod -- whether the parent method of the literal only returns it (which is the convention in Pharo to implement constant)
- method -- name of the class and method containing the literal
- specialMethod - whether the parent method is a "test" method, an "example" method, or a "normal" method (ie not test or example)
- commentLength -- length (in character) of possible comments in the parent method
- nbLiteralsInMethod -- total number of literal in the parent method (related to "idInMethod")
- project -- name of the system where the literal was found
- expId -- batch number in the project where that literal was classified by a participant ("expert").
- expert -- code of the participant that classified the literal
- expertise -- self declared participant's knowledge of the system. One of: "low", "medium", or "high"


# Keys

The tuple (idInMethod, method) uniquely identifies a literal.

The tuple (project, expId) uniquely identifies a batch of 50 literals classified by a participant

A batch contains 50 literals and the same literal may appear in different batches (intended redundancy)


# Heuristic validation

Heuristic 1 (impact of participant expertise) -- count all literals Magic or not ("status" column) for each value of column "expertise"

Heuristic 2 (self describing literals) -- for all self describing literals, count number of "status"= "Magic" or not. Self describing literals are:
  - booleans, "type" = "Boolean"
  - empty pointer, "type" = "UndefinedObject"
  - empty arrays, "type" = "Array" and "literal" = "#()" or "literal" = "{}"
  - strings, "type" = "String"
  - symbols "type" = "Symbol"

Heuristic 3 (coding convention) -- for all literals in specified contexts ("specialArg" != "other"), count number of "status"= "Magic" or not.

Heuristic 4 (assigned to a variable/returned by a method) -- for all literals directly assigned to a variable ("astNode" = "assign.") or directly returned by a method ("astNode" = "return"), count number of "status"= "Magic" or not.

Heuristic 5 (pragma argument) -- for all literals argument of a pragma ("astNode" = "pragma"), count number of "status"= "Magic" or not.

Heuristic 6 (in Test or Example method) -- for all literals in a test method ("specialMethod" = "test") or example method ("specialMethod" = "example"), count number of "status"= "Magic" or not.
