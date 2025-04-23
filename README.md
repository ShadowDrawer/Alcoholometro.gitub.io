<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Alcoholómetro Consciente - ShadowDrawer</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #111;
      color: #fff;
      padding: 20px;
    }
    h1 
    {
  color: #ff4d4d;
  text-align: center;
    }
    {
      color: #ff4d4d;
    }
    .boton {
      background-color: #6610f2;
      color: white;
      padding: 10px;
      margin: 5px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
    }

    .boton:hover {
      background-color: #4d0099;
    }

    .reset {
      background-color: #ff4444;
    }

    .reset:hover {
      background-color: #cc0000;
    }

    #resultado {
      margin-top: 20px;
      padding: 15px;
      background-color: #1a1a1a;
      border-radius: 6px;
      max-width: 600px;
    }

    .advertencia {
      font-weight: bold;
      margin-top: 10px;
    }

    .advertencia.roja {
      color: red;
    }

    .sintomas, .estado-personalizado, .compromiso, .inventario {
      margin-top: 15px;
      padding: 10px;
      background-color: #222;
      border-radius: 6px;
      border-left: 4px solid #8844ff;
    }

    .compromiso button {
      margin-top: 10px;
      background-color: #00cc66;
    }
  </style>
</head>
<body>

  <h1>Alcoholómetro Consciente de ShadowDrawer 🍷🛡️</h1>
  <p>Haz clic en cada tipo de bebida que hayas consumido para poder estimar tu situación etílica:</p>
   
  <div id="botones">
    <button class="boton" onclick="agregarBebida('Cerveza')">🍺 Cerveza</button>
    <button class="boton" onclick="agregarBebida('Tequila')">🥃 Tequila</button>
    <button class="boton" onclick="agregarBebida('Vino')">🍷 Vino</button>
    <button class="boton" onclick="agregarBebida('Martini')">🍸 Martini</button>
    <button class="boton" onclick="agregarBebida('Ron')">🥃 Ron</button>
    <button class="boton" onclick="agregarBebida('Whiskey')">🥃 Whiskey</button>
    <button class="boton" onclick="agregarBebida('Champagne')">🍾 Champagne</button>
    <button class="boton" onclick="agregarBebida('Guaro')">🍶 Guaro</button>
    <button class="boton" onclick="agregarBebida('Brandy')">🍷 Brandy</button>
    <button class="boton" onclick="agregarBebida('Vodka')">🍸 Vodka</button>
    <button class="boton" onclick="agregarBebida('Ginebra')">🍸 Ginebra</button>
    <button class="boton" onclick="agregarBebida('Margarita')">🍹 Margarita</button>
    <button class="boton" onclick="agregarBebida('JagerMeister')">🥃 Shot de JagerMeister</button>
    <button class="boton" onclick="agregarBebida('Whiskey Doble')">🥃 Whiskey Doble</button>
    <button class="boton" onclick="agregarBebida('Cantarito')">🍹 Cantarito (mezcal)</button>
    <button class="boton" onclick="agregarBebida('Chiliguaro')">🌶️ Chiliguaro</button>
    <button class="boton" onclick="agregarBebida('Cuba Libre')">🥃 Cuba Libre</button>
    <button class="boton" onclick="agregarBebida('Destornillador')">🍹 Destornillador</button>
    <button class="boton" onclick="agregarBebida('Pizco Peruano')">🇵🇪 Shot de Pizco Peruano</button>
  </div>

  <div id="resultado">
    <p>Total de bebidas: <span id="total">0</span></p>
    <p>Estimación BAC: <span id="bac">0.00</span>%</p>
    <p class="advertencia" id="mensajeAdvertencia">Aún sobrio. Puedes continuar.</p>
    <button class="boton reset" onclick="resetear()">🔄 Reset</button>

    <div class="sintomas">
      <strong>Síntomas:</strong>
      <p id="mensajeSintomas">Sin síntomas notables.</p>
    </div>

    <div class="estado-personalizado">
      <strong>Tu estado actual:</strong>
      <ul id="estadoDetalle"></ul>
    </div>

    <div class="inventario">
      <strong>Inventario de bebidas:</strong>
      <ul id="inventarioLista"></ul>
      <p id="mezclaAdvertencia"></p>
    </div>

    <div class="compromiso">
      <strong>Compromiso personal:</strong>
      <p>Si estás bajo influencia, comprometete a no conducir.</p>
      <button onclick="alert('Gracias. Tu compromiso puede salvar vidas.')">✋ Me comprometo a no conducir</button>
    </div>
  </div>
  <div class="sanciones" style="margin-top: 15px; padding: 10px; background-color: #222; border-radius: 6px; border-left: 4px solid #8844ff;">
    <strong>Sanciones legales en Costa Rica por conducir en estado de ebriedad:</strong>
    <ul>
      <li><strong>BAC de 0.20% o más:</strong> Multa de ₡327.713, retiro de 6 puntos en la licencia y posible prisión de 1 a 3 años (Artículo 254 del Código Penal).</li>
      <li><strong>BAC entre 0.05% y 0.20%:</strong> Multa de ₡327.713 y retiro de 6 puntos de la licencia.</li>
      <li><strong>Negarse a la prueba de alcoholemia:</strong> Multa de ₡327.713 más retiro de puntos.</li>
      <li><strong>Conductores novatos (menos de 3 años) y profesionales:</strong> Límite es 0.02% BAC. Excederlo conlleva las mismas sanciones.</li>
      <li>En todos los casos puede implicar retención del vehículo, suspensión de la licencia y registro en antecedentes penales.</li>
    </ul>
  </div>  
  <script>
    let total = 0;
    const BAC_POR_BEBIDA = 0.02;
    const inventario = {};

    function agregarBebida(tipo) {
      total += tipo === 'Whiskey Doble' || tipo === 'Pizco Peruano' ? 2 : 1;
      inventario[tipo] = (inventario[tipo] || 0) + 1;
      actualizarEstado();
    }

    function resetear() {
      total = 0;
      for (let key in inventario) delete inventario[key];
      actualizarEstado();
    }

    function actualizarEstado() {
      const bac = (total * BAC_POR_BEBIDA).toFixed(2);
      document.getElementById("total").textContent = total;
      document.getElementById("bac").textContent = bac;

      const advertencia = document.getElementById("mensajeAdvertencia");
      const sintomas = document.getElementById("mensajeSintomas");
      const estado = document.getElementById("estadoDetalle");
      const inventarioLista = document.getElementById("inventarioLista");
      const mezclaAdvertencia = document.getElementById("mezclaAdvertencia");

      estado.innerHTML = "";
      inventarioLista.innerHTML = "";
      Object.keys(inventario).forEach(bebida => {
        inventarioLista.innerHTML += `<li>${bebida}: ${inventario[bebida]}</li>`;
      });

      const tiposConsumidos = Object.keys(inventario).length;
      if (tiposConsumidos >= 2) {
        mezclaAdvertencia.textContent = "⚠️ Mezcla de bebidas detectada. Posible dolor abdominal intenso.";
        mezclaAdvertencia.style.color = "orange";
      } else {
        mezclaAdvertencia.textContent = "";
      }

      if (bac < 0.03) {
        advertencia.textContent = "Aún sobrio. Puedes continuar.";
        advertencia.className = "advertencia";
        sintomas.textContent = "Sin síntomas notables.";
        estado.innerHTML = `
          <li>✔️ Coordinación: Normal</li>
          <li>🧠 Juicio: Claro</li>
          <li>👁️ Visión: Estable</li>
          <li>🚗 Reacción: Rápida</li>
          <li>⚠️ Riesgo de accidente: Bajo</li>`;
      } else if (bac < 0.06) {
        advertencia.textContent = "Precaución: espera 2-3 horas antes de conducir.";
        advertencia.className = "advertencia";
        sintomas.textContent = "Ligera euforia, reducción de reflejos y atención.";
        estado.innerHTML = `
          <li>⚠️ Coordinación: Leve disminución</li>
          <li>🧠 Juicio: Afectado</li>
          <li>👁️ Visión: Ligeramente borrosa</li>
          <li>🚗 Reacción: Retardada</li>
          <li>⚠️ Riesgo de accidente: Moderado</li>`;
      } else if (bac < 0.10) {
        advertencia.textContent = "⚠️ Riesgo alto. Esperá al menos 5 horas.";
        advertencia.className = "advertencia roja";
        sintomas.textContent = "Descoordinación, mareos, visión borrosa, visión de túnel.";
        estado.innerHTML = `
          <li>❌ Coordinación: Pobre</li>
          <li>🧠 Juicio: Nublado</li>
          <li>👁️ Visión: Borrosa / Túnel</li>
          <li>🚗 Reacción: Lenta</li>
          <li>🚨 Riesgo de accidente: Alto</li>`;
      } else if (bac < 0.20) {
        advertencia.textContent = "🛑 PELIGRO: No conduzcas. Esperá 8+ horas.";
        advertencia.className = "advertencia roja";
        sintomas.textContent = "Vómitos, pérdida de equilibrio, visión doble, pérdida de sensación en rostro.";
        estado.innerHTML = `
          <li>❌ Coordinación: Muy deficiente</li>
          <li>🧠 Juicio: Severamente alterado</li>
          <li>👁️ Visión: Doble / empañada</li>
          <li>🚗 Reacción: Críticamente reducida</li>
          <li>🧊 Sensación facial: Disminuida en pómulos, nariz, mejillas</li>
          <li>🚨 Riesgo de accidente: Extremo</li>`;
      } else {
        advertencia.textContent = "🚨 URGENCIA: Riesgo de coma alcohólico.";
        advertencia.className = "advertencia roja";
        sintomas.textContent = "Inconsciencia, visión nula, pérdida total de sensación corporal.";
        estado.innerHTML = `
          <li>❌ Coordinación: Prácticamente nula</li>
          <li>🧠 Juicio: Inexistente</li>
          <li>👁️ Visión: Nula</li>
          <li>🚗 Reacción: Inactiva</li>
          <li>💀 Riesgo de accidente: Mortal</li>
          <li>🧊 Sensación facial: Ausente</li>`;
      }
    }
    
  </script>

</body>
</html>
