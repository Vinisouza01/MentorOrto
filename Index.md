<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>Mentor GPO – Classe I</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
    background: #f0f2f5;
    height: 100vh;
    display: flex;
    flex-direction: column;
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
    max-width: 85%;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 10px;
    font-size: 0.95rem;
}
.bot {
    background: white;
    border-left: 4px solid #1a2a6c;
}
.user {
    background: #1a2a6c;
    color: white;
    margin-left: auto;
}
.options {
    margin-top: 8px;
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
    font-size: 0.85rem;
}
button:hover {
    background: #1a2a6c;
    color: white;
}
.card {
    background: #f9f9f9;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 10px;
    margin-top: 8px;
}
.footer {
    font-size: 0.7rem;
    color: #777;
    margin-top: 6px;
}
</style>
</head>

<body>

<header>
<h2>Mentor Digital GPO</h2>
<small>Classe I • Diagnóstico Guiado</small>
</header>

<div id="chat"></div>

<script>
const chat = document.getElementById("chat");
let step = "inicio";
let data = {};

function bot(text, options = []) {
    const d = document.createElement("div");
    d.className = "msg bot";
    d.innerHTML = text;

    if (options.length) {
        const o = document.createElement("div");
        o.className = "options";
        options.forEach(opt => {
            const b = document.createElement("button");
            b.textContent = opt;
            b.onclick = () => user(opt);
            o.appendChild(b);
        });
        d.appendChild(o);
    }
    chat.appendChild(d);
    chat.scrollTop = chat.scrollHeight;
}

function user(text) {
    const d = document.createElement("div");
    d.className = "msg user";
    d.textContent = text;
    chat.appendChild(d);
    chat.scrollTop = chat.scrollHeight;
    next(text);
}

function next(val) {
    switch (step) {

        case "inicio":
            step = "problema";
            bot(
                "<strong>Classe I</strong><br>Relação sagital normal.<br><br>Selecione o problema:",
                ["Mordida Cruzada Posterior"]
            );
            break;

        case "problema":
            data.problema = val;
            step = "denticao";
            bot(
                "<strong>Mordida Cruzada Posterior</strong><br><br>Selecione a fase da dentição:",
                ["Dentição Decídua", "Dentição Mista", "Dentição Permanente"]
            );
            break;

        case "denticao":
            data.denticao = val;
            if (val === "Dentição Permanente") {
                step = "origem";
                bot(
                    "Na dentição permanente, determine a origem:",
                    ["Dentária", "Esquelética"]
                );
            } else {
                finalizarDeciduaMista();
            }
            break;

        case "origem":
            data.origem = val;
            finalizarPermanente();
            break;
    }
}

function finalizarDeciduaMista() {
    bot(`
        <div class="card">
        <strong>Mordida Cruzada – ${data.denticao}</strong><br><br>

        <strong>Como identificar:</strong><br>
        • Desvio funcional mandibular<br>
        • Arco superior estreito<br>
        • Cruzamento unilateral ou bilateral<br><br>

        <strong>Possíveis condutas:</strong><br>
        • Ortopedia funcional<br>
        • Desgastes seletivos<br>
        • Pistas diretas<br>
        • Disjunção
        <div class="footer">
        Intervenção precoce favorece correções ortopédicas simples
        </div>
        </div>

        <div class="options">
        <button onclick="location.reload()">Novo Caso</button>
        </div>
    `);
}

function finalizarPermanente() {
    bot(`
        <div class="card">
        <strong>Mordida Cruzada – Dentição Permanente</strong><br><br>

        <strong>Origem:</strong> ${data.origem}<br><br>

        <strong>Como diferenciar:</strong><br>
        ${data.origem === "Dentária" ?
        "• Bases ósseas compatíveis<br>• Inclinação dentária alterada" :
        "• Maxila atrésica<br>• Palato profundo<br>• Discrepância transversal evidente"}<br><br>

        <strong>Condutas possíveis:</strong><br>
        ${data.origem === "Esquelética" ?
        "• Cirurgia ortognática<br>• Compensação dentária (casos selecionados)" :
        "• Mecânica ortodôntica corretiva"}
        <div class="footer">
        Avaliar limites biomecânicos e estéticos
        </div>
        </div>

        <div class="options">
        <button onclick="location.reload()">Novo Caso</button>
        </div>
    `);
}

bot("Vamos iniciar a análise da <strong>Classe I</strong>?", ["Iniciar"]);
</script>

</body>
</html>
