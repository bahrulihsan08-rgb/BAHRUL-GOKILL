# BAHRUL-GOKILL
ILOVEEYOUU PUTRIPUSPITASARI
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>Symphony of Love</title>
    <style>
        body, html { margin: 0; padding: 0; width: 100%; height: 100%; overflow: hidden; background: #0a0005; }
        canvas { display: block; cursor: crosshair; }
        #ui {
            position: absolute; bottom: 20px; width: 100%; text-align: center;
            color: rgba(255,100,150,0.5); font-family: 'Segoe UI', sans-serif;
            letter-spacing: 2px; pointer-events: none; text-transform: uppercase; font-size: 10px;
        }
    </style>
</head>
<body>

<div id="ui">Klik untuk Mode Fullscreen • Gerakkan Mouse untuk Interaksi</div>
<canvas id="canvas"></canvas>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

let width, height, hearts = [], particles = [];
let mouse = { x: -1000, y: -1000 };

// Inisialisasi Dimensi
function init() {
    width = canvas.width = window.innerWidth;
    height = canvas.height = window.innerHeight;
}

// Objek Hati Kecil (Latar Belakang yang Ramai)
class HeartParticle {
    constructor() {
        this.reset();
    }
    reset() {
        this.x = Math.random() * width;
        this.y = height + Math.random() * 100;
        this.size = Math.random() * 15 + 5;
        this.speed = Math.random() * 1 + 0.5;
        this.opacity = Math.random() * 0.5 + 0.2;
        this.velX = Math.random() * 1 - 0.5;
    }
    draw() {
        ctx.fillStyle = `rgba(255, 50, 100, ${this.opacity})`;
        ctx.beginPath();
        const topCurveHeight = this.size * 0.3;
        ctx.moveTo(this.x, this.y + topCurveHeight);
        ctx.bezierCurveTo(this.x, this.y, this.x - this.size / 2, this.y, this.x - this.size / 2, this.y + topCurveHeight);
        ctx.bezierCurveTo(this.x - this.size / 2, this.y + (this.size + topCurveHeight) / 2, this.x, this.y + (this.size + topCurveHeight) / 2, this.x, this.y + this.size);
        ctx.bezierCurveTo(this.x, this.y + (this.size + topCurveHeight) / 2, this.x + this.size / 2, this.y + (this.size + topCurveHeight) / 2, this.x + this.size / 2, this.y + topCurveHeight);
        ctx.bezierCurveTo(this.x + this.size / 2, this.y, this.x, this.y, this.x, this.y + topCurveHeight);
        ctx.fill();
        
        this.y -= this.speed;
        this.x += this.velX;
        if (this.y < -50) this.reset();
    }
}

// Fungsi Menggambar Hati Pusat (Besar)
function drawMainHeart(t) {
    const scale = 15 + Math.sin(t * 0.05) * 2; // Efek Denyut
    const centerX = width / 2;
    const centerY = height / 2;
    
    ctx.shadowBlur = 40;
    ctx.shadowColor = "#ff2d75";
    ctx.fillStyle = "#ff2d75";
    
    ctx.beginPath();
    for (let i = 0; i < Math.PI * 2; i += 0.01) {
        // Persamaan Parametrik Hati
        const x = 16 * Math.pow(Math.sin(i), 3);
        const y = -(13 * Math.cos(i) - 5 * Math.cos(2 * i) - 2 * Math.cos(3 * i) - Math.cos(4 * i));
        ctx.lineTo(centerX + x * scale, centerY + y * scale);
    }
    ctx.fill();
    ctx.shadowBlur = 0;
}

// Sparkle mengikuti mouse
function createSparkle() {
    if (mouse.x > 0) {
        particles.push({
            x: mouse.x, y: mouse.y,
            size: Math.random() * 4,
            vx: (Math.random() - 0.5) * 2,
            vy: (Math.random() - 0.5) * 2,
            life: 1
        });
    }
}

function updateSparkles() {
    for (let i = particles.length - 1; i >= 0; i--) {
        const p = particles[i];
        p.x += p.vx; p.y += p.vy;
        p.life -= 0.02;
        if (p.life <= 0) particles.splice(i, 1);
        else {
            ctx.fillStyle = `rgba(255, 255, 255, ${p.life})`;
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
            ctx.fill();
        }
    }
}

// Setup awal hati latar belakang
for(let i=0; i<60; i++) hearts.push(new HeartParticle());

let frame = 0;
function animate() {
    // Efek trail (sedikit menghapus layar untuk kesan elegan)
    ctx.fillStyle = 'rgba(10, 0, 5, 0.15)';
    ctx.fillRect(0, 0, width, height);

    hearts.forEach(h => h.draw());
    createSparkle();
    updateSparkles();
    drawMainHeart(frame);

    frame++;
    requestAnimationFrame(animate);
}

// Event Listeners
window.addEventListener('resize', init);
window.addEventListener('mousemove', e => {
    mouse.x = e.clientX;
    mouse.y = e.clientY;
});
canvas.addEventListener('click', () => {
    if (!document.fullscreenElement) document.documentElement.requestFullscreen();
    else document.exitFullscreen();
});

init();
animate();
</script>
</body>
</html>
