// Echoes Beyond Flesh - Mobile-Playable Web Game // HTML5 + JavaScript with touch support

const canvas = document.createElement('canvas'); const ctx = canvas.getContext('2d'); canvas.width = window.innerWidth; canvas.height = window.innerHeight; document.body.appendChild(canvas);

const player = { x: 100, y: 300, width: 32, height: 32, speed: 4 }; let rescuees = [ { x: 300, y: 200, rescued: false }, { x: 500, y: 350, rescued: false }, { x: 700, y: 150, rescued: false } ];

let touchInput = { x: null, y: null }; let rescuedCount = 0; let gameOver = false;

// Handle touch input canvas.addEventListener('touchstart', e => { touchInput.x = e.touches[0].clientX; touchInput.y = e.touches[0].clientY; });

canvas.addEventListener('touchmove', e => { const dx = e.touches[0].clientX - touchInput.x; const dy = e.touches[0].clientY - touchInput.y; player.x += dx * 0.05; player.y += dy * 0.05; touchInput.x = e.touches[0].clientX; touchInput.y = e.touches[0].clientY; e.preventDefault(); }, { passive: false });

canvas.addEventListener('touchend', () => { touchInput.x = null; touchInput.y = null; });

function update() { rescuees.forEach(res => { if (!res.rescued && Math.abs(player.x - res.x) < 32 && Math.abs(player.y - res.y) < 32) { res.rescued = true; rescuedCount++; } });

if (rescuedCount === rescuees.length && !gameOver) { gameOver = true; setTimeout(() => { alert("You found the hidden truth: Myra's origin unlocks the sequel!"); window.location.href = "https://echoesbeyondflesh.com/sequel"; // Placeholder sequel link }, 1000); } }

function draw() { ctx.fillStyle = '#000'; ctx.fillRect(0, 0, canvas.width, canvas.height);

ctx.fillStyle = '#0ff'; ctx.fillRect(player.x, player.y, player.width, player.height);

rescuees.forEach(res => { if (!res.rescued) { ctx.fillStyle = '#f0f'; ctx.fillRect(res.x, res.y, 20, 20); } });

ctx.fillStyle = '#fff'; ctx.font = '20px Arial'; ctx.fillText(Rescued: ${rescuedCount}/${rescuees.length}, 10, 30); }

function gameLoop() { update(); draw(); requestAnimationFrame(gameLoop); }

gameLoop();

