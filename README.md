<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>ZEEK MEXICANO 3408 - TELCEL</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
body{margin:0;background:#000;color:#00ff00;font-family:monospace}
#header{text-align:center;padding:10px;background:#111;border-bottom:2px solid #00ff00}
#map{height:75vh;width:100%}
#panel{text-align:center;padding:8px}
button{background:#00ff00;color:#000;border:0;padding:12px 20px;font-weight:bold;font-size:16px;margin:5px;border-radius:8px}
#datos{background:#111;padding:10px;margin-top:5px}
</style>
</head>
<body>
<div id="header">
<b>ZEEK MEXICANO 3408</b><br>
Hecho en México por Blackpanter<br>
ID:3408 - TELCEL REAL
</div>
<div id="map"></div>
<div id="panel">
<button onclick="activar()">📡 ACTIVAR GPS REAL</button>
<button onclick="enviar()" style="background:yellow">📲 ENVIAR MI UBICACIÓN</button>
<div id="datos">Esperando GPS...</div>
</div>

<script>
let map = L.map('map').setView([25.5428, -103.4068], 15);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
let marker = null;
let ruta = L.polyline([], {color:'red', weight:4}).addTo(map);
let latFinal, lonFinal, accFinal;

function activar(){
  document.getElementById('datos').innerHTML="🛰️ Buscando señal Telcel GPS... sal al patio";
  if(!navigator.geolocation){ alert("Tu cel no tiene GPS"); return; }
  
  navigator.geolocation.watchPosition(function(pos){
    latFinal = pos.coords.latitude;
    lonFinal = pos.coords.longitude;
    accFinal = pos.coords.accuracy;
    
    if(!marker){
      marker = L.marker([latFinal, lonFinal]).addTo(map).bindPopup("TU ESTAS AQUI").openPopup();
    } else {
      marker.setLatLng([latFinal, lonFinal]);
    }
    ruta.addLatLng([latFinal, lonFinal]);
    map.setView([latFinal, lonFinal], 18);
    
    document.getElementById('datos').innerHTML = 
      `✅ <b>GPS REAL ACTIVO</b><br>
       Lat: ${latFinal.toFixed(6)}<br>
       Lon: ${lonFinal.toFixed(6)}<br>
       Precisión: ${accFinal.toFixed(1)} metros<br>
       <a href="https://www.google.com/maps?q=${latFinal},${lonFinal}" target="_blank" style="color:yellow">Ver en Google Maps</a>`;
       
  }, function(err){
    document.getElementById('datos').innerHTML="❌ Error: "+err.message+"<br>Activa ubicación en Ajustes > Ubicación > Alta precisión";
  }, {enableHighAccuracy:true, maximumAge:0, timeout:15000});
}

function enviar(){
  if(!latFinal){ alert("Primero ACTIVA EL GPS REAL"); return; }
  let link = `https://www.google.com/maps?q=${latFinal},${lonFinal}`;
  let msg = `ZEEK MEXICANO 3408 - Hecho en Mexico por Blackpanter - ID:3408\nMi ubicacion REAL Telcel:\n${link}\nPrecision: ${accFinal.toFixed(1)}m`;
  window.open(`https://wa.me/?text=${encodeURIComponent(msg)}`, '_blank');
}
</script>
</body>
</html>
