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
> IMAGE 

```
 <img src="<?php echo "uploads/". $_SESSION ['image'] ?>
```

> LOGIN API

```
function login($conn){
    extract($_POST);
    $data = array();
    $array_data = array();
   $query ="call login('$username','$password')";
    $result = $conn->query($query);


    if($result){

        $row = $result->fetch_assoc();

        if(isset($row ['message'])){
            if ($row['message'] == "deny"){
                $data = array("status" => false, "data" => "username or password is incorrect");

            }else {
                $data = array("status" => false, "data" => "username is locked by administrator"); 

            }

        }else{
            foreach($row as $key => $values){
                $_SESSION [$key] = $values;
            }
            $data = array("status" => true, "data" => "success");
        }

    }else{
        $data = array("status" => false, "data"=> $conn->error);
             
    }

    echo json_encode($data);
}
```

> login js

```

$("#login").on("submit", function(event){
    
    event.preventDefault();

    let username = $("#username").val();
    let password = $("#password").val();


    let sendingData ={


      "action": "login",
      "username": username,
      "password": password
  }
  
  $.ajax({
    method: "POST",
    dataType: "JSON",
    url: "api/login_api.php",
    data : sendingData,
  
      success : function(data){
          let status= data.status;
          let response= data.data;
        
  
          if(status){

            window.location.href = "index.php"
            console.log("goooood")
  
          }else{
            console.log("noooooo")

            displaymessagee("error", response);
          }
  
      },
      error: function(data){
  
      }
  
  })
    });
```