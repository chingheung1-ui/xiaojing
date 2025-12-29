<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2026年的小静 - 雪山金币游戏</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial Rounded MT Bold', 'Arial', sans-serif;
        }

        body {
            background: linear-gradient(to bottom, #1a2980, #26d0ce);
            color: #333;
            min-height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* 雪山背景和雪动画 */
        .game-container {
            position: relative;
            width: 100%;
            max-width: 900px;
            height: 600px;
            background: linear-gradient(to bottom, #87CEEB 0%, #E0F6FF 30%, #FFFFFF 100%);
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }

        .mountain {
            position: absolute;
            bottom: 0;
            width: 100%;
            height: 60%;
            background: linear-gradient(to top, #FFFFFF 0%, #F0F8FF 30%, #B0E0E6 100%);
            border-radius: 0 0 20px 20px;
            z-index: 1;
        }

        .mountain-peak {
            position: absolute;
            bottom: 60%;
            left: 20%;
            width: 200px;
            height: 150px;
            background: white;
            clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
            box-shadow: 0 5px 15px rgba(255, 255, 255, 0.5);
        }

        .mountain-peak:nth-child(2) {
            left: 50%;
            bottom: 55%;
            width: 180px;
            height: 130px;
        }

        .mountain-peak:nth-child(3) {
            left: 70%;
            bottom: 58%;
            width: 160px;
            height: 120px;
        }

        /* 雪动画 */
        .snow-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 2;
        }

        .snowflake {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            opacity: 0.8;
            animation: fall linear infinite;
        }

        @keyframes fall {
            0% {
                transform: translateY(-10px) translateX(0);
                opacity: 0;
            }
            10% {
                opacity: 0.8;
            }
            90% {
                opacity: 0.8;
            }
            100% {
                transform: translateY(600px) translateX(20px);
                opacity: 0;
            }
        }

        /* 对话框样式 */
        .dialog {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 400px;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            text-align: center;
            z-index: 10;
            display: none;
        }

        .dialog.active {
            display: block;
            animation: popIn 0.5s ease-out;
        }

        @keyframes popIn {
            0% { transform: translate(-50%, -50%) scale(0.5); opacity: 0; }
            100% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
        }

        .dialog h2 {
            color: #2c3e50;
            margin-bottom: 20px;
            font-size: 28px;
        }

        .dialog p {
            color: #7f8c8d;
            margin-bottom: 25px;
            font-size: 18px;
            line-height: 1.5;
        }

        .dialog-button {
            background: linear-gradient(to right, #3498db, #2ecc71);
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 18px;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(46, 204, 113, 0.4);
        }

        .dialog-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(46, 204, 113, 0.6);
        }

        /* 游戏界面 */
        .game-ui {
            position: absolute;
            width: 100%;
            height: 100%;
            z-index: 5;
            display: none;
        }

        .game-ui.active {
            display: block;
        }

        .score-container {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            padding: 15px 25px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
            z-index: 6;
        }

        .score-icon {
            color: #f39c12;
            font-size: 28px;
            margin-right: 10px;
        }

        .score-text {
            font-size: 24px;
            font-weight: bold;
            color: #2c3e50;
        }

        .timer-container {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            padding: 15px 25px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
            z-index: 6;
        }

        .timer-icon {
            color: #e74c3c;
            font-size: 28px;
            margin-right: 10px;
        }

        .timer-text {
            font-size: 24px;
            font-weight: bold;
            color: #2c3e50;
        }

        /* 金币样式 */
        .coin {
            position: absolute;
            width: 60px;
            height: 60px;
            background: radial-gradient(circle at 30px 30px, #FFD700, #FFA500);
            border-radius: 50%;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            cursor: pointer;
            z-index: 4;
            transition: transform 0.2s;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: #8B6914;
            font-weight: bold;
        }

        .coin:hover {
            transform: scale(1.1);
        }

        .coin:active {
            transform: scale(0.9);
        }

        .coin::after {
            content: "";
            position: absolute;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: radial-gradient(circle at 20px 20px, #FFFACD, transparent);
        }

        /* 胜利/失败界面 */
        .result-screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.85);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 10;
            display: none;
        }

        .result-screen.active {
            display: flex;
        }

        .result-content {
            background: white;
            border-radius: 20px;
            padding: 40px;
            text-align: center;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.5);
        }

        .result-title {
            color: #2c3e50;
            font-size: 32px;
            margin-bottom: 20px;
        }

        .success-title {
            color: #2ecc71;
        }

        .fail-title {
            color: #e74c3c;
        }

        .gift-box {
            width: 150px;
            height: 150px;
            margin: 20px auto;
            background: linear-gradient(to bottom, #FF416C, #FF4B2B);
            border-radius: 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            position: relative;
            box-shadow: 0 10px 25px rgba(255, 65, 108, 0.5);
            transition: transform 0.3s;
        }

        .gift-box:hover {
            transform: scale(1.05);
        }

        .gift-box::before {
            content: "";
            position: absolute;
            width: 100%;
            height: 20px;
            background: #FFD700;
            top: 50%;
            left: 0;
            transform: translateY(-50%);
        }

        .gift-box::after {
            content: "🎁";
            font-size: 60px;
            z-index: 1;
        }

        .result-message {
            font-size: 20px;
            color: #7f8c8d;
            margin: 20px 0;
        }

        .wechat-notice {
            background: #09bb07;
            color: white;
            padding: 15px;
            border-radius: 10px;
            margin: 20px 0;
            font-size: 18px;
            display: none;
        }

        .wechat-notice.active {
            display: block;
        }

        /* 游戏说明 */
        .instructions {
            position: absolute;
            bottom: 20px;
            left: 0;
            width: 100%;
            text-align: center;
            color: #34495e;
            font-size: 16px;
            z-index: 6;
            padding: 0 20px;
        }

        /* 响应式设计 */
        @media (max-width: 768px) {
            .game-container {
                height: 500px;
                max-width: 95%;
            }
            
            .dialog {
                width: 90%;
                padding: 20px;
            }
            
            .dialog h2 {
                font-size: 24px;
            }
            
            .coin {
                width: 50px;
                height: 50px;
                font-size: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <!-- 雪山背景 -->
        <div class="mountain">
            <div class="mountain-peak"></div>
            <div class="mountain-peak"></div>
            <div class="mountain-peak"></div>
        </div>
        
        <!-- 雪动画 -->
        <div class="snow-container" id="snow-container"></div>
        
        <!-- 对话框1 -->
        <div class="dialog active" id="dialog1">
            <h2>2026年的小静</h2>
            <p>新的一年即将到来，希望你每天的世界里都充满快乐和幸福AwA！</p>
            <button class="dialog-button" id="next1">2026年的小静要开心</button>
        </div>
        
        <!-- 对话框2 -->
        <div class="dialog" id="dialog2">
            <h2>2026年的小静</h2>
            <p>💴多多，买下喜欢的相机！</p>
            <button class="dialog-button" id="next2">2026年的小静会幸福</button>
        </div>
        
        <!-- 对话框3 - 开始游戏 -->
        <div class="dialog" id="dialog3">
            <h2>开启幸运游戏</h2>
            <p>点击天上掉落的金币，收集25个即可获得惊喜礼物！</p>
            <p style="color:#e74c3c; font-weight: bold;">注意：你只有7秒钟时间！</p>
            <button class="dialog-button" id="startGame">开始游戏</button>
        </div>
        
        <!-- 游戏界面 -->
        <div class="game-ui" id="gameUI">
            <div class="score-container">
                <i class="fas fa-coins score-icon"></i>
                <div class="score-text">金币: <span id="score">0</span>/25</div>
            </div>
            <div class="timer-container">
                <i class="fas fa-clock timer-icon"></i>
                <div class="timer-text">时间: <span id="timer">5.0</span>秒</div>
            </div>
            
            <!-- 金币将在这里动态生成 -->
            
            <div class="instructions">
                点击天上掉落的金币！在7秒内收集25个金币获得惊喜礼物！
            </div>
        </div>
        
        <!-- 胜利界面 -->
        <div class="result-screen" id="winScreen">
            <div class="result-content">
                <h2 class="result-title success-title">恭喜你成功了 好棒 作者都做不到T-T！</h2>
                <p class="result-message">你成功在7秒内收集了25个金币！</p>
                <p class="result-message">点击下面的礼物盒领取奖励：</p>
                <div class="gift-box" id="giftBox"></div>
                <div class="wechat-notice" id="wechatNotice">
                    <i class="fab fa-weixin"></i> 请到微信领取你的专属奖励！
                </div>
                <button class="dialog-button" id="playAgainWin">再玩一次</button>
            </div>
        </div>
        
        <!-- 失败界面 -->
        <div class="result-screen" id="failScreen">
            <div class="result-content">
                <h2 class="result-title fail-title">时间到了！</h2>
                <p class="result-message">很遗憾，你没有在7秒内收集到25个金币。</p>
                <p class="result-message">再试一次吧，小静值得这份礼物！</p>
                <button class="dialog-button" id="playAgainFail">再来一次</button>
            </div>
        </div>
    </div>

    <script>
        // 获取DOM元素
        const dialog1 = document.getElementById('dialog1');
        const dialog2 = document.getElementById('dialog2');
        const dialog3 = document.getElementById('dialog3');
        const gameUI = document.getElementById('gameUI');
        const winScreen = document.getElementById('winScreen');
        const failScreen = document.getElementById('failScreen');
        
        const next1Btn = document.getElementById('next1');
        const next2Btn = document.getElementById('next2');
        const startGameBtn = document.getElementById('startGame');
        const playAgainWinBtn = document.getElementById('playAgainWin');
        const playAgainFailBtn = document.getElementById('playAgainFail');
        const giftBox = document.getElementById('giftBox');
        const wechatNotice = document.getElementById('wechatNotice');
        
        const scoreElement = document.getElementById('score');
        const timerElement = document.getElementById('timer');
        
        // 游戏变量
        let score = 0;
        let timeLeft = 5.0;
        let gameActive = false;
        let timerInterval;
        let coins = [];
        let snowflakes = [];
        
        // 初始化雪花
        function createSnowflakes() {
            const snowContainer = document.getElementById('snow-container');
            for (let i = 0; i < 100; i++) {
                const snowflake = document.createElement('div');
                snowflake.classList.add('snowflake');
                
                // 随机大小
                const size = Math.random() * 5 + 2;
                snowflake.style.width = `${size}px`;
                snowflake.style.height = `${size}px`;
                
                // 随机位置
                snowflake.style.left = `${Math.random() * 100}%`;
                
                // 随机透明度
                snowflake.style.opacity = Math.random() * 0.7 + 0.3;
                
                // 随机动画时长和延迟
                const duration = Math.random() * 5 + 5;
                const delay = Math.random() * 5;
                snowflake.style.animationDuration = `${duration}s`;
                snowflake.style.animationDelay = `${delay}s`;
                
                snowContainer.appendChild(snowflake);
                snowflakes.push(snowflake);
            }
        }
        
        // 对话框流程
        next1Btn.addEventListener('click', () => {
            dialog1.classList.remove('active');
            dialog2.classList.add('active');
        });
        
        next2Btn.addEventListener('click', () => {
            dialog2.classList.remove('active');
            dialog3.classList.add('active');
        });
        
        // 开始游戏
        startGameBtn.addEventListener('click', () => {
            dialog3.classList.remove('active');
            gameUI.classList.add('active');
            startGame();
        });
        
        // 初始化游戏
        function startGame() {
            // 重置游戏状态
            score = 0;
            timeLeft = 8.0;
            gameActive = true;
            scoreElement.textContent = score;
            timerElement.textContent = timeLeft.toFixed(1);
            
            // 清除之前的金币
            coins.forEach(coin => coin.remove());
            coins = [];
            
            // 开始计时器
            clearInterval(timerInterval);
            timerInterval = setInterval(updateTimer, 100);
            
            // 开始生成金币
            generateCoin();
        }
        
        // 更新计时器
        function updateTimer() {
            if (!gameActive) return;
            
            timeLeft -= 0.1;
            timerElement.textContent = timeLeft.toFixed(1);
            
            // 时间到
            if (timeLeft <= 0) {
                timeLeft = 0;
                endGame(false);
            }
        }
        
        // 生成金币
        function generateCoin() {
            if (!gameActive) return;
            
            // 随机位置
            const x = Math.random() * (gameUI.clientWidth - 60);
            
            // 创建金币
            const coin = document.createElement('div');
            coin.classList.add('coin');
            coin.style.left = `${x}px`;
            coin.style.top = '-60px';
            
            // 随机金币值（1-3）
            const value = Math.floor(Math.random() * 3) + 1;
            coin.textContent = value;
            coin.dataset.value = value;
            
            // 添加点击事件
            coin.addEventListener('click', collectCoin);
            
            gameUI.appendChild(coin);
            coins.push(coin);
            
            // 金币掉落动画
            const duration = Math.random() * 1.5 + 1;
            coin.style.transition = `top ${duration}s linear`;
            
            // 使用requestAnimationFrame确保DOM已更新
            requestAnimationFrame(() => {
                coin.style.top = `${gameUI.clientHeight - 60}px`;
            });
            
            // 金币消失
            setTimeout(() => {
                if (coin.parentNode && gameActive) {
                    coin.remove();
                    const index = coins.indexOf(coin);
                    if (index > -1) coins.splice(index, 1);
                }
            }, duration * 1000);
            
            // 根据游戏状态调整生成速度
            const speed = score > 20 ? 300 : score > 15 ? 400 : score > 10 ? 500 : 600;
            
            // 继续生成金币
            if (gameActive) {
                setTimeout(generateCoin, speed);
            }
        }
        
        // 收集金币
        function collectCoin(event) {
            if (!gameActive) return;
            
            const coin = event.currentTarget;
            const value = parseInt(coin.dataset.value);
            
            // 增加分数
            score += value;
            scoreElement.textContent = score;
            
            // 移除金币
            coin.remove();
            const index = coins.indexOf(coin);
            if (index > -1) coins.splice(index, 1);
            
            // 检查是否获胜
            if (score >= 25) {
                endGame(true);
            }
            
            // 添加收集效果
            createCollectEffect(coin);
        }
        
        // 创建收集效果
        function createCollectEffect(coin) {
            const effect = document.createElement('div');
            effect.style.position = 'absolute';
            effect.style.left = coin.style.left;
            effect.style.top = coin.style.top;
            effect.style.width = '60px';
            effect.style.height = '60px';
            effect.style.borderRadius = '50%';
            effect.style.background = 'radial-gradient(circle, rgba(255,215,0,0.8), transparent)';
            effect.style.zIndex = '5';
            effect.style.pointerEvents = 'none';
            effect.style.animation = 'popOut 0.5s forwards';
            
            // 添加动画
            const style = document.createElement('style');
            style.textContent = `
                @keyframes popOut {
                    0% { transform: scale(1); opacity: 1; }
                    100% { transform: scale(1.5); opacity: 0; }
                }
            `;
            document.head.appendChild(style);
            
            gameUI.appendChild(effect);
            
            // 移除效果元素
            setTimeout(() => {
                effect.remove();
                style.remove();
            }, 500);
        }
        
        // 结束游戏
        function endGame(success) {
            gameActive = false;
            clearInterval(timerInterval);
            
            // 清除所有金币
            coins.forEach(coin => coin.remove());
            coins = [];
            
            // 显示结果界面
            if (success) {
                winScreen.classList.add('active');
            } else {
                failScreen.classList.add('active');
            }
            
            gameUI.classList.remove('active');
        }
        
        // 礼物盒点击事件
        giftBox.addEventListener('click', () => {
            wechatNotice.classList.add('active');
        });
        
        // 重新开始游戏
        playAgainWinBtn.addEventListener('click', () => {
            winScreen.classList.remove('active');
            wechatNotice.classList.remove('active');
            startGame();
            gameUI.classList.add('active');
        });
        
        playAgainFailBtn.addEventListener('click', () => {
            failScreen.classList.remove('active');
            startGame();
            gameUI.classList.add('active');
        });
        
        // 初始化雪花
        createSnowflakes();
        
        // 窗口大小变化时调整
        window.addEventListener('resize', () => {
            // 重新生成雪花
            snowflakes.forEach(snowflake => snowflake.remove());
            snowflakes = [];
            createSnowflakes();
        });
    </script>
</body>
</html>
