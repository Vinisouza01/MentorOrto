<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mentor GPO</title>

<style>
body {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
    background: #f0f2f5;
    display: flex;
    flex-direction: column;
    height: 100vh;
}
header {
    background: linear-gradient(to right, #1a2a6c, #b21f1f);
    color: white;
    padding: 15px;
    text-align: center;
}
#chat {
    flex: 1;
    padding: 15px;
    overflow-y: auto;
}
.msg {
    max-width: 80%;
    padding: 10px 14px;
    border-radius: 12px;
    margin-bottom: 10px;
}
.bot {
    background: #fff;
    border-left: 4px solid #1a2a6c;
}
.user {
    background: #1a2a6c;
    color: white;
    margin-left: auto;
}
.options {
    margin-top: 10px;
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}
button {
    padding: 8px 12px;
    border-radius: 20px;
    border: 1px solid #1a2a6c;
    background: white;
    cursor: pointer;
}
button:hover {
    background: #1a2a6c;
    color: white;
}
</style>
</head>

<body>

<header>
    <h2>Mentor Digital GPO</h2>
    <small>Diagnóstico 5D • Planejamento Ortodôntico</small>
</header>

<div id="chat"></div>

<script>
const chat = document.getElementById("chat");

let step = "classe";
let data = {};

function bot(text, options = []) {
    const m = document.createElement("div");
    m.className = "msg bot";
    m.innerHTML = text;

    if (options.length) {
        const o = document.createElement("div");
        o.className = "options";
        options.forEach(opt => {
            const b = document.createElement("button");
            b.textContent = opt;
            b.onclick = () => user(opt);
            o.appendChild(b);
        });
        m.appendChild(o);
    }

    chat.appendChild(m);
    chat.scrollTop = chat.scrollHeight;
}

function user(text) {
    const m = document.createElement("div");
    m.className = "msg user";
    m.textContent = text;
    chat.appendChild(m);
    chat.scrollTop = chat.scrollHeight;
    next(text);
}

function next(val) {
    switch (step) {

        case "classe":
            data.classe = val;
            step = "dimensao";
            bot(
                "Selecione a dimensão diagnóstica (Mapa Mental):",
                ["1D – Sagital", "2D – Transversal", "3D – Vertical", "4D – Dentária", "5D – Estética"]
            );
            break;

        case "dimensao":
            data.dimensao = val;
            step = "origem";
            bot("Origem da alteração:", ["Dentária", "Esquelética"]);
            break;

        case "origem":
            data.origem = val;
            step = "perfil";
            bot("Perfil facial:", ["Protruso", "Retruso", "Normal"]);
            break;

        case "perfil":
            data.perfil = val;
            step = "denticao";
            bot("Fase da dentição:", ["Decídua", "Mista", "Permanente"]);
            break;

        case "denticao":
            data.denticao = val;
            step = "problema";
            bot("Problema principal:", ["Apinhamento", "Protrusão", "Vertical"]);
            break;

        case "problema":
            data.problema = val;
            step = "severidade";
            bot("Severidade:", ["Leve", "Moderada", "Severa"]);
            break;

        case "severidade":
            data.severidade = val;
            finalizar();
            break;
    }
}

function finalizar() {
    bot(`
        <strong>🧠 Planejamento GPO</strong><br><br>
        Classe: ${data.classe}<br>
        Dimensão: ${data.dimensao}<br>
        Origem: ${data.origem}<br>
        Perfil: ${data.perfil}<br>
        Dentição: ${data.denticao}<br>
        Problema: ${data.problema} (${data.severidade})<br><br>
        Direcionamento baseado em Diagnóstico 5D e GPO.
        <div class="options">
            <button onclick="location.reload()">Novo Caso</button>
        </div>
    `);
}

bot(
    "Olá! Vamos iniciar o planejamento.<br><br>Qual a <strong>Classe de Angle</strong>?",
    ["Classe I", "Classe II", "Classe III"]
);
</script>

</body>
</html>
