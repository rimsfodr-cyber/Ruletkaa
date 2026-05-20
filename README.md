 <!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Премиум Рулетка — Золотые призы</title>

    <!-- Google Fonts: премиальные шрифты -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">

    <style>
        :root {
            --bg: #08080f;
            --bg-secondary: #111122;
            --gold: #d4a843;
            --gold-bright: #f0c960;
            --gold-dark: #8b6914;
            --gold-glow: #f7d774;
            --text: #e8e6e1;
            --text-secondary: #b0ada5;
            --danger: #c0392b;
            --success: #27ae60;
            --silver: #b8b8c0;
            --bronze: #b87333;
            --rose-gold: #e8c547;
            --rare-gold: #ffd700;
            --modal-bg: #1a1a2e;
            --wheel-size: 500px;
            --transition-speed: 0.35s;
        }

        *,
        *::before,
        *::after {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        html {
            font-size: 16px;
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background: radial-gradient(ellipse at 50% 30%, #12122a 0%, #08080f 65%, #040408 100%);
            color: var(--text);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            padding: 20px 16px 40px;
            overflow-x: hidden;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
            -webkit-user-select: none;
        }

        /* Декоративный фоновый узор */
        body::before {
            content: '';
            position: fixed;
            inset: 0;
            background:
                radial-gradient(circle at 20% 25%, rgba(212, 168, 67, 0.04) 0%, transparent 50%),
                radial-gradient(circle at 75% 60%, rgba(212, 168, 67, 0.03) 0%, transparent 50%),
                radial-gradient(circle at 50% 85%, rgba(240, 201, 96, 0.05) 0%, transparent 40%);
            pointer-events: none;
            z-index: 0;
        }

        .container {
            position: relative;
            z-index: 1;
            max-width: 700px;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 28px;
        }

        /* ===== Заголовок ===== */
        .header {
            text-align: center;
        }
        .header__title {
            font-family: 'Playfair Display', serif;
            font-size: clamp(2rem, 5vw, 2.8rem);
            font-weight: 800;
            letter-spacing: 0.03em;
            background: linear-gradient(180deg, #f7d774 0%, #c9973a 40%, #8b6914 70%, #c9973a 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            filter: drop-shadow(0 2px 6px rgba(212, 168, 67, 0.5));
            margin-bottom: 2px;
        }
        .header__subtitle {
            font-size: 0.9rem;
            color: var(--text-secondary);
            letter-spacing: 0.08em;
            text-transform: uppercase;
            font-weight: 400;
        }

        /* ===== Секция колеса ===== */
        .wheel-section {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            width: 100%;
        }
        .wheel-wrapper {
            position: relative;
            width: min(var(--wheel-size), 85vw);
            height: min(var(--wheel-size), 85vw);
            flex-shrink: 0;
        }
        .wheel-glow {
            position: absolute;
            inset: -18px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(212, 168, 67, 0.25) 0%, rgba(212, 168, 67, 0.06) 50%, transparent 70%);
            pointer-events: none;
            animation: glowPulse 3s ease-in-out infinite;
            z-index: 0;
        }
        @keyframes glowPulse {
            0%,
            100% {
                opacity: 0.7;
                transform: scale(1);
            }
            50% {
                opacity: 1;
                transform: scale(1.04);
            }
        }
        .wheel-glow.spinning {
            animation: glowSpin 0.6s ease-in-out infinite;
        }
        @keyframes glowSpin {
            0%,
            100% {
                opacity: 0.9;
                transform: scale(1.03);
            }
            50% {
                opacity: 1;
                transform: scale(1.08);
            }
        }
        canvas#wheelCanvas {
            position: relative;
            z-index: 1;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            filter: drop-shadow(0 0 20px rgba(180, 140, 60, 0.5)) drop-shadow(0 8px 32px rgba(0, 0, 0, 0.6));
            transition: filter 0.4s;
            cursor: default;
        }
        .wheel-wrapper.ready canvas#wheelCanvas {
            filter: drop-shadow(0 0 28px rgba(240, 200, 90, 0.7)) drop-shadow(0 8px 36px rgba(0, 0, 0, 0.6));
            cursor: pointer;
        }

        /* Указатель (стрелка) */
        .pointer {
            position: absolute;
            top: -16px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 10;
            width: 0;
            height: 0;
            border-left: 18px solid transparent;
            border-right: 18px solid transparent;
            border-top: 28px solid #f0c960;
            filter: drop-shadow(0 2px 8px rgba(240, 201, 96, 0.8)) drop-shadow(0 0 12px rgba(240, 200, 80, 0.5));
            transition: filter 0.3s;
        }
        .pointer::after {
            content: '';
            position: absolute;
            top: -34px;
            left: -10px;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: radial-gradient(circle, #fff 0%, #f0c960 60%, #8b6914 100%);
            filter: blur(1px);
            box-shadow: 0 0 16px rgba(240, 201, 96, 0.9);
        }

        /* ===== Панель управления ===== */
        .controls {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
            gap: 14px;
            width: 100%;
            max-width: 480px;
        }
        .balance-display {
            display: flex;
            align-items: center;
            gap: 8px;
            background: rgba(20, 20, 40, 0.8);
            border: 1px solid rgba(180, 150, 80, 0.35);
            border-radius: 40px;
            padding: 10px 20px;
            font-weight: 600;
            font-size: 1rem;
            letter-spacing: 0.03em;
            white-space: nowrap;
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
        }
        .balance-display .icon {
            font-size: 1.4rem;
        }
        .balance-display .count {
            color: var(--gold-bright);
            font-size: 1.3rem;
            font-weight: 700;
            min-width: 28px;
            text-align: center;
        }
        .total-spins-display {
            font-size: 0.78rem;
            color: var(--text-secondary);
            letter-spacing: 0.04em;
            text-align: center;
            margin-top: -10px;
        }
        .total-spins-display span {
            color: var(--gold);
            font-weight: 600;
        }

        /* Кнопки */
        .btn {
            font-family: 'Inter', sans-serif;
            font-weight: 600;
            font-size: 0.95rem;
            letter-spacing: 0.04em;
            padding: 12px 26px;
            border-radius: 40px;
            border: none;
            cursor: pointer;
            transition: all var(--transition-speed) ease;
            position: relative;
            overflow: hidden;
            white-space: nowrap;
            outline: none;
            text-transform: uppercase;
        }
        .btn:active {
            transform: scale(0.96);
        }

        /* Кнопка Крутить */
        .btn-spin {
            background: linear-gradient(180deg, #f0c960 0%, #c9973a 40%, #8b6914 100%);
            color: #1a1a0a;
            font-size: 1.1rem;
            font-weight: 700;
            padding: 14px 36px;
            box-shadow: 0 4px 20px rgba(200, 150, 50, 0.5), 0 0 40px rgba(220, 170, 60, 0.25);
            letter-spacing: 0.06em;
            text-shadow: 0 1px 0 rgba(255, 255, 255, 0.2);
        }
        .btn-spin:hover:not(:disabled) {
            background: linear-gradient(180deg, #f7d774 0%, #d4a843 40%, #a07818 100%);
            box-shadow: 0 6px 28px rgba(220, 170, 60, 0.65), 0 0 60px rgba(240, 200, 80, 0.35);
            transform: translateY(-2px);
        }
        .btn-spin:disabled {
            background: #3a3a4a;
            color: #777;
            cursor: not-allowed;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            text-shadow: none;
        }
        .btn-spin:disabled:hover {
            transform: none;
        }
        .btn-spin-wrapper {
            position: relative;
            display: inline-block;
        }
        .tooltip {
            visibility: hidden;
            opacity: 0;
            position: absolute;
            bottom: calc(100% + 12px);
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.9);
            color: #f0c960;
            font-size: 0.78rem;
            padding: 8px 14px;
            border-radius: 6px;
            white-space: nowrap;
            pointer-events: none;
            transition: opacity 0.3s, visibility 0.3s;
            border: 1px solid rgba(240, 201, 96, 0.5);
            letter-spacing: 0.03em;
        }
        .tooltip::after {
            content: '';
            position: absolute;
            top: 100%;
            left: 50%;
            transform: translateX(-50%);
            border: 6px solid transparent;
            border-top-color: rgba(0, 0, 0, 0.9);
        }
        .btn-spin-wrapper:hover .tooltip {
            visibility: visible;
            opacity: 1;
        }

        /* Кнопка Купить спины */
        .btn-buy {
            background: transparent;
            border: 2px solid var(--gold);
            color: var(--gold-bright);
            padding: 12px 22px;
            font-weight: 500;
            letter-spacing: 0.05em;
        }
        .btn-buy:hover {
            background: rgba(212, 168, 67, 0.12);
            border-color: var(--gold-bright);
            box-shadow: 0 0 20px rgba(200, 150, 50, 0.3);
            color: #fff;
        }

        /* Кнопка Я оплатил */
        .btn-paid {
            background: linear-gradient(180deg, #2ecc71 0%, #27ae60 50%, #1e8449 100%);
            color: #fff;
            font-weight: 700;
            padding: 14px 28px;
            box-shadow: 0 4px 18px rgba(39, 174, 96, 0.45);
            animation: paidAppear 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
            letter-spacing: 0.05em;
        }
        .btn-paid:hover {
            background: linear-gradient(180deg, #3ddb84 0%, #2ecc71 50%, #27ae60 100%);
            box-shadow: 0 6px 24px rgba(39, 174, 96, 0.6);
            transform: translateY(-2px);
        }
        @keyframes paidAppear {
            from {
                opacity: 0;
                transform: scale(0.7);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }
        .hidden {
            display: none !important;
        }

        /* ===== Список призов ===== */
        .prizes-section {
            width: 100%;
            max-width: 600px;
            background: rgba(18, 18, 35, 0.7);
            border: 1px solid rgba(180, 150, 80, 0.25);
            border-radius: 20px;
            padding: 24px 20px;
            backdrop-filter: blur(6px);
            -webkit-backdrop-filter: blur(6px);
        }
        .prizes-section h2 {
            font-family: 'Playfair Display', serif;
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--gold-bright);
            text-align: center;
            margin-bottom: 18px;
            letter-spacing: 0.04em;
        }
        .prizes-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
            gap: 12px;
        }
        .prize-card {
            background: rgba(10, 10, 25, 0.7);
            border: 1px solid rgba(150, 130, 80, 0.2);
            border-radius: 14px;
            padding: 14px 12px;
            text-align: center;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        .prize-card:hover {
            border-color: rgba(200, 160, 70, 0.5);
            box-shadow: 0 4px 16px rgba(180, 140, 50, 0.2);
            transform: translateY(-2px);
        }
        .prize-card.rare {
            border-color: rgba(255, 215, 0, 0.6);
            background: rgba(20, 15, 5, 0.8);
            box-shadow: 0 0 18px rgba(255, 215, 0, 0.2), inset 0 0 20px rgba(255, 215, 0, 0.04);
        }
        .prize-card.rare::before {
            content: '';
            position: absolute;
            inset: -1px;
            border-radius: 14px;
            background: linear-gradient(135deg, rgba(255, 215, 0, 0.5), transparent 40%, transparent 60%, rgba(255, 215, 0, 0.5));
            z-index: -1;
            animation: rareBorderShine 3s ease-in-out infinite;
        }
        @keyframes rareBorderShine {
            0%,
            100% {
                opacity: 0.5;
            }
            50% {
                opacity: 1;
            }
        }
        .prize-card .emoji {
            font-size: 2rem;
            display: block;
            margin-bottom: 6px;
        }
        .prize-card .name {
            font-weight: 600;
            font-size: 0.9rem;
            color: #e8e6e1;
            letter-spacing: 0.02em;
            line-height: 1.2;
        }
        .prize-card .chance {
            font-size: 0.75rem;
            color: var(--text-secondary);
            margin-top: 4px;
        }
        .prize-card .badge {
            display: inline-block;
            font-size: 0.68rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.06em;
            color: #1a1a0a;
            background: linear-gradient(135deg, #ffd700, #f0c960);
            padding: 3px 10px;
            border-radius: 20px;
            margin-top: 6px;
        }

        /* ===== Модальное окно ===== */
        .modal-overlay {
            position: fixed;
            inset: 0;
            z-index: 100;
            background: rgba(0, 0, 0, 0.75);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            backdrop-filter: blur(4px);
            -webkit-backdrop-filter: blur(4px);
            animation: fadeIn 0.3s ease;
        }
        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }
        .modal {
            background: var(--modal-bg);
            border-radius: 22px;
            padding: 32px 24px 24px;
            text-align: center;
            max-width: 420px;
            width: 100%;
            position: relative;
            border: 2px solid rgba(180, 150, 80, 0.5);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.7), 0 0 40px rgba(180, 140, 60, 0.25);
            animation: modalPop 0.45s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }
        .modal.rare-win {
            border: 3px solid #ffd700;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.7), 0 0 60px rgba(255, 215, 0, 0.5), 0 0 100px rgba(255, 200, 50, 0.3);
            animation: modalPopRare 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }
        @keyframes modalPop {
            from {
                opacity: 0;
                transform: scale(0.7) translateY(30px);
            }
            to {
                opacity: 1;
                transform: scale(1) translateY(0);
            }
        }
        @keyframes modalPopRare {
            0% {
                opacity: 0;
                transform: scale(0.5) translateY(40px);
            }
            60% {
                transform: scale(1.08) translateY(-8px);
            }
            100% {
                opacity: 1;
                transform: scale(1) translateY(0);
            }
        }
        .modal__emoji {
            font-size: 4rem;
            display: block;
            margin-bottom: 10px;
            animation: emojiBounce 0.6s ease-out 0.2s both;
        }
        @keyframes emojiBounce {
            0% {
                transform: scale(0);
                opacity: 0;
            }
            60% {
                transform: scale(1.3);
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }
        .modal__title {
            font-family: 'Playfair Display', serif;
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--gold-bright);
            margin-bottom: 6px;
            letter-spacing: 0.03em;
        }
        .modal__prize-name {
            font-size: 1.3rem;
            font-weight: 700;
            color: #fff;
            margin-bottom: 4px;
        }
        .modal__text {
            font-size: 0.95rem;
            color: var(--text-secondary);
            margin-bottom: 18px;
            line-height: 1.5;
        }
        .modal__btn-close {
            font-family: 'Inter', sans-serif;
            font-weight: 600;
            font-size: 0.9rem;
            letter-spacing: 0.05em;
            padding: 12px 32px;
            border-radius: 30px;
            border: 2px solid var(--gold);
            background: transparent;
            color: var(--gold-bright);
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
        }
        .modal__btn-close:hover {
            background: rgba(212, 168, 67, 0.15);
            border-color: var(--gold-bright);
            box-shadow: 0 0 16px rgba(200, 150, 50, 0.3);
        }

        /* ===== Конфетти ===== */
        .confetti-container {
            position: fixed;
            inset: 0;
            z-index: 200;
            pointer-events: none;
            overflow: hidden;
        }
        .confetti-piece {
            position: absolute;
            width: 10px;
            height: 10px;
            border-radius: 2px;
            animation: confettiFall linear forwards;
        }
        @keyframes confettiFall {
            0% {
                transform: translateY(-120px) rotate(0deg) scale(1);
                opacity: 1;
            }
            70% {
                opacity: 1;
            }
            100% {
                transform: translateY(105vh) rotate(720deg) scale(0.3);
                opacity: 0;
            }
        }

        /* ===== Адаптивность ===== */
        @media (max-width: 600px) {
            .wheel-wrapper {
                width: min(420px, 80vw);
                height: min(420px, 80vw);
            }
            .pointer {
                top: -12px;
                border-left-width: 14px;
                border-right-width: 14px;
                border-top-width: 22px;
            }
            .pointer::after {
                top: -28px;
                left: -8px;
                width: 16px;
                height: 16px;
            }
            .btn-spin {
                font-size: 0.95rem;
                padding: 12px 24px;
            }
            .btn-buy,
            .btn-paid {
                font-size: 0.85rem;
                padding: 10px 18px;
            }
            .prizes-grid {
                grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
                gap: 8px;
            }
            .prize-card {
                padding: 10px 8px;
            }
            .prize-card .emoji {
                font-size: 1.5rem;
            }
            .prize-card .name {
                font-size: 0.78rem;
            }
            .modal {
                padding: 24px 16px 18px;
            }
            .modal__title {
                font-size: 1.4rem;
            }
            .modal__prize-name {
                font-size: 1.1rem;
            }
            .header__title {
                font-size: 1.6rem;
            }
        }
        @media (max-width: 380px) {
            .wheel-wrapper {
                width: min(320px, 78vw);
                height: min(320px, 78vw);
            }
            .controls {
                gap: 8px;
            }
            .btn {
                font-size: 0.8rem;
                padding: 10px 16px;
            }
            .btn-spin {
                font-size: 0.85rem;
                padding: 10px 20px;
            }
            .prizes-grid {
                grid-template-columns: 1fr 1fr;
                gap: 6px;
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- Заголовок -->
        <header class="header">
            <h1 class="header__title">Золотая Рулетка</h1>
            <p class="header__subtitle">Премиум-призы • Эксклюзивные дропы</p>
        </header>

        <!-- Секция с колесом -->
        <section class="wheel-section" aria-label="Колесо рулетки">
            <div class="wheel-wrapper" id="wheelWrapper">
                <div class="wheel-glow" id="wheelGlow" aria-hidden="true"></div>
                <canvas id="wheelCanvas" width="500" height="500" aria-label="Колесо рулетки с 6 секторами"></canvas>
                <div class="pointer" aria-hidden="true"></div>
            </div>

            <!-- Баланс и кнопки -->
            <div class="controls">
                <div class="balance-display">
                    <span class="icon">🎫</span>
                    <span>Спинов:</span>
                    <span class="count" id="spinCount">0</span>
                </div>
                <div class="btn-spin-wrapper" id="spinBtnWrapper">
                    <button class="btn btn-spin" id="btnSpin" disabled aria-label="Крутить колесо">
                        🔄 Крутить
                    </button>
                    <span class="tooltip" id="tooltipNoSpins">Купите спины на Playerok</span>
                </div>
                <button class="btn btn-buy" id="btnBuy" aria-label="Купить спины">
                    🛒 Купить спины
                </button>
                <button class="btn btn-paid hidden" id="btnPaid" aria-label="Я оплатил">
                    ✅ Я оплатил
                </button>
            </div>
            <p class="total-spins-display">
                Всего круток за всё время: <span id="totalSpinsCount">0</span>
            </p>
        </section>

        <!-- Список возможных выигрышей -->
        <section class="prizes-section" aria-label="Возможные выигрыши">
            <h2>🎁 Возможные выигрыши</h2>
            <div class="prizes-grid" id="prizesGrid">
                <!-- Заполняется через JS -->
            </div>
        </section>
    </div>

    <!-- Модальное окно результата -->
    <div class="modal-overlay hidden" id="modalOverlay" aria-hidden="true">
        <div class="modal" id="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
            <span class="modal__emoji" id="modalEmoji"></span>
            <h3 class="modal__title" id="modalTitle">Поздравляем!</h3>
            <p class="modal__prize-name" id="modalPrizeName"></p>
            <p class="modal__text" id="modalText"></p>
            <button class="modal__btn-close" id="modalBtnClose">Закрыть</button>
        </div>
    </div>

    <!-- Контейнер для конфетти -->
    <div class="confetti-container hidden" id="confettiContainer" aria-hidden="true"></div>

    <script>
        (function() {
            // ==================== КОНСТАНТЫ И КОНФИГУРАЦИЯ ====================

            /** Призы с их вероятностями (сумма = 1.0) */
            const PRIZES = [
                { name: 'Золотая бабочка', probability: 0.003, color: '#ffd740', rare: true,
                    emoji: '🦋', description: 'Эксклюзивный редчайший приз — золотая бабочка удачи.',
                    glowColor: '#ffe57f' },
                { name: 'Ф6х10', probability: 0.394, color: '#b87333', rare: false, emoji: '🎯',
                    description: '10 единиц формата Ф6 — увеличьте свои шансы.',
                    glowColor: null },
                { name: '60UC', probability: 0.200, color: '#a8b8c8', rare: false, emoji: '💰',
                    description: '60 условных единиц — приятный бонус к балансу.',
                    glowColor: null },
                { name: 'Золотой меч', probability: 0.003, color: '#ffc107', rare: true, emoji: '⚔️',
                    description: 'Легендарный золотой меч — символ абсолютной победы.',
                    glowColor: '#ffd54f' },
                { name: 'Ф6 золото', probability: 0.300, color: '#daa520', rare: false, emoji: '✨',
                    description: 'Золотой формат Ф6 — премиум-награда.',
                    glowColor: null },
                { name: 'Сопровождение VIP', probability: 0.100, color: '#e8c547', rare: false, emoji: '👑',
                    description: 'Персональное VIP-сопровождение высшего уровня.',
                    glowColor: null },
            ];

            // Проверка суммы вероятностей (должна быть ≈ 1.0)
            const probSum = PRIZES.reduce((sum, p) => sum + p.probability, 0);
            if (Math.abs(probSum - 1.0) > 0.001) {
                console.warn('Сумма вероятностей призов:', probSum, '— ожидается 1.0');
            }

            /** Ключи localStorage */
            const LS_KEY_SPINS = 'roulette_spins';
            const LS_KEY_TOTAL_SPINS = 'roulette_total_spins';

            /** Платёжная ссылка */
            const PAYMENT_URL = 'https://playerok.com/profile/DartvelUc/products';

            /** Настройки анимации колеса */
            const SPIN_DURATION_MS = 4500; // общая длительность вращения
            const MIN_FULL_ROTATIONS = 5; // минимум полных оборотов
            const MAX_FULL_ROTATIONS = 8; // максимум полных оборотов

            // ==================== DOM-ЭЛЕМЕНТЫ ====================

            const wheelCanvas = document.getElementById('wheelCanvas');
            const ctx = wheelCanvas.getContext('2d');
            const wheelWrapper = document.getElementById('wheelWrapper');
            const wheelGlow = document.getElementById('wheelGlow');
            const btnSpin = document.getElementById('btnSpin');
            const btnBuy = document.getElementById('btnBuy');
            const btnPaid = document.getElementById('btnPaid');
            const spinCountEl = document.getElementById('spinCount');
            const totalSpinsCountEl = document.getElementById('totalSpinsCount');
            const tooltipNoSpins = document.getElementById('tooltipNoSpins');
            const prizesGrid = document.getElementById('prizesGrid');
            const modalOverlay = document.getElementById('modalOverlay');
            const modal = document.getElementById('modal');
            const modalEmoji = document.getElementById('modalEmoji');
            const modalTitle = document.getElementById('modalTitle');
            const modalPrizeName = document.getElementById('modalPrizeName');
            const modalText = document.getElementById('modalText');
            const modalBtnClose = document.getElementById('modalBtnClose');
            const confettiContainer = document.getElementById('confettiContainer');

            // ==================== СОСТОЯНИЕ ПРИЛОЖЕНИЯ ====================

            /** Текущий угол поворота колеса (в радианах) */
            let currentAngle = 0;
            /** Идёт ли вращение сейчас */
            let isSpinning = false;
            /** ID анимации requestAnimationFrame */
            let animationId = null;
            /** Доступное количество спинов */
            let spinsBalance = 0;
            /** Общее число круток за всё время */
            let totalSpinsLifetime = 0;
            /** Ожидает ли пользователь подтверждения оплаты (не сохраняется между сессиями) */
            let paymentPending = false;

            // ==================== ИНИЦИАЛИЗАЦИЯ ====================

            /**
             * Загрузка данных из localStorage
             */
            function loadState() {
                try {
                    const savedSpins = localStorage.getItem(LS_KEY_SPINS);
                    spinsBalance = savedSpins !== null ? Math.max(0, parseInt(savedSpins, 10) || 0) : 0;

                    const savedTotal = localStorage.getItem(LS_KEY_TOTAL_SPINS);
                    totalSpinsLifetime = savedTotal !== null ? Math.max(0, parseInt(savedTotal, 10) || 0) : 0;
                } catch (e) {
                    console.warn('Ошибка чтения localStorage:', e);
                    spinsBalance = 0;
                    totalSpinsLifetime = 0;
                }
            }

            /**
             * Сохранение баланса спинов в localStorage
             */
            function saveSpinsBalance() {
                try {
                    localStorage.setItem(LS_KEY_SPINS, spinsBalance.toString());
                } catch (e) {
                    console.warn('Ошибка записи localStorage (spins):', e);
                }
            }

            /**
             * Сохранение общего счётчика круток
             */
            function saveTotalSpins() {
                try {
                    localStorage.setItem(LS_KEY_TOTAL_SPINS, totalSpinsLifetime.toString());
                } catch (e) {
                    console.warn('Ошибка записи localStorage (total_spins):', e);
                }
            }

            /**
             * Обновление интерфейса в соответствии с текущим состоянием
             */
            function updateUI() {
                // Обновление счётчика спинов
                spinCountEl.textContent = spinsBalance;

                // Обновление общего счётчика
                totalSpinsCountEl.textContent = totalSpinsLifetime;

                // Кнопка "Крутить"
                if (spinsBalance > 0 && !isSpinning) {
                    btnSpin.disabled = false;
                    btnSpin.textContent = '🔄 Крутить';
                    tooltipNoSpins.style.visibility = 'hidden';
                    tooltipNoSpins.style.opacity = '0';
                    wheelWrapper.classList.add('ready');
                } else if (isSpinning) {
                    btnSpin.disabled = true;
                    btnSpin.textContent = '⏳ Вращение...';
                    tooltipNoSpins.style.visibility = 'hidden';
                    tooltipNoSpins.style.opacity = '0';
                    wheelWrapper.classList.remove('ready');
                } else {
                    btnSpin.disabled = true;
                    btnSpin.textContent = '🔒 Крутить';
                    wheelWrapper.classList.remove('ready');
                }

                // Кнопка "Я оплатил"
                if (paymentPending && !isSpinning) {
                    btnPaid.classList.remove('hidden');
                } else {
                    btnPaid.classList.add('hidden');
                }

                // Подсказка при неактивной кнопке (только если спинов 0 и нет ожидания оплаты)
                if (spinsBalance === 0 && !paymentPending && !isSpinning) {
                    tooltipNoSpins.style.visibility = 'visible';
                    tooltipNoSpins.style.opacity = '1';
                } else if (spinsBalance > 0 || paymentPending || isSpinning) {
                    tooltipNoSpins.style.visibility = 'hidden';
                    tooltipNoSpins.style.opacity = '0';
                }
            }

            // ==================== РЕНДЕРИНГ КОЛЕСА ====================

            /**
             * Отрисовка колеса рулетки на Canvas
             * @param {number} rotationAngle - текущий угол поворота в радианах
             */
            function drawWheel(rotationAngle) {
                const w = wheelCanvas.width;
                const h = wheelCanvas.height;
                const cx = w / 2;
                const cy = h / 2;
                const outerRadius = w / 2 - 8; // внешний радиус колеса
                const innerRadius = outerRadius * 0.18; // внутренний круг (ступица)
                const numSectors = PRIZES.length;
                const sectorAngle = (2 * Math.PI) / numSectors; // 60° в радианах

                ctx.clearRect(0, 0, w, h);

                // Сохраняем контекст и применяем поворот
                ctx.save();
                ctx.translate(cx, cy);
                ctx.rotate(rotationAngle);

                // --- Отрисовка секторов ---
                for (let i = 0; i < numSectors; i++) {
                    const startAngle = -Math.PI / 2 + i * sectorAngle;
                    const endAngle = startAngle + sectorAngle;

                    // Градиент сектора
                    const midAngle = startAngle + sectorAngle / 2;
                    const gradX = Math.cos(midAngle) * outerRadius * 0.5;
                    const gradY = Math.sin(midAngle) * outerRadius * 0.5;
                    const gradient = ctx.createLinearGradient(0, 0, gradX, gradY);

                    if (PRIZES[i].rare) {
                        // Редкие призы — яркий золотой градиент
                        gradient.addColorStop(0, '#fff8dc');
                        gradient.addColorStop(0.3, PRIZES[i].color);
                        gradient.addColorStop(0.7, '#c8960c');
                        gradient.addColorStop(1, '#8b6914');
                    } else {
                        // Обычные призы
                        gradient.addColorStop(0, lightenColor(PRIZES[i].color, 0.35));
                        gradient.addColorStop(0.5, PRIZES[i].color);
                        gradient.addColorStop(1, darkenColor(PRIZES[i].color, 0.4));
                    }

                    // Рисуем сектор
                    ctx.beginPath();
                    ctx.moveTo(0, 0);
                    ctx.arc(0, 0, outerRadius, startAngle, endAngle);
                    ctx.closePath();
                    ctx.fillStyle = gradient;
                    ctx.fill();

                    // Обводка сектора
                    ctx.lineWidth = 1.5;
                    ctx.strokeStyle = 'rgba(255,255,255,0.15)';
                    ctx.stroke();

                    // --- Текст в секторе ---
                    ctx.save();
                    ctx.rotate(midAngle);
                    ctx.textAlign = 'center';
                    ctx.textBaseline = 'middle';
                    const textRadius = outerRadius * 0.64;

                    // Название приза
                    const fontSize = Math.min(outerRadius * 0.115, 20);
                    ctx.font = `600 ${fontSize}px 'Inter', sans-serif`;
                    ctx.fillStyle = '#ffffff';
                    ctx.shadowColor = 'rgba(0,0,0,0.6)';
                    ctx.shadowBlur = 3;
                    ctx.shadowOffsetX = 1;
                    ctx.shadowOffsetY = 1;

                    // Если название длинное — уменьшаем шрифт
                    const name = PRIZES[i].name;
                    const adjustedFontSize = name.length > 14 ? fontSize * 0.75 :
                        name.length > 10 ? fontSize * 0.85 : fontSize;
                    ctx.font = `600 ${adjustedFontSize}px 'Inter', sans-serif`;

                    ctx.fillText(name, textRadius, 0);

                    // Эмодзи (над названием)
                    ctx.shadowBlur = 0;
                    ctx.shadowOffsetX = 0;
                    ctx.shadowOffsetY = 0;
                    const emojiFontSize = fontSize * 1.3;
                    ctx.font = `${emojiFontSize}px 'Inter', sans-serif`;
                    ctx.fillText(PRIZES[i].emoji, textRadius, -fontSize * 1.1);

                    ctx.restore();

                    // --- Линия-разделитель между секторами ---
                    ctx.beginPath();
                    ctx.moveTo(0, 0);
                    ctx.lineTo(Math.cos(startAngle) * outerRadius, Math.sin(startAngle) * outerRadius);
                    ctx.strokeStyle = 'rgba(255,255,255,0.2)';
                    ctx.lineWidth = 1;
                    ctx.stroke();
                }

                // --- Внешняя золотая окантовка ---
                ctx.beginPath();
                ctx.arc(0, 0, outerRadius, 0, 2 * Math.PI);
                ctx.lineWidth = 5;
                ctx.strokeStyle = '#d4a843';
                ctx.shadowColor = 'rgba(212,168,67,0.7)';
                ctx.shadowBlur = 10;
                ctx.stroke();
                ctx.shadowBlur = 0;

                // --- Внутренняя тонкая окантовка ---
                ctx.beginPath();
                ctx.arc(0, 0, outerRadius - 7, 0, 2 * Math.PI);
                ctx.lineWidth = 1.5;
                ctx.strokeStyle = 'rgba(255,255,255,0.25)';
                ctx.stroke();

                // --- Декоративные лампочки по периметру ---
                const numLights = 36;
                for (let i = 0; i < numLights; i++) {
                    const lightAngle = (i / numLights) * 2 * Math.PI;
                    const lx = Math.cos(lightAngle) * (outerRadius - 2);
                    const ly = Math.sin(lightAngle) * (outerRadius - 2);
                    const isLit = Math.sin(lightAngle * 6 + Date.now() / 800) > 0;
                    ctx.beginPath();
                    ctx.arc(lx, ly, 3.5, 0, 2 * Math.PI);
                    ctx.fillStyle = isLit ? '#fffbe6' : '#8b7a4a';
                    ctx.shadowColor = isLit ? 'rgba(255,240,180,0.9)' : 'transparent';
                    ctx.shadowBlur = isLit ? 6 : 0;
                    ctx.fill();
                }
                ctx.shadowBlur = 0;

                // --- Центральная ступица ---
                const hubGradient = ctx.createRadialGradient(0, 0, innerRadius * 0.2, 0, 0, innerRadius);
                hubGradient.addColorStop(0, '#f7d774');
                hubGradient.addColorStop(0.5, '#c9973a');
                hubGradient.addColorStop(1, '#6b4c10');
                ctx.beginPath();
                ctx.arc(0, 0, innerRadius, 0, 2 * Math.PI);
                ctx.fillStyle = hubGradient;
                ctx.fill();
                ctx.lineWidth = 3;
                ctx.strokeStyle = '#e8c547';
                ctx.shadowColor = 'rgba(200,150,50,0.6)';
                ctx.shadowBlur = 8;
                ctx.stroke();
                ctx.shadowBlur = 0;

                // --- Крошечный блик в центре ---
                ctx.beginPath();
                ctx.arc(0, 0, innerRadius * 0.35, 0, 2 * Math.PI);
                ctx.fillStyle = '#fffef5';
                ctx.fill();

                ctx.restore(); // восстанавливаем исходное состояние
            }

            /**
             * Вспомогательная функция: осветление цвета
             */
            function lightenColor(hex, factor) {
                const r = parseInt(hex.slice(1, 3), 16);
                const g = parseInt(hex.slice(3, 5), 16);
                const b = parseInt(hex.slice(5, 7), 16);
                const lr = Math.min(255, Math.round(r + (255 - r) * factor));
                const lg = Math.min(255, Math.round(g + (255 - g) * factor));
                const lb = Math.min(255, Math.round(b + (255 - b) * factor));
                return `rgb(${lr},${lg},${lb})`;
            }

            /**
             * Вспомогательная функция: затемнение цвета
             */
            function darkenColor(hex, factor) {
                const r = parseInt(hex.slice(1, 3), 16);
                const g = parseInt(hex.slice(3, 5), 16);
                const b = parseInt(hex.slice(5, 7), 16);
                const dr = Math.max(0, Math.round(r * (1 - factor)));
                const dg = Math.max(0, Math.round(g * (1 - factor)));
                const db = Math.max(0, Math.round(b * (1 - factor)));
                return `rgb(${dr},${dg},${db})`;
            }

            // ==================== ВЗВЕШЕННЫЙ СЛУЧАЙНЫЙ ВЫБОР ====================

            /**
             * Выбирает индекс приза на основе взвешенных вероятностей
             * @returns {number} индекс приза в массиве PRIZES
             */
            function calculateWeightedPrize() {
                const rand = Math.random();
                let cumulative = 0;
                for (let i = 0; i < PRIZES.length; i++) {
                    cumulative += PRIZES[i].probability;
                    if (rand <= cumulative) {
                        return i;
                    }
                }
                // На случай погрешности округления — возвращаем последний
                return PRIZES.length - 1;
            }

            // ==================== АНИМАЦИЯ ВРАЩЕНИЯ ====================

            /**
             * Запускает вращение колеса
             */
            function spin() {
                if (isSpinning) return;
                if (spinsBalance <= 0) {
                    updateUI();
                    return;
                }

                // Списываем один спин
                spinsBalance--;
                totalSpinsLifetime++;
                saveSpinsBalance();
                saveTotalSpins();
                updateUI();

                // Блокируем интерфейс
                isSpinning = true;
                btnSpin.disabled = true;
                btnSpin.textContent = '⏳ Вращение...';
                wheelWrapper.classList.remove('ready');
                wheelGlow.classList.add('spinning');
                paymentPending = false;
                btnPaid.classList.add('hidden');
                tooltipNoSpins.style.visibility = 'hidden';
                tooltipNoSpins.style.opacity = '0';

                // Определяем приз
                const prizeIndex = calculateWeightedPrize();
                const prize = PRIZES[prizeIndex];
                const numSectors = PRIZES.length;
                const sectorAngle = (2 * Math.PI) / numSectors;

                // Вычисляем целевой угол: центр выбранного сектора должен оказаться под указателем (сверху, -π/2)
                // Центр сектора i в локальной системе: -π/2 + sectorAngle/2 + i * sectorAngle
                // Нужно, чтобы после поворота на targetAngle этот центр был в позиции -π/2
                // => -π/2 + sectorAngle/2 + i * sectorAngle - targetAngle ≡ -π/2 (mod 2π)
                // => targetAngle ≡ sectorAngle/2 + i * sectorAngle (mod 2π)
                const desiredAngleMod = (sectorAngle / 2 + prizeIndex * sectorAngle) % (2 * Math.PI);

                // Текущий угол по модулю 2π
                const currentMod = ((currentAngle % (2 * Math.PI)) + 2 * Math.PI) % (2 * Math.PI);

                // Разница: сколько нужно довернуть (вперёд, положительное направление)
                let delta = (desiredAngleMod - currentMod + 2 * Math.PI) % (2 * Math.PI);

                // Добавляем случайное количество полных оборотов
                const fullRotations = MIN_FULL_ROTATIONS + Math.floor(Math.random() * (MAX_FULL_ROTATIONS -
                    MIN_FULL_ROTATIONS + 1));
                delta += fullRotations * 2 * Math.PI;

                const startAngle = currentAngle;
                const targetAngle = currentAngle + delta;
                const startTime = performance.now();
                const duration = SPIN_DURATION_MS;

                /**
                 * Функция плавного замедления (ease-out cubic + небольшая отдача в конце)
                 */
                function easeOutCubic(t) {
                    return 1 - Math.pow(1 - t, 3);
                }

                /**
                 * Более сложная функция для реалистичной остановки:
                 * ease-out quintic с микро-отскоком
                 */
                function easeOutQuinticWithBounce(t) {
                    if (t >= 1) return 1;
                    if (t < 0.85) {
                        // Основное замедление
                        const scaled = t / 0.85;
                        return 1 - Math.pow(1 - scaled, 5);
                    } else {
                        // Финальная фаза с лёгким перелётом
                        const remaining = (t - 0.85) / 0.15;
                        const bounce = 1 + Math.sin(remaining * Math.PI * 1.5) * 0.015 * (1 - remaining);
                        return Math.min(1, bounce);
                    }
                }

                function animate(now) {
                    const elapsed = now - startTime;
                    const rawProgress = Math.min(elapsed / duration, 1);
                    const easedProgress = easeOutQuinticWithBounce(rawProgress);

                    currentAngle = startAngle + delta * easedProgress;
                    drawWheel(currentAngle);

                    if (rawProgress < 1) {
                        animationId = requestAnimationFrame(animate);
                    } else {
                        // Анимация завершена
                        currentAngle = targetAngle;
                        drawWheel(currentAngle);
                        onSpinComplete(prizeIndex);
                    }
                }

                animationId = requestAnimationFrame(animate);
            }

            /**
             * Обработчик завершения вращения
             * @param {number} prizeIndex - индекс выпавшего приза
             */
            function onSpinComplete(prizeIndex) {
                isSpinning = false;
                animationId = null;
                wheelGlow.classList.remove('spinning');
                updateUI();
                showResultModal(prizeIndex);
            }

            // ==================== МОДАЛЬНОЕ ОКНО РЕЗУЛЬТАТА ====================

            /**
             * Показывает модальное окно с результатом
             * @param {number} prizeIndex - индекс приза
             */
            function showResultModal(prizeIndex) {
                const prize = PRIZES[prizeIndex];

                modalEmoji.textContent = prize.emoji;
                modalPrizeName.textContent = prize.name;
                modalText.textContent = prize.description;

                if (prize.rare) {
                    modalTitle.textContent = '🎉 ДЖЕКПОТ! 🎉';
                    modalTitle.style.color = '#ffd700';
                    modal.classList.add('rare-win');
                    // Запускаем конфетти
                    spawnConfetti();
                    // Звуковой сигнал (высокий торжествующий)
                    playBeepSound(true);
                } else {
                    modalTitle.textContent = 'Поздравляем!';
                    modalTitle.style.color = '';
                    modal.classList.remove('rare-win');
                    // Обычный звук
                    playBeepSound(false);
                }

                modalOverlay.classList.remove('hidden');
                modalOverlay.setAttribute('aria-hidden', 'false');
                // Сброс анимации эмодзи
                modalEmoji.style.animation = 'none';
                void modalEmoji.offsetWidth; // рефлоу
                modalEmoji.style.animation = 'emojiBounce 0.6s ease-out 0.2s both';

                // Фокус на кнопке закрытия
                setTimeout(() => modalBtnClose.focus(), 100);
            }

            /**
             * Скрывает модальное окно
             */
            function hideModal() {
                modalOverlay.classList.add('hidden');
                modalOverlay.setAttribute('aria-hidden', 'true');
                modal.classList.remove('rare-win');
                confettiContainer.classList.add('hidden');
                confettiContainer.innerHTML = '';
                updateUI();
            }

            // ==================== КОНФЕТТИ ====================

            /**
             * Создаёт анимацию золотых конфетти
             */
            function spawnConfetti() {
                confettiContainer.classList.remove('hidden');
                confettiContainer.innerHTML = '';

                const colors = [
                    '#ffd700', '#f0c960', '#ffe57f', '#ffc107', '#ffab00',
                    '#fff8dc', '#daa520', '#f7d774', '#ffecb3', '#ffd54f',
                ];

                const pieceCount = 80;

                for (let i = 0; i < pieceCount; i++) {
                    const piece = document.createElement('div');
                    piece.classList.add('confetti-piece');
                    piece.style.left = Math.random() * 100 + '%';
                    piece.style.top = -(Math.random() * 60 + 10) + 'px';
                    piece.style.width = (Math.random() * 10 + 6) + 'px';
                    piece.style.height = (Math.random() * 10 + 6) + 'px';
                    piece.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                    piece.style.animationDuration = (Math.random() * 2 + 2.5) + 's';
                    piece.style.animationDelay = Math.random() * 1.5 + 's';
                    piece.style.borderRadius = Math.random() > 0.5 ? '50%' : '2px';
                    confettiContainer.appendChild(piece);
                }

                // Автоматическая очистка через 5 секунд
                setTimeout(() => {
                    confettiContainer.classList.add('hidden');
                    confettiContainer.innerHTML = '';
                }, 5000);
            }

            // ==================== ЗВУКОВОЙ СИГНАЛ ====================

            /**
             * Проигрывает короткий звуковой сигнал через Web Audio API
             * @param {boolean} isRare - флаг редкого приза (другой тон)
             */
            function playBeepSound(isRare) {
                try {
                    const audioCtx = new(window.AudioContext || window.webkitAudioContext)();
                    const oscillator = audioCtx.createOscillator();
                    const gainNode = audioCtx.createGain();

                    oscillator.connect(gainNode);
                    gainNode.connect(audioCtx.destination);

                    if (isRare) {
                        // Торжествующая восходящая трель
                        oscillator.type = 'sine';
                        oscillator.frequency.setValueAtTime(600, audioCtx.currentTime);
                        oscillator.frequency.linearRampToValueAtTime(1200, audioCtx.currentTime + 0.35);
                        oscillator.frequency.linearRampToValueAtTime(1600, audioCtx.currentTime + 0.6);
                        gainNode.gain.setValueAtTime(0.25, audioCtx.currentTime);
                        gainNode.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.7);
                        oscillator.start(audioCtx.currentTime);
                        oscillator.stop(audioCtx.currentTime + 0.7);
                    } else {
                        // Простой приятный сигнал
                        oscillator.type = 'triangle';
                        oscillator.frequency.setValueAtTime(800, audioCtx.currentTime);
                        oscillator.frequency.linearRampToValueAtTime(1000, audioCtx.currentTime + 0.2);
                        gainNode.gain.setValueAtTime(0.2, audioCtx.currentTime);
                        gainNode.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                        oscillator.start(audioCtx.currentTime);
                        oscillator.stop(audioCtx.currentTime + 0.3);
                    }
                } catch (e) {
                    // Беззвучный режим — не страшно
                }
            }

            // ==================== РЕНДЕРИНГ СПИСКА ПРИЗОВ ====================

            /**
             * Отрисовывает карточки призов под колесом
             */
            function renderPrizeList() {
                prizesGrid.innerHTML = '';

                PRIZES.forEach((prize) => {
                    const card = document.createElement('div');
                    card.classList.add('prize-card');
                    if (prize.rare) {
                        card.classList.add('rare');
                    }

                    const chancePercent = (prize.probability * 100).toFixed(1);

                    card.innerHTML = `
                        <span class="emoji">${prize.emoji}</span>
                        <span class="name">${prize.name}</span>
                        <span class="chance">Шанс: ${chancePercent}%</span>
                        ${prize.rare ? '<span class="badge">⭐ Эксклюзив</span>' : ''}
                    `;

                    prizesGrid.appendChild(card);
                });
            }

            // ==================== ПОКУПКА СПИНОВ ====================

            /**
             * Обработчик клика по кнопке "Купить спины"
             */
            function handleBuySpins() {
                // Открываем платёжную ссылку в новой вкладке
                window.open(PAYMENT_URL, '_blank', 'noopener,noreferrer');

                // Показываем кнопку подтверждения оплаты
                paymentPending = true;
                updateUI();

                // Прокручиваем к кнопке, если нужно
                btnPaid.scrollIntoView({ behavior: 'smooth', block: 'center' });
            }

            /**
             * Обработчик клика по кнопке "Я оплатил"
             */
            function handlePaymentConfirmed() {
                spinsBalance++;
                saveSpinsBalance();
                paymentPending = false;
                updateUI();

                // Краткая визуальная обратная связь
                spinCountEl.style.transform = 'scale(1.4)';
                spinCountEl.style.transition = 'transform 0.2s ease';
                setTimeout(() => {
                    spinCountEl.style.transform = 'scale(1)';
                }, 200);

                // Прокручиваем к кнопке "Крутить"
                btnSpin.scrollIntoView({ behavior: 'smooth', block: 'center' });
            }

            // ==================== ПРИВЯЗКА СОБЫТИЙ ====================

            btnSpin.addEventListener('click', () => {
                if (!isSpinning && spinsBalance > 0) {
                    spin();
                }
            });

            btnBuy.addEventListener('click', handleBuySpins);
            btnPaid.addEventListener('click', handlePaymentConfirmed);

            modalBtnClose.addEventListener('click', hideModal);

            // Закрытие модального окна по клику на оверлей
            modalOverlay.addEventListener('click', (e) => {
                if (e.target === modalOverlay) {
                    hideModal();
                }
            });

            // Закрытие модального окна по клавише Escape
            document.addEventListener('keydown', (e) => {
                if (e.key === 'Escape' && !modalOverlay.classList.contains('hidden')) {
                    hideModal();
                }
                // Пробел или Enter на кнопке "Крутить"
                if (e.key === ' ' || e.key === 'Enter') {
                    if (document.activeElement === btnSpin && !isSpinning && spinsBalance > 0) {
                        e.preventDefault();
                        spin();
                    }
                }
            });

            // ==================== АНИМАЦИЯ ПРОСТОЯ (лампочки) ====================

            /**
             * Рекурсивная анимация для обновления лампочек в режиме ожидания
             */
            function idleAnimationLoop() {
                if (!isSpinning) {
                    drawWheel(currentAngle);
                }
                requestAnimationFrame(() => {
                    if (!isSpinning) {
                        idleAnimationLoop();
                    }
                });
            }

            // ==================== ЗАПУСК ПРИЛОЖЕНИЯ ====================

            function initApp() {
                loadState();
                renderPrizeList();
                drawWheel(currentAngle);
                updateUI();

                // Запускаем цикл анимации лампочек
                idleAnimationLoop();

                console.log('🎰 Премиум Рулетка готова. Баланс спинов:', spinsBalance);
                console.log('💡 Купить спины:', PAYMENT_URL);
            }

            // Запуск после полной загрузки DOM
            if (document.readyState === 'loading') {
                document.addEventListener('DOMContentLoaded', initApp);
            } else {
                initApp();
            }
        })();
    </script>
</body>
</html>