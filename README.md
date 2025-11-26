<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Jogo Anemia Falciforme</title>
<style>
  html, body {
    margin: 0;
    padding: 0;
    overflow: hidden;
    height: 100%;
    width: 100%;
  }
  #arteryBackground {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    z-index: -1;
  }
  #gameCanvas {
    position: absolute;
    top: 0;
    left: 0;
    touch-action: none; /* importante para mobile */
  }
</style>
</head>
<body>

<!-- Vídeo de fundo -->
<video id="arteryBackground" autoplay loop muted>
  <source src="video-da-arteria.mp4" type="video/mp4">
</video>

<!-- Canvas do jogo -->
<canvas id="gameCanvas"></canvas>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

// Ajusta o canvas para preencher toda a tela
function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

// Imagens das hemácias
const hemaciasNormais = new Image();
hemaciasNormais.src = 'hemacia-normal.png';

const hemaciasFalciformes = new Image();
hemaciasFalciformes.src = 'hemacia-falciforme.png';

// Posições iniciais (proporcionais à tela)
let hemaciaNormalPos = { x: canvas.width * 0.2, y: canvas.height * 0.3 };
let hemaciaFalciformePos = { x: canvas.width * 0.5, y: canvas.height * 0.3 };

// Função para desenhar o jogo
function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Desenha hemácias normais
    ctx.drawImage(hemaciasNormais, hemaciaNormalPos.x, hemaciaNormalPos.y, 50, 50);

    // Desenha hemácias falciformes
    ctx.drawImage(hemaciasFalciformes, hemaciaFalciformePos.x, hemaciaFalciformePos.y, 50, 50);

    requestAnimationFrame(draw);
}

// Inicia o desenho
draw();

// Controle por toque para arrastar hemácias
let selectedHemacia = null;

canvas.addEventListener('touchstart', (e) => {
    const touch = e.touches[0];
    const tx = touch.clientX;
    const ty = touch.clientY;

    if (tx > hemaciaNormalPos.x && tx < hemaciaNormalPos.x + 50 &&
        ty > hemaciaNormalPos.y && ty < hemaciaNormalPos.y + 50) {
        selectedHemacia = 'normal';
    } else if (tx > hemaciaFalciformePos.x && tx < hemaciaFalciformePos.x + 50 &&
               ty > hemaciaFalciformePos.y && ty < hemaciaFalciformePos.y + 50) {
        selectedHemacia = 'falciforme';
    }
});

canvas.addEventListener('touchmove', (e) => {
    if (!selectedHemacia) return;
    e.preventDefault(); // evita scroll da tela ao arrastar
    const touch = e.touches[0];
    const tx = touch.clientX;
    const ty = touch.clientY;

    if (selectedHemacia === 'normal') {
        hemaciaNormalPos.x = tx - 25;
        hemaciaNormalPos.y = ty - 25;
    } else if (selectedHemacia === 'falciforme') {
        hemaciaFalciformePos.x = tx - 25;
        hemaciaFalciformePos.y = ty - 25;
    }
});

canvas.addEventListener('touchend', () => {
    selectedHemacia = null;
});
</script>

</body>
</html>
