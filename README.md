<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tarjetas digitales | Florería Gloria</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#fff5f7;color:#542b3b;font-family:Arial,sans-serif}
header{background:#702b48;color:white;text-align:center;padding:22px}
header h1{margin:0;font-family:Georgia,serif}
main{max-width:1000px;margin:25px auto;padding:15px}
.layout{display:grid;grid-template-columns:320px 1fr;gap:22px}
.panel,.preview{background:white;border-radius:18px;padding:22px;box-shadow:0 5px 22px #60243b15}
label{display:block;font-weight:bold;margin:15px 0 7px}
input,textarea,select{width:100%;padding:12px;border:1px solid #e4cbd4;border-radius:9px;font:inherit}
textarea{min-height:110px;resize:vertical}
button{border:0;border-radius:25px;padding:13px 18px;background:#913d60;color:white;font-weight:bold;cursor:pointer;margin:8px 5px 0 0}
button:hover{opacity:.85}
.preview{padding:12px}
.card{min-height:500px;border-radius:14px;overflow:hidden;text-align:center;color:white;background:linear-gradient(145deg,#702b48,#c36e8d,#f0bac8);display:flex;flex-direction:column;align-items:center;justify-content:center;padding:35px 22px}
.card h2{font: bold clamp(32px,6vw,52px) Georgia,serif;margin:15px 0}
.card p{line-height:1.7;white-space:pre-line}
.rose{font-size:55px}
.small{font-size:12px;letter-spacing:3px}
.sharebox{margin-top:18px;text-align:center}
#status{font-size:13px;color:#387451}
footer{text-align:center;padding:25px;color:#85566a}
@media(max-width:720px){.layout{grid-template-columns:1fr}.card{min-height:420px}}
</style>
</head>
<body>
<header>
<h1>Florería Gloria 🌹</h1>
<p>Diseña una tarjeta digital especial</p>
</header>

<main>
<div class="layout">
<section class="panel">
<h2>Personaliza tu tarjeta</h2>

<label for="name">Nombre de la persona</label>
<input id="name" maxlength="50" placeholder="Ej. Andrea">

<label for="message">Dedicatoria</label>
<textarea id="message" maxlength="500">Que este nuevo año de vida te regale motivos para sonreír, sueños por cumplir y momentos que quieras guardar para siempre.

¡Nunca olvides lo especial que eres!</textarea>

<label for="theme">Color de la tarjeta</label>
<select id="theme">
<option value="rosa">Rosa romántico</option>
<option value="vino">Vino elegante</option>
<option value="dorado">Dorado especial</option>
<option value="azul">Azul cielo</option>
</select>

<button onclick="actualizar()">Actualizar tarjeta</button>
<p style="font-size:13px;color:#876879">Los cambios también se reflejan automáticamente en la vista previa.</p>
</section>

<section class="preview">
<div class="card" id="card">
<div class="rose">🌹</div>
<div class="small">UN DÍA MUY ESPECIAL</div>
<h2 id="previewName">¡Feliz cumpleaños!</h2>
<p id="previewMessage"></p>
<div style="font-size:32px;margin-top:20px">✨ 💗 ✨</div>
</div>
<div class="sharebox">
<button onclick="compartir()">Compartir tarjeta</button>
<button onclick="copiar()">Copiar enlace personalizado</button>
<p id="status"></p>
</div>
</section>
</div>
</main>

<footer>Una experiencia creada con cariño por <strong>Florería Gloria</strong> 🌹</footer>

<script>
const nameInput=document.getElementById("name");
const messageInput=document.getElementById("message");
const themeInput=document.getElementById("theme");

const themes={
 rosa:"linear-gradient(145deg,#702b48,#c36e8d,#f0bac8)",
 vino:"linear-gradient(145deg,#310d24,#762b4c,#b56a83)",
 dorado:"linear-gradient(145deg,#62401d,#b58a45,#ead6a0)",
 azul:"linear-gradient(145deg,#244b68,#5796b5,#b7dce8)"
};

function actualizar(){
 const nombre=nameInput.value.trim()||"Alguien especial";
 document.getElementById("previewName").textContent="¡Feliz cumpleaños, "+nombre+"!";
 document.getElementById("previewMessage").textContent=messageInput.value;
 document.getElementById("card").style.background=themes[themeInput.value];
}

function obtenerEnlace(){
 const url=new URL(location.href);
 url.search="";
 url.searchParams.set("nombre",nameInput.value.trim());
 url.searchParams.set("mensaje",messageInput.value);
 url.searchParams.set("color",themeInput.value);
 return url.toString();
}

function cargarTarjeta(){
 const params=new URLSearchParams(location.search);
 if(params.has("nombre"))nameInput.value=params.get("nombre");
 if(params.has("mensaje"))messageInput.value=params.get("mensaje");
 if(params.has("color")&&themes[params.get("color")])themeInput.value=params.get("color");
 actualizar();
}

async function copiar(){
 const enlace=obtenerEnlace();
 try{
  await navigator.clipboard.writeText(enlace);
  document.getElementById("status").textContent="¡Enlace personalizado copiado!";
 }catch(e){
  document.getElementById("status").textContent="No se pudo copiar. Puedes compartir desde el botón de compartir.";
 }
}

async function compartir(){
 const enlace=obtenerEnlace();
 const datos={
  title:"Una tarjeta especial de Florería Gloria",
  text:"Te comparto una tarjeta hecha especialmente para ti 🌹",
  url:enlace
 };
 if(navigator.share){
  try{await navigator.share(datos)}catch(e){}
 }else{
  copiar();
 }
}

[nameInput,messageInput,themeInput].forEach(element=>{
 element.addEventListener("input",actualizar);
 element.addEventListener("change",actualizar);
});

cargarTarjeta();
</script>
</body>
</html>
