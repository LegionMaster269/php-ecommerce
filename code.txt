//settings.php

<?php include ( "inc/connect.inc.php" ); ?>
<?php 

ob_start();
session_start();
if (!isset($_SESSION['user_login'])) {
	header("location: login.php");
}
else {
	$user = $_SESSION['user_login'];
	$result = mysql_query("SELECT * FROM user WHERE id='$user'");
		$get_user_email = mysql_fetch_assoc($result);
			$uname_db = $get_user_email['firstName'];
			$uemail_db = $get_user_email['email'];
			$upass = $get_user_email['password'];

			$umob_db = $get_user_email['mobile'];
			$uadd_db = $get_user_email['address'];
}

if (isset($_REQUEST['uid'])) {
	
	$user2 = mysql_real_escape_string($_REQUEST['uid']);
	if($user != $user2){
		header('location: index.php');
	}
}else {
	header('location: index.php');
}

if (isset($_POST['changesettings'])) {
//declere veriable
$email = $_POST['email'];
$opass = $_POST['opass'];
$npass = $_POST['npass'];
$npass1 = $_POST['npass1'];
//triming name
	try {
		if(empty($_POST['email'])) {
			throw new Exception('Email can not be empty');
			
		}
			if(isset($opass) && isset($npass) && isset($npass1) && ($opass != "" && $npass != "" && $npass1 != "")){
				if( md5($opass) == $upass){
					if($npass == $npass1){
						$npass = md5($npass);
						mysql_query("UPDATE user SET password='$npass' WHERE id='$user'");
						$success_message = '
						<div class="signupform_text" style="font-size: 18px; text-align: center;">
						<font face="bookman">
							Password changed.
						</font></div>';
					}else {
					$success_message = '
						<div class="signupform_text" style=" color: red; font-size: 18px; text-align: center;">
						<font face="bookman">
							New password not matched!
						</font></div>';
					}
				}else {
				$success_message = '
					<div class="signupform_text" style=" color: red; font-size: 18px; text-align: center;">
					<font face="bookman">
						Fillup password field exactly.
					</font></div>';
				}
			}else {
				$success_message = '
					<div class="signupform_text" style=" color: red; font-size: 18px; text-align: center;">
					<font face="bookman">
						Fillup password field exactly.
					</font></div>';
				}

			if($uemail_db != $email) {
				if(mysql_query("UPDATE user SET  email='$email' WHERE id='$user'")){
					//success message
					$success_message = '
					<div class="signupform_text" style="font-size: 18px; text-align: center;">
					<font face="bookman">
						Settings change successfull.
					</font></div>';
				}
			}

	}
	catch(Exception $e) {
		$error_message = $e->getMessage();
	}
}


?>

<!DOCTYPE html>
<html>
<head>
	<title>SAREE</title>
	<link rel="stylesheet" type="text/css" href="css/style.css">
	<meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body style="background-image: url(image/homebackgrndimg1.png);">
	<div class="homepageheader">
			<div class="signinButton loginButton">
				<div class="uiloginbutton signinButton loginButton" style="margin-right: 40px;">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none; color: #fff;" href="logout.php">LOG OUT</a>';
						}
						else {
							echo '<a style="text-decoration: none; color: #fff;" href="signin.php">SIGN IN</a>';
						}
					 ?>
					
				</div>
				<div class="uiloginbutton signinButton loginButton" style="">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none; color: #fff;" href="profile.php?uid='.$user.'">Hi '.$uname_db.'</a>';
						}
						else {
							echo '<a style="text-decoration: none; color: #fff;" href="login.php">LOG IN</a>';
						}
					 ?>
				</div>
			</div>
			<div style="float: left; margin: 5px 0px 0px 23px;">
				<a href="index.php">
					<img style=" height: 75px; width: 130px;" src="image/ebuybdlogo.png">
				</a>
			</div>
			<div class="">
				<div id="srcheader">
					<form id="newsearch" method="get" action="http://www.google.com">
					        <input type="text" class="srctextinput" name="q" size="21" maxlength="120"  placeholder="Search Here..."><input type="submit" value="search" class="srcbutton" >
					</form>
				<div class="srcclear"></div>
				</div>
			</div>
		</div>
	<div class="categolis">
		<table>
			<tr>
				<th>
					<a href="women/saree.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Saree</a>
				</th>
				<th><a href="women/ornament.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Ornament</a></th>
				<th><a href="women/watch.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Watch</a></th>
				<th><a href="women/perfume.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Perfume</a></th>
				<th><a href="women/hijab.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Hijab</a></th>
				<th><a href="women/tshirt.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">T-Shirt</a></th>
				<th><a href="women/footwear.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">FootWear</a></th>
				<th><a href="women/toilatry.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Toilatry</a></th>
			</tr>
		</table>
	</div>
	<div style="margin-top: 20px;">
		<div style="width: 901px; margin: 0 auto;">
		
			<ul>
				<li style="float: left;">
					<div class="settingsleftcontent">
						<ul>
							<li><?php echo '<a href="profile.php?uid='.$user.'" >My Orders</a>'; ?></li>
							<li><?php echo '<a href="settings.php?uid='.$user.'" style=" background-color: #169e8f; border-radius: 4px; color: #fff;">Settings</a>'; ?></li>
						</ul>
					</div>
				</li>
				<li style="float: right;">
					<div class="holecontainer" style=" padding-top: 20px; padding: 0 20%">
						<form action="" method="POST" class="registration">
							<div class="container signupform_content ">
								<div style="text-align: center;font-size: 20px;color: #fff;margin: 0 0 5px 0;">
									<td >Change Password:</td>
								</div>
								<div>
									<td><input class="email signupbox" type="password" name="opass" placeholder="Old Password"></td>
								</div>
								<div>
									<td><input class="email signupbox" type="password" name="npass" placeholder="New Password"></td>
								</div>
								<div>
									<td><input class="email signupbox" type="password" name="npass1" placeholder="Repeat Password"></td>
								</div>
								<div style="text-align: center;font-size: 20px;color: #fff;margin: 0 0 5px 0;">
									<td >Change Email:</td>
								</div>
								<div>
									<td><?php echo '<input class="email signupbox" required type="email" name="email" placeholder="New Email" value="'.$uemail_db.'">'; ?></td>
								</div>
								<div>
									<td><input class="uisignupbutton signupbutton" type="submit" name="changesettings" value="Update Settings"></td>
								</div>
								<div>
									<?php if (isset($success_message)) {echo $success_message;} ?>
								</div>
							</div>
						</form>
					</div>
				</li>
			</ul>
		</div>
	</div>

	
</body>
</html>


//signin.php 
<?php include ( "inc/connect.inc.php" ); ?>
<?php
ob_start();
session_start();
if (!isset($_SESSION['user_login'])) {
}
else {
	header("location: index.php");
}

$u_fname = "";
$u_lname = "";
$u_email = "";
$u_mobile = "";
$u_address = "";
$u_pass = "";

if (isset($_POST['signup'])) {
//declere veriable
$u_fname = $_POST['first_name'];
$u_lname = $_POST['last_name'];
$u_email = $_POST['email'];
$u_mobile = $_POST['mobile'];
$u_address = $_POST['signupaddress'];
$u_pass = $_POST['password'];
//triming name
$_POST['first_name'] = trim($_POST['first_name']);
$_POST['last_name'] = trim($_POST['last_name']);
	try {
		if(empty($_POST['first_name'])) {
			throw new Exception('Fullname can not be empty');
			
		}
		if (is_numeric($_POST['first_name'][0])) {
			throw new Exception('Please write your correct name!');

		}
		if(empty($_POST['last_name'])) {
			throw new Exception('Lastname can not be empty');
			
		}
		if (is_numeric($_POST['last_name'][0])) {
			throw new Exception('lastname first character must be a letter!');

		}
		if(empty($_POST['email'])) {
			throw new Exception('Email can not be empty');
			
		}
		if(empty($_POST['mobile'])) {
			throw new Exception('Mobile can not be empty');
			
		}
		if(empty($_POST['password'])) {
			throw new Exception('Password can not be empty');
			
		}
		if(empty($_POST['signupaddress'])) {
			throw new Exception('Address can not be empty');
			
		}

		
		// Check if email already exists
		
		$check = 0;
		$e_check = mysql_query("SELECT email FROM `user` WHERE email='$u_email'");
		$email_check = mysql_num_rows($e_check);
		if (strlen($_POST['first_name']) >2 && strlen($_POST['first_name']) <16 ) {
			if ($check == 0 ) {
				if ($email_check == 0) {
					if (strlen($_POST['password']) >1 ) {
						$d = date("Y-m-d"); //Year - Month - Day
						$_POST['first_name'] = ucwords($_POST['first_name']);
						$_POST['last_name'] = ucwords($_POST['last_name']);
						$_POST['last_name'] = ucwords($_POST['last_name']);
						$_POST['email'] = mb_convert_case($u_email, MB_CASE_LOWER, "UTF-8");
						$_POST['password'] = md5($_POST['password']);
						$confirmCode   = substr( rand() * 900000 + 100000, 0, 6 );
						// send email
						$msg = "
						Assalamu Alaikum...
						
						Your activation code: ".$confirmCode."
						Signup email: ".$_POST['email']."
						
						";
						if (@mail($_POST['email'],"eBuyBD Activation Code",$msg, "From:eBuyBD <no-reply@ebuybd.xyz>")) {
							
						$result = mysql_query("INSERT INTO user (firstName,lastName,email,mobile,address,password,confirmCode) VALUES ('$_POST[first_name]','$_POST[last_name]','$_POST[email]','$_POST[mobile]','$_POST[signupaddress]','$_POST[password]','$confirmCode')");
						
						//success message
						$success_message = '
						<div class="signupform_content"><h2><font face="bookman">Registration successfull!</font></h2>
						<div class="signupform_text" style="font-size: 18px; text-align: center;">
						<font face="bookman">
							Email: '.$u_email.'<br>
							Activation code sent to your email. <br>
							Your activation code: '.$confirmCode.'
						</font></div></div>';
						}else {
							throw new Exception('Email is not valid!');
						}
						
						
					}else {
						throw new Exception('Make strong password!');
					}
				}else {
					throw new Exception('Email already taken!');
				}
			}else {
				throw new Exception('Username already taken!');
			}
		}else {
			throw new Exception('Firstname must be 2-15 characters!');
		}

	}
	catch(Exception $e) {
		$error_message = $e->getMessage();
	}
}


?>


<!doctype html>
<html>
	<head>
		<title>Welcome to ebuybd online shop</title>
		<link rel="stylesheet" type="text/css" href="css/style.css">
	</head>
	<body class="home-welcome-text" style="background-image: url(image/homebackgrndimg2.png);">
		<div class="homepageheader" style="position: inherit;">
			<div class="signinButton loginButton">
				<div class="uiloginbutton signinButton loginButton" style="margin-right: 40px;">
					<a style="text-decoration: none;" href="signin.php">SIGN IN</a>
				</div>
				<div class="uiloginbutton signinButton loginButton" style="">
					<a style="text-decoration: none;" href="login.php">LOG IN</a>
				</div>
			</div>
			<div style="float: left; margin: 5px 0px 0px 23px;">
				<a href="index.php">
					<img style=" height: 75px; width: 130px;" src="image/ebuybdlogo.png">
				</a>
			</div>
			<div class="">
				<div id="srcheader">
					<form id="newsearch" method="get" action="http://www.google.com">
					        <input type="text" class="srctextinput" name="q" size="21" maxlength="120"  placeholder="Search Here..."><input type="submit" value="search" class="srcbutton" >
					</form>
				<div class="srcclear"></div>
				</div>
			</div>
		</div>
		<?php 
			if(isset($success_message)) {echo $success_message;}
			else {
				echo '
					<div class="holecontainer" style="float: right; margin-right: 36%; padding-top: 26px;">
						<div class="container">
							<div>
								<div>
									<div class="signupform_content">
										<h2>Sign Up Form!</h2>
										<div class="signupform_text"></div>
										<div>
											<form action="" method="POST" class="registration">
												<div class="signup_form">
													<div>
														<td >
															<input name="first_name" id="first_name" placeholder="First Name" required="required" class="first_name signupbox" type="text" size="30" value="'.$u_fname.'" >
														</td>
													</div>
													<div>
														<td >
															<input name="last_name" id="last_name" placeholder="Last Name" required="required" class="last_name signupbox" type="text" size="30" value="'.$u_lname.'" >
														</td>
													</div>
													<div>
														<td>
															<input name="email" placeholder="Enter Your Email" required="required" class="email signupbox" type="email" size="30" value="'.$u_email.'">
														</td
			>										</div>
													<div>
														<td>
															<input name="mobile" placeholder="Enter Your Mobile" required="required" class="email signupbox" type="text" size="30" value="'.$u_mobile.'">
														</td>
													</div>
													<div>
														<td>
															<input name="signupaddress" placeholder="Write Your Full Address" required="required" class="email signupbox" type="text" size="30" value="'.$u_address.'">
														</td>
													</div>
													<div>
														<td>
															<input name="password" id="password-1" required="required"  placeholder="Enter New Password" class="password signupbox " type="password" size="30" value="'.$u_pass.'">
														</td>
													</div>
													<div>
														<input name="signup" class="uisignupbutton signupbutton" type="submit" value="Sign Me Up!">
													</div>
													<div class="signup_error_msg">';
														
															if (isset($error_message)) {echo $error_message;}
															
														
													echo'</div>
												</div>
											</form>
											
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				';
			}

		 ?>
	</body>
</html>

//connect_inc.php 
<?php 
	mysql_connect("localhost","root","") or die("Couldn't connet to SQL server");
	mysql_select_db("ebuybd") or die("Couldn'ttt select DB");
?>

//addproduct.php 
<?php include ( "../inc/connect.inc.php" ); ?>
<?php 
ob_start();
session_start();
if (!isset($_SESSION['admin_login'])) {
	header("location: login.php");
	$user = "";
}
else {
	$user = $_SESSION['admin_login'];
	$result = mysql_query("SELECT * FROM admin WHERE id='$user'");
		$get_user_email = mysql_fetch_assoc($result);
			$uname_db = $get_user_email['firstName'];
}
$pname = "";
$price = "";
$available = "";
$category = "";
$type = "";
$item = "";
$pCode = "";
$descri = "";

if (isset($_POST['signup'])) {
//declere veriable
$pname = $_POST['pname'];
$price = $_POST['price'];
$available = $_POST['available'];
$category = $_POST['category'];
$type = $_POST['type'];
$item = $_POST['item'];
$pCode = $_POST['code'];
$descri = $_POST['descri'];
//triming name
$_POST['pname'] = trim($_POST['pname']);

//finding file extention
$profile_pic_name = @$_FILES['profilepic']['name'];
$file_basename = substr($profile_pic_name, 0, strripos($profile_pic_name, '.'));
$file_ext = substr($profile_pic_name, strripos($profile_pic_name, '.'));

if (((@$_FILES['profilepic']['type']=='image/jpeg') || (@$_FILES['profilepic']['type']=='image/png') || (@$_FILES['profilepic']['type']=='image/gif')) && (@$_FILES['profilepic']['size'] < 1000000)) {

	$item = $item;
	if (file_exists("../image/product/$item")) {
		//nothing
	}else {
		mkdir("../image/product/$item");
	}
	
	
	$filename = strtotime(date('Y-m-d H:i:s')).$file_ext;

	if (file_exists("../image/product/$item/".$filename)) {
		echo @$_FILES["profilepic"]["name"]."Already exists";
	}else {
		if(move_uploaded_file(@$_FILES["profilepic"]["tmp_name"], "../image/product/$item/".$filename)){
			$photos = $filename;
			$result = mysql_query("INSERT INTO products(pName,price,description,available,category,type,item,pCode,picture) VALUES ('$_POST[pname]','$_POST[price]','$_POST[descri]','$_POST[available]','$_POST[category]','$_POST[type]','$_POST[item]','$_POST[code]','$photos')");
				header("Location: allproducts.php");
		}else {
			echo "Something Worng on upload!!!";
		}
		//echo "Uploaded and stored in: userdata/profile_pics/$item/".@$_FILES["profilepic"]["name"];
		
		
	}
	}
	else {
		$error_message = 'Add picture!';
	}
}
$search_value = "";

?>


<!doctype html>
<html>
	<head>
		<title>Welcome to ebuybd online shop</title>
		<link rel="stylesheet" type="text/css" href="../css/style.css">
	</head>
	<body class="home-welcome-text" style="background-image: url(../image/homebackgrndimg2.png);">
		<div class="homepageheader">
			<div class="signinButton loginButton">
				<div class="uiloginbutton signinButton loginButton" style="margin-right: 40px;">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="logout.php">LOG OUT</a>';
						}
					 ?>
					
				</div>
				<div class="uiloginbutton signinButton loginButton">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">Hi '.$uname_db.'</a>';
						}
						else {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">LOG IN</a>';
						}
					 ?>
				</div>
			</div>
			<div style="float: left; margin: 5px 0px 0px 23px;">
				<a href="index.php">
					<img style=" height: 75px; width: 130px;" src="../image/ebuybdlogo.png">
				</a>
			</div>
			<div id="srcheader">
				<form id="newsearch" method="get" action="search.php">
				        <?php 
				        	echo '<input type="text" class="srctextinput" name="keywords" size="21" maxlength="120"  placeholder="Search Here..." value="'.$search_value.'"><input type="submit" value="search" class="srcbutton" >';
				         ?>
				</form>
			<div class="srcclear"></div>
			</div>
		</div>
		<div class="categolis">
			<table>
				<tr>
					<th>
						<a href="index.php" style="text-decoration: none;color: #fff;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Home</a>
					</th>
					<th><a href="addproduct.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #24bfae;border-radius: 12px;">Add Product</a></th>
					<th><a href="newadmin.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">New Admin</a></th>
					<th><a href="allproducts.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">All Products</a></th>
					<th><a href="orders.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Orders</a></th>
				</tr>
			</table>
		</div>
		<?php 
			if(isset($success_message)) {echo $success_message;}
			else {
				echo '
					<div class="holecontainer" style="float: right; margin-right: 36%; padding-top: 20px;">
						<div class="container">
							<div>
								<div>
									<div class="signupform_content">
										<h2>Add Product Form!</h2>
										<div class="signup_error_msg">';
											if (isset($error_message)) {echo $error_message;}
										echo '</div>
										<div class="signupform_text"></div>
										<div>
											<form action="" method="POST" class="registration" enctype="multipart/form-data">
												<div class="signup_form">
													<div>
														<td >
															<input name="pname" id="first_name" placeholder="Product Name" required="required" class="first_name signupbox" type="text" size="30" value="'.$pname.'" >
														</td>
													</div>
													<div>
														<td >
															<input name="price" id="last_name" placeholder="Price" required="required" class="last_name signupbox" type="text" size="30" value="'.$price.'" >
														</td>
													</div>
													<div>
														<td>
															<input name="available" placeholder="Available Quantity" required="required" class="email signupbox" type="text" size="30" value="'.$available.'">
														</td>
													</div>
													<div>
														<td >
															<input name="descri" id="first_name" placeholder="Description" required="required" class="first_name signupbox" type="text" size="30" value="'.$descri.'" >
														</td>
													</div>
													<div>
														<td>
															<select name="category" required="required" style=" font-size: 20px;
														font-style: italic;margin-bottom: 3px;margin-top: 0px;padding: 14px;line-height: 25px;border-radius: 4px;border: 1px solid #169E8F;color: #169E8F;margin-left: 0;width: 300px;background-color: transparent;" class="">
																<option selected value="women">Women</option>
															</select>
														</td>
													</div>
													<div>
														<select name="type" required="required" style=" font-size: 20px;
														font-style: italic;margin-bottom: 3px;margin-top: 0px;padding: 14px;line-height: 25px;border-radius: 4px;border: 1px solid #169E8F;color: #169E8F;margin-left: 0;width: 300px;background-color: transparent;" class="">
																<option selected value="clothing">Clothing</option>
																<option value="other">Other</option>
															</select>
													</div>
													<div>
														<td>
															<select name="item" required="required" style=" font-size: 20px;
														font-style: italic;margin-bottom: 3px;margin-top: 0px;padding: 14px;line-height: 25px;border-radius: 4px;border: 1px solid #169E8F;color: #169E8F;margin-left: 0;width: 300px;background-color: transparent;" class="">
																<option selected value="saree">Saree</option>
																<option value="ornament">Ornaments</option>
																<option value="watch">Watch</option>
																<option value="tshirt">T-Shirt</option>
																<option value="hijab">Hijab</option>
																<option value="perfume">Perfume</option>
																<option value="footwear">Footwear</option>
																<option value="toiletry">Toiletry</option>
															</select>
														</td>
													</div>
													<div>
														<td>
															<input name="code" id="password-1" required="required"  placeholder="Code" class="password signupbox " type="text" size="30" value="'.$pCode.'">
														</td>
													</div>
													<div>
														<td>
															<input name="profilepic" class="password signupbox" type="file" value="Add Pic">
														</td>
													</div>
													<div>
														<input name="signup" class="uisignupbutton signupbutton" type="submit" value="Add Product">
													</div>
												</div>
											</form>
											
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				';
			}

		 ?>
	</body>
</html>

//allproducts.php 
<?php include ( "../inc/connect.inc.php" ); ?>
<?php 
ob_start();
session_start();
if (!isset($_SESSION['admin_login'])) {
	header("location: login.php");
	$user = "";
}
else {
	$user = $_SESSION['admin_login'];
	$result = mysql_query("SELECT * FROM admin WHERE id='$user'");
		$get_user_email = mysql_fetch_assoc($result);
			$uname_db = $get_user_email['firstName'];
}

$search_value = "";

?>


<!doctype html>
<html>
	<head>
		<title>Welcome to ebuybd online shop</title>
		<link rel="stylesheet" type="text/css" href="../css/style.css">
	</head>
	<body class="home-welcome-text" style="background-image: url(../image/homebackgrndimg2.png);">
		<div class="homepageheader">
			<div class="signinButton loginButton">
				<div class="uiloginbutton signinButton loginButton" style="margin-right: 40px;">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="logout.php">LOG OUT</a>';
						}
					 ?>
					
				</div>
				<div class="uiloginbutton signinButton loginButton">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">Hi '.$uname_db.'</a>';
						}
						else {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">LOG IN</a>';
						}
					 ?>
				</div>
			</div>
			<div style="float: left; margin: 5px 0px 0px 23px;">
				<a href="index.php">
					<img style=" height: 75px; width: 130px;" src="../image/ebuybdlogo.png">
				</a>
			</div>
			<div class="">
				<div id="srcheader">
					<form id="newsearch" method="get" action="http://www.google.com">
					        <input type="text" class="srctextinput" name="q" size="21" maxlength="120"  placeholder="Search Here..."><input type="submit" value="search" class="srcbutton" >
					</form>
				<div class="srcclear"></div>
				</div>
			</div>
		</div>
		<div class="categolis">
			<table>
				<tr>
					<th>
						<a href="index.php" style="text-decoration: none;color: #fff;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Home</a>
					</th>
					<th><a href="addproduct.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Add Product</a></th>
					<th><a href="newadmin.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">New Admin</a></th>
					<th><a href="allproducts.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #24bfae;border-radius: 12px;">All Products</a></th>
					<th><a href="orders.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Orders</a></th>
				</tr>
			</table>
		</div>
		<div>
			<table class="rightsidemenu">
				<tr style="font-weight: bold;" colspan="10" bgcolor="#4DB849">
					<th>Id</th>
					<th>P Name</th>
					<th>Description</th>
					<th>Price</th>
					<th>Available</th>
					<th>Category</th>
					<th>Type</th>
					<th>Item</th>
					<th>P Code</th>
					<th>Edit</th>
				</tr>
				<tr>
					<?php include ( "../inc/connect.inc.php");
					$query = "SELECT * FROM products ORDER BY id DESC";
					$run = mysql_query($query);
					while ($row=mysql_fetch_assoc($run)) {
						$id = $row['id'];
						$pName = substr($row['pName'], 0,50);
						$descri = $row['description'];
						$price = $row['price'];
						$available = $row['available'];
						$category = $row['category'];
						$type = $row['type'];
						$item = $row['item'];
						$pCode = $row['pCode'];
						$picture = $row['picture'];
					
					 ?>
					<th><?php echo $id; ?></th>
					<th><?php echo $pName; ?></th>
					<th><?php echo $descri; ?></th>
					<th><?php echo $price; ?></th>
					<th><?php echo $available; ?></th>
					<th><?php echo $category; ?></th>
					<th><?php echo $type; ?></th>
					<th><?php echo $item; ?></th>
					<th><?php echo $pCode; ?></th>
					<th><?php echo '<div class="home-prodlist-img"><a href="editproduct.php?epid='.$id.'">
									<img src="../image/product/'.$item.'/'.$picture.'" class="home-prodlist-imgi" style="height: 75px; width: 75px;">
									</a>
								</div>' ?></th>
				</tr>
				<?php } ?>
			</table>
		</div>
	</body>
</html>

//orders.php 
<?php include ( "../inc/connect.inc.php" ); ?>
<?php 
ob_start();
session_start();
if (!isset($_SESSION['admin_login'])) {
	header("location: login.php");
	$user = "";
}
else {
	$user = $_SESSION['admin_login'];
	$result = mysql_query("SELECT * FROM admin WHERE id='$user'");
		$get_user_email = mysql_fetch_assoc($result);
			$uname_db = $get_user_email['firstName'];
}

?>


<!doctype html>
<html>
	<head>
		<title>Welcome to ebuybd online shop</title>
		<link rel="stylesheet" type="text/css" href="../css/style.css">
	</head>
	<body class="home-welcome-text" style="background-image: url(../image/homebackgrndimg2.png);">
		<div class="homepageheader">
			<div class="signinButton loginButton">
				<div class="uiloginbutton signinButton loginButton" style="margin-right: 40px;">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="logout.php">LOG OUT</a>';
						}
					 ?>
					
				</div>
				<div class="uiloginbutton signinButton loginButton">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">Hi '.$uname_db.'</a>';
						}
						else {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">LOG IN</a>';
						}
					 ?>
				</div>
			</div>
			<div style="float: left; margin: 5px 0px 0px 23px;">
				<a href="index.php">
					<img style=" height: 75px; width: 130px;" src="../image/ebuybdlogo.png">
				</a>
			</div>
			<div class="">
				<div id="srcheader">
					<form id="newsearch" method="get" action="http://www.google.com">
					        <input type="text" class="srctextinput" name="q" size="21" maxlength="120"  placeholder="Search Here..."><input type="submit" value="search" class="srcbutton" >
					</form>
				<div class="srcclear"></div>
				</div>
			</div>
		</div>
		<div class="categolis">
			<table>
				<tr>
					<th>
						<a href="index.php" style="text-decoration: none;color: #fff;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Home</a>
					</th>
					<th><a href="addproduct.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Add Product</a></th>
					<th><a href="newadmin.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">New Admin</a></th>
					<th><a href="allproducts.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">All Products</a></th>
					<th><a href="orders.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #24bfae;border-radius: 12px;">Orders</a></th>
				</tr>
			</table>
		</div>
		<div>
			<table class="rightsidemenu">
				<tr style="font-weight: bold;" colspan="10" bgcolor="#4DB849">
					<th>Id</th>
					<th>User Id</th>
					<th>Product Id</th>
					<th>Q*P=T</th>
					<th>Order Place</th>
					<th>Mobile</th>
					<th>Order Status</th>
					<th>Order Date</th>
					<th>Delevery Date</th>
					<th>User Name</th>
					<th>User Mobile</th>
					<th>User Email</th>
					<th>Edit</th>
				</tr>
				<tr>
					<?php include ( "../inc/connect.inc.php");
					$query = "SELECT * FROM orders ORDER BY id DESC";
					$run = mysql_query($query);
					while ($row=mysql_fetch_assoc($run)) {
						$oid = $row['id'];
						$ouid = $row['uid'];
						$opid = $row['pid'];
						$oquantity = $row['quantity'];
						$oplace = $row['oplace'];
						$omobile = $row['mobile'];
						$odstatus = $row['dstatus'];
						$odate = $row['odate'];
						$ddate = $row['ddate'];
						//getting user info
						$query1 = "SELECT * FROM user WHERE id='$ouid'";
						$run1 = mysql_query($query1);
						$row1=mysql_fetch_assoc($run1);
						$ofname = $row1['firstName'];
						$oumobile = $row1['mobile'];
						$ouemail = $row1['email'];

						//product info
						$query2 = "SELECT * FROM products WHERE id='$opid'";
						$run2 = mysql_query($query2);
						$row2=mysql_fetch_assoc($run2);
						$opcate = $row2['category'];
						$opitem = $row2['item'];
						$oppicture = $row2['picture'];
						$oprice = $row2['price'];

					
					 ?>
					<th><?php echo $oid; ?></th>
					<th><?php echo $ouid; ?></th>
					<th><?php echo $opid; ?></th>
					<th><?php echo ''.$oquantity.' * '.$oprice.' = '.$oquantity*$oprice.''; ?></th>
					<th><?php echo $oplace; ?></th>
					<th><?php echo $omobile; ?></th>
					<th><?php echo $odstatus; ?></th>
					<th><?php echo $odate; ?></th>
					<th><?php echo $ddate; ?></th>

					<th><?php echo $ofname; ?></th>
					<th><?php echo $oumobile; ?></th>
					<th><?php echo $ouemail; ?></th>
					<th><?php echo '<div class="home-prodlist-img"><a href="editorder.php?eoid='.$oid.'">
									<img src="../image/product/'.$opitem.'/'.$oppicture.'" class="home-prodlist-imgi" style="height: 75px; width: 75px;">
									</a>
								</div>' ?></th>
				</tr>
				<?php } ?>
			</table>
		</div>
	</body>
</html>

//search.php 
<?php include ( "../inc/connect.inc.php" ); ?>
<?php 
ob_start();
session_start();
if (!isset($_SESSION['admin_login'])) {
	header("location: login.php");
	$user = "";
}
else {
	$user = $_SESSION['admin_login'];
	$result = mysql_query("SELECT * FROM admin WHERE id='$user'");
		$get_user_email = mysql_fetch_assoc($result);
			$uname_db = $get_user_email['firstName'];
}
if (isset($_REQUEST['keywords'])) {

	$epid = mysql_real_escape_string($_REQUEST['keywords']);
	if($epid != "" && ctype_alnum($epid)){
		
	}else {
		header('location: index.php');
	}
}else {
	header('location: index.php');
}

$search_value = "";
$search_value = trim($_GET['keywords']);
?>


<!doctype html>
<html>
	<head>
		<title>Welcome to ebuybd online shop</title>
		<link rel="stylesheet" type="text/css" href="../css/style.css">
	</head>
	<body class="home-welcome-text" style="background-image: url(../image/homebackgrndimg2.png);">
		<div class="homepageheader">
			<div class="signinButton loginButton">
				<div class="uiloginbutton signinButton loginButton" style="margin-right: 40px;">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="logout.php">LOG OUT</a>';
						}
					 ?>
					
				</div>
				<div class="uiloginbutton signinButton loginButton">
					<?php 
						if ($user!="") {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">Hi '.$uname_db.'</a>';
						}
						else {
							echo '<a style="text-decoration: none;color: #fff;" href="login.php">LOG IN</a>';
						}
					 ?>
				</div>
			</div>
			<div style="float: left; margin: 5px 0px 0px 23px;">
				<a href="index.php">
					<img style=" height: 75px; width: 130px;" src="../image/ebuybdlogo.png">
				</a>
			</div>
			<div class="">
			<div id="srcheader">
				<form id="newsearch" method="get" action="search.php">
				        <?php 
				        	echo '<input type="text" class="srctextinput" name="keywords" size="21" maxlength="120"  placeholder="Search Here..." value="'.$search_value.'"><input type="submit" value="search" class="srcbutton" >';
				         ?>
				</form>
			<div class="srcclear"></div>
			</div>
		</div>
		</div>
		<div class="categolis">
			<table>
				<tr>
					<th>
						<a href="index.php" style="text-decoration: none;color: #fff;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Home</a>
					</th>
					<th><a href="addproduct.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Add Product</a></th>
					<th><a href="newadmin.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">New Admin</a></th>
					<th><a href="allproducts.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">All Products</a></th>
					<th><a href="orders.php" style="text-decoration: none;color: #ddd;padding: 4px 12px;background-color: #c7587e;border-radius: 12px;">Orders</a></th>
				</tr>
			</table>
		</div>
		<div>
			<table class="rightsidemenu">
				<tr style="font-weight: bold;" colspan="10" bgcolor="#4DB849">
					<th>Id</th>
					<th>P Name</th>
					<th>Description</th>
					<th>Price</th>
					<th>Available</th>
					<th>Category</th>
					<th>Type</th>
					<th>Item</th>
					<th>P Code</th>
					<th>Edit</th>
				</tr>
				<tr>
					<?php include ( "../inc/connect.inc.php");
					$search_value = trim($_GET['keywords']);
					$query = "SELECT * FROM products WHERE pName like '%$search_value%'  ORDER BY id DESC";
					$run = mysql_query($query);
					if ( $total = mysql_num_rows($run)) {
					while ($row=mysql_fetch_assoc($run)) {
						$id = $row['id'];
						$pName = substr($row['pName'], 0,50);
						$descri = $row['description'];
						$price = $row['price'];
						$available = $row['available'];
						$category = $row['category'];
						$type = $row['type'];
						$item = $row['item'];
						$pCode = $row['pCode'];
						$picture = $row['picture'];
					
					 ?>
					<th><?php echo $id; ?></th>
					<th><?php echo $pName; ?></th>
					<th><?php echo $descri; ?></th>
					<th><?php echo $price; ?></th>
					<th><?php echo $available; ?></th>
					<th><?php echo $category; ?></th>
					<th><?php echo $type; ?></th>
					<th><?php echo $item; ?></th>
					<th><?php echo $pCode; ?></th>
					<th><?php echo '<div class="home-prodlist-img"><a href="editproduct.php?epid='.$id.'">
									<img src="../image/product/'.$item.'/'.$picture.'" class="home-prodlist-imgi" style="height: 75px; width: 75px;">
									</a>
								</div>' ?></th>
				</tr>
				<?php }
					}
				 ?>
			</table>
		</div>
	</body>
</html>

//sql file

-- phpMyAdmin SQL Dump
-- version 4.2.11
-- http://www.phpmyadmin.net
--
-- Host: 127.0.0.1
-- Generation Time: Apr 11, 2017 at 06:48 AM
-- Server version: 5.6.21
-- PHP Version: 5.6.3

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;

--
-- Database: `ebuybd`
--

-- --------------------------------------------------------

--
-- Table structure for table `admin`
--

CREATE TABLE IF NOT EXISTS `admin` (
`id` int(11) NOT NULL,
  `firstName` varchar(125) NOT NULL,
  `lastName` varchar(125) NOT NULL,
  `email` varchar(100) NOT NULL,
  `mobile` varchar(25) NOT NULL,
  `address` text NOT NULL,
  `password` varchar(100) NOT NULL,
  `type` varchar(20) NOT NULL,
  `confirmCode` varchar(10) NOT NULL
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=latin1;

--
-- Dumping data for table `admin`
--

INSERT INTO `admin` (`id`, `firstName`, `lastName`, `email`, `mobile`, `address`, `password`, `type`, `confirmCode`) VALUES
(3, 'Borsha', 'Tasnim', 'borsha@gmail.com', '01678293748', 'Dhaka, Bangladesh', 'aa030295ae26e8acbd3d1c9415a60f12', 'manager', '117631');

-- --------------------------------------------------------

--
-- Table structure for table `orders`
--

CREATE TABLE IF NOT EXISTS `orders` (
`id` int(11) NOT NULL,
  `uid` int(11) NOT NULL,
  `pid` int(11) NOT NULL,
  `quantity` int(11) NOT NULL,
  `oplace` text NOT NULL,
  `mobile` varchar(15) NOT NULL,
  `dstatus` varchar(10) NOT NULL DEFAULT 'no',
  `odate` date NOT NULL,
  `ddate` date NOT NULL
) ENGINE=InnoDB AUTO_INCREMENT=30 DEFAULT CHARSET=latin1;

--
-- Dumping data for table `orders`
--

INSERT INTO `orders` (`id`, `uid`, `pid`, `quantity`, `oplace`, `mobile`, `dstatus`, `odate`, `ddate`) VALUES
(1, 7, 28, 0, 'Manikganj Sadar', '01677531881', 'no', '2017-04-07', '0000-00-00'),
(2, 7, 31, 0, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'yes', '2017-04-07', '0000-00-00'),
(4, 7, 26, 0, 'South Seota, Manikganj Sadar', '01677531881', 'no', '2017-04-07', '0000-00-00'),
(9, 7, 44, 1, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'no', '2017-04-08', '0000-00-00'),
(10, 7, 44, 3, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'Yes', '2017-04-08', '0000-00-00'),
(13, 7, 11, 2, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'Cancel', '2017-04-08', '0000-00-00'),
(14, 7, 40, 1, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'no', '2017-04-08', '0000-00-00'),
(15, 7, 43, 1, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'no', '2017-04-08', '0000-00-00'),
(16, 7, 29, 3, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'no', '2017-04-09', '0000-00-00'),
(17, 11, 35, 1, 'Saver, Dhaka', '01678293748', 'no', '2017-04-09', '0000-00-00'),
(18, 7, 31, 1, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'no', '2017-04-09', '0000-00-00'),
(19, 13, 43, 3, 'asdfas', '789', 'Yes', '2017-04-09', '0000-00-00'),
(20, 13, 29, 1, 'asdfas', '789', 'Yes', '2017-04-09', '2017-04-14'),
(21, 7, 43, 1, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'no', '2017-04-09', '0000-00-00'),
(22, 11, 45, 1, 'Saver, Dhaka', '01678293748', 'no', '2017-04-09', '0000-00-00'),
(23, 11, 32, 1, 'Saver, Dhaka', '01678293748', 'no', '2017-04-10', '0000-00-00'),
(24, 11, 32, 1, 'Saver, Dhaka', '01678293748', 'no', '2017-04-10', '0000-00-00'),
(25, 11, 51, 1, 'Saver, Dhaka', '01678293748', 'no', '2017-04-10', '0000-00-00'),
(26, 11, 29, 2, 'Saver, Dhaka', '01678293748', 'Cancel', '2017-04-10', '0000-00-00'),
(27, 7, 43, 1, 'Nikunja 2, Khilkhet, Dhaka', '01677531881', 'no', '2017-04-10', '0000-00-00'),
(28, 11, 29, 1, 'Saver, Dhaka', '01678293748', 'no', '2017-04-10', '2017-04-11'),
(29, 11, 43, 1, 'Saver, Dhaka', '01678293748', 'no', '2017-04-10', '2017-06-12');

-- --------------------------------------------------------

--
-- Table structure for table `products`
--

CREATE TABLE IF NOT EXISTS `products` (
`id` int(11) NOT NULL,
  `pName` varchar(100) NOT NULL,
  `price` int(11) NOT NULL,
  `description` text NOT NULL,
  `available` int(11) NOT NULL,
  `category` varchar(100) NOT NULL,
  `type` varchar(100) NOT NULL,
  `item` varchar(100) NOT NULL,
  `pCode` varchar(20) NOT NULL,
  `picture` text NOT NULL
) ENGINE=InnoDB AUTO_INCREMENT=54 DEFAULT CHARSET=latin1;

--
-- Dumping data for table `products`
--

INSERT INTO `products` (`id`, `pName`, `price`, `description`, `available`, `category`, `type`, `item`, `pCode`, `picture`) VALUES
(3, 'Metal-Collar-Bone-Chain-Set', 1200, 'Dekhte khub e sundor', 2, 'women', 'Chain Set', 'ornament', 'SR002', 'Women-Neck-Jewelry-Exaggerated-Metal-Collar-Bone-Chain-Set.jpg'),
(4, 'Colors-Women-Embellished-Saree', 1200, 'kichu koilam na', 2, 'women', 'cloathing', 'saree', 'SR002', 'Colors-Women-Embellished-Saree_2.jpg'),
(5, 'Colors-Women-Embellished-Sarees', 1200, 'kichu koilam na', 2, 'women', 'cloathing', 'saree', 'SR002', 'Colors-Women-Embellished-Saree_3.jpg'),
(6, 'Jewelry-Color-diamond-Love-font', 2000, 'Dekhte khub e sundor', 2, 'women', 'Chain Set', 'ornament', 'SR002', 'Jewelry-Color-created-created-diamond-Love-font-b.jpg'),
(7, 'Jewelry-full metal earring', 2000, 'Dekhte khub e sundor', 2, 'women', 'earring', 'ornament', 'SR002', 'earrings-online-for-women-4.jpg'),
(10, 'Boutique Saree 1', 1300, 'Osthir mama!', 4, 'women', 'cloathing', 'saree', 'SR001', 'Saree Red Color 1.jpg'),
(11, 'fancy-look-attractive-saree-2-original', 1200, 'kichu koilam na', 2, 'women', 'cloathing', 'saree', 'SR002', 'new-designer-fancy-look-attractive-saree-2-original.jpg'),
(26, 'Watches-for-women-5', 700, 'à¦¸à§à¦¨à§à¦¦à¦°, à¦†à¦•à¦°à§à¦·à¦£à§€à§Ÿ', 2, 'women', 'No', 'watch', 'W234', '1491496755.jpg'),
(27, 'Diamond-Setting-Watch-3', 700, '2016-New-Electronic-Style-Women-Dress-New-Fashion-Watches-Imitation-Diamond-Setting-Watch-Casual-Wrist-Watch-3', 3, 'women', 'yes', 'watch', 'W234', '1491496880.jpg'),
(28, 'Steel-Bracelet 2', 399, 'Steel-Bracelet-Women-Watches-Designs', 2, 'women', 'no', 'watch', 'W345', '1491496956.jpg'),
(29, 'Steel-Bracelet-Women-3', 1000, 'Steel-Bracelet-Women-Watches', 3, 'women', 'yes', 'watch', 'W345', '1491497102.jpg'),
(30, 'Right-hand-rings 2', 200, 'Right-hand-rings-collection-ornament-champagne-diamond-ring-for-women_mainro', 3, 'women', 'ok', 'ornament', 'O234', '1491497201.jpg'),
(31, 'floral-ornament-ring-3d', 1200, 'floral-ornament-ring-3d-model-stl-3dm', 3, 'women', 'ok', 'ornament', 'O254', '1491497263.jpg'),
(32, 'Nekles set', 799, 'Good, Awesome', 2, 'women', 'no', 'ornament', 'O2352', '1491497316.jpg'),
(33, 'T Shirt 1', 250, 'Nice', 10, 'women', 'no', 'tshirt', 'TS252', '1491497478.jpg'),
(34, 'T Shirt 2', 400, 'Nice Looking', 12, 'women', 'no', 'tshirt', 'TS3463', '1491497528.jpg'),
(35, 'T Shirt 3', 299, 'Nice', 27, 'women', 'no', 'tshirt', 'TS345', '1491497588.jpg'),
(36, 'T Shirt 4', 890, 'Nice, good', 30, 'women', 'no', 'tshirt', 'TS2354', '1491497644.jpg'),
(37, 'CHARCOAL_SOFT_G_HIJAB', 700, 'CHARCOAL_SOFT_G_HIJAB', 3, 'women', 'no', 'hijab', 'H98', '1491498074.jpg'),
(38, 'HijabScarf (20)', 400, 'HijabScarf ', 23, 'women', 'no', 'hijab', 'H3254', '1491498134.JPG'),
(39, 'Hijab 3', 600, 'hijab-2013', 20, 'women', 'no', 'hijab', 'H2354', '1491498202.jpg'),
(40, 'Hijab 4', 800, 'nice and cool', 23, 'women', 'no', 'hijab', 'H233', '1491498250.jpg'),
(41, 'vera_wang_princess', 2000, 'vera_wang_princess_perfume_for_women', 10, 'women', 'no', 'perfume', 'P2354', '1491498354.jpg'),
(42, 'Perfume 1', 150, 'top-perfumes-for-women', 2, 'women', 'no', 'perfume', 'P345', '1491498398.jpg'),
(43, 'Perfume 2', 3000, 'Awesome smell', 6, 'women', 'no', 'perfume', 'P252', '1491649246.jpg'),
(44, 'Perfume 3', 5000, 'Nice', 3, 'women', 'no', 'perfume', 'P254', '1491498508.jpg'),
(45, 'Latest-Fancy 2', 5000, 'Latest-Fancy-Ladiess-Shoes-Designs-2014', 300, 'kidsmom', 'clothing', 'footwear', 'S32543', '1491498848.jpg'),
(49, 'Soap 1', 80, 'Nice', 20, 'women', 'no', 'toiletry', 'SP234', '1491499503.jpg'),
(51, 'New Perfume 4', 4453, 'asdfa', 34, 'women', 'clothing', 'watch', 'asdf78', '1491707164.jpg'),
(52, 'Sareesf', 453354, 'asdfa', 342, 'women', 'clothing', 'perfume', 'S56', '1491850298.jpg'),
(53, 'Soap 3', 345, 'Cool', 30, 'women', 'clothing', 'toiletry', 'S789', '1491850339.jpg');

-- --------------------------------------------------------

--
-- Table structure for table `user`
--

CREATE TABLE IF NOT EXISTS `user` (
`id` int(11) NOT NULL,
  `firstName` varchar(25) NOT NULL,
  `lastName` varchar(25) NOT NULL,
  `email` varchar(100) NOT NULL,
  `mobile` varchar(20) NOT NULL,
  `address` varchar(120) NOT NULL,
  `password` varchar(100) NOT NULL,
  `confirmCode` varchar(10) NOT NULL,
  `activation` varchar(10) NOT NULL DEFAULT 'no'
) ENGINE=InnoDB AUTO_INCREMENT=12 DEFAULT CHARSET=latin1;

--
-- Dumping data for table `user`
--

INSERT INTO `user` (`id`, `firstName`, `lastName`, `email`, `mobile`, `address`, `password`, `confirmCode`, `activation`) VALUES
(9, 'Borsha', 'Tanjina', 'Tanjina@gmail.com', '01578399283', 'Dhaka, Bangladesh', 'aa030295ae26e8acbd3d1c9415a60f12', '217576', 'yes'),
(10, 'Trisha', 'Rehman', 'trisha@gmail.com', '01923457834', 'Mirpur 2, Dhaka', '5af7a513a7c48f6cc97253254b29509b', '0', 'yes'),
(11, 'Akhi', 'Alam', 'akhi@gmail.com', '01678293748', 'Saver, Dhaka', 'ca52febd8be7c4480ae90cdae8438a03', '0', 'yes');

--
-- Indexes for dumped tables
--

--
-- Indexes for table `admin`
--
ALTER TABLE `admin`
 ADD PRIMARY KEY (`id`);

--
-- Indexes for table `orders`
--
ALTER TABLE `orders`
 ADD PRIMARY KEY (`id`);

--
-- Indexes for table `products`
--
ALTER TABLE `products`
 ADD PRIMARY KEY (`id`);

--
-- Indexes for table `user`
--
ALTER TABLE `user`
 ADD PRIMARY KEY (`id`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `admin`
--
ALTER TABLE `admin`
MODIFY `id` int(11) NOT NULL AUTO_INCREMENT,AUTO_INCREMENT=4;
--
-- AUTO_INCREMENT for table `orders`
--
ALTER TABLE `orders`
MODIFY `id` int(11) NOT NULL AUTO_INCREMENT,AUTO_INCREMENT=30;
--
-- AUTO_INCREMENT for table `products`
--
ALTER TABLE `products`
MODIFY `id` int(11) NOT NULL AUTO_INCREMENT,AUTO_INCREMENT=54;
--
-- AUTO_INCREMENT for table `user`
--
ALTER TABLE `user`
MODIFY `id` int(11) NOT NULL AUTO_INCREMENT,AUTO_INCREMENT=12;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;