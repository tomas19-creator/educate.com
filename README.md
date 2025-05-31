<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Monitor de Estudio - ESP32</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      padding-top: 40px;
      background-color: #f0f4f8;
    }
    h1 {
      margin-bottom: 20px;
    }
    button {
      font-size: 18px;
      padding: 15px 25px;
      margin: 10px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      color: white;
    }
    .verde { background-color: #4CAF50; }
    .rojo { background-color: #f44336; }
    .azul { background-color: #2196F3; }
    #estado, #tiempo {
      font-size: 20px;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>¿Qué estás haciendo?</h1>
  <button class="verde" onclick="cambiarEstado('estudiando')">Estoy Estudiando</button>
  <button class="rojo" onclick="cambiarEstado('descansando')">No estoy haciendo nada</button>
  <button class="azul" onclick="cambiarEstado('ayuda')">Necesito ayuda</button>

  <div id="estado">Estado actual: ---</div>
  <div id="tiempo">Tiempo estudiando: 0 min</div>

  <script>
    const ipESP32 = "http://192.168.1.100"; // Cambiar por IP real del ESP32

    function cambiarEstado(estado) {
      fetch(`${ipESP32}/estado/${estado}`)
        .then(res => res.json())
        .then(data => {
          document.getElementById("estado").textContent = `Estado actual: ${data.estado}`;
          document.getElementById("tiempo").textContent = `Tiempo estudiando: ${data.minutos_estudio} min`;
        })
        .catch(() => alert("No se pudo comunicar con el ESP32"));
    }

    // Consulta automática cada 10 segundos
    setInterval(() => cambiarEstado("actual"), 10000);
  </script>
</body>
</html>
