<html lang="uk">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Тренажер тестів</title>
<style>
  :root{
    --bg:#0f1724;
    --card:#111827;
    --muted:#9ca3af;
    --accent:#3b82f6;
    --good:#10b981;
    --bad:#ef4444;
    --glass: rgba(255,255,255,0.03);
    --panel:#0b1220;
    --font-sans: "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  }
  html,body{height:100%;margin:0;background:linear-gradient(180deg,#041022 0%, #071428 60%);color:#e6eef8;font-family:var(--font-sans);}
  .app{max-width:1100px;margin:28px auto;padding:22px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));box-shadow:0 6px 30px rgba(2,6,23,0.7);}
  header{display:flex;align-items:center;justify-content:space-between;gap:16px;margin-bottom:18px;}
  header h1{font-size:20px;margin:0;font-weight:600}
  header p{margin:0;color:var(--muted);font-size:13px}
  .row{display:flex;gap:18px;align-items:flex-start;}
  .col{background:var(--panel);padding:14px;border-radius:10px;flex:1;box-shadow:inset 0 1px 0 rgba(255,255,255,0.02);}
  .col.small{flex:0 0 340px;}
  label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
  input[type="number"], select, textarea, input[type="text"]{width:100%;padding:8px 10px;border-radius:8px;background:var(--glass);border:1px solid rgba(255,255,255,0.03);color:inherit;font-size:14px;box-sizing:border-box}
  input[type="file"] { border: 1px solid var(--accent); cursor: pointer; padding: 8px 10px; border-radius: 8px; background: rgba(59,130,246,0.1); width: 100%; box-sizing: border-box; }
  textarea{min-height:180px;font-family:monospace;resize:vertical}
  .muted{color:var(--muted);font-size:13px}
  button{background:var(--accent);color:white;padding:10px 14px;border-radius:10px;border:0;font-weight:600;cursor:pointer; transition: background-color 0.2s;}
  button:hover{background: #2563eb;}
  button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.04); color: var(--muted);}
  button.ghost:hover{background: rgba(255,255,255,0.05);}
  .category-list{max-height:220px;overflow:auto;padding-right:8px}
  .cat-item{display:flex;align-items:center;justify-content:space-between;padding:8px;border-radius:8px;margin-bottom:8px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent)}
  .cat-left{display:flex;gap:10px;align-items:center}
  .checkbox{width:18px;height:18px;border-radius:4px;border:1px solid rgba(255,255,255,0.06);display:inline-block;background:transparent;cursor:pointer}
  .counter{font-size:13px;color:var(--muted)}
  .settings-row{display:flex;gap:8px;margin-top:8px}
  .tiny{font-size:12px;color:var(--muted)}
  .start-row{display:flex;gap:12px;align-items:center;margin-top:12px}
  .progress-wrap{height:12px;background:rgba(255,255,255,0.03);border-radius:8px;overflow:hidden;margin-top:12px}
  .progress{height:100%;width:0;background:linear-gradient(90deg,var(--accent),#60a5fa); transition: width 0.3s ease;}
  /* Screen management */
  .screen{display:none}
  .setup-screen-col{display:block;}
  .setup-screen-main{display:block;}
  .testing-screen, .result-screen, .stats-only-screen{display:none}
  
  .test-screen{display:none;flex-direction:column;gap:14px}
  .question-card{padding:14px;border-radius:10px;background:linear-gradient(180deg, rgba(255,255,255,0.015), transparent);box-shadow:0 4px 16px rgba(2,6,23,0.6)}
  .answers{display:flex;flex-direction:column;gap:8px;margin-top:8px}
  .answer{padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);cursor:pointer; transition: all 0.2s;}
  .answer:hover{border-color: rgba(255,255,255,0.1); background: rgba(255,255,255,0.03);}
  .answer.correct{border-color:rgba(16,185,129,0.4);background:rgba(16,185,129,0.06);box-shadow:0 0 10px rgba(16,185,129,0.2)}
  .answer.wrong-selected{border-color:rgba(239,68,68,0.4);background:rgba(239,68,68,0.04)}
  .meta{display:flex;gap:12px;align-items:center;justify-content:space-between; flex-wrap: wrap;}
  .small-pill{padding:6px 8px;border-radius:10px;background:rgba(255,255,255,0.02);font-size:13px}
  .result-screen{display:none;padding-top:8px}
  table{width:100%;border-collapse:collapse}
  th,td{padding:8px;border-bottom:1px dashed rgba(255,255,255,0.02);text-align:left;font-size:13px;color:var(--muted)}
  th{color:white; font-weight: 600;}
  .heatmap{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:8px}
  .heat-card{padding:10px;border-radius:8px;text-align:center}
  canvas{width:100%;height:160px;background:transparent;border-radius:6px}
  footer{margin-top:18px;font-size:12px;color:var(--muted);display:flex;justify-content:space-between;align-items:center}
  .topline{display:flex;gap:12px;align-items:center}
  .hint{font-size:12px;color:var(--muted)}
  .flex{display:flex;gap:8px;align-items:center; flex-wrap: wrap;}
  .answer-comment{margin-top:12px;padding:12px;border-radius:8px;background:rgba(255,255,255,0.02);border-left:4px solid var(--accent);font-size:14px;line-height:1.5;display:none}
  /* Загальні стилі для таблиць */
#categoryTable, 
#badQuestionsTable { 
    width: 100%;
    border-collapse: collapse;
    color: #e6eef8; /* Забезпечити світлий колір тексту */
    /* Важливо: задати фон для всієї таблиці як запасний варіант */
    background-color: var(--card, #111827); 
    border-radius: 6px;
    overflow: hidden; /* Допомагає для коректного відображення border-radius */
}

/* Стилі для заголовків таблиць (thead) */
#categoryTable thead th,
#badQuestionsTable thead th {
    background-color: var(--bg, #0f1724); /* Темніший колір для заголовка */
    padding: 12px 10px;
    font-weight: 600;
    text-align: left;
    border: 1px solid var(--glass, rgba(255, 255, 255, 0.1));
}

/* Стилі для тіла таблиці (tbody) */
#categoryTable tbody td,
#badQuestionsTable tbody td {
    padding: 10px;
    /* Колір межі для кращої видимості */
    border: 1px solid var(--glass, rgba(255, 255, 255, 0.1)); 
}

/* Стилі для чергування рядків у тілі таблиці (щоб не були білими) */
#categoryTable tbody tr:nth-child(even),
#badQuestionsTable tbody tr:nth-child(even) {
    background-color: var(--panel, #0b1220); /* Найтемніший фон */
}

#categoryTable tbody tr:nth-child(odd),
#badQuestionsTable tbody tr:nth-child(odd) {
    background-color: var(--card, #111827); /* Трохи світліший фон */
}

/* Додатково: Стиль для контейнера статистики */
/* Припускаючи, що div-обгортка, де знаходиться статистика (ваші два фрагменти), має клас stats-section */
.stats-section {
    background-color: var(--card, #111827); 
    padding: 20px; 
    border-radius: 8px;
    margin-top: 20px;
}  
  @media (max-width:900px){.row{flex-direction:column}.col.small{flex:unset}}
</style>
</head>
<body>
<div class="app">
  <header>
    <div>
      <h1>Тренажер тестів</h1>
      <p class="muted" id="headerNotice">Працює офлайн. Вбудуйте свій JSON-тест у код, або завантажте файл.</p>
    </div>
    <div class="topline">
      <div class="hint">Розробник - СІГСПП молодший сержант Войтович Євген • Інтерфейс українською • Зберігає історію в пам'яті браузера • v0.1.1</div>
    </div>
  </header>

  <div class="row">
    <div class="col small setup-screen-col" id="setupCol">
      <div id="setupControls">
        <label for="fileInput" id="fileLabel" style="display:none;">Завантажити файл тесту (JSON)</label>
        <input type="file" id="fileInput" accept=".json" style="display:none; margin-bottom: 15px;" />

        <div id="testConfiguration" style="display:none;">
          <label>Налаштування тесту</label>
          <div style="display:flex;gap:8px;">
            <input id="qCount" type="number" min="1" value="25" />
            <input id="timeMin" type="number" min="1" value="30" />
            <input id="maxMistakes" type="number" min="0" value="5" />
          </div>
          <div class="tiny muted" style="margin-top:6px">Кількість питань • Час (хв) • Максимум помилок</div>

          <div style="margin-top:10px">
            <label>Опції</label>
            <div style="display:flex;gap:8px;align-items:center;margin-top:6px">
              <input id="priorityToggle" type="checkbox" />
              <div style="flex:1">
                <div style="font-size:13px">Пріоритет питань із помилками</div>
                <div class="muted tiny">Працює тільки якщо є попередні проходження</div>
              </div>
            </div>
          </div>

          <div style="margin-top:12px">
            <label>Категорії (обрати)</label>
            <div class="category-list" id="categoryList">
              </div>
          </div>
        </div>

        <div class="start-row">
          <button id="startBtn" disabled>Почати тест</button>
          <button id="statsBtn" class="ghost" disabled>Статистика</button>
          <div style="flex:1"></div>
        </div>
        <div class="start-row">
          <button id="resetStats" class="ghost">Очистити статистику</button>
        </div>
      </div>
    </div>

    <div class="col">
      <div id="setupSummary" class="setup-screen-main" style="margin-bottom:10px">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <div style="font-size:16px;font-weight:600" id="testTitle">(Тест не завантажено)</div>
            <div class="muted tiny" id="testInfoLine">Інформація про тест відсутня</div>
          </div>
          <div style="text-align:right">
            <div class="small-pill" id="storageInfo">Історія: —</div>
            <div style="height:6px"></div>
          </div>
        </div>
      </div>

      <div id="configNotice" class="muted setup-screen-main">Будь ласка, завантажте файл тесту (JSON) або вбудуйте його в код, щоб продовжити.</div>

      <div class="progress-wrap" aria-hidden="true"><div class="progress" id="globalProgress"></div></div>

      <div id="testScreen" class="test-screen testing-screen">
        <div class="meta">
          <div class="flex">
            <div class="small-pill" id="qMeta">Питання 0/0</div>
            <div class="small-pill" id="timer">00:00</div>
            <div class="small-pill" id="mistakes">Помилок: 0</div>
            <div class="small-pill" id="score">Правильно: 0</div>
          </div>
          <div class="flex">
            <button id="endEarly" class="ghost">Завершити зараз</button>
            <button id="backToSetupFromTest" class="ghost">Налаштування</button>
          </div>
        </div>

        <div class="question-card">
          <div id="questionText" style="font-weight:600"></div>
          <div class="muted tiny" id="questionCategory"></div>
          <div class="answers" id="answersList"></div>
          <div id="answerComment" class="answer-comment"></div>
          <div style="margin-top:12px;text-align:right">
            <button id="nextQBtn" style="display:none">Далі →</button>
          </div>
        </div>
      </div>

      <div id="resultScreen" class="result-screen screen">
        <h3 style="margin:8px 0 6px 0" id="resultsTitle">Результат тесту</h3>
        <div style="display:flex;gap:12px;flex-wrap:wrap">
          <div class="col small" style="flex:0 0 260px">
            <div style="font-size:14px;font-weight:700" id="finalPercent">0%</div>
            <div class="muted tiny">Успішність</div>
            <div style="margin-top:10px"><div id="finalCounts" class="muted tiny"></div></div>
            <div style="margin-top:10px">
              <div style="font-size:13px;font-weight:600">Прогноз готовності</div>
              <div id="forecast" class="muted tiny">—</div>
            </div>
          </div>

          <div style="flex:1">
            <div style="display:flex;gap:10px;align-items:center;justify-content:space-between">
              <div style="font-weight:700">Статистика по категоріях</div>
            </div>
            <table id="categoryTable" style="margin-top:8px">
              <thead><tr><th>Категорія</th><th>Питань</th><th>% правильно</th></tr></thead>
              <tbody></tbody>
            </table>
          </div>

          <div style="flex:0 0 320px">
            <div style="font-weight:700">Теплова карта (слабкі місця)</div>
            <div class="heatmap" id="heatmapArea" style="margin-top:8px"></div>
          </div>
        </div>

        <div style="margin-top:16px">
          <div style="font-weight:700;margin-bottom:6px">Графік історії результатів</div>
          <canvas id="historyChart" width="800" height="160"></canvas>
        </div>

        <div style="margin-top:12px;display:flex;gap:8px">
          <button id="retakeBtn">Пройти ще раз</button>
          <button id="toSetup" class="ghost">Повернутись до налаштувань</button>
          <div style="flex:1"></div>
          <button id="exportStats" class="ghost">Експорт статистики (JSON)</button>
        </div>

        <div style="margin-top:10px">
          <div style="font-weight:700;margin-bottom:6px">Питання, в яких ви найбільше помиляєтесь</div>
          <table id="badQuestionsTable">
            <thead><tr><th>#</th><th>Питання</th><th>Помилок</th></tr></thead>
            <tbody></tbody>
          </table>
        </div>

      </div>

    </div>
  </div>

  <footer>
    <div>Версія: локальна • Без зовнішніх бібліотек</div>
    <div class="muted tiny">Історія збережена в пам'яті браузера: <span id="lsCount">0</span> записів</div>
  </footer>
</div>

<script>
/* =================================================================================
   ЛОГІКА ПРАКТИЧНОГО ТЕСТУ (оновлена)
   - Реалізовано умовне відображення поля для завантаження файлу
   - Якщо JSON вбудовано в цей блок, поле для файлу приховується.
   ================================================================================= */

// --- ВБУДОВАНИЙ JSON (PLACEHOLDER) ---
const RAW_JSON_TEXT = `{
  "testInfo": {
    "title": "Тест для підготовки на курси лідерства середнього рівня",
    "totalQuestions": 100,
    "categories": [
      "Військові статути та стандарти",
      "Військова етика та лідерство",
      "Тактика та бойові дії",
      "Домедична допомога",
      "Топографія та орієнтування",
      "Навчальна методика",
      "Вогнева підготовка",
      "Процес управління підрозділом",
      "Мінно-вибухова справа",
      "Міжнародне право та НАТО"
    ]
  },
  "questions": [
    {
      "id": 1,
      "category": "Військові статути та стандарти",
      "difficulty": 1,
      "question": "Професійний стандарт це?",
      "answers": [
        {
          "text": "Затверджені в установленому порядку вимоги до кваліфікації і компетентності, що визначаються і слугують основою для формування професійних кваліфікацій",
          "isCorrect": true
        },
        {
          "text": "Документ, що регламентує порядок проходження військової служби та визначає права і обов'язки військовослужбовців",
          "isCorrect": false
        },
        {
          "text": "Сукупність нормативних актів, що встановлюють правила поведінки військовослужбовців у повсякденній діяльності",
          "isCorrect": false
        },
        {
          "text": "Система оцінювання професійних якостей військовослужбовців під час атестації",
          "isCorrect": false
        }
      ],
      "comment": "Професійний стандарт - це офіційно затверджений документ, який визначає вимоги до знань, умінь та компетентностей для конкретної посади. Він є основою для розробки програм підготовки та оцінювання кваліфікації."
    },
    {
      "id": 2,
      "category": "Військові статути та стандарти",
      "difficulty": 1,
      "question": "Професіоналізм це?",
      "answers": [
        {
          "text": "Сукупність досягнутих теоретичних знань, практичного досвіду і професійних навиків у визначеній сфері діяльності",
          "isCorrect": true
        },
        {
          "text": "Здатність виконувати бойові завдання в умовах підвищеної небезпеки",
          "isCorrect": false
        },
        {
          "text": "Рівень фізичної підготовки та володіння зброєю військовослужбовця",
          "isCorrect": false
        },
        {
          "text": "Наявність військової освіти та досвіду участі в бойових діях",
          "isCorrect": false
        }
      ],
      "comment": "Професіоналізм включає три ключові компоненти: теоретичні знання (те, що ви вивчили), практичний досвід (те, що ви робили) та професійні навички (те, що ви вмієте робити добре)."
    },
    {
      "id": 3,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Основними способами ведення загальновійськового бою є?",
      "answers": [
        {
          "text": "Послідовний розгром і одночасне ураження",
          "isCorrect": true
        },
        {
          "text": "Наступ і оборона",
          "isCorrect": false
        },
        {
          "text": "Маневрена оборона і контрнаступ",
          "isCorrect": false
        },
        {
          "text": "Охоплення і обхід противника",
          "isCorrect": false
        }
      ],
      "comment": "Послідовний розгром - це знищення противника по частинах, одночасне ураження - це атака всіх цілей одночасно. Це два основні способи ведення сучасного загальновійськового бою."
    },
    {
      "id": 4,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Етос це?",
      "answers": [
        {
          "text": "Узагальнена характеристика культури певної соціальної спільноти, яка виражена в системі її цінностей і норм поведінки",
          "isCorrect": true
        },
        {
          "text": "Кодекс честі військовослужбовця, що визначає правила поведінки в бою",
          "isCorrect": false
        },
        {
          "text": "Система морально-етичних принципів ведення війни згідно міжнародного права",
          "isCorrect": false
        },
        {
          "text": "Сукупність традицій та звичаїв військового підрозділу",
          "isCorrect": false
        }
      ],
      "comment": "Етос - це система цінностей та норм поведінки певної групи людей. У військовому контексті - це неписані правила та традиції, які формують культуру підрозділу."
    },
    {
      "id": 5,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Назвіть гасло сержанта Збройних Сил України?",
      "answers": [
        {
          "text": "Слідуй за мною, роби як я",
          "isCorrect": true
        },
        {
          "text": "Честь і звитяга понад усе",
          "isCorrect": false
        },
        {
          "text": "Завжди готовий, завжди перший",
          "isCorrect": false
        },
        {
          "text": "Служу народу України",
          "isCorrect": false
        }
      ],
      "comment": "Гасло 'Слідуй за мною, роби як я' підкреслює роль сержанта як лідера та наставника, який веде особистим прикладом, а не просто віддає накази."
    },
    {
      "id": 6,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Назвіть цінності сержанта Збройних Сил України?",
      "answers": [
        {
          "text": "Честь, відданість, відповідальність, професіоналізм",
          "isCorrect": true
        },
        {
          "text": "Мужність, патріотизм, дисципліна, взаємодопомога",
          "isCorrect": false
        },
        {
          "text": "Відвага, вірність, справедливість, компетентність",
          "isCorrect": false
        },
        {
          "text": "Хоробрість, служіння, досконалість, єдність",
          "isCorrect": false
        }
      ],
      "comment": "Цінності сержанта - Честь (моральна чистота), Відданість (вірність присязі), Відповідальність (за підлеглих), Професіоналізм (майстерність у службі)."
    },
    {
      "id": 7,
      "category": "Військові статути та стандарти",
      "difficulty": 2,
      "question": "Командир відділення в мирний і воєнний час відповідає за?",
      "answers": [
        {
          "text": "Успішне виконання відділенням завдань за призначенням, навчання і виховання військовослужбовців відділення, їх військову дисципліну, морально-психологічний стан, охайний зовнішній вигляд, стройову і фізичну підготовку, правильне зберігання, експлуатацію і утримання в належному стані озброєння, техніки та іншого майна відділення",
          "isCorrect": true
        },
        {
          "text": "Тільки за виконання бойових завдань, підтримання дисципліни та збереження озброєння відділення",
          "isCorrect": false
        },
        {
          "text": "Виключно за бойову готовність підрозділу, фізичну підготовку особового складу та експлуатацію техніки",
          "isCorrect": false
        },
        {
          "text": "Навчання особового складу, ведення обліку майна та підтримання статутного порядку у відділенні",
          "isCorrect": false
        }
      ],
      "comment": "Командир відділення несе повну відповідальність за всі аспекти діяльності свого підрозділу - від бойових завдань до побуту, від навчання до збереження майна."
    },
    {
      "id": 8,
      "category": "Військові статути та стандарти",
      "difficulty": 2,
      "question": "Які види юридичної відповідальності може понести військовослужбовець Збройних Сил України?",
      "answers": [
        {
          "text": "Дисциплінарна, адміністративна, матеріальна, цивільно-правова та кримінальна",
          "isCorrect": true
        },
        {
          "text": "Дисциплінарна, кримінальна, адміністративна та службова",
          "isCorrect": false
        },
        {
          "text": "Матеріальна, моральна, кримінальна та дисциплінарна",
          "isCorrect": false
        },
        {
          "text": "Адміністративна, кримінальна, фінансова та особиста",
          "isCorrect": false
        }
      ],
      "comment": "П'ять видів відповідальності військовослужбовця: дисциплінарна (за порушення дисципліни), адміністративна (за адмінправопорушення), матеріальна (за шкоду майну), цивільно-правова (за цивільні спори), кримінальна (за злочини)."
    },
    {
      "id": 9,
      "category": "Військові статути та стандарти",
      "difficulty": 2,
      "question": "Скільки варіантів (абзаців) випадків прописано в Статуті внутрішньої служби ЗСУ, коли військовослужбовець має право застосовувати спеціальні засоби, засоби фізичного впливу та зброю особисто або у складі підрозділу?",
      "answers": [
        {
          "text": "5",
          "isCorrect": true
        },
        {
          "text": "3",
          "isCorrect": false
        },
        {
          "text": "7",
          "isCorrect": false
        },
        {
          "text": "4",
          "isCorrect": false
        }
      ],
      "comment": "У Статуті внутрішньої служби прописано саме 5 абзаців випадків застосування зброї та спецзасобів. Це важливо знати для правомірного застосування сили."
    },
    {
      "id": 10,
      "category": "Військові статути та стандарти",
      "difficulty": 2,
      "question": "Опишіть поняття командної ланки на рівні відділення-взводу?",
      "answers": [
        {
          "text": "Кандидат на навчання обіймає посаду командира відділення, безпосередній його командир - головний сержант взводу, а прямий командир - командир взводу",
          "isCorrect": true
        },
        {
          "text": "Командир відділення підпорядковується заступнику командира взводу, який підпорядковується командиру взводу",
          "isCorrect": false
        },
        {
          "text": "Командир відділення напряму підпорядковується командиру взводу без проміжних ланок",
          "isCorrect": false
        },
        {
          "text": "Командна ланка складається з командира відділення, старшого солдата та командира взводу",
          "isCorrect": false
        }
      ],
      "comment": "Командна ланка: командир відділення → головний сержант взводу (безпосередній начальник) → командир взводу (прямий начальник). Важливо розуміти різницю між безпосереднім та прямим командиром."
    },
    {
      "id": 11,
      "category": "Військові статути та стандарти",
      "difficulty": 1,
      "question": "Що таке військова дисципліна?",
      "answers": [
        {
          "text": "Бездоганне і неухильне додержання всіма військовослужбовцями порядку і правил, встановлених військовими статутами та іншим законодавством України",
          "isCorrect": true
        },
        {
          "text": "Система покарань і заохочень для підтримання порядку у військових підрозділах",
          "isCorrect": false
        },
        {
          "text": "Сукупність моральних якостей військовослужбовця, необхідних для виконання бойових завдань",
          "isCorrect": false
        },
        {
          "text": "Правила поведінки військовослужбовців під час несення служби",
          "isCorrect": false
        }
      ],
      "comment": "Військова дисципліна - це не просто виконання наказів, а свідоме дотримання всіх правил та законів. Вона базується на розумінні, а не на страху."
    },
    {
      "id": 12,
      "category": "Військові статути та стандарти",
      "difficulty": 2,
      "question": "Які дисциплінарні стягнення можуть бути застосовані для військовослужбовця?",
      "answers": [
        {
          "text": "Зауваження, догана, сувора догана, попередження про неповну службову відповідність, пониження в посаді, пониження у військовому звані, звільнення з військової служби",
          "isCorrect": true
        },
        {
          "text": "Зауваження, догана, позбавлення звільнення, арешт, звільнення з військової служби",
          "isCorrect": false
        },
        {
          "text": "Попередження, догана, штраф, пониження у званні, звільнення",
          "isCorrect": false
        },
        {
          "text": "Зауваження, сувора догана, позачергові наряди, гауптвахта, звільнення",
          "isCorrect": false
        }
      ],
      "comment": "Сім видів дисциплінарних стягнень від найлегшого до найсуворішого: зауваження, догана, сувора догана, попередження про неповну службову відповідність, пониження в посаді, пониження у званні, звільнення."
    },
    {
      "id": 13,
      "category": "Військові статути та стандарти",
      "difficulty": 2,
      "question": "Які заохочення може застосовувати командир відділення?",
      "answers": [
        {
          "text": "Схвалення, подяка",
          "isCorrect": true
        },
        {
          "text": "Схвалення, подяка, грошова премія",
          "isCorrect": false
        },
        {
          "text": "Подяка, додаткове звільнення, цінний подарунок",
          "isCorrect": false
        },
        {
          "text": "Схвалення, подяка, представлення до нагороди",
          "isCorrect": false
        }
      ],
      "comment": "Командир відділення має право застосовувати лише два заохочення: схвалення (усна похвала) та подяка (оголошується перед строєм). Інші заохочення - компетенція вищих командирів."
    },
    {
      "id": 14,
      "category": "Військові статути та стандарти",
      "difficulty": 2,
      "question": "Які дисциплінарні стягнення може застосовувати командир відділення?",
      "answers": [
        {
          "text": "Зауваження, догана, сувора догана, позбавлення чергового звільнення (стосовно військовослужбовців строкової військової служби та курсантів)",
          "isCorrect": true
        },
        {
          "text": "Тільки зауваження та догана",
          "isCorrect": false
        },
        {
          "text": "Зауваження, догана, позачергові наряди",
          "isCorrect": false
        },
        {
          "text": "Попередження, зауваження, догана, арешт до 3 діб",
          "isCorrect": false
        }
      ],
      "comment": "Командир відділення може накладати: зауваження, догану, сувору догану та позбавлення звільнення (тільки для строковиків та курсантів). Арешт та інші стягнення - це повноваження вищих командирів."
    },
    {
      "id": 15,
      "category": "Військові статути та стандарти",
      "difficulty": 1,
      "question": "Військова дисципліна ґрунтується на?",
      "answers": [
        {
          "text": "Усвідомленні військовослужбовцями свого військового обов'язку, відповідальності за захист Вітчизни, незалежності та територіальної цілісності України, на їх вірності Військовій присязі",
          "isCorrect": true
        },
        {
          "text": "Страху перед покаранням та системі суворих дисциплінарних стягнень",
          "isCorrect": false
        },
        {
          "text": "Безумовному виконанні наказів командирів та начальників",
          "isCorrect": false
        },
        {
          "text": "Традиціях військової служби та повазі до старших за званням",
          "isCorrect": false
        }
      ],
      "comment": "Військова дисципліна ґрунтується на усвідомленні обов'язку та патріотизмі, а не на страху покарання. Свідома дисципліна завжди ефективніша за примусову."
    },
    {
      "id": 16,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Назвіть основні риси сучасного бою?",
      "answers": [
        {
          "text": "Рішучість, маневреність, напруженість, короткочасність, швидкі зміни обстановки",
          "isCorrect": true
        },
        {
          "text": "Масованість, тривалість, позиційність, висока інтенсивність вогню",
          "isCorrect": false
        },
        {
          "text": "Раптовість, швидкоплинність, масштабність, високі втрати",
          "isCorrect": false
        },
        {
          "text": "Технологічність, дистанційність, безконтактність, високоточність",
          "isCorrect": false
        }
      ],
      "comment": "П'ять основних рис сучасного бою: рішучість (швидкі дії), маневреність (постійний рух), напруженість (висока інтенсивність), короткочасність (швидкі зіткнення), швидкі зміни обстановки."
    },
    {
      "id": 17,
      "category": "Міжнародне право та НАТО",
      "difficulty": 1,
      "question": "Комбатант це?",
      "answers": [
        {
          "text": "Особа, яка входить до складу Збройних Сил країн, які перебувають у стані військового конфлікту і має право безпосередньо брати участь у військових діях",
          "isCorrect": true
        },
        {
          "text": "Цивільна особа, яка бере участь у збройному конфлікті на стороні однієї з воюючих сторін",
          "isCorrect": false
        },
        {
          "text": "Військовослужбовець, який виконує виключно допоміжні функції без права участі в бойових діях",
          "isCorrect": false
        },
        {
          "text": "Представник миротворчих сил, який не має права застосовувати зброю",
          "isCorrect": false
        }
      ],
      "comment": "Комбатант - це законний учасник бойових дій, який має право вбивати ворога і на якого поширюється захист міжнародного гуманітарного права при полоні."
    },
    {
      "id": 18,
      "category": "Міжнародне право та НАТО",
      "difficulty": 2,
      "question": "Хто належить до некомбатантів (не менше трьох прикладів)?",
      "answers": [
        {
          "text": "Медичний і духовний персонал, інтенданти, військові кореспонденти, юристи",
          "isCorrect": true
        },
        {
          "text": "Розвідники, снайпери, водії бойових машин, зв'язківці",
          "isCorrect": false
        },
        {
          "text": "Кухарі, водії, механіки, охоронці",
          "isCorrect": false
        },
        {
          "text": "Санітари, перекладачі, інженери, картографи",
          "isCorrect": false
        }
      ],
      "comment": "Некомбатанти - це військовослужбовці, які не беруть безпосередньої участі в бойових діях. Вони захищені міжнародним правом, але втрачають захист, якщо беруть участь у бою."
    },
    {
      "id": 19,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Культура це?",
      "answers": [
        {
          "text": "Те, що формує народ як націю, це звичаї і традиції, історія та духовність, це надбання держави",
          "isCorrect": true
        },
        {
          "text": "Система правил поведінки та етикету в суспільстві",
          "isCorrect": false
        },
        {
          "text": "Рівень освіченості та вихованості окремої людини чи групи",
          "isCorrect": false
        },
        {
          "text": "Сукупність мистецьких творів та літературних надбань народу",
          "isCorrect": false
        }
      ],
      "comment": "Культура формує націю через звичаї, традиції, історію та духовність. Це те, що відрізняє один народ від іншого та створює національну ідентичність."
    },
    {
      "id": 20,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Культурна обізнаність це?",
      "answers": [
        {
          "text": "Розуміння і повага до того, як ідеї і сенси у різних культурах творчо виражаються і передаються через різні галузі мистецтва і форми культури",
          "isCorrect": true
        },
        {
          "text": "Знання історії та традицій свого народу",
          "isCorrect": false
        },
        {
          "text": "Вміння спілкуватися іноземними мовами та розуміти інші культури",
          "isCorrect": false
        },
        {
          "text": "Дотримання норм етикету та правил поведінки в різних країнах",
          "isCorrect": false
        }
      ],
      "comment": "Культурна обізнаність важлива для розуміння місцевого населення в зоні операцій та уникнення конфліктів через культурні непорозуміння."
    },
    {
      "id": 21,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Назвіть стилі керівництва?",
      "answers": [
        {
          "text": "Автократичний, демократичний, ліберальний",
          "isCorrect": true
        },
        {
          "text": "Авторитарний, колегіальний, індивідуальний",
          "isCorrect": false
        },
        {
          "text": "Директивний, консультативний, делегуючий",
          "isCorrect": false
        },
        {
          "text": "Жорсткий, м'який, змішаний",
          "isCorrect": false
        }
      ],
      "comment": "Три стилі керівництва: автократичний (одноосібне рішення), демократичний (колективне обговорення), ліберальний (мінімальне втручання). Кожен ефективний у певних ситуаціях."
    },
    {
      "id": 22,
      "category": "Військова етика та лідерство",
      "difficulty": 2,
      "question": "Лідерство це?",
      "answers": [
        {
          "text": "Тип управлінської взаємодії, який ґрунтується на більш ефективному для даної ситуації поєднанні різних джерел влади і спрямований на спонукання людей до досягнення загальних цілей",
          "isCorrect": true
        },
        {
          "text": "Здатність однієї людини підпорядковувати собі інших за допомогою авторитету та харизми",
          "isCorrect": false
        },
        {
          "text": "Формальна позиція в організаційній структурі, що дає право віддавати накази",
          "isCorrect": false
        },
        {
          "text": "Вроджена якість особистості, що дозволяє керувати людьми",
          "isCorrect": false
        }
      ],
      "comment": "Лідерство - це вміння впливати на людей для досягнення спільної мети, використовуючи різні джерела влади: авторитет, знання, харизму, посаду."
    },
    {
      "id": 23,
      "category": "Військова етика та лідерство",
      "difficulty": 2,
      "question": "Які обставини задовольняє мотивація?",
      "answers": [
        {
          "text": "Забезпечення індивідуальних потреб і досягнення організаційних цілей",
          "isCorrect": true
        },
        {
          "text": "Підвищення продуктивності праці та зменшення плинності кадрів",
          "isCorrect": false
        },
        {
          "text": "Створення позитивного клімату в колективі та підвищення лояльності",
          "isCorrect": false
        },
        {
          "text": "Розвиток професійних навичок та кар'єрне зростання",
          "isCorrect": false
        }
      ],
      "comment": "Мотивація має задовольняти як особисті потреби людини (зарплата, визнання), так і організаційні цілі (виконання завдань). Баланс між ними - ключ до успіху."
    },
    {
      "id": 24,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Які види мотивації є?",
      "answers": [
        {
          "text": "Внутрішня і зовнішня",
          "isCorrect": true
        },
        {
          "text": "Матеріальна і нематеріальна",
          "isCorrect": false
        },
        {
          "text": "Позитивна і негативна",
          "isCorrect": false
        },
        {
          "text": "Індивідуальна і колективна",
          "isCorrect": false
        }
      ],
      "comment": "Внутрішня мотивація йде зсередини (бажання служити), зовнішня - ззовні (нагороди, покарання). Внутрішня мотивація завжди сильніша та довготриваліша."
    },
    {
      "id": 25,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Що вивчає етика?",
      "answers": [
        {
          "text": "Мораль",
          "isCorrect": true
        },
        {
          "text": "Закони",
          "isCorrect": false
        },
        {
          "text": "Поведінку",
          "isCorrect": false
        },
        {
          "text": "Традиції",
          "isCorrect": false
        }
      ],
      "comment": "Етика вивчає мораль - систему норм про добро і зло, правильне і неправильне. Це основа для прийняття рішень у складних ситуаціях."
    },
    {
      "id": 26,
      "category": "Військова етика та лідерство",
      "difficulty": 2,
      "question": "Основними принципами військової етики є (вкажіть не менше трьох)?",
      "answers": [
        {
          "text": "Пріоритет службових інтересів, політична нейтральність, неупередженість, компетентність і ефективність, прозорість, нерозголошення, корпоративність",
          "isCorrect": true
        },
        {
          "text": "Патріотизм, вірність присязі, товариськість, дисциплінованість, сміливість",
          "isCorrect": false
        },
        {
          "text": "Чесність, порядність, відповідальність, професіоналізм, лояльність",
          "isCorrect": false
        },
        {
          "text": "Справедливість, гуманність, законність, об'єктивність, принциповість",
          "isCorrect": false
        }
      ],
      "comment": "Сім принципів військової етики формують кодекс поведінки військовослужбовця: від пріоритету служби над особистим до збереження військових таємниць."
    },
    {
      "id": 27,
      "category": "Військова етика та лідерство",
      "difficulty": 2,
      "question": "Що таке етична дилема?",
      "answers": [
        {
          "text": "Ситуація у якій людина, здійснюючи той або інший вчинок, приймаючи певне рішення, вимушена визначати для себе пріоритети: нормативи професійної етики або власні моральні переконання, суспільний запит, вимоги керівництва",
          "isCorrect": true
        },
        {
          "text": "Конфлікт між особистими інтересами та службовими обов'язками",
          "isCorrect": false
        },
        {
          "text": "Порушення етичних норм поведінки військовослужбовця",
          "isCorrect": false
        },
        {
          "text": "Ситуація вибору між двома негативними наслідками",
          "isCorrect": false
        }
      ],
      "comment": "Етична дилема виникає, коли треба вибирати між кількома правильними, але несумісними варіантами. Наприклад, між наказом командира та власною совістю."
    },
    {
      "id": 28,
      "category": "Військова етика та лідерство",
      "difficulty": 2,
      "question": "Опишіть поняття дискримінація?",
      "answers": [
        {
          "text": "Будь-яка відмінність, виключення, обмеження або перевага, що забезпечує або зменшує рівне здійснення прав",
          "isCorrect": true
        },
        {
          "text": "Упереджене ставлення до особи на основі її національності чи релігії",
          "isCorrect": false
        },
        {
          "text": "Порушення конституційних прав і свобод громадянина",
          "isCorrect": false
        },
        {
          "text": "Незаконне обмеження в правах окремих категорій населення",
          "isCorrect": false
        }
      ],
      "comment": "Дискримінація - це будь-яке несправедливе обмеження прав людини за певною ознакою. У війську неприпустима, бо руйнує єдність підрозділу."
    },
    {
      "id": 29,
      "category": "Військова етика та лідерство",
      "difficulty": 1,
      "question": "Які бувають військові традиції?",
      "answers": [
        {
          "text": "Бойові традиції, традиції військового навчання та виховання, традиції військового побуту",
          "isCorrect": true
        },
        {
          "text": "Історичні, сучасні, перспективні",
          "isCorrect": false
        },
        {
          "text": "Загальновійськові, видові, підрозділу",
          "isCorrect": false
        },
        {
          "text": "Офіційні, неофіційні, святкові",
          "isCorrect": false
        }
      ],
      "comment": "Три види військових традицій: бойові (як воювати), навчальні (як готуватися), побутові (як жити). Традиції згуртовують колектив та передають досвід."
    },
    {
      "id": 30,
      "category": "Військова етика та лідерство",
      "difficulty": 2,
      "question": "Що описує концепція командної групи?",
      "answers": [
        {
          "text": "Ланку управління/службові відносини на рівні відповідного командира та його головного сержанта/старшини",
          "isCorrect": true
        },
        {
          "text": "Групу військовослужбовців, об'єднаних для виконання спільного завдання",
          "isCorrect": false
        },
        {
          "text": "Систему взаємовідносин між офіцерським та сержантським складом",
          "isCorrect": false
        },
        {
          "text": "Методику формування злагодженого військового колективу",
          "isCorrect": false
        }
      ],
      "comment": "Концепція командної групи описує тандем командир-головний сержант, де командир приймає рішення, а сержант забезпечує їх виконання. Це основа ефективного управління."
    },
    {
      "id": 31,
      "category": "Тактика та бойові дії",
      "difficulty": 3,
      "question": "Який воєнний конфлікт дав поштовх до зародження і розвитку оперативного мистецтва?",
      "answers": [
        {
          "text": "Російсько-Японська війна",
          "isCorrect": true
        },
        {
          "text": "Перша світова війна",
          "isCorrect": false
        },
        {
          "text": "Франко-Прусська війна",
          "isCorrect": false
        },
        {
          "text": "Кримська війна",
          "isCorrect": false
        }
      ],
      "comment": "Російсько-Японська війна 1904-1905 років вперше показала необхідність координації між тактичним та стратегічним рівнями, що привело до появи оперативного мистецтва."
    },
    {
      "id": 32,
      "category": "Міжнародне право та НАТО",
      "difficulty": 2,
      "question": "Яка стаття НАТО регламентує колективну безпеку?",
      "answers": [
        {
          "text": "5 стаття",
          "isCorrect": true
        },
        {
          "text": "2 стаття",
          "isCorrect": false
        },
        {
          "text": "3 стаття",
          "isCorrect": false
        },
        {
          "text": "7 стаття",
          "isCorrect": false
        }
      ],
      "comment": "Стаття 5 НАТО говорить: напад на одного - напад на всіх. Це основа колективної оборони Альянсу. Важливо: стаття 2 стосується мирного співробітництва."
    },
    {
      "id": 33,
      "category": "Міжнародне право та НАТО",
      "difficulty": 2,
      "question": "На основі якого принципу застосовується прийняття рішень у комітетах НАТО після обговорювання і консультацій?",
      "answers": [
        {
          "text": "Консенсусу",
          "isCorrect": true
        },
        {
          "text": "Простої більшості голосів",
          "isCorrect": false
        },
        {
          "text": "Кваліфікованої більшості",
          "isCorrect": false
        },
        {
          "text": "Одноголосності",
          "isCorrect": false
        }
      ],
      "comment": "Консенсус в НАТО означає, що рішення приймається тільки коли всі погоджуються. Немає голосування - є обговорення до досягнення спільної позиції."
    },
    {
      "id": 34,
      "category": "Міжнародне право та НАТО",
      "difficulty": 2,
      "question": "В рамках якої програми НАТО реалізується професійний розвиток сержантського складу ЗС України?",
      "answers": [
        {
          "text": "DEEP",
          "isCorrect": true
        },
        {
          "text": "Partnership for Peace",
          "isCorrect": false
        },
        {
          "text": "NATO School",
          "isCorrect": false
        },
        {
          "text": "STANAG",
          "isCorrect": false
        }
      ],
      "comment": "DEEP (Defence Education Enhancement Programme) - програма НАТО для розвитку військової освіти в країнах-партнерах, включаючи підготовку сержантського складу України."
    },
    {
      "id": 35,
      "category": "Навчальна методика",
      "difficulty": 1,
      "question": "Із яких частин складається навчальне заняття?",
      "answers": [
        {
          "text": "Вступна, основна, заключна",
          "isCorrect": true
        },
        {
          "text": "Підготовча, навчальна, контрольна",
          "isCorrect": false
        },
        {
          "text": "Організаційна, практична, підсумкова",
          "isCorrect": false
        },
        {
          "text": "Теоретична, практична, оціночна",
          "isCorrect": false
        }
      ],
      "comment": "Три частини заняття: вступна (організація, мотивація, безпека), основна (навчання нового матеріалу), заключна (закріплення, підсумки, завдання)."
    },
    {
      "id": 36,
      "category": "Навчальна методика",
      "difficulty": 1,
      "question": "Коли доводять заходи безпеки під час навчальних занять?",
      "answers": [
        {
          "text": "У вступній частині",
          "isCorrect": true
        },
        {
          "text": "Перед початком практичних вправ",
          "isCorrect": false
        },
        {
          "text": "В заключній частині",
          "isCorrect": false
        },
        {
          "text": "Протягом усього заняття",
          "isCorrect": false
        }
      ],
      "comment": "Заходи безпеки доводять у вступній частині, щоб всі були готові до безпечного виконання вправ. Це обов'язковий елемент кожного заняття."
    },
    {
      "id": 37,
      "category": "Навчальна методика",
      "difficulty": 2,
      "question": "Що таке EDIP?",
      "answers": [
        {
          "text": "Сукупність дій інструктора та курсантів для набуття знань, вмінь та навичок",
          "isCorrect": true
        },
        {
          "text": "Методика оцінювання результатів навчання",
          "isCorrect": false
        },
        {
          "text": "Система планування навчального процесу",
          "isCorrect": false
        },
        {
          "text": "Стандарт проведення військових навчань",
          "isCorrect": false
        }
      ],
      "comment": "EDIP - це методика навчання через чотири етапи, яка забезпечує поступове засвоєння навичок від теорії до самостійної практики."
    },
    {
      "id": 38,
      "category": "Навчальна методика",
      "difficulty": 2,
      "question": "Які складові частини EDIP?",
      "answers": [
        {
          "text": "Пояснення, демонстрація, імітація, практика",
          "isCorrect": true
        },
        {
          "text": "Підготовка, виконання, контроль, оцінка",
          "isCorrect": false
        },
        {
          "text": "Теорія, показ, тренування, перевірка",
          "isCorrect": false
        },
        {
          "text": "Вступ, розвиток, закріплення, підсумок",
          "isCorrect": false
        }
      ],
      "comment": "EDIP: Explain (пояснити теорію), Demonstrate (показати виконання), Imitate (курсанти повторюють), Practice (самостійна практика). Кожен етап важливий для засвоєння."
    },
    {
      "id": 39,
      "category": "Навчальна методика",
      "difficulty": 2,
      "question": "Які методи навчання використовуються у Збройних Силах України (назвіть не менше трьох)?",
      "answers": [
        {
          "text": "Лекція, бесіда, показ, тренування, контрольне заняття, показове заняття, інструкторсько-методичне заняття, класно-групове заняття",
          "isCorrect": true
        },
        {
          "text": "Розповідь, демонстрація, вправи, самостійна робота, тестування",
          "isCorrect": false
        },
        {
          "text": "Пояснення, інструктаж, практика, семінар, консультація",
          "isCorrect": false
        },
        {
          "text": "Навчання, тренінг, майстер-клас, дискусія, моделювання",
          "isCorrect": false
        }
      ],
      "comment": "У ЗСУ використовується багато методів навчання, кожен для своєї мети: лекція - для теорії, показ - для демонстрації, тренування - для відпрацювання навичок."
    },
    {
      "id": 40,
      "category": "Навчальна методика",
      "difficulty": 2,
      "question": "Які риси ефективного інструктора Ви знаєте (назвіть не менше трьох)?",
      "answers": [
        {
          "text": "Впевненість, вихованість, ставлення, старанність, ентузіазм, ініціатива",
          "isCorrect": true
        },
        {
          "text": "Професіоналізм, досвід, авторитет, вимогливість, справедливість",
          "isCorrect": false
        },
        {
          "text": "Знання, вміння, навички, терпіння, комунікабельність",
          "isCorrect": false
        },
        {
          "text": "Компетентність, відповідальність, дисципліна, організованість, лідерство",
          "isCorrect": false
        }
      ],
      "comment": "Ефективний інструктор має шість ключових рис, які формують абревіатуру ВВССЕІ: Впевненість, Вихованість, Ставлення, Старанність, Ентузіазм, Ініціатива."
    },
    {
      "id": 41,
      "category": "Вогнева підготовка",
      "difficulty": 1,
      "question": "Назвіть п'ять правил безпечного поводження зі зброєю?",
      "answers": [
        {
          "text": "1. Поводження зі зброєю, як із зарядженою; 2. Не направляти зброю туди, куди не збираюсь стріляти; 3. Не класти палець на спусковий гачок, якщо зброя не направлена на ціль; 4. Перед стрільбою впевнитися у відсутності когось або чогось перед мішенню; 5. Не залишати свою зброю без нагляду",
          "isCorrect": true
        },
        {
          "text": "1. Перевіряти зброю на розрядженість; 2. Тримати зброю на запобіжнику; 3. Не передавати зброю стороннім; 4. Зберігати зброю в сейфі; 5. Регулярно чистити зброю",
          "isCorrect": false
        },
        {
          "text": "1. Носити зброю розрядженою; 2. Не грати зі зброєю; 3. Знати пристрій своєї зброї; 4. Дотримуватись правил стрільби; 5. Доповідати про несправності",
          "isCorrect": false
        },
        {
          "text": "1. Не направляти на людей; 2. Стріляти тільки по команді; 3. Використовувати захисні засоби; 4. Знати характеристики зброї; 5. Виконувати накази командира",
          "isCorrect": false
        }
      ],
      "comment": "П'ять правил безпеки зі зброєю - це універсальні принципи, які запобігають нещасним випадкам. Запам'ятайте: зброя завжди заряджена, поки не доведено протилежне!"
    },
    {
      "id": 42,
      "category": "Вогнева підготовка",
      "difficulty": 2,
      "question": "Коли проводиться перевірка зброї на розрядженість?",
      "answers": [
        {
          "text": "Перед заняттям і після нього, перед розбиранням, знайшовши зброю, отримуючи і здаючи зброю, перед входом в будівлю, коли приймаєте (передаєте) зброю, коли не впевнені в безпеці зброї",
          "isCorrect": true
        },
        {
          "text": "Тільки при отриманні та здачі зброї, а також перед чищенням",
          "isCorrect": false
        },
        {
          "text": "Перед стрільбою, після стрільби та при зміні варти",
          "isCorrect": false
        },
        {
          "text": "Щоранку під час огляду зброї та після кожного застосування",
          "isCorrect": false
        }
      ],
      "comment": "Перевірка на розрядженість проводиться у восьми випадках - це критично важливо для безпеки. Правило: краще перевірити зайвий раз, ніж отримати нещасний випадок."
    },
    {
      "id": 43,
      "category": "Навчальна методика",
      "difficulty": 2,
      "question": "Які є процеси комунікації?",
      "answers": [
        {
          "text": "Підготовка виступу, організація виступу, підготовка допоміжних матеріалів, визначення початку і закінчення виступу, виступ",
          "isCorrect": true
        },
        {
          "text": "Планування, кодування, передача, декодування, зворотній зв'язок",
          "isCorrect": false
        },
        {
          "text": "Аналіз аудиторії, формулювання мети, вибір каналу, передача повідомлення, отримання відгуку",
          "isCorrect": false
        },
        {
          "text": "Збір інформації, структурування, презентація, обговорення, підсумки",
          "isCorrect": false
        }
      ],
      "comment": "П'ять процесів комунікації забезпечують ефективний виступ: від підготовки змісту до правильного завершення. Кожен етап впливає на успіх презентації."
    },
    {
      "id": 44,
      "category": "Навчальна методика",
      "difficulty": 1,
      "question": "Які групи виступів існують?",
      "answers": [
        {
          "text": "Брифінг, навчальна лекція, публічний виступ",
          "isCorrect": true
        },
        {
          "text": "Інформаційний, переконуючий, мотиваційний",
          "isCorrect": false
        },
        {
          "text": "Офіційний, неофіційний, урочистий",
          "isCorrect": false
        },
        {
          "text": "Короткий, середній, розширений",
          "isCorrect": false
        }
      ],
      "comment": "Три групи виступів: брифінг (коротка інформація), навчальна лекція (передача знань), публічний виступ (переконання аудиторії). Кожен має свої особливості."
    },
    {
      "id": 45,
      "category": "Військова етика та лідерство",
      "difficulty": 2,
      "question": "Які є джерела та ознаки конфлікту?",
      "answers": [
        {
          "text": "Расизм, сексизм, упередженість, дискримінація, подвійні стандарти",
          "isCorrect": true
        },
        {
          "text": "Непорозуміння, конкуренція, різниця в цінностях, обмежені ресурси, особисті амбіції",
          "isCorrect": false
        },
        {
          "text": "Агресія, напруження, суперечки, незгода, протистояння",
          "isCorrect": false
        },
        {
          "text": "Культурні відмінності, мовний бар'єр, різний досвід, вікова різниця, освітній рівень",
          "isCorrect": false
        }
      ],
      "comment": "П'ять джерел конфлікту пов'язані з упередженнями та дискримінацією. Знання цих джерел допомагає запобігати конфліктам у багатонаціональному колективі."
    },
    {
      "id": 46,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "З яких зон повинен складатися Блокпост?",
      "answers": [
        {
          "text": "Обмежена зона руху, охоронна зона і зона перешкод, зона огляду, безпечна і адміністративна зона",
          "isCorrect": true
        },
        {
          "text": "Зона спостереження, зона затримання, зона огляду, зона евакуації",
          "isCorrect": false
        },
        {
          "text": "Передня зона, центральна зона, тилова зона, резервна зона",
          "isCorrect": false
        },
        {
          "text": "Зона попередження, зона блокування, зона контролю, зона відпочинку",
          "isCorrect": false
        }
      ],
      "comment": "Чотири зони блокпоста забезпечують поступове уповільнення та перевірку транспорту: від обмеження руху до адміністративної зони. Кожна зона має своє призначення."
    },
    {
      "id": 47,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Дайте визначення поняттю патрульна база?",
      "answers": [
        {
          "text": "Статична позиція, з якої патруль може проводити операції",
          "isCorrect": true
        },
        {
          "text": "Тимчасовий пункт дислокації підрозділу під час рейдових дій",
          "isCorrect": false
        },
        {
          "text": "Місце збору патруля перед виходом на завдання",
          "isCorrect": false
        },
        {
          "text": "Укріплена позиція для ведення кругової оборони",
          "isCorrect": false
        }
      ],
      "comment": "Патрульна база - це тимчасова захищена позиція, звідки патруль виходить на завдання. На відміну від бази взводу, вона призначена для короткочасного перебування."
    },
    {
      "id": 48,
      "category": "Домедична допомога",
      "difficulty": 1,
      "question": "Види кровотечі?",
      "answers": [
        {
          "text": "Зовнішня і внутрішня",
          "isCorrect": true
        },
        {
          "text": "Первинна і вторинна",
          "isCorrect": false
        },
        {
          "text": "Гостра і хронічна",
          "isCorrect": false
        },
        {
          "text": "Травматична і патологічна",
          "isCorrect": false
        }
      ],
      "comment": "Два види кровотечі: зовнішня (видима) та внутрішня (прихована). Внутрішня небезпечніша, бо її важко виявити та зупинити в польових умовах."
    },
    {
      "id": 49,
      "category": "Домедична допомога",
      "difficulty": 1,
      "question": "Кровотечі поділяються на?",
      "answers": [
        {
          "text": "Артеріальну, венозну і капілярну",
          "isCorrect": true
        },
        {
          "text": "Сильну, помірну і слабку",
          "isCorrect": false
        },
        {
          "text": "Масивну, середню і незначну",
          "isCorrect": false
        },
        {
          "text": "Критичну, небезпечну і легку",
          "isCorrect": false
        }
      ],
      "comment": "Три типи кровотечі за судиною: артеріальна (яскраво-червона, пульсує), венозна (темна, тече рівномірно), капілярна (сочиться). Від типу залежить спосіб зупинки."
    },
    {
      "id": 50,
      "category": "Домедична допомога",
      "difficulty": 2,
      "question": "Основні завдання домедичної допомоги в умовах бою?",
      "answers": [
        {
          "text": "Виконати бойове завдання, запобігти більшій кількості поранених, вчасно та швидко надати домедичну допомогу пораненому і відправити на евакуацію",
          "isCorrect": true
        },
        {
          "text": "Врятувати життя пораненого, зупинити кровотечу, евакуювати в безпечне місце",
          "isCorrect": false
        },
        {
          "text": "Надати першу допомогу, викликати медиків, супроводжувати до госпіталю",
          "isCorrect": false
        },
        {
          "text": "Оцінити стан пораненого, стабілізувати його стан, організувати транспортування",
          "isCorrect": false
        }
      ],
      "comment": "Чотири завдання домедичної допомоги в бою: виконати бойове завдання (бій важливіший), запобігти новим втратам, швидко допомогти, організувати евакуацію."
    },
    {
      "id": 51,
      "category": "Домедична допомога",
      "difficulty": 1,
      "question": "Етапи домедичної допомоги?",
      "answers": [
        {
          "text": "Під вогнем, в укритті, під час евакуації",
          "isCorrect": true
        },
        {
          "text": "Невідкладна допомога, стабілізація, транспортування",
          "isCorrect": false
        },
        {
          "text": "На полі бою, на пункті збору, в медпункті",
          "isCorrect": false
        },
        {
          "text": "Первинний огляд, надання допомоги, евакуація",
          "isCorrect": false
        }
      ],
      "comment": "Три етапи TCCC: Care Under Fire (допомога під вогнем - тільки турнікет), Tactical Field Care (в укритті - повна допомога), TACEVAC (під час евакуації - підтримка стану)."
    },
    {
      "id": 52,
      "category": "Домедична допомога",
      "difficulty": 2,
      "question": "Назвіть характерну ознаку наскрізного поранення?",
      "answers": [
        {
          "text": "Наявність вхідного і вихідного отворів",
          "isCorrect": true
        },
        {
          "text": "Сильна кровотеча з рани",
          "isCorrect": false
        },
        {
          "text": "Великий розмір рани",
          "isCorrect": false
        },
        {
          "text": "Наявність стороннього предмета в рані",
          "isCorrect": false
        }
      ],
      "comment": "Наскрізне поранення має вхідний (менший) та вихідний (більший) отвори. Важливо обробити обидва отвори при наданні допомоги."
    },
    {
      "id": 53,
      "category": "Домедична допомога",
      "difficulty": 2,
      "question": "Витікання пінистої крові при пораненні грудей свідчить про?",
      "answers": [
        {
          "text": "Пневмоторакс",
          "isCorrect": true
        },
        {
          "text": "Гемоторакс",
          "isCorrect": false
        },
        {
          "text": "Пошкодження серця",
          "isCorrect": false
        },
        {
          "text": "Поранення легеневої артерії",
          "isCorrect": false
        }
      ],
      "comment": "Пінista кров при пораненні грудей = повітря в плевральній порожнині (пневмоторакс). Потрібна негайна герметизація рани оклюзійною пов'язкою."
    },
    {
      "id": 54,
      "category": "Домедична допомога",
      "difficulty": 2,
      "question": "При проникаючому пораненні грудей на рану слід накласти?",
      "answers": [
        {
          "text": "Оклюзійну пов'язку",
          "isCorrect": true
        },
        {
          "text": "Тиснучу пов'язку",
          "isCorrect": false
        },
        {
          "text": "Іммобілізаційну пов'язку",
          "isCorrect": false
        },
        {
          "text": "Асептичну пов'язку",
          "isCorrect": false
        }
      ],
      "comment": "Оклюзійна пов'язка герметизує рану грудної клітки, запобігаючи потраплянню повітря. Один край залишають вільним для виходу повітря при видиху (клапанний механізм)."
    },
    {
      "id": 55,
      "category": "Домедична допомога",
      "difficulty": 2,
      "question": "Для чого використовують назофарингеальну трубку?",
      "answers": [
        {
          "text": "Відновлення прохідності дихальних шляхів",
          "isCorrect": true
        },
        {
          "text": "Зупинка носової кровотечі",
          "isCorrect": false
        },
        {
          "text": "Подача кисню пораненому",
          "isCorrect": false
        },
        {
          "text": "Фіксація язика при непритомності",
          "isCorrect": false
        }
      ],
      "comment": "Назофарингеальна трубка вставляється через ніс у глотку для забезпечення прохідності дихальних шляхів при непритомності. Протипоказана при травмі основи черепа."
    },
    {
      "id": 56,
      "category": "Домедична допомога",
      "difficulty": 1,
      "question": "Що потрібно зробити при переломі кінцівки?",
      "answers": [
        {
          "text": "Іммобілізувати за допомогою підручних засобів",
          "isCorrect": true
        },
        {
          "text": "Спробувати вправити кістки на місце",
          "isCorrect": false
        },
        {
          "text": "Туго забинтувати місце перелому",
          "isCorrect": false
        },
        {
          "text": "Накласти холодний компрес і дати знеболююче",
          "isCorrect": false
        }
      ],
      "comment": "При переломі головне - іммобілізація (нерухомість). Не намагайтеся вправити кістки - це може пошкодити судини та нерви. Фіксуйте в тому положенні, як є."
    },
    {
      "id": 57,
      "category": "Домедична допомога",
      "difficulty": 2,
      "question": "На який час накладається джгут (турнікет) у теплу/холодну пору року?",
      "answers": [
        {
          "text": "2 години / 1 годину",
          "isCorrect": true
        },
        {
          "text": "1,5 години / 45 хвилин",
          "isCorrect": false
        },
        {
          "text": "3 години / 1,5 години",
          "isCorrect": false
        },
        {
          "text": "1 година / 30 хвилин",
          "isCorrect": false
        }
      ],
      "comment": "Час накладання джгута: влітку - 2 години, взимку - 1 година. На холоді тканини гинуть швидше через погіршення кровообігу. Обов'язково вказати час накладання!"
    },
    {
      "id": 58,
      "category": "Топографія та орієнтування",
      "difficulty": 1,
      "question": "Як позначаються дружні/свої сили на картах?",
      "answers": [
        {
          "text": "Прямокутником синього кольору",
          "isCorrect": true
        },
        {
          "text": "Колом зеленого кольору",
          "isCorrect": false
        },
        {
          "text": "Трикутником синього кольору",
          "isCorrect": false
        },
        {
          "text": "Квадратом синього кольору",
          "isCorrect": false
        }
      ],
      "comment": "Свої сили - синій прямокутник (НАТО стандарт). Синій = дружні, прямокутник = наземні сили. Запам'ятайте: СПС - Свої Прямокутник Синій."
    },
    {
      "id": 59,
      "category": "Топографія та орієнтування",
      "difficulty": 1,
      "question": "Як позначаються ворожі сили на картах?",
      "answers": [
        {
          "text": "Ромбом червоного кольору",
          "isCorrect": true
        },
        {
          "text": "Прямокутником червоного кольору",
          "isCorrect": false
        },
        {
          "text": "Трикутником червоного кольору",
          "isCorrect": false
        },
        {
          "text": "Колом червоного кольору",
          "isCorrect": false
        }
      ],
      "comment": "Ворожі сили - червоний ромб. Червоний = ворог, ромб = загроза. Легко запам'ятати: ромб гострий як загроза."
    },
    {
      "id": 60,
      "category": "Топографія та орієнтування",
      "difficulty": 1,
      "question": "Як позначаються невідомі сили на картах?",
      "answers": [
        {
          "text": "Чотирикутником жовтого кольору",
          "isCorrect": true
        },
        {
          "text": "Ромбом жовтого кольору",
          "isCorrect": false
        },
        {
          "text": "Колом жовтого кольору",
          "isCorrect": false
        },
        {
          "text": "Трикутником жовтого кольору",
          "isCorrect": false
        }
      ],
      "comment": "Невідомі сили - жовтий чотирикутник. Жовтий = увага/невизначеність. Використовується, коли не можемо ідентифікувати приналежність."
    },
    {
      "id": 61,
      "category": "Топографія та орієнтування",
      "difficulty": 1,
      "question": "Як позначаються нейтральні сили на картах?",
      "answers": [
        {
          "text": "Квадратом зеленого кольору",
          "isCorrect": true
        },
        {
          "text": "Прямокутником зеленого кольору",
          "isCorrect": false
        },
        {
          "text": "Колом білого кольору",
          "isCorrect": false
        },
        {
          "text": "Ромбом зеленого кольору",
          "isCorrect": false
        }
      ],
      "comment": "Нейтральні сили - зелений квадрат. Зелений = нейтральний/безпечний. Це можуть бути миротворці або медичні служби."
    },
    {
      "id": 62,
      "category": "Топографія та орієнтування",
      "difficulty": 1,
      "question": "Що таке орієнтир?",
      "answers": [
        {
          "text": "Характерний, добре видимий на місцевості нерухомий предмет або елемент рельєфу",
          "isCorrect": true
        },
        {
          "text": "Напрямок руху підрозділу на місцевості",
          "isCorrect": false
        },
        {
          "text": "Точка на карті для визначення координат",
          "isCorrect": false
        },
        {
          "text": "Умовна позначка для цілевказівки",
          "isCorrect": false
        }
      ],
      "comment": "Орієнтир - це об'єкт для визначення місцезнаходження та цілевказівки. Має бути: нерухомий, помітний, довговічний. Приклад: вежа, перехрестя, окреме дерево."
    },
    {
      "id": 63,
      "category": "Топографія та орієнтування",
      "difficulty": 1,
      "question": "Що таке азимут?",
      "answers": [
        {
          "text": "Кут між напрямком на Північ і напрямком на певну точку",
          "isCorrect": true
        },
        {
          "text": "Відстань від спостерігача до об'єкта",
          "isCorrect": false
        },
        {
          "text": "Висота об'єкта над рівнем моря",
          "isCorrect": false
        },
        {
          "text": "Координати об'єкта на карті",
          "isCorrect": false
        }
      ],
      "comment": "Азимут - кут від напрямку на північ за годинниковою стрілкою до напрямку на об'єкт. Вимірюється від 0° до 360°. Північ = 0°/360°, схід = 90°, південь = 180°, захід = 270°."
    },
    {
      "id": 64,
      "category": "Топографія та орієнтування",
      "difficulty": 1,
      "question": "Якими бувають орієнтири місцевості?",
      "answers": [
        {
          "text": "Площинні, лінійні, точкові",
          "isCorrect": true
        },
        {
          "text": "Великі, середні, малі",
          "isCorrect": false
        },
        {
          "text": "Природні, штучні, змішані",
          "isCorrect": false
        },
        {
          "text": "Постійні, тимчасові, сезонні",
          "isCorrect": false
        }
      ],
      "comment": "Три типи орієнтирів: точкові (вежа, дерево), лінійні (дорога, річка), площинні (ліс, озеро). Кожен тип використовується для різних цілей орієнтування."
    },
    {
      "id": 65,
      "category": "Топографія та орієнтування",
      "difficulty": 2,
      "question": "Назвіть способи цілевказівки?",
      "answers": [
        {
          "text": "Від орієнтира, по азимуту і відстані до цілі, від напрямку руху, азимутальним покажчиком, наведенням гармати на ціль, трасуючими кулями і сигнальними ракетами",
          "isCorrect": true
        },
        {
          "text": "За картою, по компасу, за допомогою GPS, візуально, по радіо",
          "isCorrect": false
        },
        {
          "text": "Прямим наведенням, координатами, орієнтирами, сигналами",
          "isCorrect": false
        },
        {
          "text": "Словесний опис, графічне зображення, умовні знаки, світлові сигнали",
          "isCorrect": false
        }
      ],
      "comment": "Шість способів цілевказівки дозволяють точно вказати ціль в будь-яких умовах. Найпростіший - від орієнтира, найточніший - наведенням гармати."
    },
    {
      "id": 66,
      "category": "Топографія та орієнтування",
      "difficulty": 2,
      "question": "За якою формулою визначають довжину кроку людини?",
      "answers": [
        {
          "text": "Д = Р/4 + 37 (Р - ріст людини)",
          "isCorrect": true
        },
        {
          "text": "Д = Р/2 + 50 (Р - ріст людини)",
          "isCorrect": false
        },
        {
          "text": "Д = Р/3 + 25 (Р - ріст людини)",
          "isCorrect": false
        },
        {
          "text": "Д = Р/5 + 45 (Р - ріст людини)",
          "isCorrect": false
        }
      ],
      "comment": "Формула довжини кроку: Д = Р/4 + 37 (см). Приклад: при рості 180 см довжина кроку = 180/4 + 37 = 82 см. Використовується для вимірювання відстані кроками."
    },
    {
      "id": 67,
      "category": "Топографія та орієнтування",
      "difficulty": 3,
      "question": "За якою формулою визначається відстань, якщо відомо кутові розміри предмету?",
      "answers": [
        {
          "text": "Д = В/К × 1000 (В - висота предмету, К - кут)",
          "isCorrect": true
        },
        {
          "text": "Д = В × К × 100 (В - висота предмету, К - кут)",
          "isCorrect": false
        },
        {
          "text": "Д = К/В × 1000 (В - висота предмету, К - кут)",
          "isCorrect": false
        },
        {
          "text": "Д = В × 1000/К × 2 (В - висота предмету, К - кут)",
          "isCorrect": false
        }
      ],
      "comment": "Формула тисячної: Д = В/К × 1000, де В - реальна висота об'єкта в метрах, К - кутовий розмір в тисячних. Основний спосіб визначення відстані в польових умовах."
    },
    {
      "id": 68,
      "category": "Топографія та орієнтування",
      "difficulty": 3,
      "question": "За якою формулою визначається відстань, якщо відомо лінійні розміри предмету?",
      "answers": [
        {
          "text": "Д = В(см)/В(мм) × 5 (В(см) - висота предмету, В(мм) - ширина предмету)",
          "isCorrect": true
        },
        {
          "text": "Д = В(см) × В(мм) × 10",
          "isCorrect": false
        },
        {
          "text": "Д = В(мм)/В(см) × 5",
          "isCorrect": false
        },
        {
          "text": "Д = В(см) + В(мм) × 5",
          "isCorrect": false
        }
      ],
      "comment": "Формула для лінійних розмірів використовує співвідношення видимих розмірів. Множник 5 - це константа для переведення в метри при вимірюванні в міліметрах."
    },
    {
      "id": 69,
      "category": "Топографія та орієнтування",
      "difficulty": 2,
      "question": "Назвіть тактичні властивості місцевості?",
      "answers": [
        {
          "text": "Прохідність місцевості, захисні властивості, умови орієнтування, умови спостереження, умови маскування, умови ведення вогню, умови інженерного обладнання місцевості",
          "isCorrect": true
        },
        {
          "text": "Рельєф, рослинність, водні об'єкти, населені пункти, дороги",
          "isCorrect": false
        },
        {
          "text": "Відкритість, закритість, пересіченість, прохідність, оглядовість",
          "isCorrect": false
        },
        {
          "text": "Висота, ширина, довжина, площа, периметр, об'єм",
          "isCorrect": false
        }
      ],
      "comment": "Сім тактичних властивостей місцевості визначають можливості ведення бою. Запам'ятайте абревіатуру: ПЗО-СМВ-І (Прохідність, Захист, Орієнтування, Спостереження, Маскування, Вогонь, Інженерне обладнання)."
    },
    {
      "id": 70,
      "category": "Топографія та орієнтування",
      "difficulty": 2,
      "question": "Вкажіть ознаки місцевих предметів?",
      "answers": [
        {
          "text": "Ґрунтово-рослинний покрив, гідрографія, населені пункти, мережа доріг, об'єкти промислового та сільськогосподарського призначення",
          "isCorrect": true
        },
        {
          "text": "Природні, штучні, тимчасові, постійні, сезонні",
          "isCorrect": false
        },
        {
          "text": "Великі, середні, малі, точкові, лінійні",
          "isCorrect": false
        },
        {
          "text": "Видимі, приховані, маскувальні, орієнтирні, допоміжні",
          "isCorrect": false
        }
      ],
      "comment": "П'ять ознак місцевих предметів: рельєф, рослинність, гідрографія (води), дороги, населені пункти. Ці елементи формують тактичні властивості місцевості."
    },
    {
      "id": 71,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "На яку відстань взвод займає оборону по фронту?",
      "answers": [
        {
          "text": "До 400 метрів",
          "isCorrect": true
        },
        {
          "text": "До 300 метрів",
          "isCorrect": false
        },
        {
          "text": "До 500 метрів",
          "isCorrect": false
        },
        {
          "text": "До 600 метрів",
          "isCorrect": false
        }
      ],
      "comment": "Взвод в обороні займає фронт до 400 метрів. Це оптимальна відстань для контролю вогнем та управління. Відділення - до 100 м, рота - до 1500 м."
    },
    {
      "id": 72,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Назвіть тип укриття, що забезпечує більш надійний захист особового складу від засобів ураження та використовується для відпочинку на рівні відділення (взводу)?",
      "answers": [
        {
          "text": "Бліндаж",
          "isCorrect": true
        },
        {
          "text": "Окоп",
          "isCorrect": false
        },
        {
          "text": "Траншея",
          "isCorrect": false
        },
        {
          "text": "Сховище",
          "isCorrect": false
        }
      ],
      "comment": "Бліндаж - накрите зверху укриття для захисту від осколків та куль, на відміну від окопу (відкритий зверху). Використовується для відпочинку та укриття від негоди."
    },
    {
      "id": 73,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Назвіть складові елементи загальновійськового бою?",
      "answers": [
        {
          "text": "Удар, вогонь і маневр",
          "isCorrect": true
        },
        {
          "text": "Наступ, оборона, відхід",
          "isCorrect": false
        },
        {
          "text": "Розвідка, атака, закріплення",
          "isCorrect": false
        },
        {
          "text": "Підготовка, проведення, завершення",
          "isCorrect": false
        }
      ],
      "comment": "Три елементи бою: удар (фізичне знищення), вогонь (ураження вогневими засобами), маневр (переміщення для отримання переваги). Всі три використовуються комплексно."
    },
    {
      "id": 74,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Назвіть види маневру?",
      "answers": [
        {
          "text": "Охоплення, обхід, відхід і маневр вогнем",
          "isCorrect": true
        },
        {
          "text": "Фронтальний, фланговий, тиловий",
          "isCorrect": false
        },
        {
          "text": "Наступальний, оборонний, відступальний",
          "isCorrect": false
        },
        {
          "text": "Швидкий, повільний, комбінований",
          "isCorrect": false
        }
      ],
      "comment": "Чотири види маневру: охоплення (з флангу), обхід (в тил), відхід (відступ з боєм), маневр вогнем (перенесення вогню). Запам'ятайте: ООВ+В."
    },
    {
      "id": 75,
      "category": "Тактика та бойові дії",
      "difficulty": 1,
      "question": "Що встановлюється на кожну добу для впізнавання своїх військовослужбовців?",
      "answers": [
        {
          "text": "Пропуск і відгук",
          "isCorrect": true
        },
        {
          "text": "Гасло і пароль",
          "isCorrect": false
        },
        {
          "text": "Знак і сигнал",
          "isCorrect": false
        },
        {
          "text": "Код і шифр",
          "isCorrect": false
        }
      ],
      "comment": "Пропуск і відгук - система розпізнавання свій-чужий. Пропуск - питання, відгук - відповідь. Змінюються щодоби о 22:00. Приклад: 'Дніпро' - 'Київ'."
    },
    {
      "id": 76,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Залежно від умов обстановки і особливостей бойового завдання підрозділ у визначеній організаційній структурі може діяти в таких видах порядку?",
      "answers": [
        {
          "text": "Похідний, бойовий і передбойовий",
          "isCorrect": true
        },
        {
          "text": "Наступальний, оборонний, маневрений",
          "isCorrect": false
        },
        {
          "text": "Розгорнутий, згорнутий, змішаний",
          "isCorrect": false
        },
        {
          "text": "Лінійний, колонний, розосереджений",
          "isCorrect": false
        }
      ],
      "comment": "Три види бойового порядку: похідний (для маршу), передбойовий (готовність до бою), бойовий (безпосередньо для бою). Перехід між ними - розгортання/згортання."
    },
    {
      "id": 77,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "За напрямком стрільби вогонь поділяється на?",
      "answers": [
        {
          "text": "Фронтальний, фланговий, перехресний",
          "isCorrect": true
        },
        {
          "text": "Прямий, непрямий, навісний",
          "isCorrect": false
        },
        {
          "text": "Зосереджений, розподілений, загороджувальний",
          "isCorrect": false
        },
        {
          "text": "Ближній, середній, дальній",
          "isCorrect": false
        }
      ],
      "comment": "Три напрямки вогню: фронтальний (прямо), фланговий (збоку вздовж фронту), перехресний (з різних напрямків). Найефективніший - перехресний."
    },
    {
      "id": 78,
      "category": "Тактика та бойові дії",
      "difficulty": 3,
      "question": "Що означає поняття бойової спроможності?",
      "answers": [
        {
          "text": "Це сукупність властивостей, які притаманні військовій організаційній структурі та характеризують її здатність виконати поставлені завдання за умови збереження власної боєздатності на рівні, який забезпечує подальше їх виконання",
          "isCorrect": true
        },
        {
          "text": "Готовність підрозділу до виконання бойового завдання в будь-яких умовах",
          "isCorrect": false
        },
        {
          "text": "Рівень укомплектованості особовим складом та озброєнням",
          "isCorrect": false
        },
        {
          "text": "Здатність підрозділу вести бойові дії протягом визначеного часу",
          "isCorrect": false
        }
      ],
      "comment": "Бойова спроможність - комплексна характеристика готовності до бою. Включає: укомплектованість, навченість, озброєння, морально-психологічний стан, забезпечення."
    },
    {
      "id": 79,
      "category": "Тактика та бойові дії",
      "difficulty": 1,
      "question": "Які є види оборони?",
      "answers": [
        {
          "text": "Позиційна та маневрена",
          "isCorrect": true
        },
        {
          "text": "Активна та пасивна",
          "isCorrect": false
        },
        {
          "text": "Кругова та фронтальна",
          "isCorrect": false
        },
        {
          "text": "Тимчасова та постійна",
          "isCorrect": false
        }
      ],
      "comment": "Два види оборони: позиційна (утримання рубежів) та маневрена (оборона з відходом). Вибір залежить від завдання, місцевості та співвідношення сил."
    },
    {
      "id": 80,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Що є особливими умовами ведення загальновійськового бою?",
      "answers": [
        {
          "text": "Ведення бою у населеному пункті, на водній перешкоді (морському узбережжі), вночі, взимку, в лісі, в горах, у степовій місцевості",
          "isCorrect": true
        },
        {
          "text": "Ведення бою при поганій погоді, в умовах обмеженої видимості, при відсутності зв'язку",
          "isCorrect": false
        },
        {
          "text": "Ведення бою без підтримки артилерії, без резервів, в оточенні",
          "isCorrect": false
        },
        {
          "text": "Ведення бою проти переважаючих сил противника, в умовах РХБ зараження, при відсутності боєприпасів",
          "isCorrect": false
        }
      ],
      "comment": "Сім особливих умов бою вимагають спеціальної підготовки: населений пункт (ближній бій), водна перешкода (форсування), ніч (обмежена видимість), зима (холод), ліс (обмежений маневр), гори (вертикальний бій), степ (відсутність укриттів)."
    },
    {
      "id": 81,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "У чому полягає маневр вогнем?",
      "answers": [
        {
          "text": "У зосередженні, розподіленні і перенесенні вогню",
          "isCorrect": true
        },
        {
          "text": "У зміні позицій вогневих засобів",
          "isCorrect": false
        },
        {
          "text": "У чергуванні видів вогню",
          "isCorrect": false
        },
        {
          "text": "У зміні темпу стрільби",
          "isCorrect": false
        }
      ],
      "comment": "Маневр вогнем - це керування вогнем без переміщення зброї: зосередження (по одній цілі), розподілення (по фронту), перенесення (з цілі на ціль)."
    },
    {
      "id": 82,
      "category": "Процес управління підрозділом",
      "difficulty": 2,
      "question": "Опишіть поняття TLP?",
      "answers": [
        {
          "text": "Процедура управління підрозділом на рівні відділення-рота",
          "isCorrect": true
        },
        {
          "text": "Тактичні навчання лідерського складу",
          "isCorrect": false
        },
        {
          "text": "План тактичної підготовки підрозділу",
          "isCorrect": false
        },
        {
          "text": "Система оцінки бойової готовності",
          "isCorrect": false
        }
      ],
      "comment": "TLP (Troop Leading Procedures) - покроковий алгоритм прийняття рішення та підготовки підрозділу до бою на тактичному рівні. Забезпечує нічого не забути в стресі бою."
    },
    {
      "id": 83,
      "category": "Процес управління підрозділом",
      "difficulty": 3,
      "question": "З яких пунктів складається Процес управління підрозділом?",
      "answers": [
        {
          "text": "Отримання завдання, видача попереднього розпорядження, розробка попереднього плану дій, початок переміщень, проведення розвідки, завершення плану дій, видача бойового розпорядження, контроль",
          "isCorrect": true
        },
        {
          "text": "Планування, організація, мотивація, контроль, координація",
          "isCorrect": false
        },
        {
          "text": "Аналіз обстановки, прийняття рішення, постановка завдань, організація взаємодії, контроль виконання",
          "isCorrect": false
        },
        {
          "text": "Підготовка, виконання, звітування, аналіз результатів",
          "isCorrect": false
        }
      ],
      "comment": "Вісім кроків TLP: отримання завдання → попереднє розпорядження → план → рух → розвідка → уточнення плану → бойовий наказ → контроль. Послідовність критична!"
    },
    {
      "id": 84,
      "category": "Процес управління підрозділом",
      "difficulty": 2,
      "question": "З яких пунктів складається МЕТТ-ТС?",
      "answers": [
        {
          "text": "Завдання, аналіз противника, місцевість і погода, наявні дружні сили, час, цивільні",
          "isCorrect": true
        },
        {
          "text": "Мета, ефективність, тактика, техніка, час, ситуація",
          "isCorrect": false
        },
        {
          "text": "Місія, ворог, територія, техніка, термін, супровід",
          "isCorrect": false
        },
        {
          "text": "Маневр, евакуація, транспорт, тил, телекомунікації, снайпери",
          "isCorrect": false
        }
      ],
      "comment": "МЕТТ-ТС - мнемоніка для аналізу обстановки: Mission (завдання), Enemy (противник), Terrain & Weather (місцевість і погода), Troops (свої сили), Time (час), Civilians (цивільні)."
    },
    {
      "id": 85,
      "category": "Процес управління підрозділом",
      "difficulty": 2,
      "question": "До складу якого алгоритму входить OAKOC?",
      "answers": [
        {
          "text": "МЕТТ-ТС",
          "isCorrect": true
        },
        {
          "text": "TLP",
          "isCorrect": false
        },
        {
          "text": "EDIP",
          "isCorrect": false
        },
        {
          "text": "NATO STANAG",
          "isCorrect": false
        }
      ],
      "comment": "OAKOC - частина аналізу місцевості в МЕТТ-ТС: Observation (спостереження), Avenues of approach (шляхи підходу), Key terrain (ключова місцевість), Obstacles (перешкоди), Cover and concealment (укриття та маскування)."
    },
    {
      "id": 86,
      "category": "Процес управління підрозділом",
      "difficulty": 2,
      "question": "Які види бойових розпоряджень є?",
      "answers": [
        {
          "text": "Попереднє бойове розпорядження (наказ), Бойове розпорядження (наказ), Фрагментарне бойове розпорядження (наказ)",
          "isCorrect": true
        },
        {
          "text": "Усне, письмове, графічне",
          "isCorrect": false
        },
        {
          "text": "Терміновий, плановий, додатковий",
          "isCorrect": false
        },
        {
          "text": "Основний, допоміжний, резервний",
          "isCorrect": false
        }
      ],
      "comment": "Три види наказів: попередній (WARNO - для початку підготовки), основний (OPORD - повний наказ), фрагментарний (FRAGO - зміни до наказу). Кожен має своє призначення."
    },
    {
      "id": 87,
      "category": "Процес управління підрозділом",
      "difficulty": 2,
      "question": "Що означає правило 1/3-2/3 під час планування та видачі бойового наказу?",
      "answers": [
        {
          "text": "1/3 часу відводиться командиру, 2/3 - підлеглим",
          "isCorrect": true
        },
        {
          "text": "1/3 сил в резерві, 2/3 в бою",
          "isCorrect": false
        },
        {
          "text": "1/3 на планування, 2/3 на виконання",
          "isCorrect": false
        },
        {
          "text": "1/3 на розвідку, 2/3 на бойові дії",
          "isCorrect": false
        }
      ],
      "comment": "Правило 1/3-2/3 часу: командир використовує лише 1/3 наявного часу на своє планування, 2/3 віддає підлеглим. Це дає їм час на підготовку. Приклад: є 3 години = 1 година командиру, 2 години підлеглим."
    },
    {
      "id": 88,
      "category": "Процес управління підрозділом",
      "difficulty": 3,
      "question": "Відповідно до якого з планів дій противника розробляється та аналізується план дій свого підрозділу?",
      "answers": [
        {
          "text": "Найбільш ймовірного та найбільш небезпечного плану дій",
          "isCorrect": true
        },
        {
          "text": "Основного та запасного плану дій",
          "isCorrect": false
        },
        {
          "text": "Мінімального та максимального плану дій",
          "isCorrect": false
        },
        {
          "text": "Першочергового та альтернативного плану дій",
          "isCorrect": false
        }
      ],
      "comment": "Два плани противника: найбільш ймовірний (що він скоріше зробить) та найбільш небезпечний (що буде найгірше для нас). Готуємося до обох варіантів."
    },
    {
      "id": 89,
      "category": "Процес управління підрозділом",
      "difficulty": 2,
      "question": "Назвіть пункти бойового наказу?",
      "answers": [
        {
          "text": "Ситуація, завдання, виконання, забезпечення та підтримка, управління та зв'язок",
          "isCorrect": true
        },
        {
          "text": "Обстановка, мета, план, ресурси, контроль",
          "isCorrect": false
        },
        {
          "text": "Вступ, основна частина, заключення, додатки",
          "isCorrect": false
        },
        {
          "text": "Аналіз, рішення, завдання, координація, звітність",
          "isCorrect": false
        }
      ],
      "comment": "П'ять пунктів бойового наказу (SMEAC): Situation (ситуація), Mission (завдання), Execution (виконання), Admin & Logistics (забезпечення), Command & Signal (управління та зв'язок)."
    },
    {
      "id": 90,
      "category": "Мінно-вибухова справа",
      "difficulty": 1,
      "question": "Назвіть види протитанкових мін?",
      "answers": [
        {
          "text": "Протигусеничні, протиднищеві, протибортові",
          "isCorrect": true
        },
        {
          "text": "Контактні, безконтактні, комбіновані",
          "isCorrect": false
        },
        {
          "text": "Фугасні, кумулятивні, осколкові",
          "isCorrect": false
        },
        {
          "text": "Натискні, натяжні, сейсмічні",
          "isCorrect": false
        }
      ],
      "comment": "Три типи протитанкових мін за принципом ураження: протигусеничні (рвуть гусениці), протиднищеві (пробивають днище), протибортові (б'ють в борт). Кожна для своєї цілі."
    },
    {
      "id": 91,
      "category": "Мінно-вибухова справа",
      "difficulty": 1,
      "question": "Назвіть види протипіхотних мін?",
      "answers": [
        {
          "text": "Фугасні, осколкові",
          "isCorrect": true
        },
        {
          "text": "Натискні, розтяжні",
          "isCorrect": false
        },
        {
          "text": "Контактні, дистанційні",
          "isCorrect": false
        },
        {
          "text": "Поверхневі, заглиблені",
          "isCorrect": false
        }
      ],
      "comment": "Два типи протипіхотних мін за принципом ураження: фугасні (ударна хвиля відриває кінцівки) та осколкові (уражають осколками). Фугасні - проти одного, осколкові - проти групи."
    },
    {
      "id": 92,
      "category": "Мінно-вибухова справа",
      "difficulty": 2,
      "question": "Протипіхотні осколкові міни бувають?",
      "answers": [
        {
          "text": "Направленої дії та кругового ураження",
          "isCorrect": true
        },
        {
          "text": "Ближньої та дальньої дії",
          "isCorrect": false
        },
        {
          "text": "Наземні та підземні",
          "isCorrect": false
        },
        {
          "text": "Стаціонарні та мобільні",
          "isCorrect": false
        }
      ],
      "comment": "Осколкові міни: направленої дії (сектор ураження, як МОН) та кругового ураження (360°, як ОЗМ-72 'стрибаюча'). Перші для засідок, другі для периметра."
    },
    {
      "id": 93,
      "category": "Мінно-вибухова справа",
      "difficulty": 2,
      "question": "До якого виду мін відносяться ПМН-3, ПМН-4?",
      "answers": [
        {
          "text": "Протипіхотні міни фугасні",
          "isCorrect": true
        },
        {
          "text": "Протипіхотні міни осколкові",
          "isCorrect": false
        },
        {
          "text": "Протитанкові міни фугасні",
          "isCorrect": false
        },
        {
          "text": "Універсальні міни",
          "isCorrect": false
        }
      ],
      "comment": "ПМН (протипіхотна міна натискна) - фугасна міна. Цифри 3, 4 - це модифікації. Спрацьовує від натиску 8-25 кг, відриває ступню."
    },
    {
      "id": 94,
      "category": "Мінно-вибухова справа",
      "difficulty": 2,
      "question": "До якого виду мін відносяться ОЗМ-72, ПОМ-2?",
      "answers": [
        {
          "text": "Протипіхотні міни осколкові кругового ураження",
          "isCorrect": true
        },
        {
          "text": "Протипіхотні міни осколкові направленої дії",
          "isCorrect": false
        },
        {
          "text": "Протипіхотні міни фугасні",
          "isCorrect": false
        },
        {
          "text": "Протитанкові міни осколкові",
          "isCorrect": false
        }
      ],
      "comment": "ОЗМ-72 - осколкова міна кругового ураження, 'стрибаюча'. При спрацюванні вилітає вгору на 0.6-0.9 м і вибухає. ПОМ-2 - також кругового ураження, але стаціонарна."
    },
    {
      "id": 95,
      "category": "Мінно-вибухова справа",
      "difficulty": 2,
      "question": "До якого типу мін відносяться МОН-50, МОН-90?",
      "answers": [
        {
          "text": "Протипіхотні міни осколкові направленої дії",
          "isCorrect": true
        },
        {
          "text": "Протипіхотні міни осколкові кругового ураження",
          "isCorrect": false
        },
        {
          "text": "Протипіхотні міни фугасні",
          "isCorrect": false
        },
        {
          "text": "Протитанкові міни кумулятивні",
          "isCorrect": false
        }
      ],
      "comment": "МОН (міна осколкова направлена) - міна типу 'Клеймор'. МОН-50 має 540 осколків, МОН-90 - 2000 осколків. Сектор ураження 54°, дальність до 50-90 м."
    },
    {
      "id": 96,
      "category": "Мінно-вибухова справа",
      "difficulty": 2,
      "question": "До якого типу мін відносяться ТМ-72, ТМК-2?",
      "answers": [
        {
          "text": "Протиднищеві протитанкові міни",
          "isCorrect": true
        },
        {
          "text": "Протигусеничні протитанкові міни",
          "isCorrect": false
        },
        {
          "text": "Протибортові протитанкові міни",
          "isCorrect": false
        },
        {
          "text": "Універсальні протитанкові міни",
          "isCorrect": false
        }
      ],
      "comment": "ТМ-72 та ТМК-2 - протиднищеві міни з магнітним датчиком. Спрацьовують під днищем техніки без контакту. Важко виявляються міношукачем."
    },
    {
      "id": 97,
      "category": "Мінно-вибухова справа",
      "difficulty": 2,
      "question": "На які групи поділяються вибухові речовини?",
      "answers": [
        {
          "text": "Ініціюючі, бризантні, метальні",
          "isCorrect": true
        },
        {
          "text": "Первинні, вторинні, третинні",
          "isCorrect": false
        },
        {
          "text": "Слабкі, середні, потужні",
          "isCorrect": false
        },
        {
          "text": "Твердіз, рідкі, газоподібні",
          "isCorrect": false
        }
      ],
      "comment": "Три групи вибухових речовин: ініціюючі (детонатор, чутливі), бризантні (основний заряд, тротил), метальні (порох, викидають снаряд). Використовуються послідовно."
    },
    {
      "id": 98,
      "category": "Мінно-вибухова справа",
      "difficulty": 3,
      "question": "Перерахуйте характеристики вибухових речовин?",
      "answers": [
        {
          "text": "Чутливість до зовнішньої дії, енергія вибухового перетворення, швидкість детонації, бризантність, фугасність",
          "isCorrect": true
        },
        {
          "text": "Потужність, стійкість, безпечність, доступність, вартість",
          "isCorrect": false
        },
        {
          "text": "Температура вибуху, тиск детонації, об'єм газів, час дії, радіус ураження",
          "isCorrect": false
        },
        {
          "text": "Щільність, в'язкість, температура плавлення, хімічний склад, термін зберігання",
          "isCorrect": false
        }
      ],
      "comment": "П'ять характеристик ВР: чутливість (легкість детонації), енергія (потужність), швидкість детонації (км/с), бризантність (дроблення), фугасність (виштовхування)."
    },
    {
      "id": 99,
      "category": "Тактика та бойові дії",
      "difficulty": 2,
      "question": "Основними способами ведення загальновійськового бою є?",
      "answers": [
        {
          "text": "Послідовний розгром та одночасне ураження",
          "isCorrect": true
        },
        {
          "text": "Фронтальний наступ та флангова атака",
          "isCorrect": false
        },
        {
          "text": "Оборона та контрнаступ",
          "isCorrect": false
        },
        {
          "text": "Блокування та знищення",
          "isCorrect": false
        }
      ],
      "comment": "Два способи ведення бою відрізняються масштабом: послідовний розгром (по черзі знищуємо окремі групи ворога), одночасне ураження (атакуємо всі цілі одночасно масованим вогнем)."
    },
    {
      "id": 100,
      "category": "Тактика та бойові дії",
      "difficulty": 1,
      "question": "Що є силами та засобами ведення загальновійськового бою?",
      "answers": [
        {
          "text": "Військові частини та підрозділи родів військ та спеціальних військ ЗСУ",
          "isCorrect": true
        },
        {
          "text": "Особовий склад, озброєння та військова техніка",
          "isCorrect": false
        },
        {
          "text": "Піхота, артилерія, авіація та флот",
          "isCorrect": false
        },
        {
          "text": "Бойові, забезпечувальні та тилові підрозділи",
          "isCorrect": false
        }
      ],
      "comment": "Сили та засоби бою - це всі військові частини та підрозділи різних родів військ (піхота, танки, артилерія) та спеціальних військ (інженерні, зв'язку, РХБ захисту), які беруть участь у бою."
    },
    {
    "id": 101,
    "category": "Практичні завдання (Розрахунки та Алгоритми)",
    "difficulty": 3,
    "question": "Практичне завдання №1: «Складання картки вогню механізованого відділення» (Назвіть обов'язкові елементи, що необхідно нанести).",
    "answers": [
      {
        "text": "Рішення вимагає знання повного алгоритму.",
        "isCorrect": true
      }
    ],
    "comment": "Картка вогню (Fire Card) — це ключовий тактичний документ, який командир відділення складає для організації оборони. На неї обов'язково наносяться: 1. **Орієнтири**, їх номери, найменування та відстані до них (не менше трьох). 2. **Положення противника** (не менше двох виявлених об'єктів). 3. **Позиція відділення**. 4. **Основні та додаткові смуги вогню**. 5. **Основна та запасна позиція для БМП**. Також вказуються додаткові сектори обстрілу, зона зосередженого вогню та сектори ведення вогню для кожного бійця."
  },
  {
    "id": 102,
    "category": "Практичні завдання (Розрахунки та Алгоритми)",
    "difficulty": 3,
    "question": "Практичне завдання №2: «Алгоритм дій при наданні домедичної допомоги пораненому безпосередньо в секторі обстрілу» (Combat Zone).",
    "answers": [
      {
        "text": "Рішення вимагає знання алгоритму TCCC (CUF).",
        "isCorrect": true
      }
    ],
    "comment": "Алгоритм дій в зоні обстрілу (Care Under Fire - CUF) є першим етапом TCCC (Tactical Combat Casualty Care). Пріоритет: **ВОГОНЬ У ВІДПОВІДЬ І УКРИТТЯ!**. 1. Придушити вогонь противника. 2. Визначити, чи поранений може переміститися в укриття самостійно. 3. Якщо поранений має масивну кровотечу, що загрожує життю, **накласти турнікет** (CAT) на високе положення над раною (самодопомога або допомога товариша). **НЕ ПРОВОДИТИ** інші маніпуляції (перевірка дихальних шляхів, оцінка шоку) до переміщення в зону укриття."
  },
  {
    "id": 103,
    "category": "Практичні завдання (Розрахунки та Алгоритми)",
    "difficulty": 2,
    "question": "Практичне завдання №3: На топографічній карті визначити за допомогою чисельного масштабу відстань між точками А і В. Яка формула використовується?",
    "answers": [
      {
        "text": "Рішення вимагає знання формули.",
        "isCorrect": true
      }
    ],
    "comment": "Визначення відстані за чисельним масштабом: **Р (км) = Р (см) * М (км)**. Де: 1. **Р (см)** — вимірювальна відстань між точками на карті (в сантиметрах). 2. **М (км)** — величина масштабу, що показує, скільки кілометрів на місцевості відповідає 1 см на карті (наприклад, для масштабу 1:50000, 1 см = 0.5 км). Наприклад: на карті 1:50000 відстань 4 см -> 4 см * 0.5 км/см = 2 км."
  },
  {
    "id": 104,
    "category": "Практичні завдання (Топографія)",
    "difficulty": 2,
    "question": "Практичне завдання №4: На топографічній карті знайти об’єкт за прямокутними координатами (Назвіть послідовність дій).",
    "answers": [
      {
        "text": "Рішення вимагає знання алгоритму.",
        "isCorrect": true
      }
    ],
    "comment": "Алгоритм пошуку об'єкта за прямокутними координатами (наприклад, 48 35700 м - 34 82100 м): 1. **Визначити квадрат:** Знайти на карті вертикальну лінію, що відповідає першим двом цифрам Сходу (X) (наприклад, 82) та горизонтальну лінію, що відповідає першим двом цифрам Півночі (Y) (наприклад, 35). 2. **Розділити квадрат:** Використовуючи третю (сотні метрів) та четверту (десятки метрів) цифри, відкласти на схід від початкової вертикальної лінії (наприклад, 100 м) та на північ від початкової горизонтальної лінії (наприклад, 700 м). Це і буде місцезнаходження об'єкта."
  },
  {
    "id": 105,
    "category": "Практичні завдання (Топографія)",
    "difficulty": 2,
    "question": "Практичне завдання №5: На топографічній карті визначити прямокутні координати об’єкта (Назвіть послідовність дій).",
    "answers": [
      {
        "text": "Рішення вимагає знання алгоритму.",
        "isCorrect": true
      }
    ],
    "comment": "Алгоритм визначення прямокутних координат: 1. **Визначити квадрат:** Знайти квадрат кілометрової сітки, в якому знаходиться об'єкт, і записати його координати (наприклад, для квадрата 3482-4835 — записуємо 82 і 35). 2. **Виміряти Схід (X):** Виміряти відстань від лівої (західної) вертикальної лінії сітки до об'єкта. Перевести її у метри та додати до перших двох цифр Сходу. 3. **Виміряти Північ (Y):** Виміряти відстань від нижньої (південної) горизонтальної лінії сітки до об'єкта. Перевести її у метри та додати до перших двох цифр Півночі. **Правило:** Спочатку Схід (X), потім Північ (Y). Звірка координат відбувається зліва направо та знизу вгору."
  },
  {
    "id": 106,
    "category": "Практичні завдання (Вогнева підготовка)",
    "difficulty": 3,
    "question": "Практичне завдання №6: Розрахунок несе бойове чергування. Вдень -9°C, вночі -26°C. На відстані 450 м стрілець бачить ціль ростом 170 см. На яку відмітку треба встановити прицільну планку (АК-74)?",
    "answers": [
      {
        "text": "Приціл встановлюється в положення «5» (з урахуванням поправки на мороз).",
        "isCorrect": true
      }
    ],
    "comment": "1. **Визначення прицілу:** Для АК-74 (пристріляного на позначку 'П') на відстані 450 м ціль уражається прицілом «4» або «5». 2. **Поправка на мороз:** При температурі повітря **нижче 0°C** (в даному випадку -26°C), траєкторія кулі стає пологішою через зміну порохового заряду. При температурі **нижче -25°C** (або -20°C) на відстані **до 500 м** приціл збільшується на **один розподіл**. Отже, замість «4» використовуємо **«5»**. 3. **Додаткова поправка:** Оскільки ціль ростова і стоїть на відстані 450 м (між 400 і 500 м), стрілець цілиться під зріз цілі (в її пояс/нижню частину), щоб уразити її в груди."
  },
  {
    "id": 107,
    "category": "Практичні завдання (Розрахунки та Алгоритми)",
    "difficulty": 3,
    "question": "Практичне завдання №7: Визначте середню кількість патронів (n), необхідну для поразки кулеметника противника при стрільбі чергами (s) по 3 патрони на відстані 400 м, якщо вірогідність ураження цілей при одній черзі в 3 постріли Р1=0,54 (54%).",
    "answers": [
      {
        "text": "Середня витрата патронів: n ≈ 6 патронів (дві черги).",
        "isCorrect": true
      }
    ],
    "comment": "Середня кількість патронів (n) для ураження цілі однією чергою визначається за формулою: **n = s / P1**. Де: **s** — кількість патронів у черзі (3 патрони). **P1** — вірогідність ураження цілі однією чергою (0.54). **Розрахунок:** n = 3 / 0.54 ≈ 5.56 патронів. **Висновок:** Оскільки не можна витратити частину патрона, середній розрахунок округлюється до **6 патронів**. Тобто, для вирішення вогневого завдання стрілець повинен зробити **дві прицільних черги по три патрони**."
  },
  {
    "id": 108,
    "category": "Практичні завдання (Розрахунки та Алгоритми)",
    "difficulty": 1,
    "question": "Практичне завдання №8: Визначте дальність до цілі в нічний час по спалаху від пострілу, якщо його почули через 2 секунди?",
    "answers": [
      {
        "text": "Дальність дорівнює 700 метрів.",
        "isCorrect": true
      }
    ],
    "comment": "Визначення відстані по звуку базується на швидкості поширення звуку в повітрі. Швидкість звуку приймається приблизно 350 м/с. **Формула:** Відстань (м) = Швидкість звуку (м/с) * Час (с). **Розрахунок:** 350 м/с * 2 с = 700 метрів. **Примітка:** Світло від спалаху доходить миттєво, тому час між спалахом і звуком є чистим часом проходження звуку."
  },
  {
    "id": 109,
    "category": "Практичні завдання (Вогнева підготовка)",
    "difficulty": 3,
    "question": "Практичне завдання №9: Стрілець противника рухається на відстані 250 м, рухається по флангу зі швидкістю 3 м/с (10 км/год). Що необхідно зробити, щоб вразити ціль (АКМ/АК-74)? Боковий вітер відсутній.",
    "answers": [
      {
        "text": "Взяти упередження приблизно 2.5 фігури, цілячись під зріз цілі.",
        "isCorrect": true
      }
    ],
    "comment": "Для ураження рухомої цілі необхідно взяти поправку (упередження). На дальності 250 м і швидкості 3 м/с, табличне упередження для стрільця, що біжить, становить приблизно **2-3 фігури** (корпуси). 1. **Упередження:** Взяти упередження приблизно **2.5 фігури**. 2. **Приціл:** На відстані 250 м встановлюється приціл **«3»** (для АКМ). 3. **Точка прицілювання:** Прицілюватися слід **під зріз цілі** (в ноги або нижню частину корпуса), щоб вразити ціль в пояс/груди. Це компенсація вертикального падіння кулі."
  },
  {
    "id": 110,
    "category": "Практичні завдання (Вогнева підготовка)",
    "difficulty": 3,
    "question": "Практичне завдання №10: Кулеметник з РПК веде вогонь зверху-вниз на відстань 700 м під кутом приблизно 40 градусів. На яку позначку потрібно встановити приціл?",
    "answers": [
      {
        "text": "Приціл встановлюється на позначку «6».",
        "isCorrect": true
      }
    ],
    "comment": "При стрільбі **зверху-вниз** (з гори, високої будівлі), траєкторія кулі є більш **пологою** відносно цілі, ніж при стрільбі з горизонтальної поверхні. Для ураження цілі на відстані 700 м з РПК (приціл «7») необхідно: **Зменшити приціл** на один-два розподіли. Це компенсує балістичний ефект, коли куля фактично летить меншу відстань по горизонталі до цілі. Відповідно до інструкцій, на 700 м приціл зменшується до **«6»**."
  },
  {
    "id": 111,
    "category": "Практичні завдання (Вогнева підготовка)",
    "difficulty": 3,
    "question": "Практичне завдання №11: На відстані 400 м від стрілка знаходиться ростова фігура. Боковий вітер 12 м/с. Яке потрібно взяти упередження при стрільбі з АКМ?",
    "answers": [
      {
        "text": "Упередження 4.5 фігури в бік, звідки дме вітер.",
        "isCorrect": true
      }
    ],
    "comment": "Для компенсації бічного вітру необхідно взяти поправку (упередження) у бік вітру. 1. **Базове упередження:** Відповідно до таблиць, при швидкості вітру 4 м/с на відстані 400 м упередження для ростової фігури становить приблизно **1.5 фігури**. 2. **Розрахунок:** Вітер 12 м/с в 3 рази сильніший, ніж базовий (12 / 4 = 3). 3. **Кінцеве упередження:** 1.5 фігури * 3 = **4.5 фігури**. **Висновок:** Стрілець повинен цілитися на **4.5 фігури** у бік, **звідки дме вітер**. Наприклад, якщо вітер дме зліва, цілитися потрібно правіше цілі (на 4.5 фігури)."
  }
  ]
}
`; // ЗАМІНІТЬ ЦЕЙ БЛОК НА СВІЙ ДІЙСНИЙ JSON АБО ЗАЛИШТЕ ЛИШЕ КОМЕНТАР ДЛЯ МОЖЛИВОСТІ ЗАВАНТАЖЕННЯ ФАЙЛУ

(() => {
  // --- DOM ---
  const startBtn = document.getElementById('startBtn');
  const statsBtn = document.getElementById('statsBtn');
  const fileInput = document.getElementById('fileInput');
  const fileLabel = document.getElementById('fileLabel');
  const testConfiguration = document.getElementById('testConfiguration');
  const categoryList = document.getElementById('categoryList');
  const qCountInput = document.getElementById('qCount');
  const timeMinInput = document.getElementById('timeMin');
  const maxMistakesInput = document.getElementById('maxMistakes');
  const priorityToggle = document.getElementById('priorityToggle');
  const testTitle = document.getElementById('testTitle');
  const testInfoLine = document.getElementById('testInfoLine');
  const storageInfo = document.getElementById('storageInfo');
  const configNotice = document.getElementById('configNotice');
  const headerNotice = document.getElementById('headerNotice');
  const globalProgress = document.getElementById('globalProgress');
  const testScreen = document.getElementById('testScreen');
  const questionText = document.getElementById('questionText');
  const questionCategory = document.getElementById('questionCategory');
  const answersList = document.getElementById('answersList');
  const answerCommentEl = document.getElementById('answerComment');
  const qMeta = document.getElementById('qMeta');
  const timerEl = document.getElementById('timer');
  const mistakesEl = document.getElementById('mistakes');
  const scoreEl = document.getElementById('score');
  const endEarly = document.getElementById('endEarly');
  const backToSetupFromTest = document.getElementById('backToSetupFromTest');
  const resultScreen = document.getElementById('resultScreen');
  const finalPercent = document.getElementById('finalPercent');
  const finalCounts = document.getElementById('finalCounts');
  const categoryTableBody = document.querySelector('#categoryTable tbody');
  const heatmapArea = document.getElementById('heatmapArea');
  const historyChart = document.getElementById('historyChart');
  const badQuestionsTable = document.querySelector('#badQuestionsTable tbody');
  const retakeBtn = document.getElementById('retakeBtn');
  const toSetup = document.getElementById('toSetup');
  const resetStats = document.getElementById('resetStats');
  const lsCount = document.getElementById('lsCount');
  const exportStatsBtn = document.getElementById('exportStats');
  const setupCol = document.getElementById('setupCol');
  const setupSummary = document.getElementById('setupSummary');
  const resultsTitle = document.getElementById('resultsTitle');
  const nextQBtn = document.getElementById('nextQBtn');

  // --- Storage keys ---
  const LS_KEYS = {
    HISTORY: 'practice_test_history_v2',
    QSTATS: 'practice_test_qstats_v2',
    LASTSET: 'practice_test_lastset_v2'
  };
  
  // --- Screen states ---
  const SCREENS = { SETUP: 'setup', TESTING: 'testing', RESULTS: 'results' };
  let currentScreen = SCREENS.SETUP;


  // --- State ---
  let RAW = null; // full JSON
  let questions = []; // array of question objects used in current pool
  let categories = []; // list
  let selectedCategories = new Set();
  let pool = []; // questions chosen for the test
  let currentIndex = 0;
  let correctCount = 0;
  let mistakesCount = 0;
  let allowedMistakes = 0;
  let totalTimeSec = 0;
  let timerId = null;
  let timeLeft = 0;
  let questionStats = {}; // from storage
  let history = []; // runs history
  let priorityModeAvailable = false;
  let answerGiven = false; // to control "next" button visibility

  // --- Screen Management ---
  function showScreen(screen) {
    currentScreen = screen;
    const isSetup = screen === SCREENS.SETUP;
    const isTesting = screen === SCREENS.TESTING;
    const isResults = screen === SCREENS.RESULTS;
    
    // Manage main column children visibility
    setupSummary.style.display = isSetup ? 'flex' : 'none';
    configNotice.style.display = isSetup ? 'block' : 'none';
    testScreen.style.display = isTesting ? 'flex' : 'none';
    resultScreen.style.display = isResults ? 'block' : 'none';
    
    // Manage left column visibility
    setupCol.style.display = isSetup ? 'block' : 'none';
    
    // Manage buttons within results/stats
    retakeBtn.style.display = (isResults && pool.length > 0) ? 'block' : 'none';
    toSetup.style.display = isResults ? 'block' : 'none';
    
    // Set results title context
    resultsTitle.textContent = pool.length > 0 && pool[0].id ? 'Результат тесту' : 'Статистика';
    
    // Additional UI updates
    if (isSetup && RAW && questions.length > 0) {
      configNotice.textContent = 'Налаштуйте параметри та натисніть "Почати тест".';
      headerNotice.textContent = 'Працює офлайн. Тест завантажено.';
      startBtn.disabled = false;
      statsBtn.disabled = false;
      testConfiguration.style.display = 'block';
    } else if (isSetup) {
      configNotice.textContent = 'Будь ласка, завантажте файл тесту (JSON) або вбудуйте його в код, щоб продовжити.';
      headerNotice.textContent = 'Працює офлайн. Вбудуйте свій JSON-тест у код, або завантажте файл.';
      startBtn.disabled = true;
      statsBtn.disabled = true;
      testConfiguration.style.display = 'none';
    }
  }

  // --- Storage and Helpers ---
  function loadFromLS() {
    try {
      questionStats = JSON.parse(localStorage.getItem(LS_KEYS.QSTATS) || '{}');
    } catch(e){ questionStats = {}; }
    try {
      history = JSON.parse(localStorage.getItem(LS_KEYS.HISTORY) || '[]');
    } catch(e){ history = []; }
    const last = localStorage.getItem(LS_KEYS.LASTSET);
    if (last && categories.length > 0) { // Only load settings if there's a loaded test
      try {
        const ls = JSON.parse(last);
        qCountInput.value = ls.qCount || qCountInput.value;
        timeMinInput.value = ls.timeMin || timeMinInput.value;
        maxMistakesInput.value = ls.maxMistakes || maxMistakesInput.value;
        if (ls.selectedCategories) {
          selectedCategories = new Set(ls.selectedCategories.filter(c => categories.includes(c)));
        }
      } catch(e){}
    }
    priorityModeAvailable = history.length > 0;
    priorityToggle.disabled = !priorityModeAvailable;
    storageInfo.textContent = `Історія: ${history.length} проходжень`;
    lsCount.textContent = history.length;
  }

  function saveStats() {
    localStorage.setItem(LS_KEYS.QSTATS, JSON.stringify(questionStats));
    localStorage.setItem(LS_KEYS.HISTORY, JSON.stringify(history));
  }
  function saveLastSettings() {
    const s = {
      qCount: Number(qCountInput.value),
      timeMin: Number(timeMinInput.value),
      maxMistakes: Number(maxMistakesInput.value),
      selectedCategories: Array.from(selectedCategories)
    };
    localStorage.setItem(LS_KEYS.LASTSET, JSON.stringify(s));
  }

  function niceTime(sec) {
    const m = Math.floor(sec/60).toString().padStart(2,'0');
    const s = (sec%60).toString().padStart(2,'0');
    return `${m}:${s}`;
  }
  
  function getCorrectText(q) {
    const correct = (q.answers || []).find(a => !!a.isCorrect);
    return correct ? (correct.text || '').trim() : '';
  }

  function escapeHtml(s){ return (s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/"/g,'&quot;'); }

  function shuffle(arr){ const a = arr.slice(); for (let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]] } return a; }

  // JSON parsing and validation
  function parseJSON(text) {
    try {
        // Clean comments only for the embedded block to determine if it's empty
        let cleanText = text.replace(/\/\*[\s\S]*?\*\/|(?<=[^:])\/\/.*/g, '').trim();
        if (cleanText.length === 0) return null;

        const obj = JSON.parse(cleanText);
        if (!obj.questions || !Array.isArray(obj.questions) || obj.questions.length === 0) throw new Error('JSON має порожній або невірний масив "questions"');
        if (!obj.testInfo) obj.testInfo = {title:'Тест'};
        return obj;
    } catch(e) {
      alert('Помилка парсингу JSON: ' + e.message);
      return null;
    }
  }
  
  // Update state with loaded data
  function updateTestData(data, source) {
    RAW = data;
    questions = data.questions.slice();
    categories = Array.from(new Set(questions.map(q=>q.category))).filter(Boolean);
    testTitle.textContent = data.testInfo && data.testInfo.title ? data.testInfo.title : 'Тест';
    testInfoLine.textContent = `Питань: ${questions.length} • Категорій: ${categories.length} (Джерело: ${source})`;
    
    loadFromLS(); // Reload last settings now that categories are available
    renderCategories();
    showScreen(SCREENS.SETUP);
  }

  // Load embedded or prepare for file
  function loadEmbeddedJSON() {
    // 1. Check embedded JSON
    const cleanText = RAW_JSON_TEXT.replace(/\/\*[\s\S]*?\*\/|(?<=[^:])\/\/.*/g, '').trim();
    if (cleanText.length > 0) {
      const obj = parseJSON(RAW_JSON_TEXT);
      if (obj) {
        updateTestData(obj, 'Вбудовано');
        fileInput.style.display = 'none';
        fileLabel.style.display = 'none';
        return true;
      }
    }
    
    // 2. If no valid embedded JSON, prepare for file upload
    fileInput.style.display = 'block';
    fileLabel.style.display = 'block';
    testTitle.textContent = '(Тест не завантажено)';
    testInfoLine.textContent = 'Виберіть файл JSON для завантаження.';
    showScreen(SCREENS.SETUP);
    return false;
  }
  
  // Handle file input change
  fileInput.addEventListener('change', (e) => {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = function(event) {
      const data = parseJSON(event.target.result);
      if (data) {
        updateTestData(data, `Файл: ${file.name}`);
        fileInput.style.display = 'none';
        fileLabel.style.display = 'none';
      } else {
        // Reset file input if parsing failed
        e.target.value = ''; 
        configNotice.textContent = 'Помилка завантаження файлу. Перевірте, чи є файл коректним JSON.';
      }
    };
    reader.readAsText(file);
  });


  // render categories
  function renderCategories() {
    categoryList.innerHTML = '';
    
    // Ensure all categories are selected by default if no previous selection or none were saved
    if (selectedCategories.size === 0 && categories.length > 0) {
        categories.forEach(c => selectedCategories.add(c));
        saveLastSettings();
    }
    
    categories.forEach(cat => {
      const count = questions.filter(q => q.category === cat).length;
      const div = document.createElement('div');
      div.className = 'cat-item';
      const isChecked = selectedCategories.has(cat);
      
      div.innerHTML = `
        <div class="cat-left">
          <div style="display:flex;gap:8px;align-items:center">
            <input type="checkbox" data-cat="${escapeHtml(cat)}" ${isChecked ? 'checked' : ''} />
            <div>
              <div style="font-weight:600">${escapeHtml(cat)}</div>
              <div class="muted tiny">${count} питань</div>
            </div>
          </div>
        </div>
        <div class="counter">${count}</div>
      `;
      categoryList.appendChild(div);
      const cb = div.querySelector('input[type=checkbox]');
      cb.addEventListener('change', (e) => {
        if (e.target.checked) selectedCategories.add(cat);
        else selectedCategories.delete(cat);
        saveLastSettings(); // Save categories on change
      });
    });
  }

  // Build pool according to selected categories and algorithm
  function buildPool() {
    if (selectedCategories.size === 0) categories.forEach(c => selectedCategories.add(c));
    const poolAll = questions.filter(q => selectedCategories.has(q.category));
    const qCount = Math.max(1, Math.min(poolAll.length, Number(qCountInput.value) || 10));
    const priority = priorityToggle.checked && priorityModeAvailable;
    
    if (!priority) {
      return shuffle(poolAll).slice(0, qCount);
    } else {
      const weighted = [];
      poolAll.forEach(q => {
        const stat = questionStats[q.id] || {attempts:0, errors:0};
        const rate = stat.attempts>0 ? (stat.errors / stat.attempts) : 0;
        const weight = 1 + Math.round(rate * 4);
        for (let i=0;i<weight;i++) weighted.push(q);
      });
      if (weighted.length < 1) return shuffle(poolAll).slice(0,qCount);
      const chosen = new Set();
      while (chosen.size < qCount && weighted.length>0) {
        const pick = weighted[Math.floor(Math.random()*weighted.length)];
        chosen.add(pick);
      }
      const rem = poolAll.filter(q => !chosen.has(q));
      shuffle(rem).slice(0, Math.max(0, qCount - chosen.size)).forEach(q => chosen.add(q));
      return Array.from(chosen);
    }
  }

  // Start test
  function startTest() {
    if (!RAW) { alert('Тест не завантажений. Завантажте файл або вбудуйте JSON.'); return; }
    saveLastSettings();
    const poolAll = questions.filter(q => selectedCategories.has(q.category));
    if (poolAll.length === 0) { alert('Виберіть хоча б одну категорію'); return; }

    pool = buildPool();
    if (pool.length === 0) { alert('Не вдалося сформувати пул питань для тесту. Змініть налаштування.'); return; }
    
    allowedMistakes = Number(maxMistakesInput.value) || 0;
    totalTimeSec = (Number(timeMinInput.value) || 30) * 60;
    timeLeft = totalTimeSec;
    currentIndex = 0;
    correctCount = 0;
    mistakesCount = 0;
    answerGiven = false;
    
    showScreen(SCREENS.TESTING);
    updateMeta();
    showQuestion();
    startTimer();
    updateProgress();
  }

  // Update meta row
  function updateMeta(){
    qMeta.textContent = `Питання ${currentIndex+1}/${pool.length}`;
    timerEl.textContent = niceTime(timeLeft);
    mistakesEl.textContent = `Помилок: ${mistakesCount}/${allowedMistakes}`;
    scoreEl.textContent = `Правильно: ${correctCount}`;
  }

  // Timer
  function startTimer() {
    if (timerId) clearInterval(timerId);
    timerId = setInterval(()=>{
      timeLeft--;
      timerEl.textContent = niceTime(timeLeft);
      if (timeLeft <= 0) {
        clearInterval(timerId);
        finishTest('time');
      }
    },1000);
  }
  function stopTimer(){ if (timerId) clearInterval(timerId); timerId = null; }

  // Show question
  function showQuestion(){
    const q = pool[currentIndex];
    if (!q) return finishTest('done');
    questionText.textContent = q.question;
    questionCategory.textContent = q.category + (q.difficulty ? ` • складність ${q.difficulty}` : '');
    answersList.innerHTML = '';
    answerCommentEl.style.display = 'none';
    nextQBtn.style.display = 'none'; // hide next button initially
    answerGiven = false;

    const answers = shuffle(q.answers.map((a,i)=>({i, text:a.text, isCorrect: !!a.isCorrect})));
    answers.forEach(a => {
      const div = document.createElement('div');
      div.className = 'answer';
      div.tabIndex = 0;
      div.innerHTML = `<div style="font-size:14px">${escapeHtml(a.text)}</div>`;
      div.addEventListener('click', () => handleAnswer(div, a, q));
      answersList.appendChild(div);
    });
    updateMeta();
  }

  // Handle answer
  function handleAnswer(div, ans, question){
    if (answerGiven) return;
    answerGiven = true;
    
    // disable further clicks
    Array.from(answersList.children).forEach(c => c.style.pointerEvents = 'none');
    
    // Mark correct answer
    const correctText = getCorrectText(question);
    for (const node of answersList.children) {
      if (node.textContent.trim() === correctText) {
        node.classList.add('correct');
      }
    }

    if (ans.isCorrect) {
      div.classList.add('correct');
      correctCount++;
      recordQuestionStat(question.id, false);
      // correct answer: move to next immediately (as per original logic for quick flow)
      currentIndex++;
      updateMeta();
      updateProgress();
      // Show comment if available
      if (question.comment) {
        answerCommentEl.innerHTML = `<div style="font-weight:600;margin-bottom:4px;color:var(--accent)">Пояснення:</div>${escapeHtml(question.comment)}`;
        answerCommentEl.style.display = 'block';
      } else {
        answerCommentEl.style.display = 'none';
      }
      
      // Show next button for manual transition
      nextQBtn.style.display = 'block';
      
      // If exceeded mistakes -> force finish on next click
      if (mistakesCount > allowedMistakes) {
         nextQBtn.textContent = 'Завершити тест →';
      } else {
         nextQBtn.textContent = 'Далі →';
      }
      
    } else {
      div.classList.add('wrong-selected');
      mistakesCount++;
      recordQuestionStat(question.id, true);
      updateMeta();
      updateProgress();
      
      // Show comment if available
      if (question.comment) {
        answerCommentEl.innerHTML = `<div style="font-weight:600;margin-bottom:4px;color:var(--accent)">Пояснення:</div>${escapeHtml(question.comment)}`;
        answerCommentEl.style.display = 'block';
      } else {
        answerCommentEl.style.display = 'none';
      }
      
      // Show next button for manual transition
      nextQBtn.style.display = 'block';
      
      // If exceeded mistakes -> force finish on next click
      if (mistakesCount > allowedMistakes) {
         nextQBtn.textContent = 'Завершити тест →';
      } else {
         nextQBtn.textContent = 'Далі →';
      }
    }
  }

  function handleNextQuestion() {
      if (mistakesCount > allowedMistakes) return finishTest('mistakes');
      currentIndex++;
      if (currentIndex >= pool.length) return finishTest('done');
      showQuestion();
  }
  
  function recordQuestionStat(qid, wasError) {
    const now = Date.now();
    if (!questionStats[qid]) questionStats[qid] = {attempts:0, errors:0, lastSeen:now};
    questionStats[qid].attempts += 1;
    if (wasError) questionStats[qid].errors += 1;
    questionStats[qid].lastSeen = now;
    localStorage.setItem(LS_KEYS.QSTATS, JSON.stringify(questionStats));
  }

  // update progress bar
  // --- ЗАМІНІТЬ ВАШУ ІСНУЮЧУ ФУНКЦІЮ updateProgress ---

function updateProgress() {
    // 1. Кількість доступних питань: беремо довжину поточного, відфільтрованого масиву
    const totalQuestions = questions.length; 
    
    // 2. Кількість відповідей: беремо довжину масиву history
    const answered = history.length;
    
    // 3. Кількість правильних відповідей
    const correct = history.filter(item => item.correct).length;
    
    // 4. Відсоток успішності: рахуємо від загальної кількості ВІДПОВІДЕЙ
    const successRate = answered > 0 ? Math.round((correct / answered) * 100) : 0;

    // 5. Оновлення елементів UI
    const progressText = document.getElementById('progressText');
    const progressLine = document.getElementById('progressLine');
    const statsPassed = document.getElementById('statsPassed');
    const statsTotal = document.getElementById('statsTotal');
    const statsPercent = document.getElementById('statsPercent');

    // Оновлюємо текст у статистиці
    statsPassed.textContent = correct; 
    statsTotal.textContent = answered; // Тут показуємо скільки разів відповіли, а не загальну кількість

    // Оновлюємо індикатор прогресу
    const progressValue = totalQuestions > 0 ? Math.round((answered / totalQuestions) * 100) : 0;
    progressText.textContent = `${answered} з ${totalQuestions} (${progressValue}%)`;
    progressLine.style.width = `${progressValue}%`;
    
    // Якщо у вас є окремий елемент для відсотка успішності, оновлюємо його
    if (statsPercent) {
        statsPercent.textContent = `${successRate}%`;
    }
}

  // finish test and show results
  function finishTest(reason) {
    stopTimer();
    const total = pool.length;
    const correct = correctCount;
    const wrong = mistakesCount;
    const percent = total ? Math.round((correct / total) * 100) : 0;

    const run = {
      date: new Date().toISOString(),
      percent, correct, total, errors: wrong,
      settings: { qCount: Number(qCountInput.value), timeMin: Number(timeMinInput.value), maxMistakes: Number(maxMistakesInput.value), selectedCategories: Array.from(selectedCategories), priority: priorityToggle.checked },
      reason
    };
    
    // Only save run to history if it was a real test
    if (reason !== 'stats_only') {
      history.unshift(run);
      if (history.length > 200) history = history.slice(0,200);
      saveStats();
      saveLastSettings();
    }


    showScreen(SCREENS.RESULTS);
    finalPercent.textContent = percent + '%';
    finalCounts.innerHTML = `Правильно: ${correct} / ${total} • Помилок: ${wrong}`;
    document.getElementById('forecast').textContent = computeForecast(percent).text;
    renderCategoryStats();
    renderHeatmap();
    drawHistoryChart();
    populateBadQuestions();
    loadFromLS(); // Update storage info
  }

  // Simple forecast algorithm
  function computeForecast(latestPercent) {
    const lastN = history.slice(0, 10);
    const avg = lastN.length ? Math.round(lastN.reduce((s,r)=>s+r.percent,0)/lastN.length) : latestPercent;
    let trend = 0;
    if (lastN.length >= 2) trend = lastN[0].percent - lastN[lastN.length-1].percent;
    const readiness = avg >= 85 ? 'Готовий' : (avg >= 70 ? 'Майже готовий' : 'Потрібна практика');
    const advice = trend > 0 ? 'Покращення присутнє' : (trend < 0 ? 'Ризик погіршення — потренуйся більше' : 'Стабільно');
    return { text: `${readiness} (середній: ${avg}%). ${advice}` };
  }

  // Render per-category table
  function renderCategoryStats() {
    const catMap = {}; 
    categories.forEach(c => catMap[c] = { questions: questions.filter(q => q.category===c).length, correctPct: null, attempts:0, errors:0 });

    for (const q of questions) {
      const s = questionStats[q.id];
      if (!s) continue;
      const c = q.category;
      if (catMap[c]) {
          catMap[c].attempts += s.attempts || 0;
          catMap[c].errors += s.errors || 0;
      }
    }
    
    Object.keys(catMap).forEach(c => {
      const it = catMap[c];
      if (it.attempts > 0) {
        const correct = it.attempts - it.errors;
        it.correctPct = Math.round((correct / it.attempts) * 100);
      } else {
        it.correctPct = null; 
      }
    });

    categoryTableBody.innerHTML = '';
    Object.keys(catMap).forEach(c => {
      const it = catMap[c];
      const tr = document.createElement('tr');
      const pctText = it.correctPct === null ? '—' : (it.correctPct + '%');
      tr.innerHTML = `<td style="font-weight:600; color:white;">${escapeHtml(c)}</td><td>${it.questions}</td><td>${pctText}</td>`;
      categoryTableBody.appendChild(tr);
    });
  }

  // Heatmap
  function renderHeatmap() {
    heatmapArea.innerHTML = '';
    const catMap = [];
    categories.forEach(c => {
      const qs = questions.filter(q => q.category === c);
      let attempts = 0, errors = 0;
      qs.forEach(q => {
        const s = questionStats[q.id] || {attempts:0, errors:0};
        attempts += s.attempts;
        errors += s.errors;
      });
      const pct = attempts ? Math.round(((attempts - errors) / attempts) * 100) : null;
      catMap.push({category:c, pct, attempts});
    });
    
    catMap.sort((a,b)=> {
      if (a.pct === null && b.pct === null) return 0;
      if (a.pct === null) return 1;
      if (b.pct === null) return -1;
      return a.pct - b.pct;
    });

    catMap.forEach(item => {
      const card = document.createElement('div');
      card.className = 'heat-card';
      let bg = 'rgba(255,255,255,0.02)';
      let fg = 'var(--muted)';
      if (item.pct === null) {
        bg = 'linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.01))';
        fg = 'var(--muted)';
      } else {
        const p = item.pct / 100;
        const r = Math.round(239 + (16 - 239) * p); 
        const g = Math.round(68 + (185 - 68) * p);
        const b = Math.round(68 + (129 - 68) * p);
        bg = `linear-gradient(180deg, rgba(${r},${g},${b},0.12), rgba(${r},${g},${b},0.06))`;
        fg = p > 0.6 ? 'white' : 'var(--muted)';
      }
      card.style.background = bg;
      card.innerHTML = `<div style="font-weight:700">${escapeHtml(item.category)}</div>
                        <div class="muted tiny" style="margin-top:6px">${item.pct===null ? 'немає даних' : item.pct + '%'} ${item.attempts? ('• ' + item.attempts + ' спроб') : ''}</div>`;
      heatmapArea.appendChild(card);
    });
  }

  // Draw history chart
  function drawHistoryChart() {
    try {
      const ctx = historyChart.getContext('2d');
      const w = historyChart.width;
      const h = historyChart.height;
      ctx.clearRect(0,0,w,h);
      const data = history.slice(0, 30).reverse(); 
      if (data.length === 0) {
        ctx.fillStyle = 'rgba(255,255,255,0.03)';
        ctx.fillRect(0,0,w,h);
        ctx.fillStyle = 'var(--muted)';
        ctx.font = '13px sans-serif';
        ctx.fillText('Немає історії проходжень', 10, 20);
        return;
      }
      const padding = 30;
      const maxP = 100;
      const minP = 0;
      const stepX = (w - padding*2) / (data.length - 1 || 1);
      const pts = data.map((d,i) => {
        const x = padding + i * stepX;
        const y = padding + (h - padding*2) * (1 - (d.percent - minP) / (maxP - minP));
        return {x,y, v:d.percent, date:d.date};
      });
      ctx.fillStyle = 'rgba(255,255,255,0.02)';
      ctx.fillRect(0,0,w,h);
      ctx.strokeStyle = 'rgba(255,255,255,0.03)';
      ctx.beginPath();
      for (let g=0;g<=4;g++){
        const yy = padding + g * ((h - padding*2)/4);
        ctx.moveTo(padding, yy);
        ctx.lineTo(w-padding, yy);
      }
      ctx.stroke();
      ctx.beginPath();
      ctx.moveTo(pts[0].x, pts[0].y);
      for (let i=1;i<pts.length;i++) ctx.lineTo(pts[i].x, pts[i].y);
      ctx.strokeStyle = 'rgba(96,165,250,0.95)';
      ctx.lineWidth = 2;
      ctx.stroke();
      ctx.lineTo(pts[pts.length-1].x, h - padding);
      ctx.lineTo(pts[0].x, h - padding);
      ctx.closePath();
      ctx.fillStyle = 'rgba(96,165,250,0.06)';
      ctx.fill();
      ctx.fillStyle = 'white';
      for (const p of pts) {
        ctx.beginPath();
        ctx.arc(p.x, p.y, 3, 0, Math.PI*2);
        ctx.fill();
      }
      ctx.fillStyle = 'var(--muted)';
      ctx.font = '12px sans-serif';
      ctx.fillText('Останні проходження (ліворуч — старіше)', padding, 14);
      ctx.fillText(`Останній: ${data[data.length-1].percent}%`, w - padding - 120, 14);
    } catch(e) { console.error('chart err', e); }
  }

  // Populate bad questions table
  function populateBadQuestions() {
    const arr = [];
    for (const q of questions) {
      const s = questionStats[q.id] || {attempts:0, errors:0};
      if (s.errors > 0) arr.push({id: q.id, question: q.question, errors: s.errors, attempts: s.attempts});
    }
    arr.sort((a,b)=>b.errors - a.errors);
    badQuestionsTable.innerHTML = '';
    arr.slice(0,50).forEach((it, idx) => {
      const tr = document.createElement('tr');
      tr.innerHTML = `<td>${idx+1}</td><td style="font-weight:600; color:white;">${escapeHtml(it.question)}</td><td>${it.errors} (${it.attempts||0})</td>`;
      badQuestionsTable.appendChild(tr);
    });
  }

  // Export stats as JSON
  function exportStats() {
    const payload = {
      history,
      questionStats,
      lastSettings: JSON.parse(localStorage.getItem(LS_KEYS.LASTSET) || '{}')
    };
    const blob = new Blob([JSON.stringify(payload, null, 2)], {type:'application/json'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'practice_stats.json';
    document.body.appendChild(a);
    a.click();
    a.remove();
    URL.revokeObjectURL(url);
  }

  // Reset stats
  function clearStatsConfirm() {
    if (!confirm('Очистити всю статистику (історію та помилки)?')) return;
    questionStats = {};
    history = [];
    localStorage.removeItem(LS_KEYS.QSTATS);
    localStorage.removeItem(LS_KEYS.HISTORY);
    loadFromLS();
    alert('Статистика очищена');
    if (currentScreen === SCREENS.RESULTS) {
      // Re-render stats screen components after clear
      renderCategoryStats();
      renderHeatmap();
      drawHistoryChart();
      populateBadQuestions();
    }
  }

  // --- Event Listeners ---
  startBtn.addEventListener('click', startTest);
  nextQBtn.addEventListener('click', handleNextQuestion);
  backToSetupFromTest.addEventListener('click', () => {
    stopTimer();
    showScreen(SCREENS.SETUP);
  });
  retakeBtn.addEventListener('click', () => {
    pool = buildPool(); // rebuild pool with same settings
    currentIndex = 0;
    correctCount = 0;
    mistakesCount = 0;
    allowedMistakes = Number(maxMistakesInput.value) || 0;
    totalTimeSec = (Number(timeMinInput.value) || 30) * 60;
    timeLeft = totalTimeSec;
    answerGiven = false;
    
    showScreen(SCREENS.TESTING);
    updateMeta();
    showQuestion();
    startTimer();
  });
  toSetup.addEventListener('click', () => {
    showScreen(SCREENS.SETUP);
  });
  statsBtn.addEventListener('click', () => {
    if (!RAW) { alert('Спочатку завантажте тест.'); return; }
    if (history.length === 0 && Object.keys(questionStats).length === 0) { alert('Немає збереженої статистики для відображення.'); return; }
    
    // Fake run state to render results/stats UI
    pool = []; 
    finishTest('stats_only'); 
    resultsTitle.textContent = 'Статистика';
    retakeBtn.style.display = 'none'; // hide retake on stats-only
    finalCounts.innerHTML = 'Детальна статистика на базі всіх проходжень.';
  });
  endEarly.addEventListener('click', () => {
    if (confirm('Завершити тест достроково?')) finishTest('early');
  });
  resetStats.addEventListener('click', clearStatsConfirm);
  exportStatsBtn.addEventListener('click', exportStats);

  // --- Initialization ---
  (function init() {
    // Load last settings and history first to update UI
    loadFromLS(); 
    // Attempt to load embedded JSON and set up UI based on success
    loadEmbeddedJSON();
    updateProgress();
  })();

})(); // end of main IIFE
</script>
</body>
</html>
