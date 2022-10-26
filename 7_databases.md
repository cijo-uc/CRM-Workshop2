# Session 7

### Overview

By the end of this session you will be able to write real world applications that can persist data onto a database.

## Before You Proceed

Create necessary Database.

**Create the Database for the exercise.**

```sql
CREATE DATABASE pet_store;
use pet_store;
```

**Create the tables required for the database**
```sql
CREATE TABLE `pet` (
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR(100) NOT NULL,
	species VARCHAR(10) NOT NULL DEFAULT 'DOG',
	description TEXT,
	price FLOAT NOT NULL,
	PRIMARY KEY (`id`)
) ENGINE=InnoDB;
```

## Connecting to MYSQL in PHP

You can connect t MySQL by using the following command.

```php
new mysqli(host, username, password, databaseName);
```

Let's create a Class we can keep reusing to handle our DB Connection. 

In app/ folder create a file called db.php

```php

class DBConnector{
    public $conn;
    
    public $servername; 
    public $username;
    public $password; 
    public $db_name;        

    function __construct(){ 
        try{
            $this->servername = getenv("MYSQL_SERVER");
            $this->username = getenv("MYSQL_USER");
            $this->password = getenv("MYSQL_ROOT_PASSWORD");
            $this->db_name = getenv("MYSQL_DATABASE");
            $this->conn = new mysqli($this->servername,$this->username,$this->password,$this->db_name);
        }     
        catch(Exception $e){
            echo "<script> window.alert('Error in database connection!');</script>";
        }
    } 

    function run_queries($sql){
        return $this->conn->query($sql);
    }

    function getConnection() {
        return $this->conn;
    }
}
```

## BUILDING OUR PET STORE

Let's create a Pet Class that will be able to handle everything that needs to happen to a pet.

Create a folder called `entities` in your project folder. And add the pet.php file there.


```php

<?php

class Pet
{
    protected $id; // id of the PET in the DB.
    protected $name;
    protected $price;
    protected $description;

    public function setId($id): void
    {
        $this->id = $id;
    }

    public function getId(): int
    {
        return $this->id;
    }

    public function setName($name): void
    {
        $this->name = $name;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setPrice($price): void
    {
        $this->price = $price;
    }

    public function getPrice(): int
    {
        return $this->price;
    }

    public function setDescription(string $description): void
    {
        $this->description = $description;
    }

    public function getDescription(): string
    {
        return $this->description;
    }


    public function getSpecies(): string
    {
        return $this->species;
    }
}

class Dog extends Pet
{
    public $species = 'DOG';
}

class Cat extends Pet
{
    public $species = 'CAT';
}
```

# INSERT A NEW PET

Let's add a method to insert the Pet into DB.

```php
class Pet {
    // ...methods

    public function insertToDB($connection): void
    {
        $sql = "INSERT INTO `pet` (name,species,description, price) VALUES (?, ?, ?, ?)";
        $stmt = $connection->prepare($sql);
        $stmt->bind_param("sssd", $this->getName(), $this->getSpecies(), $this->getDescription(), $this->getPrice());
        $result = $stmt->execute();

        var_dump($result);
        if ($result) {
            echo "<script> window.alert('Successfully inserted!'); window.location.href='/index.php';</script>";
        } else {
            throw new Exception("oops! pet table does not exist");
        }

        $stmt->close();
        $connection->close();
    }
}
```
Let's create form for people to enter Pet details.

Create a folder called `views` we will add all our forms here.

Create a file called create.php

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Insert New Pet</title>
    <link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">
</head>

<body>
    <h1>Insert New Pet</h1>
    <form action="/controllers/create.php" method="post">
        <label for="name">Name: </label><input type="text" name="name" id="name" required><br>
        <label for="species" required>Species: </label>
        <select name="species" id="species">
            <option value="CAT">Cat</option>
            <option value="DOG">Dog</option>
        </select><br>
        <label for="price">Price: </label>
        <input type="number" name="price" id="price" required><br>
        <label for="description">Description: </label>
        <textarea name="description" id="description" cols="30" rows="5" required></textarea><br>
        <input type="submit" value="Create">
        <input type="reset" value="Clear">
    </form>
</body>

</html>

```

THe form above posts to a php file in controllers folder. Let's create that file.

```php
<?php

include_once '../db.php';
include_once '../entities/pet.php';


if (!isset($_POST['name']) || !isset($_POST['species']) || !isset($_POST['price']) || !isset($_POST['description'])) {
    echo "<p>ERROR! name, species, price and description are required while creating new Pet Records.</p>";
    echo "<a href = '/views/create.php'>Go back</a>";
    exit();
}

// now create a new Pet

$connector = new DBConnector();
$conn = $connector->getConnection();

switch ($_POST['species']) {
    case 'DOG':
        $dog = new Dog();
        $dog->setName($_POST['name']);
        $dog->setPrice((float) $_POST['price']);
        $dog->setDescription($_POST['description']);
        $dog->insertToDB($conn);
        header('Location: /');
        break;

    case 'CAT':
        $cat = new Cat();
        $cat->setName($_POST['name']);
        $cat->setPrice((float)$_POST['price']);
        $cat->setDescription($_POST['description']);
        $cat->insertToDB($conn);
        header('Location: /');
        break;

    default:
        echo $_POST['species'];
        echo "<p>ERROR! Unkown Species provided.</p>";
        echo "<a href = '/views/create.php'>Go back</a>";
        exit();
}


```

# GET ALL PETS AND SHOW THEM ON THE HOME PAGE

Let's add a method to our Pet class to retrieve all pets.

```php

class Pet {
    // ...methods

    public function getAllPetsFromDB($connection)
    {
        $sql = "SELECT * FROM pet";
        $result =  $connection->query($sql);
        return $result;
    }
}
```

Let's call this from the home page and display the results.

modify index.php to be the following.


```php

<?php
include 'db.php';
include 'entities/pet.php';
?>
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">
</head>

<body>
    <h1>Welcome to PetStore</h1>
    <p><a href="views/create.php">Create Pet</a></p>

    <?php
    $dbConnector = new DBConnector();
    $pet = new Pet();
    $result = $pet->getAllPetsFromDB($dbConnector->getConnection());

    if ($result != null) {
        while ($row = $result->fetch_assoc()) {
    ?>
            <div id="pet">
                <h2><?php echo $row['name'] ?></h2>
                <p><b>Species</b>: <?php echo $row['species'] ?></p>
                <p><b>Price</b>: $<?php echo $row['price'] ?></p>
                <p><b>Description</b>: <?php echo $row['description'] ?></p>
                <a href="/views/edit.php?id=<?php echo $row['id'] ?>">Edit Pet</a>
            </div>

    <?php

        }
    }
    ?>
</body>

</html>
```

The first 2 statements will allow us to access the DBConnector and Pet Classes.

The when call getAllPetsFromDB method and display the results neatly.

Note the Edit button has the Id in the URL.

# BUILDING AN EDIT A PET FEATURE

Let's build the form for editing a pet.

Create a new php script called `edit.php` in `views/` folder. The form will be very simlar to Create Form.

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Edit Pet</title>
    <link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">
</head>

<body>
    <h1>Edit Pet</h1>
    <form action="/controllers/edit.php" method="post">
        <label for="name">Name: </label><input type="text" name="name" id="name" value="<?php echo $currentPet['name']; ?>" required><br>
        <label for="species" required>Species: </label>
        <select name="species" id="species" value="<?php echo $currentPet['species']; ?>">
            <option value="CAT" <?php if ($currentPet['species'] == 'CAT') {
                                    echo 'selected';
                                } ?>>Cat</option>
            <option value="DOG" <?php if ($currentPet['species'] == 'DOG') {
                                    echo 'selected';
                                } ?>>Dog</option>
        </select><br>
        <label for="price">Price: </label>
        <input type="number" name="price" id="price" required value="<?php echo $currentPet['price']; ?>"><br>
        <label for="description">Description: </label>
        <textarea name="description" id="description" cols="30" rows="5" required><?php echo $currentPet['description']; ?></textarea><br>
        <input type="hidden" name="id" value="<?php echo $currentPet['id']; ?>" />
        <input type="submit" value="Update">
        <input type="reset" value="Clear">
    </form>
</body>

</html>
```

You may have guessed from the form above we will need to add data about Pet that needs to be edited in the form.

To do this let's modify the views/edit.php to be able to retrieve data for a particular pet id so we can pre-fill the form.

Let's add this to the top of our views/edit.php.

```php
<?php
include_once '../db.php';
include_once '../entities/pet.php';

if (!isset($_GET['id'])) {
    throw new Exception("No id was provided.");
}

$id = (int) $_GET['id'];

$pet = new Pet();

$connector = new DBConnector();
$conn = $connector->getConnection();

$result = $pet->getPet($conn, $id);
$currentPet = $result->fetch_assoc();

?>
<!DOCTYPE html>
<html lang="en">
...
```

We now need to add a getPet($id) method to our Pet class so we can retrieve Pet Information for the given Id. 

We will also need a method to update the given pet.

Modify Pet class in pet.php and the following methods.

```php
class Pet {
    //  ... 

    public function getPet($connection, $id)
    {
        $id = (int) $id;
        $sql = "SELECT * from `pet` where id = " . $id;
        return $connection->query($sql);
    }

    public function updateInDB($connection)
    {
        $sql = "UPDATE `pet` set name = ?, species = ?, description = ?, price = ? where id = ?";
        $this->printValues();
        $stmt = $connection->prepare($sql);
        $stmt->bind_param("sssdi", $this->getName(), $this->getSpecies(), $this->getDescription(), $this->getPrice(), $this->getId());
        $result = $stmt->execute();

        var_dump($result);
        if ($result) {
            echo "<script> window.alert('Successfully Updated!'); window.location.href='/';</script>";
        } else {
            throw new Exception("oops! pet table does not exist");
        }

        $stmt->close();
        $connection->close();
    }
}
```

Finally we need a controller script that actually makes the call to update the data.

create a new php script under controllers/ called edit.php

```php
<?php

include_once '../db.php';
include_once '../entities/pet.php';


if (!isset($_POST['id']) || !isset($_POST['name']) || !isset($_POST['species']) || !isset($_POST['price']) || !isset($_POST['description'])) {
    echo "<p>ERROR! id, name, species, price and description are required while updating Pet Records.</p>";
    exit();
}

// now create a new Pet

$connector = new DBConnector();
$conn = $connector->getConnection();

switch ($_POST['species']) {
    case 'DOG':
        $dog = new Dog();
        $dog->setId($_POST['id']);
        $dog->setName($_POST['name']);
        $dog->setPrice((float) $_POST['price']);
        $dog->setDescription($_POST['description']);
        $dog->updateInDB($conn);
        header('Location: /');
        break;

    case 'CAT':
        $cat = new Cat();
        $cat->setId($_POST['id']);
        $cat->setName($_POST['name']);
        $cat->setPrice((float)$_POST['price']);
        $cat->setDescription($_POST['description']);
        $cat->updateInDB($conn);
        header('Location: /');
        break;

    default:
        echo "<p>ERROR! Unkown Species provided.</p>";
        echo "<a href = '/views/create.php'>Go back</a>";
        exit();
}
```

Your edit feature should now be working.

# EXERCISE

Can you build a delete feature for the Store?