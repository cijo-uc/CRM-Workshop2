# Session 5

### Overview

By the end of this Session you will be able to introduce Object Oriented Programming in your application. Declare Classes and write methods for them.

## What you should know

1. What are classes
2. What are objects
3. What are functions
4. What is inheritance

## Declaring a Class

```php

class ClassName
{
    // body
}
```

**Declaring a class with a method**


```php
class Person
{
    public $name = 'Raj';
    function sayHi() {
        echo "Hi $this->name";
    }
}

$object = new Person(); // the `new` keyword creates an Object
$object->sayHi(); // -> operator is used to access an object's functions

```

You can use the `$this` keyword to access the current object. In our example above we tried accessing the $name variable of the current Object.

We can also use it to call other methods in the same class. `$this->methodName()`

```php

class Person
{
    public $name = 'Raj';

    public function setName(string $name): void {
        $this->name = $name;
    }

    public function getName(): string {
        return $this->name;
    }

    public function sayHi() {
        echo "Hi {$this->getName()}";
    }
}

$raj = new Person();
$raj->setName('Raj Mohan');
$raj->sayHi();
```

**Constructor**

A special method that is called while creating an object. 

```php

class Person {
    public $name;
    
    function __construct($name) {
        $this->name = $name;
    }

    function sayHi() {
        echo "Hi {$this->name}";
    }
}

$raj = new Person('Raj');
$raj->sayHi();
```

## Inheritance

tk: what is inheritance and why do we use it

You can inherit a parent's methods by using `extends` keyword in PHP.

```php

class Pet{
    public $name;

    public function setName($name): void {
        $this->name = $name;
    }

    public function getName(): string {
        return $this->name;
    }
    public function sayHi() {
        echo "Hi {$this->getName()}";
    }
}

class Dog extends Pet {
    const BREED = 'German Shepard';

    public function getBreed() {
        return Dog::BREED;
    }
}

class Cat extends Pet {
    const TYPE = 'Persian'; 
    public function getType() {
        return Cat::TYPE;
    }
}

$dog = new Dog();
$dog->setName('Alex'); // you can reuse common methods across classes through
$dog->sayHi();
echo $dog->getBreed(); //each child class can have their own custom methods

$cat = new Cat();
$cat->setName('Garfield');
$cat->sayHi();
echo $cat->getType();
?>