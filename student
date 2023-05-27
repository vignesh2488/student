<?php
session_start();

// Define your database connection details
$dbHost = 'your_database_host';
$dbUser = 'your_database_username';
$dbPass = 'your_database_password';
$dbName = 'your_database_name';

// Establish database connection
$db = new mysqli($dbHost, $dbUser, $dbPass, $dbName);
if ($db->connect_error) {
    die('Connection failed: ' . $db->connect_error);
}

// Registration form submission
if (isset($_POST['register'])) {
    $name = $_POST['name'];
    $email = $_POST['email'];
    $password = $_POST['password'];

    // Perform form validation here

    // Insert student data into the database
    $stmt = $db->prepare("INSERT INTO students (name, email, password) VALUES (?, ?, ?)");
    $stmt->bind_param("sss", $name, $email, $password);
    $stmt->execute();
    $stmt->close();

    // Redirect to login page after successful registration
    header("Location: login.php");
    exit();
}

// Login form submission
if (isset($_POST['login'])) {
    $email = $_POST['email'];
    $password = $_POST['password'];

    // Perform form validation here

    // Retrieve student data from the database
    $stmt = $db->prepare("SELECT * FROM students WHERE email = ?");
    $stmt->bind_param("s", $email);
    $stmt->execute();
    $result = $stmt->get_result();
    $student = $result->fetch_assoc();
    $stmt->close();

    // Verify password and create session
    if ($student && password_verify($password, $student['password'])) {
        $_SESSION['student_id'] = $student['id'];
        header("Location: profile.php");
        exit();
    } else {
        echo "Invalid email or password.";
    }
}

// Student profile page
if (isset($_SESSION['student_id'])) {
    $studentId = $_SESSION['student_id'];

    // Retrieve student data from the database
    $stmt = $db->prepare("SELECT * FROM students WHERE id = ?");
    $stmt->bind_param("i", $studentId);
    $stmt->execute();
    $result = $stmt->get_result();
    $student = $result->fetch_assoc();
    $stmt->close();

    // Display student information
    if ($student) {
        echo "Welcome, " . $student['name'] . "!<br>";
        echo "Email: " . $student['email'];
    } else {
        echo "Student not found.";
    }
}
?>
