---
layout: post
title: "Analise do terreno de Goiânia usando MDE do satélite Copernicus!"
date: 2024-11-23 13:36:58 -0300
categories: [Mapas, MDS]
---

{% raw %}
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<style>
  #map {
    width: 100%;
    height: 500px;
    margin: 20px 0;
  }
  .info {
    padding: 6px 8px;
    font: 14px/16px Arial, Helvetica, sans-serif;
    background: white;
    background: rgba(255,255,255,0.8);
    box-shadow: 0 0 15px rgba(0,0,0,0.2);
    border-radius: 5px;
  }
</style>

Este é um modelo de terreno em dois tons da cidade de Goiânia feito a partir das imagens do satélite Copernicus. Explore as camadas disponíveis:

<div id="map"></div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  // Inicializa o mapa
  var map = L.map('map').setView([-16.6789, -49.2539], 10);

  // Define as camadas de tiles
  var mdeLayer = L.tileLayer('/_map-resources/mds_goiania/MDE_OpenTopografy_Copernicus_30M_folder/{z}/{x}/{y}.png', {
    maxZoom: 15,
    minZoom: 0,
    tms: true,
    attribution: 'Mapa criado por Pedro Lobato'
  });

  var analiseLayer = L.tileLayer('/_map-resources/mds_goiania/Recortado_mascara_folder/{z}/{x}/{y}.png', {
    maxZoom: 15,
    minZoom: 0,
    tms: true,
    opacity: 1,
    attribution: 'Mapa criado por Pedro Lobato'
  });

  // Carrega o GeoJSON
  var geojsonLayer = L.geoJSON(null, {
    style: function(feature) {
      return {
        color: "#ff7800",
        weight: 2,
        opacity: 1,
        fillOpacity: 0.3
      };
    }
  });

  // Define as camadas base e sobreposições
  var baseMaps = {
    "Modelo Digital de Elevação": mdeLayer
  };

  var overlayMaps = {
    "Análise do Terreno": analiseLayer,
    "Contornos": geojsonLayer
  };

  // Adiciona o controle de camadas
  L.control.layers(baseMaps, overlayMaps).addTo(map);

  // Adiciona as camadas iniciais
  mdeLayer.addTo(map);  // Camada base (sempre visível)
  analiseLayer.addTo(map);  // Já inicia com a análise de terreno visível
  geojsonLayer.addTo(map);  // Já inicia com os contornos visíveis

  // Faz o carregamento assíncrono do GeoJSON
  fetch('/_map-resources/mds_goiania/Contornos20M.geojson')
    .then(response => response.json())
    .then(data => {
      geojsonLayer.addData(data);
      map.fitBounds(geojsonLayer.getBounds());
    })
    .catch(error => console.error('Erro ao carregar o GeoJSON:', error));
});
</script>
{% endraw %}