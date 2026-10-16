<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PRO1 Student Hub</title>
    <!-- خطوط جوجل وأيقونات -->
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        /* === الإعدادات العامة والتصميم الأساسي === */
        :root {
            --bg-color: #0f172a;
            --surface: #1e293b;
            --primary: #0ea5e9;
            --secondary: #8b5cf6;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --danger: #ef4444;
            --success: #22c55e;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Tajawal', sans-serif; }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        /* === شريط التنقل السفلي === */
        .navbar {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(10px);
            border-top: 1px solid rgba(255,255,255,0.1);
            display: flex;
            justify-content: space-around;
            padding: 15px 0;
            position: fixed;
            bottom: 0;
            width: 100%;
            z-index: 100;
        }

        .nav-item {
            color: var(--text-muted);
            font-size: 1.5rem;
            cursor: pointer;
            transition: 0.3s;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }

        .nav-item span { font-size: 0.8rem; font-weight: 700; }
        .nav-item.active { color: var(--primary); transform: translateY(-5px); text-shadow: 0 0 15px var(--primary); }

        /* === حاوية المحتوى (التبويبات) === */
        .content-area {
            flex: 1;
            padding: 20px;
            padding-bottom: 80px; /* لترك مساحة للشريط السفلي */
            overflow-y: auto;
        }

        .tab-content {
            display: none;
            animation: fadeIn 0.4s ease-out forwards;
        }
        .tab-content.active { display: block; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* === العناوين === */
        .header {
            text-align: center;
            margin-bottom: 30px;
            padding-top: 20px;
        }
        .header h1 {
            font-weight: 900;
            font-size: 2.2rem;
            background: linear-gradient(to left, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* === قسم المؤقت الدائري === */
        .timer-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 20px;
        }
        
        .circle-timer {
            position: relative;
            width: 250px;
            height: 250px;
            margin-bottom: 30px;
        }

        .circle-timer svg { width: 100%; height: 100%; transform: rotate(-90deg); }
        .circle-timer circle {
            fill: none;
            stroke-width: 8;
            stroke-linecap: round;
            cx: 125; cy: 125; r: 115;
        }
        .circle-bg { stroke: var(--surface); }
        .circle-progress {
            stroke: var(--primary);
            stroke-dasharray: 722; /* محيط الدائرة (2 * pi * r) */
            stroke-dashoffset: 0;
            transition: stroke-dashoffset 1s linear;
            filter: drop-shadow(0 0 8px var(--primary));
        }

        .time-display {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            font-size: 3.5rem;
            font-weight: 900;
        }

        .controls button {
            background: var(--surface);
            color: var(--text-main);
            border: 1px solid rgba(255,255,255,0.1);
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 1.2rem;
            margin: 0 10px;
            cursor: pointer;
            transition: 0.3s;
        }
        .controls button:hover { background: var(--primary); border-color: var(--primary); box-shadow: 0 0 15px var(--primary); }

        /* === قسم المهام (To-Do) === */
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        input[type="text"] {
            flex: 1;
            padding: 15px;
            background: var(--surface);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 12px;
            color: white;
            font-size: 1rem;
            outline: none;
        }
        input[type="text"]:focus { border-color: var(--primary); }
        
        .add-btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 0 25px;
            border-radius: 12px;
            font-size: 1.2rem;
            cursor: pointer;
            transition: 0.3s;
        }
        .add-btn:hover { background: var(--secondary); }

        .task-list { list-style: none; }
        .task-item {
            background: var(--surface);
            padding: 15px;
            border-radius: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            border-left: 4px solid var(--primary);
            transition: 0.3s;
        }
        .task-item.completed { border-left-color: var(--success); opacity: 0.7; }
        .task-item.completed .task-text { text-decoration: line-through; color: var(--text-muted); }
        .task-text { flex: 1; cursor: pointer; font-size: 1.1rem; }
        .del-btn { color: var(--danger); background: none; border: none; font-size: 1.2rem; cursor: pointer; }

        /* === قسم بطاقات السيمنار (3D Flashcards) === */
        .flashcard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        /* تأثير الـ 3D للبطاقة */
        .card-scene {
            perspective: 1000px; /* العمق */
            height: 200px;
        }
        .card {
            width: 100%;
            height: 100%;
            position: relative;
            transition: transform 0.6s;
            transform-style: preserve-3d; /* يحافظ على شكل الوجوه */
            cursor: pointer;
        }
        .card.is-flipped {
            transform: rotateY(180deg);
        }
        
        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden; /* يخفي الظهر لما ينقلب */
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            text-align: center;
            font-size: 1.2rem;
            font-weight: 700;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            border: 1px solid rgba(255,255,255,0.05);
        }
        
        .card-front {
            background: linear-gradient(135deg, var(--surface), #2a3b52);
            color: var(--primary);
        }
        
        .card-back {
            background: linear-gradient(135deg, var(--secondary), #6d28d9);
            color: white;
            transform: rotateY(180deg);
        }

        .delete-card {
            position: absolute;
            top: 10px;
            right: 10px;
            color: rgba(255,255,255,0.5);
            background: none;
            border: none;
            z-index: 10;
            font-size: 1.2rem;
        }
        .delete-card:hover { color: var(--danger); }

    </style>
</head>
<body>

    <!-- العناوين العلوية -->
    <div class="header">
        <h1>PRO1 Hub</h1>
        <p>مساحتك الذكية للتفوق والتركيز</p>
    </div>

    <!-- منطقة المحتوى -->
    <div class="content-area">
        
        <!-- التبويب 1: المهام -->
        <div id="tasksTab" class="tab-content active">
            <div class="input-group">
                <input type="text" id="taskInput" placeholder="شنو مخطط تدرس اليوم؟ (مثال: قراءة ملزمة الإحصاء)">
                <button class="add-btn" onclick="addTask()"><i class="fas fa-plus"></i></button>
            </div>
            <ul class="task-list" id="taskList">
                <!-- المهام تضاف هنا برمجياً -->
            </ul>
        </div>

        <!-- التبويب 2: المؤقت -->
        <div id="timerTab" class="tab-content">
            <div class="timer-container">
                <div class="circle-timer">
                    <svg>
                        <circle class="circle-bg"></circle>
                        <circle class="circle-progress" id="progressCircle"></circle>
                    </svg>
                    <div class="time-display" id="timeDisplay">25:00</div>
                </div>
                <div class="controls">
                    <button id="startTimerBtn" onclick="toggleTimer()"><i class="fas fa-play"></i> تشغيل</button>
                    <button onclick="resetTimer()"><i class="fas fa-redo"></i> إعادة</button>
                </div>
            </div>
        </div>

        <!-- التبويب 3: البطاقات -->
        <div id="cardsTab" class="tab-content">
            <div class="input-group">
                <input type="text" id="frontInput" placeholder="المصطلح الإنجليزي (السؤال)">
                <input type="text" id="backInput" placeholder="الترجمة العربية (الجواب)">
                <button class="add-btn" onclick="addCard()"><i class="fas fa-save"></i></button>
            </div>
            <div class="flashcard-grid" id="cardGrid">
                <!-- البطاقات تضاف هنا برمجياً -->
            </div>
        </div>

    </div>

    <!-- شريط التنقل السفلي -->
    <nav class="navbar">
        <div class="nav-item active" onclick="switchTab('tasksTab', this)">
            <i class="fas fa-list-check"></i>
            <span>المهام</span>
        </div>
        <div class="nav-item" onclick="switchTab('timerTab', this)">
            <i class="fas fa-stopwatch"></i>
            <span>المؤقت</span>
        </div>
        <div class="nav-item" onclick="switchTab('cardsTab', this)">
            <i class="fas fa-layer-group"></i>
            <span>البطاقات</span>
        </div>
    </nav>

    <!-- الجافاسكربت (المنطق البرمجي) -->
    <script>
        // === 1. نظام التنقل بين التبويبات ===
        function switchTab(tabId, element) {
            // إخفاء كل التبويبات
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
            // إزالة التفعيل من كل الأزرار
            document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
            
            // تفعيل التبويب المطلوب والزر الخاص به
            document.getElementById(tabId).classList.add('active');
            element.classList.add('active');
        }


        // === 2. نظام المهام (To-Do List) ===
        let tasks = JSON.parse(localStorage.getItem('pro1_tasks')) || [];

        function renderTasks() {
            const list = document.getElementById('taskList');
            list.innerHTML = '';
            tasks.forEach((task, index) => {
                list.innerHTML += `
                    <li class="task-item ${task.completed ? 'completed' : ''}">
                        <div class="task-text" onclick="toggleTask(${index})">${task.text}</div>
                        <button class="del-btn" onclick="deleteTask(${index})"><i class="fas fa-trash"></i></button>
                    </li>
                `;
            });
            localStorage.setItem('pro1_tasks', JSON.stringify(tasks));
        }

        function addTask() {
            const input = document.getElementById('taskInput');
            if (input.value.trim() !== '') {
                tasks.push({ text: input.value, completed: false });
                input.value = '';
                renderTasks();
            }
        }

        function toggleTask(index) {
            tasks[index].completed = !tasks[index].completed;
            renderTasks();
        }

        function deleteTask(index) {
            tasks.splice(index, 1);
            renderTasks();
        }


        // === 3. نظام بطاقات السيمنار (Flashcards) ===
        let cards = JSON.parse(localStorage.getItem('pro1_cards')) || [];

        function renderCards() {
            const grid = document.getElementById('cardGrid');
            grid.innerHTML = '';
            cards.forEach((card, index) => {
                grid.innerHTML += `
                    <div class="card-scene">
                        <div class="card" onclick="this.classList.toggle('is-flipped')">
                            <div class="card-face card-front">
                                ${card.front}
                                <button class="delete-card" onclick="event.stopPropagation(); deleteCard(${index})"><i class="fas fa-times"></i></button>
                            </div>
                            <div class="card-face card-back">
                                ${card.back}
                            </div>
                        </div>
                    </div>
                `;
            });
            localStorage.setItem('pro1_cards', JSON.stringify(cards));
        }

        function addCard() {
            const front = document.getElementById('frontInput');
            const back = document.getElementById('backInput');
            if (front.value.trim() !== '' && back.value.trim() !== '') {
                cards.push({ front: front.value, back: back.value });
                front.value = '';
                back.value = '';
                renderCards();
            }
        }

        function deleteCard(index) {
            cards.splice(index, 1);
            renderCards();
        }


        // === 4. نظام المؤقت الدائري ===
        let totalTime = 25 * 60; // 25 دقيقة بالثواني
        let timeLeft = totalTime;
        let timerId = null;
        let isRunning = false;
        
        const circle = document.getElementById('progressCircle');
        const timeDisplay = document.getElementById('timeDisplay');
        const startBtn = document.getElementById('startTimerBtn');
        const circumference = 722; // محيط الدائرة المحسوب بالـ CSS

        function formatTime(seconds) {
            const m = Math.floor(seconds / 60).toString().padStart(2, '0');
            const s = (seconds % 60).toString().padStart(2, '0');
            return `${m}:${s}`;
        }

        function updateCircle() {
            const offset = circumference - (timeLeft / totalTime) * circumference;
            circle.style.strokeDashoffset = offset;
            timeDisplay.textContent = formatTime(timeLeft);
        }

        function toggleTimer() {
            if (isRunning) {
                clearInterval(timerId);
                startBtn.innerHTML = '<i class="fas fa-play"></i> إكمال';
                isRunning = false;
            } else {
                timerId = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateCircle();
                    } else {
                        clearInterval(timerId);
                        alert("انتهى وقت الدراسة! خذ استراحة.");
                        resetTimer();
                    }
                }, 1000);
                startBtn.innerHTML = '<i class="fas fa-pause"></i> إيقاف مؤقت';
                isRunning = true;
            }
        }

        function resetTimer() {
            clearInterval(timerId);
            timeLeft = totalTime;
            isRunning = false;
            startBtn.innerHTML = '<i class="fas fa-play"></i> تشغيل';
            updateCircle();
        }

        // تشغيل الوظائف عند تحميل الصفحة
        renderTasks();
        renderCards();
        updateCircle();
    </script>
</body>
</html>
