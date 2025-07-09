<!DOCTYPE html>
<html lang="da">
<head>
  <meta charset="UTF-8">
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
      display: none;
    }
    #timer {
      font-weight: bold;
      margin-top: 1rem;
      font-size: 1.1rem;
    }
    #tidligereSvarListe {
      margin-top: 1rem;
    }
    .tidligereSvar {
      background-color: #eef2f5;
      padding: 1rem;
      border-left: 4px solid #ccc;
      font-style: italic;
      white-space: pre-wrap;
      margin-bottom: 1rem;
    }
    #mistedeContainer {
      display: none;
    }
  </style>
</head>
<body>
  <h1>Savner ord</h1>
  <p>Du bruger ikke alle bogstaver altid. Men hvor mange kan du miste, før du mærker, at du savner ord?</p>

  <div>
    <button onclick="startAuto()">Start</button>
    <button id="resetBtn" onclick="resetGame()" style="display:none;">Nulstil</button>
  </div>

  <p id="mistedeContainer">Mistede bogstaver: <span id="forbudteBogstaver" class="mistede"></span></p>

  <div id="sporgsmaalsboks" class="sporgsmal">Hvad værdsætter du især ved naturen?</div>

  <textarea id="tekstfelt" oninput="checkInput(event)"></textarea>

  <div id="tidligereSvarListe"></div>

  <div id="timer"></div>

  <script>
    const tekstfelt = document.getElementById("tekstfelt");
    const forbudtEl = document.getElementById("forbudteBogstaver");
    const sporgsmaalBox = document.getElementById("sporgsmaalsboks");
    const timerEl = document.getElementById("timer");
    const tidligereSvarListe = document.getElementById("tidligereSvarListe");
    const mistedeContainer = document.getElementById("mistedeContainer");
    const resetBtn = document.getElementById("resetBtn");

    let forbudte = [];
    let currentRound = 0;
    let countdown;

    const bogstaverEfterFrekvens = [
      "E", "R", "N", "T", "A", "I", "S", "L", "O", "D", "M", "G", "K", "U", "B", "V", "F", "H", "Æ", "P", "Ø", "Å", "J", "Y", "C", "X", "W", "Z", "Q"
    ];

    const runder = {
      0: [],
      1: bogstaverEfterFrekvens.slice(-4),
      2: bogstaverEfterFrekvens.slice(-8),
      3: bogstaverEfterFrekvens.slice(-12),
      4: bogstaverEfterFrekvens.slice(-16),
      5: bogstaverEfterFrekvens.slice(),
    };

    const roundTimes = [15, 15, 15, 15, 15];

    function procentMistede(runde) {
      if (!runder[runde] || runder[runde].length === 0) return "0% mistet";
      const procent = Math.round((runder[runde].length / 29) * 100);
      return `${procent}% mistet`;
    }

    function setRound(runde) {
      currentRound = runde;
      forbudte = runder[runde];
      forbudtEl.textContent = forbudte.length >= 29 ? "alle" : (forbudte.length ? forbudte.join(", ") : "Ingen");
      mistedeContainer.style.display = "block";
      resetBtn.style.display = "inline";
      if (runde < 5) {
        tekstfelt.value = "";
        tekstfelt.focus();
        tekstfelt.style.display = "block";
        sporgsmaalBox.style.display = "block";
        sporgsmaalBox.textContent = "Hvad værdsætter du især ved naturen?";
        timerEl.textContent = "";
      } else {
        tekstfelt.style.display = "none";
        sporgsmaalBox.style.display = "none";
        timerEl.textContent = "";
      }
    }

    function resetGame() {
      clearInterval(countdown);
      forbudte = [];
      currentRound = 0;
      forbudtEl.textContent = "";
      tekstfelt.value = "";
      tekstfelt.style.display = "none";
      sporgsmaalBox.style.display = "none";
      mistedeContainer.style.display = "none";
      resetBtn.style.display = "none";
      sporgsmaalBox.textContent = "Hvad værdsætter du især ved naturen?";
      timerEl.textContent = "";
      tidligereSvarListe.innerHTML = "";
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

    function startAuto() {
      setRound(0);
      let sekunder = roundTimes[0];
      timerEl.textContent = `Næste niveau om ${sekunder} sekunder...`;

      countdown = setInterval(() => {
        sekunder--;
        timerEl.textContent = `Næste niveau om ${sekunder} sekunder...`;

        if (sekunder <= 0) {
          if (tekstfelt.style.display !== "none" && tekstfelt.value) {
            const svarDiv = document.createElement("div");
            svarDiv.className = "tidligereSvar";
            svarDiv.textContent = `(${procentMistede(currentRound)}):\n` + tekstfelt.value;
            tidligereSvarListe.prepend(svarDiv);
          }

          currentRound++;
          if (currentRound > 5) {
            clearInterval(countdown);
            return;
          }
          setRound(currentRound);
          if (currentRound < 5) {
            sekunder = roundTimes[currentRound];
            timerEl.textContent = `Næste niveau om ${sekunder} sekunder...`;
          } else {
            clearInterval(countdown);
            forbudte = bogstaverEfterFrekvens.slice();
            forbudtEl.textContent = "alle";
            tekstfelt.value = "";
            tekstfelt.style.display = "none";
            tekstfelt.blur();
            sporgsmaalBox.style.display = "none";
            timerEl.textContent = "";
          }
        }
      }, 1000);
    }
  </script>
</body>
</html>
