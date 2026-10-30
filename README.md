[pikachu_volleyball (2).html](https://github.com/user-attachments/files/34166225/pikachu_volleyball.2.html)
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>피카츄 배구 (Pikachu Volleyball)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Pixel Font -->
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Noto+Sans+KR:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans KR', sans-serif;
            user-select: none;
            -webkit-user-select: none;
            background-color: #0f172a;
            color: #f8fafc;
        }
        .pixel-font {
            font-family: 'Press Start 2P', cursive;
        }
        canvas {
            image-rendering: pixelated;
            image-rendering: crisp-edges;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.5), 0 8px 10px -6px rgba(0, 0, 0, 0.5);
        }
        .retro-btn {
            transition: all 0.1s ease;
            box-shadow: 0 4px 0 #000;
        }
        .retro-btn:active {
            transform: translateY(4px);
            box-shadow: 0 0 0 #000;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-2 sm:p-4">

    <!-- Game Container -->
    <div class="w-full max-w-4xl flex flex-col items-center">
        
        <!-- Header & Title -->
        <header class="w-full text-center mb-3 flex justify-between items-center bg-slate-800/80 p-3 rounded-2xl border border-slate-700 backdrop-blur-md">
            <div class="flex items-center gap-2">
                <div class="w-8 h-8 rounded-full bg-yellow-400 flex items-center justify-center text-slate-900 font-bold text-xl border-2 border-amber-600">
                    ⚡
                </div>
                <h1 class="text-xl sm:text-2xl font-black text-yellow-400 tracking-wider pixel-font">피카츄 배구</h1>
            </div>
            
            <!-- Quick Controls Header Info -->
            <div class="flex items-center gap-2 text-xs sm:text-sm">
                <button id="soundToggleBtn" class="bg-slate-700 hover:bg-slate-600 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-600 flex items-center gap-2 transition">
                    <i class="fas fa-volume-up text-amber-400" id="soundIcon"></i>
                    <span id="soundText" class="hidden sm:inline">사운드 ON</span>
                </button>
                <button id="pauseBtn" class="bg-slate-700 hover:bg-slate-600 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-600 flex items-center gap-2 transition">
                    <i class="fas fa-pause text-blue-400" id="pauseIcon"></i>
                    <span id="pauseText" class="hidden sm:inline">일시정지</span>
                </button>
                <button id="helpBtn" class="bg-amber-500 hover:bg-amber-400 text-slate-900 font-bold px-3 py-1.5 rounded-lg shadow transition">
                    <i class="fas fa-question-circle"></i> 조작법
                </button>
            </div>
        </header>

        <!-- Canvas Container -->
        <div class="relative w-full aspect-[4/3] max-h-[600px] bg-sky-300 rounded-2xl overflow-hidden border-4 border-slate-700">
            <canvas id="gameCanvas" width="800" height="600" class="w-full h-full block"></canvas>

            <!-- Overlay Screen: Start / Mode Selection -->
            <div id="startOverlay" class="absolute inset-0 bg-slate-950/85 backdrop-blur-md flex flex-col items-center justify-center p-6 text-center z-10">
                <div class="text-yellow-400 text-3xl sm:text-5xl font-black mb-2 pixel-font tracking-wide drop-shadow-[0_4px_4px_rgba(0,0,0,0.8)]">
                    PIKACHU
                </div>
                <div class="text-amber-500 text-xl sm:text-3xl font-extrabold mb-8 tracking-wider drop-shadow-md">
                    VOLLEYBALL
                </div>

                <!-- Mode Select Buttons -->
                <div class="flex flex-col sm:flex-row gap-4 w-full max-w-md mb-6">
                    <button id="mode1P" class="retro-btn flex-1 bg-yellow-400 hover:bg-yellow-300 text-slate-950 font-black py-4 px-6 rounded-xl border-2 border-black flex flex-col items-center justify-center gap-1 group">
                        <span class="text-lg sm:text-xl"><i class="fas fa-user group-hover:scale-110 transition-transform"></i> 1인용 (vs AI)</span>
                        <span class="text-xs text-slate-800 font-normal">컴퓨터 AI와 대결</span>
                    </button>
                    <button id="mode2P" class="retro-btn flex-1 bg-emerald-400 hover:bg-emerald-300 text-slate-950 font-black py-4 px-6 rounded-xl border-2 border-black flex flex-col items-center justify-center gap-1 group">
                        <span class="text-lg sm:text-xl"><i class="fas fa-user-friends group-hover:scale-110 transition-transform"></i> 2인용 (Local)</span>
                        <span class="text-xs text-slate-800 font-normal">한 키보드로 2인 대결</span>
                    </button>
                </div>

                <!-- AI Difficulty Selector -->
                <div id="aiDifficultyGroup" class="mb-6 flex items-center gap-3 bg-slate-800/80 px-4 py-2 rounded-xl border border-slate-700">
                    <span class="text-sm text-slate-300 font-bold">AI 난이도:</span>
                    <button data-diff="easy" class="diff-btn px-3 py-1 text-xs font-bold rounded-lg bg-slate-700 text-slate-300 hover:bg-slate-600 transition">쉬움</button>
                    <button data-diff="normal" class="diff-btn px-3 py-1 text-xs font-bold rounded-lg bg-amber-500 text-slate-900 transition">보통</button>
                    <button data-diff="hard" class="diff-btn px-3 py-1 text-xs font-bold rounded-lg bg-slate-700 text-slate-300 hover:bg-slate-600 transition">어려움</button>
                </div>

                <div class="text-slate-400 text-xs sm:text-sm">
                    목표 점수: <span class="text-yellow-400 font-bold">15점</span> | 먼저 15점을 얻는 플레이어가 승리합니다!
                </div>
            </div>

            <!-- Overlay Screen: Pause -->
            <div id="pauseOverlay" class="hidden absolute inset-0 bg-slate-950/80 backdrop-blur-sm flex flex-col items-center justify-center text-center z-10">
                <div class="text-4xl font-black text-yellow-400 mb-6 pixel-font">PAUSED</div>
                <button id="resumeBtn" class="retro-btn bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black py-3 px-8 rounded-xl border-2 border-black mb-3">
                    게임 재개
                </button>
                <button id="quitBtn" class="retro-btn bg-rose-500 hover:bg-rose-400 text-slate-950 font-black py-3 px-8 rounded-xl border-2 border-black">
                    메인 메뉴로
                </button>
            </div>

            <!-- Overlay Screen: Game Over -->
            <div id="gameOverOverlay" class="hidden absolute inset-0 bg-slate-950/90 backdrop-blur-md flex flex-col items-center justify-center text-center z-10 p-6">
                <div class="text-xl sm:text-2xl font-bold text-amber-400 mb-2">MATCH FINISHED!</div>
                <div id="winnerText" class="text-3xl sm:text-5xl font-black text-yellow-400 mb-6 pixel-font drop-shadow-md">
                    PLAYER 1 WIN!
                </div>
                <div class="text-slate-300 text-lg mb-8 bg-slate-800 px-6 py-3 rounded-xl border border-slate-700">
                    최종 스코어: <span id="finalScore" class="font-bold text-white">15 - 12</span>
                </div>
                <div class="flex gap-4">
                    <button id="restartBtn" class="retro-btn bg-yellow-400 hover:bg-yellow-300 text-slate-950 font-black py-3 px-6 rounded-xl border-2 border-black text-lg">
                        <i class="fas fa-redo"></i> 다시 하기
                    </button>
                    <button id="menuBtn" class="retro-btn bg-slate-600 hover:bg-slate-500 text-white font-black py-3 px-6 rounded-xl border-2 border-black text-lg">
                        <i class="fas fa-home"></i> 메뉴
                    </button>
                </div>
            </div>

            <!-- Help Modal Window -->
            <div id="helpModal" class="hidden absolute inset-0 bg-slate-950/90 backdrop-blur-md flex flex-col items-center justify-center p-6 z-20">
                <div class="bg-slate-800 border-2 border-amber-400 rounded-2xl p-6 max-w-lg w-full text-slate-200">
                    <div class="flex justify-between items-center mb-4 border-b border-slate-700 pb-2">
                        <h3 class="text-xl font-bold text-yellow-400"><i class="fas fa-gamepad"></i> 조작법 안내</h3>
                        <button id="closeHelpBtn" class="text-slate-400 hover:text-white text-xl"><i class="fas fa-times"></i></button>
                    </div>

                    <div class="space-y-4 text-sm">
                        <!-- Player 1 -->
                        <div class="bg-slate-900 p-3 rounded-xl border border-slate-700">
                            <div class="font-bold text-yellow-400 mb-1 flex items-center gap-2">
                                <span class="w-3 h-3 rounded-full bg-red-500 inline-block"></span> 1P (좌측 피카츄 / 레드)
                            </div>
                            <div class="grid grid-cols-2 gap-2 text-xs text-slate-300">
                                <div><kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">A</kbd> / <kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">D</kbd> : 이동</div>
                                <div><kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">W</kbd> : 점프</div>
                                <div class="col-span-2"><kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">Space</kbd> / <kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">Z</kbd> : 강타/스파이크</div>
                            </div>
                        </div>

                        <!-- Player 2 -->
                        <div class="bg-slate-900 p-3 rounded-xl border border-slate-700">
                            <div class="font-bold text-sky-400 mb-1 flex items-center gap-2">
                                <span class="w-3 h-3 rounded-full bg-blue-500 inline-block"></span> 2P (우측 피카츄 / 블루)
                            </div>
                            <div class="grid grid-cols-2 gap-2 text-xs text-slate-300">
                                <div><kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">←</kbd> / <kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">→</kbd> : 이동</div>
                                <div><kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">↑</kbd> : 점프</div>
                                <div class="col-span-2"><kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">Enter</kbd> / <kbd class="px-1.5 py-0.5 bg-slate-700 rounded border border-slate-600">↓</kbd> : 강타/스파이크</div>
                            </div>
                        </div>

                        <!-- Tips -->
                        <div class="bg-amber-500/10 border border-amber-500/30 p-3 rounded-xl text-xs text-amber-300">
                            <strong>💡 팁 & 스파이크 룰:</strong><br>
                            - 점프 상태에서 공을 치며 스파이크 키를 누르면 고속 스파이크가 발동합니다!<br>
                            - 공이 네트를 넘어가서 상대편 바닥에 닿으면 득점합니다.
                        </div>
                    </div>

                    <button id="closeHelpModalBtn" class="mt-5 w-full bg-yellow-400 hover:bg-yellow-300 text-slate-950 font-bold py-2 rounded-xl border border-black">
                        확인
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Touch Controls overlay if touch supported -->
        <div id="touchControls" class="w-full mt-4 grid grid-cols-2 gap-4 hidden sm:hidden">
            <!-- P1 Left Touch Controls -->
            <div class="bg-slate-800/80 p-3 rounded-xl border border-slate-700 flex justify-around items-center">
                <button id="btnTouchLeft" class="w-12 h-12 bg-slate-700 active:bg-yellow-500 rounded-full text-white text-xl font-bold border border-slate-500 flex items-center justify-center">◀</button>
                <button id="btnTouchRight" class="w-12 h-12 bg-slate-700 active:bg-yellow-500 rounded-full text-white text-xl font-bold border border-slate-500 flex items-center justify-center">▶</button>
                <button id="btnTouchJump" class="w-12 h-12 bg-amber-500 active:bg-amber-400 rounded-full text-slate-950 text-xl font-bold border border-black flex items-center justify-center">▲</button>
                <button id="btnTouchSpike" class="w-12 h-12 bg-rose-500 active:bg-rose-400 rounded-full text-white text-sm font-bold border border-black flex items-center justify-center">⚡</button>
            </div>
        </div>

        <!-- Game Info Footer -->
        <footer class="w-full text-center mt-3 text-xs text-slate-500">
            Classic Retro Pikachu Volleyball Remake • Web Audio Synth Sound Included
        </footer>
    </div>

    <!-- JavaScript Game Logic -->
    <script>
        // --- Web Audio Synth Sound Manager ---
        class SoundFX {
            constructor() {
                this.ctx = null;
                this.enabled = true;
            }

            init() {
                if (!this.ctx) {
                    const AudioContext = window.AudioContext || window.webkitAudioContext;
                    if (AudioContext) {
                        this.ctx = new AudioContext();
                    }
                }
            }

            playBounce() {
                if (!this.enabled) return;
                this.init();
                if (!this.ctx) return;

                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();

                osc.type = 'sine';
                osc.frequency.setValueAtTime(300, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(150, this.ctx.currentTime + 0.1);

                gain.gain.setValueAtTime(0.3, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.1);

                osc.connect(gain);
                gain.connect(this.ctx.destination);

                osc.start();
                osc.stop(this.ctx.currentTime + 0.1);
            }

            playSpike() {
                if (!this.enabled) return;
                this.init();
                if (!this.ctx) return;

                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();

                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(600, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(120, this.ctx.currentTime + 0.2);

                gain.gain.setValueAtTime(0.5, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.2);

                osc.connect(gain);
                gain.connect(this.ctx.destination);

                osc.start();
                osc.stop(this.ctx.currentTime + 0.2);
            }

            playScore() {
                if (!this.enabled) return;
                this.init();
                if (!this.ctx) return;

                const now = this.ctx.currentTime;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();

                osc.type = 'triangle';
                osc.frequency.setValueAtTime(440, now); // A4
                osc.frequency.setValueAtTime(554.37, now + 0.1); // C#5
                osc.frequency.setValueAtTime(659.25, now + 0.2); // E5

                gain.gain.setValueAtTime(0.3, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);

                osc.connect(gain);
                gain.connect(this.ctx.destination);

                osc.start();
                osc.stop(now + 0.4);
            }

            playWhistle() {
                if (!this.enabled) return;
                this.init();
                if (!this.ctx) return;

                const now = this.ctx.currentTime;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();

                osc.type = 'sine';
                osc.frequency.setValueAtTime(1200, now);
                osc.frequency.setValueAtTime(1250, now + 0.05);
                osc.frequency.setValueAtTime(1200, now + 0.1);

                gain.gain.setValueAtTime(0.2, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);

                osc.connect(gain);
                gain.connect(this.ctx.destination);

                osc.start();
                osc.stop(now + 0.3);
            }
        }

        const soundFX = new SoundFX();

        // --- Game Setup Constants ---
        const CANVAS_WIDTH = 800;
        const CANVAS_HEIGHT = 600;
        const GROUND_Y = 520;
        const NET_X = CANVAS_WIDTH / 2;
        const NET_WIDTH = 12;
        const NET_HEIGHT = 160;
        const NET_TOP = GROUND_Y - NET_HEIGHT;

        const GRAVITY = 0.55;
        const SCORE_LIMIT = 15;

        // Player Physical Dimensions
        const PLAYER_RADIUS = 36;
        const PLAYER_SPEED = 7.5;
        const JUMP_FORCE = -13.5;

        // Ball Setup
        const BALL_RADIUS = 22;

        // Canvas Setup
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // State variables
        let isGameRunning = false;
        let isPaused = false;
        let isVsAI = true;
        let aiDifficulty = 'normal'; // easy, normal, hard
        let winner = null;

        let scoreP1 = 0;
        let scoreP2 = 0;
        let servingPlayer = 1; // 1 or 2

        // Key states
        const keys = {};

        // Animated Background Clouds
        const clouds = [
            { x: 50, y: 80, speed: 0.3, scale: 1 },
            { x: 300, y: 120, speed: 0.2, scale: 0.8 },
            { x: 600, y: 60, speed: 0.4, scale: 1.2 }
        ];

        // --- Player Class ---
        class Player {
            constructor(id, x, color, isP2 = false) {
                this.id = id;
                this.startX = x;
                this.x = x;
                this.y = GROUND_Y;
                this.vx = 0;
                this.vy = 0;
                this.radius = PLAYER_RADIUS;
                this.isGrounded = true;
                this.color = color; // Accent color (Red / Blue)
                this.isP2 = isP2;
                this.facingLeft = isP2;
                this.isSpiking = false;
                this.spikeTimer = 0;
            }

            reset() {
                this.x = this.startX;
                this.y = GROUND_Y;
                this.vx = 0;
                this.vy = 0;
                this.isGrounded = true;
                this.isSpiking = false;
            }

            update() {
                // Apply Gravity
                if (!this.isGrounded) {
                    this.vy += GRAVITY;
                }

                // Move X & Y
                this.x += this.vx;
                this.y += this.vy;

                // Ground Collision
                if (this.y >= GROUND_Y) {
                    this.y = GROUND_Y;
                    this.vy = 0;
                    this.isGrounded = true;
                } else {
                    this.isGrounded = false;
                }

                // Boundary Limits & Net Collision
                if (!this.isP2) {
                    // P1 (Left Side)
                    if (this.x - this.radius < 10) this.x = 10 + this.radius;
                    if (this.x + this.radius > NET_X - NET_WIDTH / 2) {
                        this.x = NET_X - NET_WIDTH / 2 - this.radius;
                    }
                } else {
                    // P2 (Right Side)
                    if (this.x - this.radius < NET_X + NET_WIDTH / 2) {
                        this.x = NET_X + NET_WIDTH / 2 + this.radius;
                    }
                    if (this.x + this.radius > CANVAS_WIDTH - 10) {
                        this.x = CANVAS_WIDTH - 10 - this.radius;
                    }
                }

                // Spike Animation Timer
                if (this.spikeTimer > 0) {
                    this.spikeTimer--;
                    if (this.spikeTimer === 0) this.isSpiking = false;
                }
            }

            triggerSpike() {
                this.isSpiking = true;
                this.spikeTimer = 15;
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);

                // Direction flip
                const dir = this.facingLeft ? -1 : 1;

                // Shadow
                ctx.beginPath();
                ctx.ellipse(0, 0, this.radius * 0.8, 10, 0, 0, Math.PI * 2);
                ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
                ctx.fill();

                // Body offset when jumping
                const bodyY = -this.radius;

                // Tail (Zigzag Pikachu Tail)
                ctx.fillStyle = '#b45309'; // Brown base
                ctx.beginPath();
                ctx.moveTo(-dir * 15, bodyY + 10);
                ctx.lineTo(-dir * 30, bodyY - 10);
                ctx.lineTo(-dir * 25, bodyY - 15);
                ctx.lineTo(-dir * 40, bodyY - 35);
                ctx.lineTo(-dir * 25, bodyY - 30);
                ctx.lineTo(-dir * 10, bodyY);
                ctx.fill();

                // Tail Yellow Part
                ctx.fillStyle = '#facc15';
                ctx.beginPath();
                ctx.moveTo(-dir * 25, bodyY - 15);
                ctx.lineTo(-dir * 40, bodyY - 35);
                ctx.lineTo(-dir * 25, bodyY - 30);
                ctx.fill();

                // Ears
                // Left Ear
                ctx.save();
                ctx.translate(-dir * 12, bodyY - 20);
                ctx.rotate(dir * -0.3);
                ctx.fillStyle = '#facc15';
                ctx.beginPath();
                ctx.ellipse(0, -15, 7, 20, 0, 0, Math.PI * 2);
                ctx.fill();
                // Ear Tip (Black)
                ctx.fillStyle = '#0f172a';
                ctx.beginPath();
                ctx.ellipse(0, -28, 6, 7, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();

                // Right Ear
                ctx.save();
                ctx.translate(dir * 12, bodyY - 20);
                ctx.rotate(dir * 0.3);
                ctx.fillStyle = '#facc15';
                ctx.beginPath();
                ctx.ellipse(0, -15, 7, 20, 0, 0, Math.PI * 2);
                ctx.fill();
                // Ear Tip (Black)
                ctx.fillStyle = '#0f172a';
                ctx.beginPath();
                ctx.ellipse(0, -28, 6, 7, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();

                // Main Body (Cute Pikachu Oval/Circle)
                ctx.fillStyle = '#facc15'; // Primary Yellow
                ctx.beginPath();
                ctx.arc(0, bodyY, this.radius, 0, Math.PI * 2);
                ctx.fill();
                ctx.lineWidth = 3;
                ctx.strokeStyle = '#eab308';
                ctx.stroke();

                // Cheeks (Red Spots)
                ctx.fillStyle = '#ef4444';
                ctx.beginPath();
                ctx.arc(dir * 18, bodyY + 6, 8, 0, Math.PI * 2);
                ctx.fill();

                // Eyes
                ctx.fillStyle = '#0f172a';
                ctx.beginPath();
                ctx.arc(dir * 12, bodyY - 6, 5, 0, Math.PI * 2);
                ctx.fill();

                // Eye Glint
                ctx.fillStyle = '#ffffff';
                ctx.beginPath();
                ctx.arc(dir * 14, bodyY - 8, 2, 0, Math.PI * 2);
                ctx.fill();

                // Nose
                ctx.fillStyle = '#0f172a';
                ctx.beginPath();
                ctx.arc(dir * 20, bodyY, 2, 0, Math.PI * 2);
                ctx.fill();

                // Mouth
                ctx.beginPath();
                ctx.arc(dir * 18, bodyY + 8, 4, 0, Math.PI);
                ctx.strokeStyle = '#0f172a';
                ctx.lineWidth = 2;
                ctx.stroke();

                // Player Indicator Band/Headband (Red for P1, Blue for P2)
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.fillRect(-15, bodyY - 28, 30, 6);

                // Spiking Effect Aura
                if (this.isSpiking) {
                    ctx.beginPath();
                    ctx.arc(0, bodyY, this.radius + 8, 0, Math.PI * 2);
                    ctx.strokeStyle = '#f59e0b';
                    ctx.lineWidth = 4;
                    ctx.stroke();
                }

                ctx.restore();
            }
        }

        // --- Ball Class ---
        class Ball {
            constructor() {
                this.x = CANVAS_WIDTH / 4;
                this.y = 200;
                this.vx = 0;
                this.vy = 0;
                this.radius = BALL_RADIUS;
                this.rotation = 0;
                this.rotationSpeed = 0;
                this.trail = [];
            }

            reset(servingPlayer) {
                this.x = servingPlayer === 1 ? CANVAS_WIDTH / 4 : (CANVAS_WIDTH * 3) / 4;
                this.y = 180;
                this.vx = 0;
                this.vy = 0.5; // Slight downward start
                this.rotation = 0;
                this.rotationSpeed = 0;
                this.trail = [];
            }

            update() {
                // Gravity
                this.vy += GRAVITY * 0.75;

                // Position update
                this.x += this.vx;
                this.y += this.vy;

                // Rotation based on horizontal velocity
                this.rotation += this.vx * 0.05;

                // Motion Trail Effect
                if (Math.abs(this.vx) > 8 || Math.abs(this.vy) > 8) {
                    this.trail.push({ x: this.x, y: this.y, alpha: 0.6 });
                    if (this.trail.length > 5) this.trail.shift();
                } else {
                    if (this.trail.length > 0) this.trail.shift();
                }

                // Wall Collisions
                if (this.x - this.radius < 0) {
                    this.x = this.radius;
                    this.vx = -this.vx * 0.85;
                    soundFX.playBounce();
                }
                if (this.x + this.radius > CANVAS_WIDTH) {
                    this.x = CANVAS_WIDTH - this.radius;
                    this.vx = -this.vx * 0.85;
                    soundFX.playBounce();
                }

                // Top Ceiling Collision
                if (this.y - this.radius < 0) {
                    this.y = this.radius;
                    this.vy = -this.vy * 0.8;
                }

                // Net Collision
                const netLeft = NET_X - NET_WIDTH / 2;
                const netRight = NET_X + NET_WIDTH / 2;

                if (this.y + this.radius > NET_TOP) {
                    // Horizontal hit against net sides
                    if (this.x + this.radius > netLeft && this.x < NET_X && this.vx > 0) {
                        this.x = netLeft - this.radius;
                        this.vx = -this.vx * 0.7;
                        soundFX.playBounce();
                    } else if (this.x - this.radius < netRight && this.x > NET_X && this.vx < 0) {
                        this.x = netRight + this.radius;
                        this.vx = -this.vx * 0.7;
                        soundFX.playBounce();
                    }

                    // Top of the net bounce
                    if (this.x > netLeft - 5 && this.x < netRight + 5 && this.y + this.radius >= NET_TOP && this.y < NET_TOP + 15) {
                        this.y = NET_TOP - this.radius;
                        this.vy = -Math.abs(this.vy) * 0.8;
                        soundFX.playBounce();
                    }
                }
            }

            draw() {
                // Motion Trail
                this.trail.forEach((t) => {
                    ctx.save();
                    ctx.beginPath();
                    ctx.arc(t.x, t.y, this.radius, 0, Math.PI * 2);
                    ctx.fillStyle = `rgba(239, 68, 68, ${t.alpha})`;
                    ctx.fill();
                    ctx.restore();
                });

                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate(this.rotation);

                // Ball Outer Body (Pokeball Styling)
                ctx.beginPath();
                ctx.arc(0, 0, this.radius, 0, Math.PI * 2);
                ctx.fillStyle = '#ffffff';
                ctx.fill();

                // Top Half (Red)
                ctx.beginPath();
                ctx.arc(0, 0, this.radius, Math.PI, Math.PI * 2);
                ctx.fillStyle = '#ef4444';
                ctx.fill();

                // Center Black Line
                ctx.fillStyle = '#0f172a';
                ctx.fillRect(-this.radius, -2, this.radius * 2, 4);

                // Center Circle
                ctx.beginPath();
                ctx.arc(0, 0, this.radius * 0.35, 0, Math.PI * 2);
                ctx.fillStyle = '#0f172a';
                ctx.fill();

                ctx.beginPath();
                ctx.arc(0, 0, this.radius * 0.22, 0, Math.PI * 2);
                ctx.fillStyle = '#ffffff';
                ctx.fill();

                // Outer border
                ctx.beginPath();
                ctx.arc(0, 0, this.radius, 0, Math.PI * 2);
                ctx.strokeStyle = '#0f172a';
                ctx.lineWidth = 2.5;
                ctx.stroke();

                ctx.restore();
            }
        }

        // --- Instantiate Objects ---
        const p1 = new Player(1, 180, '#ef4444', false);
        const p2 = new Player(2, 620, '#3b82f6', true);
        const ball = new Ball();

        // --- AI Logic for Player 2 ---
        function updateAI() {
            if (!isVsAI || winner) return;

            // Target prediction: track ball position
            let targetX = ball.x;

            // Simple difficulty speed caps
            let speedCap = PLAYER_SPEED * 0.8;
            let reactionDist = 350;

            if (aiDifficulty === 'easy') {
                speedCap = PLAYER_SPEED * 0.6;
                reactionDist = 250;
            } else if (aiDifficulty === 'hard') {
                speedCap = PLAYER_SPEED * 0.95;
                reactionDist = 450;
            }

            // AI only moves when ball is coming to its side or high enough
            if (ball.x > CANVAS_WIDTH - reactionDist || ball.vx > 0) {
                if (p2.x < targetX - 10) {
                    p2.vx = Math.min(PLAYER_SPEED, speedCap);
                } else if (p2.x > targetX + 10) {
                    p2.vx = -Math.min(PLAYER_SPEED, speedCap);
                } else {
                    p2.vx = 0;
                }

                // AI Jump & Spike Trigger
                const distToBallX = Math.abs(p2.x - ball.x);
                const distToBallY = p2.y - ball.y;

                if (distToBallX < 50 && distToBallY > 60 && distToBallY < 140 && p2.isGrounded) {
                    // Random chance based on difficulty to jump
                    if (Math.random() < (aiDifficulty === 'hard' ? 0.9 : 0.7)) {
                        p2.vy = JUMP_FORCE;
                        p2.isGrounded = false;
                    }
                }

                // AI Spike timing
                if (!p2.isGrounded && distToBallX < 40 && Math.abs(distToBallY - 50) < 30) {
                    p2.triggerSpike();
                }
            } else {
                // Return to default defensive stance
                const defaultX = 620;
                if (p2.x < defaultX - 10) p2.vx = PLAYER_SPEED * 0.5;
                else if (p2.x > defaultX + 10) p2.vx = -PLAYER_SPEED * 0.5;
                else p2.vx = 0;
            }
        }

        // --- Collision Detection between Player and Ball ---
        function checkPlayerBallCollision(player) {
            const headY = player.y - player.radius;
            const dx = ball.x - player.x;
            const dy = ball.y - headY;
            const distance = Math.sqrt(dx * dx + dy * dy);

            if (distance < player.radius + ball.radius) {
                // Angle of deflection
                let angle = Math.atan2(dy, dx);

                // Force magnitude
                let power = 11;

                if (player.isSpiking) {
                    power = 18; // Fast Spike Force
                    soundFX.playSpike();
                } else {
                    soundFX.playBounce();
                }

                // Calculate bounce velocity
                ball.vx = Math.cos(angle) * power;
                ball.vy = Math.sin(angle) * power;

                // Ensure ball moves upward if hit from top/side of head
                if (ball.vy > -3) ball.vy = -6;

                // Push ball out of overlap
                const overlap = (player.radius + ball.radius) - distance;
                ball.x += Math.cos(angle) * overlap;
                ball.y += Math.sin(angle) * overlap;
            }
        }

        // --- Score & Round Logic ---
        function checkPointScored() {
            if (ball.y + ball.radius >= GROUND_Y) {
                // Point Scored!
                soundFX.playScore();

                if (ball.x < NET_X) {
                    // Scored in Left Side -> P2 Gets Point
                    scoreP2++;
                    servingPlayer = 1; // Loser serves next
                } else {
                    // Scored in Right Side -> P1 Gets Point
                    scoreP1++;
                    servingPlayer = 2; // Loser serves next
                }

                // Check Win Condition
                if (scoreP1 >= SCORE_LIMIT || scoreP2 >= SCORE_LIMIT) {
                    endGame();
                } else {
                    resetRound();
                }
            }
        }

        function resetRound() {
            p1.reset();
            p2.reset();
            ball.reset(servingPlayer);
        }

        function endGame() {
            isGameRunning = false;
            winner = scoreP1 >= SCORE_LIMIT ? 1 : 2;

            const winnerTextEl = document.getElementById('winnerText');
            const finalScoreEl = document.getElementById('finalScore');

            if (winner === 1) {
                winnerTextEl.textContent = "PLAYER 1 WIN!";
                winnerTextEl.className = "text-3xl sm:text-5xl font-black text-red-400 mb-6 pixel-font drop-shadow-md";
            } else {
                winnerTextEl.textContent = isVsAI ? "COMPUTER WIN!" : "PLAYER 2 WIN!";
                winnerTextEl.className = "text-3xl sm:text-5xl font-black text-blue-400 mb-6 pixel-font drop-shadow-md";
            }

            finalScoreEl.textContent = `${scoreP1} - ${scoreP2}`;
            document.getElementById('gameOverOverlay').classList.remove('hidden');
        }

        // --- Controls Handler ---
        function handleControls() {
            // Player 1 Controls (WASD)
            p1.vx = 0;
            if (keys['KeyA'] || keys['keya']) p1.vx = -PLAYER_SPEED;
            if (keys['KeyD'] || keys['keyd']) p1.vx = PLAYER_SPEED;
            if ((keys['KeyW'] || keys['keyw']) && p1.isGrounded) {
                p1.vy = JUMP_FORCE;
                p1.isGrounded = false;
            }
            if (keys['Space'] || keys['KeyZ'] || keys['keyz']) {
                p1.triggerSpike();
            }

            // Player 2 Controls (Arrows) if 2-Player mode
            if (!isVsAI) {
                p2.vx = 0;
                if (keys['ArrowLeft']) p2.vx = -PLAYER_SPEED;
                if (keys['ArrowRight']) p2.vx = PLAYER_SPEED;
                if (keys['ArrowUp'] && p2.isGrounded) {
                    p2.vy = JUMP_FORCE;
                    p2.isGrounded = false;
                }
                if (keys['Enter'] || keys['ArrowDown']) {
                    p2.triggerSpike();
                }
            }
        }

        // Keyboard Event Listeners
        window.addEventListener('keydown', (e) => {
            keys[e.code] = true;
            if (e.code === 'KeyP' || e.code === 'Escape') {
                togglePause();
            }
        });

        window.addEventListener('keyup', (e) => {
            keys[e.code] = false;
        });

        // Touch Control Listeners for Mobile
        function setupTouchControls() {
            const bindTouch = (id, keyName) => {
                const btn = document.getElementById(id);
                if (!btn) return;
                btn.addEventListener('touchstart', (e) => { e.preventDefault(); keys[keyName] = true; });
                btn.addEventListener('touchend', (e) => { e.preventDefault(); keys[keyName] = false; });
            };

            bindTouch('btnTouchLeft', 'KeyA');
            bindTouch('btnTouchRight', 'KeyD');
            bindTouch('btnTouchJump', 'KeyW');
            bindTouch('btnTouchSpike', 'Space');
        }
        setupTouchControls();

        // --- Render Functions ---
        function drawBackground() {
            // Sky Gradient
            const skyGradient = ctx.createLinearGradient(0, 0, 0, GROUND_Y);
            skyGradient.addColorStop(0, '#38bdf8');
            skyGradient.addColorStop(1, '#bae6fd');
            ctx.fillStyle = skyGradient;
            ctx.fillRect(0, 0, CANVAS_WIDTH, GROUND_Y);

            // Sun
            ctx.beginPath();
            ctx.arc(CANVAS_WIDTH / 2, 70, 45, 0, Math.PI * 2);
            ctx.fillStyle = '#fef08a';
            ctx.fill();

            // Clouds Animation
            clouds.forEach(cloud => {
                cloud.x += cloud.speed;
                if (cloud.x > CANVAS_WIDTH + 80) cloud.x = -80;

                ctx.save();
                ctx.translate(cloud.x, cloud.y);
                ctx.scale(cloud.scale, cloud.scale);
                ctx.fillStyle = 'rgba(255, 255, 255, 0.85)';
                ctx.beginPath();
                ctx.arc(0, 0, 20, 0, Math.PI * 2);
                ctx.arc(20, -10, 25, 0, Math.PI * 2);
                ctx.arc(45, 0, 20, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            });

            // Ground Sand Beach
            ctx.fillStyle = '#fde047';
            ctx.fillRect(0, GROUND_Y, CANVAS_WIDTH, CANVAS_HEIGHT - GROUND_Y);

            // Sand Texture / Boundary Line
            ctx.fillStyle = '#eab308';
            ctx.fillRect(0, GROUND_Y, CANVAS_WIDTH, 8);

            // Net Body
            ctx.fillStyle = '#cbd5e1';
            ctx.fillRect(NET_X - NET_WIDTH / 2, NET_TOP, NET_WIDTH, NET_HEIGHT);
            ctx.strokeStyle = '#64748b';
            ctx.lineWidth = 2;
            ctx.strokeRect(NET_X - NET_WIDTH / 2, NET_TOP, NET_WIDTH, NET_HEIGHT);

            // Net Mesh Grid Lines
            ctx.strokeStyle = 'rgba(100, 116, 139, 0.4)';
            ctx.lineWidth = 1;
            for (let y = NET_TOP + 10; y < GROUND_Y; y += 12) {
                ctx.beginPath();
                ctx.moveTo(NET_X - NET_WIDTH / 2, y);
                ctx.lineTo(NET_X + NET_WIDTH / 2, y);
                ctx.stroke();
            }

            // Net Top Cap
            ctx.fillStyle = '#ef4444';
            ctx.fillRect(NET_X - NET_WIDTH / 2 - 2, NET_TOP - 4, NET_WIDTH + 4, 8);
        }

        function drawScoreBoard() {
            ctx.save();
            ctx.font = '28px "Press Start 2P", cursive';

            // P1 Score (Left)
            ctx.fillStyle = '#ef4444';
            ctx.textAlign = 'left';
            ctx.fillText(`1P: ${scoreP1}`, 40, 50);

            // P2 Score (Right)
            ctx.fillStyle = '#3b82f6';
            ctx.textAlign = 'right';
            const p2Label = isVsAI ? 'COM' : '2P';
            ctx.fillText(`${p2Label}: ${scoreP2}`, CANVAS_WIDTH - 40, 50);

            // Mode Label (Center)
            ctx.font = '12px "Press Start 2P", cursive';
            ctx.fillStyle = '#0f172a';
            ctx.textAlign = 'center';
            ctx.fillText(isVsAI ? `VS AI (${aiDifficulty.toUpperCase()})` : '2 PLAYER LOCAL', CANVAS_WIDTH / 2, 30);

            ctx.restore();
        }

        // --- Main Game Loop ---
        function gameLoop() {
            if (isGameRunning && !isPaused) {
                // Update Physics
                handleControls();
                updateAI();

                p1.update();
                p2.update();
                ball.update();

                checkPlayerBallCollision(p1);
                checkPlayerBallCollision(p2);
                checkPointScored();
            }

            // Clear Screen
            ctx.clearRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);

            // Render World
            drawBackground();
            p1.draw();
            p2.draw();
            ball.draw();
            drawScoreBoard();

            requestAnimationFrame(gameLoop);
        }

        // --- UI & Game State Control ---
        function startGame(vsAI) {
            isVsAI = vsAI;
            scoreP1 = 0;
            scoreP2 = 0;
            servingPlayer = 1;
            winner = null;

            p1.reset();
            p2.reset();
            ball.reset(servingPlayer);

            document.getElementById('startOverlay').classList.add('hidden');
            document.getElementById('gameOverOverlay').classList.add('hidden');
            document.getElementById('pauseOverlay').classList.add('hidden');

            soundFX.playWhistle();
            isGameRunning = true;
            isPaused = false;
        }

        function togglePause() {
            if (!isGameRunning) return;
            isPaused = !isPaused;

            const pauseOverlay = document.getElementById('pauseOverlay');
            if (isPaused) {
                pauseOverlay.classList.remove('hidden');
            } else {
                pauseOverlay.classList.add('hidden');
            }
        }

        // Event Listeners for UI
        document.getElementById('mode1P').addEventListener('click', () => startGame(true));
        document.getElementById('mode2P').addEventListener('click', () => startGame(false));

        document.getElementById('pauseBtn').addEventListener('click', togglePause);
        document.getElementById('resumeBtn').addEventListener('click', togglePause);

        document.getElementById('quitBtn').addEventListener('click', () => {
            isGameRunning = false;
            isPaused = false;
            document.getElementById('pauseOverlay').classList.add('hidden');
            document.getElementById('startOverlay').classList.remove('hidden');
        });

        document.getElementById('restartBtn').addEventListener('click', () => startGame(isVsAI));
        document.getElementById('menuBtn').addEventListener('click', () => {
            document.getElementById('gameOverOverlay').classList.add('hidden');
            document.getElementById('startOverlay').classList.remove('hidden');
        });

        // AI Difficulty selector buttons
        document.querySelectorAll('.diff-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                document.querySelectorAll('.diff-btn').forEach(b => {
                    b.className = "diff-btn px-3 py-1 text-xs font-bold rounded-lg bg-slate-700 text-slate-300 hover:bg-slate-600 transition";
                });
                e.target.className = "diff-btn px-3 py-1 text-xs font-bold rounded-lg bg-amber-500 text-slate-900 transition";
                aiDifficulty = e.target.dataset.diff;
            });
        });

        // Help Modal Controls
        const helpModal = document.getElementById('helpModal');
        document.getElementById('helpBtn').addEventListener('click', () => helpModal.classList.remove('hidden'));
        document.getElementById('closeHelpBtn').addEventListener('click', () => helpModal.classList.add('hidden'));
        document.getElementById('closeHelpModalBtn').addEventListener('click', () => helpModal.classList.add('hidden'));

        // Sound Mute Toggle
        const soundToggleBtn = document.getElementById('soundToggleBtn');
        soundToggleBtn.addEventListener('click', () => {
            soundFX.enabled = !soundFX.enabled;
            document.getElementById('soundIcon').className = soundFX.enabled ? "fas fa-volume-up text-amber-400" : "fas fa-volume-mute text-slate-400";
            document.getElementById('soundText').textContent = soundFX.enabled ? "사운드 ON" : "사운드 OFF";
        });

        // Start Loop on Window Load
        window.onload = function() {
            requestAnimationFrame(gameLoop);
        };
    </script>
</body>
</html>
