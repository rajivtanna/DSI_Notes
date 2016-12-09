background info:
A decorator is a function (function_1) which can be added to the
definition of another function (function_2) using a syntax we saw before:

Useful Tutorial: http://thecodeship.com/patterns/guide-to-python-function-decorators/

Examples
```
def function_1(calling_function):
    def clean(string):
        # We clean the string here
        cleaned_string = string.replace("dirty", "clean")
        # we call the original function (in this example we call)
        calling_function(cleaned_string)
    return clean

@function_1
def function_2(my_string):
    print(my_string)
>> function_2("I'm dirty")
output is "I'm clean"




def tell_me_more(double):
    def inner(x):
        return "Hey! The answer you wanted was " + str(double(x * 3))
    return inner

@tell_me_more
def double(x):
    return x * 2
double(2)
