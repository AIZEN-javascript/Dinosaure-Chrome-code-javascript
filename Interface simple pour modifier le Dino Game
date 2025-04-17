// === Interface simple pour modifier le Dino Game ===
// Auteur : AIZEN-javascript

(function () {
  const container = document.createElement("div");
  container.innerHTML = `
    <style>
      #dino-tools {
        position: fixed;
        top: 20px;
        left: 20px;
        background-color: rgba(30, 30, 30, 0.95);
        border: 1px solid #0f0;
        padding: 16px;
        border-radius: 8px;
        width: 260px;
        font-family: monospace;
        color: #ccc;
        z-index: 9999;
      }
      #dino-tools h3 {
        margin: 0 0 12px;
        color: #0f0;
        font-size: 18px;
        text-align: center;
      }
      .toggle-line {
        display: flex;
        justify-content: space-between;
        margin-bottom: 8px;
        padding: 6px 8px;
        border-radius: 4px;
        background-color: #222;
        border: 1px solid #0f0;
        cursor: pointer;
        transition: all 0.3s;
      }
      .toggle-line.active {
        background-color: #0f0;
        color: #000;
        font-weight: bold;
      }
      .toggle-line span {
        font-weight: normal;
      }
      #color-picker-container {
        display: flex;
        align-items: center;
        gap: 8px;
        margin-top: 10px;
      }
      #color-picker-container input {
        flex-grow: 1;
        height: 30px;
        border: none;
        cursor: pointer;
        padding: 0;
      }
      .btn-close {
        background-color: #a00;
        color: #fff;
        border: none;
        padding: 6px 8px;
        width: 100%;
        border-radius: 4px;
        cursor: pointer;
        margin-top: 10px;
      }
    </style>
    <div id="dino-tools">
      <h3>Dino Tools 🦖</h3>
      <div class="toggle-line" onclick="toggleImmortality(this)">Immortalité <span>OFF</span></div>
      <div class="toggle-line" onclick="toggleSpeed(this)">Turbo Vitesse <span>OFF</span></div>
      <div class="toggle-line" onclick="toggleJumpHeight(this)">Saut Haut <span>OFF</span></div>
      <div class="toggle-line" onclick="toggleFlight(this)">Marche dans l'air <span>OFF</span></div>
      <div class="toggle-line" onclick="toggleScoreBoost(this)">Boost Score <span>OFF</span></div>
      <div class="toggle-line" onclick="toggleAutoPlay(this)">Auto Play <span>OFF</span></div>
      <div id="color-picker-container">
        <label for="bgcolor">🎨 Fond :</label>
        <input type="color" id="bgcolor" value="#1e1e1e" />
      </div>
      <button class="btn-close" onclick="document.getElementById('dino-tools').remove()">Fermer</button>
    </div>
  `;
  document.body.appendChild(container);

  // On garde la fonction originale au cas où
  window.__originalGameOver = Runner.prototype.gameOver;

  let autoPlayTimer = null;

  window.toggleImmortality = function (el) {
    const active = el.classList.toggle("active");
    el.querySelector("span").textContent = active ? "ON" : "OFF";
    Runner.prototype.gameOver = active ? function () {} : __originalGameOver;
  };

  window.toggleSpeed = function (el) {
    const active = el.classList.toggle("active");
    el.querySelector("span").textContent = active ? "ON" : "OFF";
    Runner.instance_.setSpeed(active ? 1000 : 10);
  };

  window.toggleJumpHeight = function (el) {
    const active = el.classList.toggle("active");
    el.querySelector("span").textContent = active ? "ON" : "OFF";
    Runner.instance_.tRex.setJumpVelocity(active ? 20 : 10);
  };

  window.toggleFlight = function (el) {
    const active = el.classList.toggle("active");
    el.querySelector("span").textContent = active ? "ON" : "OFF";
    Runner.instance_.tRex.groundYPos = active ? 0 : 93;
  };

  window.toggleScoreBoost = function (el) {
    const active = el.classList.toggle("active");
    el.querySelector("span").textContent = active ? "ON" : "OFF";
    if (active) {
      Runner.instance_.distanceRan = 12345 / Runner.instance_.distanceMeter.config.COEFFICIENT;
    } else {
      Runner.instance_.distanceRan = 0;
    }
  };

  window.toggleAutoPlay = function (el) {
    const active = el.classList.toggle("active");
    el.querySelector("span").textContent = active ? "ON" : "OFF";
    if (active) {
      autoPlayTimer = setInterval(() => {
        const key = (type, code) =>
          document.dispatchEvent(new KeyboardEvent(type, { keyCode: code }));
        const obs = Runner.instance_.horizon.obstacles[0];
        const speed = Runner.instance_.currentSpeed;
        const canvasH = Runner.instance_.dimensions.HEIGHT;
        const dinoH = Runner.instance_.tRex.config.HEIGHT;
        if (obs) {
          const x = obs.xPos, y = obs.yPos;
          const yFromBottom = canvasH - y - obs.typeConfig.height;
          const isNear = x < 25 * speed - obs.width / 2;
          if (isNear) {
            if (yFromBottom > dinoH) return;
            if (y > canvasH / 2) {
              key("keyup", 40);
              key("keydown", 32);
            } else {
              key("keydown", 40);
            }
          }
        }
      }, Runner.instance_.msPerFrame);
    } else {
      clearInterval(autoPlayTimer);
    }
  };

  // 🎨 Changement de fond en live avec input color
  document.getElementById('bgcolor').addEventListener('input', function (e) {
    document.body.style.backgroundColor = e.target.value;
  });
})();
