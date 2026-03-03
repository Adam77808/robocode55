<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🤖 Jeux Robot - PARTIE 2 (Niveau Avancé)</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Rajdhani', sans-serif; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); min-height: 100vh; overflow-x: hidden; position: relative; }
        #starfield { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; }
        .star { position: absolute; width: 2px; height: 2px; background: white; border-radius: 50%; animation: twinkle 3s infinite; }
        @keyframes twinkle { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
        .container { position: relative; z-index: 1; max-width: 1200px; margin: 0 auto; padding: 40px 20px; }
        .header { text-align: center; margin-bottom: 50px; animation: slideDown 0.8s ease-out; }
        @keyframes slideDown { from { opacity: 0; transform: translateY(-50px); } to { opacity: 1; transform: translateY(0); } }
        .title { font-family: 'Orbitron', sans-serif; font-size: 4em; font-weight: 900; background: linear-gradient(45deg, #00f5ff, #ff00ff, #00f5ff); background-size: 200% 200%; -webkit-background-clip: text; -webkit-text-fill-color: transparent; animation: gradientShift 3s ease infinite; text-shadow: 0 0 30px rgba(0, 245, 255, 0.5); margin-bottom: 10px; }
        @keyframes gradientShift { 0%, 100% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } }
        .subtitle { color: white; font-size: 1.8em; font-weight: 600; text-shadow: 0 0 20px rgba(255, 0, 255, 0.8); margin-bottom: 15px; }
        .level-badge { display: inline-block; background: linear-gradient(135deg, #ff6b6b, #ff0055); color: white; padding: 10px 30px; border-radius: 30px; font-weight: 700; font-size: 1.2em; box-shadow: 0 0 30px rgba(255, 0, 85, 0.6); animation: pulse 2s ease-in-out infinite; }
        @keyframes pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }
        .games-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 30px; margin-bottom: 50px; }
        .game-card { background: rgba(255, 255, 255, 0.1); backdrop-filter: blur(10px); border-radius: 20px; padding: 30px; border: 2px solid rgba(255, 255, 255, 0.2); transition: all 0.3s ease; cursor: pointer; position: relative; overflow: hidden; }
        .game-card:hover { transform: translateY(-10px) scale(1.02); border-color: #00f5ff; box-shadow: 0 20px 60px rgba(0, 245, 255, 0.4); }
        .game-icon { font-size: 4em; margin-bottom: 15px; filter: drop-shadow(0 0 20px rgba(0, 245, 255, 0.6)); }
        .game-title { font-family: 'Orbitron', sans-serif; font-size: 1.8em; color: #00f5ff; margin-bottom: 10px; font-weight: 700; }
        .game-desc { color: rgba(255, 255, 255, 0.9); font-size: 1.1em; line-height: 1.6; margin-bottom: 15px; }
        .difficulty { display: inline-block; background: linear-gradient(135deg, #ff6b6b, #ff0055); color: white; padding: 5px 15px; border-radius: 15px; font-size: 0.9em; font-weight: 600; margin-top: 10px; }
        .modal { display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.8); backdrop-filter: blur(5px); }
        .modal-content { background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%); margin: 3% auto; padding: 40px; border: 3px solid #00f5ff; border-radius: 20px; width: 90%; max-width: 900px; max-height: 85vh; overflow-y: auto; box-shadow: 0 0 50px rgba(0, 245, 255, 0.5); position: relative; }
        .close-btn { position: absolute; right: 20px; top: 20px; font-size: 35px; font-weight: bold; color: #00f5ff; cursor: pointer; background: none; border: none; transition: all 0.3s; }
        .close-btn:hover { color: #ff00ff; transform: rotate(90deg); }
        .modal-content h2 { font-family: 'Orbitron', sans-serif; color: #00f5ff; font-size: 2.2em; margin-bottom: 20px; }
        .play-btn { background: linear-gradient(135deg, #00f5ff, #00a8cc); color: white; border: none; padding: 15px 40px; font-size: 1.3em; font-weight: 700; border-radius: 30px; cursor: pointer; transition: all 0.3s; font-family: 'Orbitron', sans-serif; box-shadow: 0 10px 30px rgba(0, 245, 255, 0.4); margin-top: 20px; }
        .play-btn:hover { transform: scale(1.05); }
        .variable-display { background: rgba(255, 0, 255, 0.1); border: 2px solid #ff00ff; border-radius: 10px; padding: 15px; margin: 15px 0; color: white; font-size: 1.1em; }
        .btn-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; margin: 20px 0; }
        .action-btn { background: rgba(0, 245, 255, 0.2); border: 2px solid #00f5ff; color: white; padding: 15px; border-radius: 10px; cursor: pointer; font-size: 1.1em; font-weight: 600; transition: all 0.3s; font-family: 'Rajdhani', sans-serif; }
        .action-btn:hover { background: rgba(0, 245, 255, 0.4); transform: scale(1.05); }
        input[type="text"], input[type="number"], input[type="range"], textarea { background: rgba(0,0,0,0.3); border: 2px solid #00f5ff; border-radius: 10px; padding: 12px; color: white; font-size: 1.1em; width: 100%; margin: 10px 0; }
        .robot-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 10px; margin: 20px 0; }
        .robot-cell { aspect-ratio: 1; background: rgba(255,255,255,0.1); border: 2px solid rgba(255,255,255,0.3); border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 2em; cursor: pointer; transition: all 0.3s; }
        .robot-cell.active { background: rgba(0,245,255,0.5); border-color: #00f5ff; box-shadow: 0 0 20px rgba(0,245,255,0.6); }
    </style>
</head>
<body>
    <div id="starfield"></div>
    <div class="container">
        <div class="header">
            <h1 class="title">🤖 JEUX ROBOT</h1>
            <p class="subtitle">Partie 2 - Niveau Avancé</p>
            <div class="level-badge">⚡ EXPERT ⚡</div>
        </div>
        <div class="games-grid">
            <div class="game-card" onclick="openGame('game1')">
                <div class="game-icon">💾</div>
                <h3 class="game-title">JEU 1 : VARIABLES</h3>
                <p class="game-desc">Le robot doit stocker et modifier des valeurs en mémoire</p>
                <span class="difficulty">⭐⭐⭐ Avancé</span>
            </div>
            <div class="game-card" onclick="openGame('game2')">
                <div class="game-icon">🔁</div>
                <h3 class="game-title">JEU 2 : BOUCLES</h3>
                <p class="game-desc">Programme des répétitions automatiques avec FOR et WHILE</p>
                <span class="difficulty">⭐⭐⭐⭐ Expert</span>
            </div>
            <div class="game-card" onclick="openGame('game3')">
                <div class="game-icon">⚙️</div>
                <h3 class="game-title">JEU 3 : FONCTIONS</h3>
                <p class="game-desc">Crée tes propres commandes réutilisables</p>
                <span class="difficulty">⭐⭐⭐⭐ Expert</span>
            </div>
            <div class="game-card" onclick="openGame('game4')">
                <div class="game-icon">🐛</div>
                <h3 class="game-title">JEU 4 : DEBUG EXPERT</h3>
                <p class="game-desc">Trouve et corrige des bugs dans du vrai code</p>
                <span class="difficulty">⭐⭐⭐⭐⭐ Maître</span>
            </div>
            <div class="game-card" onclick="openGame('game5')">
                <div class="game-icon">🎮</div>
                <h3 class="game-title">JEU 5 : PROJET FINAL</h3>
                <p class="game-desc">Code ton propre jeu de A à Z</p>
                <span class="difficulty">⭐⭐⭐⭐⭐ Maître</span>
            </div>
        </div>
    </div>

    <!-- Modals -->
    <div id="game1" class="modal">
        <div class="modal-content">
            <button class="close-btn" onclick="closeGame('game1')">×</button>
            <h2>🚀 JEU 1 : MISSION SPATIALE</h2>
            <p style="color:white;font-size:1.1em;">Gère tes ressources pour atteindre la planète Zeta-9 !</p>
            <div id="missionGame" style="margin-top:30px;">
                <div style="display:flex;justify-content:space-between;margin-bottom:20px;">
                    <div><h3 style="color:#00f5ff;">⏰ Temps</h3><div id="missionTimer" style="font-size:3em;color:#ffaa00;font-family:'Orbitron';">60</div></div>
                    <div><h3 style="color:#00f5ff;">📍 Distance</h3><div id="missionDistance" style="font-size:3em;color:#00ff88;font-family:'Orbitron';">0/100</div></div>
                </div>
                <h3 style="color:#00f5ff;font-size:1.5em;">📊 Ressources</h3>
                <div class="variable-display">
                    <div>⚡ <strong>energie:</strong> <span id="varEnergie">100</span>/100</div>
                    <div>🛢️ <strong>carburant:</strong> <span id="varCarburant">50</span>/100</div>
                    <div>💨 <strong>oxygene:</strong> <span id="varOxygene">100</span>/100</div>
                    <div>💰 <strong>credits:</strong> <span id="varCredits">50</span></div>
                </div>
                <h3 style="color:#00f5ff;font-size:1.5em;margin-top:30px;">🎮 Actions</h3>
                <div class="btn-grid">
                    <button class="action-btn" onclick="missionAction('avancer')">🚀 Avancer (-10 carbu)</button>
                    <button class="action-btn" onclick="missionAction('scanner')">🔍 Scanner (-5 ener)</button>
                    <button class="action-btn" onclick="missionAction('collecter')">💎 Collecter (+20 cr)</button>
                    <button class="action-btn" onclick="missionAction('recharger')">🔋 Recharger (-30 cr)</button>
                    <button class="action-btn" onclick="missionAction('boost')">⚡ Turbo x2</button>
                    <button class="action-btn" onclick="missionAction('repos')">😴 Repos (+O2)</button>
                </div>
                <div id="missionFeedback" style="margin-top:20px;font-size:1.2em;font-weight:bold;min-height:30px;"></div>
                <button class="play-btn" onclick="startMission()" id="startMissionBtn">▶️ DÉMARRER</button>
            </div>
            <div id="missionWin" style="display:none;text-align:center;margin-top:30px;">
                <div style="font-size:5em;">🎉</div>
                <h2 style="color:#00ff88;font-size:2.5em;margin:20px 0;">MISSION RÉUSSIE !</h2>
                <p style="color:#00f5ff;">Temps : <span id="finalTime"></span>s</p>
                <div id="missionRank" style="font-size:1.8em;margin:20px 0;"></div>
                <button class="play-btn" onclick="restartMission()">🔄 REJOUER</button>
            </div>
            <div id="missionLose" style="display:none;text-align:center;margin-top:30px;">
                <div style="font-size:5em;">💥</div>
                <h2 style="color:#ff0055;font-size:2.5em;margin:20px 0;">MISSION ÉCHOUÉE</h2>
                <p id="loseReason" style="color:white;font-size:1.3em;"></p>
                <button class="play-btn" onclick="restartMission()">🔄 RÉESSAYER</button>
            </div>
        </div>
    </div>

    <div id="game2" class="modal">
        <div class="modal-content">
            <button class="close-btn" onclick="closeGame('game2')">×</button>
            <h2>🔁 JEU 2 : ROBOT RÉPÉTEUR</h2>
            <p style="color:white;font-size:1.1em;">Fais répéter des actions au robot !</p>
            <div id="loopGameArea" style="margin-top:30px;">
                <div style="display:flex;justify-content:space-between;margin-bottom:20px;">
                    <div><h3 style="color:#00f5ff;">Niveau <span id="loopLevel">1</span>/6</h3></div>
                    <div><h3 style="color:#00f5ff;">Score : <span id="loopScore">0</span></h3></div>
                </div>
                <div style="background:rgba(0,245,255,0.1);padding:20px;border-radius:10px;margin-bottom:20px;border:2px solid #00f5ff;">
                    <h3 style="color:#00f5ff;">🎯 Mission :</h3>
                    <p id="loopMission" style="color:white;font-size:1.3em;margin-top:10px;"></p>
                </div>
                <h3 style="color:#00f5ff;">Choisis une action :</h3>
                <div class="btn-grid" style="margin-bottom:30px;">
                    <button class="action-btn" onclick="selectLoopAction('sauter')"><div style="font-size:2em;">⬆️</div><div>SAUTER</div></button>
                    <button class="action-btn" onclick="selectLoopAction('tourner')"><div style="font-size:2em;">🔄</div><div>TOURNER</div></button>
                    <button class="action-btn" onclick="selectLoopAction('collecter')"><div style="font-size:2em;">💎</div><div>COLLECTER</div></button>
                    <button class="action-btn" onclick="selectLoopAction('danser')"><div style="font-size:2em;">💃</div><div>DANSER</div></button>
                </div>
                <div id="loopActionSelected" style="display:none;">
                    <h3 style="color:#00f5ff;">Combien de fois ?</h3>
                    <div style="margin:20px 0;">
                        <input type="range" id="loopCount" min="1" max="20" value="1" oninput="updateLoopCount()">
                        <div style="text-align:center;margin-top:10px;">
                            <span id="loopCountDisplay" style="font-size:4em;color:#00f5ff;font-family:'Orbitron';">1</span>
                            <span style="font-size:2em;color:white;"> fois</span>
                        </div>
                    </div>
                    <button class="play-btn" onclick="executeLoop()">▶️ LANCER</button>
                </div>
                <div id="loopAnimation" style="margin-top:30px;text-align:center;min-height:100px;">
                    <div id="loopRobot" style="font-size:5em;">🤖</div>
                    <div id="loopCounter" style="font-size:2em;color:#00f5ff;margin-top:10px;"></div>
                </div>
                <div id="loopResult" style="margin-top:20px;font-size:1.3em;font-weight:bold;text-align:center;"></div>
            </div>
            <div id="loopGameWin" style="display:none;text-align:center;">
                <div style="font-size:5em;">🎉</div>
                <h2 style="color:#00ff88;font-size:2.5em;margin:20px 0;">EXPERT EN BOUCLES !</h2>
                <p style="color:white;font-size:1.3em;">Score : <span id="loopFinalScore" style="color:#00f5ff;font-size:1.5em;"></span></p>
                <div id="loopRank" style="font-size:1.8em;margin:20px 0;"></div>
                <button class="play-btn" onclick="restartLoopGame()">🔄 RECOMMENCER</button>
            </div>
        </div>
    </div>

    <div id="game3" class="modal">
        <div class="modal-content">
            <button class="close-btn" onclick="closeGame('game3')">×</button>
            <h2>⚙️ JEU 3 : CRÉATEUR DE COMBOS</h2>
            <p style="color:white;font-size:1.1em;">Crée des suites d'actions réutilisables !</p>
            <div id="funcGameArea" style="margin-top:30px;">
                <div style="display:flex;justify-content:space-between;margin-bottom:20px;">
                    <div><h3 style="color:#00f5ff;">Niveau <span id="funcLevel">1</span>/5</h3></div>
                    <div><h3 style="color:#00f5ff;">Score : <span id="funcScore">0</span></h3></div>
                </div>
                <div style="background:rgba(0,245,255,0.1);padding:20px;border-radius:10px;margin-bottom:20px;border:2px solid #00f5ff;">
                    <h3 style="color:#00f5ff;">🎯 Mission :</h3>
                    <p id="funcMissionText" style="color:white;font-size:1.3em;margin-top:10px;"></p>
                </div>
                <h3 style="color:#00f5ff;">📦 Actions :</h3>
                <div class="btn-grid" style="margin-bottom:30px;">
                    <button class="action-btn" onclick="addFuncAction('⬆️','AVANCER')"><div style="font-size:2em;">⬆️</div><div>AVANCER</div></button>
                    <button class="action-btn" onclick="addFuncAction('🔄','TOURNER')"><div style="font-size:2em;">🔄</div><div>TOURNER</div></button>
                    <button class="action-btn" onclick="addFuncAction('💎','COLLECTER')"><div style="font-size:2em;">💎</div><div>COLLECTER</div></button>
                    <button class="action-btn" onclick="addFuncAction('🎵','CHANTER')"><div style="font-size:2em;">🎵</div><div>CHANTER</div></button>
                </div>
                <h3 style="color:#00f5ff;">✨ Mon COMBO :</h3>
                <div id="funcCombo" style="min-height:100px;background:rgba(0,0,0,0.3);border:3px dashed #00f5ff;border-radius:10px;padding:20px;display:flex;flex-wrap:wrap;gap:10px;align-items:center;">
                    <span style="color:rgba(255,255,255,0.3);font-size:1.2em;">Clique sur les actions...</span>
                </div>
                <div style="margin-top:20px;display:flex;gap:15px;">
                    <button class="play-btn" onclick="testFuncCombo()" style="flex:1;">🧪 TESTER</button>
                    <button class="action-btn" onclick="clearFuncCombo()" style="flex:0.3;background:rgba(255,0,85,0.2);border-color:#ff0055;">🗑️</button>
                </div>
                <div style="margin-top:30px;text-align:center;"><div id="funcRobot" style="font-size:5em;">🤖</div></div>
                <div id="funcResult" style="margin-top:20px;font-size:1.3em;font-weight:bold;text-align:center;"></div>
            </div>
            <div id="funcGameWin" style="display:none;text-align:center;">
                <div style="font-size:5em;">🎉</div>
                <h2 style="color:#00ff88;font-size:2.5em;margin:20px 0;">EXPERT EN COMBOS !</h2>
                <p style="color:white;font-size:1.3em;">Score : <span id="funcFinalScore" style="color:#00f5ff;font-size:1.5em;"></span></p>
                <div id="funcRank" style="font-size:1.8em;margin:20px 0;"></div>
                <button class="play-btn" onclick="restartFuncGame()">🔄 RECOMMENCER</button>
            </div>
        </div>
    </div>

    <div id="game4" class="modal">
        <div class="modal-content">
            <button class="close-btn" onclick="closeGame('game4')">×</button>
            <h2>🐛 JEU 4 : DÉTECTIVE ROBOT</h2>
            <p style="color:white;font-size:1.1em;">Trouve l'erreur dans la séquence !</p>
            <div id="debugGameArea" style="margin-top:30px;">
                <div style="display:flex;justify-content:space-between;margin-bottom:20px;">
                    <div><h3 style="color:#00f5ff;">Niveau <span id="debugLevelNum">1</span>/6</h3></div>
                    <div><h3 style="color:#00f5ff;">Score : <span id="debugScore">0</span></h3></div>
                </div>
                <div style="background:rgba(0,245,255,0.1);padding:20px;border-radius:10px;margin-bottom:20px;border:2px solid #00f5ff;">
                    <h3 style="color:#00f5ff;">🎯 Le robot devait faire :</h3>
                    <p id="debugGoal" style="color:white;font-size:1.3em;margin-top:10px;"></p>
                </div>
                <h3 style="color:#ff0055;">❌ Mais il a fait ça (trouve l'erreur !) :</h3>
                <div id="debugSequence" style="display:flex;flex-wrap:wrap;gap:15px;padding:20px;background:rgba(255,0,85,0.1);border:3px solid #ff0055;border-radius:10px;min-height:100px;margin:20px 0;"></div>
                <div id="debugResult" style="margin-top:20px;font-size:1.3em;font-weight:bold;text-align:center;"></div>
            </div>
            <div id="debugGameWin" style="display:none;text-align:center;">
                <div style="font-size:5em;">🎉</div>
                <h2 style="color:#00ff88;font-size:2.5em;margin:20px 0;">DÉTECTIVE EXPERT !</h2>
                <p style="color:white;font-size:1.3em;">Score : <span id="debugFinalScore" style="color:#00f5ff;font-size:1.5em;"></span></p>
                <div id="debugRank" style="font-size:1.8em;margin:20px 0;"></div>
                <button class="play-btn" onclick="restartDebugGame()">🔄 RECOMMENCER</button>
            </div>
        </div>
    </div>

    <div id="game5" class="modal">
        <div class="modal-content">
            <button class="close-btn" onclick="closeGame('game5')">×</button>
            <h2>🎮 JEU 5 : PROJET FINAL</h2>
            <p style="color:white;font-size:1.1em;">Code ton propre mini-jeu !</p>
            <div style="margin-top:30px;">
                <h3 style="color:#00f5ff;font-size:1.5em;">🎨 Choisis ton projet</h3>
                <div class="btn-grid">
                    <button class="action-btn" onclick="selectProject('memory')">🧠 Jeu de Mémoire</button>
                    <button class="action-btn" onclick="selectProject('clicker')">🎵 Simon Says</button>
                    <button class="action-btn" onclick="selectProject('quiz')">🎓 Quiz Codage</button>
                </div>
                <div id="projectArea" style="margin-top:30px;display:none;">
                    <h3 style="color:#00f5ff;">Grille de jeu</h3>
                    <div id="gameGrid" class="robot-grid"></div>
                    <div style="margin-top:20px;"><p style="color:white;font-size:1.2em;">Score : <span id="projectScore" style="color:#00f5ff;font-size:1.5em;">0</span></p></div>
                    <button class="play-btn" onclick="resetProject()" style="margin-top:20px;">🔄 RECOMMENCER</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const starfield = document.getElementById('starfield');
        for (let i = 0; i < 100; i++) {
            const star = document.createElement('div');
            star.className = 'star';
            star.style.left = Math.random() * 100 + '%';
            star.style.top = Math.random() * 100 + '%';
            star.style.animationDelay = Math.random() * 3 + 's';
            starfield.appendChild(star);
        }

        function openGame(gameId) {
            document.getElementById(gameId).style.display = 'block';
            if (gameId === 'game2') loadLoopLevel();
            if (gameId === 'game3') loadFuncMission();
            if (gameId === 'game4') loadDebugLevel();
        }
        function closeGame(gameId) { document.getElementById(gameId).style.display = 'none'; }
        window.onclick = function(e) { if (e.target.classList.contains('modal')) e.target.style.display = 'none'; }

        let mission = { active: false, timer: null, timeLeft: 60, distance: 0, targetDistance: 100, vars: { energie: 100, carburant: 50, oxygene: 100, credits: 50 } };

        function startMission() {
            mission.active = true; mission.timeLeft = 60; mission.distance = 0;
            mission.vars = { energie: 100, carburant: 50, oxygene: 100, credits: 50 };
            document.getElementById('missionGame').style.display = 'block';
            document.getElementById('missionWin').style.display = 'none';
            document.getElementById('missionLose').style.display = 'none';
            document.getElementById('startMissionBtn').style.display = 'none';
            updateMissionDisplay();
            mission.timer = setInterval(() => {
                mission.timeLeft--;
                mission.vars.oxygene = Math.max(0, mission.vars.oxygene - 1);
                updateMissionDisplay();
                if (mission.vars.oxygene <= 0) endMission(false, "Plus d'oxygène ! 💨");
                else if (mission.timeLeft <= 0) endMission(false, "Temps écoulé ! ⏰");
            }, 1000);
        }

        function missionAction(action) {
            if (!mission.active) return;
            const feedback = document.getElementById('missionFeedback');
            let message = '';
            switch(action) {
                case 'avancer':
                    if (mission.vars.carburant >= 10) { mission.vars.carburant -= 10; mission.distance += 10; message = '<span style="color:#00ff88;">🚀 +10 distance</span>'; if (mission.distance >= 100) endMission(true); }
                    else message = '<span style="color:#ff0055;">❌ Pas assez de carburant !</span>'; break;
                case 'scanner':
                    if (mission.vars.energie >= 5) { mission.vars.energie -= 5; mission.vars.credits += 10; message = '<span style="color:#00f5ff;">🔍 +10 crédits</span>'; }
                    else message = '<span style="color:#ff0055;">❌ Pas assez d'énergie !</span>'; break;
                case 'collecter':
                    if (mission.vars.energie >= 3) { mission.vars.energie -= 3; mission.vars.credits += 20; message = '<span style="color:#FFD700;">💎 +20 crédits</span>'; }
                    else message = '<span style="color:#ff0055;">❌ Pas assez d'énergie !</span>'; break;
                case 'recharger':
                    if (mission.vars.credits >= 30) { mission.vars.credits -= 30; mission.vars.energie = Math.min(100, mission.vars.energie + 50); mission.vars.carburant = Math.min(100, mission.vars.carburant + 30); message = '<span style="color:#00ff88;">🔋 Rechargé !</span>'; }
                    else message = '<span style="color:#ff0055;">❌ Pas assez de crédits !</span>'; break;
                case 'boost':
                    if (mission.vars.energie >= 20 && mission.vars.carburant >= 15) { mission.vars.energie -= 20; mission.vars.carburant -= 15; mission.distance += 20; message = '<span style="color:#ff00ff;">⚡ TURBO ! +20 distance</span>'; if (mission.distance >= 100) endMission(true); }
                    else message = '<span style="color:#ff0055;">❌ Ressources insuffisantes !</span>'; break;
                case 'repos':
                    mission.vars.oxygene = Math.min(100, mission.vars.oxygene + 20); message = '<span style="color:#00f5ff;">😴 +20 oxygène</span>'; break;
            }
            feedback.innerHTML = message;
            setTimeout(() => { feedback.innerHTML = ''; }, 2000);
            updateMissionDisplay();
            if (mission.vars.energie <= 0) endMission(false, "Plus d'énergie ! ⚡");
        }

        function updateMissionDisplay() {
            document.getElementById('missionTimer').textContent = mission.timeLeft;
            document.getElementById('missionDistance').textContent = mission.distance + '/100';
            document.getElementById('varEnergie').textContent = mission.vars.energie;
            document.getElementById('varCarburant').textContent = mission.vars.carburant;
            document.getElementById('varOxygene').textContent = mission.vars.oxygene;
            document.getElementById('varCredits').textContent = mission.vars.credits;
            document.getElementById('missionTimer').style.color = mission.timeLeft <= 10 ? '#ff0055' : '#ffaa00';
        }

        function endMission(won, reason = '') {
            mission.active = false; clearInterval(mission.timer);
            document.getElementById('missionGame').style.display = 'none';
            if (won) {
                document.getElementById('missionWin').style.display = 'block';
                document.getElementById('finalTime').textContent = 60 - mission.timeLeft;
                const t = 60 - mission.timeLeft;
                const rank = t <= 20 ? '🏆 EXPLORATEUR LÉGENDAIRE 🏆' : t <= 35 ? '⭐ PILOTE EXPERT ⭐' : t <= 50 ? '🎖️ VOYAGEUR COMPÉTENT 🎖️' : '💪 SURVIVANT 💪';
                document.getElementById('missionRank').innerHTML = '<span style="color:#00f5ff;">' + rank + '</span>';
            } else {
                document.getElementById('missionLose').style.display = 'block';
                document.getElementById('loseReason').textContent = reason;
            }
        }

        function restartMission() {
            document.getElementById('startMissionBtn').style.display = 'block';
            document.getElementById('missionWin').style.display = 'none';
            document.getElementById('missionLose').style.display = 'none';
            document.getElementById('missionGame').style.display = 'block';
            mission.vars = { energie: 100, carburant: 50, oxygene: 100, credits: 50 };
            mission.distance = 0; mission.timeLeft = 60; updateMissionDisplay();
        }

        const loopMissions = [
            { action: 'sauter', count: 3, desc: 'Fais SAUTER le robot 3 fois' },
            { action: 'tourner', count: 5, desc: 'Fais TOURNER le robot 5 fois' },
            { action: 'collecter', count: 7, desc: 'Fais COLLECTER 7 objets au robot' },
            { action: 'danser', count: 4, desc: 'Fais DANSER le robot 4 fois' },
            { action: 'sauter', count: 10, desc: 'Fais SAUTER le robot 10 fois' },
            { action: 'collecter', count: 15, desc: 'Fais COLLECTER 15 objets au robot' }
        ];
        let loopGame = { currentLevel: 0, score: 0, selectedAction: null };

        function loadLoopLevel() {
            if (loopGame.currentLevel >= loopMissions.length) { showLoopWin(); return; }
            document.getElementById('loopGameArea').style.display = 'block';
            document.getElementById('loopGameWin').style.display = 'none';
            document.getElementById('loopActionSelected').style.display = 'none';
            document.getElementById('loopLevel').textContent = loopGame.currentLevel + 1;
            document.getElementById('loopScore').textContent = loopGame.score;
            document.getElementById('loopMission').textContent = loopMissions[loopGame.currentLevel].desc;
            document.getElementById('loopResult').innerHTML = '';
            document.getElementById('loopCounter').innerHTML = '';
            loopGame.selectedAction = null;
        }

        function selectLoopAction(action) { loopGame.selectedAction = action; document.getElementById('loopActionSelected').style.display = 'block'; document.getElementById('loopCount').value = 1; document.getElementById('loopCountDisplay').textContent = 1; }
        function updateLoopCount() { document.getElementById('loopCountDisplay').textContent = document.getElementById('loopCount').value; }

        async function executeLoop() {
            if (!loopGame.selectedAction) return;
            const count = parseInt(document.getElementById('loopCount').value);
            const lm = loopMissions[loopGame.currentLevel];
            const robot = document.getElementById('loopRobot');
            const counter = document.getElementById('loopCounter');
            document.getElementById('loopResult').innerHTML = '';
            for (let i = 1; i <= count; i++) {
                counter.textContent = i + ' / ' + count;
                switch(loopGame.selectedAction) {
                    case 'sauter': robot.style.transform = 'translateY(-50px) scale(1.2)'; break;
                    case 'tourner': robot.style.transform = 'rotate(' + (i*90) + 'deg)'; break;
                    case 'collecter': robot.textContent = i%2===0?'🤖':'💎'; robot.style.transform = 'scale(1.3)'; break;
                    case 'danser': robot.textContent = i%2===0?'🕺':'💃'; robot.style.transform = 'rotate(' + (i%2===0?-15:15) + 'deg) scale(1.2)'; break;
                }
                await sleep(400); robot.style.transform = 'scale(1) rotate(0deg)'; robot.textContent = '🤖'; await sleep(200);
            }
            counter.textContent = '';
            if (loopGame.selectedAction === lm.action && count === lm.count) {
                document.getElementById('loopResult').innerHTML = '<span style="color:#00ff88;">✅ PARFAIT ! +100 points</span>';
                loopGame.score += 100; document.getElementById('loopScore').textContent = loopGame.score;
                setTimeout(() => { loopGame.currentLevel++; loadLoopLevel(); }, 2000);
            } else {
                let fb = '<span style="color:#ff0055;">❌ Pas tout à fait...</span><br>';
                fb += loopGame.selectedAction !== lm.action ? '<span style="color:white;">Mauvaise action !</span>' : '<span style="color:white;">Il faut exactement ' + lm.count + ' fois !</span>';
                document.getElementById('loopResult').innerHTML = fb;
            }
        }

        function showLoopWin() {
            document.getElementById('loopGameArea').style.display = 'none';
            document.getElementById('loopGameWin').style.display = 'block';
            document.getElementById('loopFinalScore').textContent = loopGame.score;
            const rank = loopGame.score >= 600 ? '🏆 MAÎTRE DES BOUCLES 🏆' : loopGame.score >= 500 ? '⭐ EXPERT EN RÉPÉTITIONS ⭐' : '🎖️ CHAMPION DES BOUCLES 🎖️';
            document.getElementById('loopRank').innerHTML = '<span style="color:#00f5ff;">' + rank + '</span>';
        }
        function restartLoopGame() { loopGame.currentLevel = 0; loopGame.score = 0; loadLoopLevel(); }
        function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

        const funcMissionsKids = [
            { combo: ['⬆️','⬆️','💎'], desc: 'Crée un combo : AVANCER, AVANCER, COLLECTER' },
            { combo: ['🔄','⬆️','🔄','⬆️'], desc: 'Crée : TOURNER, AVANCER, TOURNER, AVANCER' },
            { combo: ['💎','💎','💎','🎵'], desc: 'Crée : COLLECTER 3 fois puis CHANTER' },
            { combo: ['⬆️','🔄','⬆️','🔄','💎'], desc: 'Crée : AVANCER, TOURNER, AVANCER, TOURNER, COLLECTER' },
            { combo: ['🎵','⬆️','⬆️','🔄','💎','🎵'], desc: 'DÉFI FINAL : CHANTER, AVANCER 2x, TOURNER, COLLECTER, CHANTER' }
        ];
        let funcGame = { currentLevel: 0, score: 0, combo: [] };

        function loadFuncMission() {
            if (funcGame.currentLevel >= funcMissionsKids.length) { showFuncWin(); return; }
            document.getElementById('funcGameArea').style.display = 'block';
            document.getElementById('funcGameWin').style.display = 'none';
            document.getElementById('funcLevel').textContent = funcGame.currentLevel + 1;
            document.getElementById('funcScore').textContent = funcGame.score;
            document.getElementById('funcMissionText').textContent = funcMissionsKids[funcGame.currentLevel].desc;
            document.getElementById('funcResult').innerHTML = '';
            funcGame.combo = []; updateFuncComboDisplay();
        }
        function addFuncAction(emoji, name) { funcGame.combo.push(emoji); updateFuncComboDisplay(); }
        function updateFuncComboDisplay() {
            const d = document.getElementById('funcCombo');
            if (funcGame.combo.length === 0) { d.innerHTML = '<span style="color:rgba(255,255,255,0.3);font-size:1.2em;">Clique sur les actions...</span>'; return; }
            d.innerHTML = '';
            funcGame.combo.forEach(e => { const b = document.createElement('div'); b.style.cssText = 'font-size:3em;background:rgba(0,245,255,0.2);padding:10px 20px;border-radius:10px;border:2px solid #00f5ff;'; b.textContent = e; d.appendChild(b); });
        }
        function clearFuncCombo() { funcGame.combo = []; updateFuncComboDisplay(); }

        async function testFuncCombo() {
            if (funcGame.combo.length === 0) { document.getElementById('funcResult').innerHTML = '<span style="color:#ff0055;">❌ Ajoute des actions d'abord !</span>'; return; }
            const robot = document.getElementById('funcRobot');
            document.getElementById('funcResult').innerHTML = '<span style="color:#00f5ff;">🤖 Exécution...</span>';
            for (let i = 0; i < funcGame.combo.length; i++) {
                const a = funcGame.combo[i];
                if (a === '⬆️') robot.style.transform = 'translateY(-30px)';
                else if (a === '🔄') robot.style.transform = 'rotate(' + ((i+1)*90) + 'deg)';
                else { robot.textContent = a; robot.style.transform = 'scale(1.3)'; }
                await sleep(500); robot.style.transform = 'scale(1) rotate(0deg)'; robot.textContent = '🤖'; await sleep(300);
            }
            const fm = funcMissionsKids[funcGame.currentLevel];
            if (JSON.stringify(funcGame.combo) === JSON.stringify(fm.combo)) {
                document.getElementById('funcResult').innerHTML = '<span style="color:#00ff88;">✅ COMBO PARFAIT ! +100 points</span>';
                funcGame.score += 100; document.getElementById('funcScore').textContent = funcGame.score;
                setTimeout(() => { funcGame.currentLevel++; loadFuncMission(); }, 2000);
            } else {
                document.getElementById('funcResult').innerHTML = '<span style="color:#ff0055;">❌ Pas le bon combo !</span>';
            }
        }
        function showFuncWin() {
            document.getElementById('funcGameArea').style.display = 'none';
            document.getElementById('funcGameWin').style.display = 'block';
            document.getElementById('funcFinalScore').textContent = funcGame.score;
            const rank = funcGame.score >= 500 ? '🏆 MAÎTRE DES COMBOS 🏆' : funcGame.score >= 400 ? '⭐ EXPERT EN COMBOS ⭐' : '🎖️ CRÉATEUR DE COMBOS 🎖️';
            document.getElementById('funcRank').innerHTML = '<span style="color:#00f5ff;">' + rank + '</span>';
        }
        function restartFuncGame() { funcGame.currentLevel = 0; funcGame.score = 0; funcGame.combo = []; loadFuncMission(); }

        const debugLevelsKids = [
            { goal: "AVANCER, AVANCER, COLLECTER", sequence: ['⬆️','⬆️','🔄','💎'], errorIndex: 2 },
            { goal: "TOURNER, COLLECTER, TOURNER, COLLECTER", sequence: ['🔄','💎','💎','🔄','💎'], errorIndex: 2 },
            { goal: "CHANTER, AVANCER, AVANCER, CHANTER", sequence: ['🎵','⬆️','💎','⬆️','🎵'], errorIndex: 2 },
            { goal: "COLLECTER 3 fois puis CHANTER", sequence: ['💎','💎','⬆️','💎','🎵'], errorIndex: 2 },
            { goal: "AVANCER, TOURNER, AVANCER, TOURNER, COLLECTER", sequence: ['⬆️','🔄','⬆️','⬆️','🔄','💎'], errorIndex: 3 },
            { goal: "CHANTER, AVANCER 2 fois, COLLECTER, CHANTER", sequence: ['🎵','⬆️','⬆️','🔄','💎','🎵'], errorIndex: 3 }
        ];
        let debugGame = { currentLevel: 0, score: 0 };

        function loadDebugLevel() {
            if (debugGame.currentLevel >= debugLevelsKids.length) { showDebugWin(); return; }
            document.getElementById('debugGameArea').style.display = 'block';
            document.getElementById('debugGameWin').style.display = 'none';
            const level = debugLevelsKids[debugGame.currentLevel];
            document.getElementById('debugLevelNum').textContent = debugGame.currentLevel + 1;
            document.getElementById('debugScore').textContent = debugGame.score;
            document.getElementById('debugGoal').textContent = level.goal;
            document.getElementById('debugResult').innerHTML = '';
            const sd = document.getElementById('debugSequence');
            sd.innerHTML = '';
            level.sequence.forEach((emoji, index) => {
                const b = document.createElement('div');
                b.style.cssText = 'font-size:3em;background:rgba(255,255,255,0.1);padding:15px 25px;border-radius:10px;border:3px solid rgba(255,255,255,0.3);cursor:pointer;transition:all 0.3s;';
                b.textContent = emoji;
                b.onclick = () => checkDebugAnswer(index);
                b.onmouseover = () => { b.style.background = 'rgba(0,245,255,0.3)'; b.style.borderColor = '#00f5ff'; b.style.transform = 'scale(1.1)'; };
                b.onmouseout = () => { b.style.background = 'rgba(255,255,255,0.1)'; b.style.borderColor = 'rgba(255,255,255,0.3)'; b.style.transform = 'scale(1)'; };
                sd.appendChild(b);
            });
        }

        function checkDebugAnswer(index) {
            const level = debugLevelsKids[debugGame.currentLevel];
            if (index === level.errorIndex) {
                document.getElementById('debugResult').innerHTML = '<span style="color:#00ff88;">✅ BRAVO ! +100 points</span>';
                debugGame.score += 100; document.getElementById('debugScore').textContent = debugGame.score;
                document.getElementById('debugSequence').children[index].style.background = 'rgba(0,255,136,0.3)';
                document.getElementById('debugSequence').children[index].style.borderColor = '#00ff88';
                setTimeout(() => { debugGame.currentLevel++; loadDebugLevel(); }, 2000);
            } else {
                document.getElementById('debugResult').innerHTML = '<span style="color:#ff0055;">❌ Pas l'erreur ! Réessaie...</span>';
                setTimeout(() => { document.getElementById('debugResult').innerHTML = ''; }, 1500);
            }
        }

        function showDebugWin() {
            document.getElementById('debugGameArea').style.display = 'none';
            document.getElementById('debugGameWin').style.display = 'block';
            document.getElementById('debugFinalScore').textContent = debugGame.score;
            const rank = debugGame.score >= 600 ? '🏆 DÉTECTIVE LÉGENDAIRE 🏆' : debugGame.score >= 500 ? '⭐ CHASSEUR DE BUGS EXPERT ⭐' : '🎖️ DÉTECTIVE DES ERREURS 🎖️';
            document.getElementById('debugRank').innerHTML = '<span style="color:#00f5ff;">' + rank + '</span>';
        }
        function restartDebugGame() { debugGame.currentLevel = 0; debugGame.score = 0; loadDebugLevel(); }

        let currentProject = null, projectScore = 0, memoryCards = [], flippedCards = [], matchedPairs = 0;

        function selectProject(type) {
            currentProject = type; document.getElementById('projectArea').style.display = 'block';
            projectScore = 0; matchedPairs = 0; document.getElementById('projectScore').textContent = 0;
            if (type === 'memory') initMemoryGame();
            else if (type === 'clicker') initClickerGame();
            else if (type === 'quiz') initQuizGame();
        }

        function initMemoryGame() {
            const emojis = ['🤖','🚀','⚡','💎','🎮','🌟','🔧','💻'];
            const pairs = [...emojis,...emojis].sort(() => Math.random() - 0.5);
            memoryCards = pairs; flippedCards = []; matchedPairs = 0;
            const grid = document.getElementById('gameGrid');
            grid.style.gridTemplateColumns = 'repeat(4,1fr)'; grid.innerHTML = '';
            pairs.forEach((emoji, i) => {
                const cell = document.createElement('div'); cell.className = 'robot-cell';
                cell.textContent = '❓'; cell.dataset.emoji = emoji; cell.dataset.matched = 'false';
                cell.onclick = () => flipCard(cell); grid.appendChild(cell);
            });
        }

        function flipCard(cell) {
            if (flippedCards.length >= 2 || cell.classList.contains('active') || cell.dataset.matched === 'true') return;
            cell.textContent = cell.dataset.emoji; cell.classList.add('active'); flippedCards.push(cell);
            if (flippedCards.length === 2) setTimeout(checkMatch, 800);
        }

        function checkMatch() {
            const [c1,c2] = flippedCards;
            if (c1.dataset.emoji === c2.dataset.emoji) {
                c1.dataset.matched = 'true'; c2.dataset.matched = 'true'; projectScore += 10; matchedPairs++;
                document.getElementById('projectScore').textContent = projectScore;
                if (matchedPairs === 8) setTimeout(() => alert('🎉 BRAVO ! Score : ' + projectScore), 500);
            } else { c1.textContent = '❓'; c2.textContent = '❓'; c1.classList.remove('active'); c2.classList.remove('active'); }
            flippedCards = [];
        }

        function initClickerGame() {
            const grid = document.getElementById('gameGrid');
            grid.style.gridTemplateColumns = 'repeat(3,1fr)'; grid.innerHTML = '';
            projectScore = 0; let simonLevel = 1, simonSequence = [], playerSequence = [], isPlayerTurn = false;
            const colors = [{emoji:'🔴',color:'#ff0055'},{emoji:'🔵',color:'#0055ff'},{emoji:'🟢',color:'#00ff55'},{emoji:'🟡',color:'#ffaa00'},{emoji:'🟣',color:'#aa00ff'},{emoji:'🟠',color:'#ff6600'}];
            document.getElementById('projectScore').textContent = 'Niveau 1 | Score: 0';
            for (let i = 0; i < 6; i++) {
                const cell = document.createElement('div'); cell.className = 'robot-cell';
                cell.textContent = colors[i].emoji; cell.dataset.index = i; cell.style.background = 'rgba(255,255,255,0.1)';
                cell.onclick = () => {
                    if (!isPlayerTurn) return;
                    playerSequence.push(i); flashCell(cell, colors[i].color);
                    const ci = playerSequence.length - 1;
                    if (playerSequence[ci] !== simonSequence[ci]) { isPlayerTurn = false; setTimeout(() => alert('❌ ERREUR ! Niveau ' + simonLevel + '\nScore: ' + projectScore), 500); return; }
                    if (playerSequence.length === simonSequence.length) { isPlayerTurn = false; projectScore += simonLevel * 10; simonLevel++; if (simonLevel > 10) { setTimeout(() => alert('🎉 INCROYABLE ! Score: ' + projectScore), 500); return; } setTimeout(playSeq, 1000); }
                };
                grid.appendChild(cell);
            }
            function playSeq() {
                playerSequence = []; simonSequence.push(Math.floor(Math.random()*6));
                document.getElementById('projectScore').textContent = 'Niveau ' + simonLevel + ' | Score: ' + projectScore;
                isPlayerTurn = false; let delay = 0;
                simonSequence.forEach((ci, i) => { setTimeout(() => { flashCell(grid.children[ci], colors[ci].color); if (i === simonSequence.length-1) setTimeout(() => { isPlayerTurn = true; }, 600); }, delay); delay += 800; });
            }
            function flashCell(cell, color) { cell.style.background = color; cell.style.transform = 'scale(1.2)'; setTimeout(() => { cell.style.background = 'rgba(255,255,255,0.1)'; cell.style.transform = 'scale(1)'; }, 400); }
            setTimeout(playSeq, 1000);
        }

        function initQuizGame() {
            const questions = [
                { q: "Que fait une BOUCLE ?", a: ['Répète des actions','Saute en l\'air','Change de couleur'], correct: 'Répète des actions' },
                { q: "Une VARIABLE c'est quoi ?", a: ['Une boîte pour stocker','Un robot','Un jeu'], correct: 'Une boîte pour stocker' },
                { q: "Que fait un COMBO ?", a: ['Suite d\'actions','Danse','Vole'], correct: 'Suite d\'actions' },
                { q: "Pourquoi DEBUGGER ?", a: ['Trouver les erreurs','Faire danser','Peindre'], correct: 'Trouver les erreurs' },
                { q: "Une SÉQUENCE c'est ?", a: ['Actions dans l\'ordre','Un animal','De la musique'], correct: 'Actions dans l\'ordre' }
            ];
            let currentQ = 0; projectScore = 0; document.getElementById('projectScore').textContent = 0;
            function showQ() {
                const grid = document.getElementById('gameGrid'); grid.style.gridTemplateColumns = '1fr'; grid.innerHTML = '';
                if (currentQ >= questions.length) {
                    const r = projectScore >= 100 ? '🏆 EXPERT EN CODAGE 🏆' : projectScore >= 60 ? '⭐ BON ÉLÈVE ⭐' : '💪 CONTINUE À APPRENDRE 💪';
                    const d = document.createElement('div'); d.style.cssText = 'color:white;padding:20px;font-size:1.5em;text-align:center;';
                    d.innerHTML = '🎉 Quiz terminé !<br><br>Score : ' + projectScore + '/100<br><br>' + r; grid.appendChild(d); return;
                }
                const q = questions[currentQ]; const qd = document.createElement('div');
                qd.style.cssText = 'color:white;padding:20px;font-size:1.3em;font-weight:bold;';
                qd.textContent = 'Q' + (currentQ+1) + '/' + questions.length + ' : ' + q.q; grid.appendChild(qd);
                [...q.a].sort(() => Math.random()-0.5).forEach(ans => {
                    const btn = document.createElement('button'); btn.className = 'action-btn';
                    btn.textContent = ans; btn.style.margin = '10px'; btn.style.fontSize = '1.1em';
                    btn.onclick = () => {
                        if (ans === q.correct) { projectScore += 20; document.getElementById('projectScore').textContent = projectScore; btn.style.background = 'rgba(0,255,136,0.4)'; setTimeout(() => { currentQ++; showQ(); }, 1000); }
                        else { btn.style.background = 'rgba(255,0,85,0.4)'; setTimeout(() => { btn.style.background = 'rgba(0,245,255,0.2)'; }, 1000); }
                    };
                    grid.appendChild(btn);
                });
            }
            showQ();
        }
        function resetProject() { selectProject(currentProject); }
    </script>
</body>
</html>
