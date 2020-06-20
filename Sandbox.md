[Sandbox Address](https://www.tutorialspoint.com/php_mysql_online.php)

Setup
<hr />

```php
<?php
$database = "CODINGGROUND";
$dsn = "localhost";
$username = 'root';
$password = 'root';

$conn = new mysqli($dsn, $username, $password, $database);

$sql = "CREATE TABLE persons(
            id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
            fname VARCHAR(30) NOT NULL,
            lname VARCHAR(30) NOT NULL,
            age INT(3) NOT NULL,
            email VARCHAR(70) NOT NULL UNIQUE
    )";
if($conn->query($sql) === true){
    echo "Table created successfully.";
} else{
    echo "ERROR: Could not able to execute $sql. " . $conn->error;
}

$sql2 = "INSERT INTO persons (fname, lname, age, email) VALUES
            ('John', 'Rambo', 33, 'johnrambo@mail.com'),
            ('Sandra', 'Bullock', 55, 'sandrabullock@mail.com'),
            ('Karen', 'Gillan',32, 'karengillan@mail.com'),            
            ('Clark', 'Kent', 45, 'clarkkent@mail.com'),
            ('John', 'Carter',28, 'johncarter@mail.com'),
            ('Jessica', 'Alba',39, 'jessicaalba@mail.com'),
            ('Harry', 'Potter',18, 'harrypotter@mail.com')";
if($conn->query($sql2) === true){
    echo "Records inserted successfully.";
} else{
    echo "ERROR: Could not able to execute $sql. " . $conn->error;
}
 
$conn->close(); 

?>

?>
```
That's because fetch_assoc is not part of a mysqli_stmt object. fetch_assoc belongs to the mysqli_result class. You can use mysqli_stmt::get_result to first get a result object and then call fetch_assoc:

```php
$selectUser = $db->prepare("SELECT `id`,`password`,`salt` FROM `users` WHERE `username`=?");
$selectUser->bind_param('s', $username);
$selectUser->execute();
$result = $selectUser->get_result();
$assoc = $result->fetch_assoc();
```

Alternatively, you can use bind_result to bind the query's columns to variables and use fetch() instead:

```php
$selectUser = $db->prepare("SELECT `id`,`password`,`salt` FROM `users` WHERE `username`=?");
$selectUser->bind_param('s', $username);
$selectUser->bind_result($id, $password, $salt);
$selectUser->execute();
while($selectUser->fetch())
{
    //$id, $password and $salt contain the values you're looking for
}
```

```php
<?php
$database = "CODINGGROUND";
$dsn = "localhost";
$username = 'root';
$password = 'root';

$conn = new mysqli($dsn, $username, $password, $database);
 
$sql = 'SELECT `id`, `name`, `age`, `sex` FROM users';
//$sql = 'SELECT * FROM users';
$stmt = $conn->prepare($sql);
$stmt->execute();
$stmt -> store_result();
$stmt -> bind_result($id, $name, $age, $sex);
//======================================================= One
// while($stmt->fetch()){
//   $items[] = compact('id', 'name', 'age', 'sex');
// }
// print_r($items);
//======================================================= Two
// $items= array();
// while($stmt->fetch()){
//   array_push($items, array('id'=>$id, 'name'=>$name, 'age'=>$age, 'sex'=>$sex));
// }
// print_r($items);
//======================================================= Three
$items= array();
while($row = $stmt->fetch(MYSQLI_ASSOC)){
   //array_push($items, $row);
   print_r($row);
}
//print_r($items);
?>
```
