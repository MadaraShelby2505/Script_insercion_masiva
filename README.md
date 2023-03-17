<?php

// Conexión a la base de datos
$host = "localhost";
$user = "root";
$password = "";
$dbname = "tbl_usuario";

$conn = mysqli_connect($host, $user, $password, $dbname);

// Configuración de la base de datos
mysqli_query($conn, "SET NAMES 'utf8mb4'");
mysqli_query($conn, "SET AUTOCOMMIT=0");
mysqli_query($conn, "SET unique_checks=0");
mysqli_query($conn, "SET foreign_key_checks=0");

// Generación de los datos
$start = microtime(true);
for ($i = 1; $i <= 1000000; $i++) {
    $name = generateName();
    $age = generateAge();
    $email = generateEmail($name);
    $sql = "INSERT INTO tbl_usuario (name, age, email) VALUES ('$name', $age, '$email')";
    mysqli_query($conn, $sql);
}
$end = microtime(true);

// Confirmación de los datos
mysqli_query($conn, "COMMIT");
mysqli_query($conn, "SET unique_checks=1");
mysqli_query($conn, "SET foreign_key_checks=1");

// Cierre de la conexión a la base de datos
mysqli_close($conn);

// Resultados
echo "Tiempo de inserción: " . ($end - $start) . " segundos";

// Funciones auxiliares
function generateName() {
    $first_names = array("Juan", "María", "Pedro", "Ana", "Luis", "Elena", "Carlos", "Laura", "Jorge", "Isabel");
    $last_names = array("García", "Martínez", "López", "González", "Rodríguez", "Fernández", "Gómez", "Ruiz", "Pérez", "Sánchez");
    $first_name = $first_names[array_rand($first_names)];
    $last_name = $last_names[array_rand($last_names)];
    return "$first_name $last_name";
}

function generateAge() {
    return rand(18, 60);
}

function generateEmail($name) {
    $name = str_replace(" ", ".", strtolower($name));
    $domain = array("gmail.com", "yahoo.com", "hotmail.com", "outlook.com", "aol.com");
    $suffix = $domain[array_rand($domain)];
    return "$name@$suffix";
}
