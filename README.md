/game
  /assets
    /images
      - character.png
      - monster.png
  - index.html
  - main.js
  - style.css
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>寶可夢捕捉遊戲</title>
  <link rel="stylesheet" href="style.css">
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
</head>
<body>
  <div id="game-container">
    <div id="map"></div>
    <div id="info-panel">
      <p>角色位置：(<span id="latitude">0</span>, <span id="longitude">0</span>)</p>
      <p>怪獸數量：<span id="monster-count">100</span></p>
      <button id="login-btn">登入</button>
    </div>
  </div>
  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js"></script>
  <script src="main.js"></script>
</body>
</html>
body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  font-family: Arial, sans-serif;
}

#game-container {
  display: flex;
  height: 100%;
}

#map {
  width: 75%;
  height: 100%;
}

#info-panel {
  width: 25%;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.8);
}

button {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}
// Firebase 配置
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

// 初始化 Firebase
const app = firebase.initializeApp(firebaseConfig);
const auth = firebase.getAuth(app);
const db = firebase.getFirestore(app);

// 初始化地圖
const map = L.map('map').setView([23.5, 121], 13); // 預設台灣地區

// 加載地圖圖磚
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

// 角色圖示
const characterIcon = L.icon({
  iconUrl: 'assets/images/character.png',
  iconSize: [50, 50],
  iconAnchor: [25, 25]
});

// 創建角色標記
const characterMarker = L.marker([23.5, 121], { icon: characterIcon }).addTo(map);

// 更新角色位置
function updatePosition(lat, lon) {
  characterMarker.setLatLng([lat, lon]);
  document.getElementById('latitude').textContent = lat;
  document.getElementById('longitude').textContent = lon;
}

// 監聽地圖點擊事件，更新角色位置
map.on('click', function(e) {
  const lat = e.latlng.lat;
  const lon = e.latlng.lng;
  updatePosition(lat, lon);
  generateMonsters(lat, lon);
});

// 怪獸圖示
const monsterIcon = L.icon({
  iconUrl: 'assets/images/monster.png',
  iconSize: [50, 50],
  iconAnchor: [25, 25]
});

// 儲存怪獸標記
const monsters = [];

// 隨機生成怪獸
function generateMonsters(lat, lon) {
  // 清除現有怪獸
  monsters.forEach(monster => map.removeLayer(monster));
  monsters.length = 0;

  // 生成 5 隻怪獸
  for (let i = 0; i < 5; i++) {
    const offsetLat = (Math.random() - 0.5) * 0.01;
    const offsetLon = (Math.random() - 0.5) * 0.01;
    const monsterLat = lat + offsetLat;
    const monsterLon = lon + offsetLon;

    const monsterMarker = L.marker([monsterLat, monsterLon], { icon: monsterIcon }).addTo(map);
    monsters.push(monsterMarker);
  }

  // 更新怪獸數量
  document.getElementById('monster-count').textContent = monsters.length;
}

// 捕捉怪獸
function catchMonster() {
  if (monsters.length > 0) {
    // 隨機選擇一隻怪獸
    const monsterIndex = Math.floor(Math.random() * monsters.length);
    const monster = monsters[monsterIndex];

    // 從地圖上移除怪獸
    map.removeLayer(monster);
    monsters.splice(monsterIndex, 1);

    // 更新怪獸數量
    document.getElementById('monster-count').textContent = monsters.length;

    // 顯示捕捉成功訊息
    alert('捕捉成功！');
  } else {
    alert('周圍沒有怪獸可供捕捉。');
  }
}

// 登入功能
document.getElementById('login-btn').addEventListener('click', function() {
  const provider = new firebase.GoogleAuthProvider();
  firebase.signInWithPopup(auth, provider)
    .then((result) => {
      // 登入成功，取得使用者資訊
      const user = result.user;
      alert('歡迎，' + user.displayName);
     
::contentReference[oaicite:0]{index=0}
 
