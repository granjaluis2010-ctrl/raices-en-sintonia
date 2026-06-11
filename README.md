
# raices-en-[gemini-code-1781216809243.html](https://github.com/user-attachments/files/28858455/gemini-code-1781216809243.html)
sintonia
Publico
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>RES - Raíces en Sintonía</title>

<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js"></script>
<script src="https://meet.jit.si/external_api.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
/* Paleta de Colores basada en el Logo */
:root {
  --primary-color: #2F6984;    /* Azul-Verde profundo */
  --secondary-color: #A96245;  /* Óxido / Naranja cálido */
  --light-color: #F7EFE9;      /* Fondo beige claro y cálido */
  --accent-light: #52BF90;     /* Verde complementario para el degradado */
}

/* Estilos Base y Reseteo */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  background: var(--light-color);
  overscroll-behavior: none;   /* Evita el efecto de rebote al deslizar en apps */
  -webkit-tap-highlight-color: transparent; /* Elimina el recuadro azul al tocar en móviles */
}

/* Header con Degradado del Logo */
header {
  background: linear-gradient(135deg, var(--primary-color), var(--accent-light));
  color: white;
  padding: 15px;
  text-align: center;
  font-weight: bold;
  font-size: 1.2em;
  letter-spacing: 1px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* Contenedor Flexbox Responsivo */
.container {
  display: flex;
  flex-direction: column; /* Apilado por defecto en móviles */
}

/* Barra Lateral (Sidebar) */
.sidebar {
  width: 100%;
  background: white;
  padding: 15px;
  box-sizing: border-box;
  border-bottom: 3px solid var(--secondary-color);
}

.sidebar select {
  width: 100%;
  padding: 12px;
  margin-bottom: 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 1em;
  background-color: #fff;
}

/* Área Principal de Contenido */
.main {
  flex: 1;
  padding: 15px;
  box-sizing: border-box;
}

/* Escenario de Constelaciones (Adaptable) */
#escenario {
  height: 45vh; /* Altura relativa a la pantalla del dispositivo */
  background: #EBF1F5;
  position: relative;
  border-radius: 12px;
  overflow: hidden; /* Evita que los avatares se salgan del recuadro */
  border: 2px dashed var(--primary-color);
  margin-bottom: 15px;
}

/* Avatares */
.avatar {
  position: absolute;
  width: 65px;
  height: 65px;
  cursor: move;
  border-radius: 50%; /* Por si usas imágenes circulares */
  object-fit: cover;
  touch-action: none; /* Crucial: detiene el scroll nativo del celular al arrastrar */
  user-select: none;
  -webkit-user-drag: none;
}

/* Tarjetas (Cards) Modernizadas */
.card {
  background: white;
  margin-top: 15px;
  padding: 15px;
  border-radius: 10px;
  border-left: 5px solid var(--secondary-color);
  box-shadow: 0 4px 6px rgba(0,0,0,0.05);
}

/* Inputs y Botones adaptados para dedos (Touch Targets) */
input {
  width: 100%;
  margin-bottom: 12px;
  padding: 12px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 1em;
}

button {
  width: 100%;
  margin-top: 8px;
  padding: 14px; /* Más alto para facilitar el toque en móviles */
  background: var(--primary-color);
  border: none;
  border-radius: 6px;
  color: white;
  cursor: pointer;
  font-size: 1em;
  font-weight: bold;
  transition: background 0.2s ease;
}

button:active {
  background: #20495C; /* Feedback visual al presionar */
}

/* Bordes de Emociones */
.emotion-love { border: 4px solid #52BF90 !important; }
.emotion-conflict { border: 4px solid #D9534F !important; }
.emotion-neutral { border: 4px solid #A0AAB2 !important; }

/* Media Query para Pantallas Medianas/Grandes (Tablets y Escritorio) */
@media screen and (min-width: 768px) {
  .container {
    flex-direction: row; /* Diseño en paralelo */
    min-height: calc(100vh - 54px);
  }

  .sidebar {
    width: 280px;
    border-bottom: none;
    border-right: 3px solid var(--secondary-color);
  }

  #escenario {
    height: 60vh; /* Más espacio vertical en pantallas grandes */
  }

  .avatar {
    width: 80px;
    height: 80px;
  }
}
</style>
</head>
<body>

<header>🌿 RAÍCES EN SINTONÍA</header>

<div class='container'>
  <div class='sidebar'>
    <select id='personaje'>
      <option value='madre.png'>Madre</option>
      <option value='padre.png'>Padre</option>
      <option value='abuelo.png'>Abuelo</option>
      <option value='perro.png'>Perro</option>
    </select>
    <select id='emocion'>
      <option value='neutral'>Neutral</option>
      <option value='amor'>Amor</option>
      <option value='conflicto'>Conflicto</option>
    </select>
    <button onclick='crearAvatar()'>Añadir Personaje</button>
    <button onclick='guardarSesion()'>Guardar Sesión</button>
    <button onclick='crearSala()'>Iniciar Videollamada</button>
  </div>

  <div class='main'>
    <div class='card'>
      <input id='email' placeholder='Correo electrónico'>
      <input id='password' type='password' placeholder='Contraseña'>
      <button onclick='login()'>Ingresar</button>
      <p id='user'></p>
    </div>

    <div id='escenario'></div>

    <div class='card'>
      <input type='date' id='fecha'>
      <input type='time' id='hora'>
      <button onclick='crearCita()'>Reservar Cita</button>
      <div id='citas'></div>
    </div>

    <div class='card'>
      <canvas id='grafica'></canvas>
    </div>

    <div class='card'>
      <div id='video' style='height:300px; background: #000; border-radius: 6px;'></div>
    </div>
  </div>
</div>

<script>
// Configuración de Firebase
const firebaseConfig = { apiKey: 'TU_API_KEY', authDomain: 'TU_DOMINIO', projectId: 'TU_PROYECTO' };
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();
const auth = firebase.auth();
let personajes = [];

function login() {
  auth.signInWithEmailAndPassword(email.value, password.value).then(u => {
    user.innerText = u.user.email;
    verCitas();
    dashboard();
  }).catch(err => alert("Error de login: " + err.message));
}

function crearAvatar() {
  let img = personaje.value, em = emocion.value;
  let el = document.createElement('img');
  el.src = img;
  el.className = 'avatar';
  
  // Posición inicial por defecto corregida para que no queden fuera visualmente
  let obj = { img, x: 50, y: 50, emocion: em };
  personajes.push(obj);
  
  // Aplicar estilos iniciales
  el.style.left = obj.x + 'px';
  el.style.top = obj.y + 'px';
  
  color(el, em);
  drag(el, obj);
  escenario.appendChild(el);
} 

function color(el, e) {
  let emotionMap = {
    'amor': 'emotion-love',
    'conflicto': 'emotion-conflict',
    'neutral': 'emotion-neutral'
  };
  el.classList.add(emotionMap[e]);
}

// Función Drag & Drop Híbrida (Soporta Mouse y Pantallas Táctiles)
function drag(el, obj) {
  let ox = 0, oy = 0;

  const iniciarArrastre = (e) => {
    // Detecta si es un evento touch o un click de mouse
    let clienteX = e.touches ? e.touches[0].clientX : e.clientX;
    let clienteY = e.touches ? e.touches[0].clientY : e.clientY;
    
    ox = clienteX - el.offsetLeft;
    oy = clienteY - el.offsetTop;

    document.addEventListener('mousemove', mover);
    document.addEventListener('touchmove', mover, { passive: false });
  };

  const mover = (e) => {
    // Si es touch, evita que la pantalla se mueva (scrollee) mientras arrastras el avatar
    if (e.touches) e.preventDefault();

    let clienteX = e.touches ? e.touches[0].pageX : e.pageX;
    let clienteY = e.touches ? e.touches[0].pageY : e.pageY;

    // Calcular nuevas coordenadas relativizándolo al contenedor #escenario
    let rectEscenario = escenario.getBoundingClientRect();
    let xRelativa = clienteX - rectEscenario.left - ox;
    let yRelativa = clienteY - rectEscenario.top - oy;

    // Límites para que los avatares no salgan volando fuera del escenario
    let maxX = rectEscenario.width - el.offsetWidth;
    let maxY = rectEscenario.height - el.offsetHeight;

    obj.x = Math.max(0, Math.min(xRelativa, maxX));
    obj.y = Math.max(0, Math.min(yRelativa, maxY));

    el.style.left = obj.x + 'px';
    el.style.top = obj.y + 'px';
  };

  const detenerArrastre = () => {
    document.removeEventListener('mousemove', mover);
    document.removeEventListener('touchmove', mover);
  };

  // Eventos para Ratón
  el.addEventListener('mousedown', iniciarArrastre);
  document.addEventListener('mouseup', detenerArrastre);

  // Eventos para Pantallas Táctiles (Móvil/App)
  el.addEventListener('touchstart', iniciarArrastre);
  document.addEventListener('touchend', detenerArrastre);
}

function guardarSesion() {
  if(!auth.currentUser) return alert("Debes iniciar sesión primero");
  db.collection('sesiones').add({ usuario: auth.currentUser.email, personajes })
    .then(() => alert("Sesión guardada exitosamente"));
}

function crearCita() {
  if(!auth.currentUser) return alert("Debes iniciar sesión primero");
  db.collection('citas').add({ usuario: auth.currentUser.email, fecha: fecha.value, hora: hora.value })
    .then(() => { alert("Cita reservada"); verCitas(); });
}

function verCitas() {
  db.collection('citas').where('usuario', '==', auth.currentUser.email).get().then(s => {
    let h = '';
    s.forEach(d => {
      let c = d.data();
      h += `<p>📅 ${c.fecha} - ⏰ ${c.hora}</p>`
    });
    citas.innerHTML = h;
  });
}

function dashboard() {
  db.collection('sesiones').get().then(s => {
    db.collection('citas').get().then(c => {
      new Chart(grafica, {
        type: 'bar',
        data: {
          labels: ['Sesiones', 'Citas'],
          datasets: [{
            label: 'Total',
            data: [s.size, c.size],
            backgroundColor: ['#2F6984', '#A96245'] // Colores corporativos en las barras
          }]
        },
        options: { responsive: true }
      });
    })
  })
}

function crearSala() {
  new JitsiMeetExternalAPI('meet.jit.si', {
    roomName: 'RES-' + Math.random().toString(36).substring(7),
    parentNode: video,
    width: '100%',
    height: '100%'
  });
}
</script>

</body>
</html>
