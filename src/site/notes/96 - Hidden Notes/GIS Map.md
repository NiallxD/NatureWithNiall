---
{"dg-publish":true,"dg-permalink":"/gis-map","permalink":"/gis-map/","title":"🗺️ GIS Map","hide":true,"noteIcon":null,"created":"2024-10-08T22:42:37.161+01:00","updated":"2024-10-09T11:45:30.926+01:00"}
---

# GIS Map

This page is where I host my online GIS maps that I want to share with others.


<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="initial-scale=1,user-scalable=no,maximum-scale=1,width=device-width">
        <meta name="mobile-web-app-capable" content="yes">
        <meta name="apple-mobile-web-app-capable" content="yes">
        <link rel="stylesheet" href="css/leaflet.css">
        <link rel="stylesheet" href="css/L.Control.Layers.Tree.css">
        <link rel="stylesheet" href="css/qgis2web.css">
        <link rel="stylesheet" href="css/fontawesome-all.min.css">
        <style>
        #map {
            width: 1915px;
            height: 1177px;
        }
        </style>
        <title>No.  Photos Taken by UK County</title>
    </head>
    <body>
        <div id="map">
        </div>
        <script src="js/qgis2web_expressions.js"></script>
        <script src="js/leaflet.js"></script>
        <script src="js/L.Control.Layers.Tree.min.js"></script>
        <script src="js/leaflet.rotatedMarker.js"></script>
        <script src="js/leaflet.pattern.js"></script>
        <script src="js/leaflet-hash.js"></script>
        <script src="js/Autolinker.min.js"></script>
        <script src="js/rbush.min.js"></script>
        <script src="js/labelgun.min.js"></script>
        <script src="js/labels.js"></script>
        <script src="data/Count_1.js"></script>
        <script>
        var map = L.map('map', {
            zoomControl:false, maxZoom:28, minZoom:1
        })
        var hash = new L.Hash(map);
        map.attributionControl.setPrefix('<a href="https://github.com/tomchadwin/qgis2web" target="_blank">qgis2web</a> &middot; <a href="https://leafletjs.com" title="A JS library for interactive maps">Leaflet</a> &middot; <a href="https://qgis.org">QGIS</a>');
        var autolinker = new Autolinker({truncate: {length: 30, location: 'smart'}});
        // remove popup's row if "visible-with-data"
        function removeEmptyRowsFromPopupContent(content, feature) {
         var tempDiv = document.createElement('div');
         tempDiv.innerHTML = content;
         var rows = tempDiv.querySelectorAll('tr');
         for (var i = 0; i < rows.length; i++) {
             var td = rows[i].querySelector('td.visible-with-data');
             var key = td ? td.id : '';
             if (td && td.classList.contains('visible-with-data') && feature.properties[key] == null) {
                 rows[i].parentNode.removeChild(rows[i]);
             }
         }
         return tempDiv.innerHTML;
        }
        // add class to format popup if it contains media
		function addClassToPopupIfMedia(content, popup) {
			var tempDiv = document.createElement('div');
			tempDiv.innerHTML = content;
			if (tempDiv.querySelector('td img')) {
				popup._contentNode.classList.add('media');
					// Delay to force the redraw
					setTimeout(function() {
						popup.update();
					}, 10);
			} else {
				popup._contentNode.classList.remove('media');
			}
		}
        var title = new L.Control({'position':'topright'});
        title.onAdd = function (map) {
            this._div = L.DomUtil.create('div', 'info');
            this.update();
            return this._div;
        };
        title.update = function () {
            this._div.innerHTML = '<h2>No.  Photos Taken by UK County</h2>';
        };
        title.addTo(map);
        var abstract = new L.Control({'position':'topright'});
        abstract.onAdd = function (map) {
            this._div = L.DomUtil.create('div',
            'leaflet-control abstract');
            this._div.id = 'abstract'

                abstract.show();
                return this._div;
            };
            abstract.show = function () {
                this._div.classList.remove("abstract");
                this._div.classList.add("abstractUncollapsed");
                this._div.innerHTML = 'This map shows the number of photographs I have taken in each of the UK\'s counties where darker colours represent less photographs and lighter colours represent more.';
        };
        abstract.addTo(map);
        var zoomControl = L.control.zoom({
            position: 'topleft'
        }).addTo(map);
        var bounds_group = new L.featureGroup([]);
        function setBounds() {
            if (bounds_group.getLayers().length) {
                map.fitBounds(bounds_group.getBounds());
            }
        }
        map.createPane('pane_EsriLightGray_0');
        map.getPane('pane_EsriLightGray_0').style.zIndex = 400;
        var layer_EsriLightGray_0 = L.tileLayer('https://server.arcgisonline.com/arcgis/rest/services/Canvas/World_Light_Gray_Base/MapServer/tile/{z}/{y}/{x}', {
            pane: 'pane_EsriLightGray_0',
            opacity: 1.0,
            attribution: '',
            minZoom: 1,
            maxZoom: 28,
        });
        layer_EsriLightGray_0;
        map.addLayer(layer_EsriLightGray_0);
        function pop_Count_1(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['CTYUA23CD'] !== null ? autolinker.link(feature.properties['CTYUA23CD'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['CTYUA23NM'] !== null ? autolinker.link(feature.properties['CTYUA23NM'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['CTYUA23NMW'] !== null ? autolinker.link(feature.properties['CTYUA23NMW'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['BNG_E'] !== null ? autolinker.link(feature.properties['BNG_E'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['BNG_N'] !== null ? autolinker.link(feature.properties['BNG_N'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['LONG'] !== null ? autolinker.link(feature.properties['LONG'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['LAT'] !== null ? autolinker.link(feature.properties['LAT'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['GlobalID'] !== null ? autolinker.link(feature.properties['GlobalID'].toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['NUMPOINTS'] !== null ? autolinker.link(feature.properties['NUMPOINTS'].toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_Count_1_0(feature) {
            if (feature.properties['NUMPOINTS'] >= 0.000000 && feature.properties['NUMPOINTS'] <= 61.000000 ) {
                return {
                pane: 'pane_Count_1',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(0,0,4,1.0)',
                interactive: true,
            }
            }
            if (feature.properties['NUMPOINTS'] >= 61.000000 && feature.properties['NUMPOINTS'] <= 269.000000 ) {
                return {
                pane: 'pane_Count_1',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(80,18,123,1.0)',
                interactive: true,
            }
            }
            if (feature.properties['NUMPOINTS'] >= 269.000000 && feature.properties['NUMPOINTS'] <= 746.000000 ) {
                return {
                pane: 'pane_Count_1',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(182,55,122,1.0)',
                interactive: true,
            }
            }
            if (feature.properties['NUMPOINTS'] >= 746.000000 && feature.properties['NUMPOINTS'] <= 3118.000000 ) {
                return {
                pane: 'pane_Count_1',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(251,135,97,1.0)',
                interactive: true,
            }
            }
            if (feature.properties['NUMPOINTS'] >= 3118.000000 && feature.properties['NUMPOINTS'] <= 11855.000000 ) {
                return {
                pane: 'pane_Count_1',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(252,253,191,1.0)',
                interactive: true,
            }
            }
        }
        map.createPane('pane_Count_1');
        map.getPane('pane_Count_1').style.zIndex = 401;
        map.getPane('pane_Count_1').style['mix-blend-mode'] = 'normal';
        var layer_Count_1 = new L.geoJson(json_Count_1, {
            attribution: '',
            interactive: true,
            dataVar: 'json_Count_1',
            layerName: 'layer_Count_1',
            pane: 'pane_Count_1',
            onEachFeature: pop_Count_1,
            style: style_Count_1_0,
        });
        bounds_group.addLayer(layer_Count_1);
        map.addLayer(layer_Count_1);
        setBounds();
        </script>
    </body>
</html>



---
Created by Niall Bell (niall@niallbell.com)