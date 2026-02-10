<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Sans commentaire</title>

  <style>
    body {
      height: 100vh;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(135deg, #f0dbff, #dfb7ff);
      font-family: Arial, sans-serif;
    }

    /* === CARTE = CADRE DE JEU === */
    .card {
      position: relative;
      background: white;
      padding: 80px;
      width: 420px;
      border-radius: 20px;
      text-align: center;
      box-shadow: 0 10px 30px rgba(0,0,0,0.15);
      overflow: hidden; /* interdit toute fuite hors du cadre */
    }

    h1 {
      color: #6b10d0;
      font-size: 22px;
    }

    p {
      margin: 12px 0;
    }

    /* === ZONE BOUTONS === */
    .buttons {
      margin-top: 40px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 25px;
    }

    button {
      padding: 12px 24px;
      font-size: 16px;
      border-radius: 25px;
      border: none;
      cursor: pointer;
      transition: transform 0.15s ease;
    }

    #yes {
      background: #6b10d0;
      color: white;
      z-index: 2;
    }

    /* === BOUTON NON : ÉTAT NORMAL === */
    #no {
      background: white;
      color: #ff4d88;
      border: 2px dashed #ff4d88;
    }

    /* === ÉTAT FUITE === */
    #no.flee {
      position: absolute;
      z-index: 1;
    }
  </style>
</head>

<body>

  <div class="card" id="container">
    <h1>OK ma belle,</h1>
    <p>J’ai une question primordiale</p>
    <p><strong>Un date ?</strong></p>

    <div class="buttons">
      <button id="yes">Comment résister... 💕</button>
      <button id="no">Nop :D</button>
    </div>
  </div>

<script>
  const noButton = document.getElementById("no");
  const yesButton = document.getElementById("yes");
  const container = document.getElementById("container");

  let escapeCount = 0;
  let scale = 1;
  let fleeing = false;

  function isOverlapping(x, y) {
    const noW = noButton.offsetWidth * scale;
    const noH = noButton.offsetHeight * scale;

    const noRect = {
      left: x,
      right: x + noW,
      top: y,
      bottom: y + noH
    };

    const yesRect = yesButton.getBoundingClientRect();
    const containerRect = container.getBoundingClientRect();

    const yes = {
      left: yesRect.left - containerRect.left,
      right: yesRect.right - containerRect.left,
      top: yesRect.top - containerRect.top,
      bottom: yesRect.bottom - containerRect.top
    };

    return !(
      noRect.right < yes.left ||
      noRect.left > yes.right ||
      noRect.bottom < yes.top ||
      noRect.top > yes.bottom
    );
  }

  function moveButton() {
    if (!fleeing) {
      fleeing = true;
      noButton.classList.add("flee");
    }

    escapeCount++;

    if (escapeCount >= 5) {
      scale = Math.max(0.4, scale - 0.1);
      noButton.style.transform = `scale(${scale})`;
    }

    const maxX = container.clientWidth - noButton.offsetWidth * scale;
    const maxY = container.clientHeight - noButton.offsetHeight * scale;

    let x, y, tries = 0;

    do {
      x = Math.random() * maxX;
      y = Math.random() * maxY;
      tries++;
    } while (isOverlapping(x, y) && tries < 100);

    noButton.style.left = x + "px";
    noButton.style.top = y + "px";
  }

  noButton.addEventListener("mouseenter", moveButton);
  noButton.addEventListener("touchstart", moveButton);

  yesButton.addEventListener("click", () => {
    window.location.href = "evidement.html";
  });

  noButton.addEventListener("click", () => {
    window.location.href = "Certes.html";
  });
</script>

</body>
</html>
