<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Valentine Surprise for Gyanvi</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Poppins:wght@600&display=swap');
        
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { 
            font-family: 'Poppins', sans-serif; 
            display: flex; justify-content: center; align-items: center; 
            min-height: 100vh; 
            background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 50%, #fecfef 100%);
            overflow: hidden; position: relative;
            touch-action: manipulation;
            user-select: none; -webkit-user-select: none;
        }
        body::before {
            content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="20" r="3" fill="%23ff69b4"><animate attributeName="cy" values="20;80;20" dur="3s" repeatCount="indefinite"/></circle></svg>') repeat;
            animation: sparkle 20s linear infinite; opacity: 0.3;
        }
        @keyframes sparkle { 0% { transform: translateY(0) rotate(0deg); } 100% { transform: translateY(-100px) rotate(360deg); } }

        .container {
            position: relative; width: 90%; max-width: 500px; height: 500px;
            background: rgba(255,255,255,0.95); border-radius: 30px;
            display: flex; justify-content: center; align-items: center; flex-direction: column;
            text-align: center; box-shadow: 0 20px 40px rgba(0,0,0,0.2);
            backdrop-filter: blur(10px); touch-action: none;
        }

        .question {
            font-family: 'Dancing Script', cursive; font-size: 3.5em; color: #d63384;
            margin-bottom: 40px; text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
            animation: pulse 2s infinite;
        }
        @keyframes pulse { 0%,100% { transform: scale(1); } 50% { transform: scale(1.05); } }

        .btn-group { position: relative; width: 300px; height: 80px; }

        button {
            position: absolute; width: 130px; height: 55px; font-size: 1.4em; font-weight: 600;
            border: none; border-radius: 25px; cursor: pointer; transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2); min-height: 55px;
        }
        .yes-btn { left: 0; background: linear-gradient(45deg, #ff6b9d, #c44569); color: white; }
        .yes-btn:hover { transform: scale(1.1); box-shadow: 0 8px 25px rgba(255,107,157,0.4); }
        .no-btn { right: 0; background: linear-gradient(45deg, #f093fb, #f5576c); color: white; }
        .no-btn:hover { transform: scale(1.1); }

        .success-message {
            display: none; flex-direction: column; align-items: center;
            animation: bounceIn 0.8s ease;
        }
        @keyframes bounceIn { 0% { opacity: 0; transform: scale(0.5); } 60% { opacity: 1; transform: scale(1.2); } 100% { transform: scale(1); } }

        .success-text {
            font-family: 'Dancing Script', cursive; font-size: 4em; color: #28a745;
            margin-bottom: 20px; text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }
        .success-gif { width: 200px; height: 200px; }

        .hearts { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; display: none; }
        .heart {
            position: absolute; font-size: 20px;
            animation: float 3s ease-out forwards;
        }
        @keyframes float {
            0% { transform: translateY(100vh) rotate(0deg); opacity: 1; }
            100% { transform: translateY(-100px) rotate(360deg); opacity: 0; }
        }

        @media (max-width: 480px) {
            .question { font-size: 2.2em; margin-bottom: 30px; }
            .btn-group { width: 280px; height: 85px; }
            button { width: 120px; height: 60px; font-size: 1.5em; }
            .container { height: 500px; border-radius: 20px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="question" id="question">Gyanvi, Will you be mine valentine?</div>
        <div class="btn-group">
            <button class="yes-btn" id="yesBtn">Yes ❤️</button>
            <button class="no-btn" id="noBtn">No 😏</button>
        </div>
        <div class="success-message" id="successMsg">
            <div class="success-text">It's a date! 🎉</div>
            <img class="success-gif" src="https://media.giphy.com/media/Ju7l5y9osyymQ/giphy.gif" alt="It's a Date!">
        </div>
        <div class="hearts" id="hearts"></div>
    </div>

    <script>
        const question = document.getElementById('question');
        const yesBtn = document.getElementById('yesBtn');
        const noBtn = document.getElementById('noBtn');
        const successMsg = document.getElementById('successMsg');
        const hearts = document.getElementById('hearts');
        const container = document.querySelector('.container');
        let touchTimeout;

        function moveNoButton(e) {
            const rect = container.getBoundingClientRect();
            const maxX = rect.width - 140;
            const maxY = rect.height - 70;
            
            const clientX = e?.clientX || window.innerWidth / 2;
            const clientY = e?.clientY || window.innerHeight / 2;
            let dodgeX = (clientX - rect.left) / rect.width * maxX * 0.7;
            let dodgeY = (clientY - rect.top) / rect.height * maxY * 0.7;
            
            const newX = Math.max(10, Math.min(maxX - 10, (dodgeX + (Math.random() - 0.5) * maxX * 0.4)));
            const newY = Math.max(10, Math.min(maxY - 10, (dodgeY + (Math.random() - 0.5) * maxY * 0.4)));
            
            noBtn.style.left = newX + 'px';
            noBtn.style.top = newY + 'px';
            
            if (navigator.vibrate) navigator.vibrate(50);
        }

        // Enhanced touch + mouse events
        noBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            moveNoButton(e);
            clearTimeout(touchTimeout);
            touchTimeout = setTimeout(() => {}, 200);
        }, { passive: false });

        noBtn.addEventListener('mouseover', moveNoButton);

        yesBtn.addEventListener('click', () => {
            question.style.display = 'none';
            document.querySelector('.btn-group').style.display = 'none';
            successMsg.style.display = 'flex';
            for (let i = 0; i < 25; i++) {
                setTimeout(() => createHeart(), i * 80);
            }
        });

        function createHeart() {
            const heart = document.createElement('div');
            heart.className = 'heart';
            heart.innerHTML = ['❤️', '💖', '💕', '💝', '✨'][Math.floor(Math.random() * 5)];
            heart.style.left = Math.random() * 100 + '%';
            heart.style.animationDuration = (Math.random() * 2 + 2) + 's';
            hearts.appendChild(heart);
            hearts.style.display = 'block';
            setTimeout(() => heart.remove(), 4000);
        }
    </script>
</body>
</html>
