# R7 classes: print nicely

    Code
      foo2
    Output
      <R7_class>
      @ name  :  foo2
      @ parent: <foo1>
      @ properties:
       $ x: <integer>
       $ y: <integer>
    Code
      str(foo2)
    Output
      <foo2/foo1/R7_object> constructor
       @ name       : chr "foo2"
       @ parent     : <foo1/R7_object> constructor
       @ package    : NULL
       @ properties :List of 2
       .. $ x: <R7_property> 
       ..  ..$ name   : chr "x"
       ..  ..$ class  : <R7_base_class>: <integer>
       ..  ..$ getter : NULL
       ..  ..$ setter : NULL
       ..  ..$ default: NULL
       .. $ y: <R7_property> 
       ..  ..$ name   : chr "y"
       ..  ..$ class  : <R7_base_class>: <integer>
       ..  ..$ getter : NULL
       ..  ..$ setter : NULL
       ..  ..$ default: NULL
       @ abstract   : logi FALSE
       @ constructor: function (x = class_missing, y = class_missing)  
       @ validator  : NULL
    Code
      str(list(foo2))
    Output
      List of 1
       $ : <foo2/foo1/R7_object> constructor

# R7 classes: checks inputs

    Code
      new_class(1)
    Error <simpleError>
      `name` must be a single string
    Code
      new_class("foo", 1)
    Error <simpleError>
      Can't convert `parent` to a valid class. Class specification must be an R7 class object, the result of `new_S3_class()`, an S4 class object, or a base class, not a <double>.
    Code
      new_class("foo", package = 1)
    Error <simpleError>
      `package` must be a single string
    Code
      new_class("foo", constructor = 1)
    Error <simpleError>
      `constructor` must be a function
    Code
      new_class("foo", constructor = function() { })
    Error <simpleError>
      `constructor` must contain a call to `new_object()`
    Code
      new_class("foo", validator = function() { })
    Error <simpleError>
      `validator` must be function(self), not function()

# R7 classes: can't inherit from S4 or class unions

    Code
      new_class("test", parent = parentS4)
    Error <simpleError>
      `parent` must be an R7 class, S3 class, or base type, not an S4 class.
    Code
      new_class("test", parent = new_union("character"))
    Error <simpleError>
      Can't convert `X[[i]]` to a valid class. Class specification must be an R7 class object, the result of `new_S3_class()`, an S4 class object, or a base class, not a <character>.

# abstract classes: can't be instantiated

    Code
      foo <- new_class("foo", abstract = TRUE)
      foo()
    Error <simpleError>
      Can't construct an object from abstract class <foo>

# abstract classes: can't inherit from concrete class

    Code
      foo1 <- new_class("foo1")
      new_class("foo2", parent = foo1, abstract = TRUE)
    Error <simpleError>
      Abstract classes must have abstract parents

# new_object(): gives useful error if called directly

    Code
      new_object()
    Error <simpleError>
      `new_object()` must be called from within a constructor

# new_object(): validates object

    Code
      foo("x")
    Error <simpleError>
      <foo> object properties are invalid:
      - @x must be <double>, not <character>
    Code
      foo(-1)
    Error <simpleError>
      <foo> object is invalid:
      - x must be positive

# R7 object: displays nicely

    Code
      foo <- new_class("foo", properties = list(x = class_double, y = class_double))
      foo()
    Output
      <foo>
       @ x: num(0) 
       @ y: num(0) 
    Code
      str(list(foo()))
    Output
      List of 1
       $ : <foo>
        ..@ x: num(0) 
        ..@ y: num(0) 

# R7 object: displays objects with data nicely

    Code
      text <- new_class("text", class_character)
      text("x")
    Output
      <text> chr "x"
    Code
      str(list(text("x")))
    Output
      List of 1
       $ : <text> chr "x"

# R7 object: displays list objects nicely

    Code
      foo1(list(x = 1, y = list(a = 21, b = 22)), x = 3, y = list(a = 41, b = 42))
    Output
      <foo1> List of 2
       $ x: num 1
       $ y:List of 2
        ..$ a: num 21
        ..$ b: num 22
       @ x: num 3
       @ y:List of 2
       .. $ a: num 41
       .. $ b: num 42

# c(<R7_class>, ...) gives error

    Code
      c(foo1, foo1)
    Error <simpleError>
      Can not combine R7 class objects

