<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Generador de QR y Puntos</title>
  <script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
      text-align: center;
    }
    .container {
      margin-top: 50px;
    }
    .qr-code {
      margin-top: 20px;
      margin-bottom: 30px;
    }
    .points {
      font-size: 20px;
    }
    .button {
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
      margin-top: 20px;
    }
    .button:hover {
      background-color: #45a049;
    }
    .password-section {
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>Bienvenido a nuestro sistema de puntos</h1>
  <div class="container">
    <h2>Tu código QR</h2>
    <div id="qrcode" class="qr-code"></div>
    
    <div class="points">
      <p>Puntos acumulados: <span id="points">0</span></p>
    </div>

    <div class="password-section">
      <input type="password" id="password" placeholder="Contraseña del dueño" />
      <button class="button" onclick="verificarContraseña()">Acceder como dueño</button>
    </div>

    <button id="acumularPuntosBtn" class="button" style="display:none;" onclick="acumularPuntos()">Acumular 10 puntos</button>
  </div>

  <script>
    // Generar un ID único para el cliente
    const clienteId = 'cliente-' + Math.random().toString(36).substr(2, 9);

    // Cargar el cliente desde localStorage si existe
    let cliente = JSON.parse(localStorage.getItem(clienteId));

    if (!cliente) {
      cliente = { points: 0 }; // Si no existe, inicializar los puntos a 0
      localStorage.setItem(clienteId, JSON.stringify(cliente)); // Guardarlo
    }

    // Cargar los puntos almacenados
    document.getElementById('points').innerText = cliente.points;

    // Generar el código QR con el ID único del cliente
    QRCode.toCanvas(document.getElementById('qrcode'), clienteId, function (error) {
      if (error) console.error(error);
    });

    // Contraseña del dueño
    const contraseñaDueño = "dueño123";  // Aquí puedes cambiar la contraseña

    // Función para verificar la contraseña
    function verificarContraseña() {
      const passwordInput = document.getElementById('password').value;
      if (passwordInput === contraseñaDueño) {
        alert("Acceso permitido");
        document.getElementById('acumularPuntosBtn').style.display = 'inline-block';  // Mostrar el botón
      } else {
        alert("Contraseña incorrecta");
      }
    }

    // Función para acumular puntos
    function acumularPuntos() {
      cliente.points += 10; // Acumular 10 puntos
      localStorage.setItem(clienteId, JSON.stringify(cliente)); // Guardar los puntos en localStorage
      document.getElementById('points').innerText = cliente.points;
    }
  </script>
</body>
</html>
