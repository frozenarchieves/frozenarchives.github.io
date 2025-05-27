---
layout: post
title: "3 Map Test 🌊 Expedition Alert: Heading to the Labrador Sea!"
date: 2025-05-25 16:12:00-0400
inline: false
related_posts: false
map: true
---
<div id="map" style="height: 600px; margin: 20px 0;"></div>
<div id="filter-controls" style="margin: 1rem 0;"></div>
<div id="buttons" style="margin-bottom: 1rem;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  const map = L.map('map').setView([54.5, -54.5], 4);

  L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
    attribution: '',
    subdomains: 'abcd',
    maxZoom: 19
  }).addTo(map);

  const stationColors = {
    "Waypoint": "blue",
    "Sampling Station": "green",
    "Transect": "orange",
    "Unknown Type": "gray"
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
    ["Waypoint", "Pick up Yang", "44° 32' 52'' N", "063° 29' 37'' W"],
    ["Sampling Station", "Sta.1", "42° 24' 14'' N", "061° 05' 08'' W"],
    ["Waypoint", "Waypoint - 3", "46° 41' 34'' N", "052° 10' 15'' W"],
    ["Transect", "Lab Shelf survey Fe", "55° 05' 29'' N", "055° 01' 08'' W"],
    ["", "", "54° 38' 55'' N", "055° 29' 29'' W"],
    ["", "", "54° 14' 19'' N", "055° 52' 06'' W"],
    ["Sampling Station", "Sta.2", "54° 14' 31'' N", "055° 52' 06'' W"],
    ["Sampling Station", "Sta.3", "54° 39' 09'' N", "055° 29' 50'' W"],
    ["Sampling Station", "Sta.4", "55° 05' 29'' N", "055° 01' 08'' W"],
    ["Transect", "Drift survey Nd", "54° 44' 28'' N", "052° 58' 33'' W"],
    ["", "", "54° 51' 32'' N", "052° 23' 13'' W"],
    ["", "", "55° 00' 34'' N", "051° 56' 17'' W"],
    ["", "", "55° 07' 47'' N", "051° 46' 51'' W"],
    ["Sampling Station", "Sta.5", "55° 07' 42'' N", "051° 47' 07'' W"],
    ["Sampling Station", "Sta.6", "54° 51' 53'' N", "052° 23' 09'' W"],
    ["Sampling Station", "Sta.7", "54° 44' 53'' N", "052° 58' 06'' W"],
    ["Sampling Station", "Sta.8", "55° 00' 17'' N", "051° 57' 13'' W"],
    ["Sampling Station", "Sta.9", "55° 46' 44'' N", "051° 05' 14'' W"],
    ["Sampling Station", "Sta.10", "57° 15' 13'' N", "050° 10' 14'' W"],
    ["Waypoint", "Waypoint - 2", "58° 58' 56'' N", "048° 46' 30'' W"],
    ["Transect", "S Greenland survey Fe", "60° 12' 07'' N", "047° 44' 44'' W"],
    ["", "", "60° 25' 47'' N", "047° 26' 41'' W"],
    ["", "", "60° 43' 50'' N", "047° 03' 33'' W"],
    ["Sampling Station", "Sta.11", "60° 43' 30'' N", "047° 03' 53'' W"],
    ["Sampling Station", "Sta.12", "60° 25' 18'' N", "047° 26' 54'' W"],
    ["Sampling Station", "Sta.13", "60° 12' 18'' N", "047° 44' 53'' W"],
    ["Waypoint", "Waypoint - 4", "60° 54' 30'' N", "049° 33' 12'' W"],
    ["Transect", "N Greenland survey", "61° 36' 11'' N", "051° 02' 22'' W"],
    ["", "", "61° 37' 18'' N", "050° 54' 31'' W"],
    ["", "", "61° 37' 00'' N", "050° 48' 21'' W"],
    ["", "", "61° 37' 48'' N", "050° 40' 05'' W"],
    ["", "", "61° 37' 00'' N", "050° 34' 08'' W"],
    ["", "", "61° 42' 02'' N", "050° 13' 45'' W"],
    ["", "", "61° 57' 40'' N", "049° 36' 36'' W"],
    ["Sampling Station", "Sta.13", "61° 57' 35'' N", "049° 37' 03'' W"],
    ["Sampling Station", "Sta.14", "61° 42' 01'' N", "050° 14' 24'' W"],
    ["Sampling Station", "Sta.15", "61° 37' 12'' N", "050° 34' 12'' W"],
    ["Sampling Station", "Sta.16", "61° 37' 12'' N", "050° 48' 36'' W"],
    ["Sampling Station", "Sta.17", "61° 36' 00'' N", "051° 02' 24'' W"],
    ["Sampling Station", "Sta.18", "61° 37' 12'' N", "050° 54' 36'' W"],
    ["Sampling Station", "Sta.19", "61° 37' 48'' N", "050° 40' 48'' W"],
    ["Waypoint", "Waypoint - 5", "60° 59' 23'' N", "049° 20' 36'' W"]
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
        const color = stationColors[type] || "gray";
        return L.circleMarker(latlng, {
          radius: 8,
          fillColor: color,
          color: "#000",
          weight: 1,
          opacity: 1,
          fillOpacity: 0.8
        });
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

    // Redraw transect lines
    const transectCoords = features
      .filter(f => f.properties.Station_Type === "Transect" && typesToShow.includes("Transect"))
      .map(f => [f.properties.Coordinates[1], f.properties.Coordinates[0]]);

    if (transectCoords.length > 1) {
      L.polyline(transectCoords, { color: 'orange', weight: 2, opacity: 0.7 }).addTo(map);
    }
  }

  const stationTypes = Object.keys(stationColors);
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
    label.style.marginRight = '1rem';
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
  #filter-controls {
    color: white;
  }
</style>
