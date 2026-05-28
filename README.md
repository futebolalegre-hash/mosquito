<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>簡易蚊よけ音ジェネレーター</title>
    <style>
        body { font-family: sans-serif; text-align: center; padding: 50px; background: #f0f8ff; }
        button { font-size: 20px; padding: 15px 30px; margin: 10px; cursor: pointer; border-radius: 10px; border: none; }
        #start { background: #4CAF50; color: white; }
        #stop { background: #f44336; color: white; }
        .slider-container { margin: 30px 0; }
        input[type="range"] { width: 80%; }
        p { color: #555; }
    </style>
</head>
<body>
    <h1>蚊よけ音 試作機</h1>
    <p>15kHz〜20kHzの高周波を出します</p>
    
    <div class="slider-container">
        <label>周波数: <span id="freq-val">17000</span> Hz</label><br>
        <input type="range" id="freq" min="15000" max="20000" step="100" value="17000">
    </div>

    <button id="start">音を出す</button>
    <button id="stop">止める</button>

    <script>
        let audioCtx = null;
        let oscillator = null;

        const freqSlider = document.getElementById('freq');
        const freqDisp = document.getElementById('freq-val');

        freqSlider.oninput = function() {
            freqDisp.innerText = this.value;
            if (oscillator) {
                oscillator.frequency.setValueAtTime(this.value, audioCtx.currentTime);
            }
        };

        document.getElementById('start').onclick = function() {
            if (audioCtx) return; // 二重再生防止
            
            audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            oscillator = audioCtx.createOscillator();
            
            oscillator.type = 'sine'; // 滑らかなサイン波
            oscillator.frequency.setValueAtTime(freqSlider.value, audioCtx.currentTime);
            
            oscillator.connect(audioCtx.destination);
            oscillator.start();
        };

        document.getElementById('stop').onclick = function() {
            if (oscillator) {
                oscillator.stop();
                oscillator.disconnect();
                oscillator = null;
                audioCtx = null;
            }
        };
    </script>
</body>
</html>
