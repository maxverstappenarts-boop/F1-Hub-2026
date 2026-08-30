index.hrml
<!DOCTYPE html> 
<html lang="ru" data-theme="dark"> 
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"> 
    <title>F1 Hub 2026</title> 
    <!-- Google Fonts: Outfit (для заголовков) и Inter (для текста) --> 
    <link rel="preconnect" href="https://fonts.googleapis.com"> 
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin> 
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Outfit:wght@600;800;900&display=swap" rel="stylesheet"> 
    
    <style> 
        :root { 
            --bg: #07090e; 
            --panel: #121721; 
            --text: #f0f6fc; 
            --muted: #8b949e; 
            --neon-cyan: #00f2fe; 
            --neon-pink: #ff007f; 
            --card-border: rgba(0, 242, 254, 0.15); 
            --glow-cyan: rgba(0, 242, 254, 0.25); 
            --ios-ease: cubic-bezier(0.16, 1, 0.3, 1); 
            --font-heading: 'Outfit', -apple-system, sans-serif; 
            --font-body: 'Inter', -apple-system, sans-serif; 
        } 

        * {  
            box-sizing: border-box;  
            margin: 0;  
            padding: 0;  
            font-family: var(--font-body);  
            -webkit-font-smoothing: antialiased;  
            -webkit-tap-highlight-color: transparent; 
        } 

        body {  
            background: var(--bg);  
            color: var(--text);  
            min-height: 100vh;  
            padding-bottom: 85px; 
            overflow-x: hidden;  
        } 

        h1, h2, h3, h4, .logo, .timer-val, .nav-btn { 
            font-family: var(--font-heading); 
        } 

        canvas#bgCanvas {  
            position: fixed;  
            top: 0;  
            left: 0;  
            width: 100%;  
            height: 100%;  
            z-index: -1;  
            opacity: 0.35;  
        } 

        header { 
            display: flex;  
            justify-content: center;  
            align-items: center; 
            padding: 1.1rem 1.2rem;  
            background: rgba(18, 23, 33, 0.85);  
            backdrop-filter: blur(12px); 
            -webkit-backdrop-filter: blur(12px); 
            border-bottom: 1px solid var(--card-border); 
            position: sticky; 
            top: 0; 
            z-index: 50; 
        } 

        .logo {  
            font-weight: 900;  
            font-size: 1.5rem;  
            letter-spacing: 1.5px; 
            background: linear-gradient(45deg, var(--neon-cyan), var(--neon-pink));  
            -webkit-background-clip: text;  
            -webkit-text-fill-color: transparent; 
            text-shadow: 0 0 15px var(--glow-cyan); 
        } 

        /* Bottom Nav Bar */ 
        nav {  
            display: flex;  
            justify-content: space-around; 
            padding: 8px 6px;  
            background: rgba(18, 23, 33, 0.92);  
            backdrop-filter: blur(16px); 
            -webkit-backdrop-filter: blur(16px); 
            border-top: 1px solid var(--card-border);  
            position: fixed; 
            bottom: 0; 
            left: 0; 
            right: 0; 
            z-index: 90; 
        } 

        .nav-btn {  
            background: none;  
            border: none;  
            color: var(--muted);  
            padding: 6px 12px;  
            font-size: 0.75rem; 
            font-weight: 600;  
            cursor: pointer;  
            border-radius: 12px;  
            transition: all 0.3s var(--ios-ease);  
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            gap: 4px; 
        } 

        .nav-btn.active {  
            color: var(--neon-cyan);  
            background: rgba(0, 242, 254, 0.12);  
            box-shadow: 0 0 16px var(--glow-cyan); 
        } 

        .nav-btn:active { transform: scale(0.92); } 

        .container { max-width: 800px; margin: 0 auto; padding: 12px; } 
        
        .tab-content { display: none; opacity: 0; transform: translateY(12px); transition: all 0.4s var(--ios-ease); } 
        .tab-content.active { display: block; opacity: 1; transform: translateY(0); } 

        /* Hero Timer */ 
        .hero { 
            background: var(--panel);  
            border-radius: 20px;  
            padding: 20px 16px;  
            margin-bottom: 16px; 
            border: 1px solid var(--card-border);  
            box-shadow: 0 8px 32px var(--glow-cyan); 
            display: flex;  
            flex-direction: column;  
            align-items: center;  
            text-align: center; 
            position: relative; 
            overflow: hidden; 
        } 

        .timer { display: flex; gap: 8px; margin-top: 14px; width: 100%; justify-content: center; } 
        .timer-box {  
            background: rgba(0, 0, 0, 0.35);  
            padding: 8px 12px;  
            border-radius: 12px;  
            border: 1px solid var(--card-border);  
            min-width: 65px; 
        } 
        .timer-val { font-size: 1.6rem; font-weight: 800; color: var(--neon-cyan); font-variant-numeric: tabular-nums; text-shadow: 0 0 10px var(--glow-cyan); } 
        .timer-lbl { font-size: 0.65rem; color: var(--muted); text-transform: uppercase; margin-top: 2px; font-weight: 600; } 

        .grid { display: grid; grid-template-columns: 1fr; gap: 12px; } 
        @media (min-width: 600px) { .grid { grid-template-columns: repeat(2, 1fr); } } 

        .race-card, .news-card {  
            background: var(--panel);  
            padding: 16px;  
            border-radius: 16px;  
            border: 1px solid var(--card-border);  
            transition: transform 0.3s var(--ios-ease), box-shadow 0.3s var(--ios-ease), border-color 0.3s var(--ios-ease); 
            box-shadow: 0 4px 16px rgba(0,0,0,0.2); 
            cursor: pointer;
        } 

        .race-card:hover, .news-card:hover {
            transform: translateY(-2px);
            border-color: rgba(0, 242, 254, 0.4);
            box-shadow: 0 6px 20px var(--glow-cyan);
        }

        .race-card.upcoming {  
            border-color: var(--neon-cyan);  
            box-shadow: 0 0 20px var(--glow-cyan);  
        } 
        .schedule-item { display: flex; justify-content: space-between; font-size: 0.8rem; margin-top: 6px; } 

        .news-card { margin-bottom: 14px; } 
        .news-tag { display: inline-block; font-size: 0.65rem; font-weight: 700; text-transform: uppercase; padding: 3px 8px; border-radius: 6px; background: rgba(0, 242, 254, 0.1); color: var(--neon-cyan); margin-bottom: 8px; } 
        .news-title { font-size: 1.05rem; font-weight: 800; color: var(--text); margin-bottom: 6px; line-height: 1.3; } 
        .news-date { font-size: 0.7rem; color: var(--muted); margin-bottom: 8px; } 
        .news-text { font-size: 0.85rem; color: #c9d1d9; line-height: 1.5; } 

        table { width: 100%; border-collapse: separate; border-spacing: 0; background: var(--panel); border-radius: 16px; overflow: hidden; border: 1px solid var(--card-border); } 
        th, td { padding: 12px 14px; text-align: left; border-bottom: 1px solid var(--card-border); font-size: 0.85rem; vertical-align: middle; } 
        th { color: var(--muted); font-size: 0.75rem; text-transform: uppercase; background: rgba(0,0,0,0.3); font-weight: 700; } 
        tr.clickable { cursor: pointer; transition: background 0.25s var(--ios-ease), transform 0.2s var(--ios-ease); } 
        tr.clickable:hover { background: rgba(0, 242, 254, 0.08); } 
        tr.clickable:active { background: rgba(0, 242, 254, 0.15); transform: scale(0.99); } 

        /* Аватары пилотов */
        .driver-cell { display: flex; align-items: center; gap: 12px; } 
        .driver-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
            border: 1.5px solid var(--neon-cyan);
            background: #000;
            box-shadow: 0 0 8px var(--glow-cyan);
            flex-shrink: 0;
        }

        /* Дополнения для Кубка Конструкторов */
        .team-row-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 6px; }
        .team-color-indicator { width: 4px; height: 18px; border-radius: 4px; display: inline-block; margin-right: 8px; vertical-align: middle; }
        .progress-bar-bg { width: 100%; height: 6px; background: rgba(255,255,255,0.08); border-radius: 4px; overflow: hidden; margin-top: 6px; }
        .progress-bar-fill { height: 100%; background: linear-gradient(90deg, var(--neon-cyan), var(--neon-pink)); border-radius: 4px; transition: width 0.8s var(--ios-ease); }
        .team-drivers-list { font-size: 0.7rem; color: var(--muted); margin-top: 4px; }

        /* Gemini Rainbow Glow Border Modal */ 
        .modal {  
            display: flex;  
            position: fixed;  
            top: 0;  
            left: 0;  
            width: 100%;  
            height: 100%;  
            background: rgba(0,0,0,0.8);  
            backdrop-filter: blur(16px);  
            -webkit-backdrop-filter: blur(16px);  
            z-index: 100;  
            justify-content: center;  
            align-items: flex-end;  
            opacity: 0;  
            pointer-events: none;  
            transition: opacity 0.4s var(--ios-ease);  
        } 
        
        .modal.active { opacity: 1; pointer-events: auto; } 
        
        /* Modal Wrap with Gemini Border */ 
        .modal-rainbow-wrap { 
            position: relative; 
            width: 100%; 
            max-width: 500px; 
            max-height: 88vh; 
            min-height: 42vh; 
            border-top-left-radius: 28px; 
            border-top-right-radius: 28px; 
            padding: 2px; 
            background: linear-gradient(60deg, #4285f4, #9b51e0, #ff4081, #00f2fe, #4285f4); 
            background-size: 300% 300%; 
            animation: geminiGlow 4s ease infinite; 
            box-shadow: 0 -10px 40px rgba(0, 242, 254, 0.3); 
            transform: translateY(100%);  
            transition: transform 0.4s var(--ios-ease); 
        } 

        @keyframes geminiGlow { 
            0% { background-position: 0% 50%; } 
            50% { background-position: 100% 50%; } 
            100% { background-position: 0% 50%; } 
        } 

        .modal.active .modal-rainbow-wrap { transform: translateY(0); } 

        .modal-content {  
            background: var(--panel);  
            width: 100%;  
            height: 100%; 
            border-top-left-radius: 26px;  
            border-top-right-radius: 26px;  
            display: flex; 
            flex-direction: column; 
            overflow: hidden; 
            position: relative; 
            padding: 24px 20px; 
        } 

        /* Pull Handle for Swipe */ 
        .swipe-handle-bar { 
            width: 100%; 
            padding: 8px 0; 
            display: flex; 
            justify-content: center; 
            position: absolute; 
            top: 0; 
            left: 0; 
            cursor: pointer;
        } 
        .swipe-indicator { 
            width: 40px; 
            height: 5px; 
            background: rgba(255, 255, 255, 0.3); 
            border-radius: 10px; 
        } 

        .modal-body-text { 
            overflow-y: auto; 
            margin-top: 12px; 
            padding-right: 4px; 
        } 
    </style> 
</head> 
<body> 

    <canvas id="bgCanvas"></canvas> 

    <header> 
        <div class="logo">F1 HUB 2026</div> 
    </header> 

    <div class="container"> 
        <!-- HERO TIMER --> 
        <div class="hero"> 
            <span id="currentRaceBadge" style="color:var(--neon-cyan); font-size:0.75rem; font-weight:700;">Загрузка этапа...</span> 
            <h3 id="gpTitle" style="margin-top:4px; font-size:1.3rem;">Загрузка данных F1...</h3> 
            <div id="sessionTarget" style="color:var(--muted); font-size:0.8rem; margin-top:2px;">Пожалуйста, подождите</div> 
            
            <div class="timer"> 
                <div class="timer-box"><div class="timer-val" id="d">00</div><div class="timer-lbl">Дней</div></div> 
                <div class="timer-box"><div class="timer-val" id="h">00</div><div class="timer-lbl">Часов</div></div> 
                <div class="timer-box"><div class="timer-val" id="m">00</div><div class="timer-lbl">Мин</div></div> 
                <div class="timer-box"><div class="timer-val" id="s">00</div><div class="timer-lbl">Сек</div></div> 
            </div> 
        </div> 

        <!-- TABS --> 
        <div id="tab-calendar" class="tab-content active"> 
            <div class="grid" id="calendarGrid">Загрузка календаря...</div> 
        </div> 

        <div id="tab-news" class="tab-content"> 
            <div id="newsContainer">Загрузка новостей...</div> 
        </div> 

        <div id="tab-drivers" class="tab-content"> 
            <table> 
                <thead> 
                    <tr><th>ПИЛОТ</th><th>КОМАНДА</th><th>ОЧКИ</th></tr> 
                </thead> 
                <tbody id="driversBody"> 
                    <tr><td colspan="3">Загрузка таблицы пилотов...</td></tr> 
                </tbody> 
            </table> 
        </div> 

        <div id="tab-constructors" class="tab-content"> 
            <table> 
                <thead> 
                    <tr><th>КОМАНДА & ОБЗОР</th><th style="text-align: right;">ОЧКИ</th></tr> 
                </thead> 
                <tbody id="constructorsBody"> 
                    <tr><td colspan="2">Загрузка Кубка Конструкторов...</td></tr> 
                </tbody> 
            </table> 
        </div> 
    </div> 

    <!-- BOTTOM NAV BAR --> 
    <nav> 
        <button class="nav-btn active" onclick="switchTab(event, 'calendar')"> 
            <span>🏁</span> Календарь 
        </button> 
        <button class="nav-btn" onclick="switchTab(event, 'news')"> 
            <span>📰</span> Новости 
        </button> 
        <button class="nav-btn" onclick="switchTab(event, 'drivers')"> 
            <span>🏎️</span> Пилоты 
        </button> 
        <button class="nav-btn" onclick="switchTab(event, 'constructors')"> 
            <span>🏆</span> Команды 
        </button> 
    </nav> 

    <!-- MODAL BIO (ONLY TEXT + GEMINI RAINBOW GLOW + SWIPE) --> 
    <div class="modal" id="bioModal" onclick="handleModalBackdropClick(event)"> 
        <div class="modal-rainbow-wrap" id="modalWrap"> 
            <div class="modal-content"> 
                <div class="swipe-handle-bar" onclick="closeBio()"> 
                    <div class="swipe-indicator"></div> 
                </div> 
                <h2 id="mName" style="font-size:1.4rem; margin-top:8px;">Пилот</h2> 
                <div id="mTeam" style="color:var(--neon-cyan); font-size:0.85rem; font-weight:700; margin-bottom:12px;">Команда</div> 
                <div class="modal-body-text"> 
                    <p id="mBio" style="font-size:0.95rem; line-height:1.7; color:#d0d7de;">Загрузка биографии...</p> 
                </div> 
            </div> 
        </div> 
    </div> 

    <script> 
        const API_BASE = 'https://api.jolpi.ca/ergast/f1/2026'; 

        let all24Races = []; 
        let realDrivers = []; 
        let realConstructors = []; 
        let activeTargetSessionTime = null; 

        const RU_DRIVER_NAMES = { 
            "Max Verstappen": "Ферстаппен, Макс", 
            "Lewis Hamilton": "Хэмилтон, Льюис", 
            "Charles Leclerc": "Леклер, Шарль", 
            "Lando Norris": "Норрис, Ландо", 
            "Fernando Alonso": "Алонсо, Фернандо", 
            "George Russell": "Расселл, Джордж", 
            "Carlos Sainz": "Сайнс, Карлос (младший)", 
            "Oscar Piastri": "Пиастри, Оскар", 
            "Sergio Pérez": "Перес, Серхио", 
            "Pierre Gasly": "Гасли, Пьер", 
            "Esteban Ocon": "Окон, Эстебан", 
            "Alexander Albon": "Албон, Александр", 
            "Nico Hülkenberg": "Хюлькенберг, Нико", 
            "Valtteri Bottas": "Боттас, Вальттери", 
            "Lance Stroll": "Стролл, Лэнс", 
            "Kevin Magnussen": "Магнуссен, Кевин", 
            "Zhou Guanyu": "Чжоу Гуаньюй", 
            "Liam Lawson": "Лоусон, Лиам", 
            "Oliver Bearman": "Берман, Оливер", 
            "Andrea Kimi Antonelli": "Антонелли, Андреа Кими", 
            "Jack Doohan": "Дуэн, Джек", 
            "Gabriel Bortoleto": "Бортолето, Габриэл" 
        }; 

        // Изображения пилотов высокой четкости
        const DRIVER_AVATARS = {
            "Max Verstappen": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/M/MAXVER01_Max_Verstappen/maxver01.png",
            "Lewis Hamilton": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/L/LEWHAM01_Lewis_Hamilton/lewham01.png",
            "Charles Leclerc": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/C/CHALEC01_Charles_Leclerc/chalec01.png",
            "Lando Norris": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/L/LANNOR01_Lando_Norris/lannor01.png",
            "Fernando Alonso": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/F/FERALO01_Fernando_Alonso/feralo01.png",
            "George Russell": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/G/GEORUS01_George_Russell/georus01.png",
            "Carlos Sainz": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/C/CARSAI01_Carlos_Sainz/carsai01.png",
            "Oscar Piastri": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/O/OSCPIA01_Oscar_Piastri/oscpia01.png",
            "Sergio Pérez": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/S/SERPER01_Sergio_Perez/serper01.png",
            "Pierre Gasly": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/P/PIEGAS01_Pierre_Gasly/piegas01.png",
            "Esteban Ocon": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/E/ESTOCO01_Esteban_Ocon/estoco01.png",
            "Alexander Albon": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/A/ALEALB01_Alexander_Albon/alealb01.png",
            "Nico Hülkenberg": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/N/NICHUL01_Nico_Hulkenberg/nichul01.png",
            "Valtteri Bottas": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/V/VALBOT01_Valtteri_Bottas/valbot01.png",
            "Lance Stroll": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/L/LANSTR01_Lance_Stroll/lanstr01.png",
            "Kevin Magnussen": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/K/KEVMAG01_Kevin_Magnussen/kevmag01.png",
            "Zhou Guanyu": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/G/GUAZHO01_Zhou_Guanyu/guazho01.png",
            "Liam Lawson": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/L/LIALAW01_Liam_Lawson/lialaw01.png",
            "Oliver Bearman": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/O/OLIBEA01_Oliver_Bearman/olibea01.png",
            "Andrea Kimi Antonelli": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/A/ANDANT01_Andrea_Kimi_Antonelli/andant01.png",
            "Jack Doohan": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/J/JACDOO01_Jack_Doohan/jacdoo01.png",
            "Gabriel Bortoleto": "https://media.formula1.com/d_driver_fallback_image.png/content/dam/fom-website/drivers/G/GABBOR01_Gabriel_Bortoleto/gabbor01.png"
        };

        // Нейтральная заглушка-силуэт (SVG), используется только если совсем ничего не удалось найти.
        // Раньше при ошибке загрузки картинка заменялась на фото Ферстаппена -- из-за этого
        // у нескольких разных пилотов отображалось одно и то же лицо. Теперь вместо этого
        // используется нейтральный силуэт, а затем в фоне подтягивается настоящее фото пилота.
        const FALLBACK_AVATAR_SVG = "data:image/svg+xml;utf8," + encodeURIComponent(
            "<svg xmlns='http://www.w3.org/2000/svg' width='100' height='100'>" +
            "<rect width='100' height='100' fill='#121721'/>" +
            "<circle cx='50' cy='38' r='20' fill='#2a3446'/>" +
            "<rect x='18' y='64' width='64' height='36' rx='18' fill='#2a3446'/>" +
            "</svg>"
        );

        // Кэш реальных фото пилотов, подтянутых с Wikipedia (ключ -- имя пилота)
        let driverAvatarCache = {};

        // Достаём настоящее фото пилота с Wikipedia (тот же источник, что уже используется для биографии)
        async function fetchDriverPhoto(name, wikiUrl) {
            if (driverAvatarCache[name] !== undefined) return driverAvatarCache[name];
            const ruTitle = RU_DRIVER_NAMES[name] || name;
            let photo = null;
            try {
                let res = await fetch(`https://ru.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(ruTitle)}`);
                if (res.ok) {
                    const data = await res.json();
                    photo = (data.thumbnail && data.thumbnail.source) || (data.originalimage && data.originalimage.source) || null;
                }
            } catch (e) {}
            if (!photo) {
                try {
                    let res = await fetch(`https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(name)}`);
                    if (res.ok) {
                        const data = await res.json();
                        photo = (data.thumbnail && data.thumbnail.source) || (data.originalimage && data.originalimage.source) || null;
                    }
                } catch (e) {}
            }
            driverAvatarCache[name] = photo;
            return photo;
        }

        // Вызывается, если основная ссылка на фото сломана. Больше НЕ подставляет фото другого
        // пилота -- сначала показывает нейтральный силуэт, а затем пытается подтянуть настоящее
        // фото именно этого пилота с Wikipedia.
        function handleAvatarError(imgEl, name, wikiUrl) {
            imgEl.onerror = null;
            if (driverAvatarCache[name] !== undefined) {
                imgEl.src = driverAvatarCache[name] || FALLBACK_AVATAR_SVG;
                return;
            }
            imgEl.src = FALLBACK_AVATAR_SVG;
            fetchDriverPhoto(name, wikiUrl).then(photo => {
                if (photo) imgEl.src = photo;
            });
        }

        // Цвета команд для Кубка Конструкторов
        const TEAM_COLORS = {
            "Red Bull": "#3671C6",
            "Ferrari": "#E80020",
            "Mercedes": "#27F4D2",
            "McLaren": "#FF8000",
            "Aston Martin": "#229971",
            "Alpine": "#0093CC",
            "Williams": "#64C4FF",
            "RB": "#6692FF",
            "Sauber": "#52E252",
            "Haas F1 Team": "#B6BABD"
        };

        // Интересные новости про личную жизнь, семьи и атмосферу в командах 
        const F1_NEWS_DATA = [ 
            {  
                tag: "Семья и Жизнь", 
                title: "Льюис Хэмилтон о переезде в Маранелло и поддержке семьи",  
                text: "Льюис признался, что адаптация в Италии проходит очень тепло. Его отец Энтони и семья регулярно приезжают на базы Ferrari, чтобы поддержать его перед важнейшим этапом в карьере.",  
                time: "2 часа назад"  
            }, 
            {  
                tag: "За кулисами", 
                title: "Макс Ферстаппен проводит свободное время за симрейсингом вместе с папой",  
                text: "Даже во время перерывов в чемпионате Макс вместе со своим отцом Йосом организует виртуальные суточные марафоны. Пилот Red Bull отмечает, что семейная страсть к гонкам помогает ему оставаться в форме.",  
                time: "6 часов назад"  
            }, 
            {  
                tag: "Атмосфера в командe", 
                title: "Шарль Леклер о дружбе и соперничестве в Ferrari",  
                text: "Леклер поделился, как меняется микроклимат в гараже: 'Мы сохраняем отличные отношения вне трассы, много общаемся с семьями, но в болиде каждый из нас борется за максимум'.",  
                time: "14 часов назад"  
            }, 
            {  
                tag: "Личная жизнь", 
                title: "Ландо Норрис показал, как проводит уик-энд с близкими",  
                text: "Британский гонщик опубликовал кадры отдыха с семьей перед вылетом на очередной этап. По словам Ландо, именно поддержка родных помогает ему справляться с прессингом борьбы за титул.",  
                time: "21 час назад"  
            } 
        ]; 

        function loadNews() { 
            const container = document.getElementById('newsContainer'); 
            container.innerHTML = F1_NEWS_DATA.map(item => ` 
                <div class="news-card"> 
                    <span class="news-tag">${item.tag}</span> 
                    <div class="news-title">${item.title}</div> 
                    <div class="news-date">${item.time}</div> 
                    <div class="news-text">${item.text}</div> 
                </div> 
            `).join(''); 
        } 

        async function fetchCalendar() { 
            try { 
                const res = await fetch(`${API_BASE}.json`); 
                const data = await res.json(); 
                const races = data.MRData.RaceTable.Races || []; 

                all24Races = races.map(r => ({ 
                    round: parseInt(r.round), 
                    name: r.raceName, 
                    loc: `${r.Circuit.Location.locality}, ${r.Circuit.Location.country}`, 
                    dateStr: r.date, 
                    raceTime: `${r.date}T${r.time || '12:00:00Z'}`, 
                    Qualifying: r.Qualifying ? `${r.Qualifying.date}T${r.Qualifying.time}` : null 
                })); 

                renderCalendar(); 
            } catch (err) { 
                document.getElementById('calendarGrid').innerText = "Ошибка загрузки календаря."; 
            } 
        } 

        async function fetchDriverStandings() { 
            try { 
                const res = await fetch(`${API_BASE}/driverstandings.json`); 
                const data = await res.json(); 
                const standings = data.MRData.StandingsTable.StandingsLists[0]?.DriverStandings || []; 

                realDrivers = standings 
                    .map(d => ({ 
                        name: `${d.Driver.givenName} ${d.Driver.familyName}`, 
                        wikiUrl: d.Driver.url, 
                        team: d.Constructors[0]?.name || 'N/A', 
                        pts: d.points 
                    })) 
                    .filter(d => !d.name.toLowerCase().includes('tsunoda') && !d.name.toLowerCase().includes('цунода')) 
                    .slice(0, 22); 

                renderDrivers(); 
                renderConstructors(); // Перерисовываем для привязки гонщиков к командам
            } catch (err) { console.error(err); } 
        } 

        async function fetchConstructorStandings() { 
            try { 
                const res = await fetch(`${API_BASE}/constructorstandings.json`); 
                const data = await res.json(); 
                const standings = data.MRData.StandingsTable.StandingsLists[0]?.ConstructorStandings || []; 

                realConstructors = standings.map(c => ({ 
                    team: c.Constructor.name, 
                    pts: c.points 
                })); 

                renderConstructors(); 
            } catch (err) { console.error(err); } 
        } 

        function renderCalendar() { 
            if (!all24Races.length) return; 
            const now = new Date().getTime(); 
            let upcomingFound = false; 

            document.getElementById('calendarGrid').innerHTML = all24Races.map(r => { 
                const raceDate = new Date(r.raceTime); 
                const qualDate = r.Qualifying ? new Date(r.Qualifying) : new Date(raceDate.getTime() - (24 * 3600 * 1000)); 
                const isUpcoming = (raceDate.getTime() + (2 * 3600 * 1000) >= now) && !upcomingFound; 

                if (isUpcoming) { 
                    upcomingFound = true; 
                    setupHeroTimer(r, raceDate.getTime()); 
                } 

                return ` 
                    <div class="race-card ${isUpcoming ? 'upcoming' : ''}"> 
                        <div style="font-size:0.7rem; color:var(--neon-cyan); font-weight:700;">ЭТАП ${r.round}</div> 
                        <h4 style="margin: 2px 0; font-size:1.1rem;">${r.name}</h4> 
                        <div style="font-size: 0.75rem; color: var(--muted); margin-bottom:8px;">${r.loc}</div> 
                        <div class="schedule-list"> 
                            <div class="schedule-item"> 
                                <span style="color:var(--muted);">Квалификация</span> 
                                <span style="color:var(--neon-cyan); font-weight:600;">${qualDate.toLocaleTimeString([], {weekday:'short', hour:'2-digit', minute:'2-digit'})}</span> 
                            </div> 
                            <div class="schedule-item"> 
                                <span style="color:var(--muted);">Гонка</span> 
                                <span style="color:var(--neon-cyan); font-weight:600;">${raceDate.toLocaleTimeString([], {weekday:'short', hour:'2-digit', minute:'2-digit'})}</span> 
                            </div> 
                        </div> 
                    </div> 
                `; 
            }).join(''); 
        } 

        function renderDrivers() { 
            const tbody = document.getElementById('driversBody'); 
            tbody.innerHTML = realDrivers.map((d, i) => {
                const hasStatic = !!DRIVER_AVATARS[d.name];
                const avatar = hasStatic ? DRIVER_AVATARS[d.name] : (driverAvatarCache[d.name] || FALLBACK_AVATAR_SVG);
                return ` 
                    <tr class="clickable" onclick="fetchWikiBio('${d.wikiUrl}', '${d.name}', '${d.team}')"> 
                        <td> 
                            <div class="driver-cell">
                                <img id="avatar-${i}" src="${avatar}" alt="${d.name}" class="driver-avatar" loading="lazy" onerror="handleAvatarError(this, '${d.name.replace(/'/g, "\\'")}', '${(d.wikiUrl || '').replace(/'/g, "\\'")}')">
                                <strong>${d.name}</strong> 
                            </div>
                        </td> 
                        <td style="color:var(--muted);">${d.team}</td> 
                        <td><strong style="color:var(--neon-cyan);">${d.pts}</strong></td> 
                    </tr> 
                `;
            }).join(''); 

            // Для пилотов без статичной ссылки на фото сразу подгружаем реальное фото с Wikipedia,
            // чтобы никто не оставался с общей заглушкой или чужим лицом.
            realDrivers.forEach((d, i) => {
                if (!DRIVER_AVATARS[d.name] && driverAvatarCache[d.name] === undefined) {
                    fetchDriverPhoto(d.name, d.wikiUrl).then(photo => {
                        const imgEl = document.getElementById(`avatar-${i}`);
                        if (imgEl && photo) imgEl.src = photo;
                    });
                }
            });
        } 

        function renderConstructors() { 
            if (!realConstructors.length) return;
            
            const maxPts = Math.max(...realConstructors.map(c => parseFloat(c.pts) || 1));
            
            document.getElementById('constructorsBody').innerHTML = realConstructors.map(c => {
                const teamColor = TEAM_COLORS[c.team] || 'var(--neon-cyan)';
                const percentage = Math.min(100, Math.max(5, (parseFloat(c.pts) / maxPts) * 100));
                
                // Собираем пилотов команды
                const teamDrivers = realDrivers
                    .filter(d => d.team.toLowerCase().includes(c.team.toLowerCase()) || c.team.toLowerCase().includes(d.team.toLowerCase()))
                    .map(d => d.name)
                    .join(' • ');

                return ` 
                    <tr> 
                        <td style="padding: 14px;">
                            <div class="team-row-header">
                                <div>
                                    <span class="team-color-indicator" style="background:${teamColor}; box-shadow: 0 0 8px ${teamColor};"></span>
                                    <strong style="font-size:0.95rem;">${c.team}</strong>
                                </div>
                            </div>
                            ${teamDrivers ? `<div class="team-drivers-list">🏎️ ${teamDrivers}</div>` : ''}
                            <div class="progress-bar-bg">
                                <div class="progress-bar-fill" style="width: ${percentage}%; background: ${teamColor};"></div>
                            </div>
                        </td> 
                        <td style="text-align: right;"><strong style="color:var(--neon-cyan); font-size:1.05rem;">${c.pts}</strong></td> 
                    </tr> 
                `;
            }).join(''); 
        } 

        function setupHeroTimer(raceObj, targetTime) { 
            document.getElementById('currentRaceBadge').innerText = `Этап ${raceObj.round} • ${raceObj.name}`; 
            document.getElementById('gpTitle').innerText = raceObj.name; 
            document.getElementById('sessionTarget').innerText = `До Главной Гонки`; 
            activeTargetSessionTime = targetTime; 
        } 

        function updateCountdown() { 
            if (!activeTargetSessionTime) return; 
            const diff = activeTargetSessionTime - new Date().getTime(); 

            if (diff <= 0) { 
                ['d','h','m','s'].forEach(id => document.getElementById(id).innerText = '00'); 
                return; 
            } 

            document.getElementById('d').innerText = String(Math.floor(diff / (1000 * 60 * 60 * 24))).padStart(2, '0'); 
            document.getElementById('h').innerText = String(Math.floor((diff / (1000 * 60 * 60)) % 24)).padStart(2, '0'); 
            document.getElementById('m').innerText = String(Math.floor((diff / 1000 / 60) % 60)).padStart(2, '0'); 
            document.getElementById('s').innerText = String(Math.floor((diff / 1000) % 60)).padStart(2, '0'); 
        } 

        async function fetchWikiBio(url, name, team) { 
            document.getElementById('mName').innerText = name; 
            document.getElementById('mTeam').innerText = team; 
            document.getElementById('mBio').innerText = "Загрузка биографии..."; 
            
            const modal = document.getElementById('bioModal'); 
            const wrap = document.getElementById('modalWrap'); 
            wrap.style.transform = ''; 
            modal.classList.add('active'); 

            const ruTitle = RU_DRIVER_NAMES[name] || name; 

            try { 
                let res = await fetch(`https://ru.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(ruTitle)}`); 
                if (!res.ok) res = await fetch(`https://ru.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(name)}`); 

                const data = await res.json(); 
                if (data.extract) { 
                    document.getElementById('mBio').innerText = data.extract; 
                } else { 
                    document.getElementById('mBio').innerText = "Информация временно недоступна."; 
                } 
            } catch (e) { 
                document.getElementById('mBio').innerText = "Не удалось загрузить биографию пилота."; 
            } 
        } 

        function handleModalBackdropClick(e) {
            if (e.target.id === 'bioModal') {
                closeBio();
            }
        }

        // Swipe Down Gesture Control for Modal Close 
        const modalWrap = document.getElementById('modalWrap'); 
        let startY = 0; 
        let currentY = 0; 

        modalWrap.addEventListener('touchstart', (e) => { 
            startY = e.touches[0].clientY; 
        }, { passive: true }); 

        modalWrap.addEventListener('touchmove', (e) => { 
            currentY = e.touches[0].clientY; 
            const diffY = currentY - startY; 
            if (diffY > 0) { 
                modalWrap.style.transform = `translateY(${diffY}px)`; 
            } 
        }, { passive: true }); 

        modalWrap.addEventListener('touchend', () => { 
            const diffY = currentY - startY; 
            if (diffY > 100) { 
                closeBio(); 
            } else { 
                modalWrap.style.transform = 'translateY(0)'; 
            } 
            startY = 0; 
            currentY = 0; 
        }); 

        function closeBio() {  
            const modal = document.getElementById('bioModal'); 
            modal.classList.remove('active');  
        } 

        function switchTab(evt, tabName) { 
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active')); 
            document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active')); 
            
            const btn = evt.currentTarget || evt.target; 
            btn.classList.add('active'); 
            document.getElementById(`tab-${tabName}`).classList.add('active'); 
        } 

        function refreshAllData() { 
            fetchCalendar(); 
            fetchDriverStandings(); 
            fetchConstructorStandings(); 
            loadNews(); 
        } 

        // Canvas Background Particles 
        const canvas = document.getElementById('bgCanvas'); 
        const ctx = canvas.getContext('2d'); 
        let w, h, particles = []; 

        function resize() { w = canvas.width = window.innerWidth; h = canvas.height = window.innerHeight; } 
        window.addEventListener('resize', resize); 
        resize(); 

        class Particle { 
            constructor() { this.reset(); } 
            reset() { 
                this.x = Math.random() * w; this.y = Math.random() * h; 
                this.size = Math.random() * 2 + 0.5; 
                this.vx = (Math.random() - 0.5) * 0.3; this.vy = (Math.random() - 0.5) * 0.3; 
            } 
            update() { 
                this.x += this.vx; this.y += this.vy; 
                if (this.x < 0 || this.x > w || this.y < 0 || this.y > h) this.reset(); 
            } 
            draw() { 
                ctx.fillStyle = '#00f2fe'; 
                ctx.beginPath(); ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2); ctx.fill(); 
            } 
        } 
        particles = Array.from({ length: 25 }, () => new Particle()); 
        function animate() { 
            ctx.clearRect(0, 0, w, h); 
            particles.forEach(p => { p.update(); p.draw(); }); 
            requestAnimationFrame(animate); 
        } 

        // Init 
        refreshAllData(); 
        setInterval(updateCountdown, 1000); 
        setInterval(refreshAllData, 60000); // Автоматическое обновление каждые 60 секунд
        animate(); 
    </script> 
</body> 
</html>
