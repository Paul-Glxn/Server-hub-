<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <title>Minecraft Server Webinterface</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #121212;
      color: #fff;
      margin: 0;
      padding: 40px;
      text-align: center;
    }

    .container {
      max-width: 700px;
      margin: auto;
      padding: 30px;
      background-color: #1e1e1e;
      border-radius: 12px;
      box-shadow: 0 0 15px rgba(0,0,0,0.4);
    }

    h1 {
      margin-bottom: 20px;
    }

    select, input[type="number"] {
      padding: 8px;
      font-size: 16px;
      margin: 5px;
      border-radius: 5px;
      border: none;
    }

    button {
      padding: 10px 20px;
      margin: 10px;
      font-size: 16px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      color: white;
    }

    button.start {
      background-color: #4CAF50;
    }

    button.stop {
      background-color: #f44336;
    }

    #status {
      margin-top: 20px;
      font-weight: bold;
    }

    pre {
      background: #000;
      color: #0f0;
      text-align: left;
      padding: 15px;
      height: 200px;
      overflow-y: auto;
      border-radius: 8px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>🟢 Minecraft Server Panel</h1>

    <label>🧠 RAM (MB):</label>
    <select id="ram">
      <option value="1024">1 GB</option>
      <option value="2048">2 GB</option>
      <option value="4096">4 GB</option>
      <option value="8192">8 GB</option>
      <option value="16384" selected>16 GB</option>
    </select><br>

    <label>👥 Slots:</label>
    <input type="number" id="slots" value="20" min="1" max="100"><br>

    <button class="start" onclick="startServer()">🚀 Server starten</button>
    <button class="stop" onclick="stopServer()">🛑 Server stoppen</button>

    <div id="status">Status: ⏳ Warten auf Aktion...</div>

    <h3>🔧 Server-Konsole</h3>
    <pre id="log">[LOG] Serverausgabe wird geladen...</pre>
  </div>

  <script>
    function startServer() {
      const ram = document.getElementById('ram').value;
      const slots = document.getElementById('slots').value;
      fetch(`http://localhost:3000/start?ram=${ram}&slots=${slots}`)
        .then(res => res.text())
        .then(text => {
          document.getElementById('status').innerText = '✅ ' + text;
        })
        .catch(() => {
          document.getElementById('status').innerText = '❌ Fehler beim Start';
        });
    }

    function stopServer() {
      fetch('http://localhost:3000/stop')
        .then(res => res.text())
        .then(text => {
          document.getElementById('status').innerText = '🛑 ' + text;
        })
        .catch(() => {
          document.getElementById('status').innerText = '❌ Fehler beim Stoppen';
        });
    }

    function fetchLogs() {
      fetch('http://localhost:3000/logs')
        .then(res => res.text())
        .then(logs => {
          document.getElementById('log').innerText = logs;
        });
    }

    setInterval(fetchLogs, 3000);
  </script>
</body>
</html>
