```
<?php 
session_start();
if(isset($_SESSION['username'])){
    header('Location:index.php');
}

?>
```
> LOGIN FILE
```
<?php 
session_start();
unset($_SESSION['username']);
unset($_SESSION['image']);
unset($_SESSION['date']);
    header('Location:login.php');
?>
```
