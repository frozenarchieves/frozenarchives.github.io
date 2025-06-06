---
layout: post
title: "1 Map Test 🌊 Expedition Alert: Heading to the Labrador Sea!"
date: 2025-05-25 16:11:00-0400
inline: false
related_posts: false
map: true
---


<!--Working Map in a container 
<div id="map" style="height: 600px; margin: 20px 0;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  const map = L.map('map').setView([54.5, -54.5], 5);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);

  const geojsonData = {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-63.4936111111111, 44.54777777777777]
        },
        "properties": {
          "Name": "Pick up Yang",
          "Station_Type": "Waypoint"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-61.08555555555556, 42.40388888888891]
        },
        "properties": {
          "Name": "Sta.1",
          "Station_Type": "Sampling Station"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-52.17083333333333, 46.6927777777778]
        },
        "properties": {
          "Name": "Waypoint - 3",
          "Station_Type": "Waypoint"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-55.01888888888887, 55.09138888888889]
        },
        "properties": {
          "Name": "Lab Shelf survey Fe",
          "Station_Type": "Transect"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-55.86833333333333, 54.24194444444444]
        },
        "properties": {
          "Name": "Sta.2",
          "Station_Type": "Sampling Station"
        }
      }
      // Add additional features here if needed...
    ]
  };

  const geoLayer = L.geoJSON(geojsonData, {
    onEachFeature: function (feature, layer) {
      const name = feature.properties.Name || "Unknown Station";
      const type = feature.properties.Station_Type || "Unknown Type";
      layer.bindPopup(`<strong>${name}</strong><br>${type}`);
    }
  }).addTo(map);

  map.fitBounds(geoLayer.getBounds());
</script>

-->

 another beautiful map 
<div id="map" style="height: 600px; margin: 20px 0;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  const map = L.map('map').setView([54.5, -54.5], 5);

  // Map style using CartoDB Positron
  L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> &copy; <a href="https://carto.com/attributions">CARTO</a>',
    subdomains: 'abcd',
    maxZoom: 19
  }).addTo(map);

  const stationColors = {
    "Waypoint": "blue",
    "Sampling Station": "green",
    "Transect": "orange",
    "Unknown Type": "gray"
  };

  const geojsonData = {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-63.4936111111111, 44.54777777777777]
        },
        "properties": {
          "Name": "Pick up Yang",
          "Station_Type": "Waypoint"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-61.08555555555556, 42.40388888888891]
        },
        "properties": {
          "Name": "Sta.1",
          "Station_Type": "Sampling Station"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-52.17083333333333, 46.6927777777778]
        },
        "properties": {
          "Name": "Waypoint - 3",
          "Station_Type": "Waypoint"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-55.01888888888887, 55.09138888888889]
        },
        "properties": {
          "Name": "Lab Shelf survey Fe",
          "Station_Type": "Transect"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-55.86833333333333, 54.24194444444444]
        },
        "properties": {
          "Name": "Sta.2",
          "Station_Type": "Sampling Station"
        }
      }
    ]
  };

  const geoLayer = L.geoJSON(geojsonData, {
    pointToLayer: function (feature, latlng) {
      const type = feature.properties.Station_Type || "Unknown Type";
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
      const name = feature.properties.Name || "Unknown Station";
      const type = feature.properties.Station_Type || "Unknown Type";
      layer.bindPopup(`<strong>${name}</strong><br>${type}`);
    }
  }).addTo(map);

  map.fitBounds(geoLayer.getBounds());
</script>

<!---


<div id="map" style="height: 600px; margin: 20px 0;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://unpkg.com/leaflet.globe@0.2.2/leaflet.globe.min.js"></script>

<script>
  const map = L.globe('map', {
    atmosphere: true,
    sky: true,
    enableRotation: true,
    zoom: 2
  });

  const stationColors = {
    "Waypoint": "blue",
    "Sampling Station": "green",
    "Transect": "orange",
    "Unknown Type": "gray"
  };

  const geojsonData = {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-63.4936111111111, 44.54777777777777]
        },
        "properties": {
          "Name": "Pick up Yang",
          "Station_Type": "Waypoint"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-61.08555555555556, 42.40388888888891]
        },
        "properties": {
          "Name": "Sta.1",
          "Station_Type": "Sampling Station"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-52.17083333333333, 46.6927777777778]
        },
        "properties": {
          "Name": "Waypoint - 3",
          "Station_Type": "Waypoint"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-55.01888888888887, 55.09138888888889]
        },
        "properties": {
          "Name": "Lab Shelf survey Fe",
          "Station_Type": "Transect"
        }
      },
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-55.86833333333333, 54.24194444444444]
        },
        "properties": {
          "Name": "Sta.2",
          "Station_Type": "Sampling Station"
        }
      }
    ]
  };

  const geoLayer = L.geoJSON(geojsonData, {
    pointToLayer: function (feature, latlng) {
      const type = feature.properties.Station_Type || "Unknown Type";
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
      const name = feature.properties.Name || "Unknown Station";
      const type = feature.properties.Station_Type || "Unknown Type";
      layer.bindPopup(`<strong>${name}</strong><br>${type}`);
    }
  });

  geoLayer.addTo(map);
</script>
-->
<style>
  .leaflet-control-attribution {
    display: none !important;
  }
</style>

<h2>🌊 <strong>SEALS to the Labrador Sea! 🧽</strong></h2>
<p>Starting <strong>June 3rd, 2025</strong>, I'll be embarking on a month-long research expedition aboard R/V Roger Revelle for <strong>SEALS 2025</strong> (<em>Sediment Exchange Along Labrador Sea</em>) to investigate neodymium and iron fluxes in the Labrador and Greenland coasts — a critical region for understanding deep ocean circulation and climate change.</p>
<p>We'll be collecting seawater and sediment samples across multiple stations, working with an amazing team to uncover what lies beneath the waves.</p>
