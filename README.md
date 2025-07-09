<!DOCTYPE html>
<html lang="da">
<head>
  <meta charset="UTF-8">
  <title>Savner ord</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 600px;
      margin: 2rem auto;
      padding: 1rem;
    }
    textarea {
      width: 100%;
      height: 150px;
      font-size: 1.2rem;
    }
    .mistede {
      font-weight: bold;
      color: darkred;
    }
    button {
      margin-right: 1rem;
      margin-top: 1rem;
    }
    .sporgsmal {
      margin-top: 1rem;
      background-color: #f0f0f0;
      padding: 1rem;
      border-left: 4px solid #999;
    }
  </style>
</head>
<body>
  <h1>Savner ord</h1>
  <p>Du bruger ikke alle bogstaver altid. Men hvor mange kan du miste, før du mærker, at du savner ord?</p>

  <div>
    <button onclick="setRound(0)">Start Runde 0</button>
    <button onclick="setRound(1)">Start Runde 1</button>
    <button onclick="setRound(2)">Start Runde 2</button>
    <button onclick="setRound(3)">Start Runde 3</button>
    <button onclick="resetGame()">Nulstil</button>
  </div>

  <p>Mistede bogstaver: <span id="forbudteBogstaver" class="mistede"></span></p>
  <textarea id="tekstfelt" oninput="checkInput(event)"></textarea>

  <div id="sporgsmaalsboks" class="sporgsmal"></div>

  <script>
    const tekstfelt = document.getElementById("tekstfelt");
    const forbudtEl = document.getElementById("forbudteBogstaver");
    const sporgsmaalBox = document.getElementById("sporgsmaalsboks");

    let forbudte = [];

    const runder = {
      0: [],
      1: ["Z", "Q", "W", "X"],
      2: ["Z", "Q", "W", "X", "G", "B", "J", "Å"],
      3: ["Z", "Q", "W", "X", "G", "B", "J", "Å", "D", "M", "K", "U"]
    };

    const sporgsmaal = {
      0: "Skriv en beskrivelse af noget, du holder af i naturen. Hvorfor betyder det noget for dig?",
      1: "Skriv samme tekst igen. Hvad begynder at føles besværligt? Hvilke ord må du ændre eller opgive?",
      2: "Skriv nu noget om, hvorfor nogle ting i naturen virker uvigtige – men måske ikke er det. Hvad mangler du nu i sproget?",
      3: "Skriv en refleksion: Hvordan hænger det, du oplever her, sammen med natur, tab og kompleksitet?"
    };

    function setRound(runde) {
      forbudte = runder[runde];
      forbudtEl.textContent = forbudte.length ? forbudte.join(", ") : "Ingen";
      tekstfelt.value = "";
      tekstfelt.focus();
      sporgsmaalBox.textContent = sporgsmaal[runde];
    }

    function resetGame() {
      forbudte = [];
      forbudtEl.textContent = "";
      tekstfelt.value = "";
      tekstfelt.focus();
      sporgsmaalBox.textContent = "";
    }

    function checkInput(event) {
      let nyTekst = "";
      for (let i = 0; i < tekstfelt.value.length; i++) {
        const tegn = tekstfelt.value[i];
        if (!forbudte.includes(tegn.toUpperCase())) {
          nyTekst += tegn;
        }
      }
      tekstfelt.value = nyTekst;
    }
  </script>
</body>
</html>
