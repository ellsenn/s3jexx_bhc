<?php
include 'config.php';
session_start();

if (isset($_POST['login'])) {
    $username = htmlspecialchars($_POST['username']);
    $password = $_POST['password'];

    $query = "SELECT * FROM users WHERE username=?";
    $stmt  = $conn->prepare($query);
    $stmt->bind_param("s", $username);
    $stmt->execute();
    $result = $stmt->get_result();
    $user   = $result->fetch_assoc();

    if ($user && password_verify($password, $user['password'])) {
        $_SESSION['user'] = $user['username'];
        header("Location: dashboard.php");
    } else {
        echo "Username atau password salah!";
    }
}
?>
<form method="post">
    <h2>Login</h2>
    <input type="text" name="username" placeholder="Username" required><br><br>
    <input type="password" name="password" placeholder="Password" required><br><br>
    <button type="submit" name="login">Login</button>
</form>
