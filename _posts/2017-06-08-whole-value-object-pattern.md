---
layout:     post
title:      Whole Value Object Pattern
date:       2017-06-08 11:21:29
summary:    This is an empty post to illustrate the pagination component with Pixyll.
categories: programming
---

One of the most common pitfalls in designing your objects is to use primitive types (integers or strings) to represent domain concepts. This is a documented code smell and it’s called Primitive Obsession.

In that app, I am working on; we needed to store the concept of a specific month, like March 2017 or April 2016. This was used to perform some validations and other calculations. I had stored this value as an integer in YYYYMM format. This way it was unique and easy to compare with each other.

The problem with that approach was an increase in complexity and scattering of related logic all over the place. In every place, I used this concept I had a bunch of extra methods creeping up; a method to display the name of the month (e.g. January); methods to calculate previous and next months; methods to check if the given integer is a valid month.

~~~ ruby	
def month_name(month)
  ...
end
 
def next_month(month)
  ...
end
 
def previous_month(month)
  ...
end
 
def valid_month?(month)
 ...
end
~~~

Methods like these and more would creep up quickly in different places. Now I am smart and I know I can place these methods in a module and DRY up the code. But this code still didn’t feel right to me. The naming is awkward and all the methods take the same argument. It was time to put functionality with the data it was acting on. So, I made a new class called CalendarMonth.

~~~ ruby	
class CalendarMonth
  include Comparable
  attr_reader :value
 
  def initialize(input_month = nil)
    @value = convert_input(input_month)
    raise ArgumentError unless valid?
    freeze
  end
 
  ...
 
  def month_name
    ...
  end
 
  def next_month
    ...
  end
 
  def previous_month
    ...
  end
 
  def (other)
    ...
  end
 
  ...
 
private
  def valid?
    ...
  end
 
  ...
 
end
~~~

CalendarMonth objects contain all the functionality relating to the concept. Another benefit was that this class now acts like a magnet for more related functionality. if in future I need anything related I know where to put it. Whereas, before, anytime I had any new logic it was in the place I needed making the complexity of the app skyrocket.

Another big advantage is how easier it is to test this class. It is immutable and has no outside dependencies I need to stub. Tests will be concise and faster to execute.

This closely resembles the Whole Value pattern. Although in most cases I found while reading this pattern up, it was used for quantities with units. As I understand, Whole Value or Whole Object pattern describes transforming primitive values to domain-specific value objects.

I think this is a very low-hanging fruit in refactoring, especially because often we think of smaller domain concepts through the lens of underlying database columns. Anytime you have a domain concept that is represented using primitives, see if you can improve the code by modeling it properly and promoting it to be a first-class citizen.
