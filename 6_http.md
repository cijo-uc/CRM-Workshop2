# Session 6

### Overview
By the end of this session you will be able to build dynamic web applications using PHP and end data across multiple pages in your application.


## Request Response Lifecycle of PHP

tk: What is the http lifecycle. How does a page from the browser get loaded?

## Requests over HTTP

A request has at least 3 parts.

1. The URL of the request. 
2. They method of the request.
3. The HEADERS of the request.

https://www.unicourt.com

When you type this in your browser you can view the request in the insepct section to see what type of request is being made.

### Request Methods:

1. GET 
2. POST
3. PUT
4. PATCH
5. DELETE

#### GET

create a form in your index.php

```html
<form action = '/pets.php' method='GET'>
    <input type = "text" name = "id"/><br/>
    <input type = "submit" value = "GET PET"/><br/>
</form>
```

notice the browser URL change when you click on submit.

let's create a new file called pets.php in your app folder.

```php

$pets = ['1' => 'sally', '2' => 'milo', '3' => 'jasper'];
$id = $_GET['id'];

if (isset($id)) {
    echo "You asked for {$pets[$id]}";
}
```

#### POST

A POST request is generally used to POST data to a web page.

Let's use it to insert our own pet.

Create a new page in your app folder called new.php

```html
<h1> Insert New Pet </h1>
<!--request type is now POST-->
<form action = '/newPet.php' method = 'POST'>
    <input type = 'text' name = 'name' /><br/>
    <select name = 'type'>
        <option value = 'Dog'>Dog</option>
        <option value = 'Cat'>Cat</option> 
    </select>
    <input type = "submit" value = "submit"/><br/>
</form>
```

Create a new file called newPet.php

```php

if(!isset($_POST['name'])) {
    echo "ERROR! You need to submit a name for your Pet.";
    exit;
}
if (!isset($_POST['type']) || !in_array($_POST['type'], ['Cat', 'Dog'])) {
    echo "ERROR! Please enter a Valid Pet Type";
    exit;
}

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

switch($_POST['type']) {
    case 'Cat':
        $cat = new Cat();
        $cat->setName($_POST['name']);
        $cat->sayHi();
        break;

    case 'Dog': 
        $dog = new Dog();
        $dog->setName($_POST['name']);
        $dog->sayHi();
        break;

    default:
        echo 'Unknown error occurred :(';
}
```

TRY OUT YOUR APPLICATION. Don't forget to make it feel good while you're at it.