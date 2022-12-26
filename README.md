<h1 align="center">Train Ticket Booking System</h1>
<h4 align="center">Train Ticket Reservation System</h4>

<p align="center"><img src="https://wakatime.com/badge/user/0ac30051-5698-4ae9-851e-7d4853d4aba7/project/67466c4b-ebc9-4e81-8250-db76db6d993a.svg"></p>

<h1>Explain Functions</h1>
  <h4>25 December 2022</h4>
       <h5><b>reg_user()</b></h5>
        


[function file](https://github.com/JehanKandy/Train-Ticket-Booking-System-/blob/main/lib/function/function.php)

in first codeline

          function reg_user($username, $email, $pass, $cpass){
          $con = Connection();

          $chech_user = "SELECT * FROM user_tbl WHERE username = '$username' && email_user = '$email'";
          $chech_user_result = mysqli_query($con, $chech_user);
          $chech_user_nor = mysqli_num_rows($chech_user_result);
          
          

I used function for get user input username, email, passweord and Confirm Password from reg.php
   
          if(isset($_POST['register'])){
              $result = reg_user($_POST['username'], $_POST['email'],md5($_POST['pass']), md5($_POST['cpass']));
              echo $result;
          }
          
in first code i create php variable called `$check_user` and assign data which from `user_tbl` where username equal to `$username` and email equal to `email`
and then I create a variable called `$chech_user_result` and assign `mysqli_query($con, $chech_user);`
<br>
`mysqli_query`  function performs a query against a database. 

  [more about mysqli_query](https://www.w3schools.com/php/func_mysqli_query.asp)

and the i again create variable called `$chech_user_nor` and assign `mysqli_num_rows($chech_user_result);` inoder to that the meaning of `mysqli_num_rows()` function for count data according to ` $chech_user_result = mysqli_query($con, $chech_user);`

  [more about mysqli_num_rows](https://www.w3schools.com/php/func_mysqli_num_rows.asp)
  
         if(empty($username)){
            return  "<div class='alert alert-danger alert-dismissible fade show' role='alert'>
                    <strong>Username Error</strong> Username Cannot be Empty....!
                    <button type='button' class='close' data-dismiss='alert' aria-label='Close'>
                    <span aria-hidden='true'>&times;</span>
                    </button>
            </div>";
        } 
        
 in above code i check the `$username` is not empty, if the variable empty it gives output as 'Username Cannot be Empty....!'
 it's same for other 3 variable (`$email`, `$pass`, `$cpass`)
 
         if($pass != $cpass){
            return  "<div class='alert alert-danger alert-dismissible fade show' role='alert'>
                    <strong>Password Error</strong> Password and Confirm Password not match..!
                    <button type='button' class='close' data-dismiss='alert' aria-label='Close'>
                    <span aria-hidden='true'>&times;</span>
                    </button>
            </div>"; 
        }
 
 in above code I check `pass` and `$cpass` it means password and Confirm Password is equal or not, if not equal it gives output as `Password and Confirm Password not match..!`
 
 
             if($chech_user_nor > 0){
                return  "<div class='alert alert-danger alert-dismissible fade show' role='alert'>
                    <strong>User Error</strong> User Already Exists...!
                    <button type='button' class='close' data-dismiss='alert' aria-label='Close'>
                    <span aria-hidden='true'>&times;</span>
                    </button>
            </div>"; 
            }
            
  in above code I check that are there any users according to user enterd values, if there is existing user it gives output as ` User Already Exists...!`        
  if there are not any user according to user added values the values insert to the database table called `user_tbl` as following
  
                  $insert_user = "INSERT INTO user_tbl(username,email_user,user_pass,user_type,is_active,join_date)VALUES('$username','$email','$pass','user',0,NOW())";
                $insert_user_result = mysqli_query($con, $insert_user);
  
  
   
  [more about IF statements](https://www.w3schools.com/php/php_if_else.asp)
 
 
 <hr>
 
   <h4>25 December 2022</h4>
       <h5><b>login_user()</b></h5>
       
 in this function first codlines are same as reg_user() function.      
 
     function login_user($username, $user_pass){
        $con = Connection();

        $check_user = "SELECT * FROM user_tbl WHERE username = '$username' && user_pass = '$user_pass' && is_active = 1 && is_pending = 0";
        $check_user_result = mysqli_query($con, $check_user);
        $check_user_nor = mysqli_num_rows($check_user_result);
        $check_user_row = mysqli_fetch_assoc($check_user_result);
        
but in last codeline i create a variable called `$check_user_row` and assign `mysqli_fetch_assoc($check_user_result);` the meaning of 'mysqli_fetch_assoc()' The fetch_assoc() / mysqli_fetch_assoc() function fetches a result row as an associative array.  

[more about mysqli_fetch_assoc](https://www.w3schools.com/php/func_mysqli_fetch_assoc.asp)

and then i check the username and password is empty or not usoing following codelines


        if(empty($username)){
            return  "<div class='alert alert-danger alert-dismissible fade show' role='alert'>
                    <strong>Username Error</strong> Username Cannot be Empty....!
                    <button type='button' class='close' data-dismiss='alert' aria-label='Close'>
                    <span aria-hidden='true'>&times;</span>
                    </button>
            </div>";
        }
        if(empty($user_pass)){
            return  "<div class='alert alert-danger alert-dismissible fade show' role='alert'>
                    <strong>Password Error</strong>  Password Cannot be Empty....!
                    <button type='button' class='close' data-dismiss='alert' aria-label='Close'>
                    <span aria-hidden='true'>&times;</span>
                    </button>
            </div>"; 
        }
        
        
now I check the user is panding user or not for that I use following codelines

        $waiting_user = "SELECT * FROM user_tbl WHERE username = '$username' && user_pass = '$user_pass' $$ is_active = 0 && is_pending = 1";
        $waiting_user_result = mysqli_query($con, $waiting_user);
        $waiting_user_nor = mysqli_num_rows($waiting_user_result);

        if($waiting_user_nor > 0){
            header("location:waiting_user.php");
        }


if the user is pending user and header to waiting_user.php file

        
and I check are there any user according to user input values using

        if($check_user_nor > 0){
        
 then I check the user is user or an admin for that i use following codelines 
 
                 if($check_user_row['user_type'] == 'user'){
                    setcookie('login',$check_user_row['email_user'],time()+60*60,'/');
                    $_SESSION['LoginSession'] = $check_user_row['email_user'];
                    header("location:../routes/user.php");
                }
                elseif($check_user_row['user_type'] == 'admin'){
                    setcookie('login',$check_user_row['email_user'],time()+60*60,'/');
                    $_SESSION['LoginSession'] = $check_user_row['email_user'];
                    header("location:../routes/user.php");
                }
        
in here i use `setcookie` and `$_SESSION` the meaning of `setcookie` is create a cookie for login user and it will expire after 1 hour (by `time()+60*60,'/'`)       
  
 [more about cookies](https://www.w3schools.com/php/php_cookies.asp) 
 
 [more about session](https://www.w3schools.com/php/php_sessions.asp)
 
 
 and lastly in this function i use following codelines 
 
         else{
            if($check_user_row['is_active'] == 0){
                return  "<div class='alert alert-danger alert-dismissible fade show' role='alert'>
                        <strong>User Error</strong>  User Deactivate....!
                        <button type='button' class='close' data-dismiss='alert' aria-label='Close'>
                        <span aria-hidden='true'>&times;</span>
                        </button>
                </div>";  
            }else{
                return  "<div class='alert alert-danger alert-dismissible fade show' role='alert'>
                        <strong>User Error</strong>  User Does not Exists....!
                        <button type='button' class='close' data-dismiss='alert' aria-label='Close'>
                        <span aria-hidden='true'>&times;</span>
                        </button>
                </div>";  
            }
        } 
        
for if the user is deactivated buy admin the user can get output as `User Deactivate` if there are no existing user in database user can get outupt as `User Does not Exists....!`    
  
<hr>  
  
  
<h1>Development Timelne</h1>
  <h4>18 December 2022</h4>
  
  1. DEVELOP
  - Project Start

  <hr>
  
  <h4>19 December 2022</h4>
  
  1. DEVELOP
  - lib/layouts/header.php
  - index.php
  - lib/layouts/main_footer.php
  - css/style.css
  
  <hr>
  
  <h4>21 December 2022</h4>
  
  1. DEVELOP
  - docs/find_train.php
  - lib/layouts/nav_bar.php
  2. UPDATE
  - index.php
  - css/style.css

  <hr>
  
  <h4>22 December 2022</h4>
  
  1. DEVELOP
  2. UPDATE
  - css/style.css
  - docs/find_train.php
  
  <hr>
  
  <h4>23 December 2022</h4>
  
  1. DEVELOP
  - docs/gallery.php
  - docs/about.php
  - docs/train_routes.php
  2. UPDATE
  - lib/layouts/main_footer.php
  - css/style.css
  - lib/layouts/nav_bar.php

  <hr>
  
  <h4>25 December 2022</h4>
  
  1. DEVELOP
  - docs/news.php
  - lib/layouts/login_footer.php
  - lib/views/login.php
  - lib/views/reg.php
  - lib/function/config.php
  - lib/function/function.php
  - - functions:  reg_user()
    
  
  2. UPDATE
  - css/style.css
  - lib/layouts/nav_bar.php  
  - index.php
  
  <hr>
  
  <h4>26 December 2022</h4>
  
  1. DEVELOP
    
  
  2. UPDATE
  - lib/layouts/login.php
  - lib/function/function.php
  - - functions login_user()

<h1>Developers</h1>
  <h4>JehanKandy</h4>

  - Github [JehanKandy](https://github.com/JehanKandy)
  - Twitter [@JehanKandy](https://twitter.com/jehankandy)
  
<h1>Copyright and license</h1>

Copyright 2021–2022 [JehanKandy](https://github.com/JehanKandy). <b>Train Ticket Booking System</b> released under the [MIT License](https://github.com/JehanKandy/Train-Ticket-Booking-System-/blob/main/LICENSE)
