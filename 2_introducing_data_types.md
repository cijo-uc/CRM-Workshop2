# Session 2

### Overview

By the end of this chapter, you will be able to use the different data types in PHP to store and work with data; create and use arrays; work with operators to evaluate the values of operations; and perform type casting to convert variables from one type to another.

## Evaluating Varaibles In PHP

Open up [app/2_introducing_data_types.php](app/2_introducing_data_types.php)


Let's write a short program to calculate how much Cashback a customer earned based on the following rules.

1. Bill Amount < 100 give 0% Cashback.
2. Bill Amount > 100 and <  1000 give 2% Cashback.
3. Bill Amount >= 1000 give 5% Cashback.

```php
<?php
$billAmount = 15000;
$cashBack = 0;

if ($billAmount < 100) {
    $cashBack = 0;
} else if ($billAmount < 1000) {
    $cashBack  = 0.02 * $billAmount;
} else {
    $cashBack = 0.05 * $billAmount;
}

echo "Total Cashback earned: {$cashBack}";
?>
```

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

## Arrays

tk: What is an Array

Here's how you can declare an Array in PHP

```php
<?php
$fruits = ['apple', 'orange', 'pear'];
print_r($fruits);
?>
```

**Inserting new element in array**

```php
<?php
$fruits = ['apple', 'orange', 'pear'];
array_push($fruits, 'banana');
print_r($fruits);
?>
```

**Deleting an element in array**

use `unset` or `array_pop`

```php
<?php
$fruits = ['apple', 'orange', 'pear'];
unset($fruits[2]);
print_r($fruits);
?>
```

**Associative Arrays**

Suppose you need to store the price of each fruit as well.

You can use associative arrays for this.

```php
<?php
$fruit_prices = ['apple' => 40, 'orange' => 60, 'pear' => 30];
$fruit = 'apple';
echo "Price of {$fruit} is {$fruit_prices['apple']}";
?>
```

To loop through each fruit and print its price you can also use the foreach loop.

```php
<?php
foreach($fruit_prices as $fruit => $price) {
    echo "Price of $fruit is {$price}";
}
?>
```