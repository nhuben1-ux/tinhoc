<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bài 5: Thao tác nháy đúp chuột - Tin học lớp 1</title>
    <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&family=Nunito:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #ff6b6b;
            --secondary: #4ecdc4;
            --accent: #ffe66d;
            --dark: #2d3436;
            --light: #f7f1e3;
            --blue: #0984e3;
            --green: #00b894;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Nunito', 'Comic Neue', sans-serif;
            background: linear-gradient(135deg, #a8ede0 0%, #fed6e3 100%);
            color: var(--dark);
            padding: 15px;
            min-height: 100vh;
        }

        .container {
            max-width: 950px;
            margin: 0 auto;
            background: #ffffff;
            border-radius: 30px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.15);
            overflow: hidden;
            border: 6px solid #ffffff;
        }

        header {
            background: linear-gradient(90deg, #ff9a9e 0%, #fecfef 99%, #fecfef 100%);
            padding: 25px;
            text-align: center;
            border-bottom: 6px dashed #ff6b6b;
            position: relative;
        }

        h1 {
            font-size: 2.4rem;
            color: #d63031;
            text-shadow: 3px 3px 0px #fff;
            margin-bottom: 12px;
            font-weight: 900;
        }

        .audio-btn {
            background: var(--blue);
            color: white;
            border: none;
            padding: 10px 22px;
            font-size: 1.15rem;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(9,132,227,0.3);
            transition: all 0.2s ease;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            font-weight: bold;
            margin-top: 5px;
        }

        .audio-btn:hover {
            transform: scale(1.06);
            background: #74b9ff;
        }

        section {
            padding: 25px 35px;
            border-bottom: 4px solid #f1f2f6;
        }

        .section-khoidong { background: #e3f2fd; }
        .section-mct { background: #fff3e0; }
        .section-kien-thuc { background: #e8f5e9; }
        .section-luyentap { background: #fce4ec; }
        .section-vandung { background: #f3e5f5; }
        .section-danhgia { background: #fffde7; }

        h2 {
            font-size: 1.8rem;
            color: #e17055;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 12px;
            font-weight: 900;
        }

        p, li {
            font-size: 1.25rem;
            line-height: 1.6;
            margin-bottom: 10px;
        }

        .mouse-animation-container {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
            gap: 30px;
            background: #ffffff;
            padding: 25px;
            border-radius: 20px;
            margin-top: 20px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.06);
            border: 3px solid #c8d6e5;
        }

        .svg-demo-box {
            width: 240px;
            height: 240px;
            background: linear-gradient(135deg, #f5f6fa, #dcdde1);
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: inset 0 4px 10px rgba(0,0,0,0.08);
            position: relative;
            overflow: hidden;
        }

        /* Keyframe animation for realistic double-click motion: 
           - Starts at rest position (0%)
           - Presses down 1st time (15% - 25%)
           - Returns to initial position / lifts up (35% - 45%)
           - Presses down 2nd time quickly (55% - 65%)
           - Returns to initial resting position (75% - 100%)
        */
        @keyframes preciseDoubleClick {
            0% { transform: translate(0, 0) rotate(0deg); }
            15% { transform: translate(2px, 8px) rotate(-3deg); }
            25% { transform: translate(2px, 8px) rotate(-3deg); }
            38% { transform: translate(0, 0) rotate(0deg); }
            45% { transform: translate(0, 0) rotate(0deg); }
            58% { transform: translate(2px, 8px) rotate(-3deg); }
            68% { transform: translate(2px, 8px) rotate(-3deg); }
            80% { transform: translate(0, 0) rotate(0deg); }
            100% { transform: translate(0, 0) rotate(0deg); }
        }

        @keyframes rippleEffect {
            0% { r: 0; opacity: 1; }
            50% { r: 15px; opacity: 0.5; }
            100% { r: 25px; opacity: 0; }
        }

        .animated-clicking-finger {
            animation: preciseDoubleClick 2.2s infinite ease-in-out;
            transform-origin: 120px 80px;
        }

        .click-ripple-1 {
            animation: rippleEffect 2.2s infinite ease-out;
            animation-delay: 0.3s;
            transform-origin: 75px 75px;
        }

        .click-ripple-2 {
            animation: rippleEffect 2.2s infinite ease-out;
            animation-delay: 1.3s;
            transform-origin: 75px 75px;
        }

        .explanation-text {
            flex: 1;
            min-width: 280px;
        }

        .interactive-click-area {
            background: linear-gradient(135deg, #ffeaa7, #fab1a0);
            border: 4px dashed #d63031;
            border-radius: 20px;
            padding: 25px;
            text-align: center;
            cursor: pointer;
            margin-top: 20px;
            user-select: none;
            transition: all 0.15s ease;
            box-shadow: 0 6px 15px rgba(214,48,49,0.15);
        }

        .interactive-click-area:active {
            transform: scale(0.97);
            background: linear-gradient(135deg, #fab1a0, #ffeaa7);
        }

        .interactive-click-area h3 {
            font-size: 1.6rem;
            color: #d63031;
            margin-bottom: 8px;
            font-weight: 900;
        }

        .quiz-box, .app-box {
            background: white;
            padding: 22px;
            border-radius: 18px;
            margin-top: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            border: 2px solid #dfe6e9;
        }

        .options-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .opt-btn {
            background: #74b9ff;
            color: white;
            border: none;
            padding: 16px;
            font-size: 1.15rem;
            border-radius: 14px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.2s;
            box-shadow: 0 5px 0 #0984e3;
        }

        .opt-btn:hover { background: #0984e3; }
        .opt-btn:active { transform: translateY(5px); box-shadow: none; }
        
        .opt-btn.correct { background: var(--green) !important; box-shadow: 0 5px 0 #006644 !important; }
        .opt-btn.incorrect { background: var(--primary) !important; box-shadow: 0 5px 0 #8b0000 !important; }

        .feedback-msg {
            margin-top: 12px;
            font-weight: bold;
            font-size: 1.25rem;
            min-height: 35px;
        }

        .app-icons {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 20px;
            flex-wrap: wrap;
        }

        .app-icon-item {
            text-align: center;
            cursor: pointer;
            padding: 18px;
            background: white;
            border-radius: 18px;
            border: 3px solid #dfe6e9;
            width: 160px;
            transition: all 0.2s ease;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }

        .app-icon-item:hover {
            border-color: #e17055;
            transform: translateY(-5px) scale(1.04);
            box-shadow: 0 8px 20px rgba(0,0,0,0.1);
        }

        .eval-list {
            list-style: none;
            margin-top: 15px;
        }

        .eval-item {
            background: white;
            padding: 14px 22px;
            border-radius: 14px;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border: 3px solid #ffeaa7;
            font-weight: bold;
            box-shadow: 0 3px 10px rgba(0,0,0,0.03);
        }

        .checkbox-custom {
            width: 35px;
            height: 35px;
            border: 3px solid #fdcb6e;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.3rem;
            background: #fff;
            transition: all 0.2s ease;
        }

        .checkbox-custom.checked {
            background: var(--green);
            border-color: var(--green);
            color: white;
            transform: scale(1.1);
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>🌟 BÀI 5: THAO TÁC NHÁY ĐÚP CHUỘT 🌟</h1>
        <button class="audio-btn" onclick="speakText('Chào mừng các em đến với bài học Thao tác nháy đúp chuột. Hãy cùng học và chơi nhé!')">
            🔊 Nghe giới thiệu bài học
        </button>
    </header>

    <!-- 1. KHỞI ĐỘNG -->
    <section class="section-khoidong">
        <h2>🚀 1. Khởi động cùng chú chuột thông minh</h2>
        <button class="audio-btn" onclick="speakText('Phần khởi động. Các em hãy giúp chú chuột tìm đường đến màn hình máy tính bằng cách trả lời câu hỏi nhé!')">🔊 Nghe hướng dẫn</button>
        <div class="quiz-box">
            <p><strong>Câu hỏi:</strong> Khi em muốn mở một chương trình trên máy tính nhanh chóng, em dùng bộ phận nào để điều khiển?</p>
            <div class="options-grid">
                <button class="opt-btn" onclick="checkQuiz(this, false)">A. Bàn phím</button>
                <button class="opt-btn" onclick="checkQuiz(this, true)">B. Con chuột máy tính</button>
                <button class="opt-btn" onclick="checkQuiz(this, false)">C. Loa máy tính</button>
            </div>
            <div class="feedback-msg" id="quiz-feedback"></div>
        </div>
    </section>

    <!-- 2. MỤC TIÊU BÀI HỌC -->
    <section class="section-mct">
        <h2>🎯 2. Mục tiêu bài học</h2>
        <button class="audio-btn" onclick="speakText('Học xong bài này, em sẽ: Biết được thao tác nháy đúp chuột. Biết vận dụng thao tác nháy đúp chuột để làm việc với máy tính.')">🔊 Nghe mục tiêu</button>
        <ul style="margin-left: 25px; margin-top: 10px;">
            <li>Biết được thao tác <strong>nháy đúp chuột</strong>.</li>
            <li>Biết vận dụng thao tác nháy đúp chuột để làm việc với máy tính.</li>
        </ul>
    </section>

    <!-- 3. KIẾN THỨC MỚI & HOẠT HỌA 3D SVG THAO TÁC ĐÚP CHUỘT -->
    <section class="section-kien-thuc">
        <h2>💡 3. Kiến thức mới: Thao tác nháy đúp chuột</h2>
        <button class="audio-btn" onclick="speakText('Kiến thức mới. Dùng ngón trỏ nhấn nhanh hai lần liên tiếp vào nút trái chuột rồi thả ra.')">🔊 Nghe lý thuyết</button>
        <p style="margin-top: 10px;">Dùng ngón trỏ <strong>nhấn nhanh hai lần liên tiếp vào nút trái chuột rồi thả tay về vị trí ban đầu</strong>.</p>
        
        <div class="mouse-animation-container">
            <!-- Trực quan hóa hoạt họa SVG mô phỏng bàn tay cầm chuột và ngón trỏ nháy đúp rồi thả về vị trí ban đầu -->
            <div class="svg-demo-box">
                <svg width="200" height="200" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
                    <!-- Bàn di chuột / mặt bàn -->
                    <ellipse cx="100" cy="150" rx="75" ry="22" fill="#dfe4ea" opacity="0.7"/>
                    
                    <!-- Thân chuột 3D -->
                    <path d="M 60 75 C 60 40 140 40 140 75 L 140 135 C 140 165 60 165 60 135 Z" fill="url(#mouseGrad)" stroke="#2f3542" stroke-width="3"/>
                    <defs>
                        <linearGradient id="mouseGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                            <stop offset="0%" stop-color="#70a1ff" />
                            <stop offset="100%" stop-color="#1e90ff" />
                        </linearGradient>
                        <linearGradient id="leftBtnGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                            <stop offset="0%" stop-color="#ff4757" />
                            <stop offset="100%" stop-color="#ff6b81" />
                        </linearGradient>
                    </defs>
                    
                    <!-- Nút chuột trái -->
                    <path d="M 62 75 C 62 48 100 48 100 48 L 100 120 C 75 120 62 100 62 75 Z" fill="url(#leftBtnGrad)" stroke="#2f3542" stroke-width="2.5"/>
                    <!-- Nút cuộn -->
                    <rect x="96" y="58" width="8" height="26" rx="4" fill="#ced6e0" stroke="#2f3542" stroke-width="2"/>
                    
                    <!-- Hiệu ứng gợn sóng nháy chuột lần 1 và lần 2 -->
                    <circle class="click-ripple-1" cx="78" cy="75" r="0" fill="none" stroke="#2ed573" stroke-width="3"/>
                    <circle class="click-ripple-2" cx="78" cy="75" r="0" fill="none" stroke="#2ed573" stroke-width="3"/>

                    <!-- Bàn tay & Ngón trỏ hoạt họa thực hiện nhấn đúp rồi thả về vị trí ban đầu -->
                    <g class="animated-clicking-finger">
                        <!-- Lòng bàn tay & ngón cái -->
                        <path d="M 115 110 Q 145 110 155 142 Q 160 162 140 168 Q 115 152 105 132 Z" fill="#ffb8b8" stroke="#d63031" stroke-width="2"/>
                        <!-- Ngón trỏ cầm chuột -->
                        <path d="M 98 48 Q 80 25 70 42 Q 65 55 82 78 Z" fill="#ffcccc" stroke="#d63031" stroke-width="2.5"/>
                    </g>

                    <!-- Nhãn hướng dẫn x2 -->
                    <text x="115" y="45" font-family="Arial" font-size="15" font-weight="bold" fill="#00b894">Nhấn x2</text>
                </svg>
            </div>
            
            <div class="explanation-text">
                <p style="color: #2d3436; font-size: 1.15rem;">👉 <strong>Mô phỏng chân thực:</strong> Ngón trỏ đặt lên nút trái, nhấn nhanh xuống 2 lần liên tiếp (cốc! cốc!) rồi tự động thả ngón tay về vị trí ban đầu như khi em cầm chuột thực tế.</p>
                <p style="color: #0984e3; font-size: 1.1rem; margin-top: 8px;">✨ <em>Bé hãy quan sát hình động để làm theo đúng nhịp nhé!</em></p>
            </div>
        </div>

        <!-- Khu vực tập bấm thực tế -->
        <div class="interactive-click-area" id="click-area" onclick="handleDoubleClickSimulation(event)">
            <h3>🖲️ KHU VỰC THỰC HÀNH NHÁY ĐÚP CHUỘT</h3>
            <p id="click-status" style="font-size: 1.15rem; color: #2d3436;">Hãy nháy đúp (nhấn chuột 2 lần thật nhanh liên tiếp) vào ô này để kiểm tra!</p>
        </div>
    </section>

    <!-- 4. LUYỆN TẬP -->
    <section class="section-luyentap">
        <h2>✏️ 4. Luyện tập</h2>
        <button class="audio-btn" onclick="speakText('Phần luyện tập. Thao tác nhấn nhanh hai lần liên tiếp vào nút trái chuột rồi thả ra là thao tác gì? A, Nháy chuột. B, Nháy đúp chuột. C, Kéo thả chuột.')">🔊 Nghe câu hỏi luyện tập</button>
        <div class="quiz-box">
            <p><strong>Thao tác nhấn nhanh hai lần liên tiếp vào nút trái chuột rồi thả ra là thao tác:</strong></p>
            <div class="options-grid">
                <button class="opt-btn" onclick="checkPractice(this, false, 'pra-fb1')">A. Nháy chuột</button>
                <button class="opt-btn" onclick="correctPractice('pra-fb1', this)">B. Nháy đúp chuột</button>
                <button class="opt-btn" onclick="checkPractice(this, false, 'pra-fb1')">C. Kéo thả chuột</button>
            </div>
            <div class="feedback-msg" id="pra-fb1"></div>
        </div>
    </section>

    <!-- 5. VẬN DỤNG -->
    <section class="section-vandung">
        <h2>🌟 5. Vận dụng thực tế</h2>
        <button class="audio-btn" onclick="speakText('Phần vận dụng. Sử dụng thao tác nháy đúp chuột để mở các chương trình: Paint hoặc Google Chrome. Hãy bấm vào các biểu tượng dưới đây để mở thử nhé!')">🔊 Nghe hướng dẫn</button>
        <p>Sử dụng thao tác nháy đúp chuột để mở các chương trình sau:</p>
        
        <div class="app-icons">
            <div class="app-icon-item" onclick="openApp('Phần mềm vẽ Paint')">
                <svg width="60" height="60" viewBox="0 0 24 24" fill="none" stroke="#e17055" stroke-width="2.2"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>
                <p style="font-weight: bold; margin-top: 8px;">A. Paint</p>
                <span style="font-size: 0.9rem; color: #0984e3; display:inline-block; margin-top:4px;">(Nháy đúp để mở)</span>
            </div>
            
            <div class="app-icon-item" onclick="openApp('Trình duyệt Google Chrome')">
                <svg width="60" height="60" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="#00b894" stroke-width="2.2"/><circle cx="12" cy="12" r="3.5" fill="#0984e3"/><path d="M12 8.5h9.5M4.8 15.2l4.8-8.3M19.2 15.2l-6.8-11.8" stroke="#fdcb6e" stroke-width="2.2"/></svg>
                <p style="font-weight: bold; margin-top: 8px;">B. Google Chrome</p>
                <span style="font-size: 0.9rem; color: #0984e3; display:inline-block; margin-top:4px;">(Nháy đúp để mở)</span>
            </div>
        </div>
        <div class="feedback-msg" id="app-feedback" style="text-align: center; color: var(--green); margin-top: 15px;"></div>
    </section>

    <!-- 6. ĐÁNH GIÁ -->
    <section class="section-danhgia">
        <h2>⭐ 6. Đánh giá sau bài học</h2>
        <button class="audio-btn" onclick="speakText('Phần đánh giá. Sau bài học, em đã biết các thao tác: Thao tác nháy đúp chuột. Khởi động mở chương trình. Thoát khỏi chương trình. Hãy bấm vào các ô tròn để tự đánh giá nhé!')">🔊 Nghe đánh giá</button>
        <p>Sau bài học, em biết:</p>
        
        <ul class="eval-list">
            <li class="eval-item">
                <span>1. Thao tác nháy đúp chuột</span>
                <div class="checkbox-custom" onclick="toggleCheck(this)">✓</div>
            </li>
            <li class="eval-item">
                <span>2. Khởi động (mở) chương trình</span>
                <div class="checkbox-custom" onclick="toggleCheck(this)">✓</div>
            </li>
            <li class="eval-item">
                <span>3. Thoát khỏi chương trình</span>
                <div class="checkbox-custom" onclick="toggleCheck(this)">✓</div>
            </li>
        </ul>
    </section>
</div>

<script>
    // Web Speech API helper function for female Vietnamese speech guide
    function speakText(text) {
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'vi-VN';
            utterance.rate = 0.9;
            
            const voices = window.speechSynthesis.getVoices();
            const viVoice = voices.find(v => v.lang.includes('vi') || v.lang.includes('VN'));
            if (viVoice) {
                utterance.voice = viVoice;
            }
            window.speechSynthesis.speak(utterance);
        } else {
            console.log('Speech synthesis not supported.');
        }
    }

    // Quiz evaluation logic
    function checkQuiz(button, isCorrect) {
        const fb = document.getElementById('quiz-feedback');
        const parent = button.parentElement;
        parent.querySelectorAll('.opt-btn').forEach(b => b.classList.remove('correct', 'incorrect'));
        
        if (isCorrect) {
            button.classList.add('correct');
            fb.style.color = 'var(--green)';
            fb.innerHTML = '🎉 Chính xác! Bé thật thông minh!';
            speakText('Chính xác! Bé thật thông minh!');
        } else {
            button.classList.add('incorrect');
            fb.style.color = 'var(--primary)';
            fb.innerHTML = '❌ Chưa đúng lắm, em thử chọn lại nhé!';
            speakText('Chưa đúng lắm, em thử chọn lại nhé!');
        }
    }

    function correctPractice(feedbackId, button) {
        const fb = document.getElementById(feedbackId);
        button.parentElement.querySelectorAll('.opt-btn').forEach(b => b.classList.remove('correct', 'incorrect'));
        button.classList.add('correct');
        fb.style.color = 'var(--green)';
        fb.innerHTML = '🌟 Đúng rồi! Đây chính là thao tác nháy đúp chuột!';
        speakText('Đúng rồi! Đây chính là thao tác nháy đúp chuột!');
    }

    function checkPractice(button, isCorrect, feedbackId) {
        const fb = document.getElementById(feedbackId);
        button.parentElement.querySelectorAll('.opt-btn').forEach(b => b.classList.remove('correct', 'incorrect'));
        button.classList.add('incorrect');
        fb.style.color = 'var(--primary)';
        fb.innerHTML = '❌ Chưa chính xác, đáp án đúng là Nháy đúp chuột em nhé!';
        speakText('Chưa chính xác, đáp án đúng là Nháy đúp chuột em nhé!');
    }

    // Interactive double click practice simulator
    let lastClickTime = 0;
    function handleDoubleClickSimulation(event) {
        const currentTime = new Date().getTime();
        const statusEl = document.getElementById('click-status');
        
        if (currentTime - lastClickTime < 500 && currentTime - lastClickTime > 50) {
            statusEl.innerHTML = '🎉 TUYỆT VỜI! Bé đã nháy đúp chuột thành công!';
            statusEl.style.color = 'var(--green)';
            speakText('Tuyệt vời! Bé đã nháy đúp chuột thành công!');
        } else {
            statusEl.innerHTML = '⚡ Đã nháy 1 lần! Hãy nháy nhanh thêm 1 lần nữa nhé (cốc cốc)!';
            statusEl.style.color = '#e17055';
            speakText('Đã nháy một lần! Hãy nháy nhanh thêm một lần nữa nhé!');
        }
        lastClickTime = currentTime;
    }

    // Application simulation
    function openApp(appName) {
        const fb = document.getElementById('app-feedback');
        fb.innerHTML = `🚀 Đã mở thành công chương trình <strong>${appName}</strong> bằng thao tác nháy đúp chuột!`;
        speakText(`Đã mở thành công chương trình ${appName} bằng thao tác nháy đúp chuột!`);
    }

    // Evaluation checkboxes toggle
    function toggleCheck(element) {
        element.classList.toggle('checked');
        if (element.classList.contains('checked')) {
            element.innerHTML = '✔';
            speakText('Giỏi lắm! Em đã hoàn thành tốt nội dung này.');
        } else {
            element.innerHTML = '✓';
        }
    }
</script>

</body>
</html>
