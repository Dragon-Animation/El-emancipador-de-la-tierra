<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>El Emancipador de la Tierra</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Roboto+Mono:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #3dff57;
            --background-color: #0d0c1d;
            --text-color: #ffffff;
            --danger-color: #ff3d3d;
            --accent-color: #2a2a3a;
            --gemini-purple: #8952ff;
            --win-gold: #ffd700;
        }

        body {
            background-color: var(--background-color);
            color: var(--text-color);
            font-family: 'Roboto Mono', monospace;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        .game-container {
            width: 100%;
            max-width: 800px;
            height: 100%;
            max-height: 600px;
            position: relative;
            box-shadow: 0 0 30px 5px var(--primary-color);
            border: 2px solid var(--primary-color);
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        canvas {
            background-color: #111;
            display: block;
            width: 100%;
            height: 100%;
            border-radius: 6px;
        }

        .menu {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(13, 12, 29, 0.95);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10;
            border-radius: 6px;
            text-align: center;
            padding: 20px;
            box-sizing: border-box;
            overflow-y: auto;
        }
        
        .hidden {
            display: none;
        }

        h1, h2 {
            font-family: 'Press Start 2P', cursive;
            color: var(--primary-color);
            text-shadow: 0 0 10px var(--primary-color);
        }
        
        h1 { font-size: 2.5rem; margin-bottom: 2rem; }
        h2 { font-size: 1.8rem; margin-bottom: 1.5rem; }

        .menu-button, .level-button, .weapon-button {
            background-color: transparent;
            border: 2px solid var(--primary-color);
            color: var(--primary-color);
            padding: 15px 30px;
            font-size: 1.2rem;
            font-family: 'Roboto Mono', monospace;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 10px;
            border-radius: 8px;
            min-width: 200px;
            text-transform: uppercase;
        }

        .menu-button:hover, .level-button:hover, .weapon-button:hover {
            background-color: var(--primary-color);
            color: var(--background-color);
            box-shadow: 0 0 20px var(--primary-color);
        }
        .menu-button:disabled { cursor: not-allowed; opacity: 0.5; }
        
        .gemini-button { border-color: var(--gemini-purple); color: var(--gemini-purple); }
        .gemini-button:hover { background-color: var(--gemini-purple); color: var(--text-color); box-shadow: 0 0 20px var(--gemini-purple); }

        #level-select .levels { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; width: 100%; max-width: 600px; }
        .level-card { background: var(--accent-color); padding: 15px; border-radius: 8px; border: 1px solid var(--primary-color); display: flex; flex-direction: column; align-items: center; }
        .level-card .level-button { margin-bottom: 10px; width: 100%; }
        .level-card .gemini-button { font-size: 0.9rem; padding: 8px 12px; min-width: unset; width: 100%;}
        .level-description { font-size: 0.8rem; margin-top: 10px; min-height: 50px; color: #ccc; }
        
        #game-over-screen { padding: 40px; }
        #game-over-screen h2 { color: var(--danger-color); text-shadow: 0 0 10px var(--danger-color); }
        #game-over-screen .menu-button { border-color: var(--danger-color); color: var(--danger-color); }
        #game-over-screen .menu-button:hover { background-color: var(--danger-color); color: var(--text-color); box-shadow: 0 0 20px var(--danger-color); }
        
        #win-screen h2 { color: var(--win-gold); text-shadow: 0 0 10px var(--win-gold); }
        #win-screen .menu-button { border-color: var(--win-gold); color: var(--win-gold); }
        #win-screen .menu-button:hover { background-color: var(--win-gold); color: var(--text-color); box-shadow: 0 0 20px var(--win-gold); }


        .controls-list { text-align: left; margin-bottom: 2rem; font-size: 1.1rem; line-height: 2; background-color: var(--accent-color); padding: 20px; border-radius: 8px; border-left: 5px solid var(--primary-color); }
        .controls-list strong { color: var(--primary-color); margin-right: 10px; }

        /* Tienda */
        #shop-screen { justify-content: flex-start; }
        #total-points { font-size: 1.5rem; color: var(--win-gold); margin-bottom: 20px; font-family: 'Press Start 2P'; }
        .weapon-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; width: 100%; }
        .weapon-card { background-color: var(--accent-color); padding: 20px; border-radius: 8px; border: 1px solid #444; }
        .weapon-card h3 { margin-top: 0; color: var(--primary-color); }
        .weapon-card p { margin: 5px 0; font-size: 0.9rem; }
        .weapon-button { width: 100%; padding: 10px; font-size: 1rem; min-width: unset; }
        .weapon-button.equipped { background-color: var(--primary-color); color: var(--background-color); border-color: var(--primary-color); }

        .creator-credit {
            position: absolute;
            bottom: 20px;
            left: 20px;
            font-size: 0.9rem;
            color: var(--primary-color);
            opacity: 0.7;
        }

        /* Modal y Loader */
        .modal-backdrop { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.7); z-index: 20; display: flex; justify-content: center; align-items: center; }
        .modal-content { background-color: var(--background-color); border: 2px solid var(--gemini-purple); padding: 30px; border-radius: 8px; max-width: 80%; text-align: center; }
        .modal-content h3 { font-family: 'Press Start 2P'; color: var(--gemini-purple); margin-top: 0;}
        .modal-content p { line-height: 1.6; }
        .loading-spinner { border: 8px solid #f3f3f3; border-top: 8px solid var(--gemini-purple); border-radius: 50%; width: 60px; height: 60px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        
        @media (max-width: 600px) {
            h1 { font-size: 1.8rem; }
            h2 { font-size: 1.5rem; }
            #level-select .levels { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <!-- Pantallas de Menú -->
        <div id="main-menu" class="menu">
            <h1>El Emancipador de la Tierra</h1>
            <button id="play-button" class="menu-button">Jugar</button>
            <button id="shop-button" class="menu-button">Tienda</button>
            <button id="controls-button" class="menu-button">Controles</button>
            <p class="creator-credit">creado por:Dragon Animation</p>
        </div>

        <div id="level-select" class="menu hidden">
            <h2>Selecciona Misión</h2>
            <div class="levels">
                <div class="level-card" data-level="1"><button class="level-button">Nivel 1</button><p class="level-description"></p><button class="gemini-button">✨ Generar Misión</button></div>
                <div class="level-card" data-level="2"><button class="level-button">Nivel 2</button><p class="level-description"></p><button class="gemini-button">✨ Generar Misión</button></div>
                <div class="level-card" data-level="3"><button class="level-button">Nivel 3</button><p class="level-description"></p><button class="gemini-button">✨ Generar Misión</button></div>
                <div class="level-card" data-level="4"><button class="level-button">Nivel 4</button><p class="level-description"></p><button class="gemini-button">✨ Generar Misión</button></div>
            </div>
             <button id="back-to-main-menu" class="menu-button">Volver</button>
        </div>
        
        <div id="shop-screen" class="menu hidden">
            <h2>Arsenal</h2>
            <p id="total-points">Puntos: 0</p>
            <div id="weapon-grid" class="weapon-grid"></div>
            <button id="back-from-shop" class="menu-button">Volver</button>
        </div>

        <div id="controls-screen" class="menu hidden">
            <h2>Controles</h2>
            <div class="controls-list">
                <p><strong>Moverse:</strong> Flechas Izquierda/Derecha o Teclas A/D</p>
                <p><strong>Saltar:</strong> Flecha Arriba, Tecla W o Barra Espaciadora</p>
                <p><strong>Disparar:</strong> Clic del Ratón</p>
                <p><strong>Recargar:</strong> Tecla R</p>
            </div>
            <button id="back-button" class="menu-button">Volver</button>
        </div>

        <div id="game-over-screen" class="menu hidden">
            <h2>FIN DEL JUEGO</h2>
            <p id="final-score">Puntuación: 0</p>
            <div style="display: flex;">
                <button id="restart-button" class="menu-button">Volver al Menú</button>
                <button id="analyze-button" class="menu-button gemini-button">✨ Analizar Partida</button>
            </div>
        </div>

        <div id="win-screen" class="menu hidden">
            <h2>¡VICTORIA!</h2>
            <p id="win-score">Puntuación: 0</p>
            <p id="win-points">Puntos Ganados: 0</p>
            <button id="win-continue-button" class="menu-button">Continuar</button>
        </div>

        <!-- Contenedor para Modales -->
        <div id="modal-container"></div>

        <canvas id="gameCanvas"></canvas>
    </div>

    <script>
        // --- Referencias a Elementos del DOM ---
        const canvas = document.getElementById('gameCanvas'); const ctx = canvas.getContext('2d');
        const gameContainer = document.querySelector('.game-container');
        const mainMenu = document.getElementById('main-menu');
        const levelSelect = document.getElementById('level-select');
        const gameOverScreen = document.getElementById('game-over-screen');
        const controlsScreen = document.getElementById('controls-screen');
        const shopScreen = document.getElementById('shop-screen');
        const winScreen = document.getElementById('win-screen');
        const modalContainer = document.getElementById('modal-container');
        const playButton = document.getElementById('play-button');
        const shopButton = document.getElementById('shop-button');
        const levelCards = document.querySelectorAll('.level-card');
        const restartButton = document.getElementById('restart-button');
        const finalScoreEl = document.getElementById('final-score');
        const controlsButton = document.getElementById('controls-button');
        const backButton = document.getElementById('back-button');
        const analyzeButton = document.getElementById('analyze-button');
        const backToMainMenuButton = document.getElementById('back-to-main-menu');
        const backFromShopButton = document.getElementById('back-from-shop');
        const totalPointsEl = document.getElementById('total-points');
        const weaponGridEl = document.getElementById('weapon-grid');
        const winContinueButton = document.getElementById('win-continue-button');
        const winScoreEl = document.getElementById('win-score');
        const winPointsEl = document.getElementById('win-points');

        // --- Estado Global del Juego ---
        let player, bullets, monsters, keys, score, levelConfig, gameLoopId, currentLevel, startTime, currentGameDuration;
        let isGameOver = false;

        // --- Estado Persistente (Puntos y Armas) ---
        let totalPoints = 0;
        let equippedWeapon = 'default';
        const weapons = {
            'default': { name: 'Emancipador Básico', damage: 1, ammo: 15, fireRate: 500, price: 0, owned: true, color: '#ffeb3b' },
            'plasma': { name: 'Rifle de Plasma', damage: 2, ammo: 20, fireRate: 400, price: 500, owned: false, color: '#00ffff' },
            'ion': { name: 'Cañón de Iones', damage: 3, ammo: 10, fireRate: 700, price: 1200, owned: false, color: '#ff00ff' },
            'tachyon': { name: 'Desintegrador Taquiónico', damage: 1.5, ammo: 30, fireRate: 250, price: 2000, owned: false, color: '#ffffff' }
        };

        const LEVEL_SETTINGS = {
            1: { monsterSpeed: 1, spawnRate: 150, monsterHealth: 1, monsterTypes: ['slime'] },
            2: { monsterSpeed: 1.5, spawnRate: 100, monsterHealth: 2, monsterTypes: ['slime', 'brute'] },
            3: { monsterSpeed: 2, spawnRate: 70, monsterHealth: 3, monsterTypes: ['brute', 'flyer'] },
            4: { monsterSpeed: 2.5, spawnRate: 50, monsterHealth: 4, monsterTypes: ['slime', 'brute', 'flyer'] }
        };

        const LEVEL_DURATIONS = {
            1: 1 * 60 * 1000, // 1 minuto
            2: 2 * 60 * 1000, // 2 minutos
            3: 3 * 60 * 1000, // 3 minutos
            4: 4 * 60 * 1000  // 4 minutos
        };

        // --- MANEJO DE MENÚS Y ESTADO ---
        playButton.addEventListener('click', () => { mainMenu.classList.add('hidden'); levelSelect.classList.remove('hidden'); });
        shopButton.addEventListener('click', () => { mainMenu.classList.add('hidden'); setupShop(); shopScreen.classList.remove('hidden'); });
        controlsButton.addEventListener('click', () => { mainMenu.classList.add('hidden'); controlsScreen.classList.remove('hidden'); });
        backButton.addEventListener('click', () => { controlsScreen.classList.add('hidden'); mainMenu.classList.remove('hidden'); });
        backToMainMenuButton.addEventListener('click', () => { levelSelect.classList.add('hidden'); mainMenu.classList.remove('hidden'); });
        backFromShopButton.addEventListener('click', () => { shopScreen.classList.add('hidden'); mainMenu.classList.remove('hidden'); });
        restartButton.addEventListener('click', () => { gameOverScreen.classList.add('hidden'); mainMenu.classList.remove('hidden'); });
        winContinueButton.addEventListener('click', () => { winScreen.classList.add('hidden'); mainMenu.classList.remove('hidden'); });
        
        levelCards.forEach(card => {
            const level = parseInt(card.dataset.level);
            card.querySelector('.level-button').addEventListener('click', () => startGame(level));
            card.querySelector('.gemini-button').addEventListener('click', () => generateMission(card, level));
        });

        analyzeButton.addEventListener('click', () => generateAnalysis());

        function startGame(level) {
            levelSelect.classList.add('hidden'); isGameOver = false; currentLevel = level;
            levelConfig = LEVEL_SETTINGS[level]; 
            startTime = Date.now();
            currentGameDuration = LEVEL_DURATIONS[level];
            
            const weaponStats = weapons[equippedWeapon];
            player = {
                x: 100, y: canvas.height - 60, width: 40, height: 50, speed: 5, dx: 0, dy: 0,
                gravity: 0.5, isJumping: false, health: 100, maxHealth: 100,
                ammo: weaponStats.ammo, maxAmmo: weaponStats.ammo,
                isReloading: false, reloadTime: 1500, facing: 'right', gunColor: '#888',
                canShoot: true
            };
            bullets = []; monsters = []; keys = {}; score = 0;
            if(gameLoopId) cancelAnimationFrame(gameLoopId);
            gameLoop();
        }

        function showGameOver() {
            isGameOver = true; cancelAnimationFrame(gameLoopId);
            finalScoreEl.textContent = `Puntuación: ${score}`;
            gameOverScreen.classList.remove('hidden');
        }
        
        function showWinScreen() {
            isGameOver = true; cancelAnimationFrame(gameLoopId);
            totalPoints += score;
            winScoreEl.textContent = `Puntuación Final: ${score}`;
            winPointsEl.textContent = `Puntos Obtenidos: ${score}`;
            winScreen.classList.remove('hidden');
        }

        // --- LÓGICA DEL JUEGO ---
        function update() {
            if (isGameOver) return;
            const elapsedTime = Date.now() - startTime;
            if (elapsedTime >= currentGameDuration) { showWinScreen(); return; }
            
            // Player logic
            player.x += player.dx;
            if (player.y + player.height < canvas.height || player.dy < 0) {
                 player.dy += player.gravity; player.y += player.dy; player.isJumping = true;
            } else { player.y = canvas.height - player.height; player.dy = 0; player.isJumping = false; }
            if (player.x < 0) player.x = 0;
            if (player.x + player.width > canvas.width) player.x = canvas.width - player.width;
            if (keys['arrowright'] || keys['d']) { player.dx = player.speed; player.facing = 'right'; } 
            else if (keys['arrowleft'] || keys['a']) { player.dx = -player.speed; player.facing = 'left'; } 
            else { player.dx = 0; }
            if (keys['r'] && !player.isReloading && player.ammo < player.maxAmmo) reload();
            
            // Bullet logic
            bullets.forEach((b, i) => { b.x += b.speed; if (b.x > canvas.width || b.x < 0) bullets.splice(i, 1); });
            
            // Monster logic
            if (Math.random() * levelConfig.spawnRate < 1) spawnMonster();
            monsters.forEach((m, i) => {
                m.x -= m.speed;
                if (m.type === 'flyer') m.y += Math.sin(m.x / 50) * 2;
                if (m.x + m.width < 0) monsters.splice(i, 1);
            });
            handleCollisions();
        }

        function draw() {
            if (isGameOver) return;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBackground(); 
            drawPlayer();
            bullets.forEach(b => { ctx.fillStyle = b.color; ctx.fillRect(b.x, b.y, b.width, b.height); });
            drawMonsters(); 
            drawHUD();
        }
        
        function gameLoop() { 
            update(); 
            draw(); 
            gameLoopId = requestAnimationFrame(gameLoop); 
        }

        function drawHUD() {
            ctx.fillStyle = 'white'; ctx.font = '20px "Roboto Mono"'; ctx.textAlign = 'left';
            ctx.fillText(`Puntuación: ${score}`, 20, 30);
            ctx.fillStyle = 'red'; ctx.fillRect(20, 40, 200, 20); ctx.fillStyle = '#3dff57';
            ctx.fillRect(20, 40, (player.health / player.maxHealth) * 200, 20);
            ctx.strokeStyle = 'white'; ctx.strokeRect(20, 40, 200, 20);
            
            const elapsedTime = Date.now() - startTime;
            const remainingTime = Math.max(0, currentGameDuration - elapsedTime);
            const minutes = Math.floor(remainingTime / 60000);
            const seconds = Math.floor((remainingTime % 60000) / 1000).toString().padStart(2, '0');
            ctx.font = '24px "Press Start 2P"';
            ctx.textAlign = 'center';
            ctx.fillStyle = 'white';
            ctx.fillText(`${minutes}:${seconds}`, canvas.width / 2, 40);

            ctx.fillStyle = 'white'; ctx.font = '24px "Roboto Mono"'; ctx.textAlign = 'right';
            if (player.isReloading) { ctx.fillStyle = '#ffc107'; ctx.fillText('RECARGANDO...', canvas.width - 20, 50); } 
            else { ctx.fillText(`Munición: ${player.ammo} / ${player.maxAmmo}`, canvas.width - 20, 50); }
        }

        function shoot() {
            const weaponStats = weapons[equippedWeapon];
            if (player.ammo > 0 && !player.isReloading && player.canShoot) {
                player.ammo--; player.canShoot = false;
                const bulletSpeed = 15; const bulletY = player.y + 20 + 4 - 2.5; 
                const bullet = { 
                    width: 10, height: 5, y: bulletY, 
                    speed: player.facing === 'right' ? bulletSpeed : -bulletSpeed, 
                    x: player.facing === 'right' ? player.x + player.width / 2 + 15 + 20 : player.x + player.width / 2 - 15 - 20 - 10,
                    color: weaponStats.color
                };
                bullets.push(bullet);
                setTimeout(() => { player.canShoot = true; }, weaponStats.fireRate);
            }
        }
        
        function handleCollisions() {
            const weaponDamage = weapons[equippedWeapon].damage;
            bullets.forEach((bullet, bIndex) => {
                monsters.forEach((monster, mIndex) => {
                    if (
                        bullet.x < monster.x + monster.width &&
                        bullet.x + bullet.width > monster.x &&
                        bullet.y < monster.y + monster.height &&
                        bullet.y + bullet.height > monster.y
                    ) {
                        bullets.splice(bIndex, 1);
                        monster.health -= weaponDamage;
                        if (monster.health <= 0) {
                            monsters.splice(mIndex, 1);
                            score += 10;
                        }
                    }
                });
            });
            
            monsters.forEach((monster, mIndex) => {
                if (
                    player.x < monster.x + monster.width &&
                    player.x + player.width > monster.x &&
                    player.y < monster.y + monster.height &&
                    player.y + player.height > monster.y
                ) {
                   player.health -= 1;
                   if (player.health <= 0) {
                       showGameOver();
                   }
                }
            });
        }
        
        // --- LÓGICA DE LA TIENDA ---
        function setupShop() {
            weaponGridEl.innerHTML = '';
            totalPointsEl.textContent = `Puntos: ${totalPoints}`;

            for (const id in weapons) {
                if (id === 'default') continue;
                const weapon = weapons[id];
                const card = document.createElement('div');
                card.className = 'weapon-card';
                card.innerHTML = `
                    <h3>${weapon.name}</h3>
                    <p>Daño: ${weapon.damage}</p>
                    <p>Munición: ${weapon.ammo}</p>
                    <p>Cadencia: ${weapon.fireRate}ms</p>
                    <p><strong>Precio: ${weapon.price} Puntos</strong></p>
                    <button class="weapon-button" data-id="${id}"></button>
                `;
                weaponGridEl.appendChild(card);
            }
            updateShopButtons();
        }

        function updateShopButtons() {
            document.querySelectorAll('.weapon-button').forEach(button => {
                const id = button.dataset.id;
                const weapon = weapons[id];
                if (weapon.owned) {
                    if (equippedWeapon === id) {
                        button.textContent = 'Equipada';
                        button.classList.add('equipped');
                        button.disabled = true;
                    } else {
                        button.textContent = 'Equipar';
                        button.classList.remove('equipped');
                        button.disabled = false;
                        button.onclick = () => equipWeapon(id);
                    }
                } else {
                    button.textContent = 'Comprar';
                    button.classList.remove('equipped');
                    button.disabled = totalPoints < weapon.price;
                    button.onclick = () => buyWeapon(id);
                }
            });
        }
        
        function buyWeapon(id) {
            const weapon = weapons[id];
            if (totalPoints >= weapon.price && !weapon.owned) {
                totalPoints -= weapon.price;
                weapon.owned = true;
                totalPointsEl.textContent = `Puntos: ${totalPoints}`;
                updateShopButtons();
            }
        }

        function equipWeapon(id) {
            if (weapons[id].owned) {
                equippedWeapon = id;
                updateShopButtons();
            }
        }

        // --- FUNCIONES DE DIBUJO DETALLADAS ---
        function drawBackground() {
            const sky = ctx.createLinearGradient(0, 0, 0, canvas.height); sky.addColorStop(0, '#87CEEB'); sky.addColorStop(1, '#ADD8E6');
            ctx.fillStyle = sky; ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#FFD700'; ctx.beginPath(); ctx.arc(canvas.width - 100, 100, 40, 0, Math.PI * 2); ctx.fill();
            ctx.fillStyle = '#a9a9a9'; ctx.beginPath(); ctx.moveTo(0, canvas.height - 100); ctx.lineTo(150, canvas.height - 200); ctx.lineTo(300, canvas.height - 120); ctx.lineTo(500, canvas.height - 250); ctx.lineTo(700, canvas.height - 150); ctx.lineTo(canvas.width, canvas.height - 100); ctx.closePath(); ctx.fill();
            ctx.fillStyle = '#6b8e23'; ctx.beginPath(); ctx.moveTo(0, canvas.height - 50); ctx.bezierCurveTo(200, canvas.height - 150, 400, canvas.height - 150, canvas.width, canvas.height - 50); ctx.closePath(); ctx.fill();
            ctx.fillStyle = '#8B4513'; ctx.fillRect(0, canvas.height - 20, canvas.width, 20);
            ctx.fillStyle = '#A0522D'; for (let i = 0; i < canvas.width; i += 20) { ctx.fillRect(i, canvas.height - 20, 10, 20); }
        }
        function drawPlayer() {
            const headSize = 15, bodyWidth = 25, bodyHeight = 25, gunWidth = 20, gunHeight = 8, armLength = 15, gunY = player.y + 20;
            ctx.fillStyle = '#006400'; ctx.fillRect(player.x + (player.width - bodyWidth)/2, player.y + headSize, bodyWidth, bodyHeight);
            ctx.fillStyle = '#F0D2B6'; ctx.fillRect(player.x + (player.width - headSize)/2, player.y, headSize, headSize);
            ctx.fillStyle = '#F0D2B6';
            if (player.facing === 'right') {
                ctx.fillRect(player.x + player.width / 2, gunY, armLength, 6);
                ctx.fillStyle = player.gunColor; ctx.fillRect(player.x + player.width / 2 + armLength, gunY, gunWidth, gunHeight);
            } else {
                ctx.fillRect(player.x + player.width / 2 - armLength, gunY, armLength, 6);
                ctx.fillStyle = player.gunColor; ctx.fillRect(player.x + player.width / 2 - armLength - gunWidth, gunY, gunWidth, gunHeight);
            }
        }
        function drawMonsters() {
             monsters.forEach(m => {
                switch(m.type) {
                    case 'slime': ctx.fillStyle = m.color; ctx.beginPath(); ctx.arc(m.x + m.width / 2, m.y + m.height, m.width / 2, Math.PI, 2 * Math.PI); ctx.fill(); ctx.fillStyle = 'white'; ctx.beginPath(); ctx.arc(m.x + m.width * 0.3, m.y + m.height * 0.5, 3, 0, Math.PI * 2); ctx.fill(); ctx.beginPath(); ctx.arc(m.x + m.width * 0.7, m.y + m.height * 0.5, 3, 0, Math.PI * 2); ctx.fill(); break;
                    case 'brute': ctx.fillStyle = m.color; ctx.fillRect(m.x, m.y, m.width, m.height); ctx.fillStyle = '#a52525'; ctx.fillRect(m.x + m.width*0.2, m.y - m.height*0.2, m.width*0.6, m.height*0.2); ctx.fillStyle = 'yellow'; ctx.fillRect(m.x + m.width*0.4, m.y - m.height*0.15, 5, 5); break;
                    case 'flyer': ctx.fillStyle = m.color; ctx.beginPath(); ctx.arc(m.x + m.width/2, m.y + m.height/2, m.width/2, 0, Math.PI * 2); ctx.fill(); const wingY = m.y + m.height/2 + Math.sin(m.x / 10) * 5; ctx.fillStyle = '#ADD8E6'; ctx.beginPath(); ctx.moveTo(m.x + m.width/2, m.y + m.height/2); ctx.lineTo(m.x - 10, wingY); ctx.lineTo(m.x + m.width/2, wingY-10); ctx.fill(); ctx.beginPath(); ctx.moveTo(m.x + m.width/2, m.y + m.height/2); ctx.lineTo(m.x + m.width + 10, wingY); ctx.lineTo(m.x + m.width/2, wingY-10); ctx.fill(); ctx.fillStyle = 'red'; ctx.beginPath(); ctx.arc(m.x + m.width * 0.2, m.y + m.height/2, 4, 0, Math.PI * 2); ctx.fill(); break;
                    default: ctx.fillStyle = m.color; ctx.fillRect(m.x, m.y, m.width, m.height);
                }
                if (m.health < m.maxHealth) {
                    ctx.fillStyle = 'red'; ctx.fillRect(m.x, m.y - 10, m.width, 5);
                    ctx.fillStyle = 'green'; ctx.fillRect(m.x, m.y - 10, m.width * (m.health / m.maxHealth), 5);
                }
            });
        }
        function spawnMonster() {
            const size = Math.random() * 20 + 20; const monsterType = levelConfig.monsterTypes[Math.floor(Math.random() * levelConfig.monsterTypes.length)];
            let monster = { x: canvas.width, y: canvas.height - size - 20, width: size, height: size, speed: levelConfig.monsterSpeed + (Math.random() - 0.5), health: levelConfig.monsterHealth, maxHealth: levelConfig.monsterHealth, type: monsterType, color: '#ff3d3d' };
            switch(monsterType) {
                case 'slime': monster.color = '#7e57c2'; monster.height /= 2; monster.y = canvas.height - monster.height - 20; break;
                case 'brute': monster.color = '#d32f2f'; monster.width *= 1.5; monster.height *= 1.5; monster.y = canvas.height - monster.height - 20; monster.health *= 1.5; monster.maxHealth *= 1.5; break;
                case 'flyer': monster.color = '#00acc1'; monster.y = Math.random() * (canvas.height / 2); break;
            } monsters.push(monster);
        }
        function reload() {
            player.isReloading = true; player.gunColor = '#ffc107';
            setTimeout(() => { player.ammo = player.maxAmmo; player.isReloading = false; player.gunColor = '#888'; }, player.reloadTime);
        }

        // --- INTEGRACIÓN CON GEMINI API ---
        async function callGemini(prompt, button) {
            if (button) button.disabled = true;
            showLoader();

            const apiKey = ""; 
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
            const payload = { contents: [{ parts: [{ text: prompt }] }] };

            let text = "Error al contactar con la IA. Inténtalo de nuevo.";
            try {
                const response = await fetch(apiUrl, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) });
                if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
                const result = await response.json();
                text = result.candidates?.[0]?.content?.parts?.[0]?.text || "No se pudo generar una respuesta.";
            } catch (error) {
                console.error("Gemini API call failed:", error);
            } finally {
                hideLoader();
                if (button) button.disabled = false;
                return text.replace(/\*/g, ''); 
            }
        }
        async function generateMission(card, level) {
            const descriptionEl = card.querySelector('.level-description');
            const button = card.querySelector('.gemini-button');
            const monsterTypesText = LEVEL_SETTINGS[level].monsterTypes.join(', ');
            const prompt = `Eres un comandante de una fuerza de élite. Genera un nombre de misión en una línea, y debajo una descripción breve (1-2 frases) para el Nivel ${level} del juego "El Emancipador de la Tierra". Los monstruos en este nivel son: ${monsterTypesText}. El tono debe ser épico y de ciencia ficción.`;
            
            descriptionEl.textContent = "Generando informe de misión...";
            const missionText = await callGemini(prompt, button);
            descriptionEl.innerHTML = missionText.replace('\n', '<br><strong>');
        }
        async function generateAnalysis() {
            const prompt = `Eres un sargento instructor cínico pero motivador. Un jugador acaba de perder en el Nivel ${currentLevel} del juego "El Emancipador de la Tierra" con una puntuación final de ${score}. Dale un breve comentario sarcástico sobre su derrota y un consejo útil (2-3 frases en total).`;
            
            const analysisText = await callGemini(prompt, analyzeButton);
            showModal("Análisis Táctico", analysisText);
        }

        // --- FUNCIONES DE UI (MODAL Y LOADER) ---
        function showLoader() { modalContainer.innerHTML = `<div class="modal-backdrop"><div class="loading-spinner"></div></div>`; }
        function hideLoader() { modalContainer.innerHTML = ''; }
        function showModal(title, content) {
            modalContainer.innerHTML = `
                <div class="modal-backdrop">
                    <div class="modal-content">
                        <h3>${title}</h3>
                        <p>${content}</p>
                        <button class="menu-button">Cerrar</button>
                    </div>
                </div>`;
            modalContainer.querySelector('button').addEventListener('click', hideModal);
        }
        function hideModal() { modalContainer.innerHTML = ''; }
        
        // --- EVENT LISTENERS Y SETUP---
        window.addEventListener('keydown', (e) => {
            keys[e.key.toLowerCase()] = true;
            if ((e.key === 'ArrowUp' || e.key.toLowerCase() === 'w' || e.key === ' ') && !player.isJumping) {
                player.dy = -12; player.isJumping = true;
            }
        });
        window.addEventListener('keyup', (e) => { keys[e.key.toLowerCase()] = false; });
        canvas.addEventListener('click', shoot);
        function resizeCanvas() {
            const { width, height } = gameContainer.getBoundingClientRect();
            canvas.width = width; canvas.height = height;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();
    </script>
</body>
</html>
