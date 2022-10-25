# Session 3

### Overview

By the end of this session, you will be able to use control statemetns in your PHP Applications.

## WHILE LOOP

In his exercise let's loop through the numbers 1 - 10 using a `while` loop

```php

// syntax for while loop

while(expression) {
    statement 1;
    statement 2;
}

```

```php

// print 1 - 10 using a while loop
$count = 1;
while($count <= 10) {
    print_r($count);
    $count++;
}
```

## FOR LOOP

```php
// for loop syntax
for(expresion1; expression2; expression3) {
    statement 1;
    statement 2;   
}
```

```php
// print 1-10 using a for loop

for($count = 1; $count <= 10; $count ++) {
    print_r($count);
}

```

## EXERCISE

Complete the below code to print all months of a year.

```php
$months = ['January', 'February', 'March', 'April', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

$totalMonths = 12;

for($count = _; $count <= _; $count ++) {
    print_r($months[$count]);
}
```