<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Geovisor Catastral de Quito + Supabase</title>

  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; font-family: Arial, sans-serif; }
    header, footer {
      background-color: #004b87;
      color: white;
      text-align: center;
      padding: 1em;
    }
    #map {
      height: 60vh;
      width: 100%;
    }
    #panel {
      max-width: 800px;
      margin: 2em auto;
    }
    input, button { width: 100%; padding: .5em; margin: .5em 0; }
    ul { list-style: none; padding: 0; }
    li { margin: .3em 0; }
    a { color: #0366d6; cursor: pointer; }
    a:hover { text-decoration: underline; }
    #status { color: firebrick; margin-top: .5em; }
    table { border-collapse: collapse; width:100%; margin-top:1em; }
    th, td { border:1px solid #ccc; padding:.4em .6em; }
    th { background:#f5f5f5; }
  </style>
</head>
<body>

<header>
  <h1>Geovisor Catastral de Quito</h1>
  <p>Visualizador con conexión a Supabase y Leaflet</p>
</header>

<!-- Mapa -->
<div id="map"></div>

<!-- Panel de tablas Supabase -->
<div id="panel">
  <h2>Explorador de Tablas Supabase</h2>

  <label>Project URL</label>
  <input id="url" value="https://wtuhbbmapoindpuaywxo.supabase.co" readonly />

  <label>API Key (anon)</label>
  <input id="key" type="password" value="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Ind0dWhiYm1hcG9pbmRwdWF5d3hvIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTI3Mjc5NTEsImV4cCI6MjA2ODMwMzk1MX0.1vH0zSrQ5EHHz1p_SS3gtzyDGTBpzHBOjq5ART7Uc8k" readonly />

  <button onclick="listarTablas()">Listar tablas</button>
  <div id="status"></div>
  <div id="tables"></div>
  <div id="preview"></div>
</div>

<footer>
  &copy; 2025 Municipio del Distrito Metropolitano de Quito
</footer>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  // Inicializar mapa
  const map = L.map('map').setView([-0.22985, -78.52495], 13);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap'
  }).addTo(map);

  // Marcador base
  L.marker([-0.22985, -78.52495])
    .addTo(map)
    .bindPopup("Centro de Quito")
    .openPopup();

  // Cargar puntos desde una tabla específica
  async function cargarPuntosDesdeTabla(tabla = "capas_mapa") {
    const url = document.getElementById('url').value.trim();
    const key = document.getElementById('key').value.trim();

    const res = await fetch(`${url}/rest/v1/${tabla}?select=nombre,latitud,longitud`, {
      headers: {
        apikey: key,
        Authorization: `Bearer ${key}`
      }
    });

    const data = await res.json();

    data.forEach(punto => {
      if (punto.latitud && punto.longitud) {
        L.marker([punto.latitud, punto.longitud])
          .addTo(map)
          .bindPopup(`<strong>${punto.nombre}</strong>`);
      }
    });
  }

  // Puedes descomentar esto si sabes que tienes una tabla con puntos
  // cargarPuntosDesdeTabla("capas_mapa");
</script>

<!-- Script de Supabase Explorer -->
<script>
  async function listarTablas() {
    const url = document.getElementById('url').value.trim();
    const key = document.getElementById('key').value.trim();
    const status = document.getElementById('status');
    const tablesDiv = document.getElementById('tables');
    const preview = document.getElementById('preview');
    tablesDiv.innerHTML = preview.innerHTML = '';
    status.textContent = '';

    if (!url || !key) {
      status.textContent = '⚠️ Completa URL y API key';
      return;
    }
    status.textContent = '🔎 Introspeccionando esquema via GraphQL…';

    const gql = `{
      __schema {
        queryType { fields { name } }
      }
    }`;

    let fields;
    try {
      const res = await fetch(`${url}/graphql/v1`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'apikey': key,
          'Authorization': `Bearer ${key}`
        },
        body: JSON.stringify({ query: gql })
      });
      if (!res.ok) throw new Error(res.statusText);
      const { data } = await res.json();
      fields = data.__schema.queryType.fields.map(f => f.name);
    } catch (err) {
      status.textContent = '❌ GraphQL error: ' + err.message;
      return;
    }

    const candidates = fields.filter(n => n.endsWith('Collection')).map(n => n.slice(0, -'Collection'.length));

    if (candidates.length === 0) {
      status.textContent = '🚫 No hay tablas "Collection" detectadas.';
      return;
    }

    status.textContent = `⏳ Validando ${candidates.length} candidato(s)…`;

    const valid = [];
    await Promise.all(candidates.map(async tbl => {
      try {
        const r = await fetch(`${url}/rest/v1/${tbl}?select=*&limit=1`, {
          headers: { 'apikey': key, 'Authorization': `Bearer ${key}` }
        });
        if (r.ok) valid.push(tbl);
      } catch {}
    }));

    if (valid.length === 0) {
      status.textContent = '❌ Ninguna tabla es accesible con esta anon key.';
      return;
    }

    status.textContent = `✅ ${valid.length} tabla(s) accesible(s):`;
    const ul = document.createElement('ul');
    valid.forEach(tbl => {
      const li = document.createElement('li');
      const a = document.createElement('a');
      a.textContent = tbl;
      a.onclick = () => previewTabla(url, key, tbl);
      li.appendChild(a);
      ul.appendChild(li);
    });
    tablesDiv.innerHTML = '<h2>Tablas disponibles:</h2>';
    tablesDiv.appendChild(ul);
  }

  async function previewTabla(url, key, tabla) {
    const status = document.getElementById('status');
    const preview = document.getElementById('preview');
    preview.innerHTML = '';
    status.textContent = `⏳ Cargando primeras filas de "${tabla}"…`;

    try {
      const r = await fetch(`${url}/rest/v1/${tabla}?select=*&limit=5`, {
        headers: { 'apikey': key, 'Authorization': `Bearer ${key}` }
      });
      if (!r.ok) throw new Error(r.statusText);
      const data = await r.json();

      let html = `<h3>Primeras filas de <strong>${tabla}</strong></h3>`;
      if (!data.length) {
        html += '<p>— tabla vacía —</p>';
      } else {
        html += '<table><tr>';
        Object.keys(data[0]).forEach(c => html += `<th>${c}</th>`);
        html += '</tr>';
        data.forEach(row => {
          html += '<tr>';
          Object.values(row).forEach(v => html += `<td>${v}</td>`);
          html += '</tr>';
        });
        html += '</table>';
      }
      preview.innerHTML = html;
      status.textContent = `✅ Datos de "${tabla}" cargados`;
    } catch (err) {
      status.textContent = '❌ Error: ' + err.message;
    }
  }
</script>

</body>
</html>
