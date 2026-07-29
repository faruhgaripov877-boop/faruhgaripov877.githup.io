<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FluentAI — Практика английского с AI</title>
    <!-- Google Fonts & FontAwesome -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --primary: #4255ff;
            --primary-hover: #3b4be0;
            --primary-light: #eef0ff;
            --bg-main: #f6f7fb;
            --surface: #ffffff;
            --text-dark: #2e3856;
            --text-muted: #646f90;
            --border-color: #e2e8f0;
            --success: #23b26d;
            --warning: #ffcd1f;
            --danger: #ff725b;
            --shadow-sm: 0 2px 8px rgba(0,0,0,0.04);
            --shadow-md: 0 8px 24px rgba(0,0,0,0.08);
            --radius-md: 12px;
            --radius-lg: 16px;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-dark);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        /* Navbar Quizlet Style */
        .navbar {
            background: var(--surface);
            border-bottom: 2px solid var(--border-color);
            padding: 0.8rem 2rem;
            display: flex;
            align-items: center;
            justify-content: space-between;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.5rem;
            font-weight: 800;
            color: var(--primary);
            text-decoration: none;
        }

        .logo i { font-size: 1.8rem; }

        .nav-controls {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .api-key-btn {
            background: var(--primary-light);
            color: var(--primary);
            border: none;
            padding: 8px 16px;
            border-radius: var(--radius-md);
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .api-key-btn:hover {
            background: #e0e4ff;
        }

        /* Container */
        .container {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 1.5rem;
            width: 100%;
            flex: 1;
        }

        .section-title {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: var(--text-dark);
        }

        /* Level Selector */
        .level-selector {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 12px;
            margin-bottom: 2.5rem;
        }

        .level-card {
            background: var(--surface);
            border: 2px solid var(--border-color);
            border-radius: var(--radius-md);
            padding: 1rem;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .level-card:hover {
            border-color: var(--primary);
            transform: translateY(-2px);
            box-shadow: var(--shadow-sm);
        }

        .level-card.active {
            border-color: var(--primary);
            background: var(--primary);
            color: white;
        }

        .level-badge {
            font-size: 1.25rem;
            font-weight: 800;
            margin-bottom: 4px;
        }

        .level-desc {
            font-size: 0.75rem;
            opacity: 0.8;
        }

        /* Scenarios Grid */
        .scenarios-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
            gap: 20px;
        }

        .scenario-card {
            background: var(--surface);
            border-radius: var(--radius-lg);
            border: 2px solid var(--border-color);
            padding: 1.5rem;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            box-shadow: var(--shadow-sm);
        }

        .scenario-card:hover {
            border-color: var(--primary);
            box-shadow: var(--shadow-md);
            transform: translateY(-3px);
        }

        .scenario-icon {
            width: 48px;
            height: 48px;
            background: var(--primary-light);
            color: var(--primary);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.4rem;
            margin-bottom: 1rem;
        }

        .scenario-title {
            font-weight: 700;
            font-size: 1.1rem;
            margin-bottom: 0.5rem;
        }

        .scenario-desc {
            color: var(--text-muted);
            font-size: 0.875rem;
            line-height: 1.4;
            margin-bottom: 1.5rem;
        }

        .scenario-footer {
            display: flex;
            align-items: center;
            justify-content: space-between;
            font-size: 0.85rem;
            font-weight: 600;
            color: var(--primary);
        }

        /* Chat View Interface */
        .chat-view {
            display: none;
            grid-template-columns: 1fr 380px;
            gap: 24px;
            height: calc(100vh - 120px);
        }

        @media (max-width: 900px) {
            .chat-view {
                grid-template-columns: 1fr;
                height: auto;
            }
        }

        .chat-main {
            background: var(--surface);
            border-radius: var(--radius-lg);
            border: 2px solid var(--border-color);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            box-shadow: var(--shadow-sm);
        }

        .chat-header {
            padding: 1rem 1.5rem;
            border-bottom: 2px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: var(--surface);
        }

        .back-btn {
            background: none;
            border: none;
            color: var(--text-muted);
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .back-btn:hover { color: var(--text-dark); }

        .chat-messages {
            flex: 1;
            padding: 1.5rem;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .message {
            max-width: 80%;
            padding: 12px 16px;
            border-radius: 16px;
            font-size: 0.95rem;
            line-height: 1.5;
            position: relative;
        }

        .message.ai {
            align-self: flex-start;
            background: var(--bg-main);
            color: var(--text-dark);
            border-bottom-left-radius: 4px;
        }

        .message.user {
            align-self: flex-end;
            background: var(--primary);
            color: white;
            border-bottom-right-radius: 4px;
        }

        .message.has-error {
            border: 2px solid var(--danger);
        }

        .chat-input-area {
            padding: 1rem 1.5rem;
            border-top: 2px solid var(--border-color);
            display: flex;
            gap: 12px;
            background: var(--surface);
        }

        .chat-input {
            flex: 1;
            padding: 12px 16px;
            border: 2px solid var(--border-color);
            border-radius: var(--radius-md);
            outline: none;
            font-size: 0.95rem;
            transition: border-color 0.2s;
        }

        .chat-input:focus {
            border-color: var(--primary);
        }

        .send-btn {
            background: var(--primary);
            color: white;
            border: none;
            border-radius: var(--radius-md);
            padding: 0 20px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
        }

        .send-btn:hover { background: var(--primary-hover); }

        /* Feedback Analysis Sidebar */
        .feedback-sidebar {
            background: var(--surface);
            border-radius: var(--radius-lg);
            border: 2px solid var(--border-color);
            padding: 1.5rem;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 16px;
            box-shadow: var(--shadow-sm);
        }

        .feedback-card {
            background: var(--bg-main);
            border-radius: var(--radius-md);
            padding: 1rem;
            border-left: 4px solid var(--primary);
        }

        .feedback-card.error { border-left-color: var(--danger); }
        .feedback-card.warning { border-left-color: var(--warning); }
        .feedback-card.success { border-left-color: var(--success); }

        .feedback-title {
            font-weight: 700;
            font-size: 0.9rem;
            margin-bottom: 6px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .feedback-text {
            font-size: 0.875rem;
            color: var(--text-dark);
            line-height: 1.4;
        }

        .original-text {
            text-decoration: line-through;
            color: var(--danger);
            margin-right: 6px;
        }

        .corrected-text {
            color: var(--success);
            font-weight: 600;
        }

        /* Modal Settings */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.5);
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }

        .modal-content {
            background: var(--surface);
            border-radius: var(--radius-lg);
            padding: 2rem;
            width: 100%;
            max-width: 450px;
        }

        .modal-content input {
            width: 100%;
            padding: 10px;
            margin: 12px 0 20px 0;
            border: 2px solid var(--border-color);
            border-radius: var(--radius-md);
        }
    </style>
</head>
<body>

    <!-- Header -->
    <header class="navbar">
        <a href="#" class="logo" onclick="location.reload()">
            <i class="fa-solid fa-shapes"></i> FluentAI
        </a>
        <div class="nav-controls">
            <button class="api-key-btn" onclick="openKeyModal()">
                <i class="fa-solid fa-key"></i> OpenAI / Gemini Key
            </button>
        </div>
    </header>

    <div class="container">
        <!-- Main Dashboard View -->
        <div id="dashboard-view">
            <h2 class="section-title">1. Выберите ваш уровень языка</h2>
            <div class="level-selector">
                <div class="level-card active" data-level="A1">
                    <div class="level-badge">A1</div>
                    <div class="level-desc">Beginner</div>
                </div>
                <div class="level-card" data-level="A2">
                    <div class="level-badge">A2</div>
                    <div class="level-desc">Elementary</div>
                </div>
                <div class="level-card" data-level="B1">
                    <div class="level-badge">B1</div>
                    <div class="level-desc">Intermediate</div>
                </div>
                <div class="level-card" data-level="B2">
                    <div class="level-badge">B2</div>
                    <div class="level-desc">Upper-Int</div>
                </div>
                <div class="level-card" data-level="C1">
                    <div class="level-badge">C1</div>
                    <div class="level-desc">Advanced</div>
                </div>
                <div class="level-card" data-level="C2">
                    <div class="level-badge">C2</div>
                    <div class="level-desc">Proficient</div>
                </div>
            </div>

            <h2 class="section-title">2. Выберите сценарий для практики</h2>
            <div class="scenarios-grid" id="scenarios-grid">
                <!-- Сценарии генерируются динамически -->
            </div>
        </div>

        <!-- Chat Practice View -->
        <div class="chat-view" id="chat-view">
            <div class="chat-main">
                <div class="chat-header">
                    <button class="back-btn" onclick="exitChat()">
                        <i class="fa-solid fa-arrow-left"></i> Выйти к выбору
                    </button>
                    <div>
                        <span id="current-scenario-title" style="font-weight: 700;"></span>
                        <span id="current-level-tag" style="background: var(--primary-light); color: var(--primary); padding: 4px 8px; border-radius: 6px; font-weight: 700; font-size: 0.8rem; margin-left: 8px;"></span>
                    </div>
                </div>
                <div class="chat-messages" id="chat-messages">
                    <!-- Сообщения чата -->
                </div>
                <div class="chat-input-area">
                    <input type="text" id="chat-input" class="chat-input" placeholder="Напишите ответ на английском..." onkeypress="handleKeyPress(event)">
                    <button class="send-btn" onclick="sendMessage()">
                        <i class="fa-solid fa-paper-plane"></i>
                    </button>
                </div>
            </div>

            <!-- Разбор ошибок -->
            <div class="feedback-sidebar">
                <h3 style="font-size: 1.1rem; font-weight: 700; margin-bottom: 0.5rem;">
                    <i class="fa-solid fa-magnifying-glass-chart" style="color: var(--primary);"></i> Подробный разбор
                </h3>
                <p style="font-size: 0.85rem; color: var(--text-muted);">Здесь AI анализирует вашу последнюю реплику и дает подсказки по грамматике и стилю.</p>
                <div id="feedback-container">
                    <div class="feedback-card success">
                        <div class="feedback-title">Готов к диалогу</div>
                        <div class="feedback-text">Начните диалог. При отправке сообщений здесь появятся исправления и альтернативы от носителя.</div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal for API Key -->
    <div class="modal" id="key-modal">
        <div class="modal-content">
            <h3 style="margin-bottom: 10px;">Настройка API ключа</h3>
            <p style="font-size: 0.85rem; color: var(--text-muted);">По умолчанию сайт работает в интерактивном ДЕМО-режиме с имитацией AI. Для подключения реального OpenAI API введите ваш ключ:</p>
            <input type="password" id="api-key-input" placeholder="sk-proj-...">
            <div style="display: flex; gap: 10px; justify-content: flex-end;">
                <button class="api-key-btn" style="background: var(--border-color); color: var(--text-dark);" onclick="closeKeyModal()">Отмена</button>
                <button class="send-btn" style="padding: 8px 16px;" onclick="saveApiKey()">Сохранить</button>
            </div>
        </div>
    </div>

    <script>
        // Данные сценариев
        const scenarios = [
            {
                id: 'job-interview',
                title: 'Собеседование на работу',
                desc: 'Интервью на позицию в международной IT-компании. Вопросы про опыт и ваши сильные стороны.',
                icon: 'fa-briefcase',
                prompt: 'You are an HR interviewer at a global tech firm. Conduct a polite professional interview.'
            },
            {
                id: 'flirting-street',
                title: 'Знакомство в парке',
                desc: 'Неформальный разговор, комплименты и флирт в непринужденной обстановке.',
                icon: 'fa-heart',
                prompt: 'You are a friendly person relaxing in a city park. Respond naturally to someone coming up to talk with you.'
            },
            {
                id: 'coffee-shop',
                title: 'Заказ в кофейне',
                desc: 'Диалог с бариста: выбор напитка, десерта, обсуждение размера и типа молока.',
                icon: 'fa-mug-hot',
                prompt: 'You are a lively barista in London. Take the customer order and ask standard coffee shop follow-up questions.'
            },
            {
                id: 'clothes-boutique',
                title: 'Консультант в бутике',
                desc: 'Подбор размера, поиск примерочной, консультация по стилю и скидкам.',
                icon: 'fa-shirt',
                prompt: 'You are a stylish shop assistant in a fashion boutique. Help the customer find fitting clothes.'
            },
            {
                id: 'taxi-phone',
                title: 'Заказ такси по телефону',
                desc: 'Уточнение адреса, времени подачи, стоимости и количества багажа.',
                icon: 'fa-taxi',
                prompt: 'You are a taxi dispatcher on the phone. Take location details and confirm the booking politely.'
            },
            {
                id: 'hotel-checkin',
                title: 'Заселение в отель',
                desc: 'Регистрация на рецепшн, уточнение брони, правил завтрака и ключей от номера.',
                icon: 'fa-hotel',
                prompt: 'You are a hotel receptionist welcoming a guest at check-in.'
            }
        ];

        let selectedLevel = 'B1';
        let activeScenario = null;
        let apiKey = localStorage.getItem('fluent_ai_key') || '';

        // Инициализация
        document.addEventListener('DOMContentLoaded', () => {
            renderScenarios();
            setupLevelSelector();
        });

        function setupLevelSelector() {
            document.querySelectorAll('.level-card').forEach(card => {
                card.addEventListener('click', () => {
                    document.querySelectorAll('.level-card').forEach(c => c.classList.remove('active'));
                    card.classList.add('active');
                    selectedLevel = card.dataset.level;
                });
            });
        }

        function renderScenarios() {
            const grid = document.getElementById('scenarios-grid');
            grid.innerHTML = scenarios.map(sc => `
                <div class="scenario-card" onclick="startScenario('${sc.id}')">
                    <div>
                        <div class="scenario-icon">
                            <i class="fa-solid ${sc.icon}"></i>
                        </div>
                        <div class="scenario-title">${sc.title}</div>
                        <div class="scenario-desc">${sc.desc}</div>
                    </div>
                    <div class="scenario-footer">
                        <span>Начать практику</span>
                        <i class="fa-solid fa-chevron-right"></i>
                    </div>
                </div>
            `).join('');
        }

        function startScenario(id) {
            activeScenario = scenarios.find(s => s.id === id);
            document.getElementById('dashboard-view').style.display = 'none';
            document.getElementById('chat-view').style.display = 'grid';
            
            document.getElementById('current-scenario-title').innerText = activeScenario.title;
            document.getElementById('current-level-tag').innerText = `Уровень ${selectedLevel}`;
            
            document.getElementById('chat-messages').innerHTML = '';
            
            // Первое приветственное сообщение AI
            const initialGreeting = getInitialGreeting(activeScenario.id);
            addMessage(initialGreeting, 'ai');
        }

        function exitChat() {
            document.getElementById('chat-view').style.display = 'none';
            document.getElementById('dashboard-view').style.display = 'block';
        }

        function addMessage(text, sender, hasError = false) {
            const container = document.getElementById('chat-messages');
            const msgDiv = document.createElement('div');
            msgDiv.className = `message ${sender} ${hasError ? 'has-error' : ''}`;
            msgDiv.innerText = text;
            container.appendChild(msgDiv);
            container.scrollTop = container.scrollHeight;
        }

        function handleKeyPress(e) {
            if (e.key === 'Enter') sendMessage();
        }

        async function sendMessage() {
            const input = document.getElementById('chat-input');
            const text = input.value.trim();
            if (!text) return;

            addMessage(text, 'user');
            input.value = '';

            if (apiKey) {
                // Если введен реальный ключ API OpenAI
                await processWithOpenAI(text);
            } else {
                // Демонстрационный режим с имитацией и разбором ошибок
                setTimeout(() => simulateAiResponse(text), 700);
            }
        }

        // Имитация ответа и разбора ошибок для демонстрации
        function simulateAiResponse(userText) {
            // Анализ типичных ошибок пользователей для интерактивного урока
            const mistakes = detectDemoMistakes(userText);

            if (mistakes.length > 0) {
                renderFeedback(mistakes);
            } else {
                renderSuccessFeedback();
            }

            const reply = generateAiReply(userText, activeScenario.id);
            addMessage(reply, 'ai');
        }

        // Автоматическая проверка частых грамматических ошибок
        function detectDemoMistakes(text) {
            const list = [];
            const lower = text.toLowerCase();

            if (/\bi am agree\b/i.test(text)) {
                list.push({
                    type: 'Грамматика',
                    original: 'I am agree',
                    corrected: 'I agree',
                    explain: 'Глагол "agree" означает "соглашаться". Вспомогательный глагол "am" здесь не нужен.'
                });
            }
            if (/\bi feel myself\b/i.test(text)) {
                list.push({
                    type: 'Стиль / Лексика',
                    original: 'I feel myself fine',
                    corrected: 'I feel fine / good',
                    explain: 'В английском языке после "feel" возвратное местоимение "myself" обычно не используется.'
                });
            }
            if (/\bwant do\b/i.test(text) || /\blike do\b/i.test(text)) {
                list.push({
                    type: 'Инфинитив',
                    original: 'want/like do',
                    corrected: 'want to do / like doing',
                    explain: 'После "want" требуется частица "to" перед следующим глаголом.'
                });
            }
            if (lower.startsWith('i think yes') || lower.startsWith('i think no')) {
                list.push({
                    type: 'Естественная речь',
                    original: 'I think yes',
                    corrected: 'I think so / I don\'t think so',
                    explain: 'Носители языка говорят "I think so" (Думаю, да) вместо прямой кальки с русского.'
                });
            }

            return list;
        }

        function renderFeedback(mistakes) {
            const container = document.getElementById('feedback-container');
            container.innerHTML = mistakes.map(m => `
                <div class="feedback-card error">
                    <div class="feedback-title">
                        <i class="fa-solid fa-circle-exclamation" style="color: var(--danger);"></i> ${m.type}
                    </div>
                    <div class="feedback-text">
                        <span class="original-text">${m.original}</span> ➔ <span class="corrected-text">${m.corrected}</span>
                        <div style="margin-top: 6px; color: var(--text-muted);">${m.explain}</div>
                    </div>
                </div>
            `).join('');
        }

        function renderSuccessFeedback() {
            const container = document.getElementById('feedback-container');
            container.innerHTML = `
                <div class="feedback-card success">
                    <div class="feedback-title">
                        <i class="fa-solid fa-circle-check" style="color: var(--success);"></i> Отлично сформулировано!
                    </div>
                    <div class="feedback-text">Грамматических ошибок не обнаружено. Вы звучите естественнно для уровня ${selectedLevel}!</div>
                </div>
            `;
        }

        function getInitialGreeting(scenarioId) {
            const greetings = {
                'job-interview': 'Hello! Thanks for coming today. To start off, could you briefly tell me about yourself and your background?',
                'flirting-street': 'Hey there! Beautiful day in the park, isn\'t it? Are you enjoying the weather?',
                'coffee-shop': 'Hi! Welcome to Artisan Coffee. What can I get started for you today?',
                'clothes-boutique': 'Good afternoon! Let me know if you need any help finding sizes or trying anything on.',
                'taxi-phone': 'City Cabs dispatch, hello! Where can we pick you up today?',
                'hotel-checkin': 'Welcome to Grand Hotel! Checking in today?'
            };
            return greetings[scenarioId] || 'Hello! How can I help you today?';
        }

        function generateAiReply(userText, scenarioId) {
            // Имитация контекстного диалога
            const replies = {
                'job-interview': 'That sounds impressive! How do you usually handle tight deadlines when working on complex team projects?',
                'flirting-street': 'I totally agree! I love coming here to get some fresh air. Are you from around here?',
                'coffee-shop': 'Great choice! Would you like that iced or hot, and what size?',
                'clothes-boutique': 'Sure thing! The fitting rooms are right around the corner on your left.',
                'taxi-phone': 'Got it. A driver will be there in about 7 minutes. Anything else?',
                'hotel-checkin': 'May I please have your passport and confirmation number?'
            };
            return replies[scenarioId] || 'That sounds great! Tell me more about it.';
        }

        /* Интеграция с OpenAI API */
        async function processWithOpenAI(userText) {
            const systemPrompt = `
You are an expert English native speaker and language coach. 
Current User CEFR Level: ${selectedLevel}.
Scenario Role: ${activeScenario.prompt}.

Instructions:
1. Respond to the user's input in character in natural conversational English appropriate for level ${selectedLevel}. Keep replies under 3-4 sentences to keep conversation fluid.
2. Analyze the user's input for ANY grammar, vocabulary, preposition, or awkward phrasing mistakes.
3. Return the response strictly as JSON in the following format:
{
  "reply": "AI response in English character",
  "has_errors": true/false,
  "corrections": [
    {
      "type": "Grammar/Vocabulary/Natural Phrasing",
      "original": "incorrect user phrase",
      "corrected": "native correct phrase",
      "explain": "Detailed explanation in Russian"
    }
  ]
}
`;

            try {
                const res = await fetch('https://api.openai.com/v1/chat/completions', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${apiKey}`
                    },
                    body: JSON.stringify({
                        model: 'gpt-4o-mini',
                        messages: [
                            { role: 'system', content: systemPrompt },
                            { role: 'user', content: userText }
                        ],
                        response_format: { type: 'json_object' }
                    })
                });

                const data = await res.json();
                const parsed = JSON.parse(data.choices[0].message.content);

                if (parsed.has_errors && parsed.corrections) {
                    renderFeedback(parsed.corrections);
                } else {
                    renderSuccessFeedback();
                }

                addMessage(parsed.reply, 'ai');
            } catch (err) {
                console.error(err);
                addMessage("Error connecting to API. Please check your key or try demo mode.", "ai");
            }
        }

        /* Modal Functions */
        function openKeyModal() {
            document.getElementById('key-modal').style.display = 'flex';
            document.getElementById('api-key-input').value = apiKey;
        }

        function closeKeyModal() {
            document.getElementById('key-modal').style.display = 'none';
        }

        function saveApiKey() {
            apiKey = document.getElementById('api-key-input').value.trim();
            localStorage.setItem('fluent_ai_key', apiKey);
            closeKeyModal();
            alert('API Key сохранен!');
        }
    </script>
</body>
</html>
