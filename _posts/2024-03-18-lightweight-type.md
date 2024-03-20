---
layout: post
title:  "Lightweight Type Design"
date:   2024-03-18 11:58:32 +0800
categories: style
---

Here is a paragraph quoted from [PEP 483](https://peps.python.org/pep-0483/):

> There are many definitions of the concept of type in the literature.
> Here we assume that type is a set of values and a set of functions that one can apply to these values.

This is the most concise definition of _type_ yet makes sense for me as far as I have seen,
and my opinion in this blog is that
the design of types in a program should be that simple if it doesn't have to be complex.

Modern programming languages have developed sophisticated grammar and mechanism to
help their users to handle types. These tools usually have two goals:

1. if there are types similar to the type that you are going to define already, you should be
   able to make use of those existing types to ease your work;
2. if there are several types that share some common features, you should be able to eliminate
   replicated code that deal with those common features.

The envisioned output from these goals is promising, and as programmers, who have a straightforward
aesthetic feeling for the way that the abstract concepts is represented in our code,
we find it beautiful: a grand hierarchy of concepts is about to be built if we adopt
the methodology imposed by these modern programming languages.
However, regardless of how much effort we have put on the design of types, the code we write is
never approaching the truth by the strictest means. We always view the concepts from
different perspectives, and only take the meaningful part for us to our code. That's a reason 
that I don't like the abstraction from object-oriented programming, which inherently
allows only one view of concepts.

So we should always take the design of types practically, focusing on the engineering value
that a programming language's type system deliver to us.

Let's think a bit of the concepts that be dealt by the most jobs in the world.
You have to confess that these jobs, they are not some projects
used by so many people like frameworks or libraries, their main purpose is simply to implement
some bussiness logic for employers. Bussiness logic, as we all know, rarely be complex in type design.
So everytime I start a new project, I will ask myself:
__Does this project involve a lot of coherent concepts so I should use a complicated type system?__
Most of the time, the answer is no. So why should I write all these code to support the mechanisms that
I don't even use. Writing these code is not a piece of cake;
defining a class is not trivial if you want your project to live longer.

These thinking leads to the core problem that this blog manage to solve:
_How can I minimize the effort to solidify the concepts in types?_

Now go back to the quotation at beginning. It seems like that I must figure out the meaning of
_value_ and _function_ first.
The following two sections are my understanding of them
with a classic example that arises frequently in programming textbooks: building a system
to manage employees.

# Value

To manage employees, we must identify them in the system that we are going to build.
This identification for a program is chunks of bits, so comes the word, _value_.
A set of values is the representation of a set of employees in the program, so the set
of values must following some same principles of the set of employees.
One of such principles is that if we pick up two employees from the set
we must be able to differentiate them, so is the values. That means, for any value _a_
and _b_ in the same set, statement _a is equal to b_ is defined.

This is not as simple as it seems, especially in modern programming languages,
but the topic here is about simplifying. I will show my way of defining equivalence
in several programming languages.

Here is some guidelines (the importance is from high to low):

1. do not define my own equivalence, use existing language mechanisms,
2. prefer meaningful information to memory address,
3. use hash if meaningful information takes too much memory,
4. take care of computing efficiency.

To make the proposals more concrete, let's try to map employees to values in C. 
Following the first guideline, we can choose integer, float, array, struct, pointer.
Second, if the company you work for attached an employee ID to everry employee,
that ID might be a good choice, but this is not always the case.
Let's assume that name, age, sex can be combined to identify an employee.
So struct is the natural choice here. Third, my definition of _taking too much memory_ is
that the value can be passed to functions only by reference rather than also by value.
The struct we created is _taking too much memory_. To comply with the third, we compute
a hash value from the struct, and use that hash value as value mapping to employee.
We havn't put these values in computing, so the fouth guideline will be talked in the last
section.

# Function

__Functions are components of the statements that can be made for the concepts.__

For example, from the statement _Employee X is named Chu_ we can factor out _is named Chu_ as
a function of one argument employee and returning a boolean, or we can factor out _is named_,
which has two arguments, employee and name, but has same returned value with _is named Chu_.
The statement can be modified to _employee X's name is Chu_, now we have _'s name_, which
is a function of one argument employee and returning a name.

So the key point is finding out the set of statements you can say about the concepts;
from the statements we derive the functions.
It's a simple and clear idea, but when it comes to engineering, a lot of technical details
emerges.

Think about the implementation of function _'s name_. We can not get any useful information
from the hash value, so we must store the mapping from employees to names for the function.
A good way to do this is using hash table (hash again!). It works well, but what if we have
to modify the set of employees, for example, the company employed a new person.

In the traditional way, we allocate a chunk of memory to save all information we need for the employee
that we gonna put under consideration; the value for the employee in that case is usually a pointer, 
and _'s name_ is a dereferencing of a pointer, it even doesn't have to be a C function.
I will give my reasons of not using pointers for values, but just continue the implementaion for now.
The hash table created in the previous paragraph serves only for a single
function, so a centralized memory management is not desirable here.
A seperated method should be used to modify that hash table. Because the name of employee is part
of the value of employee, we can bind this method with the computing of hash value.
I use the term _method_ even it doesn't differ from function for the form in a C program;
they are both implemented as C functions.
The methods, which serve for the changing of semantics, should be treated differently with functions.

There is a workflow to implement functions:

1. determine the set of statements used in the project,
2. factor out functions from the statements selected,
3. determine the set of statements whose semantics can be modified,
4. implement the methods to modify semantics.

One more example shows the whole workflow; I will add the support for statement _employee X's salary is N_.
First, factor out the function _'s salary_, the implementation is similar to _'s name_;
second, this statement has different semantics at different time points,
one's salary will increase or decrease; last, implement the method to modify function _'s salary_'s
hash table.

# Practice

The ingredients are all prepared; let's go ahead to see what this style can bring to us in work.
I list a few here, more might be added to this list in future.

_Reference concepts by literal values_. Memory address, pointer in C, is usually a runtime concept
(except RTOS embedded projects or OS kernel development); you may get different values for a same concept
among several excutions of a program, so you have to compute the value even the value is constant.
Consider the implementation for function _has same salary with employee X_.
There's a constant _X_. If you use pointers, you have to implement the function that maps name, age, sex to
employee, then compare the argument with the _X_ through function _'s salary_. Have you noticed?
This process is basically getting the hash value from meaningful information, which is what we do
when attempting to use meaningful information to represent concepts; why not doing it at build time?
There is a lot of programming languages supporting build time computing, C++, Zig, etc; it's
easy to get the same effect in C programs too.

_Add support for statements linearly_. As the example of adding support for statement _employee X's salary is N_
shows, the new concept _salary_ and new function _'s salary_ is added to the program without modifying
exsisting code even one line. If we were using a centralized memory management, we at least had to
modify the struct to get enough memory.

_Support polymorphism for free_. Employees might differ in some minor ways; here's an example,
statement _employee X's car's brand is B_, this statement is totally meaningless to
employees who doesn't own a car. It's not practical to define a totally new type
for employee who owns a car, and if you want to use type system for such a trivial variation
the code will soon become unmaintainable. The solution given here is that, because the statement's
semantic is managed mainly by function _'s car_, we can safely instruct _'s car_ to throw out
an exception if the employee doesn't own a car, so that no new type is needed and no security flaw is introduced.
The difference in the sets of applicable functions implies there is actually a new type,
to borrow the term from [PEP 483](https://peps.python.org/pep-0483/), employee who owns a car
is a subtype of employee. This approach is similar to the dynamically typed languages.

# At last

I will continue to practice this style and improve it in the future. More experience on applying
it to other programming languages might be shared in future posts.
