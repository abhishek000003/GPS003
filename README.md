<!DOCTYPE html>
<html>
<head>
  <title>GPS Location Map</title>
  <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_GOOGLE_MAPS_API_KEY&libraries=geometry"></script>
  <script>
    var map, marker;

    function initMap() {
      var myLatLng = { lat: 0, lng: 0 }; // Initial position
      map = new google.maps.Map(document.getElementById('map'), {
        center: myLatLng,
        zoom: 15
      });
      marker = new google.maps.Marker({
        position: myLatLng,
        map: map
      });

      var ws = new WebSocket('ws://' + window.location.hostname + ':80/');
      ws.onmessage = function (event) {
        var coords = event.data.split(',');
        var latitude = parseFloat(coords[0]);
        var longitude = parseFloat(coords[1]);

        var newLatLng = new google.maps.LatLng(latitude, longitude);
        marker.setPosition(newLatLng);
        map.panTo(newLatLng);
      };
    }
  </script>
  <style>
    #map {
      height: 400px;
      width: 100%;
    }
  </style>
</head>
<body onload="initMap()">
  <div id="map"></div>
</body>
</html>
