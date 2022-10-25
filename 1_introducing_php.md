# Session 1

### Overview

By the end of this session, you will be able to work with PHP's built-in templating engine; write simple HTML files; run a PHP script from the command line; create and assign variables to print simple messages on the web browser.

## Introducing PHP

## PHP's Templating Engine

Open up app/1_introducing_php.php

You should see a section in the HTML document as follows.

```php
<?php
    // your code here
?>
```

You can write php code by enclosing it in `<?php ?>`

Let's make our Application say Hi to the user.

Add the following code to the php template.

```php
<?php
echo "Hello World";
?>
```
Hit Save and head to [https://localhost:8081](https://localhost:8081)

## Using Variables In PHP

All variables in php start  with **$**. 

```php
<?php
$favoriteFruit = 'apple';
?>
```

Let's alter your PHP Application to print your name with a variable.

```php
<?php
$userName = 'Rajmohan Kamath';
echo 'Welcome back $userName !';
?>
```
Hit Save and head to [https://localhost:8081](https://localhost:8081)

**Variables can also store other data types such as**

- string
- int
- float
- boolean

However you don't need to specify the type of the variable while declaring it. Unlike other languages. This is because PHP is a ***dynamically typed language***. It understands the type of a variable based on the value assigned to it at runtime.

```php
<?php
$name = 'Rajmohan';
$age = 23;
$bankBalance = 200000099.04
?>
```

You don't need to tell PHP what the type of variable should be. It can figure it out based on the value you give it.

## Before you move ahead

 Let's make our Application look a little pretty.

```html
<link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">
```

Add this to the head section of your HTML file and reload the page.