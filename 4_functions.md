# Session 4

### Overview

By the end of this Session you will be able to create your own functions and use built-in php functions.

## What's a function

tk: What's a function

## Creating your own function


You can create your function using the following syntax

```php
function sayHi() {
    echo 'Hi';
}
```

You can make this function run by calling it as follows.

```php
sayHi();
```

Functions can also receive arguments and use them. For example,

```php

function sayHiToUser($userName) {
    echo "Hi {$userName}";
}

sayHiToUser('Raj');
```

## Built-In PHP Functions You can use

|Use Case|Function Name|
|---|---|
|Cut a String|```substr(string string, int start[, int length] )```|
|Find the length of a string|```strlen()```|
|Find and Replace in String|```str_replace()```|
|Trim any whitespaces in the start or end of a string|```trim()```|
|Get the position of the first occurance of a string inside a string|```strpos()```|
|Check if a variable is a string|```is_string()```|
|Check if a variable is an array|```is_array()```|
|Check if a value is in an array|```in_array()```|
|Merge 2 Arrays|```array_merge()```|
|Get the Keys of an Array|```array_keys()```|
|Check if a Key exists in an Array|```array_key_exists()```|
|Insert a value in an array|```array_push()```|
|Delete an element from an Array|```array_pop()```|
|Prints the values in an array|```array_values()```|
|Removes duplicate items from an array|```array_unique()```|
|Count the number of elements in a list|```count()```|
|check if a value is empty|```empty()```|
|check if a value is set|```isset()```|
|check if a value is null|```isnull()```|
