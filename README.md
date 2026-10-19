<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>My Vinyl Player</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #171615;
  color: #f5eee5;
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
}

header {
  height: 65px;
  padding: 0 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid #39332d;
}

.logo {
  font-size: 14px;
  letter-spacing: 4px;
  font-weight: bold;
}

button {
  cursor: pointer;
  font: inherit;
}

button:active {
  opacity: 0.7;
}

.menu-btn {
  background: none;
  color: #f5eee5;
  border: 1px solid #55483c;
  padding: 9px 13px;
  border-radius: 30px;
}

main {
  max-width: 950px;
  margin: auto;
  padding: 25px 18px 45px;
}

h1 {
  font-size: 24px;
  font-weight: 500;
  margin-bottom: 8px;
}

.sub {
  color: #a99b8c;
  font-size: 12px;
  margin-bottom: 28px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 26px 15px;
}

.album {
  min-width: 0;
  cursor: pointer;
}

.cover {
  aspect-ratio: 1;
  overflow: hidden;
  background: linear-gradient(135deg, #78604c, #302824);
  box-shadow: 0 8px 20px #0005;
}

.cover img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.placeholder {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  color: #e0cdb5;
  gap: 10px;
}

.placeholder strong {
  font-size: 25px;
  font-weight: 300;
  letter-spacing: 3px;
}

.album-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 5px;
  margin-top: 10px;
}

.album-name {
  font-size: 13px;
  overflow-wrap: anywhere;
}

.edit-btn {
  border: none;
  background: none;
  color: #b7a28c;
  font-size: 11px;
  text-decoration: underline;
  padding: 5px;
}

.add-album {
  aspect-ratio: 1;
  width: 100%;
  background: transparent;
  color: #c5b29d;
  border: 1px dashed #655548;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 10px;
}

.add-album span:first-child {
  font-size: 35px;
  font-weight: 200;
}

#player,
#settings {
  display: none;
}

.player-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 20px;
}

.player-header h1 {
  flex: 1;
  font-size: 18px;
  overflow-wrap: anywhere;
}

.back {
  background: none;
  color: #eee2d3;
  border: 1px solid #55483c;
  padding: 9px 12px;
  border-radius: 30px;
}

.scene {
  position: relative;
  min-height: 360px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background: #332b25;
}

#ambient {
  position: absolute;
  inset: -30px;
  background-size: cover;
  background-position: center;
  filter: blur(40px) brightness(.5);
  transform: scale(1.2);
}

.deck {
  position: relative;
  width: min(100%, 510px);
  aspect-ratio: 1.2;
}

#jacket {
  position: absolute;
  left: 4%;
  top: 15%;
  width: 57%;
  aspect-ratio: 1;
  background: #514338;
  z-index: 1;
  box-shadow: 0 15px 35px #0008;
  overflow: hidden;
}

#jacket img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

#record {
  position: absolute;
  right: 0;
  top: 9%;
  width: 70%;
  aspect-ratio: 1;
  border-radius: 50%;
  z-index: 2;

  background:
    repeating-radial-gradient(
      circle,
      #191919 0px,
      #282828 2px,
      #111 3px,
      #222 5px
    );

  box-shadow: 0 15px 35px #0009;

  display: flex;
  align-items: center;
  justify-content: center;
}

#record::before {
  content: "";
  position: absolute;
  inset: 5%;
  border-radius: 50%;
  border: 1px solid #ffffff20;
}

#record.spinning {
  animation: rotateRecord 2.8s linear infinite;
}

@keyframes rotateRecord {
  to {
    transform: rotate(360deg);
  }
}

.label {
  width: 37%;
  aspect-ratio: 1;
  border-radius: 50%;
  background: #b99b76;
  border: 5px solid #171615;
  color: #30231a;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 7px;
  overflow: hidden;
  position: relative;
}

#labelText {
  font-size: 10px;
  overflow-wrap: anywhere;
}

.label::after {
  content: "";
  position: absolute;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: #f5e7d0;
}

#armHit {
  position: absolute;
  right: 0;
  top: 0;
  width: 48%;
  height: 55%;
  z-index: 5;
  cursor: pointer;
  touch-action: manipulation;
}

#arm {
  position: absolute;
  inset: 0;
  transform-origin: 89% 10%;
  transform: rotate(-9deg);
  transition: transform .6s ease;
  pointer-events: none;
}

#arm .pivot {
  position: absolute;
  right: 3%;
  top: 1%;
  width: 34px;
  height: 34px;
  border-radius: 50%;
  background: radial-gradient(circle at 35% 30%, #eee4d7, #837568 65%);
  border: 3px solid #a49380;
}

#arm .shaft {
  position: absolute;
  right: 13%;
  top: 8%;
  height: 70%;
  width: 8px;
  border-radius: 9px;
  background: linear-gradient(90deg, #62564a, #e7dac9, #78695a);
  transform: rotate(-4deg);
  transform-origin: top center;
}

#arm .head {
  position: absolute;
  right: 9%;
  top: 70%;
  width: 23px;
  height: 15px;
  background: linear-gradient(#d4c2ad, #77695c);
  border-radius: 3px;
  transform: rotate(20deg);
}

#arm.down {
  transform: rotate(13deg);
}

.play-status {
  text-align: center;
  color: #b4a18d;
  font-size: 10px;
  letter-spacing: 2px;
  margin-top: 20px;
}

.now-title {
  text-align: center;
  font-size: 15px;
  margin-top: 10px;
}

.now-sub {
  text-align: center;
  color: #938679;
  font-size: 11px;
}

.card {
  background: #24211e;
  border: 1px solid #494039;
  padding: 18px;
  margin-bottom: 15px;
}

.card h2 {
  font-size: 15px;
  font-weight: 500;
}

.field {
  margin: 16px 0;
}

.field label {
  display: block;
  font-size: 12px;
  color: #b7a99b;
  margin-bottom: 8px;
}

input[type="text"],
select {
  width: 100%;
  padding: 12px;
  background: #171615;
  color: white;
  border: 1px solid #55483c;
}

.action {
  padding: 12px 15px;
  background: #c6a780;
  color: #211912;
  border: none;
  margin: 5px 5px 5px 0;
}

.secondary {
  background: transparent;
  color: #eee2d3;
  border: 1px solid #55483c;
}

.file-label {
  display: inline-block;
  padding: 11px 13px;
  background: #393027;
  border: 1px solid #625142;
  font-size: 12px;
}

.file-label input {
  display: none;
}

.note {
  color: #a99b8c;
  font-size: 11px;
  line-height: 1.7;
}

@media (min-width: 650px) {
  .grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }

  .scene {
    min-height: 520px;
  }

  .deck {
    width: min(80%, 620px);
  }
}
</style>
</head>

<body>

<header>
  <div class="logo">VINYL / MY MUSIC</div>
  <button class="menu-btn" onclick="showSettings()">⚙ 設定</button>
</header>

<main>

<section id="collection">
  <div class="sub">YOUR PERSONAL COLLECTION</div>
  <h1>My Records</h1>
  <p class="sub">好きな音楽を、レコードで。</p>
  <div class="grid" id="grid"></div>
</section>

<section id="player">
  <div class="player-header">
    <button class="back" onclick="showCollection()">← 戻る</button>
    <h1 id="screenTitle">アルバム</h1>
    <button class="back" onclick="showSettings()">⚙</button>
  </div>

  <div class="scene">
    <div id="ambient"></div>

    <div class="deck">
      <div id="jacket"></div>

      <div id="record">
        <div class="label">
          <span id="labelText">MY RECORD</span>
        </div>
      </div>

      <div id="armHit"
           role="button"
           aria-label="トーンアームをタップして再生・停止"
           onclick="togglePlayback()">
        <div id="arm">
          <div class="pivot"></div>
          <div class="shaft"></div>
          <div class="head"></div>
        </div>
      </div>
    </div>
  </div>

  <p class="play-status" id="playStatus">TAP THE TONEARM TO PLAY</p>
  <p class="now-title" id="nowTitle">アルバムを選択</p>
  <p class="now-sub" id="nowSub">YOUR PERSONAL RECORD PLAYER</p>

  <audio id="audio"></audio>
</section>

<section id="settings">
  <div class="player-header">
    <button class="back" onclick="backFromSettings()">← 戻る</button>
    <h1>Settings</h1>
  </div>

  <div class="card">
    <h2>アルバムの編集</h2>

    <div class="field">
      <label for="chooseAlbum">アルバムを選択</label>
      <select id="chooseAlbum" onchange="loadForm()"></select>
    </div>

    <div class="field">
      <label for="nameInput">アルバム名</label>
      <input id="nameInput" type="text" placeholder="アルバム名">
    </div>

    <div class="field">
      <label>ジャケット画像</label>
      <label class="file-label">
        画像を選択
        <input id="imageInput" type="file" accept="image/*" onchange="imagePicked()">
      </label>
      <p class="note" id="imageStatus">未登録</p>
    </div>

    <div class="field">
      <label>音楽ファイル</label>
      <label class="file-label">
        音楽を選択
        <input id="audioInput" type="file" accept="audio/*" onchange="audioPicked()">
      </label>
      <p class="note" id="audioStatus">未登録</p>
    </div>

    <button class="action" onclick="saveForm()">変更を保存</button>
    <button class="action secondary" onclick="removeAlbum()">アルバムを削除</button>
  </div>

  <div class="card">
    <h2>新しいアルバムを追加</h2>

    <div class="field">
      <label for="newName">アルバム名</label>
      <input id="newName" type="text" placeholder="新しいアルバム">
    </div>

    <button class="action" onclick="addAlbum()">＋ アルバムを追加</button>
  </div>

  <p class="note">
    アルバム・画像・音楽はブラウザ内に保存されます。
    ブラウザのデータを消去すると登録内容が消える場合があります。
  </p>
</section>

</main>

<script>
const DB = "myVinylCollection";
const STORE = "albums";

let albums = [];
let current = 0;
let returnTo = "collection";
let newImage = null;
let newAudio = null;
let playing = false;

const $ = id => document.getElementById(id);

function openDB() {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(DB, 1);

    request.onupgradeneeded = () => {
      request.result.createObjectStore(STORE, {
        keyPath: "id",
        autoIncrement: true
      });
    };

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

async function getAlbums() {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const request = db.transaction(STORE).objectStore(STORE).getAll();
    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

async function putAlbum(album) {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const tx = db.transaction(STORE, "readwrite");
    tx.objectStore(STORE).put(album);
    tx.oncomplete = resolve;
    tx.onerror = () => reject(tx.error);
  });
}

async function insertAlbum(album) {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const request = db.transaction(STORE, "readwrite")
      .objectStore(STORE).add(album);

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

async function deleteAlbumDB(id) {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const tx = db.transaction(STORE, "readwrite");
    tx.objectStore(STORE).delete(id);
    tx.oncomplete = resolve;
    tx.onerror = () => reject(tx.error);
  });
}

async function init() {
  try {
    albums = await getAlbums();

    if (albums.length === 0) {
      for (const name of ["ライラック", "ブルーアワー", "ローズ", "オータム"]) {
        await insertAlbum({
          name,
          image: null,
          audio: null,
          audioName: ""
        });
      }

      albums = await getAlbums();
    }

    renderAlbums();
  } catch (error) {
    alert("保存機能を利用できませんでした。SafariまたはChromeで開いてください。");
  }
}

function makeCover(album) {
  if (album.image) {
    return '<img src="' + URL.createObjectURL(album.image) + '" alt="ジャケット">';
  }

  return '<div class="placeholder"><strong>VINYL</strong><span>YOUR RECORD</span></div>';
}

function renderAlbums() {
  const grid = $("grid");
  grid.innerHTML = "";

  albums.forEach((album, index) => {
    const card = document.createElement("div");
    card.className = "album";
    card.onclick = () => openAlbum(index);

    const cover = document.createElement("div");
    cover.className = "cover";
    cover.innerHTML = makeCover(album);

    const info = document.createElement("div");
    info.className = "album-info";

    const name = document.createElement("div");
    name.className = "album-name";
    name.textContent = album.name;

    const edit = document.createElement("button");
    edit.className = "edit-btn";
    edit.textContent = "編集";

    edit.onclick = event => {
      event.stopPropagation();
      showSettings(index);
    };

    info.append(name, edit);
    card.append(cover, info);
    grid.append(card);
  });

  const add = document.createElement("button");
  add.className = "add-album";
  add.innerHTML = "<span>＋</span><span>アルバムを追加</span>";

  add.onclick = () => {
    showSettings();
    $("newName").focus();
  };

  grid.append(add);
}

function showCollection() {
  stopPlayback();

  $("collection").style.display = "block";
  $("player").style.display = "none";
  $("settings").style.display = "none";

  renderAlbums();
}

function openAlbum(index) {
  current = index;

  $("collection").style.display = "none";
  $("settings").style.display = "none";
  $("player").style.display = "block";

  updatePlayer();
}

function updatePlayer() {
  const album = albums[current];
  if (!album) return;

  $("screenTitle").textContent = album.name;
  $("nowTitle").textContent = album.name;
  $("labelText").textContent = album.name;

  $("jacket").innerHTML = makeCover(album);

  $("ambient").style.backgroundImage = album.image
    ? 'url("' + URL.createObjectURL(album.image) + '")'
    : "none";

  const audio = $("audio");
  audio.pause();
  audio.removeAttribute("src");

  if (album.audio) {
    audio.src = URL.createObjectURL(album.audio);
    $("nowSub").textContent = album.audioName;
  } else {
    $("nowSub").textContent = "設定から音楽ファイルを登録してください";
  }

  audio.load();
  playing = false;

  $("record").classList.remove("spinning");
  $("arm").classList.remove("down");
  $("playStatus").textContent = "TAP THE TONEARM TO PLAY";
}

function showSettings(index) {
  returnTo = $("player").style.display === "block"
    ? "player"
    : "collection";

  if (Number.isInteger(index)) current = index;

  $("collection").style.display = "none";
  $("player").style.display = "none";
  $("settings").style.display = "block";

  newImage = null;
  newAudio = null;

  fillAlbumSelect();
  loadForm();
}

function fillAlbumSelect() {
  $("chooseAlbum").innerHTML = "";

  albums.forEach((album, index) => {
    const option = document.createElement("option");
    option.value = index;
    option.textContent = album.name;
    $("chooseAlbum").append(option);
  });

  if (albums.length) $("chooseAlbum").value = current;
}

function loadForm() {
  current = Number($("chooseAlbum").value) || 0;

  const album = albums[current];
  if (!album) return;

  $("nameInput").value = album.name;
  $("imageStatus").textContent = album.image ? "画像登録済み" : "未登録";
  $("audioStatus").textContent = album.audio
    ? "登録済み：" + album.audioName
    : "未登録";

  newImage = null;
  newAudio = null;
}

function imagePicked() {
  newImage = $("imageInput").files[0] || null;

  if (newImage) {
    $("imageStatus").textContent = "選択中：" + newImage.name;
  }
}

function audioPicked() {
  newAudio = $("audioInput").files[0] || null;

  if (newAudio) {
    $("audioStatus").textContent = "選択中：" + newAudio.name;
  }
}

async function saveForm() {
  const album = albums[current];
  if (!album) return;

  const name = $("nameInput").value.trim();

  if (!name) {
    alert("アルバム名を入力してください。");
    return;
  }

  album.name = name;

  if (newImage) album.image = newImage;

  if (newAudio) {
    album.audio = newAudio;
    album.audioName = newAudio.name;
  }

  await putAlbum(album);
  albums = await getAlbums();
  renderAlbums();

  if (returnTo === "player") {
    $("settings").style.display = "none";
    $("player").style.display = "block";
    updatePlayer();
  } else {
    showCollection();
  }
}

async function addAlbum() {
  const name = $("newName").value.trim();

  if (!name) {
    alert("アルバム名を入力してください。");
    return;
  }

  const id = await insertAlbum({
    name,
    image: null,
    audio: null,
    audioName: ""
  });

  albums = await getAlbums();
  current = albums.findIndex(album => album.id === id);

  $("newName").value = "";

  fillAlbumSelect();
  loadForm();
  renderAlbums();

  alert("アルバムを追加しました。");
}

async function removeAlbum() {
  const album = albums[current];
  if (!album) return;

  if (!confirm("「" + album.name + "」を削除しますか？")) return;

  await deleteAlbumDB(album.id);
  albums = await getAlbums();

  if (albums.length === 0) {
    await insertAlbum({
      name: "新しいアルバム",
      image: null,
      audio: null,
      audioName: ""
    });

    albums = await getAlbums();
  }

  current = Math.min(current, albums.length - 1);
  showCollection();
}

function backFromSettings() {
  if (returnTo === "player") {
    $("settings").style.display = "none";
    $("player").style.display = "block";
    updatePlayer();
  } else {
    showCollection();
  }
}

async function togglePlayback() {
  const album = albums[current];

  if (!album || !album.audio) {
    alert("設定から音楽ファイルを登録してください。");
    return;
  }

  if (playing) {
    $("audio").pause();
    playing = false;

    $("record").classList.remove("spinning");
    $("arm").classList.remove("down");
    $("playStatus").textContent = "TAP THE TONEARM TO PLAY";
  } else {
    try {
      await $("audio").play();

      playing = true;
      $("record").classList.add("spinning");
      $("arm").classList.add("down");
      $("playStatus").textContent = "NOW PLAYING · TAP ARM TO PAUSE";
    } catch (error) {
      alert("この音楽ファイルを再生できませんでした。");
    }
  }
}

function stopPlayback() {
  $("audio").pause();

  try {
    $("audio").currentTime = 0;
  } catch (error) {}

  playing = false;

  $("record").classList.remove("spinning");
  $("arm").classList.remove("down");
}

$("audio").addEventListener("ended", () => {
  playing = false;

  $("record").classList.remove("spinning");
  $("arm").classList.remove("down");
  $("playStatus").textContent = "PLAYBACK FINISHED";
});

init();
</script>

</body>
</html>
