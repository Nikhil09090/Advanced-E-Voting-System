//code
SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET AUTOCOMMIT = 0;
START TRANSACTION;
SET time_zone = "+00:00";
/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;
--
-- Database: `online-voting-system`
--
--
-- Table structure for table `user`
--
CREATE TABLE `user` (
`id` int(11) NOT NULL,
`name` text NOT NULL,
`mobile` bigint(10) NOT NULL,
`password` int(11) NOT NULL,
`address` varchar(255) NOT NULL,
`photo` varchar(255) NOT NULL,
`status` int(11) NOT NULL,
`votes` int(11) NOT NULL,
`role` int(11) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
--
-- Indexes for dumped tables
--
--
-- Indexes for table `user`
--
19
ALTER TABLE `user`
ADD PRIMARY KEY (`id`);
--
-- AUTO_INCREMENT for dumped tables
--
--
-- AUTO_INCREMENT for table `user`
--
ALTER TABLE `user`
MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=8;
COMMIT;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
 The above SQL commands and queries have been used to create a database for our Online Voting System to store and
retrieve the information from this database in SQL.
o PHP Source Code :-
<html>
<head>
<title>Online voting system - Home</title>
<link rel="stylesheet" href="css/stylesheet.css">
</head>
<body>
<center>
<div id="headerSection">
<h1>Online Voting System</h1>
</div>
<hr>
<div id="loginSection">
<h2>Login</h2>
<form action="api/login.php" method="POST">
<input type="number" name="mob" placeholder="Enter mobile" required><br><br>
<input type="password" name="pass" placeholder="Enter password" required><br><br>
<select name="role" style="width: 15%; border: 2px solid black">
<option value="1">Voter</option>
20
<option value="2">Group</option>
</select><br><br>
<button id="loginbtn" type="submit" name="loginbtn">Login</button><br><br>
New user? <a href="routes/register.php">Register here</a>
</form>
</div>
</center>
</body>
</html>
 The above PHP code has been used to index the project application.
o PHP Source Code (API) :-
<?php
include("connection.php");
$name = $_POST['name'];
$mobile = $_POST['mob'];
$pass = $_POST['pass'];
$cpass = $_POST['cpass'];
$add = $_POST['add'];
$image = $_FILES['image']['name'];
$tmp_name = $_FILES['image']['tmp_name'];
$role = $_POST['role'];
if($cpass!=$pass){
echo '<script>
alert("Passwords do not match!");
window.location = "../routes/register.php";
</script>';
}
else{
move_uploaded_file($tmp_name,"../uploads/$image");
$insert = mysqli_query($connect, "insert into user (name, mobile, password, address, photo, status, votes, role)
values('$name', '$mobile', '$pass', '$add', '$image', 0, 0, '$role') ");
if($insert){
echo '<script>
alert("Registration successfull!");
window.location = "../";
</script>';
}
21
}
?>
 The above PHP code has been used to create and insert the data into the Registration Page of our Voting System, which
also serves as the API.
o PHP Source Code :-
<html>
<head>
<title>Online voting system - Registratrion</title>
<link rel="stylesheet" href="../css/stylesheet.css">
</head>
<body>
<center>
<div id="headerSection">
<h1>Online Voting System</h1>
</div>
<hr>
<h2>Registration</h2>
<form action="../api/register.php" method="POST" enctype="multipart/form-data">
<input type="text" name="name" placeholder="Name" required>&nbsp
<input type="number" name="mob" placeholder="Mobile" required><br><br>
<input type="password" name="pass" placeholder="Password" required>&nbsp
<input type="password" name="cpass" placeholder="Confirm Password" required><br><br>
<input style="width: 31%" type="text" name="add" placeholder="Address" required><br><br>
<div id="upload" style="width: 30%">
Upload image: <input type="file" id="profile" name="image" required>
</div><br>
<div id="upload" style="width: 30%">
Select your role:
<select name="role">
<option value="1">Voter</option>
<option value="2">Group</option>
</select><br>
</div><br>
<button id="loginbtn" type="submit" name="registerbtn">Register</button><br><br>
Already user? <a href="../">Login here</a>
</form>
</center>
</body>
22
</html>
 The above PHP code has been used for displaying the Registration Page of our Voting System.
o PHP Source Code :-
<?php
session_start();
include("connection.php");
$mobile = $_POST['mob'];
$pass = $_POST['pass'];
$role = $_POST['role'];
$check = mysqli_query($connect, "select * from user where mobile='$mobile' and password='$pass' and role='$role' ");
if(mysqli_num_rows($check)>0){
$getGroups = mysqli_query($connect, "select name, photo, votes, id from user where role=2 ");
if(mysqli_num_rows($getGroups)>0){
$groups = mysqli_fetch_all($getGroups, MYSQLI_ASSOC);
$_SESSION['groups'] = $groups;
}
$data = mysqli_fetch_array($check);
$_SESSION['id'] = $data['id'];
$_SESSION['status'] = $data['status'];
$_SESSION['data'] = $data;
echo '<script>
window.location = "../routes/dashboard.php";
</script>';
}
else{
echo '<script>
alert("Invalid credentials!");
window.location = "../";
</script>';
}
?>
 This PHP code has been used to create the Login Page of our Voting System, which also serves as the API.
23
o PHP Source Code :-
<?php
session_start();
session_destroy();
header("location: ../");
?>
 The above PHP code has been used which helps the user logout of the Voting System with ease.
o PHP Source Code :-
<?php
session_start();
if(!isset($_SESSION['id'])){
header("location: ../");
}
$data = $_SESSION['data'];
if($_SESSION['status']==1){
$status = '<b style="color: green">Voted</b>';
}
else{
$status = '<b style="color: red">Not Voted</b>';
}
?>
<html>
<head>
<title>Online voting system - Dashboard</title>
<link rel="stylesheet" href="../css/stylesheet.css">
</head>
<body>
<center>
<div id="headerSection">
<a href="../"><button id="back-button"> Back</button></a>
<a href="logout.php"><button id="logout-button">Logout</button></a>
<h1>Online Voting System</h1>
</div>
</center>
24
<hr>
<div id="mainSection">
<div id="profileSection">
<center><img src="../uploads/<?php echo $data['photo']?>" height="100" width="100"></center><br>
<b>Name : </b><?php echo $data['name'] ?><br><br>
<b>Mobile : </b><?php echo $data['mobile'] ?><br><br>
<b>Address : </b><?php echo $data['address'] ?><br><br>
<b>Status : </b><?php echo $status ?>
</div>
<div id="groupSection">
<?php
if(isset($_SESSION['groups'])){
$groups = $_SESSION['groups'];
for($i=0; $i<count($groups); $i++){
?>
<div style="border-bottom: 1px solid #bdc3c7; margin-bottom: 10px">
<img style="float: right" src="../uploads/<?php echo $groups[$i]['photo']?>" height="80" width="80">
<b>Group Name : </b><?php echo $groups[$i]['name']?><br><br>
<b>Votes :</b> <?php echo $groups[$i]['votes']?><br><br>
<form method="POST" action="../api/vote.php">
<input type="hidden" name="gvotes" value="<?php echo $groups[$i]['votes'] ?>">
<input type="hidden" name = "gid" value="<?php echo $groups[$i]['id'] ?>">
<?php
if($_SESSION['status']==1){
?>
<button disabled style="padding: 5px; font-size: 15px; background-color: #27ae60; color: white;
border-radius: 5px;" type="button">Voted</button>
<?php
}
else{
?>
<button style="padding: 5px; font-size: 15px; background-color: #3498db; color: white; border￾radius: 5px;" type="submit">Vote</button>
<?php
}
?>
</form>
</div>
<?php
}
}
else{
25
?>
<div style="border-bottom: 1px solid #bdc3c7; margin-bottom: 10px">
<b>No groups available right now.</b>
</div>
<?php
}
?>
</div>
</div>
</body>
</html>
 The above PHP code has been used to create and display the Dashboard (main UI) of our Voting System.
o PHP Source Code (API) :-
<?php
session_start();
include("connection.php");
$votes = $_POST['gvotes'];
$total_votes= $votes+1;
$gid = $_POST['gid'];
$uid = $_SESSION['id'];
$update_votes = mysqli_query($connect, "update user set votes='$total_votes' where id='$gid'");
$update_status = mysqli_query($connect, "update user set status=1 where id='$uid'");
if($update_status and $update_votes){
$getGroups = mysqli_query($connect, "select name, photo, votes, id from user where role=2 ");
$groups = mysqli_fetch_all($getGroups, MYSQLI_ASSOC);
$_SESSION['groups'] = $groups;
$_SESSION['status'] = 1;
echo '<script>
alert("Voting successfull!");
window.location = "../routes/dashboard.php";
</script>';
}
else{
echo '<script>
alert("Voting failed!.. Try again.");
window.location = "../routes/dashboard.php";
26
</script>';
}
?>
 The above PHP code has been used which helps the voter to cast the vote with ease in the Voting System. It updates the
tally of the number of votes casted in real-time and also displays a message when the vote has been casted successfully or
throws an error when the voting goes unsuccessful, asking the user to try again. It also serves as the API.
o PHP Source Code :-
<?php
$connect = mysqli_connect("localhost", "root", "", "online-voting-system");
?>
 The above PHP code has been used to establish a connection between the SQL database and the Voting System
Application and also serves as the API.
o CSS Source Code :-
input {
padding: 10px;
border-radius: 5px;
}
select {
padding: 10px;
border-radius: 5px;
}
#upload {
padding: 10px;
border-radius: 5px;
border: 2px solid black;
}
#headerSection {
padding: 2px;
font-family: Cursive;
}
27
#loginSection {
padding: 5px;
}
body {
background-color: #b8e994;
}
#loginbtn {
padding: 5px;
font-size: 15px;
background-color: #3498db;
color: white;
border-radius: 5px;
}
#reglink {
padding: 5px;
font-size: 15px;
background-color: #3498db;
color: white;
border-radius: 5px;
text-decoration: none;
}
a {
text-decoration: none;
}
#mainSection {
padding: 10px;
}
#profileSection {
width: 30%;
float: left;
background-color: white;
padding: 20px;
}
#groupSection {
width: 60%;
float: right;
background-color: white;
28
padding: 20px;
}
#back-button {
float: left;
margin-left: 20px;
margin-top: 20px;
padding: 5px;
font-size: 15px;
background-color: #3498db;
color: white;
border-radius: 5px;
}
#logout-button {
float: right;
margin-right: 20px;
margin-top: 20px;
padding: 5px;
font-size: 15px;
background-color: #3498db;
color: white;
border-radius: 5px;
}
