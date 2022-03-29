# Modules

## Why Modules
- In Ruby, a class can only directly inherit from a single superclass: **single inheritance**. This constraint made certain problem domains (where subclass have traits from more than one superclass) difficult to model.
- It is also common for some behaviors to be applicable for a subset of subclasses (e.g. `swim` is only applicable for `Dog` and `Fish` classes in the example below). There is no neat way to place this behaviour without duplicating code.
![](https://d1b1wr57ag5rdp.cloudfront.net/images/oop/lesson2/module_class_hierarchy.png)
- In Ruby, the use of modules can help address both problems:
	- Each class can mixin as many modules as required
	- We can house behaviours that do not fit in nicely in class hierarchy tree into a module, then mixin that module in classes that need them. Code is then housed in one place and we do not need to repeat ourselves.
	```ruby
	module Swim
	  def swim
		"swimming!"
	  end
	end

	class Dog
	  include Swim
	  # ... rest of class omitted
	end

	class Fish
	  include Swim
	  # ... rest of class omitted
	end
	```

## Modules and Method Lookup Path
Mixin modules affects the [method lookup path](inheritance.md#method-lookup-path).


- We use `::ancestor` class method to check the method lookup path of a class


## Practice Questions
**Question 1**\
Which of the following are differences between modules and classes? Select all that apply.
- [x] **A** Classes let you instantiate objects whereas modules don't.
- [x] **B** Modules are often used for namespacing.
- [x] **C** You can sub-class from precisely one parent class, but you can mix in as many modules as you need.
- [ ] **D** Classes may contain more than one method, but modules must have precisely one method.

**D** is incorrect because Modules can contain more than one method.
<br>


**Question 2**\
![Class hierarchy diagram](https://d1b1wr57ag5rdp.cloudfront.net/images/quizzes/restaurant-hierarchy.png "Restaurant Staff Class Hierarchy")\
One reason to use modules in Ruby is to share common behavior between classes when you can't share them via class inheritance. Given this context, which of the following methods would it be appropriate to extract to a module? Select all that apply.
- [ ] **A** `cook`
- [x] **B** `supervise`
- [x] **C** `speak_to_customer`
- [ ] **D** `take_food_order`

**Explanations**\
**A**: Although the `cook` method occurs in two classes, `Chef` and `PastryChef`, we can see that `PastryChef` inherits from `Chef`, so we don't need to extract `#cook` to a module. We can remove the method from `PastryChef` if it's identical to the `Chef` version; if they are different, then `PastryChef#cook` overrides `Chef#cook`.

**D**: The `take_food_order` method only occurs in one class, `Waiter`. Although you might want to extract this method to a module for other reasons, such as cleaning up the class structure, you don't need to do it for sharing purposes.


**Question 3**
You're designing a Recipe Book application. You've identified some classes that you need:

-   `RecipeBook`
-   `Recipe`
-   `StarterRecipe`
-   `MainCourseRecipe`
-   `DessertRecipe`
-   `Ingredient`

Each Recipe Book has one or more recipes. Starter recipes, main course recipes, and dessert recipes are all recipe types and share some characteristics but not others. Recipes have one or more ingredients.

You decide that the application also needs a `Conversion` module that contains some temperature and measurement conversion methods required by `Recipe` and `Ingredient` objects.


1. What kind of Object Oriented relationship should exist between `RecipeBook` and `MainCourseRecipe`?
- Collaboration

2. What kind of Object Oriented relationship should exist between `Ingredient` and `Conversion`?
- Mixin

3. What kind of Object Oriented relationship should exist between `Recipe` and `DessertRecipe`?
- Inheritance

4. What kind of Object Oriented relationship should exist between `RecipeBook` and `Ingredient`?
- None




