Alex Brisnehan

CSCI 3155

Integer For-Loops in Python
============================

**Abstract**

This Python proposal initially proposed to make the  iteration over intervals of
integers simpler, by way of lengthening the range of values allowed after the "for" keyword to allow three-way comparisons such as

        for lower <= var < upper:

 to substitute the current way of using the for loop in Python

        for item in list:

syntax.  The resulting for loop starts at the lower bound that is passed in and ends at the upper bound. The original way of building the for loop in Python causes a list to be sometimes unnecessarily built and does not allow for easy iteration most of the time.

Unfortunately, this Python proposal was rejected. The reasons it was rejected was due to a few issues. The biggest reason for implementing the for loop as such is to extend the capability of range() and xrange() in Python (two functions that generate lists and iterators over intervals) to include being an iterator itself. This is done in a roundabout way through this proposal but, in Python 3.0, the range() and xrange() function in Python were extended to include the valuable iteration needed to simplify the for loop, making it more accessible in an already greatly accessible programming language.

**Introduction**

Python is used as a teaching language for specific reason. It's syntax makes it easier to read and understand more than any of the other languages. It's almost like it is written in English instead of an abstract programming language. Although it's glaring issue cannot be fixed too easily(even though we would all love the format heavy language to stop putting so much emphasis on empty spaces), it is possible to add on traits to make it an easier language to use and understand. This proposal does just that. It takes the original implementation of the for-loop and converts it in to something more compact and easier to use. No longer is it needed to build a list in most cases. Given an upper bound and a lower bound, a loop can be run on the sequence in between. I search farther in to the way that the proposal would have been implemented had it been used by Python. Unfortunately though, this proposal would not pick up traction in the language for a few tiny reasons, but the issue it brings up would be later solved in Python 3.0.

**Reasoning**

The reasoning for this proposal was to implement an integer iterative method for the for loop. This exact problem was the root of four other different proposals. Two of the proposals, PEP 212 and PEP 281 both proposed a way to directly convert a list to a sequence of integer indices.

This proposal attacks the problem in a different way. It iterates over a consecutive sequence of integers from one bound to another. This is great in the scenario that the integer values all fall in line consecutively. But in the odd and rare case that you need to use the for loop over a certain variety of things, the original list implementation of the for loop is beneficial. In the case of this proposal, the comparisons at the bounds create a little bit of freedom with the bounds. In the original example,

    for lower <= var < upper

the value "lower" is included in the sequence performed. This means that the sequence starts at lower but runs up until the upper bound, not including the upper bound. The issue presented in this proposal is that iterations can only be done in increments of one or decrements of one. If skipping ahead by three or some other value than one is needed then the 
  
    for item in list

implementation pays off. The resemblance of the two is that they only run while the inner statement is true. As in, 

    for item in list

only runs when 
    
    item in list == true

and 

    for lower <= var < upper

only operates when

    var >= lower && var < upper

This is the basis of the for loop in every language, so an implementation that follows it while also improving on the original form pays off.

**Specification**

The proposal wanted to take the current implementation of the for-loop in Python, 
    
    for_stmt:"for" target_list "in" expression_list ":" suite
    ["else" ":" suite] 

and change it to reflect the comparisons of the integer and its iterations. This looks like 

    for_stmt: "for" for_test ":" suite ["else" ":" suite]
    for_test: target_list "in" expression_list |
        or_expr less_comp or_expr less_comp or_expr |
        or_expr greater_comp or_expr greater_comp or_expr
    less_comp: "<" | "<="
    greater_comp: ">" | ">="

It also changes the syntax of the list comprehensions from

    list_for: "for" expression_list "in" testlist [list_iter]

to a syntax that handles iteration over a sequence of integers that is represented as

    list_for: "for" for_test [list_iter]

The expression formed by the test case would be subject to the same precedence rules as comparisons in expressions as well as the two comparison operators in a test case being required to be of similar types, unlike expressions which do not have those certain restrictions, such as chain comparisons.

We refer to the two or\_expr expressions occurring on the two different sides of the for-loop syntax as the upper bound(right) and lower bound (left) of the loop, and the middle or\_expr as the variable of the loop.  When the new syntax is used for the for-loop during execution, the comparisons for both bounds will be tested, and an iterator will be created that iterates through the sequence of all integers between the two bounds according to the comparison operations used.  The iterator will begin with an integer equal or near to the left bound, and then step through the remaining integers with a step size of positive or negative one if the comparison operation is in the set described by less\_comp 
or greater\_comp respectively. The execution will then continue as if the expression had been

        for variable in iterator

where "variable" refers to the variable of the loop and "iterator" refers to the iterator created for the specific integer sequence.

The values taken by the variable inside of the loop in an integer for-loop can be both normal integers and double integers. The only thing that matters is the value of the bound and its comparison to the variable.  Both of the bounds of the integer for-loop must get watered down to a real numeric type (integer, long, or float).  Any other value will throw a TypeError exception.

**Conclusion**

This proposal was another take on what would be a valuable programming commodity in Python. It is such a valuable commodity that it is to be implemented in Python 3.0. The reason that this said proposal did not get built into the language was because of a few small issues. There were the ones previously mentioned, such as only being able to iterate one at a time, either positively or negatively. In order to traverse through the sequence of integers in a different way, a list comprehension must be formed, like 

    [2*x for 0 <= x <= 100]

Another issue was that it was unclear how the bounds should be evaluated. They could either be evaluated every time through or they could each be evaluated once and stored. If they are evaluated every time through then it makes it to where the loop can change dynamically depending on the current value of the  bounds. the fact that list comprehensions might have to be used, complicates an overly simple task. It also makes it to where it is not possible to access the iterator. The new implementation in Python 3.0 does implement this though, due to it using xrange(), which has always been able to allow access to the iterator. Overall, this new proposal was a step in the right direction. there is no doubt that in Python 3.0, some of the implementations used in this proposal will be used to better the already accessible language of Python.

**Sources**:
Main: http://www.python.org/dev/peps/pep-0284/ (Integer For Loop)
Resources:http://www.python.org/dev/peps/pep-0212/ (Integer For Loop)
http://www.python.org/dev/peps/pep-0281/ (Integer For Loop)
Main:http://www.python.org/dev/peps/pep-3103/ (Switch-Case Statement)
Resources: http://www.python.org/dev/peps/pep-0275/ (Switch-Case Statement)
http://effbot.org/zone/idea-switch.htm (Switch-Case Statement)
