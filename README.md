<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Calificación Victoriana</title>
<style>
body { 
    background:#f5f0e1; 
    font-family:'Times New Roman', serif; 
    color:#2e1c0c; 
    text-align:center; 
    padding:50px;
}
h1 { 
    font-size:50px; 
    border-bottom:3px solid #bfa36f; 
    padding-bottom:10px; 
    margin-bottom:50px;
}
.estrella { 
    font-size:50px; 
    cursor:pointer; 
    color:#bfa36f; 
    transition: transform 0.2s;
}
.estrella:hover { 
    transform:scale(1.2);
}
.estrella.activa { 
    color:#e1c17b;
}
.marco { 
    border:5px double #bfa36f; 
    padding:30px; 
    display:inline-block; 
    background:#fff8e7; 
    box-shadow:5px 5px 15px rgba(0,0,0,0.2); 
    border-radius:15px;
}
.resultado { 
    margin-top:20px; 
    font-size:24px; 
    font-weight:bold;
}
button { 
    margin-top:20px; 
    font-size:20px; 
    padding:10px 30px; 
    border:2px solid #bfa36f; 
    border-radius:10px; 
    background:#fff8e7; 
    cursor:pointer;
}
button:hover { 
    background:#e1c17b;
}
</style>
</head>
<body>

<h1>Califica la Presentación</h1>
<div class="marco">
<p>¿Qué tan buena fue la presentación?</p>
<div id="estrellas">
  <span class="estrella" data-value="1">&#9733;</span>
  <span class="estrella" data-value="2">&#9733;</span>
  <span class="estrella" data-value="3">&#9733;</span>
  <span class="estrella" data-value="4">&#9733;</span>
  <span class="estrella" data-value="5">&#9733;</span>
</div>
<button onclick="enviarVoto()">Enviar Voto</button>
<div class="resultado" id="resultado">Promedio: 0 | Votos: 0</div>
</div>

<script>
let estrellas = document.querySelectorAll('.estrella');
let calificacion = 0;

// Resaltar estrellas al pasar el mouse
estrellas.forEach(st => {
    st.addEventListener('mouseover', () => resaltar(st.getAttribute('data-value')));
    st.addEventListener('mouseout', () => resaltar(calificacion));
    st.addEventListener('click', () => { calificacion = st.getAttribute('data-value'); resaltar(calificacion); });
});

function resaltar(valor){
    estrellas.forEach(st => st.classList.toggle('activa', st.getAttribute('data-value') <= valor));
}

// Guardar votos en localStorage
function enviarVoto(){
    if(calificacion==0){ alert("Selecciona al menos una estrella!"); return; }

    let votos = JSON.parse(localStorage.getItem('votos') || "[]");
    votos.push(parseInt(calificacion));
    localStorage.setItem('votos', JSON.stringify(votos));
    calificacion = 0;
    resaltar(0);
    mostrarResultado();
    alert("¡Gracias por tu calificación!");
}

// Mostrar promedio
function mostrarResultado(){
    let votos = JSON.parse(localStorage.getItem('votos') || "[]");
    let total = votos.length;
    let suma = votos.reduce((a,b)=>a+b,0);
    let promedio = total>0 ? (suma/total).toFixed(2) : 0;
    document.getElementById('resultado').innerText = `Promedio: ${promedio} | Votos: ${total}`;
}

mostrarResultado();
</script>

</body>
</html>