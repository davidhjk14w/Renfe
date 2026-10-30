<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>RailTrack España | Live Control</title>

<link rel="stylesheet"
href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">

<style>
:root {
  --bg:#080d18;
  --panel:#111b2b;
  --panel2:#17243a;
  --border:#263650;
  --text:#f1f5ff;
  --muted:#8c9db6;
  --green:#42e6b1;
  --blue:#65a4ff;
  --red:#ff6376;
  --orange:#ffbd59;
}

* {box-sizing:border-box}

body {
  margin:0;
  background:var(--bg);
  color:var(--text);
  font-family:Inter,Arial,sans-serif;
}

header {
  height:76px;
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:0 25px;
  border-bottom:1px solid var(--border);
  background:#0c1422;
}

.brand {
  display:flex;
  align-items:center;
  gap:12px;
}

.logo {
  background:#163b3c;
  color:var(--green);
  width:42px;
  height:42px;
  display:grid;
  place-items:center;
  border-radius:12px;
  font-size:24px;
}

h1 {
  margin:0;
  font-size:21px;
}

h1 span {color:var(--green)}

.subtitle {
  color:var(--muted);
  font-size:10px;
  letter-spacing:1.5px;
}

.connection {
  font-size:11px;
  color:var(--green);
  text-align:right;
}

.connection small {
  display:block;
  color:var(--muted);
  margin-top:4px;
}

.dot {
  display:inline-block;
  width:8px;
  height:8px;
  border-radius:50%;
  background:var(--green);
  margin-right:6px;
  box-shadow:0 0 10px var(--green);
}

main {
  max-width:1800px;
  margin:auto;
  padding:22px;
}

.hero {
  display:flex;
  justify-content:space-between;
  align-items:center;
  gap:20px;
  margin-bottom:22px;
}

.hero h2 {
  font-size:35px;
  margin:0;
  letter-spacing:-1px;
}

.hero h2 span {color:var(--green)}

.hero p {
  color:var(--muted);
  font-size:13px;
}

.stats {
  display:flex;
  gap:10px;
}

.stat {
  background:var(--panel);
  border:1px solid var(--border);
  padding:14px 20px;
  border-radius:12px;
  min-width:115px;
}

.stat strong {
  display:block;
  font-size:25px;
}

.stat small {
  color:var(--muted);
  font-size:10px;
}

.toolbar {
  display:flex;
  flex-wrap:wrap;
  gap:10px;
  margin-bottom:14px;
}

input,select,button {
  background:var(--panel);
  border:1px solid var(--border);
  color:white;
  border-radius:9px;
  padding:12px;
  font-size:12px;
}

input {
  flex:1;
  min-width:180px;
}

button {
  cursor:pointer;
  background:#164039;
  color:var(--green);
  font-weight:bold;
}

button:hover {filter:brightness(1.3)}

.company-filters {
  display:flex;
  flex-wrap:wrap;
  gap:8px;
  margin-bottom:15px;
}

.company {
  border:1px solid var(--border);
  background:var(--panel);
  color:var(--text);
  border-radius:9px;
  padding:9px 13px;
  font-size:11px;
  cursor:pointer;
}

.company.active {
  border-color:var(--green);
  color:var(--green);
  background:#12342e;
}

.layout {
  display:grid;
  grid-template-columns:minmax(0,1fr) 360px;
  gap:15px;
}

.panel {
  background:var(--panel);
  border:1px solid var(--border);
  border-radius:14px;
  overflow:hidden;
  min-width:0;
}

.panel-header {
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:16px;
  border-bottom:1px solid var(--border);
}

.panel-header strong {font-size:13px}

.panel-header small {
  display:block;
  color:var(--muted);
  font-size:10px;
  margin-top:4px;
}

.badge {
  font-size:9px;
  color:var(--green);
  border:1px solid #276452;
  padding:5px 8px;
  border-radius:20px;
}

#map {
  height:650px;
  background:#101a2a;
}

.train-list {
  height:650px;
  overflow-y:auto;
}

.train-card {
  padding:15px;
  border-bottom:1px solid var(--border);
  cursor:pointer;
}

.train-card:hover {
  background:var(--panel2);
}

.train-card.selected {
  background:#193247;
  border-left:3px solid var(--green);
}

.train-top {
  display:flex;
  justify-content:space-between;
  gap:8px;
  align-items:center;
}

.operator {
  font-weight:bold;
  font-size:12px;
}

.operator-logo {
  display:inline-block;
  padding:4px 7px;
  border-radius:5px;
  margin-right:5px;
  background:#26364e;
  font-size:10px;
  font-weight:900;
}

.train-number {
  color:var(--green);
  font-size:11px;
}

.route {
  font-size:13px;
  font-weight:bold;
  margin:10px 0;
}

.train-info {
  display:flex;
  flex-wrap:wrap;
  gap:7px;
  font-size:10px;
  color:var(--muted);
}

.status {
  padding:5px 7px;
  border-radius:5px;
  font-size:10px;
  font-weight:bold;
}

.ok {
  background:#123c32;
  color:var(--green);
}

.late {
  background:#49351b;
  color:var(--orange);
}

.problem {
  background:#4a202a;
  color:var(--red);
}

.no-data {
  color:var(--muted);
}

.empty {
  padding:25px;
  text-align:center;
  color:var(--muted);
  font-size:12px;
}

.train-marker {
  background:transparent;
  border:0;
}

.train-symbol {
  font-size:27px;
  filter:drop-shadow(0 2px 5px #000);
  transform:rotate(0deg);
}

.leaflet-popup-content-wrapper,
.leaflet-popup-tip {
  background:#142136;
  color:white;
}

.leaflet-popup-content {
  font-size:12px;
  line-height:1.6;
}

footer {
  color:var(--muted);
  padding:22px 0;
  font-size:10px;
  line-height:1.7;
}

footer a {color:var(--blue)}

@media(max-width:1000px) {
  .layout {grid-template-columns:1fr}
  .train-list {height:450px}
  #map {height:500px}
  .hero {display:block}
  .stats {margin-top:20px;flex-wrap:wrap}
}

@media(max-width:600px) {
  main {padding:12px}
  header {padding:12px}
  .hero h2 {font-size:28px}
  .stat {flex:1;min-width:90px;padding:10px}
  .stat strong {font-size:20px}
  #map {height:400px}
  .connection {font-size:9px}
}
</style>
</head>

<body>

<header>
  <div class="brand">
    <div class="logo">🚄</div>
    <div>
      <h1>RailTrack <span>España</span></h1>
      <div class="subtitle">LIVE RAILWAY CONTROL SYSTEM</div>
    </div>
  </div>

  <div class="connection">
    <span class="dot"></span>
    <span id="connection">CONECTANDO</span>
    <small id="lastUpdate">Esperando datos...</small>
  </div>
</header>

<main>

<section class="hero">
  <div>
    <h2>Control ferroviario <span>en directo</span></h2>
    <p>Seguimiento nacional · Prioridad Cuenca – Madrid</p>
  </div>

  <div class="stats">
    <div class="stat">
      <strong id="total">0</strong>
      <small>TRENES</small>
    </div>

    <div class="stat">
      <strong id="delayed">0</strong>
      <small>RETRASADOS</small>
    </div>

    <div class="stat">
      <strong id="incidents">0</strong>
      <small>INCIDENCIAS PUBLICADAS</small>
    </div>

    <div class="stat">
      <strong id="cuenca">0</strong>
      <small>CUENCA</small>
    </div>
  </div>
</section>

<section class="toolbar">

  <input id="search"
    placeholder="Buscar número de tren, origen o destino...">

  <select id="filter">
    <option value="all">Todos los trenes</option>
    <option value="madrid">Destino Madrid</option>
    <option value="atocha">Puerta de Atocha</option>
    <option value="chamartin">Chamartín</option>
    <option value="cuenca">Cuenca - Madrid</option>
    <option value="delayed">Solo retrasados</option>
    <option value="incidents">Con incidencias publicadas</option>
  </select>

  <button id="refresh">↻ Actualizar</button>

</section>

<div class="company-filters" id="companies">
  <button class="company active" data-company="all">TODAS</button>
  <button class="company" data-company="renfe">🔴 RENFE</button>
  <button class="company" data-company="avlo">🟣 AVLO</button>
  <button class="company" data-company="iryo">🔴 iryo</button>
  <button class="company" data-company="ouigo">🔵 OUIGO</button>
</div>

<section class="layout">

  <div class="panel">

    <div class="panel-header">
      <div>
        <strong>Mapa GPS de España</strong>
        <small>Las posiciones se actualizan al recibir nuevos datos</small>
      </div>

      <span class="badge">LIVE MAP</span>
    </div>

    <div id="map"></div>

  </div>

  <aside class="panel">

    <div class="panel-header">
      <div>
        <strong>Trenes detectados</strong>
        <small id="resultCount">Esperando información</small>
      </div>
      <span class="badge">ESPAÑA</span>
    </div>

    <div class="train-list" id="trainList">
      <div class="empty">Conectando con la red ferroviaria...</div>
    </div>

  </aside>

</section>

<footer>
  RailTrack España · Proyecto independiente y no oficial.<br>
  Fuente de posiciones y datos: <a href="https://radardetrenes.com" target="_blank">RadarDeTrenes</a>,
  que agrega información pública de Renfe y ADIF.<br>
  Mapa: OpenStreetMap. Las posiciones GPS pueden tener retraso.
  Las averías y cancelaciones solo se mostrarán si la fuente las publica.
  La ausencia de una incidencia publicada no significa que no exista.
</footer>

</main>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

const API = "https://radardetrenes.com/api/v1";

const map = L.map("map").setView([40.2,-3.5],6);

L.tileLayer(
  "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  {
    maxZoom:19,
    attribution:"© OpenStreetMap"
  }
).addTo(map);

let trains = [];
let markers = new Map();
let selectedCompany = "all";
let selectedTrain = null;
let stream = null;

const $ = id => document.getElementById(id);

function normalize(v) {
  return String(v ?? "")
    .normalize("NFD")
    .replace(/[\u0300-\u036f]/g,"")
    .toLowerCase();
}

function field(t,...keys) {
  for(const k of keys) {
    if(t[k] !== undefined && t[k] !== null) {
      return t[k];
    }
  }
  return "";
}

function origin(t) {
  return field(t,"originName","origin","from");
}

function destination(t) {
  return field(t,"destinationName","destination","to");
}

function id(t) {
  return String(field(t,"id","trainId","trainCode","number"));
}

function company(t) {
  const p = normalize(
    field(t,"productName","operator","company","type")
  );

  if(p.includes("iryo")) return "iryo";
  if(p.includes("ouigo")) return "ouigo";
  if(p.includes("avlo")) return "avlo";
  if(p.includes("renfe") || p.includes("ave") ||
     p.includes("alvia") || p.includes("avant")) return "renfe";

  return "other";
}

function companyName(t) {
  return field(t,"productName","operator","company","type")
    || "Tren";
}

function delay(t) {
  const n = Number(field(t,"delayMinutes","delay"));
  return Number.isFinite(n) ? n : null;
}

function isIncident(t) {

  const status = normalize(
    field(t,"incident","incidentStatus","status","alert")
  );

  return Boolean(
    t.incident === true ||
    status.includes("averia") ||
    status.includes("incidencia") ||
    status.includes("breakdown") ||
    status.includes("disruption")
  );
}

function isCuenca(t) {
  return normalize(origin(t)+" "+destination(t))
    .includes("cuenca");
}

function isMadrid(t) {
  return normalize(destination(t)).includes("madrid");
}

function matches(t) {

  const q = normalize($("search").value);
  const f = $("filter").value;

  const text = normalize([
    id(t),origin(t),destination(t),companyName(t)
  ].join(" "));

  if(q && !text.includes(q)) return false;

  if(selectedCompany !== "all" &&
     company(t) !== selectedCompany) return false;

  if(f==="madrid" && !isMadrid(t)) return false;

  if(f==="atocha" &&
     !normalize(destination(t)).includes("atocha")) return false;

  if(f==="chamartin" &&
     !normalize(destination(t)).includes("chamartin")) return false;

  if(f==="cuenca" && !isCuenca(t)) return false;

  if(f==="delayed" && !(delay(t)>0)) return false;

  if(f==="incidents" && !isIncident(t)) return false;

  return true;
}

function safe(v) {
  return String(v ?? "No disponible")
    .replace(/[&<>"']/g,c => ({
      "&":"&amp;",
      "<":"&lt;",
      ">":"&gt;",
      '"':"&quot;",
      "'":"&#39;"
    })[c]);
}

function logo(t) {
  const c=company(t);

  const labels={
    renfe:"RENFE",
    avlo:"AVLO",
    iryo:"iryo",
    ouigo:"OUIGO",
    other:"TREN"
  };

  return `<span class="operator-logo">${labels[c]}</span>`;
}

function icon() {

  return L.divIcon({
    className:"train-marker",
    html:`<div class="train-symbol">🚄</div>`,
    iconSize:[35,35],
    iconAnchor:[17,17]
  });

}

function popup(t) {

  const d=delay(t);

  return `
    <strong>${safe(companyName(t))} ${safe(id(t))}</strong>
    <br>
    ${safe(origin(t))} → ${safe(destination(t))}
    <br>
    Retraso: ${d===null?"No publicado":d+" minutos"}
    <br>
    Próxima parada:
    ${safe(field(t,"nextStationName","nextStation")||"No publicada")}
  `;
}

function animateMarker(marker, from, to, duration=1200) {

  if(!from || !to) {
    marker.setLatLng(to);
    return;
  }

  const start=performance.now();

  function step(now) {

    const progress=Math.min((now-start)/duration,1);

    const eased=progress<.5
      ? 2*progress*progress
      : 1-Math.pow(-2*progress+2,2)/2;

    const lat=from.lat+(to.lat-from.lat)*eased;
    const lng=from.lng+(to.lng-from.lng)*eased;

    marker.setLatLng([lat,lng]);

    if(progress<1) requestAnimationFrame(step);
  }

  requestAnimationFrame(step);
}

function updateStats() {

  $("total").textContent=trains.length;

  $("delayed").textContent=
    trains.filter(t=>delay(t)>0).length;

  $("incidents").textContent=
    trains.filter(isIncident).length;

  $("cuenca").textContent=
    trains.filter(isCuenca).length;
}

function render() {

  const visible=trains.filter(matches);

  updateStats();

  $("resultCount").textContent=
    visible.length+" trenes";

  $("trainList").innerHTML=visible.length
    ? visible.map(t=>{

      const d=delay(t);

      let statusClass="no-data";
      let statusText="Sin estado publicado";

      if(isIncident(t)) {
        statusClass="problem";
        statusText="INCIDENCIA PUBLICADA";
      } else if(d!==null && d>=5) {
        statusClass="late";
        statusText="RETRASO +"+d+" min";
      } else if(d!==null) {
        statusClass="ok";
        statusText=d>0 ? "Retraso "+d+" min" : "EN HORA";
      }

      return `
        <article class="train-card ${selectedTrain===id(t)?"selected":""}"
          data-id="${safe(id(t))}">

          <div class="train-top">
            <div class="operator">
              ${logo(t)}
              ${safe(companyName(t))}
            </div>

            <span class="train-number">${safe(id(t))}</span>
          </div>

          <div class="route">
            ${safe(origin(t))} → ${safe(destination(t))}
          </div>

          <div class="train-info">
            <span class="status ${statusClass}">
              ${statusText}
            </span>
            <span>Próxima:
              ${safe(field(t,"nextStationName","nextStation")||"No publicada")}
            </span>
          </div>

        </article>
      `;

    }).join("")
    : `<div class="empty">No hay trenes para este filtro.</div>`;

  document.querySelectorAll(".train-card").forEach(el=>{

    el.addEventListener("click",()=>{

      selectedTrain=el.dataset.id;

      const marker=markers.get(selectedTrain);

      if(marker) {
        map.setView(marker.getLatLng(),11,{animate:true});
        marker.openPopup();
      }

      render();
    });

  });

  const active=new Set();

  visible.forEach(t=>{

    const lat=Number(field(t,"latitude","lat"));
    const lng=Number(field(t,"longitude","lon","lng"));

    const trainId=id(t);

    if(!Number.isFinite(lat) ||
       !Number.isFinite(lng) ||
       lat < 27 || lat > 45 ||
       lng < -19 || lng > 5 ||
       !trainId) return;

    active.add(trainId);

    const position=L.latLng(lat,lng);

    if(markers.has(trainId)) {

      const marker=markers.get(trainId);

      const previous=marker.getLatLng();

      if(previous.distanceTo(position)>2) {
        animateMarker(marker,previous,position);
      }

      marker.setPopupContent(popup(t));

    } else {

      const marker=L.marker(position,{icon:icon()})
        .addTo(map);

      marker.bindPopup(popup(t));

      markers.set(trainId,marker);
    }

  });

  for(const [key,marker] of markers) {

    if(!active.has(key)) {
      map.removeLayer(marker);
      markers.delete(key);
    }

  }

}

function receiveData(data) {

  const list=Array.isArray(data)
    ? data
    : data.trains || data.vehicles || data.data || [];

  if(!Array.isArray(list)) return;

  trains=list;

  $("connection").textContent="CONECTADO";

  const timestamp=data.updatedAt
    ? new Date(data.updatedAt)
    : new Date();

  $("lastUpdate").textContent=
    "Última actualización: "+
    timestamp.toLocaleTimeString("es-ES");

  render();
}

async function loadSnapshot() {

  try {

    const response=await fetch(
      API+"/fleet?type=all",
      {cache:"no-store"}
    );

    if(!response.ok) throw new Error("Error HTTP "+response.status);

    const data=await response.json();

    receiveData(data);

  } catch(error) {

    $("connection").textContent="SIN CONEXIÓN";

    $("lastUpdate").textContent=error.message;

  }

}

function connectStream() {

  if(stream) stream.close();

  $("connection").textContent="CONECTANDO";

  stream=new EventSource(
    API+"/fleet/stream?type=all"
  );

  stream.onopen=()=>{

    $("connection").textContent="CONECTADO EN DIRECTO";

  };

  stream.onmessage=event=>{

    try {
      receiveData(JSON.parse(event.data));
    } catch(error) {
      console.error("Error leyendo el flujo:",error);
    }

  };

  stream.addEventListener("fleet",event=>{

    try {
      receiveData(JSON.parse(event.data));
    } catch(error) {
      console.error("Error en datos de flota:",error);
    }

  });

  stream.onerror=()=>{

    $("connection").textContent="RECONECTANDO";

    // EventSource intenta reconectar automáticamente.
    // Usamos una consulta de respaldo si no llega el flujo.
    if(!trains.length) loadSnapshot();

  };

}

$("search").addEventListener("input",render);

$("filter").addEventListener("change",render);

$("refresh").addEventListener("click",loadSnapshot);

document.querySelectorAll(".company").forEach(button=>{

  button.addEventListener("click",()=>{

    document.querySelectorAll(".company")
      .forEach(b=>b.classList.remove("active"));

    button.classList.add("active");

    selectedCompany=button.dataset.company;

    render();

  });

});

// Primera carga y conexión en directo.
loadSnapshot();
connectStream();

// Respaldo de datos por si el flujo se interrumpe.
setInterval(()=>{
  if(!stream || stream.readyState===EventSource.CLOSED) {
    loadSnapshot();
  }
},60000);

</script>

</body>
</html>
