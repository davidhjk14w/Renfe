
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AVE Tracker España | Seguimiento ferroviario</title>
<link rel="preconnect" href="https://unpkg.com">
<link rel="preconnect" href="https://tile.openstreetmap.org">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
<style>
:root{
  --bg:#080d18;--panel:#101a2b;--panel2:#142137;--line:#24344d;
  --text:#eef4ff;--muted:#91a2bd;--blue:#4c8dff;--green:#35d49a;
  --red:#ff687d;--yellow:#ffc76b;--purple:#a78bfa;
}
*{box-sizing:border-box}
body{margin:0;background:radial-gradient(ellipse at top,#142744 0,#080d18 48%);color:var(--text);font-family:Inter,Segoe UI,Arial,sans-serif}
header{padding:22px 4%;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid #24344d;background:#0a1220dd;position:sticky;top:0;z-index:1000;backdrop-filter:blur(15px)}
.brand{display:flex;align-items:center;gap:13px}
.logo{width:43px;height:43px;border-radius:14px;background:linear-gradient(135deg,#3985ff,#765bff);display:grid;place-items:center;font-size:23px;box-shadow:0 8px 30px #397fff44}
.brand h1{font-size:19px;margin:0;letter-spacing:.2px}.brand small{display:block;color:var(--muted);font-size:11px;margin-top:4px}
.live{display:flex;align-items:center;gap:8px;color:#a8f3d3;font-size:12px;font-weight:700}
.dot{width:8px;height:8px;border-radius:50%;background:var(--green);box-shadow:0 0 12px var(--green)}
main{max-width:1600px;margin:auto;padding:25px 4% 50px}
.hero{display:flex;justify-content:space-between;align-items:flex-end;gap:20px;margin:10px 0 24px}
.hero h2{font-size:30px;margin:0 0 8px;letter-spacing:-.8px}.hero p{margin:0;color:var(--muted);font-size:13px}
.updated{color:var(--muted);font-size:12px;text-align:right}
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-bottom:20px}
.stat{background:linear-gradient(145deg,#14233a,#101a2b);border:1px solid var(--line);padding:18px;border-radius:17px;min-width:0}
.stat-label{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:1px}
.stat-value{font-size:29px;font-weight:800;margin-top:8px}.stat-sub{font-size:11px;color:var(--muted);margin-top:4px}
.layout{display:grid;grid-template-columns:minmax(0,1.55fr) minmax(340px,.8fr);gap:18px}
.panel{background:linear-gradient(145deg,#111d30,#0e1828);border:1px solid var(--line);border-radius:18px;overflow:hidden;min-width:0}
.panel-head{padding:17px 19px;border-bottom:1px solid var(--line);display:flex;justify-content:space-between;align-items:center;gap:10px}
.panel-head h3{font-size:14px;margin:0}.panel-head span{font-size:11px;color:var(--muted)}
#map{height:510px;width:100%;background:#111b2a}
.map-legend{display:flex;gap:15px;flex-wrap:wrap;padding:12px 17px;color:var(--muted);font-size:11px}
.legend-dot{display:inline-block;width:8px;height:8px;border-radius:50%;margin-right:5px}
.filters{display:grid;grid-template-columns:1.2fr 1fr 1fr 1fr;gap:10px;padding:15px;border-bottom:1px solid var(--line)}
input,select{width:100%;background:#0a1423;color:var(--text);border:1px solid #2a3d58;border-radius:10px;padding:11px 12px;font-size:12px;outline:none;min-width:0}
input:focus,select:focus{border-color:var(--blue);box-shadow:0 0 0 3px #4c8dff20}
input::placeholder{color:#71829c}
.train-list{max-height:620px;overflow:auto}
.train-card{padding:15px 17px;border-bottom:1px solid #223149;cursor:pointer;transition:.18s}
.train-card:hover,.train-card.selected{background:#1a2b45}
.train-top{display:flex;justify-content:space-between;gap:10px;align-items:center}
.train-name{font-size:13px;font-weight:800}.train-number{font-size:11px;color:var(--muted);margin-left:5px}
.operator{font-size:10px;padding:5px 8px;border-radius:7px;background:#263c5c;color:#c9dcff;font-weight:800}
.route{display:flex;align-items:center;gap:9px;margin:13px 0 10px}
.station{font-size:12px;font-weight:700;max-width:42%;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.route-line{height:2px;flex:1;background:linear-gradient(90deg,var(--blue),#63738e);position:relative}
.route-line:after{content:"";position:absolute;right:0;top:-3px;border:4px solid transparent;border-left-color:#8798b5}
.train-bottom{display:flex;justify-content:space-between;gap:10px;color:var(--muted);font-size:10px}
.delay{font-weight:800}.late{color:var(--red)}.ontime{color:var(--green)}
.progress-wrap{margin-top:12px}
.progress-label{display:flex;justify-content:space-between;color:var(--muted);font-size:10px;margin-bottom:6px}
.progress{height:6px;border-radius:20px;background:#26364d;overflow:hidden}
.progress>div{height:100%;background:linear-gradient(90deg,#438dff,#45d5b0);border-radius:20px}
.detail{padding:18px;min-height:240px}
.detail-empty{color:var(--muted);font-size:12px;line-height:1.7;padding:30px 10px;text-align:center}
.detail-title{font-size:18px;font-weight:800;margin-bottom:5px}
.detail-sub{color:var(--muted);font-size:11px;margin-bottom:18px}
.detail-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.detail-box{background:#0a1525;border:1px solid #263750;border-radius:11px;padding:12px}
.detail-box small{display:block;color:var(--muted);font-size:10px;margin-bottom:6px}
.detail-box strong{font-size:14px}
.big-progress{margin:17px 0}
.big-progress .progress{height:10px}
.notice{padding:12px 16px;color:#9eb0ca;font-size:10px;line-height:1.6;border-top:1px solid var(--line)}
.empty{padding:35px 15px;text-align:center;color:var(--muted);font-size:12px}
.footer{text-align:center;color:#70819b;font-size:10px;padding:25px 10px;line-height:1.8}
.leaflet-popup-content-wrapper,.leaflet-popup-tip{background:#122139;color:#fff}
.leaflet-popup-content{font-size:12px}
.leaflet-control-attribution{font-size:9px!important}
@media(max-width:1050px){.layout{grid-template-columns:1fr}.train-list{max-height:500px}}
@media(max-width:700px){header{padding:15px 4%}.brand h1{font-size:15px}.hero{display:block}.hero h2{font-size:24px}.updated{text-align:left;margin-top:12px}.stats{grid-template-columns:1fr 1fr}.stat-value{font-size:24px}.filters{grid-template-columns:1fr 1fr}#map{height:390px}}
@media(max-width:420px){.filters{grid-template-columns:1fr}.stats{gap:8px}.stat{padding:13px}}
</style>
</head>
<body>
<header>
  <div class="brand">
    <div class="logo">🚄</div>
    <div><h1>AVE TRACKER ESPAÑA</h1><small>Seguimiento ferroviario en directo</small></div>
  </div>
  <div class="live"><span class="dot"></span> DATOS EN DIRECTO</div>
</header>

<main>
  <section class="hero">
    <div>
      <h2>Red ferroviaria española</h2>
      <p>Consulta circulaciones, destinos, retrasos y recorridos en tiempo real.</p>
    </div>
    <div class="updated">Última actualización<br><strong id="updated">Conectando...</strong></div>
  </section>

  <section class="stats">
    <div class="stat"><div class="stat-label">Trenes detectados</div><div class="stat-value" id="total">—</div><div class="stat-sub">Circulaciones disponibles</div></div>
    <div class="stat"><div class="stat-label">Con retraso</div><div class="stat-value" id="delayed" style="color:var(--red)">—</div><div class="stat-sub">Más de 5 minutos</div></div>
    <div class="stat"><div class="stat-label">Sin retraso</div><div class="stat-value" id="ontime" style="color:var(--green)">—</div><div class="stat-sub">5 minutos o menos</div></div>
    <div class="stat"><div class="stat-label">Con posición GPS</div><div class="stat-value" id="gps">—</div><div class="stat-sub">Visibles en el mapa</div></div>
  </section>

  <section class="layout">
    <div class="panel">
      <div class="panel-head"><h3>Mapa de circulaciones</h3><span id="mapCount">Cargando posiciones...</span></div>
      <div id="map"></div>
      <div class="map-legend">
        <span><i class="legend-dot" style="background:#35d49a"></i>Sin retraso</span>
        <span><i class="legend-dot" style="background:#ff687d"></i>Con retraso</span>
        <span><i class="legend-dot" style="background:#a78bfa"></i>Sin posición GPS</span>
      </div>
      <div class="notice">El mapa muestra únicamente trenes con coordenadas GPS publicadas. Los servicios sin posición no se colocan en ubicaciones inventadas.</div>
    </div>

    <div class="panel">
      <div class="panel-head"><h3>Detalle del tren</h3><span id="detailStatus">Selecciona un tren</span></div>
      <div class="detail" id="detail">
        <div class="detail-empty">🚄<br><br>Selecciona una circulación de la lista para consultar su información, retraso y porcentaje de recorrido completado.</div>
      </div>
    </div>
  </section>

  <section class="panel" style="margin-top:18px">
    <div class="panel-head"><h3>Circulaciones disponibles</h3><span id="listCount">—</span></div>
    <div class="filters">
      <input id="search" placeholder="Buscar tren, origen o destino...">
      <select id="destination"><option value="">Todos los destinos</option></select>
      <select id="arrival"><option value="">Todas las llegadas</option></select>
      <select id="delayFilter">
        <option value="">Todos los estados</option>
        <option value="late">Con retraso (+5 min)</option>
        <option value="ok">Sin retraso (≤5 min)</option>
      </select>
    </div>
    <div class="train-list" id="trainList"><div class="empty">Conectando con la fuente de datos...</div></div>
    <div class="notice">Los filtros se actualizan con los destinos y servicios recibidos de las fuentes disponibles. Iryo y OUIGO pueden aparecer en los tablones de estación sin seguimiento GPS.</div>
  </section>

  <div class="footer">
    AVE Tracker España · Proyecto independiente y no oficial.<br>
    Datos ferroviarios: Renfe y ADIF, a través de RadarDeTrenes · Cartografía: OpenStreetMap.<br>
    La información es orientativa y puede contener desfases. Consulta siempre al operador para información oficial.
  </div>
</main>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
const API="https://radardetrenes.com/api/v1";
const $=id=>document.getElementById(id);
let trains=[],markers={},selectedId=null,map,markerLayer;
let stationCache=[];

function initMap(){
  map=L.map("map",{zoomControl:true}).setView([40.2,-3.7],6);
  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",{
    maxZoom:19,attribution:'&copy; OpenStreetMap'
  }).addTo(map);
  markerLayer=L.layerGroup().addTo(map);
  setTimeout(()=>map.invalidateSize(),300);
}
initMap();

function val(obj,keys,fallback=""){
  for(const k of keys){
    const v=obj?.[k];
    if(v!==undefined&&v!==null&&v!=="")return v;
  }
  return fallback;
}
function number(v,fallback=0){
  const n=Number(v);
  return Number.isFinite(n)?n:fallback;
}
function normalise(t,source="fleet"){
  const delay=number(val(t,["delayMinutes","delay","delay_minutes"],0));
  const lat=number(val(t,["latitude","lat"],NaN),NaN);
  const lng=number(val(t,["longitude","lon","lng"],NaN),NaN);
  const operator=String(val(t,["operatorName","operator","company","productName","commercialName"],"")).toUpperCase();
  const name=String(val(t,["productName","commercialName","category","type"],operator||"Tren"));
  return {
    id:String(val(t,["id","trainId","trainCode","number","trainNumber"],"")),
    number:String(val(t,["trainCode","trainNumber","number","id"],"")),
    name,operator:detectOperator(operator+" "+name),
    origin:String(val(t,["originName","origin","originStation","departureStation"],"Origen no disponible")),
    destination:String(val(t,["destinationName","destination","destinationStation","arrivalStation"],"Destino no disponible")),
    next:String(val(t,["nextStationName","nextStation","next_stop"],"")),
    delay,lat,lng,
    gps:Number.isFinite(lat)&&Number.isFinite(lng)&&Math.abs(lat)>0&&Math.abs(lng)>0,
    source,
    departure:val(t,["departureTime","scheduledDeparture","originDeparture"],""),
    arrival:val(t,["arrivalTime","scheduledArrival","destinationArrival"],""),
    raw:t,progress:null
  };
}
function detectOperator(s){
  s=String(s).toUpperCase();
  if(s.includes("IRYO"))return "IRYO";
  if(s.includes("OUIGO"))return "OUIGO";
  if(s.includes("AVLO"))return "AVLO";
  if(s.includes("AVE"))return "AVE";
  if(s.includes("ALVIA"))return "ALVIA";
  if(s.includes("AVANT"))return "AVANT";
  if(s.includes("EUROMED"))return "EUROMED";
  if(s.includes("INTERCITY"))return "INTERCITY";
  if(s.includes("CERCAN"))return "CERCANÍAS";
  if(s.includes("MEDIA"))return "MEDIA DISTANCIA";
  return s||"RENFE";
}
function isLate(t){return t.delay>5}
function delayText(t){
  return t.delay>0?`+${t.delay} min`:t.delay<0?`${t.delay} min`:"0 min";
}
function escapeHtml(s){
  return String(s??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));
}
async function getJSON(url){
  const r=await fetch(url,{cache:"no-store"});
  if(!r.ok)throw new Error("HTTP "+r.status);
  return r.json();
}
function unwrapList(data){
  if(Array.isArray(data))return data;
  for(const k of ["trains","departures","arrivals","services","results","data","stations"]){
    if(Array.isArray(data?.[k]))return data[k];
  }
  return [];
}
async function loadFleet(){
  try{
    const data=await getJSON(API+"/fleet?type=all");
    const fresh=unwrapList(data).map(t=>normalise(t,"GPS Renfe")).filter(t=>t.id);
    trains=fresh;
    $("updated").textContent=new Date().toLocaleTimeString("es-ES");
    await loadOperatorBoards();
    updateEverything();
  }catch(e){
    $("updated").textContent="Error de conexión";
    $("trainList").innerHTML='<div class="empty">No se ha podido conectar con la fuente de trenes. Comprueba tu conexión e inténtalo de nuevo.</div>';
    console.error(e);
  }
}

/* Estaciones principales: se localizan en el catálogo público para no inventar códigos. */
const majorStationNames=[
  "Madrid Puerta de Atocha","Madrid Chamartín","Barcelona Sants","Valencia Joaquín Sorolla",
  "Sevilla Santa Justa","Málaga María Zambrano","Zaragoza Delicias","Córdoba",
  "Alicante Terminal","Valladolid Campo Grande","Bilbao Abando","Granada",
  "Girona","Tarragona","Cuenca Fernando Zóbel","Albacete Los Llanos"
];
function stationName(s){return String(val(s,["name","stationName","nombre"],""))}
function stationCode(s){return String(val(s,["code","stationCode","id","codigo"],""))}
async function loadOperatorBoards(){
  try{
    if(!stationCache.length){
      const st=await getJSON(API+"/stations");
      stationCache=unwrapList(st);
    }
    const selected=majorStationNames.map(target=>{
      const norm=target.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g,"");
      return stationCache.find(s=>stationName(s).toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g,"").includes(norm));
    }).filter(Boolean);
    const requests=selected.slice(0,16).map(async s=>{
      const code=stationCode(s);
      if(!code)return [];
      try{
        const board=await getJSON(API+"/stations/"+encodeURIComponent(code)+"/board-renfe");
        return unwrapList(board).map(x=>normalise(x,"Tablón ADIF")).filter(t=>
          ["IRYO","OUIGO"].includes(t.operator)
        );
      }catch(e){return []}
    });
    const boards=(await Promise.all(requests)).flat();
    const known=new Set(trains.map(t=>t.number+"|"+t.origin+"|"+t.destination));
    for(const t of boards){
      const key=t.number+"|"+t.origin+"|"+t.destination;
      if(!known.has(key)&&t.number){
        t.id="board-"+key;
        t.gps=false;
        trains.push(t);
        known.add(key);
      }
    }
  }catch(e){console.warn("No se pudieron consultar los tablones de estaciones",e)}
}

function updateStats(){
  const gps=trains.filter(t=>t.gps).length;
  $("total").textContent=trains.length;
  $("delayed").textContent=trains.filter(isLate).length;
  $("ontime").textContent=trains.filter(t=>!isLate(t)).length;
  $("gps").textContent=gps;
  $("mapCount").textContent=gps+" posiciones GPS";
}
function updateFilters(){
  const destinations=[...new Set(trains.map(t=>t.destination).filter(x=>x&&x!=="Destino no disponible"))].sort((a,b)=>a.localeCompare(b,"es"));
  const arrivals=[...new Set(trains.map(t=>t.destination).filter(x=>x&&x!=="Destino no disponible"))].sort((a,b)=>a.localeCompare(b,"es"));
  const oldD=$("destination").value,oldA=$("arrival").value;
  $("destination").innerHTML='<option value="">Todos los destinos</option>'+destinations.map(x=>`<option>${escapeHtml(x)}</option>`).join("");
  $("arrival").innerHTML='<option value="">Todas las llegadas</option>'+arrivals.map(x=>`<option>${escapeHtml(x)}</option>`).join("");
  $("destination").value=destinations.includes(oldD)?oldD:"";
  $("arrival").value=arrivals.includes(oldA)?oldA:"";
}
function filteredTrains(){
  const q=$("search").value.toLowerCase().trim();
  const dest=$("destination").value;
  const arrival=$("arrival").value;
  const delay=$("delayFilter").value;
  return trains.filter(t=>{
    const hay=[t.number,t.name,t.operator,t.origin,t.destination].join(" ").toLowerCase();
    if(q&&!hay.includes(q))return false;
    if(dest&&t.destination!==dest)return false;
    if(arrival&&t.destination!==arrival)return false;
    if(delay==="late"&&!isLate(t))return false;
    if(delay==="ok"&&isLate(t))return false;
    return true;
  });
}
function renderList(){
  const list=filteredTrains();
  $("listCount").textContent=list.length+" circulaciones";
  if(!list.length){
    $("trainList").innerHTML='<div class="empty">No hay trenes que coincidan con los filtros seleccionados.</div>';
    return;
  }
  $("trainList").innerHTML=list.map(t=>{
    const selected=t.id===selectedId?"selected":"";
    const progress=t.progress;
    return `<article class="train-card ${selected}" data-id="${escapeHtml(t.id)}">
      <div class="train-top">
        <div><span class="train-name">${escapeHtml(t.name)}</span><span class="train-number">${escapeHtml(t.number)}</span></div>
        <span class="operator">${escapeHtml(t.operator)}</span>
      </div>
      <div class="route"><span class="station">${escapeHtml(t.origin)}</span><span class="route-line"></span><span class="station" style="text-align:right">${escapeHtml(t.destination)}</span></div>
      <div class="train-bottom">
        <span>${t.gps?"● Posición GPS":"○ Sin posición GPS"}</span>
        <span class="delay ${isLate(t)?"late":"ontime"}">${isLate(t)?"Con retraso":"Sin retraso"} · ${escapeHtml(delayText(t))}</span>
      </div>
      ${progress!==null?`<div class="progress-wrap"><div class="progress-label"><span>Recorrido completado</span><strong>${progress}%</strong></div><div class="progress"><div style="width:${progress}%"></div></div></div>`:""}
    </article>`;
  }).join("");
  document.querySelectorAll(".train-card").forEach(el=>{
    el.addEventListener("click",()=>selectTrain(el.dataset.id));
  });
}
function markerIcon(t){
  const color=isLate(t)?"#ff687d":"#35d49a";
  return L.divIcon({className:"",html:`<div style="width:16px;height:16px;border:3px solid white;border-radius:50%;background:${color};box-shadow:0 0 12px ${color}"></div>`,iconSize:[16,16],iconAnchor:[8,8]});
}
function renderMap(){
  markerLayer.clearLayers();markers={};
  const visible=filteredTrains().filter(t=>t.gps);
  visible.forEach(t=>{
    const marker=L.marker([t.lat,t.lng],{icon:markerIcon(t)}).addTo(markerLayer);
    marker.bindPopup(`<b>${escapeHtml(t.operator)} ${escapeHtml(t.number)}</b><br>${escapeHtml(t.origin)} → ${escapeHtml(t.destination)}<br>${isLate(t)?"Con retraso":"Sin retraso"} · ${escapeHtml(delayText(t))}`);
    marker.on("click",()=>selectTrain(t.id));
    markers[t.id]=marker;
  });
  $("mapCount").textContent=visible.length+" posiciones GPS visibles";
}
async function selectTrain(id){
  selectedId=id;
  const t=trains.find(x=>x.id===id);
  if(!t)return;
  $("detailStatus").textContent=t.number||"Tren seleccionado";
  renderList();
  if(t.gps&&markers[id])map.panTo([t.lat,t.lng]);
  $("detail").innerHTML=`
    <div class="detail-title">${escapeHtml(t.operator)} ${escapeHtml(t.number)}</div>
    <div class="detail-sub">${escapeHtml(t.name)} · ${t.gps?"Seguimiento GPS":"Información de tablón de estación"}</div>
    <div class="detail-grid">
      <div class="detail-box"><small>ORIGEN</small><strong>${escapeHtml(t.origin)}</strong></div>
      <div class="detail-box"><small>DESTINO</small><strong>${escapeHtml(t.destination)}</strong></div>
      <div class="detail-box"><small>ESTADO</small><strong class="${isLate(t)?"late":"ontime"}">${isLate(t)?"Con retraso":"Sin retraso"}</strong></div>
      <div class="detail-box"><small>RETRASO</small><strong class="${isLate(t)?"late":"ontime"}">${escapeHtml(delayText(t))}</strong></div>
      <div class="detail-box"><small>PRÓXIMA PARADA</small><strong>${escapeHtml(t.next||"No disponible")}</strong></div>
      <div class="detail-box"><small>POSICIÓN</small><strong>${t.gps?"GPS disponible":"No publicada"}</strong></div>
    </div>
    ${t.progress!==null?`<div class="big-progress"><div class="progress-label"><span>Trayecto completado</span><strong>${t.progress}%</strong></div><div class="progress"><div style="width:${t.progress}%"></div></div><div class="progress-label" style="margin-top:7px"><span>Restante</span><strong>${100-t.progress}%</strong></div></div>`:
    `<div class="notice" style="padding:12px 0;border:0">El porcentaje del trayecto se mostrará cuando la fuente publique una ruta geográfica utilizable para este tren.</div>`}
  `;
  if(t.gps)loadProgress(t);
}

/* Calcula progreso con geometría de ruta cuando el endpoint la proporciona. */
function getCoordinates(obj){
  if(!obj)return [];
  if(obj.type==="Feature")return getCoordinates(obj.geometry);
  if(obj.type==="LineString"&&Array.isArray(obj.coordinates))return obj.coordinates;
  if(obj.type==="MultiLineString"&&Array.isArray(obj.coordinates))return obj.coordinates.flat();
  if(obj.geometry)return getCoordinates(obj.geometry);
  if(obj.routeGeometry)return getCoordinates(obj.routeGeometry);
  if(obj.coordinates&&Array.isArray(obj.coordinates))return obj.coordinates;
  if(Array.isArray(obj))return obj.flatMap(x=>{
    if(Array.isArray(x)&&typeof x[0]==="number")return [x];
    return getCoordinates(x);
  });
  return [];
}
function haversine(a,b){
  const R=6371,toRad=x=>x*Math.PI/180;
  const dLat=toRad(b[1]-a[1]),dLon=toRad(b[0]-a[0]);
  const x=Math.sin(dLat/2)**2+Math.cos(toRad(a[1]))*Math.cos(toRad(b[1]))*Math.sin(dLon/2)**2;
  return 2*R*Math.asin(Math.sqrt(x));
}
function routeProgress(coords,lat,lng){
  if(coords.length<2)return null;
  let lengths=[0],total=0;
  for(let i=1;i<coords.length;i++){total+=haversine(coords[i-1],coords[i]);lengths.push(total)}
  if(total<=0)return null;
  let best={dist:Infinity,progress:null};
  for(let i=0;i<coords.length;i++){
    const d=haversine(coords[i],[lng,lat]);
    if(d<best.dist)best={dist:d,progress:Math.round(lengths[i]/total*100)};
  }
  return best.dist<80?Math.max(0,Math.min(100,best.progress)):null;
}
async function loadProgress(t){
  if(!t.id.startsWith("ld-"))return;
  try{
    const data=await getJSON(API+"/trains/"+encodeURIComponent(t.id)+"/route");
    const coords=getCoordinates(data);
    if(coords.length>1){
      const p=routeProgress(coords,t.lat,t.lng);
      if(p!==null){t.progress=p;if(selectedId===t.id)selectTrain(t.id);renderList()}
    }
  }catch(e){console.debug("Ruta no disponible para",t.id)}
}
function updateEverything(){
  updateStats();updateFilters();renderList();renderMap();
  if(selectedId&&!trains.some(t=>t.id===selectedId))selectedId=null;
  if(selectedId){
    const selected=trains.find(t=>t.id===selectedId);
    if(selected)selectTrain(selectedId);
  }
}
["search","destination","arrival","delayFilter"].forEach(id=>{
  $(id).addEventListener(id==="search"?"input":"change",()=>{renderList();renderMap()});
});
window.addEventListener("resize",()=>map.invalidateSize());

loadFleet();
setInterval(loadFleet,30000);
</script>
</body>
</html>

</script>

</body>
</html>
