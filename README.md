<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Entrenamiento Ultra-Pro v16.1</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.4/dist/chart.umd.min.js"></script>
<style>
:root {
  --background-dark: #1f1f1f;
  --surface-card: #2c2c2c;
  --surface-hover: #3a3a3a;
  --text-primary: #e0e0e0;
  --text-secondary: #a0a0a0;
  --accent-blue: #00bcd4; /* Cian vibrante */
  --accent-blue-dark: #0097a7;
  --shadow-color: rgba(0, 0, 0, 0.4);
}
* { box-sizing: border-box; }
body {
  font-family: 'Montserrat', sans-serif;
  background-color: var(--background-dark);
  color: var(--text-primary);
  margin: 0;
  padding: 8px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow-x: hidden;
}
h1 {
  text-align: center;
  color: var(--accent-blue);
  text-shadow: 0 2px 4px var(--shadow-color);
  margin-bottom: 8px;
  font-size: 1.6rem;
  font-weight: 700;
}
.header-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    gap: 8px;
}
.header-container h1 {
    margin-bottom: 0;
}
.header-container button {
    padding: 6px 12px;
    font-size: 0.8rem;
    margin: 0;
}
#rutinaActual {
  font-size: 1.1rem;
  color: var(--text-secondary);
  text-align: center;
  margin-bottom: 8px;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
  padding: 8px 16px;
  border-radius: 8px;
  background: var(--surface-card);
  border: 1px solid transparent;
}
#rutinaActual:hover {
  color: var(--accent-blue);
  background: var(--surface-hover);
  border: 1px solid var(--accent-blue);
}
#rutinaActualEditor {
  font-size: 0.9rem;
  color: var(--text-secondary);
  text-align: center;
  margin-bottom: 12px;
}
#rutinaActualEditor button {
  background: var(--accent-blue);
  color: #fff;
  border: none;
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8rem;
  margin-left: 8px;
  transition: all 0.2s ease-in-out;
}
#rutinaActualEditor button:hover {
  background: var(--accent-blue-dark);
}

/* Modal */
.modal {
  display: none;
  position: fixed;
  z-index: 1000;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.8);
  align-items: center;
  justify-content: center;
}
.modal.active {
  display: flex;
}
.modal-content {
  background: var(--surface-card);
  padding: 24px;
  border-radius: 12px;
  width: 90%;
  max-width: 450px;
  max-height: 80vh;
  overflow-y: auto;
  box-shadow: 0 8px 20px var(--shadow-color);
}
.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
.modal-header h3 {
  margin: 0;
  color: var(--accent-blue);
  font-size: 1.4rem;
  font-weight: 700;
}
.close-modal-btn {
  background: #e74c3c;
  color: #fff;
  border: none;
  padding: 6px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background 0.2s ease-in-out;
}
.close-modal-btn:hover {
  background: #c0392b;
}
#listaModalRutinas {
  list-style: none;
  padding: 0;
  margin: 0;
}
#listaModalRutinas li {
  padding: 12px;
  margin: 8px 0;
  background: var(--surface-hover);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
  font-weight: 500;
}
#listaModalRutinas li:hover {
  background: var(--background-dark);
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
#changelogContent h4 {
    color: var(--accent-blue);
    margin-top: 20px;
}
#changelogContent ul {
    list-style: none;
    padding: 0;
}
#changelogContent li {
    background: var(--surface-hover);
    padding: 10px;
    border-radius: 6px;
    margin-bottom: 8px;
}

/* Pestañas */
.tab-container {
  display: flex;
  justify-content: center;
  margin-bottom: 16px;
  flex-wrap: wrap;
}
.tab-btn {
  padding: 10px 18px;
  font-size: 0.9rem;
  cursor: pointer;
  border: none;
  border-radius: 8px;
  background: var(--surface-card);
  color: var(--text-secondary);
  margin: 4px;
  transition: all 0.2s ease-in-out;
  font-weight: 500;
}
.tab-btn.active {
  background: var(--accent-blue);
  color: #fff;
  box-shadow: 0 4px 10px rgba(0, 189, 212, 0.3);
}
.tab-btn:hover:not(.active) {
  background: var(--surface-hover);
  transform: translateY(-2px);
}
.tab-content {
  display: none;
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
  background: var(--surface-card);
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 8s 20px var(--shadow-color);
  /* Asegura que el contenido sea visible debajo del footer flotante */
  padding-bottom: 80px; 
}
.tab-content.active {
  display: block;
}

/* Sección Principal */
#ejercicio {
  font-size: clamp(1rem, 4.5vw, 1.8rem);
  margin-bottom: 8px;
  padding: 12px;
  border-radius: 12px;
  text-align: center;
  width: 100%;
  transition: all 0.3s ease;
  box-shadow: 0 4px 8px var(--shadow-color);
  height: 110px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  text-shadow: 0 1px 3px rgba(0,0,0,0.5);
  cursor: pointer;
  overflow: hidden;
  position: relative;
}
#ejercicio-text {
  transition: opacity 0.5s ease-in-out;
  opacity: 1;
}
#ejercicio.fade-out #ejercicio-text {
  opacity: 0;
}
#descripcion {
  font-size: 0.9rem;
  margin-bottom: 8px;
  text-align: center;
  width: 100%;
  color: var(--text-secondary);
  background: var(--surface-hover);
  padding: 10px;
  border-radius: 8px;
  min-height: 40px;
  transition: all 0.3s ease;
  font-weight: 400;
}
.timer-container {
  background: var(--surface-card);
  border-radius: 12px;
  padding: 16px;
  margin: 12px auto;
  width: 100%;
  box-shadow: 0 8px 20px var(--shadow-color);
  text-align: center;
}
#tiempo {
  font-size: 3rem;
  margin-bottom: 8px;
  text-shadow: 0 2px 4px var(--shadow-color);
  text-align: center;
  font-weight: 700;
  color: var(--accent-blue);
  transition: color 0.3s ease;
}
#tiempo.warning {
  color: #e74c3c;
}
#repeticiones {
  font-size: 1rem;
  margin-bottom: 8px;
  text-align: center;
  color: var(--text-secondary);
}
#duracionTotal {
    font-size: 0.9rem;
    color: var(--text-secondary);
    margin-top: 8px;
    text-align: center;
}
#navBtns {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-top: 12px;
}
#navBtns button {
  padding: 8px 16px;
  font-size: 0.9rem;
  background: var(--accent-blue);
  border-radius: 8px;
  color: #fff;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
#navBtns button:hover {
  background: var(--accent-blue-dark);
  transform: translateY(-2px);
}
button {
  padding: 10px 20px;
  font-size: 0.9rem;
  cursor: pointer;
  border: none;
  border-radius: 8px;
  margin: 6px;
  background: var(--accent-blue);
  color: #fff;
  transition: all 0.2s ease-in-out;
  font-weight: 500;
  box-shadow: 0 4px 8px var(--shadow-color);
}
#navBtns button:disabled, button:disabled {
  background: #555;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}
.progress-container, .progress-total-container {
  width: 100%;
  max-width: 400px;
  margin: 8px auto;
  background: var(--surface-hover);
  border-radius: 16px;
  position: relative;
  box-shadow: inset 0 2px 5px rgba(0,0,0,0.3);
}
.progress-bar, .progress-total-bar {
  width: 0%;
  height: 24px;
  background: linear-gradient(90deg, #2ecc71, #27ae60);
  border-radius: 16px;
  transition: width 0.4s linear;
}
.progress-total-container span {
  position: absolute;
  width: 100%;
  text-align: center;
  line-height: 24px;
  font-weight: 700;
  color: #fff;
  text-shadow: 0 1px 2px rgba(0,0,0,0.5);
}
input, select, textarea {
  margin: 6px;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid var(--surface-hover);
  font-size: 0.9rem;
  background: var(--background-dark);
  color: var(--text-primary);
  width: 100%;
}
textarea {
  height: 80px;
  resize: vertical;
}
.form-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 12px;
  width: 100%;
}
/* Estilo para las listas */
.list-container {
  width: 100%;
  max-width: 500px;
  min-height: 80px;
  max-height: 300px;
  overflow-y: auto;
  padding: 12px;
  margin-top: 16px;
  border-radius: 12px;
  background: var(--background-dark);
  box-shadow: inset 0 2px 5px rgba(0,0,0,0.3);
}
#listaEjercicios, #listaRutinasGuardadas, #listaHistorial {
  list-style: none;
  padding: 0;
  margin: 0;
}
#listaEjercicios li, #listaRutinasGuardadas li, #listaHistorial li {
  background: var(--surface-card);
  margin: 8s 0;
  padding: 12px;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
  color: var(--text-primary);
  font-size: 0.9rem;
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
}
#listaEjercicios li:hover, #listaRutinasGuardadas li:hover, #listaHistorial li:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  background: var(--surface-hover);
}
#listaEjercicios button, #listaRutinasGuardadas button {
  margin-left: 8px;
  background: #e74c3c;
  padding: 4px 8px;
  font-size: 0.8rem;
}
#listaEjercicios h3 {
  color: var(--accent-blue);
  margin: 8px 0;
  font-size: 1rem;
  text-align: center;
  font-weight: 700;
}
#listaHistorial li {
  cursor: pointer;
}

/* Gráfico */
.chart-container {
  position: relative;
  width: 100%;
  max-width: 500px;
  margin: 0 auto 16px;
  min-height: 250px;
  background: var(--surface-card);
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 8px 20px var(--shadow-color);
}

/* Contenedor de botones de control */
.controls-container {
    text-align: center;
    margin-top: 8px;
}

/* Nuevo Estilo para el editor flotante */
.editor-controls-sticky {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  max-width: 500px;
  background: var(--surface-card);
  padding: 10px;
  box-shadow: 0 -4px 10px var(--shadow-color);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  z-index: 500;
  border-top-left-radius: 12px;
  border-top-right-radius: 12px;
  left: 50%;
  transform: translateX(-50%);
}
.editor-controls-sticky button {
  width: 100%;
  margin: 0;
}
.editor-controls-actions {
  display: flex;
  justify-content: space-between;
  width: 100%;
  gap: 8px;
}
.editor-controls-actions button {
    flex-grow: 1;
    margin: 0;
}
</style>
</head>
<body>

<div class="header-container">
    <h1>Entrenamiento Ultra-Pro v16.1</h1>
    <button onclick="mostrarNovedades()">✨ Novedades</button>
</div>
<div id="rutinaActual" onclick="abrirSelectorRutinas()">Rutina Predeterminada</div>

<div id="modalNovedades" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3>Novedades</h3>
            <button class="close-modal-btn" onclick="cerrarNovedades()">X</button>
        </div>
        <div id="changelogContent"></div>
    </div>
</div>

<div id="modalRutinas" class="modal">
  <div class="modal-content">
    <div class="modal-header">
      <h3>Seleccionar Rutina</h3>
      <button class="close-modal-btn" id="closeModal">X</button>
    </div>
    <ul id="listaModalRutinas"></ul>
  </div>
</div>

<div id="modalCompletado" class="modal">
    <div class="modal-content">
        <h3>¡Rutina Completada! 🎉</h3>
        <p>¿Quieres ver tu sesión en el historial?</p>
        <div style="display: flex; justify-content: center; gap: 15px;">
            <button id="aceptarCompletado" style="background-color: #2ecc71;">Aceptar</button>
            <button id="cancelarCompletado" style="background-color: #e74c3c;">Cancelar</button>
        </div>
    </div>
</div>

<div class="tab-container">
  <button class="tab-btn active" onclick="mostrarPestana('principal')">🏋️ Principal</button>
  <button class="tab-btn" onclick="mostrarPestana('editor')">✏️ Editor</button>
  <button class="tab-btn" onclick="mostrarPestana('biblioteca')">📚 Biblioteca</button>
  <button class="tab-btn" onclick="mostrarPestana('log')">📊 Log</button>
</div>

<div id="principal" class="tab-content active">
  <div id="ejercicio">
    <div id="ejercicio-text">Iniciar</div>
  </div>
  <div id="descripcion"></div>
  <div class="timer-container">
    <div id="tiempo">00:00</div>
    <div id="repeticiones"></div>
    <div id="duracionTotal"></div>
    <div id="navBtns">
      <button id="prevEjercicioBtn" disabled>⬅ Anterior</button>
      <button id="nextEjercicioBtn" disabled>Siguiente ➡</button>
    </div>
  </div>
  <div class="progress-container">
    <div class="progress-bar" id="progressBar"></div>
  </div>
  <div class="progress-total-container">
    <div class="progress-total-bar" id="progressTotalBar"></div>
    <span id="progressTotalText">0%</span>
  </div>
  <div class="controls-container">
    <button id="pauseBtn" disabled>Pausar</button>
    <button id="resumeBtn" disabled>Reanudar</button>
    <button id="resetBtn">Reiniciar</button>
  </div>
</div>

<div id="editor" class="tab-content">
  <div id="rutinaActualEditor">Rutina Actual: Predeterminada <button onclick="abrirSelectorRutinas()">Cambiar Rutina</button></div>
  <div style="text-align: center; margin-bottom: 15px;">
    <button id="limpiarRutinaBtn" style="background-color: #e67e22;">✨ Nueva Rutina</button>
  </div>
  <div class="form-grid">
    <input type="text" id="nombreEjercicio" placeholder="Nombre del ejercicio">
    <label for="tipoConteo" style="margin-top: 10px;">Tipo de Conteo:</label>
    <select id="tipoConteo">
      <option value="tiempo">Tiempo</option>
      <option value="repeticiones">Repeticiones</option>
    </select>
    <input type="number" id="duracionEjercicio" placeholder="Duración (segundos)">
    <input type="number" id="repeticionesEjercicio" placeholder="Repeticiones" style="display: none;">
    <input type="number" id="seriesEjercicio" placeholder="Series">
    <input type="number" id="descansoEjercicio" placeholder="Descanso (segundos)" value="5">
    <textarea id="descripcionEjercicio" placeholder="Descripción del ejercicio" rows="2"></textarea>
  </div>
  <div style="margin-top:12px; text-align: center;">
    <label style="margin-left: 15px;">Ronda:</label>
    <select id="seleccionRonda"></select>
    <button id="nuevaRondaBtn">Añadir Nueva Ronda</button>
    <button id="eliminarRondaBtn">Eliminar Ronda</button>
  </div>
  <div class="list-container">
    <ul id="listaEjercicios"></ul>
  </div>
  <div class="editor-controls-sticky">
    <div class="editor-controls-actions">
        <button id="guardarRutinaEditorBtn">💾 Guardar Rutina</button>
        <button onclick="exportarRutina()">📤 Exportar</button>
        <input type="file" id="importarRutinaInput" style="display: none;" accept=".txt">
        <button onclick="document.getElementById('importarRutinaInput').click()">📥 Importar</button>
    </div>
    <button id="agregarEjercicioBtn">➕ Añadir / Actualizar Ejercicio</button>
  </div>
</div>

<div id="biblioteca" class="tab-content">
  <p>Selecciona una rutina de tu lista para cargarla y empezar a entrenar.</p>
  <div class="list-container">
    <ul id="listaRutinasGuardadas"></ul>
  </div>
</div>

<div id="log" class="tab-content">
  <div class="chart-container">
    <canvas id="progresoChart"></canvas>
  </div>
  <div class="list-container">
    <ul id="listaHistorial"></ul>
  </div>
  <button id="limpiarHistorialBtn" style="margin-top: 20px;">Limpiar Log</button>
</div>

<script>
// Sonidos
function beep(f=440,d=200){const a=new (window.AudioContext||window.webkitAudioContext)();const o=a.createOscillator();o.type='sine';o.frequency.setValueAtTime(f,a.currentTime);o.connect(a.destination);o.start();o.stop(a.currentTime+d/1000);}
function reproducirSonido(){beep(400,200);}
function sonidoInicio(){beep(800,400);}

// Funciones de Importar/Exportar
function exportarRutina() {
  const rutinaData = {
    nombre: nombreRutinaActual,
    rondas: rondas
  };
  const dataStr = JSON.stringify(rutinaData, null, 2);
  const blob = new Blob([dataStr], {type: "text/plain"});
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url;
  a.download = `rutina_${nombreRutinaActual.replace(/ /g, '_')}.txt`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

document.getElementById("importarRutinaInput").addEventListener("change", function(event) {
  const file = event.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = function(e) {
      try {
        const importedData = JSON.parse(e.target.result);
        if (importedData.nombre && importedData.rondas) {
          if (rutinaModificada && !confirm("Tienes cambios sin guardar. ¿Quieres importar esta rutina y sobrescribir la actual?")) {
            return;
          }
          const nuevaRutina = {
            nombre: importedData.nombre,
            rondas: JSON.stringify(importedData.rondas)
          };
          cargarRutinaEspecifica(nuevaRutina);
          alert(`Rutina "${importedData.nombre}" importada con éxito.`);
        } else {
          alert("El archivo no tiene el formato de rutina correcto.");
        }
      } catch (error) {
        alert("Error al leer el archivo. Asegúrate de que sea un archivo de texto válido.");
      }
    };
    reader.readAsText(file);
  }
});


// Data de novedades
const changelogData = {
    "v16.1": [
        "**Botón 'Siguiente' Corregido:** Se solucionó el error que impedía que el botón 'Siguiente' avanzara correctamente el progreso de la rutina, especialmente en los ejercicios por repeticiones.",
        "**Lógica de Progreso Optimizada:** La barra de progreso total ahora se actualiza solo cuando se completa un ejercicio entero (todas sus series), tanto en ejercicios por tiempo como por repeticiones."
    ],
    "v16.0": [
        "**Diseño Minimalista del Editor:** Los botones de `Exportar` e `Importar` ahora están integrados en la barra inferior flotante del editor, para un diseño más limpio y accesible sin necesidad de hacer scroll."
    ],
    "v15.0": [
        "**Importar/Exportar Rutinas:** Ahora puedes exportar tu rutina actual a un archivo de texto (`.txt`) y compartirla. También puedes importar rutinas para cargarlas en tu aplicación, permitiendo un fácil intercambio y respaldo."
    ],
    "v14.0": [
        "**Editor con Conteo por Repeticiones:** Ahora puedes elegir si un ejercicio se cuenta por **Tiempo** o por **Repeticiones**. En la pantalla principal, los ejercicios por repeticiones mostrarán el conteo en lugar del temporizador y el botón **'Siguiente'** te permitirá avanzar cuando los completes."
    ],
    "v13.0": [
        "**Editor Minimalista y Móvil:** Se ha mejorado la experiencia del editor en el móvil. Los botones **'Añadir / Actualizar Ejercicio'** y **'Guardar Rutina'** ahora se encuentran en una barra fija en la parte inferior de la pantalla, siempre accesibles, eliminando la necesidad de hacer scroll para guardar cambios."
    ],
    "v12.0": [
        "**Progreso Automático de la Rutina:** Se solucionó el error principal: el progreso total ahora se actualiza automáticamente al finalizar cada ejercicio, no solo al presionar 'Siguiente'. ¡Tu avance se refleja en tiempo real!"
    ],
    "v11.0": [
        "**Bloqueo de Pantalla Robusto**: Se solucionó el problema de la pantalla que se apagaba. Ahora el sistema detecta si regresas a la aplicación y reactiva el bloqueo de pantalla automáticamente, garantizando que el temporizador no se detenga por inactividad."
    ],
    "v10.0": [
        "**Pantalla Siempre Encendida:** Se solucionó el problema de la pantalla que se apagaba. Ahora la aplicación mantiene la pantalla activa durante el entrenamiento."
    ],
    "v9.9": [
        "**Guardado Intuitivo:** El botón 'Guardar Rutina' se movió a la pestaña **Editor**, para que puedas guardar tus cambios al instante."
    ],
    "v9.8": [
        "**Carga Automática:** Se solucionó el problema principal del guardado. Ahora, la aplicación carga automáticamente la última rutina que guardaste o seleccionaste al refrescar la página. ¡Tu progreso se mantiene!"
    ],
    "v9.7": [
        "**Corrección de Guardado:** Se solucionó el problema donde las rutinas guardadas no se mantenían. Ahora, la aplicación detecta si una rutina ya existe y te pregunta si quieres sobrescribirla."
    ],
    "v9.6": [
        "**Temporizador en Rojo a 3s:** Se corrigió el error. El temporizador ahora se pone en rojo correctamente cuando faltan 3 segundos para el final de un ejercicio o descanso.",
        "**Descanso por Defecto en Rutinas:** Se agregó 5 segundos de descanso por defecto a todas las rutinas predeterminadas."
    ],
    "v9.5": [
        "**Preparación entre Ejercicios:** Se añadió un temporizador de 5 segundos de preparación antes de cada ejercicio o descanso, dando más tiempo para la transición.",
        "**Bug de Reinicio Resuelto:** El botón de 'Reiniciar' ahora funciona correctamente, restableciendo la aplicación al estado inicial 'Iniciar' y permitiendo comenzar un nuevo entrenamiento sin errores."
    ],
    "v9.4": [
        "**Bug de Inicio Resuelto (FINAL):** El botón de 'Iniciar' ahora es más robusto y funciona siempre, sin importar la rutina seleccionada.",
        "**Cálculo de Duración Mejorado:** La duración estimada de la rutina ahora se actualiza de forma inmediata al seleccionar una rutina de tu biblioteca."
    ],
    "v9.3": [
        "**Bug de Inicio Resuelto:** El temporizador ahora se inicia correctamente sin tener que presionar el botón de 'Siguiente'.",
        "**Tiempo Total Arreglado:** El cálculo del tiempo total ahora muestra el valor correcto en lugar de 'undefined'.",
        "**Uso de Repeticiones:** Se explica cómo usar el campo de 'Descripción' para incluir el número de repeticiones de cada ejercicio."
    ],
    "v9.2": [
        "**Bug de Inicio Resuelto:** El temporizador ahora se inicia correctamente.",
        "**Tiempo Total Arreglado:** El cálculo del tiempo total ahora muestra el valor correcto en lugar de 'undefined'.",
        "**Uso de Repeticiones:** Se explica cómo usar el campo de 'Descripción' para incluir el número de repeticiones de cada ejercicio."
    ],
    "v9.1": [
        "**Bug de Inicio Resuelto:** El botón de 'Iniciar' ahora funciona correctamente, y el entrenamiento comienza sin fallos.",
        "**Sugerencia de Repeticiones:** Se explica cómo usar el campo de 'Descripción' para incluir el número de repeticiones en tus ejercicios."
    ],
    "v9.0": [
        "**Bug de Inicio Resuelto:** El botón de 'Iniciar' ahora funciona correctamente, y el entrenamiento comienza sin fallos.",
        "**Cálculo de Tiempo Total Mejorado:** El tiempo total ahora incluye la duración de los ejercicios y todos los descansos entre series, proporcionando una estimación más precisa.",
        "**Contador de Series:** Se muestra el progreso de las series (ej. 'Serie 1 de 3') en la pantalla principal."
    ],
    "v8.0": [
        "**Solución de Bug:** Corregido el problema que impedía que la rutina se iniciara al presionar el botón.",
        "**Tiempo Total:** Se añadió un indicador de tiempo total estimado de la rutina en la pantalla principal.",
        "**Historial de Novedades:** Ahora el botón de Novedades guarda y muestra un historial de las versiones anteriores."
    ],
    "v7.0": [
        "**Contador de Series:** Ahora se muestra el número de serie actual (ej. \"Serie 1 de 3\").",
        "**Pausa Personalizable:** Nuevo campo en el editor para establecer el tiempo de descanso entre ejercicios.",
        "**Animaciones Suaves:** Transición de texto más fluida entre ejercicios.",
        "**Log Mejorado:** El historial ahora también registra la duración total de la rutina."
    ]
};

// Wake Lock API
let wakeLock = null;
let isTimerRunning = false;
let wakeLockRetryInterval = null;

const requestWakeLock = async () => {
  if ('wakeLock' in navigator) {
    try {
      if (wakeLock === null) {
        wakeLock = await navigator.wakeLock.request('screen');
        console.log('Bloqueo de pantalla activado.');
        wakeLock.addEventListener('release', () => {
          console.log('Bloqueo de pantalla liberado. Re-solicitando...');
          wakeLock = null;
          // La lógica de reintento ahora se encarga de esto.
        });
      }
    } catch (err) {
      console.error(`${err.name}, ${err.message}`);
    }
  }
};

const releaseWakeLock = () => {
  if (wakeLock) {
    wakeLock.release();
    wakeLock = null;
    isTimerRunning = false;
    clearInterval(wakeLockRetryInterval);
    wakeLockRetryInterval = null;
    console.log('Bloqueo de pantalla liberado manualmente.');
  }
};

const retryWakeLock = () => {
    if (wakeLockRetryInterval) {
        clearInterval(wakeLockRetryInterval);
    }
    wakeLockRetryInterval = setInterval(() => {
        if (isTimerRunning && wakeLock === null) {
            requestWakeLock();
        }
    }, 5000); // Intenta adquirir el bloqueo cada 5 segundos si no está activo
};

document.addEventListener('visibilitychange', () => {
  if (isTimerRunning && document.visibilityState === 'visible') {
    requestWakeLock();
  }
});

// Pestañas
function mostrarPestana(pestana) {
  document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
  document.getElementById(pestana).classList.add('active');
  const btn = document.querySelector(`.tab-btn[onclick="mostrarPestana('${pestana}')"]`);
  if (btn) btn.classList.add('active');
  if (pestana === 'log') actualizarGráficoProgreso();
  if (pestana === 'principal') actualizarDuracionTotal();
}

// Modal de novedades
const modalNovedades = document.getElementById("modalNovedades");
const changelogContent = document.getElementById("changelogContent");
function mostrarNovedades() {
    changelogContent.innerHTML = "";
    for (const version in changelogData) {
        const versionHeader = document.createElement("h4");
        versionHeader.textContent = `Versión ${version}`;
        changelogContent.appendChild(versionHeader);
        const ul = document.createElement("ul");
        changelogData[version].forEach(item => {
            const li = document.createElement("li");
            li.innerHTML = item;
            ul.appendChild(li);
        });
        changelogContent.appendChild(ul);
    }
    modalNovedades.classList.add("active");
}
function cerrarNovedades() { modalNovedades.classList.remove("active"); }

// Modal para selector de rutinas
const modalRutinas = document.getElementById("modalRutinas");
const closeModal = document.getElementById("closeModal");
function abrirSelectorRutinas() {
  const rutinasGuardadas = JSON.parse(localStorage.getItem("rutinasGuardadas") || "[]");
  const listaModal = document.getElementById("listaModalRutinas");
  listaModal.innerHTML = "";
  if (rutinasGuardadas.length === 0) {
    const li = document.createElement("li");
    li.textContent = "No hay rutinas guardadas";
    li.style.cursor = "default";
    li.style.color = "#999";
    listaModal.appendChild(li);
  } else {
    rutinasGuardadas.forEach((rutina) => {
      const li = document.createElement("li");
      li.textContent = rutina.nombre;
      li.onclick = () => {
        cargarRutinaEspecifica(rutina);
        modalRutinas.classList.remove("active");
      };
      listaModal.appendChild(li);
    });
  }
  modalRutinas.classList.add("active");
}
closeModal.addEventListener("click", () => {
  modalRutinas.classList.remove("active");
});


// Rutina predeterminada
const rutinaPredeterminada = [
  [
    {nombre:"Calentamiento general",duracion:120,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Calentamiento 2 minutos", tipoConteo: "tiempo"},
    {nombre:"Caminar en el sitio pasos largos",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Caminar en el sitio con pasos largos 30s", tipoConteo: "tiempo"},
    {nombre:"Movimientos de tobillos y rodillas",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Movimientos de tobillos y rodillas 30s", tipoConteo: "tiempo"},
    {nombre:"Golpes suaves en piernas para activar",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Golpes suaves en piernas 30s", tipoConteo: "tiempo"},
    {nombre:"Estiramientos ligeros cuádriceps y pantorrillas",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Estiramientos 30s", tipoConteo: "tiempo"}
  ],
  [
    {nombre:"Juego de piernas básico",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Paso adelante/atrás ligero sin saltar fuerte", tipoConteo: "tiempo"},
    {nombre:"Sombra de boxeo suave",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Sombra mientras te mueves", tipoConteo: "tiempo"},
    {nombre:"Sentadillas parciales",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Bajas solo hasta donde no duela", tipoConteo: "tiempo"},
    {nombre:"Desplazamientos laterales",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Paso izquierda/derecha con guardia arriba", tipoConteo: "tiempo"},
    {nombre:"Elevaciones de talones",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"En puntilla para pantorrilla", tipoConteo: "tiempo"},
    {nombre:"Caminar activo",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Caminar en sitio respirando activamente", tipoConteo: "tiempo"}
  ],
  [
    {nombre:"Juego de piernas con pivote",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Girar ligero sobre la punta del pie delantero controlado", tipoConteo: "tiempo"},
    {nombre:"Sombra de boxeo con combinaciones",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Jab, cross moviéndote", tipoConteo: "tiempo"},
    {nombre:"Step-up",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Subir y bajar un escalón bajo o ladrillo grande", tipoConteo: "tiempo"},
    {nombre:"Desplazamientos en círculo",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Rodea un punto imaginario siempre en guardia", tipoConteo: "tiempo"},
    {nombre:"Isométrico de sentadilla",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Aguanta medio sentado", tipoConteo: "tiempo"},
    {nombre:"Caminar activo",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Caminar en sitio respirando activamente", tipoConteo: "tiempo"}
  ],
  [
    {nombre:"Juego de piernas rápido",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Pasos cortos como saltitos cortos", tipoConteo: "tiempo"},
    {nombre:"Sombra con defensa",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Esquivas hacia los lados moviendo la cintura", tipoConteo: "tiempo"},
    {nombre:"Zancadas cortas",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Un paso adelante, rodilla baja un poco pero sin forzar", tipoConteo: "tiempo"},
    {nombre:"Shuttle",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Pasos laterales rápidos bajito en guardia", tipoConteo: "tiempo"},
    {nombre:"Elevaciones de rodillas",duracion:30,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Subir una rodilla alterna al frente controlado", tipoConteo: "tiempo"},
    {nombre:"Caminar activo",duracion:60,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Caminar en sitio respirando activamente", tipoConteo: "tiempo"}
  ],
  [
    {nombre:"Estiramiento final",duracion:120,descanso:5,tipo:"ejercicio",color:"#2ecc71",series:1,descripcion:"Estira suavemente cuádriceps llevando talón al glúteo, pantorrillas contra la pared, glúteo sentado pierna sobre la otra", tipoConteo: "tiempo"}
  ]
];

let rondas;
let nombreRutinaActual;
let rutinaModificada = false;
let rondaIndex=0, ejercicioIndex=0, tiempoRestante=0, interval=null, prepInterval=null, descansoInterval=null, editarIndex=null, pausado=false;
let totalEjercicios, ejerciciosCompletados=0, serieActual=1;
let tiempoInicioSesion = null;

const ejercicioDiv=document.getElementById("ejercicio");
const ejercicioTextDiv = document.getElementById("ejercicio-text");
const descripcionDiv=document.getElementById("descripcion");
const tiempoDiv=document.getElementById("tiempo");
const repeticionesDiv=document.getElementById("repeticiones");
const duracionTotalDiv = document.getElementById("duracionTotal");
const progressBar=document.getElementById("progressBar");
const progressTotalBar=document.getElementById("progressTotalBar");
const progressTotalText=document.getElementById("progressTotalText");
const pauseBtn=document.getElementById("pauseBtn");
const resumeBtn=document.getElementById("resumeBtn");
const resetBtn=document.getElementById("resetBtn");
const prevEjercicioBtn=document.getElementById("prevEjercicioBtn");
const nextEjercicioBtn=document.getElementById("nextEjercicioBtn");
const listaEjercicios=document.getElementById("listaEjercicios");
const seleccionRonda=document.getElementById("seleccionRonda");
const rutinaActualDiv = document.getElementById("rutinaActual");
const rutinaActualEditor = document.getElementById("rutinaActualEditor");
const guardarRutinaEditorBtn = document.getElementById("guardarRutinaEditorBtn");
const listaRutinasGuardadas = document.getElementById("listaRutinasGuardadas");
const listaHistorial = document.getElementById("listaHistorial");
const limpiarHistorialBtn = document.getElementById("limpiarHistorialBtn");
const progresoChart = document.getElementById("progresoChart").getContext("2d");
const modalCompletado = document.getElementById("modalCompletado");

const tipoConteoSelect = document.getElementById("tipoConteo");
const duracionInput = document.getElementById("duracionEjercicio");
const repeticionesInput = document.getElementById("repeticionesEjercicio");

tipoConteoSelect.addEventListener("change", () => {
  if (tipoConteoSelect.value === 'tiempo') {
    duracionInput.style.display = 'block';
    repeticionesInput.style.display = 'none';
  } else {
    duracionInput.style.display = 'none';
    repeticionesInput.style.display = 'block';
  }
});

function actualizarNombreRutina() {
    let nombreMostrar = nombreRutinaActual;
    if (rutinaModificada) {
        nombreMostrar += " (Sin guardar)";
    }
    rutinaActualDiv.textContent = nombreMostrar;
    rutinaActualEditor.innerHTML = `Rutina Actual: ${nombreMostrar} <button onclick="abrirSelectorRutinas()">Cambiar Rutina</button>`;
}

function marcarRutinaModificada() {
    if (!rutinaModificada) {
        rutinaModificada = true;
        actualizarNombreRutina();
    }
}

function calcularTiempoTotal() {
    let tiempo = 0;
    rondas.forEach(ronda => {
        ronda.forEach(ejercicio => {
            if (ejercicio.tipoConteo === 'tiempo' && typeof ejercicio.duracion === 'number' && typeof ejercicio.series === 'number' && typeof ejercicio.descanso === 'number') {
                tiempo += (ejercicio.duracion * ejercicio.series) + (ejercicio.descanso * (ejercicio.series > 1 ? ejercicio.series - 1 : 0));
            }
        });
    });
    const min = Math.floor(tiempo / 60);
    const sec = tiempo % 60;
    duracionTotalDiv.textContent = `Duración Total Estimada (Solo Tiempos): ${min}m ${sec}s`;
}

function actualizarDuracionTotal() {
    calcularTiempoTotal();
}


// Gráfico de progreso (Log de Sesiones)
let chartProgreso = null;
function actualizarGráficoProgreso() {
  try {
    const completadas = JSON.parse(localStorage.getItem("rutinasCompletadas") || "[]");
    const fechas = [];
    const valores = [];
    
    // Obtener y ordenar las fechas únicas de las sesiones completadas
    const fechasUnicas = [...new Set(completadas.map(item => item.fecha))].sort((a, b) => {
        const [dayA, monthA, yearA] = a.split('/').map(Number);
        const [dayB, monthB, yearB] = b.split('/').map(Number);
        return new Date(`20${yearA}-${monthA}-${dayA}`) - new Date(`20${yearB}-${monthB}-${dayB}`);
    });
    
    let acumulado = 0;
    const conteoDiario = {};
    completadas.forEach(item => {
        conteoDiario[item.fecha] = (conteoDiario[item.fecha] || 0) + 1;
    });

    fechasUnicas.forEach(fecha => {
        fechas.push(fecha);
        acumulado += conteoDiario[fecha];
        valores.push(acumulado);
    });

    if (chartProgreso) chartProgreso.destroy();

    chartProgreso = new Chart(progresoChart, {
      type: 'line',
      data: {
        labels: fechas,
        datasets: [{
          label: 'Sesiones Completadas (Acumulado)',
          data: valores,
          borderColor: '#2ecc71',
          backgroundColor: ctx => {
            const gradient = ctx.chart.ctx.createLinearGradient(0, 0, 0, 200);
            gradient.addColorStop(0, 'rgba(46, 204, 113, 0.2)');
            gradient.addColorStop(1, 'rgba(46, 204, 113, 0)');
            return gradient;
          },
          tension: 0.4,
          fill: true,
          pointRadius: 3,
          pointHoverRadius: 5,
          pointBackgroundColor: '#2ecc71'
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: { labels: { color: '#fff', font: { size: 12 } } },
          tooltip: {
            backgroundColor: 'rgba(0, 0, 0, 0.8)',
            titleColor: '#fff',
            bodyColor: '#fff',
            borderColor: '#2ecc71',
            borderWidth: 1
          }
        },
        scales: {
          x: {
            ticks: { color: '#bdc3c7', maxRotation: 45, minRotation: 45 },
            grid: { color: 'rgba(255, 255, 255, 0.1)' }
          },
          y: {
            min: 0,
            ticks: { color: '#bdc3c7', stepSize: 1 },
            grid: { color: 'rgba(255, 255, 255, 0.1)' }
          }
        }
      }
    });

    if (valores.every(v => v === 0)) {
      const canvas = document.getElementById("progresoChart");
      const ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.font = "16px Montserrat";
      ctx.fillStyle = "#999";
      ctx.textAlign = "center";
      ctx.fillText("Sin sesiones completadas", canvas.width / 2, canvas.height / 2);
    }
  } catch (error) {
    console.error("Error al actualizar el gráfico de progreso:", error);
    const canvas = document.getElementById("progresoChart");
    const ctx = canvas.getContext("2d");
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.font = "16px Montserrat";
    ctx.fillStyle = "#999";
    ctx.textAlign = "center";
    ctx.fillText("Error al cargar el gráfico", canvas.width / 2, canvas.height / 2);
  }
}

// Funciones para biblioteca de rutinas
function guardarRutina() {
  const nombre = prompt("Nombre para esta rutina:", nombreRutinaActual.includes("Sin Guardar") ? "" : nombreRutinaActual);
  if (nombre) {
    const rutinasGuardadas = JSON.parse(localStorage.getItem("rutinasGuardadas") || "[]");
    const existingIndex = rutinasGuardadas.findIndex(r => r.nombre === nombre);
    
    if (existingIndex !== -1) {
        if (confirm(`Una rutina con el nombre "${nombre}" ya existe. ¿Deseas reemplazarla?`)) {
            rutinasGuardadas[existingIndex] = { nombre, rondas: JSON.stringify(rondas) };
            localStorage.setItem("rutinasGuardadas", JSON.stringify(rutinasGuardadas));
            localStorage.setItem("lastLoadedRoutine", nombre);
            rutinaModificada = false;
            alert("Rutina actualizada.");
        } else {
            alert("Guardado cancelado.");
            return;
        }
    } else {
        rutinasGuardadas.push({ nombre, rondas: JSON.stringify(rondas) });
        localStorage.setItem("rutinasGuardadas", JSON.stringify(rutinasGuardadas));
        localStorage.setItem("lastLoadedRoutine", nombre);
        rutinaModificada = false;
        alert("Rutina guardada como: " + nombre);
    }
    
    nombreRutinaActual = nombre;
    actualizarNombreRutina();
    actualizarListaRutinasGuardadas();
  }
}
function actualizarListaRutinasGuardadas() {
  const rutinasGuardadas = JSON.parse(localStorage.getItem("rutinasGuardadas") || "[]");
  listaRutinasGuardadas.innerHTML = "";
  if (rutinasGuardadas.length === 0) {
    const li = document.createElement("li");
    li.textContent = "No hay rutinas guardadas";
    li.style.color = "#999";
    li.style.cursor = "default";
    listaRutinasGuardadas.appendChild(li);
    return;
  }
  rutinasGuardadas.forEach((rutina, idx) => {
    const li = document.createElement("li");
    li.textContent = rutina.nombre;
    li.onclick = () => cargarRutinaEspecifica(rutina);
    
    const btnRenombrar = document.createElement("button");
    btnRenombrar.textContent = "Renombrar";
    btnRenombrar.onclick = (ev) => {
      ev.stopPropagation();
      const nuevoNombre = prompt("Nuevo nombre:", rutina.nombre);
      if (nuevoNombre) {
        rutinasGuardadas[idx].nombre = nuevoNombre;
        localStorage.setItem("rutinasGuardadas", JSON.stringify(rutinasGuardadas));
        actualizarListaRutinasGuardadas();
      }
    };
    
    const btnEliminar = document.createElement("button");
    btnEliminar.textContent = "Eliminar";
    btnEliminar.onclick = (ev) => {
      ev.stopPropagation();
      if (confirm("¿Eliminar '" + rutina.nombre + "'?")) {
        rutinasGuardadas.splice(idx, 1);
        localStorage.setItem("rutinasGuardadas", JSON.stringify(rutinasGuardadas));
        actualizarListaRutinasGuardadas();
        // Si eliminamos la rutina que estaba cargada, volvemos a la predeterminada
        if (nombreRutinaActual === rutina.nombre) {
            cargarRutinaInicial();
        }
      }
    };
    
    li.appendChild(btnRenombrar);
    li.appendChild(btnEliminar);
    listaRutinasGuardadas.appendChild(li);
  });
}

function cargarRutinaInicial() {
    const lastLoadedName = localStorage.getItem("lastLoadedRoutine");
    const rutinasGuardadas = JSON.Sondeando.parse(localStorage.getItem("rutinasGuardadas") || "[]");
    let rutinaACargar;

    if (lastLoadedName) {
        rutinaACargar = rutinasGuardadas.find(r => r.nombre === lastLoadedName);
    }

    if (rutinaACargar) {
        rondas = JSON.parse(rutinaACargar.rondas);
        nombreRutinaActual = rutinaACargar.nombre;
    } else {
        rondas = JSON.parse(JSON.stringify(rutinaPredeterminada)); // Copia profunda
        nombreRutinaActual = "Rutina Predeterminada";
        localStorage.removeItem("lastLoadedRoutine");
    }

    rutinaModificada = false;
    actualizarNombreRutina();
    totalEjercicios = rondas.flat().length;
    actualizarListaRondas();
    seleccionRonda.value = 0;
    actualizarListaEjercicios();
    actualizarDuracionTotal();
}


function cargarRutinaEspecifica(rutina) {
    if (rutinaModificada && !confirm("Tienes cambios sin guardar. ¿Cargar '" + rutina.nombre + "'?")) {
        return;
    }
    rondas = JSON.parse(rutina.rondas);
    nombreRutinaActual = rutina.nombre;
    localStorage.setItem("lastLoadedRoutine", nombreRutinaActual);
    rutinaModificada = false;
    actualizarNombreRutina();
    totalEjercicios = rondas.flat().length;
    actualizarListaRondas();
    seleccionRonda.value = 0;
    actualizarListaEjercicios();
    actualizarDuracionTotal();
    alert("Rutina cargada: " + rutina.nombre);
}


// Funciones para log de sesiones
function eliminarEntradaHistorial(index) {
  if (confirm("¿Estás seguro de que quieres eliminar este registro?")) {
    const completadas = JSON.parse(localStorage.getItem("rutinasCompletadas") || "[]");
    completadas.splice(index, 1);
    localStorage.setItem("rutinasCompletadas", JSON.stringify(completadas));
    actualizarListaHistorial();
    actualizarGráficoProgreso();
  }
}

function registrarCompletacion() {
  const tiempoFinalSesion = new Date();
  const duracionMs = tiempoFinalSesion - tiempoInicioSesion;
  const duracionMin = Math.floor(duracionMs / 60000);
  const duracionSeg = Math.floor((duracionMs % 60000) / 1000);

  const completadas = JSON.parse(localStorage.getItem("rutinasCompletadas") || "[]");
  completadas.push({
    nombre: nombreRutinaActual,
    fecha: new Date().toLocaleDateString('es-ES', { day: '2-digit', month: '2-digit', year: '2-digit' }),
    duracion: `${duracionMin}m ${duracionSeg}s`
  });
  localStorage.setItem("rutinasCompletadas", JSON.stringify(completadas));
  actualizarListaHistorial();
  actualizarGráficoProgreso();
  mostrarPestana('log');
  reiniciarApp();
}
function actualizarListaHistorial() {
  const completadas = JSON.parse(localStorage.getItem("rutinasCompletadas") || "[]");
  listaHistorial.innerHTML = "";
  completadas.forEach((item, index) => {
    const li = document.createElement("li");
    li.textContent = `${item.nombre} - ${item.fecha} - Duración: ${item.duracion || 'N/A'}`;
    li.onclick = () => eliminarEntradaHistorial(index);
    listaHistorial.appendChild(li);
  });
}
limpiarHistorialBtn.addEventListener("click", () => {
  if (confirm("¿Limpiar log?")) {
    localStorage.removeItem("rutinasCompletadas");
    actualizarListaHistorial();
    actualizarGráficoProgreso();
  }
});

// Inicializar al cargar página
document.addEventListener('DOMContentLoaded', () => {
    cargarRutinaInicial();
    actualizarListaRutinasGuardadas();
    actualizarListaHistorial();
    actualizarGráficoProgreso();
});


// Gestión de rutina (editor)
function limpiarRutinaParaEdicion() {
  if (rutinaModificada && !confirm("Tienes cambios sin guardar. ¿Crear una nueva rutina en blanco?")) {
        return;
  }
  nombreRutinaActual = "Nueva Rutina";
  rondas = [
    []
  ];
  totalEjercicios = 0;
  
  rutinaModificada = true;
  actualizarNombreRutina();
  localStorage.removeItem("lastLoadedRoutine");
  
  actualizarListaRondas();
  seleccionRonda.value = 0;
  actualizarListaEjercicios();
  actualizarDuracionTotal();
  alert("Editor limpiado. ¡Listo para crear tu nueva rutina!");
}
function actualizarListaRondas(){
  seleccionRonda.innerHTML = "";
  rondas.forEach((r, idx) => {
    const o = document.createElement("option");
    o.value = idx;
    o.textContent = `Ronda ${idx + 1}`;
    seleccionRonda.appendChild(o);
  });
}
function actualizarListaEjercicios(){
  listaEjercicios.innerHTML = "";
  const rondaIdx = parseInt(seleccionRonda.value);
  const ronda = rondas[rondaIdx];
  const rondaTitle = document.createElement("h3");
  rondaTitle.textContent = `Ronda ${rondaIdx + 1}`;
  listaEjercicios.appendChild(rondaTitle);
  if (!ronda || ronda.length === 0) {
    const li = document.createElement("li");
    li.textContent = "No hay ejercicios en esta ronda";
    li.style.color = "#999";
    li.style.padding = "8px";
    listaEjercicios.appendChild(li);
  } else {
    ronda.forEach((e, ejercicioIdx) => {
      const li = document.createElement("li");
      const tipoConteoTexto = e.tipoConteo === 'repeticiones' ? `${e.repeticiones} reps` : `${e.duracion}s`;
      li.textContent = `${e.nombre} - ${tipoConteoTexto} - Series: ${e.series} - Descanso: ${e.descanso}s`;
      li.style.backgroundColor = e.color;
      li.onclick = () => {
        document.getElementById("nombreEjercicio").value = e.nombre;
        document.getElementById("tipoConteo").value = e.tipoConteo;
        if (e.tipoConteo === 'tiempo') {
            duracionInput.style.display = 'block';
            repeticionesInput.style.display = 'none';
            document.getElementById("duracionEjercicio").value = e.duracion;
        } else {
            duracionInput.style.display = 'none';
            repeticionesInput.style.display = 'block';
            document.getElementById("repeticionesEjercicio").value = e.repeticiones;
        }
        document.getElementById("seriesEjercicio").value = e.series;
        document.getElementById("descansoEjercicio").value = e.descanso;
        document.getElementById("descripcionEjercicio").value = e.descripcion;
        editarIndex = ejercicioIdx;
        seleccionRonda.value = rondaIdx;
      };
      const btnEliminar = document.createElement("button");
      btnEliminar.textContent = "Eliminar";
      btnEliminar.onclick = (ev) => {
        ev.stopPropagation();
        if (confirm("¿Seguro quieres eliminar este ejercicio?")) {
          rondas[rondaIdx].splice(ejercicioIdx, 1);
          marcarRutinaModificada();
          actualizarListaEjercicios();
          actualizarListaRondas();
          totalEjercicios = rondas.flat().length;
          actualizarDuracionTotal();
        }
      };
      li.appendChild(btnEliminar);
      listaEjercicios.appendChild(li);
    });
  }
}
seleccionRonda.addEventListener("change", () => {
  editarIndex = null;
  document.getElementById("nombreEjercicio").value = "";
  document.getElementById("tipoConteo").value = "tiempo";
  duracionInput.style.display = 'block';
  repeticionesInput.style.display = 'none';
  document.getElementById("duracionEjercicio").value = "";
  document.getElementById("repeticionesEjercicio").value = "";
  document.getElementById("seriesEjercicio").value = "";
  document.getElementById("descansoEjercicio").value = "5"; // Mantiene el valor por defecto
  document.getElementById("descripcionEjercicio").value = "";
  actualizarListaEjercicios();
});
document.getElementById("agregarEjercicioBtn").addEventListener("click", () => {
  const rondaIdx = parseInt(seleccionRonda.value);
  const nombre = document.getElementById("nombreEjercicio").value;
  const tipoConteo = tipoConteoSelect.value;
  const series = parseInt(document.getElementById("seriesEjercicio").value);
  const descanso = parseInt(document.getElementById("descansoEjercicio").value);
  const descripcion = document.getElementById("descripcionEjercicio").value;
  const tipo = "ejercicio";
  const color = "#2ecc71";
  let nuevoEjercicio;

  if (tipoConteo === 'tiempo') {
    const dur = parseInt(document.getElementById("duracionEjercicio").value);
    if (nombre && dur > 0 && series > 0 && descanso >= 0) {
      nuevoEjercicio = { nombre, duracion: dur, descanso, tipo, series, color, descripcion, tipoConteo };
    } else {
      alert("Por favor, rellena todos los campos correctamente para un ejercicio de tiempo.");
      return;
    }
  } else { // 'repeticiones'
    const reps = parseInt(document.getElementById("repeticionesEjercicio").value);
    if (nombre && reps > 0 && series > 0 && descanso >= 0) {
      nuevoEjercicio = { nombre, repeticiones: reps, descanso, tipo, series, color, descripcion, tipoConteo };
    } else {
      alert("Por favor, rellena todos los campos correctamente para un ejercicio de repeticiones.");
      return;
    }
  }

  if (editarIndex !== null) {
    rondas[rondaIdx][editarIndex] = nuevoEjercicio;
    editarIndex = null;
  } else {
    rondas[rondaIdx].push(nuevoEjercicio);
  }
  marcarRutinaModificada();
  document.getElementById("nombreEjercicio").value = "";
  document.getElementById("duracionEjercicio").value = "";
  document.getElementById("repeticionesEjercicio").value = "";
  document.getElementById("seriesEjercicio").value = "";
  document.getElementById("descansoEjercicio").value = "5";
  document.getElementById("descripcionEjercicio").value = "";
  tipoConteoSelect.value = "tiempo";
  duracionInput.style.display = 'block';
  repeticionesInput.style.display = 'none';
  actualizarListaEjercicios();
  totalEjercicios = rondas.flat().length;
  actualizarDuracionTotal();
});

document.getElementById("limpiarRutinaBtn").addEventListener("click", limpiarRutinaParaEdicion);
document.getElementById("nuevaRondaBtn").addEventListener("click", () => {
  rondas.push([]);
  marcarRutinaModificada();
  actualizarListaRondas();
  seleccionRonda.value = rondas.length - 1;
  actualizarListaEjercicios();
});
document.getElementById("eliminarRondaBtn").addEventListener("click", () => {
  if (rondas.length > 1) {
    const rondaIdx = parseInt(seleccionRonda.value);
    if (confirm(`¿Eliminar la Ronda ${rondaIdx + 1}?`)) {
      rondas.splice(rondaIdx, 1);
      marcarRutinaModificada();
      actualizarListaRondas();
      seleccionRonda.value = Math.max(0, rondaIdx - 1);
      actualizarListaEjercicios();
      totalEjercicios = rondas.flat().length;
      actualizarDuracionTotal();
    }
  } else {
    alert("No puedes eliminar la última ronda.");
  }
});
guardarRutinaEditorBtn.addEventListener("click", guardarRutina);

// Funciones del entrenamiento
function reiniciarApp() {
    clearInterval(interval);
    clearInterval(prepInterval);
    clearInterval(descansoInterval);
    ejercicioDiv.classList.remove("fade-out");
    ejercicioDiv.style.background = 'var(--accent-blue)';
    ejercicioTextDiv.textContent = "Iniciar";
    descripcionDiv.textContent = "¡Presiona para comenzar!";
    tiempoDiv.textContent = "00:00";
    tiempoDiv.style.display = 'block';
    repeticionesDiv.textContent = "";
    pauseBtn.style.display = 'inline-block';
    resumeBtn.style.display = 'inline-block';
    progressBar.style.display = 'block';
    
    pausado = false;
    pauseBtn.disabled = true;
    resumeBtn.disabled = true;
    prevEjercicioBtn.disabled = true;
    nextEjercicioBtn.disabled = true;
    ejerciciosCompletados = 0;
    rondaIndex = 0;
    ejercicioIndex = 0;
    serieActual = 1;
    progressBar.style.width = "0%";
    progressTotalBar.style.width = "0%";
    progressTotalText.textContent = "0%";
    ejercicioDiv.style.cursor = "pointer";
    actualizarDuracionTotal();
    releaseWakeLock();
    modalCompletado.classList.remove("active");
}
function avanzarEjercicio() {
  clearInterval(interval);
  clearInterval(descansoInterval);
  
  const ejercicioAnterior = rondas[rondaIndex][ejercicioIndex - 1];
  
  // Condición para avanzar si se completaron todas las series o si el ejercicio anterior fue de tiempo
  if (ejercicioAnterior && (ejercicioAnterior.tipoConteo === 'tiempo' || serieActual > ejercicioAnterior.series)) {
    ejerciciosCompletados++;
    // Actualizar progreso total
    const progresoTotal = (ejerciciosCompletados / totalEjercicios) * 100;
    progressTotalBar.style.width = `${progresoTotal}%`;
    progressTotalText.textContent = `${Math.round(progresoTotal)}%`;
  }
  
  if (ejercicioIndex < rondas[rondaIndex].length) {
    const ejercicio = rondas[rondaIndex][ejercicioIndex];
    ejercicioDiv.style.background = ejercicio.color;
    ejercicioTextDiv.textContent = ejercicio.nombre;
    descripcionDiv.textContent = ejercicio.descripcion;
    repeticionesDiv.textContent = `Serie ${serieActual} de ${ejercicio.series}`;
    
    if (ejercicio.tipoConteo === 'tiempo') {
      tiempoDiv.style.display = 'block';
      progressBar.style.display = 'block';
      pauseBtn.style.display = 'inline-block';
      resumeBtn.style.display = 'inline-block';
      nextEjercicioBtn.disabled = false;
      tiempoRestante = ejercicio.duracion;
      iniciarTemporizador(ejercicio);
    } else { // 'repeticiones'
      tiempoDiv.style.display = 'none';
      progressBar.style.display = 'none';
      pauseBtn.style.display = 'none';
      resumeBtn.style.display = 'none';
      repeticionesDiv.textContent = `Serie ${serieActual} de ${ejercicio.series} - ${ejercicio.repeticiones} Reps.`;
      
      progressBar.style.width = `${100 * (serieActual - 1) / ejercicio.series}%`;
      nextEjercicioBtn.disabled = false;
    }
  } else {
    if (rondaIndex < rondas.length - 1) {
      rondaIndex++;
      ejercicioIndex = 0;
      iniciarDescanso(5);
    } else {
      mostrarModalCompletado();
    }
  }
}

function iniciarDescanso(duracion) {
  tiempoRestante = duracion;
  ejercicioDiv.style.background = "#e67e22";
  ejercicioTextDiv.textContent = "¡Descanso!";
  const siguienteEjercicio = rondas[rondaIndex][ejercicioIndex];
  if(siguienteEjercicio) {
    descripcionDiv.textContent = `Siguiente: ${siguienteEjercicio.nombre}`;
  } else {
    descripcionDiv.textContent = "Último descanso!";
  }
  repeticionesDiv.textContent = "";

  tiempoDiv.style.display = 'block';
  progressBar.style.display = 'block';
  pauseBtn.style.display = 'inline-block';
  resumeBtn.style.display = 'inline-block';
  
  descansoInterval = setInterval(() => {
    if (!pausado) {
      tiempoRestante--;
      if (tiempoRestante <= 3 && tiempoRestante > 0) {
        tiempoDiv.classList.add('warning');
        reproducirSonido();
      } else {
        tiempoDiv.classList.remove('warning');
      }
      const min = Math.floor(tiempoRestante / 60);
      const sec = tiempoRestante % 60;
      tiempoDiv.textContent = `${min.toString().padStart(2, '0')}:${sec.toString().padStart(2, '0')}`;
      progressBar.style.width = `${100 - (tiempoRestante / duracion * 100)}%`;
      
      if (tiempoRestante <= 0) {
        clearInterval(descansoInterval);
        sonidoInicio();
        avanzarEjercicio();
      }
    }
  }, 1000);
}

function iniciarTemporizador(ejercicio) {
  const duracionTotalEjercicio = ejercicio.duracion;
  interval = setInterval(() => {
    if (!pausado) {
      tiempoRestante--;
      if (tiempoRestante <= 3 && tiempoRestante > 0) {
        tiempoDiv.classList.add('warning');
        reproducirSonido();
      } else {
        tiempoDiv.classList.remove('warning');
      }
      const min = Math.floor(tiempoRestante / 60);
      const sec = tiempoRestante % 60;
      tiempoDiv.textContent = `${min.toString().padStart(2, '0')}:${sec.toString().padStart(2, '0')}`;
      progressBar.style.width = `${100 - (tiempoRestante / duracionTotalEjercicio * 100)}%`;
      
      if (tiempoRestante <= 0) {
        clearInterval(interval);
        sonidoInicio();
        serieActual++;
        if (serieActual > ejercicio.series) {
            ejercicioIndex++;
            serieActual = 1;
            avanzarEjercicio();
        } else {
            iniciarDescanso(ejercicio.descanso);
        }
      }
    }
  }, 1000);
}
function iniciarEntrenamiento() {
    if (!rondas || !rondas.flat().length) {
        alert("¡No hay ejercicios en la rutina! Por favor, añade ejercicios en la pestaña 'Editor'.");
        return;
    }
    clearInterval(interval);
    clearInterval(prepInterval);
    clearInterval(descansoInterval);
    
    // Bloquear la pantalla
    isTimerRunning = true;
    requestWakeLock();
    retryWakeLock(); // Inicia el reintento de bloqueo
    
    // Reiniciar contadores para un nuevo inicio
    rondaIndex=0; 
    ejercicioIndex=0; 
    serieActual=1;
    ejerciciosCompletados=0;
    
    ejercicioDiv.classList.remove("fade-out");
    ejercicioDiv.style.background = 'var(--accent-blue-dark)';
    ejercicioTextDiv.textContent = "Preparación...";
    descripcionDiv.textContent = "";
    repeticionesDiv.textContent = "";
    pauseBtn.disabled = false;
    resumeBtn.disabled = true;
    prevEjercicioBtn.disabled = false;
    nextEjercicioBtn.disabled = false;
    ejercicioDiv.style.cursor = "default";
    
    // Mostrar u ocultar controles de temporizador según el primer ejercicio
    const primerEjercicio = rondas[0][0];
    if (primerEjercicio && primerEjercicio.tipoConteo === 'repeticiones') {
        tiempoDiv.style.display = 'none';
        progressBar.style.display = 'none';
        pauseBtn.style.display = 'none';
        resumeBtn.style.display = 'none';
    } else {
        tiempoDiv.style.display = 'block';
        progressBar.style.display = 'block';
        pauseBtn.style.display = 'inline-block';
        resumeBtn.style.display = 'inline-block';
    }
    
    let conteo = 5;
    sonidoInicio();
    ejercicioTextDiv.textContent = `Preparación... ${conteo}`;
    
    prepInterval = setInterval(() => {
      conteo--;
      if (conteo > 0) {
        ejercicioTextDiv.textContent = `Preparación... ${conteo}`;
        reproducirSonido();
      } else {
        clearInterval(prepInterval);
        tiempoInicioSesion = new Date();
        avanzarEjercicio();
      }
    }, 1000);
}

function mostrarModalCompletado() {
    clearInterval(interval);
    clearInterval(prepInterval);
    clearInterval(descansoInterval);
    releaseWakeLock();
    modalCompletado.classList.add("active");
}


// Botones del entrenamiento
ejercicioDiv.onclick = iniciarEntrenamiento;

pauseBtn.addEventListener("click", () => {
  pausado = true;
  pauseBtn.disabled = true;
  resumeBtn.disabled = false;
  releaseWakeLock();
});
resumeBtn.addEventListener("click", () => {
  pausado = false;
  pauseBtn.disabled = false;
  resumeBtn.disabled = true;
  requestWakeLock();
  retryWakeLock();
});
resetBtn.addEventListener("click", reiniciarApp);

prevEjercicioBtn.addEventListener("click", () => {
  clearInterval(interval);
  clearInterval(descansoInterval);

  const ejercicioAnterior = rondas[rondaIndex][ejercicioIndex];

  if (serieActual > 1) {
    serieActual--;
  } else {
    // Si no es la primera serie, o si estamos al inicio, retroceder al ejercicio anterior.
    if (ejercicioIndex > 0) {
      ejercicioIndex--;
    } else if (rondaIndex > 0) {
      rondaIndex--;
      ejercicioIndex = rondas[rondaIndex].length - 1;
    } else {
      reiniciarApp();
      return;
    }
    // Después de retroceder al ejercicio anterior, volvemos a la última serie de ese ejercicio.
    serieActual = rondas[rondaIndex][ejercicioIndex].series;
  }

  // Descontar progreso solo si no es un ejercicio por repeticiones (no hay progreso parcial)
  if (ejercicioAnterior.tipoConteo !== 'repeticiones') {
    ejerciciosCompletados = Math.max(0, ejerciciosCompletados - 1);
  }

  const progresoTotal = (ejerciciosCompletados / totalEjercicios) * 100;
  progressTotalBar.style.width = `${progresoTotal}%`;
  progressTotalText.textContent = `${Math.round(progresoTotal)}%`;

  avanzarEjercicio();
});


nextEjercicioBtn.addEventListener("click", () => {
  clearInterval(interval);
  clearInterval(descansoInterval);
  const ejercicio = rondas[rondaIndex][ejercicioIndex];
  
  if (ejercicio.tipoConteo === 'repeticiones') {
      if (serieActual < ejercicio.series) {
          serieActual++;
      } else {
          ejerciciosCompletados++;
          const progresoTotal = (ejerciciosCompletados / totalEjercicios) * 100;
          progressTotalBar.style.width = `${progresoTotal}%`;
          progressTotalText.textContent = `${Math.round(progresoTotal)}%`;
          ejercicioIndex++;
          serieActual = 1;
      }
  } else {
      ejerciciosCompletados++;
      const progresoTotal = (ejerciciosCompletados / totalEjercicios) * 100;
      progressTotalBar.style.width = `${progresoTotal}%`;
      progressTotalText.textContent = `${Math.round(progresoTotal)}%`;
      ejercicioIndex++;
      serieActual = 1;
  }
  
  if (ejercicioIndex >= rondas[rondaIndex].length) {
    if (rondaIndex < rondas.length - 1) {
      rondaIndex++;
      ejercicioIndex = 0;
      iniciarDescanso(5);
      return;
    } else {
      mostrarModalCompletado();
      return;
    }
  }

  avanzarEjercicio();
});


document.getElementById("aceptarCompletado").addEventListener("click", registrarCompletacion);
document.getElementById("cancelarCompletado").addEventListener("click", reiniciarApp);

</script>
</body>
</html>
