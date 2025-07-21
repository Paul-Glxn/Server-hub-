<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Byteblitz Clone - Minecraft Server Hosting</title>
  <style>
    /* Reset & Basics */
    * {
      box-sizing: border-box;
    }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0; padding: 0;
      background: #121212;
      color: #fff;
      line-height: 1.5;
    }
    a {
      color: #4CAF50;
      text-decoration: none;
    }
    a:hover {
      text-decoration: underline;
    }
    button {
      cursor: pointer;
      background-color: #4CAF50;
      border: none;
      padding: 12px 25px;
      border-radius: 6px;
      font-weight: 600;
      color: #fff;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #388E3C;
    }
    input, select {
      padding: 10px;
      margin: 5px 0 15px 0;
      width: 100%;
      border-radius: 6px;
      border: none;
      font-size: 16px;
    }
    label {
      font-weight: 600;
      display: block;
      margin-bottom: 6px;
    }

    /* Container */
    .container {
      max-width: 900px;
      margin: 30px auto;
      padding: 25px;
      background-color: #1E1E1E;
      border-radius: 15px;
      box-shadow: 0 0 20px #000000cc;
    }

    /* Header */
    header {
      text-align: center;
      margin-bottom: 30px;
    }
    header h1 {
      font-size: 2.5rem;
      margin-bottom: 5px;
    }
    header p {
      color: #aaa;
    }

    /* Form Grid */
    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 30px;
    }
    @media (max-width: 700px) {
      .form-grid {
        grid-template-columns: 1fr;
      }
    }

    /* Section Titles */
    h2 {
      margin-top: 0;
      border-bottom: 2px solid #4CAF50;
      padding-bottom: 5px;
      margin-bottom: 20px;
    }

    /* Price Display */
    .price {
      font-size: 1.8rem;
      font-weight: 700;
      color: #4CAF50;
      margin-top: 10px;
    }

    /* Log/Output Area */
    #serverConsole {
      background: #000;
      color: #0f0;
      font-family: monospace;
      height: 200px;
      padding: 15px;
      border-radius: 10px;
      overflow-y: auto;
      white-space: pre-wrap;
    }

    /* Footer */
    footer {
      text-align: center;
      padding: 20px;
      color: #666;
      font-size: 0.9rem;
      margin-top: 40px;
    }
  </style>
</head>
<body>

  <div class="container">
    <header>
      <h1>Byteblitz Minecraft Server Hosting Clone</h1>
      <p>Erstelle & verwalte deinen eigenen Minecraft Server einfach online</p>
    </header>

    <form id="serverForm">
      <div class="form-grid">

        <!-- Serverdaten -->
        <section>
          <h2>Server konfigurieren</h2>

          <label for="serverName">Servername</label>
          <input type="text" id="serverName" name="serverName" placeholder="z.B. MeinMinecraftServer" required />

          <label for="serverVersion">Minecraft Version</label>
          <select id="serverVersion" name="serverVersion" required>
            <option value="1.19.4">1.19.4</option>
            <option value="1.18.2">1.18.2</option>
            <option value="1.17.1">1.17.1</option>
            <option value="1.16.5">1.16.5</option>
            <option value="1.12.2">1.12.2</option>
          </select>

          <label for="serverType">Servertyp</label>
          <select id="serverType" name="serverType" required>
            <option value="vanilla">Vanilla</option>
            <option value="spigot">Spigot</option>
            <option value="paper">Paper</option>
            <option value="forge">Forge</option>
            <option value="fabric">Fabric</option>
          </select>

          <label for="ramAmount">RAM (GB)</label>
          <select id="ramAmount" name="ramAmount" required>
            <option value="1">1 GB</option>
            <option value="2">2 GB</option>
            <option value="4">4 GB</option>
            <option value="8">8 GB</option>
            <option value="16" selected>16 GB</option>
          </select>

          <label for="slots">Spieler Slots</label>
          <input type="number" id="slots" name="slots" min="1" max="100" value="20" required />
        </section>

        <!-- Konto & Bestellung -->
        <section>
          <h2>Account & Bestellung</h2>

          <label for="username">Benutzername</label>
          <input type="text" id="username" name="username" placeholder="Dein Benutzername" required />

          <label for="email">E-Mail Adresse</label>
          <input type="email" id="email" name="email" placeholder="deine@email.de" required />

          <label for="password">Passwort</label>
          <input type="password" id="password" name="password" placeholder="Passwort" required />

          <label for="paymentMethod">Zahlungsmethode</label>
          <select id="paymentMethod" name="paymentMethod" required>
            <option value="paypal">PayPal</option>
            <option value="creditcard">Kreditkarte</option>
            <option value="sofort">SOFORT Überweisung</option>
          </select>

          <div class="price" id="priceDisplay">Preis: 5,99 € / Monat</div>

          <button type="submit">Server bestellen</button>
        </section>
      </div>
    </form>

    <section>
      <h2>Server Konsole</h2>
      <div id="serverConsole">Logs erscheinen hier...</div>
    </section>
  </div>

  <footer>
    &copy; 2025 Byteblitz Clone Demo
  </footer>

  <script>
    // Preis-Logik
    const priceDisplay = document.getElementById('priceDisplay');
    const ramSelect = document.getElementById('ramAmount');
    const slotsInput = document.getElementById('slots');

    function calculatePrice() {
      const ram = parseInt(ramSelect.value);
      const slots = parseInt(slotsInput.value);

      // Beispiel Preisformel: Basis 5 € + 1€/GB RAM + 0.2€/Slot
      let price = 5 + ram * 1 + slots * 0.2;
      priceDisplay.textContent = 'Preis: ' + price.toFixed(2) + ' € / Monat';
    }

    ramSelect.addEventListener('change', calculatePrice);
    slotsInput.addEventListener('input', calculatePrice);
    calculatePrice();

    // Formular-Handling
    const form = document.getElementById('serverForm');
    const consoleDiv = document.getElementById('serverConsole');

    form.addEventListener('submit', e => {
      e.preventDefault();

      // Hier würdest du die Daten an dein Backend schicken
      const data = {
        serverName: form.serverName.value,
        serverVersion: form.serverVersion.value,
        serverType: form.serverType.value,
        ramAmount: form.ramAmount.value,
        slots: form.slots.value,
        username: form.username.value,
        email: form.email.value,
        password: form.password.value,
        paymentMethod: form.paymentMethod.value
      };

      consoleDiv.textContent += `\nBestellung erhalten:\n${JSON.stringify(data, null, 2)}\n`;

      alert('Bestellung erfolgreich! (Demo - Backend fehlt)');
      form.reset();
      calculatePrice();
    });

    // Demo-Log Update (simuliert Server-Logs)
    setInterval(() => {
      const date = new Date().toLocaleTimeString();
      consoleDiv.textContent += `[${date}] Server läuft... (Simulierte Logs)\n`;
      consoleDiv.scrollTop = consoleDiv.scrollHeight;
    }, 3000);
  </script>
</body>
</html>
