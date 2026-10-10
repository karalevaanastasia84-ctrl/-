<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Gaps Universal Quantum — Цифровой Оазис</title>
    <style>
        body {
            background-color: #0d1b2a;
            color: #e0e1dd;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        .container {
            text-align: center;
            max-width: 650px;
            background: rgba(27, 38, 59, 0.4);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }
        h2 { color: #62b6cb; margin-bottom: 5px; }
        .subtitle { color: #8d99ae; font-style: italic; margin-bottom: 25px; }
        .manifest-text, .steps-text {
            text-align: justify;
            line-height: 1.6;
            font-size: 15px;
            margin-bottom: 20px;
            color: #e0e1dd;
        }
        .steps-text b { color: #e9c46a; }
        #canvas-zone {
            width: 320px;
            height: 320px;
            border: 2px solid #415a77;
            border-radius: 15px;
            margin: 25px auto;
            background: radial-gradient(circle, #1b263b 0%, #0d1b2a 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.5s ease;
            font-weight: bold;
            color: #e0e1dd;
            text-align: center;
            padding: 10px;
            box-sizing: border-box;
        }
        @keyframes quantumBreathe {
            0% { transform: scale(1.0); box-shadow: 0 0 15px rgba(98, 182, 203, 0.2); }
            50% { transform: scale(1.06); box-shadow: 0 0 35px rgba(141, 153, 174, 0.5); }
            100% { transform: scale(1.0); box-shadow: 0 0 15px rgba(98, 182, 203, 0.2); }
        }
        .breathe-active {
            animation: quantumBreathe 10s infinite ease-in-out;
        }
        button {
            background-color: #1b4965;
            color: white;
            border: none;
            padding: 15px 35px;
            font-size: 18px;
            border-radius: 25px;
            cursor: pointer;
            transition: background 0.3s, transform 0.2s;
            margin-top: 10px;
        }
        button:hover { background-color: #62b6cb; transform: scale(1.05); }
        button:disabled { background-color: #415a77; cursor: not-allowed; transform: none; }
        .timer { font-size: 26px; color: #e9c46a; margin-top: 15px; font-weight: bold; }
    </style>
</head>
<body>
    <div class="container">
        <h2>Gaps Universal Quantum</h2>
        <div class="subtitle">Пространство сонастройки и природного баланса</div>
        
        <div class="manifest-text">
            Этот проект родился на стыке человеческой интуиции и цифровой гармонии. Мы заглянули в пустоту между числами и обнаружили, что хаоса не существует. Всё в нашем мире — от структуры ДНК до ритмов природы — стремится к балансу. Здесь вы можете замедлить бег мыслей и соприкоснуться с этой скрытой тишиной.
        </div>

        <div class="steps-text">
            <b>Шаг 1. Настройка.</b> Сделайте глоток воды. Установите тихий, комфортный для вас уровень звука.<br>
            <b>Шаг 2. Погружение (3 минуты).</b> Нажмите кнопку ниже. Окно гармонии начнет мягко пульсировать, а динамик выдаст природный ритм. Расслабьте взгляд и просто следуйте за окном: плавно вдыхайте, когда оно расширяется, и мягко выдыхайте, когда оно сужается.<br>
            <b>Шаг 3. Время тишины (71 секунда).</b> Ровно через 3 минуты звук полностью отключится, а экран погаснет. В этот момент закройте глаза и побудьте в полной темноте. Это даст вашим глазам и уставшей нервной системе идеальный, глубокий отдых.
        </div>
        
        <div id="canvas-zone">
            <span id="status-text">Окно гармонии</span>
        </div>
        
        <button id="start-btn" onclick="startQuantumSession()">Прикоснуться к тишине</button>
        <div id="time-display" class="timer"></div>
    </div>

    <script>
        let audioCtx = null;
        let oscillator = null;
        let gainNode = null;

        function startQuantumSession() {
            try {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                oscillator = audioCtx.createOscillator();
                gainNode = audioCtx.createGain();
                
                oscillator.type = 'sine'; 
                oscillator.frequency.setValueAtTime(432, audioCtx.currentTime); 
                
                gainNode.gain.setValueAtTime(0.04, audioCtx.currentTime); 
                oscillator.connect(gainNode);
                gainNode.connect(audioCtx.destination);
                oscillator.start();
                
                // Ритм пульсации строго каждые 1666 мс
                let pulse = true;
                window.pulseInterval = setInterval(() => {
                    if(gainNode) {
                        gainNode.gain.setTargetAtTime(pulse ? 0.07 : 0.01, audioCtx.currentTime, 0.4);
                        document.getElementById('canvas-zone').style.background = pulse ? "radial-gradient(circle, #1b4965 0%, #0d1b2a 100%)" : "radial-gradient(circle, #1b263b 0%, #0d1b2a 100%)";
                        pulse = !pulse;
                    }
                }, 1666); 
                
            } catch(e) {
                console.log("Аудио не поддерживается");
            }

            document.getElementById('canvas-zone').classList.add('breathe-active');
            document.getElementById('status-text').innerText = "Мягко следуйте за дыханием окна...";
            document.getElementById('canvas-zone').style.borderColor = "#62b6cb";
            document.getElementById('start-btn').disabled = true;
            
            let timeLeft = 180; 
            let timerAnimate = setInterval(function() {
                let minutes = Math.floor(timeLeft / 60);
                let seconds = timeLeft % 60;
                document.getElementById('time-display').innerText = minutes + ":" + (seconds < 10 ? "0" : "") + seconds;
                
                timeLeft--;
                if (timeLeft < 0) {
                    clearInterval(timerAnimate);
                    clearInterval(window.pulseInterval);
                    
                    if (oscillator) { oscillator.stop(); }
                    document.getElementById('canvas-zone').classList.remove('breathe-active');
                    
                    // Фаза тишины
                    document.getElementById('status-text').innerText = "А теперь закройте глаза и побудьте в темноте.";
                    document.getElementById('canvas-zone').style.borderColor = "#000000";
                    document.getElementById('canvas-zone').style.background = "#000000"; 
                    
                    let voidLeft = 71;
                    let voidTimer = setInterval(function() {
                        document.getElementById('time-display').innerText = "Отдых: " + voidLeft + "с";
                        voidLeft--;
                        if (voidLeft < 0) {
                            clearInterval(voidTimer);
                            if (audioCtx) { audioCtx.close(); }
                            document.getElementById('status-text').innerText = "Гармонизация завершена. Вы можете открыть глаза.";
                            document.getElementById('time-display').innerText = "";
                            document.getElementById('canvas-zone').style.borderColor = "#415a77";
                            document.getElementById('canvas-zone').style.background = "radial-gradient(circle, #1b263b 0%, #0d1b2a 100%)";
                            document.getElementById('start-btn').disabled = false;
                        }
                    }, 1000);
                }
            }, 1000);
        }
    </script>
</body>
</html>
