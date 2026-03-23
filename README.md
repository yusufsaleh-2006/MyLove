
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>I Love You</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            height: 100vh;
            overflow: hidden;
            background: linear-gradient(135deg, #1a1a2e, #16213e, #0f3460);
            font-family: 'Segoe UI', sans-serif;
            cursor: pointer;
        }

        canvas {
            position: fixed;
            top: 0;
            left: 0;
        }

        .container {
            position: relative;
            z-index: 10;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .love-text {
            font-size: clamp(3rem, 12vw, 7rem);
            font-weight: bold;
            text-align: center;
            background: linear-gradient(45deg, #ff6b6b, #f9ca24, #6c5ce7);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            50% { transform: translateY(-20px); }
        }

        .subtitle {
            margin-top: 1rem;
            color: white;
            opacity: 0.7;
        }

        .heart {
            position: fixed;
            top: -20px;
            animation: fall linear forwards;
        }

        @keyframes fall {
            to {
                transform: translateY(100vh);
                opacity: 0;
            }
        }

        button {
            position: absolute;
            bottom: 20px;
            padding: 10px 15px;
            border: none;
            border-radius: 20px;
            background: rgba(255,255,255,0.2);
            color: white;
            cursor: pointer;
        }
    </style>
</head>

<body>

<canvas id="canvas"></canvas>

<div class="container">
    <h1 class="love-text" id="text"></h1>
    <p class="subtitle">Forever & Always 💖</p>
</div>

<button id="musicBtn">🎵 Music</button>

<script>
const name = "Your Name";
const message = `I LOVE YOU ${name}`;

const textEl = document.getElementById("text");
let i = 0;

function typeWriter() {
    if (i < message.length) {
        textEl.innerHTML += message.charAt(i);
        i++;
        setTimeout(typeWriter, 150);
    }
}
typeWriter();

const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

canvas.width = innerWidth;
canvas.height = innerHeight;

let particles = [];
const MAX_PARTICLES = 150;

class Particle {
    constructor(x, y, color) {
        this.x = x;
        this.y = y;
        this.size = Math.random() * 4 + 2;
        this.speedX = Math.random() * 4 - 2;
        this.speedY = Math.random() * 4 - 2;
        this.life = 1;
        this.color = color;
    }

    update() {
        this.x += this.speedX;
        this.y += this.speedY;
        this.life -= 0.02;
    }

    draw() {
        ctx.globalAlpha = this.life;
        ctx.fillStyle = this.color;
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fill();
        ctx.globalAlpha = 1;
    }
}

function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    particles.forEach((p, i) => {
        p.update();
        p.draw();
        if (p.life <= 0) particles.splice(i, 1);
    });

    requestAnimationFrame(animate);
}
animate();

const colors = ["#ff6b6b","#f9ca24","#6c5ce7","#fd79a8"];

document.addEventListener("click", (e) => {
    for (let i = 0; i < 25; i++) {
        if (particles.length < MAX_PARTICLES) {
            particles.push(
                new Particle(
                    e.clientX,
                    e.clientY,
                    colors[Math.floor(Math.random() * colors.length)]
                )
            );
        }
    }
});

function createHeart() {
    const heart = document.createElement("div");
    heart.className = "heart";
    heart.innerHTML = "❤️";

    heart.style.left = Math.random() * 100 + "vw";
    heart.style.fontSize = Math.random() * 20 + 10 + "px";
    heart.style.animationDuration = (Math.random() * 3 + 3) + "s";

    document.body.appendChild(heart);

    setTimeout(() => heart.remove(), 6000);
}

const isMobile = innerWidth < 768;
setInterval(createHeart, isMobile ? 600 : 300);

const music = new Audio("https://www.fesliyanstudios.com/play-mp3/387");
let playing = false;

document.getElementById("musicBtn").onclick = () => {
    if (!playing) {
        music.loop = true;
        music.play();
    } else {
        music.pause();
    }
    playing = !playing;
};
</script>

</body>
</html>
