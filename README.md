<!DOCTYPE html><html lang="ja">
<head>
<meta charset="UTF-8">
<title>お散歩マップ</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0"><!-- 地図ライブラリ --><link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script><style>
body {
  margin: 0;
  font-family: sans-serif;
}

#map {
  height: 90vh;
}

.controls {
  text-align: center;
  padding: 10px;
}
button {
  padding: 10px 20px;
  font-size: 16px;
}
</style></head>
<body><div id="map"></div><div class="controls">
  <button onclick="startTracking()">記録開始</button>
  <button onclick="stopTracking()">停止</button>
</div><script>
let map = L.map('map').setView([33.5597, 133.5311], 15); // 高知あたり

// 地図表示
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  attribution: '© OpenStreetMap'
}).addTo(map);

let path = [];
let polyline = L.polyline(path, {color: 'red'}).addTo(map);

let watchId = null;

// 記録開始
function startTracking() {
  if (!navigator.geolocation) {
    alert("位置情報が使えません");
    return;
  }

  watchId = navigator.geolocation.watchPosition(position => {
    const lat = position.coords.latitude;
    const lng = position.coords.longitude;

    path.push([lat, lng]);
    polyline.setLatLngs(path);

    map.setView([lat, lng]);
  },
  error => {
    alert("位置情報の取得に失敗しました");
  },
  {
    enableHighAccuracy: true
  });
}

// 記録停止
function stopTracking() {
  if (watchId !== null) {
    navigator.geolocation.clearWatch(watchId);
    watchId = null;
  }
}
</script></body>
</html># -
