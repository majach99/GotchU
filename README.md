# GotchU
<!DOCTYPE html>
<html lang="cs">
<head>
<meta charset="UTF-8">
<title>Retro Tamagotchi</title>
<style>
  body {
    font-family: monospace;
    background: black;
    color: lime;
    text-align: center;
  }
  .box {
    border: 3px solid lime;
    padding: 20px;
    display: inline-block;
    margin-top: 40px;
  }
  button {
    background: black;
    color: lime;
    border: 1px solid lime;
    margin: 5px;
    padding: 10px;
    cursor: pointer;
  }
</style>
</head>
<body>

<div class="box">
  <h2 id="name"></h2>
  <p id="stage"></p>
  <p>🍔 Hlad: <span id="hunger"></span></p>
  <p>😊 Nálada: <span id="mood"></span></p>
  <p>⚡ Energie: <span id="energy"></span></p>
  <p>🤒 Nemoc: <span id="sick"></span></p>

  <button onclick="feed()">Krmit</button>
  <button onclick="play()">Hrát si</button>
  <button onclick="clean()">Uklidit</button>
</div>

<script>
let pet = JSON.parse(localStorage.getItem("pet"));

if (!pet) {
  const name = prompt("Jméno mazlíčka:");
  const animal = prompt("Zvíře (např. pes, kočka, had):");

  pet = {
    name,
    animal,
    hunger: 50,
    mood: 50,
    energy: 50,
    sick: false,
    age: 0,
    lastUpdate: Date.now()
  };
}

function updateStats() {
  const now = Date.now();
  const diff = Math.floor((now - pet.lastUpdate) / 1000);

  pet.hunger += diff * 0.1;
  pet.energy -= diff * 0.1;
  pet.mood -= diff * 0.05;

  if (pet.hunger > 80) pet.sick = true;

  pet.age += diff;
  pet.lastUpdate = now;

  save();
}

function getStage() {
  if (pet.age < 60) return "👶 Miminko";
  if (pet.age < 180) return "🧒 Teen";
  if (pet.age < 400) return "🧑 Dospělý";
  return "👴 Důchodce";
}

function render() {
  updateStats();

  document.getElementById("name").innerText =
    pet.name + " (" + pet.animal + ")";
  document.getElementById("stage").innerText = getStage();
  document.getElementById("hunger").innerText = Math.floor(pet.hunger);
  document.getElementById("mood").innerText = Math.floor(pet.mood);
  document.getElementById("energy").innerText = Math.floor(pet.energy);
  document.getElementById("sick").innerText = pet.sick ? "ANO" : "NE";
}

function feed() {
  pet.hunger -= 10;
  pet.mood += 5;
  save();
  render();
}

function play() {
  pet.mood += 10;
  pet.energy -= 5;
  save();
  render();
}

function clean() {
  pet.sick = false;
  pet.mood += 5;
  save();
  render();
}

function save() {
  localStorage.setItem("pet", JSON.stringify(pet));
}

setInterval(render, 1000);
render();
</script>

</body>
</html>
