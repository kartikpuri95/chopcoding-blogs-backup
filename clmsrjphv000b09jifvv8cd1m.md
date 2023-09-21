---
title: "Python Functions: Writing Better Code with Ease"
datePublished: Thu Sep 21 2023 05:59:39 GMT+0000 (Coordinated Universal Time)
cuid: clmsrjphv000b09jifvv8cd1m
slug: python-functions-writing-better-code-with-ease
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1695275682287/547bc47d-d944-4510-9496-29e935156523.png
tags: python, python-beginner, python-projects

---

### **Introduction**

Programming is all about functions. They help you organize and reuse code. Python is a popular programming language that has a lot of tools for working with functions. In this guide, we're going to explore Python functions from the basics to advanced techniques. We'll give you some tips to make it more interesting!

## **Python Function Fundamentals**

### **How to define a function?**

In the Python programming language, functions are defined using the def keyword. As an example, we have already demonstrated a simple case:

```python
import random

def animal_sound():
    sounds = ["Meow", "Woof", "Moo", "Quack", "Oink"]
    return random.choice(sounds)
```

This article explores the topic of defining functions in Python, including parameters and return values. The code sample demonstrates the use of the 'def' keyword to define a function called 'sum()', and the keyword 'return' is used to indicate that the function returns a value.

### Parameterized Functions

Function parameters in Python are like these little placeholders that you can use to pass data into a function. They're really helpful for making your functions more versatile and customizable. Here's how to use them!

```python
def birthday_wish(name, age):
    return f"🎉 Happy {age} Birthday, {name}! May your day be filled with joy and your year with adventures! 🎂🎈"

message = birthday_wish("Chop", 25)
print(message)  # 🎉 Happy 25th Birthday, Emma! May your day be filled with joy and your year with adventures! 🎂🎈
```

The `birthday_wish` function takes two parameters: `name` and `age`, It then combines these parameters to create a personalized birthday wish.

Now, when you call `birthday_wish("Chop", "25th")`, the first argument "Chop" is assigned to `name`, and the second argument "25th" is assigned to `age`, resulting in a customized birthday message.

###   
Flexible function **Parameters with Defaults**

In Python, you have the flexibility to specify default values for function parameters. This feature makes parameters optional when calling the function. If no value is provided for an optional parameter, it automatically adopts the default value.

```python
def order_pizza(pizza_size, toppings=[]):
    pizza = {"size": pizza_size, "toppings": toppings}
    return pizza
```

In this example, the `order_pizza` function takes two parameters: `pizza_size` and `toppings`. The `toppings` parameter has a default value of an empty list, indicating that if no toppings are specified, the pizza will be ordered plain.

Now, let's see how this function is used:

```python
# Ordering a plain pizza with the default toppings
plain_pizza = order_pizza("large")
print(plain_pizza)

# Ordering a customized pizza with extra toppings
custom_pizza = order_pizza("medium", ["pepperoni", "mushrooms"])
print(custom_pizza)
```

In the first call to `order_pizza("large")`, no toppings are specified, so the function defaults to ordering a plain pizza with an empty toppings list. In the second call to `order_pizza("medium", ["pepperoni", "mushrooms"])`, custom toppings are provided, resulting in a customized pizza order.

This example showcases how default parameters can make a function more versatile by allowing users to provide additional information when needed while providing sensible default behaviour when information is missing.

### **Keyword Arguments in Python Functions**

Python provides a powerful feature known as keyword arguments that allows you to pass arguments to a function by specifying parameter names as keywords. This feature not only enhances code readability but also offers flexibility when dealing with functions that have multiple parameters.

Consider the following example:

```python
def order_pizza(pizza_size, toppings=[]):
    pizza = {"size": pizza_size, "toppings": toppings}
    return pizza
```

In this function, `order_pizza`, we have two parameters: `pizza_size` and `toppings`. The `toppings` parameter has a default value of an empty list.

Now, let's explore how we can use keyword arguments to make pizza orders more explicit and clear:

```python
# Ordering a plain pizza with the default toppings using keyword arguments
plain_pizza = order_pizza(pizza_size="large")
print(plain_pizza)

# Ordering a customized pizza with extra toppings using keyword arguments
custom_pizza = order_pizza(pizza_size="medium", toppings=["pepperoni", "mushrooms"])
print(custom_pizza)
```

By using keyword arguments, we explicitly specify which value corresponds to each parameter when calling the `order_pizza` function. This not only improves code clarity but also allows us to provide arguments out of order if needed.

Keyword arguments are especially valuable when dealing with functions that have many parameters or when you want to make it crystal clear which argument corresponds to which parameter. They offer a powerful way to write more readable and maintainable code in Python.

### **Variable-Length Argument Lists in Python Functions**

In some situations, you might encounter scenarios where you don't know in advance how many arguments will be passed to a function. Python provides a flexible solution for such cases using the `*args` and `**kwargs` syntax.

* `*args` collects positional arguments into a tuple.
    
* `**kwargs` collects keyword arguments into a dictionary.
    

Let's illustrate this with an example:

```python
def print_args(*args, **kwargs):
    print("Positional arguments:", args)
    print("Keyword arguments:", kwargs)

print_args(1, 2, 3, a="apple", b="banana")
```

In this example, the `print_args` function accepts both positional arguments (1, 2, 3) and keyword arguments (a="apple", b="banana"). The `*args` notation gathers the positional arguments into a tuple, and `**kwargs` collects the keyword arguments into a dictionary. When you run this function, you get the following output:

```python
Positional arguments: (1, 2, 3)
Keyword arguments: {'a': 'apple', 'b': 'banana'}
```

This flexibility is incredibly valuable when you want to create functions that can handle different numbers of arguments without having to define a fixed set of parameters. It allows you to design more versatile and adaptable functions.

Consider a simple function like `sum`:

```python
def sum(x, y):
    return x + y
```

In this case, `a` and `b` are parameters, and you pass specific values to the function. However, in scenarios where you need to handle varying numbers of arguments, `*args` and `**kwargs` become indispensable tools in your Python programming arsenal.

## Conclusion

In the world of Python programming, functions are like the Swiss Army knives of your code. They allow you to encapsulate logic, promote reusability, and make your code more organized and readable. In this article, we've explored the fundamental concepts of Python functions and their various features, showcasing their versatility through engaging examples.

You can connect me on :

💼 Linkedin: [https://www.linkedin.com/in/kartikchop/](https://www.linkedin.com/in/kartikchop/)

🐦 Twitter: [https://twitter.com/chopcoding](https://twitter.com/chopcoding)