PHP My SQLi Prepared Statement

<h5> 1.1. SELECT - Selecting one row</h5> 
<hr style="border-color:gold !important;"/>

```PHP
$uid = $_POST['cuid'];
$stmt = $mysqli -> prepare('SELECT name, email FROM users WHERE id = ?'); 
$stmt -> bind_param('i', $userId); 
$userId = $uid;
$stmt -> execute(); 
$stmt -> store_result(); 
$stmt -> bind_result($name, $email); 
$stmt -> fetch();
 
echo $name;
echo $email;

```

<h5> 1.2. SELECT - Selecting Multiple Rows</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare('SELECT name, email FROM users'); 
$stmt -> execute(); 
$stmt -> store_result(); 
$stmt -> bind_result($name, $email); 
while ($stmt -> fetch()) { 
    echo $name; 
    echo $email; 
}

```

<h5> 1.3. SELECT - Getting Number of Selected Rows</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare('SELECT name, email FROM users');

$stmt -> execute();
$stmt -> store_result();

echo $stmt -> num_rows;


<h5> 1.4. SELECT - Get Results</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare('SELECT name, email FROM users WHERE id > ?');
$greaterThan = 1;
$stmt -> bind_param('i', $greaterThan);
$stmt -> execute();
$result = $stmt -> get_result();

```

<h5>1.5. SELECT - With Wildcards</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare('SELECT name, email FROM users WHERE name LIKE ?');

$like = 'a%';
$stmt -> bind_param('s', $like);
$stmt -> execute();
$stmt -> store_result();
$stmt -> bind_result($name, $email);

while ($stmt -> fetch()) {
	echo $name;
	echo $email;
}

```

<h5>1.6. SELECT - With An Array of IDs</h5> 
<hr />

```PHP
// array of user IDs
$userIdArray = [1,2,3,4];
// number of question marks
$questionMarksCount = count($userIdArray);
// create a array with question marks
$questionMarks = array_fill(0, $questionMarksCount, '?');
// join them with ,
$questionMarks = implode(',', $questionMarks);
// data types for bind param
$dataTypes = str_repeat('i', $questionMarksCount);

$stmt = $mysqli -> prepare("SELECT name, email FROM users WHERE id IN ($questionMarks)");

$stmt -> bind_param($dataTypes, ...$userIdArray);
$stmt -> execute();
$stmt -> store_result();
$stmt -> bind_result($name, $email);

while ($stmt -> fetch()) {
	echo $name;
	echo $email;
}

```

<h5>1.7. SELECT - LIMIT and OFFSET</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare("SELECT name, email FROM users LIMIT ? OFFSET ?");

// limit of rows
$limit = 2;
// skip n rows
$offset = 1;

$stmt -> bind_param('ii', $limit, $offset);
$stmt -> execute();
$stmt -> store_result();
$stmt -> bind_result($name, $email);

while ($stmt -> fetch()) {
	echo $name;
	echo $email;
}
```

<h5> 1.8. SELECT - BETWEEN</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare("SELECT name, email FROM users WHERE id BETWEEN ? AND ?");

$betweenStart = 2;
$betweenEnd = 4;

$stmt -> bind_param('ii', $betweenStart, $betweenEnd);
$stmt -> execute();
$stmt -> store_result();
$stmt -> bind_result($name, $email);

while ($stmt -> fetch()) {
	echo $name;
	echo $email;
}
```

<h5> 2. INSERT - One Row</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare('INSERT INTO users (name, email) VALUES (?,?)');

$name = 'John';
$email = 'john@gmail.com';

$stmt -> bind_param('ss', $name, $email);
$stmt -> execute();
10. INSERT - Getting Insert ID
$stmt = $mysqli -> prepare('INSERT INTO users (name, email) VALUES (?,?)');

$name = 'John';
$email = 'john@gmail.com';

$stmt -> bind_param('ss', $name, $email);
$stmt -> execute();

echo 'Your account id is ' . $stmt -> insert_id;

```

<h5> 11. INSERT - Multiple Rows (Recursive)</h5> 
<hr />

```PHP
$newUsers = [
	[ 'sulliops', 'sulliops@gmail.com' ],
	[ 'infinity', 'infinity@gmail.com' ],
	[ 'aivarasco', 'aivarasco@gmail.com' ]
];

$stmt = $mysqli -> prepare('INSERT INTO users (name, email) VALUES (?,?)');

foreach ($newUsers as $user) {
		
	$name = $user[0];
	$email = $user[1];

	$stmt -> bind_param('ss', $name, $email);
	$stmt -> execute();

	echo "{$name}'s account id is {$stmt -> insert_id}";

}
```

<h5> 12. UPDATE</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare('UPDATE users SET email = ? WHERE id = ? LIMIT 1');
	
$email = 'newemail@hyvor.com';
$id = 2;

$stmt -> bind_param('si', $email, $id);
$stmt -> execute();
```

<h5>13. UPDATE - Get Affected Rows</h5> 
<hr />

```PHP
$stmt = $mysqli -> prepare('UPDATE users SET email = ? WHERE name = ? LIMIT 1');
	
$email = 'newemail@hyvor.com';
$name = 'teodor';

$stmt -> bind_param('ss', $email, $name);
$stmt -> execute();

// 1
echo $stmt -> affected_rows;
14. DELETE
$stmt = $mysqli -> prepare('DELETE FROM users WHERE id = ?');
	
$userId = 4;

$stmt -> bind_param('i', $userId);
$stmt -> execute();

// number of deleted rows
echo $stmt -> affected_rows;

```
