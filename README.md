
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cronômetro de Futebol</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap');
        body {
            font-family: 'Orbitron', sans-serif;
            text-align: center;
            background-color: #f4f4f4;
            padding: 20px;
            transition: background 0.3s, color 0.3s;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            display: inline-block;
            transition: background 0.3s, color 0.3s;
        }
        .placar {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            border-top: 2px solid #ccc;
            padding-top: 20px;
        }
        .placar h2 {
            text-align: center;
            font-size: 26px;
            color: #444;
        }
        .time {
            text-align: center;
            font-size: 24px;
        }
        input[type="number"] {
            width: 100px;
            height: 50px;
            font-size: 24px;
            text-align: center;
            border: 2px solid #0072ff;
            border-radius: 8px;
            margin-top: 10px;
        }
        .cronometro {
            font-size: 60px;
            margin: 20px 0;
            font-weight: bold;
        }
        .cronometro span {
            color: red;
        }
        button {
            padding: 12px 20px;
            margin: 10px;
            font-size: 18px;
            cursor: pointer;
            border: none;
            border-radius: 8px;
            background: linear-gradient(45deg, #00c6ff, #0072ff);
            color: white;
            font-weight: bold;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        button:hover {
            transform: scale(1.1);
            box-shadow: 0 0 15px rgba(0, 114, 255, 0.5);
        }
        .modo-escuro {
            background-color: #121212;
            color: white;
        }
        .modo-escuro .container {
            background-color: #1e1e1e;
            color: white;
        }
        .resultado {
            font-size: 28px;
            font-weight: bold;
            margin-top: 20px;
            padding: 10px;
            border-radius: 10px;
            display: none;
            color: white;
        }
        .vitoria-azul { background: #0072ff; }
        .vitoria-amarelo { background: gold; color: black; }
        .empate { background: gray; }
    </style>
</head>
<body>
    <button onclick="alternarModo()">Alternar Modo</button>
    <div class="container">
        <h1>Cronômetro de Futebol</h1>
        <label for="tempo">Definir Tempo (minutos):</label>
        <input type="number" id="tempo" min="1" value="45">
        <button onclick="iniciarCronometro()">Iniciar</button>
        <div class="cronometro" id="cronometro"><span>45</span>:<span>00</span></div>
        <h2>Placar</h2>
        <div class="placar">
            <div class="time">
                <h2 style="color: blue;">Time Azul</h2>
                <input type="number" id="placarAzul" min="0" value="0">
            </div>
            <div class="time">
                <h2 style="color: gold;">Time Amarelo</h2>
                <input type="number" id="placarAmarelo" min="0" value="0">
            </div>
        </div>
        <div id="resultado" class="resultado"></div>
    </div>
    <audio id="somAlerta" src="https://www.soundjay.com/button/beep-09.wav"></audio>
    <script>
        let tempoRestante, intervalo;
        function iniciarCronometro() {
            let minutos = parseInt(document.getElementById("tempo").value);
            tempoRestante = minutos * 60;
            atualizarCronometro();
            intervalo = setInterval(() => {
                if (tempoRestante > 0) {
                    tempoRestante--;
                    atualizarCronometro();
                } else {
                    clearInterval(intervalo);
                    document.getElementById("somAlerta").play();
                    verificarResultado();
                }
            }, 1000);
        }
        function atualizarCronometro() {
            let minutos = Math.floor(tempoRestante / 60);
            let segundos = tempoRestante % 60;
            document.getElementById("cronometro").innerHTML = 
                `<span>${minutos.toString().padStart(2, '0')}</span>:<span style='color:red;'>${segundos.toString().padStart(2, '0')}</span>`;
        }
        function verificarResultado() {
            let placarAzul = parseInt(document.getElementById("placarAzul").value);
            let placarAmarelo = parseInt(document.getElementById("placarAmarelo").value);
            let resultado = document.getElementById("resultado");
            if (placarAzul > placarAmarelo) {
                resultado.innerHTML = "🏆 Time Azul venceu!";
                resultado.className = "resultado vitoria-azul";
            } else if (placarAmarelo > placarAzul) {
                resultado.innerHTML = "🏆 Time Amarelo venceu!";
                resultado.className = "resultado vitoria-amarelo";
            } else {
                resultado.innerHTML = "⚖️ O jogo terminou empatado!";
                resultado.className = "resultado empate";
            }
            resultado.style.display = "block";
        }
        function alternarModo() {
            document.body.classList.toggle("modo-escuro");
        }
    </script>
</body>
</html>
