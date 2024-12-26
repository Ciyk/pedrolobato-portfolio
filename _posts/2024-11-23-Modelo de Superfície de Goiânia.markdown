---
layout: post
title:  "Modelo de superfície de Goiânia usando imagens do satélite Copernicus!"
date:   2024-11-23 13:36:58 -0300
categories: [Mapas, MDS]
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" crossorigin=""></script>

Este é um modelo de terreno em dois tons da cidade de Goiânia feito a partir das imagens do satélite Copernicus. Explore as camadas disponíveis:

<div id="map" style="width: 100%; height: 500px; margin: 20px 0;"></div>

<script>
  // Inicializa o mapa
  var map = L.map('map').setView([-16.6789, -49.2539], 10);

  // Adiciona os tiles gerados pelo gdal2tiles.py
  var tiles = L.tileLayer('/_map-resources/mds_goiania/MDE_OpenTopografy_Copernicus_30M_folder/{z}/{x}/{y}.png', {
    maxZoom: 15,
    attribution: 'Mapa criado por Pedro Lobato'
  }).addTo(map);

  // Carrega o GeoJSON
  var geojsonLayer = L.geoJSON(null, {
    style: function (feature) {
      return {
        color: "#ff7800",
        weight: 2
      };
    }
  }).addTo(map);

  // Faz o carregamento assíncrono do GeoJSON
  fetch('/_map-resources/mds_goiania/Contornos20M.geojson')
    .then(response => response.json())
    .then(data => geojsonLayer.addData(data))
    .catch(error => console.error('Erro ao carregar o GeoJSON:', error));
</script>