# Collaborator Objects

**Objects stored as states**, i.e. assigned to instance variables, **within another objects** are known as **"collaborator objects"**

These objects are called collaborators because they **work in conjunction (collaborate) with the class they are associated with**. The methods of these collaborator objects are available for use to perform required actions

Collaborator objects can be:
- **Custom**: objects of classes that are user defined and not part of the standard Ruby library
- **Non-custom**: objects of standard Ruby classes such as `String`, `Integer`, `Array` and `Hash`

Collaborators objects allow us to chop up and modularize the problem domain into cohesive pieces and play an important role in modelling complicated problem domain.


**Example**
```ruby
class Person
  def initialize(name)
    @name = name
  end
end

class Cat
  def initialize(name, owner)
    @name = name
    @owner = owner
  end
end

sara = Person.new("Sara")
fluffy = Cat.new("Fluffy", sara)
```
Identify all **custom defined objects** that act as collaborator objects within the code. Select all that apply.
1. The `Person` object referenced by the variable `sara`
2. `"Sara"`
3. The `Cat` object referenced by the variable `fluffy`
4. `"Fluffy"`

Answer:
Only **1** is correct
	
- "Sara" is technically a collaborator object, but not a custom one. It is a String that comes with the Ruby core library. 						   
- The `Cat` object referenced by the variable `fluffy` is **not assigned to an instance variable** in another object, and so does not act as a collaborator object.                                
- "Fluffy", like "Sara", is a string instance and not a custom object that has been defined in the code above.                                    


