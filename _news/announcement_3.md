---
layout: post
title: "2 Map Test 🌊 Expedition Alert: Heading to the Labrador Sea!"
date: 2025-05-25 16:13:00-0400
inline: false
related_posts: false
map: true
---

<div id="map" style="height: 600px; margin: 20px 0;"></div>
<div style="position: absolute; top: 100px; right: 20px; z-index: 1000;">
  <details>
    <summary style="cursor: pointer; padding: 0.5rem; background: white; border-radius: 5px; font-weight: bold;">Filter Stations</summary>
    <div id="filter-controls" style="background: white; padding: 1rem; border-radius: 5px;"></div>
  </details>
</div>
<div id="buttons" style="margin-bottom: 1rem;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://unpkg.com/leaflet.awesome-markers@2.0.4/dist/leaflet.awesome-markers.min.js"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet.awesome-markers@2.0.4/dist/leaflet.awesome-markers.css">

<script>
  const map = L.map('map').setView([54.5, -54.5], 4);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '',
    maxZoom: 19
  }).addTo(map);

  const iconMap = {
    "Sampling Station": L.AwesomeMarkers.icon({ icon: 'flask', prefix: 'fa', markerColor: 'green' }),
    "Transect": L.AwesomeMarkers.icon({ icon: 'line-chart', prefix: 'fa', markerColor: 'orange' }),
    "Waypoint": L.AwesomeMarkers.icon({ icon: 'anchor', prefix: 'fa', markerColor: 'blue' }),
    "Departure Port": L.AwesomeMarkers.icon({ icon: 'ship', prefix: 'fa', markerColor: 'darkred' }),
    "Arrival Port": L.AwesomeMarkers.icon({ icon: 'flag-checkered', prefix: 'fa', markerColor: 'purple' }),
    "Unknown Type": L.AwesomeMarkers.icon({ icon: 'question', prefix: 'fa', markerColor: 'gray' })
  };

  function parseCoord(lat, lon) {
    function dmsToDecimal(dms, direction) {
      const [deg, min, sec] = dms.map(Number);
      let dec = deg + min / 60 + sec / 3600;
      if (["S", "W"].includes(direction)) dec *= -1;
      return dec;
    }
    const latParts = lat.match(/(\d+)[^\d]+(\d+)[^\d]+(\d+)[^\d]+([NS])/);
    const lonParts = lon.match(/(\d+)[^\d]+(\d+)[^\d]+(\d+)[^\d]+([EW])/);
    return [
      dmsToDecimal([lonParts[1], lonParts[2], lonParts[3]], lonParts[4]),
      dmsToDecimal([latParts[1], latParts[2], latParts[3]], latParts[4])
    ];
  }

  const rawData = [
  ["Departure Port", "Woods Hole", "41° 31' 36'' N", "070° 40' 43'' W"],
  ["Sampling Station", "Sta.1", "42° 24' 14'' N", "061° 05' 08'' W"],
  ["Waypoint", "Waypoint - 3", "46° 41' 34'' N", "052° 10' 15'' W"],
  ["Transect", "Lab Shelf survey Fe", "55° 05' 29'' N", "055° 01' 08'' W"],
  ["Sampling Station", "Sta.2", "54° 14' 31'' N", "055° 52' 06'' W"],
  ["Sampling Station", "Sta.3", "54° 39' 09'' N", "055° 29' 50'' W"],
  ["Sampling Station", "Sta.4", "55° 05' 29'' N", "055° 01' 08'' W"],
  ["Transect", "Drift survey Nd", "54° 44' 28'' N", "052° 58' 33'' W"],
  ["Sampling Station", "Sta.5", "55° 07' 42'' N", "051° 47' 07'' W"],
  ["Sampling Station", "Sta.6", "54° 51' 53'' N", "052° 23' 09'' W"],
  ["Sampling Station", "Sta.7", "54° 44' 53'' N", "052° 58' 06'' W"],
  ["Sampling Station", "Sta.8", "55° 00' 17'' N", "051° 57' 13'' W"],
  ["Sampling Station", "Sta.9", "55° 46' 44'' N", "051° 05' 14'' W"],
  ["Sampling Station", "Sta.10", "57° 15' 13'' N", "050° 10' 14'' W"],
  ["Waypoint", "Waypoint - 2", "58° 58' 56'' N", "048° 46' 30'' W"],
  ["Transect", "S Greenland survey Fe", "60° 12' 07'' N", "047° 44' 44'' W"],
  ["Sampling Station", "Sta.11", "60° 43' 30'' N", "047° 03' 53'' W"],
  ["Sampling Station", "Sta.12", "60° 25' 18'' N", "047° 26' 54'' W"],
  ["Sampling Station", "Sta.13", "60° 12' 18'' N", "047° 44' 53'' W"],
  ["Waypoint", "Waypoint - 4", "60° 54' 30'' N", "049° 33' 12'' W"],
  ["Transect", "N Greenland survey", "61° 36' 11'' N", "051° 02' 22'' W"],
  ["Sampling Station", "Sta.14", "61° 42' 01'' N", "050° 14' 24'' W"],
  ["Sampling Station", "Sta.15", "61° 37' 12'' N", "050° 34' 12'' W"],
  ["Sampling Station", "Sta.16", "61° 37' 12'' N", "050° 48' 36'' W"],
  ["Sampling Station", "Sta.17", "61° 36' 00'' N", "051° 02' 24'' W"],
  ["Sampling Station", "Sta.18", "61° 37' 12'' N", "050° 54' 36'' W"],
  ["Sampling Station", "Sta.19", "61° 37' 48'' N", "050° 40' 48'' W"],
  ["Waypoint", "Waypoint - 5", "60° 59' 23'' N", "049° 20' 36'' W"],
  ["Arrival Port", "Reykjavik", "64° 08' 48'' N", "021° 56' 24'' W"]
];

  const features = rawData.map(([type, name, lat, lon]) => {
    const coords = parseCoord(lat, lon);
    return {
      type: "Feature",
      geometry: {
        type: "Point",
        coordinates: coords
      },
      properties: {
        Name: name || "Unnamed",
        Station_Type: type || "Unknown Type",
        Coordinates: coords
      }
    };
  });

  let geoLayer;

  function renderLayer(typesToShow) {
    if (geoLayer) geoLayer.remove();

    geoLayer = L.geoJSON({ type: "FeatureCollection", features }, {
      filter: f => typesToShow.includes(f.properties.Station_Type),
      pointToLayer: function (feature, latlng) {
        const type = feature.properties.Station_Type;
        return L.marker(latlng, { icon: iconMap[type] || iconMap["Unknown Type"] });
      },
      onEachFeature: function (feature, layer) {
        const name = feature.properties.Name;
        const type = feature.properties.Station_Type;
        const coords = feature.properties.Coordinates;
        layer.bindPopup(`<strong>${name}</strong><br>${type}`);

        const btn = document.createElement('button');
        btn.textContent = name;
        btn.style.margin = '0.25rem';
        btn.onclick = () => {
          map.setView([coords[1], coords[0]], 7);
          layer.openPopup();
        };
        document.getElementById('buttons').appendChild(btn);
      }
    }).addTo(map);

    const fullTrack = features
      .filter(f => typesToShow.includes(f.properties.Station_Type))
      .map(f => [f.properties.Coordinates[1], f.properties.Coordinates[0]]);

    if (fullTrack.length > 1) {
      L.polyline(fullTrack, { color: 'red', weight: 2, opacity: 0.8 }).addTo(map);
    }
  }

  const stationTypes = Object.keys(iconMap);
  const filterControls = document.getElementById('filter-controls');
  const selectedTypes = new Set(stationTypes);

  stationTypes.forEach(type => {
    const label = document.createElement('label');
    const checkbox = document.createElement('input');
    checkbox.type = 'checkbox';
    checkbox.checked = true;
    checkbox.onchange = () => {
      if (checkbox.checked) selectedTypes.add(type);
      else selectedTypes.delete(type);
      document.getElementById('buttons').innerHTML = '';
      renderLayer([...selectedTypes]);
    };
    label.appendChild(checkbox);
    label.append(` ${type} `);
    label.style.display = 'block';
    label.style.margin = '0.25rem 0';
    filterControls.appendChild(label);
  });

  renderLayer([...selectedTypes]);
</script>

<style>
  .leaflet-control-attribution {
    display: none !important;
  }
  #buttons button {
    padding: 0.4rem 0.6rem;
    background: #222;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  #buttons button:hover {
    background: #444;
  }
</style>

<h2>🌊 <strong>SEALS to the Labrador Sea! 🧽</strong></h2>
<p>Starting <strong>June 3rd, 2025</strong>, I'll be embarking on a month-long research expedition aboard R/V Roger Revelle for <strong>SEALS 2025</strong> (<em>Sediment Exchange Along Labrador Sea</em>) to investigate neodymium and iron fluxes in the Labrador and Greenland coasts — a critical region for understanding deep ocean circulation and climate change.</p>
<p>We'll be collecting seawater and sediment samples across multiple stations, working with an amazing team to uncover what lies beneath the waves.</p>
