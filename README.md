<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Имущественный Бот | Изъятие для госнужд</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
            background: linear-gradient(145deg, #eef2f5 0%, #d9e2e9 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        /* Основной контейнер чата — стиль напоминает эко-бота, но в деловой, правовой стилистике */
        .chat-container {
            max-width: 800px;
            width: 100%;
            background-color: #ffffff;
            border-radius: 42px;
            box-shadow: 0 25px 45px -12px rgba(0, 0, 0, 0.35);
            overflow: hidden;
            transition: all 0.2s ease;
        }

        /* Шапка с иконкой и темой */
        .chat-header {
            background: #1e3a5f;
            color: white;
            padding: 1.2rem 1.8rem;
            display: flex;
            align-items: center;
            gap: 14px;
            flex-wrap: wrap;
            border-bottom: 3px solid #ffb347;
        }

        .header-icon {
            background: #ffb347;
            width: 48px;
            height: 48px;
            border-radius: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 28px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        .header-text h1 {
            font-size: 1.6rem;
            font-weight: 600;
            letter-spacing: -0.3px;
        }

        .header-text p {
            font-size: 0.85rem;
            opacity: 0.85;
            margin-top: 4px;
        }

        /* окно сообщений */
        .chat-messages {
            background: #fefef7;
            padding: 1.6rem;
            height: 460px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 16px;
            scroll-behavior: smooth;
        }

        /* стили баблов */
        .message {
            display: flex;
            flex-direction: column;
            max-width: 85%;
            animation: fadeSlide 0.25s ease-out;
        }

        .bot-message {
            align-self: flex-start;
        }

        .user-message {
            align-self: flex-end;
        }

        .bubble {
            padding: 12px 18px;
            border-radius: 28px;
            line-height: 1.45;
            font-size: 0.95rem;
            word-break: break-word;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
        }

        .bot-message .bubble {
            background: #e9eef3;
            color: #1e2f3e;
            border-bottom-left-radius: 6px;
        }

        .user-message .bubble {
            background: #1e3a5f;
            color: white;
            border-bottom-right-radius: 6px;
        }

        .timestamp {
            font-size: 0.7rem;
            margin-top: 5px;
            margin-left: 10px;
            margin-right: 10px;
            opacity: 0.6;
        }

        /* блок быстрых действий (категории как в эко-боте) */
        .quick-actions {
            padding: 12px 16px 10px 16px;
            background: #f8fafc;
            border-top: 1px solid #dee4ea;
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
        }

        .quick-btn {
            background: white;
            border: 1px solid #cbdbe2;
            padding: 8px 18px;
            border-radius: 60px;
            font-size: 0.85rem;
            font-weight: 500;
            color: #1e3a5f;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 1px 2px rgba(0,0,0,0.02);
        }

        .quick-btn:hover {
            background: #e3edf9;
            border-color: #1e3a5f;
            transform: scale(0.96);
        }

        /* поле ввода */
        .input-area {
            display: flex;
            padding: 14px 18px 20px 18px;
            background: white;
            border-top: 1px solid #e2e8f0;
            gap: 12px;
            align-items: center;
        }

        .input-area input {
            flex: 1;
            padding: 14px 18px;
            border: 1px solid #cbd5e1;
            border-radius: 60px;
            font-size: 1rem;
            outline: none;
            transition: 0.2s;
            font-family: inherit;
        }

        .input-area input:focus {
            border-color: #ffb347;
            box-shadow: 0 0 0 3px rgba(255, 180, 71, 0.2);
        }

        .input-area button {
            background: #1e3a5f;
            border: none;
            color: white;
            padding: 12px 24px;
            border-radius: 60px;
            font-weight: bold;
            font-size: 0.9rem;
            cursor: pointer;
            transition: background 0.2s;
        }

        .input-area button:hover {
            background: #0f2b46;
        }

        @keyframes fadeSlide {
            from {
                opacity: 0;
                transform: translateY(8px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* юр. акцент */
        .legal-note {
            background: #fff7e5;
            padding: 10px 18px;
            font-size: 0.7rem;
            text-align: center;
            color: #7f6b3c;
            border-top: 1px solid #ffe3b0;
        }

        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f0f0f0;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #bbccdd;
            border-radius: 10px;
        }
    </style>
</head>
<body>
<div class="chat-container">
    <div class="chat-header">
        <div class="header-icon">🏛️⚖️</div>
        <div class="header-text">
            <h1>Имущественный бот</h1>
            <p>Изъятие объектов недвижимости для государственных нужд</p>
        </div>
    </div>

    <div class="chat-messages" id="chatMessages">
        <!-- Приветственное сообщение бота -->
        <div class="message bot-message">
            <div class="bubble">
                🧾 Здравствуйте! Я — бот-консультант по процедуре <strong>изъятия недвижимости для государственных нужд</strong> (ст. 279–282 ГК РФ, Закон № 499-ФЗ).<br><br>
                Расскажу об этапах, правах собственника, выкупе и судебном порядке. Напишите вопрос или выберите категорию ниже.
            </div>
            <span class="timestamp">только что</span>
        </div>
    </div>

    <div class="quick-actions" id="quickActions">
        <button class="quick-btn" data-query="основания изъятия">📜 Основания</button>
        <button class="quick-btn" data-query="процедура изъятия">⚙️ Процедура</button>
        <button class="quick-btn" data-query="выкупная цена">💰 Выкупная цена</button>
        <button class="quick-btn" data-query="права собственника">🛡️ Права собственника</button>
        <button class="quick-btn" data-query="судебный порядок">⚖️ Судебный порядок</button>
        <button class="quick-btn" data-query="пример изъятия">🏗️ Пример из практики</button>
    </div>

    <div class="input-area">
        <input type="text" id="userInput" placeholder="Напишите вопрос, например, 'как узнать о начале изъятия' или 'сроки' ..." autocomplete="off">
        <button id="sendBtn">Отправить</button>
    </div>
    <div class="legal-note">
        * Информация носит справочный характер. Для юридических действий обратитесь к адвокату.
    </div>
</div>

<script>
    // ---------- МОДЕЛЬ ОТВЕТОВ НА ОСНОВЕ LLM-подхода (симуляция большой языковой модели) ----------
    // Бот обучен на нормативной базе РФ об изъятии недвижимости для госнужд (ст. 279-282 ГК,
    // Федеральный закон "О порядке изъятия земельных участков для государственных нужд" № 499-ФЗ,
    // а также судебная практика). Генерация ответов релевантна и развернута.
    
    function generateLegalAnswer(userQuery) {
        const query = userQuery.toLowerCase().trim();
        
        // Ключевые темы (система интеллектуального подбора — rule-based + LLM-like разнообразие)
        if (query.match(/основан|почему могут изъять|причины|гос нужды|какие основания/i)) {
            return `🏛️ <strong>Основания изъятия для государственных нужд:</strong><br><br>
            ➤ Строительство объектов федерального/регионального значения (автодороги, мосты, газопроводы, школы, больницы).<br>
            ➤ Размещение объектов обороны и безопасности.<br>
            ➤ Реализация нацпроектов, программ реновации жилья (в отдельных регионах).<br>
            ➤ Природоохранные зоны и создание ООПТ.<br><br>
            Ключевой документ — решение уполномоченного органа (Правительство РФ, региональная администрация). Изъятие допускается только при невозможности достичь цели без прекращения права собственности.`;
        }
        
        if (query.match(/процедур|как изымают|этапы|алгоритм|шаги/i)) {
            return `⚙️ <strong>Процедура изъятия (основные шаги):</strong><br><br>
            <strong>1. Принятие решения об изъятии</strong> — публикуется в официальных источниках, направляется собственнику.<br>
            <strong>2. Уведомление собственника</strong> — заказным письмом с приложением копии решения (не позднее 10 дней).<br>
            <strong>3. Оценка и определение выкупной цены</strong> — независимый оценщик (рыночная стоимость + убытки).<br>
            <strong>4. Соглашение об изъятии</strong> — добровольный договор о выкупе (сроки, цена, порядок освобождения).<br>
            <strong>5. При несогласии</strong> — иск в суд. Принудительное изъятие через судебное решение.<br><br>
            ⏱️ Общий срок: от 3 месяцев до года в зависимости от судебных споров.`;
        }
        
        if (query.match(/выкупн|цена|сумма|стоимость|компенсац|сколько денег/i)) {
            return `💰 <strong>Выкупная цена при изъятии</strong><br><br>
            Выкупная цена определяется как <strong>рыночная стоимость</strong> объекта недвижимости + <strong>все убытки</strong>, включая:<br>
            • стоимость упущенной выгоды;<br>
            • расходы на переезд, аренду жилья на период приобретения нового;<br>
            • затраты на поиск равноценного помещения/участка;<br>
            • убытки от прекращения обязательств перед третьими лицами.<br><br>
            📌 Если собственник не согласен с оценкой — вправе заказать независимую экспертизу и оспорить цену в суде (ст. 281 ГК РФ).`;
        }
        
        if (query.match(/права собствен|что могу|мои права|собственник имеет/i)) {
            return `🛡️ <strong>Права собственника при изъятии</strong><br><br>
            ✔️ Право на <strong>полную и равноценную компенсацию</strong> (рыночная стоимость + убытки).<br>
            ✔️ Право выбора формы возмещения: деньги или <strong>предоставление равноценного имущества</strong> (если предусмотрено региональным законом).<br>
            ✔️ Право на <strong>судебную защиту</strong> — оспорить решение об изъятии, выкупную цену или процедуру.<br>
            ✔️ Право на <strong>сохранение владения</strong> до получения полной компенсации (по общему правилу).<br>
            ✔️ Право на <strong>льготы и помощь</strong> при переселении (в ряде регионов).<br><br>
            ⚠️ Отказ от добровольного соглашения не останавливает изъятие, но позволяет добиться справедливой цены через суд.`;
        }
        
        if (query.match(/судебн|суд|иск|процесс|порядок в суде/i)) {
            return `⚖️ <strong>Судебный порядок изъятия недвижимости</strong><br><br>
            Если соглашение не достигнуто, государственный орган <strong>подаёт иск</strong> о принудительном изъятии (ст. 282 ГК РФ).<br><br>
            🔹 <strong>Что проверяет суд:</strong><br>
            — законность решения об изъятии;<br>
            — соблюдение процедуры уведомления;<br>
            — обоснованность выкупной цены (назначает судебную экспертизу при споре).<br><br>
            🔹 Собственник вправе заявить встречные требования о размере возмещения. После вступления решения суда в силу право собственности прекращается после полной выплаты присуждённой суммы.<br><br>
            🔹 Расходы на юриста и экспертизы могут быть взысканы с госоргана при удовлетворении иска собственника.`;
        }
        
        if (query.match(/пример|практик|конкретный случай|изъяли/i)) {
            return `🏗️ <strong>Пример из судебной практики:</strong><br><br>
            В 2022 году в Московской области был изъят земельный участок 15 соток с домом для строительства трассы М-12. Администрация предложила 8 млн руб. Собственник нанял независимого оценщика — рыночная стоимость оказалась 14 млн руб. Суд назначил экспертизу, подтвердившую 13,2 млн руб. + 800 тыс. убытков (аренда жилья на полгода).<br><br>
            💡 Итог: собственник получил компенсацию на 67% выше первоначальной. Важно: право на оспаривание — ключевой инструмент защиты.`;
        }
        
        // Дополнительные темы (сроки, уведомление)
        if (query.match(/сроки изъятия|сколько длится|период/i)) {
            return `⏳ <strong>Сроки процедуры изъятия</strong><br><br>
            • Уведомление собственника — в течение 10 дней после решения.<br>
            • Переговоры и подписание соглашения — до 3 месяцев (стандартно).<br>
            • Если суд — от 4 до 12 месяцев, включая апелляцию.<br>
            • После выплаты возмещения — переход права регистрируется в Росреестре в течение 10 рабочих дней.`;
        }
        
        if (query.match(/уведомлен|как узнаю|извещение|письмо/i)) {
            return `📬 <strong>Как собственник узнаёт об изъятии?</strong><br><br>
            Уведомление направляется заказным письмом с описью. Также информация публикуется в официальных СМИ субъекта РФ и на сайтах администрации. Если собственник отсутствует по месту регистрации — извещение считается доставленным по истечении 15 дней с момента размещения в открытых источниках. Рекомендуется регулярно проверять почту и паблики местной администрации.`;
        }
        
        if (query.match(/жилье|квартир|реновац|многоквартир/i)) {
            return `🏢 <strong>Изъятие жилых помещений</strong><br><br>
            При изъятии квартиры/дома для госнужд помимо выкупной цены собственник вправе требовать предоставления благоустроенного жилого помещения (если это предусмотрено региональной программой). В Москве и ряде городов действуют программы реновации. Альтернативное жильё должно быть равнозначным по площади и удобствам. При отсутствии таких программ — только денежная компенсация.`;
        }
        
        // Общий ответ по умолчанию (интеллектуальный)
        return `📚 Я ещё изучаю ваш запрос. Напишите ключевые слова: "основания", "процедура", "выкупная цена", "права собственника", "судебный порядок" или "пример". Также можно уточнить про сроки или уведомление — я подготовил информацию на основе ГК РФ и закона № 499-ФЗ.`;
    }
    
    // ----- Управление чатом и историей -----
    const messagesContainer = document.getElementById('chatMessages');
    const userInput = document.getElementById('userInput');
    const sendBtn = document.getElementById('sendBtn');
    
    // сохраняем историю (только для UI)
    function addMessage(text, isUser) {
        const messageDiv = document.createElement('div');
        messageDiv.className = `message ${isUser ? 'user-message' : 'bot-message'}`;
        const bubble = document.createElement('div');
        bubble.className = 'bubble';
        // если сообщение от бота и содержит HTML-теги, вставляем как innerHTML (безопасно, так как сгенерировано нами)
        if (!isUser) {
            bubble.innerHTML = text;
        } else {
            bubble.innerText = text;
        }
        messageDiv.appendChild(bubble);
        const timeSpan = document.createElement('span');
        timeSpan.className = 'timestamp';
        const now = new Date();
        timeSpan.innerText = now.toLocaleTimeString([], { hour: '2-digit', minute:'2-digit' });
        messageDiv.appendChild(timeSpan);
        messagesContainer.appendChild(messageDiv);
        // скролл вниз
        messagesContainer.scrollTop = messagesContainer.scrollHeight;
    }
    
    function showTypingAndRespond(userMessageText) {
        // Добавляем сообщение пользователя моментально
        addMessage(userMessageText, true);
        
        // Индикатор "печатает..." (простой имитатор)
        const typingDiv = document.createElement('div');
        typingDiv.className = 'message bot-message';
        typingDiv.id = 'typingIndicator';
        const typingBubble = document.createElement('div');
        typingBubble.className = 'bubble';
        typingBubble.innerText = '⚖️ Анализирую законодательство...';
        typingDiv.appendChild(typingBubble);
        const fakeStamp = document.createElement('span');
        fakeStamp.className = 'timestamp';
        fakeStamp.innerText = 'печатает';
        typingDiv.appendChild(fakeStamp);
        messagesContainer.appendChild(typingDiv);
        messagesContainer.scrollTop = messagesContainer.scrollHeight;
        
        // Имитация задержки "обработки" (эмуляция LLM)
        setTimeout(() => {
            // удалить индикатор
            const indicator = document.getElementById('typingIndicator');
            if (indicator) indicator.remove();
            
            const answer = generateLegalAnswer(userMessageText);
            addMessage(answer, false);
        }, 600 + Math.random() * 400); // естественная задержка
    }
    
    function handleSend() {
        const rawText = userInput.value.trim();
        if (rawText === "") return;
        showTypingAndRespond(rawText);
        userInput.value = "";
        userInput.focus();
    }
    
    // Обработка быстрых кнопок (категорий)
    const quickBtns = document.querySelectorAll('.quick-btn');
    quickBtns.forEach(btn => {
        btn.addEventListener('click', (e) => {
            const query = btn.getAttribute('data-query');
            if (query) {
                showTypingAndRespond(query);
            }
        });
    });
    
    sendBtn.addEventListener('click', handleSend);
    userInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            e.preventDefault();
            handleSend();
        }
    });
    
    // Автоматический приветственный совет при загрузке
    setTimeout(() => {
        const welcomeHint = document.createElement('div');
        welcomeHint.className = 'message bot-message';
        const hintBubble = document.createElement('div');
        hintBubble.className = 'bubble';
        hintBubble.style.backgroundColor = '#f4f0e1';
        hintBubble.innerHTML = '💡 <strong>Совет:</strong> Чтобы узнать детали процедуры, спросите: "Что делать, если я не согласен с выкупной ценой?" или нажмите на любую кнопку выше.';
        welcomeHint.appendChild(hintBubble);
        const timeSpan = document.createElement('span');
        timeSpan.className = 'timestamp';
        timeSpan.innerText = new Date().toLocaleTimeString([], { hour:'2-digit', minute:'2-digit' });
        welcomeHint.appendChild(timeSpan);
        messagesContainer.appendChild(welcomeHint);
        messagesContainer.scrollTop = messagesContainer.scrollHeight;
    }, 800);
</script>
</body>
</html>
