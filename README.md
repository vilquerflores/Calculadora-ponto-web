<!DOCTYPE html><html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculadora de Ponto</title>
  <style>
    body {
      font-family: sans-serif;
      padding: 20px;
      max-width: 600px;
      margin: auto;
    }
    input, select {
      margin: 5px 0;
      padding: 8px;
      width: 100%;
    }
    .alerta {
      color: red;
      font-weight: bold;
    }
    .resultado {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h2>Calculadora de Ponto de Trabalho</h2><label for="entrada">Horário de Entrada:</label> <input type="time" id="entrada">

<label for="intervalo">Início do Intervalo:</label> <input type="time" id="intervalo">

<label for="saida">Horário de Saída:</label> <input type="time" id="saida">

<label for="escala">Escala de Trabalho:</label> <select id="escala"> <option value="07:20">7h20min</option> <option value="08:00">8h00min</option> </select>

<button onclick="calcularPonto()">Calcular</button>

  <div class="resultado" id="resultado"></div>  <script>
    function parseTime(timeStr) {
      const [h, m] = timeStr.split(":").map(Number);
      return h * 60 + m;
    }

    function formatMinutes(mins) {
      const h = Math.floor(mins / 60).toString().padStart(2, '0');
      const m = (mins % 60).toString().padStart(2, '0');
      return `${h}:${m}`;
    }

    function calcularPonto() {
      const entrada = document.getElementById("entrada").value;
      const intervalo = document.getElementById("intervalo").value;
      const saida = document.getElementById("saida").value;
      const escala = document.getElementById("escala").value;

      const resultado = document.getElementById("resultado");
      resultado.innerHTML = "";

      if (!entrada || !intervalo || !saida) {
        resultado.innerHTML = "<p class='alerta'>Por favor, preencha todos os horários.</p>";
        return;
      }

      const entradaMin = parseTime(entrada);
      const intervaloMin = parseTime(intervalo);
      const saidaMin = parseTime(saida);
      const escalaMin = parseTime(escala);

      const trabalhoAntes = intervaloMin - entradaMin;
      const trabalhoDepois = saidaMin - intervaloMin;
      const totalTrabalho = trabalhoAntes + trabalhoDepois;

      const intervaloDuracao = saidaMin - intervaloMin - trabalhoDepois;
      const excessoHoras = totalTrabalho - escalaMin;

      resultado.innerHTML += `<p><strong>Tempo Trabalhado:</strong> ${formatMinutes(totalTrabalho)}</p>`;

      // Alertas
      if (intervaloDuracao < 60) {
        resultado.innerHTML += `<p class='alerta'>Intervalo menor que 1 hora.</p>`;
      }
      if (intervaloDuracao > 180) {
        resultado.innerHTML += `<p class='alerta'>Intervalo maior que 3 horas.</p>`;
      }
      if (trabalhoAntes > 360) {
        resultado.innerHTML += `<p class='alerta'>Turno antes do intervalo excede 6 horas sem pausa.</p
