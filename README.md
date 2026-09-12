<!DOCTYPE html>
<html lang="ar" dir="rtl" id="htmlRoot">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Study Planner</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,500;0,600;1,500&family=Tajawal:wght@400;500;700&family=Caveat:wght@500;700&display=swap" rel="stylesheet">
<style>
  :root{ --cream:#FBF7F0; --panel:#FFFFFF; --ink:#2C2A4A; --ink-soft:#6E6A8A; --line:#E7E1D6; --gold:#C9A24B; --danger:#C9605A; --shadow:0 6px 20px rgba(44,42,74,0.06); }
  *{box-sizing:border-box;}
  body{ margin:0; font-family:'Tajawal', sans-serif; background:var(--cream); color:var(--ink); min-height:100vh; }
  .layout{ display:flex; min-height:100vh; }
  .sidebar{ width:235px; background:var(--panel); border-inline-end:1px solid var(--line); padding:24px 16px; display:flex; flex-direction:column; gap:16px; flex-shrink:0; }
  .brand{ display:flex; flex-direction:column; gap:2px; padding-bottom:14px; border-bottom:1px solid var(--line); }
  .brand .script{ font-family:'Cormorant Garamond', serif; font-style:italic; font-size:0.9rem; color:var(--accent); }
  .brand .title{ font-family:'Cormorant Garamond', serif; font-size:1.5rem; font-weight:600; }
  .lang-select{ width:100%; padding:8px 10px; border-radius:8px; border:1px solid var(--line); background:var(--cream); font-family:inherit; font-size:0.82rem; color:var(--ink); }
  .xp-box{ background:var(--accent-light); border-radius:12px; padding:12px 14px; }
  .xp-box .level{ font-weight:700; font-size:0.82rem; color:var(--accent); margin-bottom:6px; }
  .xp-bar-bg{ height:7px; background:var(--panel); border-radius:5px; overflow:hidden; }
  .xp-bar-fill{ height:100%; background:var(--accent); width:0%; transition:width .3s ease; }
  .xp-sub{ font-size:0.66rem; color:var(--ink-soft); margin-top:5px; }
  .streak-line{ display:flex; align-items:center; gap:6px; font-size:0.76rem; color:var(--ink-soft); margin-top:8px; }
  .streak-line b{ color:var(--ink); }
  .nav{ display:flex; flex-direction:column; gap:4px; }
  .nav button{ display:flex; align-items:center; gap:10px; text-align:start; background:none; border:none; padding:10px 12px; border-radius:10px; font-family:'Tajawal', sans-serif; font-size:0.85rem; color:var(--ink-soft); cursor:pointer; }
  .nav button .dot{ width:7px; height:7px; border-radius:50%; background:var(--line); flex-shrink:0; }
  .nav button:hover{ background:var(--cream); }
  .nav button.active{ background:var(--accent-light); color:var(--accent); font-weight:700; }
  .nav button.active .dot{ background:var(--accent); }
  .theme-picker{ margin-top:auto; display:flex; flex-direction:column; gap:8px; }
  .theme-picker .label{ font-size:0.7rem; color:var(--ink-soft); }
  .swatches{ display:flex; gap:8px; }
  .swatch{ width:22px; height:22px; border-radius:50%; cursor:pointer; border:2px solid transparent; }
  .swatch.selected{ border-color: var(--ink); }
  .main{ flex:1; padding:28px 34px 60px; max-width:1180px; }
  header.page-head{ display:flex; justify-content:space-between; align-items:flex-end; flex-wrap:wrap; gap:14px; margin-bottom:22px; }
  header.page-head h1{ font-family:'Cormorant Garamond', serif; font-size:2rem; font-weight:600; margin:0; }
  header.page-head .date-badge{ background:var(--panel); border:1px solid var(--line); border-radius:20px; padding:7px 18px; font-size:0.8rem; color:var(--ink-soft); box-shadow:var(--shadow); }
  .cards-row{ display:grid; grid-template-columns:repeat(4,1fr); gap:14px; margin-bottom:24px; }
  @media (max-width:900px){ .cards-row{ grid-template-columns:repeat(2,1fr); } }
  .mini-card{ background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:16px; box-shadow:var(--shadow); }
  .mini-card .num{ font-family:'Cormorant Garamond', serif; font-size:1.7rem; font-weight:600; color:var(--accent); }
  .mini-card .lbl{ font-size:0.72rem; color:var(--ink-soft); margin-top:2px; }
  .add-task{ background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:18px; margin-bottom:24px; display:grid; grid-template-columns: 1.5fr 1fr 1fr 1fr 0.9fr auto; gap:12px; align-items:end; box-shadow:var(--shadow); }
  .field{ display:flex; flex-direction:column; gap:6px; }
  .field label{ font-size:0.72rem; color:var(--ink-soft); }
  .field input, .field select{ padding:9px 11px; border:1px solid var(--line); border-radius:8px; font-size:0.85rem; background:var(--cream); color:var(--ink); font-family:inherit; }
  .field input:focus, .field select:focus{ outline:2px solid var(--accent); outline-offset:1px; }
  .btn-add{ background:var(--ink); color:var(--panel); border:none; padding:10px 20px; border-radius:8px; cursor:pointer; font-size:0.85rem; height:40px; font-family:inherit; }
  .btn-add:hover{ background:var(--accent); }
  .board{ display:grid; grid-template-columns: repeat(7, 1fr); gap:12px; }
  @media (max-width:900px){ .board{ grid-template-columns: repeat(2,1fr); } .add-task{ grid-template-columns:1fr 1fr; } }
  .day-col{ background:var(--panel); border:1px solid var(--line); border-radius:12px; min-height:180px; display:flex; flex-direction:column; box-shadow:var(--shadow); }
  .day-head{ padding:10px 12px; border-bottom:1px solid var(--line); font-family:'Cormorant Garamond', serif; font-weight:600; font-size:0.98rem; text-align:center; color:var(--accent); }
  .day-body{ padding:10px; display:flex; flex-direction:column; gap:8px; flex:1; }
  .task-card{ background:var(--cream); border:1px solid var(--line); border-radius:9px; padding:9px 11px; font-size:0.8rem; border-inline-start:4px solid var(--accent); }
  .task-card.priority-high{ border-inline-start-color:var(--danger); }
  .task-card.priority-medium{ border-inline-start-color:var(--gold); }
  .task-card.done{ opacity:0.5; }
  .task-card.done .task-title{ text-decoration:line-through; }
  .task-title{ font-weight:700; margin-bottom:3px; word-break:break-word; }
  .task-subject{ font-size:0.68rem; color:var(--ink-soft); }
  .task-actions{ display:flex; gap:10px; margin-top:6px; }
  .task-actions button{ border:none; background:none; cursor:pointer; font-size:0.7rem; color:var(--accent); padding:0; font-family:inherit; }
  .task-actions button.del{ color:var(--danger); }
  .empty-note{ color:var(--ink-soft); font-size:0.74rem; text-align:center; margin-top:14px; }
  .calendar-nav{ display:flex; align-items:center; justify-content:center; gap:20px; margin-bottom:16px; }
  .calendar-nav button{ background:var(--panel); border:1px solid var(--line); border-radius:8px; width:32px; height:32px; cursor:pointer; font-size:1rem; }
  .calendar-nav .month-label{ font-family:'Cormorant Garamond', serif; font-size:1.25rem; font-weight:600; min-width:170px; text-align:center; }
  .calendar-grid{ display:grid; grid-template-columns: repeat(7, 1fr); gap:8px; }
  .cal-dow{ text-align:center; font-size:0.72rem; color:var(--ink-soft); font-weight:700; padding-bottom:4px; }
  .cal-cell{ background:var(--panel); border:1px solid var(--line); border-radius:10px; min-height:82px; padding:6px 7px; font-size:0.68rem; display:flex; flex-direction:column; gap:4px; box-shadow:var(--shadow); }
  .cal-cell.empty{ background:transparent; border:none; box-shadow:none; }
  .cal-cell .num{ font-weight:700; font-size:0.76rem; }
  .cal-cell .mini-task{ background:var(--accent-light); color:var(--accent); border-radius:5px; padding:1px 5px; font-size:0.6rem; overflow:hidden; text-overflow:ellipsis; white-space:nowrap; }
  .subjects-grid{ display:grid; grid-template-columns: repeat(auto-fill, minmax(210px, 1fr)); gap:14px; }
  .subject-card{ background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:16px; box-shadow:var(--shadow); }
  .subject-card .sname{ font-family:'Cormorant Garamond', serif; font-size:1.2rem; font-weight:600; margin-bottom:6px; color:var(--accent); }
  .subject-card .scount{ font-size:0.74rem; color:var(--ink-soft); }
  .subject-card .stime{ font-size:0.7rem; color:var(--ink-soft); margin-top:2px; }
  .focus-wrap{ display:flex; flex-direction:column; align-items:center; gap:18px; background:var(--panel); border:1px solid var(--line); border-radius:18px; padding:36px; box-shadow:var(--shadow); }
  .timer-display{ font-family:'Cormorant Garamond', serif; font-size:4rem; font-weight:600; color:var(--accent); }
  .focus-select{ display:flex; gap:10px; align-items:center; flex-wrap:wrap; justify-content:center; }
  .focus-select input, .focus-select select{ padding:8px 12px; border:1px solid var(--line); border-radius:8px; background:var(--cream); font-family:inherit; font-size:0.85rem; }
  .preset-btns{ display:flex; gap:6px; }
  .preset-btns button{ padding:6px 10px; border-radius:7px; border:1px solid var(--line); background:var(--cream); cursor:pointer; font-family:inherit; font-size:0.75rem; }
  .timer-controls{ display:flex; gap:12px; }
  .timer-controls button{ padding:10px 22px; border-radius:9px; border:none; cursor:pointer; font-family:inherit; font-size:0.88rem; }
  .btn-start{ background:var(--accent); color:#fff; }
  .btn-reset{ background:var(--cream); color:var(--ink); border:1px solid var(--line) !important; }
  .focus-log{ width:100%; max-width:460px; }
  .focus-log h3{ font-family:'Cormorant Garamond', serif; font-size:1.05rem; margin-bottom:8px; }
  .log-row{ display:flex; justify-content:space-between; font-size:0.78rem; padding:6px 0; border-bottom:1px solid var(--line); color:var(--ink-soft); }
  .exam-form{ background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:18px; margin-bottom:22px; display:grid; grid-template-columns: 1.3fr 1fr 1fr auto; gap:12px; align-items:end; box-shadow:var(--shadow); }
  .exam-list{ display:flex; flex-direction:column; gap:12px; }
  .exam-card{ background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:16px 18px; box-shadow:var(--shadow); }
  .exam-card .etitle{ font-family:'Cormorant Garamond', serif; font-size:1.25rem; font-weight:600; }
  .exam-card .ecount{ font-size:0.8rem; color:var(--accent); font-weight:700; }
  .exam-card .emeta{ font-size:0.76rem; color:var(--ink-soft); margin-top:4px; }
  .exam-card button.del{ background:none; border:none; color:var(--danger); cursor:pointer; font-size:0.74rem; margin-top:8px; font-family:inherit; }
  .heatmap-wrap{ background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:20px; box-shadow:var(--shadow); overflow-x:auto; }
  .heatmap-grid{ display:grid; grid-auto-flow:column; grid-template-rows: repeat(7, 14px); gap:3px; direction:ltr; }
  .heat-cell{ width:14px; height:14px; border-radius:3px; background:var(--cream); }
  .heat-legend{ display:flex; gap:6px; align-items:center; margin-top:12px; font-size:0.68rem; color:var(--ink-soft); }
  .heat-legend .heat-cell{ width:11px; height:11px; }
  .notes-layout{ display:flex; gap:20px; align-items:flex-start; flex-wrap:wrap; }
  .notes-toolbar{ background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:16px; display:flex; flex-direction:column; gap:12px; width:210px; flex-shrink:0; box-shadow:var(--shadow); }
  .notes-toolbar h4{ font-family:'Cormorant Garamond', serif; margin:0 0 4px; font-size:1rem; }
  .tpl-options{ display:flex; flex-direction:column; gap:6px; }
  .tpl-options button{ text-align:start; padding:8px 10px; border-radius:8px; border:1px solid var(--line); background:var(--cream); cursor:pointer; font-family:inherit; font-size:0.78rem; color:var(--ink-soft); }
  .tpl-options button.active{ background:var(--accent-light); color:var(--accent); border-color:var(--accent); font-weight:700; }
  .pen-colors{ display:flex; gap:8px; flex-wrap:wrap; }
  .pen-swatch{ width:22px; height:22px; border-radius:50%; cursor:pointer; border:2px solid transparent; }
  .pen-swatch.selected{ border-color:var(--ink); }
  .pen-size{ display:flex; align-items:center; gap:8px; }
  .pen-size input, .pen-size select{ flex:1; }
  .notes-toolbar .tool-btn{ padding:8px 10px; border-radius:8px; border:1px solid var(--line); background:var(--cream); cursor:pointer; font-family:inherit; font-size:0.78rem; color:var(--ink); }
  .notes-toolbar .tool-btn.primary{ background:var(--ink); color:#fff; border-color:var(--ink); }
  .notes-title-input{ padding:8px 10px; border:1px solid var(--line); border-radius:8px; font-family:'Caveat', cursive; font-size:1.05rem; background:var(--cream); }
  .page-frame{ background:var(--panel); border-radius:14px; box-shadow:var(--shadow); padding:16px; position:relative; }
  .page-canvas-wrap{ position:relative; width:620px; max-width:78vw; aspect-ratio:3/4; border-radius:8px; overflow:hidden; border:1px solid var(--line); }
  .page-canvas-wrap canvas{ position:absolute; top:0; left:0; width:100%; height:100%; }
  #drawCanvas{ cursor:crosshair; touch-action:none; }
  .text-overlay-input{ position:absolute; background:rgba(255,253,249,0.85); border:1px dashed var(--accent); border-radius:4px; padding:4px 6px; font-family:'Tajawal',sans-serif; resize:both; min-width:110px; min-height:32px; z-index:5; outline:none; }
  .saved-notes{ display:flex; flex-direction:column; gap:10px; width:165px; flex-shrink:0; }
  .saved-notes h4{ font-family:'Cormorant Garamond', serif; margin:0; font-size:0.98rem; }
  .saved-thumb{ border:1px solid var(--line); border-radius:10px; overflow:hidden; cursor:pointer; background:var(--panel); box-shadow:var(--shadow); position:relative; }
  .saved-thumb img{ width:100%; display:block; }
  .saved-thumb .cap{ font-size:0.68rem; padding:5px 7px; color:var(--ink-soft); display:flex; justify-content:space-between; align-items:center; }
  .saved-thumb .cap button{ border:none; background:none; color:var(--danger); cursor:pointer; font-size:0.66rem; font-family:inherit; }
  .hidden{ display:none; }
  footer{ text-align:center; color:var(--ink-soft); font-size:0.74rem; margin-top:30px; }
  @media (max-width: 900px){
    .layout{ flex-direction:column; }
    .sidebar{ width:100%; flex-direction:row; align-items:center; overflow-x:auto; padding:14px; }
    .brand{ border-bottom:none; padding-bottom:0; }
    .nav{ flex-direction:row; }
    .xp-box{ display:none; }
    .lang-select{ width:auto; }
    .theme-picker{ margin-top:0; }
    .main{ padding:18px; }
    .notes-layout{ flex-direction:column; }
    .page-canvas-wrap{ width:100%; max-width:100%; }
  }
</style>
</head>
<body>
<div class="layout">
  <aside class="sidebar">
    <div class="brand">
      <div class="script">the</div>
      <div class="title">Study Planner</div>
    </div>
    <select class="lang-select" id="langSelect">
      <option value="ar">العربية</option>
      <option value="en">English</option>
      <option value="fr">Français</option>
      <option value="es">Español</option>
      <option value="de">Deutsch</option>
      <option value="tr">Türkçe</option>
      <option value="ur">اردو</option>
      <option value="hi">हिन्दी</option>
      <option value="id">Bahasa Indonesia</option>
      <option value="pt">Português</option>
      <option value="ru">Русский</option>
      <option value="zh">中文</option>
      <option value="ja">日本語</option>
    </select>

    <div class="xp-box">
      <div class="level" id="levelLabel">—</div>
      <div class="xp-bar-bg"><div class="xp-bar-fill" id="xpFill"></div></div>
      <div class="xp-sub" id="xpSub"></div>
      <div class="streak-line">🔥 <span id="streakLine"></span></div>
    </div>

    <nav class="nav">
      <button data-view="week" class="active"><span class="dot"></span><span data-i18n="navWeek"></span></button>
      <button data-view="month"><span class="dot"></span><span data-i18n="navMonth"></span></button>
      <button data-view="focus"><span class="dot"></span><span data-i18n="navFocus"></span></button>
      <button data-view="exams"><span class="dot"></span><span data-i18n="navExams"></span></button>
      <button data-view="notes"><span class="dot"></span><span data-i18n="navNotes"></span></button>
      <button data-view="subjects"><span class="dot"></span><span data-i18n="navSubjects"></span></button>
      <button data-view="stats"><span class="dot"></span><span data-i18n="navStats"></span></button>
    </nav>

    <div class="theme-picker">
      <div class="label" data-i18n="colorLabel"></div>
      <div class="swatches">
        <div class="swatch selected" style="background:#C98D89" data-theme="rose"></div>
        <div class="swatch" style="background:#8CA79A" data-theme="sage"></div>
        <div class="swatch" style="background:#3A4A6B" data-theme="navy"></div>
        <div class="swatch" style="background:#C9A24B" data-theme="gold"></div>
      </div>
    </div>
  </aside>

  <main class="main">
    <header class="page-head">
      <h1 id="pageTitle"></h1>
      <div class="date-badge" id="todayBadge"></div>
    </header>

    <div class="cards-row">
      <div class="mini-card"><div class="num" id="cardTasksDone">0</div><div class="lbl" data-i18n="cardTasksDone"></div></div>
      <div class="mini-card"><div class="num" id="cardFocusMin">0</div><div class="lbl" data-i18n="cardFocusMin"></div></div>
      <div class="mini-card"><div class="num" id="cardStreak">0</div><div class="lbl" data-i18n="cardStreak"></div></div>
      <div class="mini-card"><div class="num" id="cardExams">0</div><div class="lbl" data-i18n="cardExams"></div></div>
    </div>

    <section id="view-week">
      <div class="add-task">
        <div class="field"><label data-i18n="taskTitleLabel"></label><input type="text" id="taskTitle"></div>
        <div class="field"><label data-i18n="taskSubjectLabel"></label><input type="text" id="taskSubject" list="subjectsDatalist"></div>
        <div class="field"><label data-i18n="taskDayLabel"></label><select id="taskDay"></select></div>
        <div class="field"><label data-i18n="taskDateLabel"></label><input type="date" id="taskDate"></div>
        <div class="field"><label data-i18n="taskPriorityLabel"></label>
          <select id="taskPriority">
            <option value="low" data-i18n="priorityLow"></option>
            <option value="medium" data-i18n="priorityMedium"></option>
            <option value="high" data-i18n="priorityHigh"></option>
          </select>
        </div>
        <button class="btn-add" id="addBtn" data-i18n="addBtn"></button>
      </div>
      <div class="board" id="board"></div>
    </section>

    <section id="view-month" class="hidden">
      <div class="calendar-nav"><button id="prevMonth">‹</button><div class="month-label" id="monthLabel"></div><button id="nextMonth">›</button></div>
      <div class="calendar-grid" id="calendarGrid"></div>
    </section>

    <section id="view-focus" class="hidden">
      <div class="focus-wrap">
        <div class="focus-select">
          <input type="text" id="focusSubject" list="subjectsDatalist" data-i18n-placeholder="focusSubjectPlaceholder">
          <input type="number" id="focusDuration" min="1" max="240" value="25" style="width:80px;">
          <span data-i18n="minutesLabel"></span>
          <div class="preset-btns">
            <button data-min="25">25</button><button data-min="45">45</button><button data-min="60">60</button>
          </div>
        </div>
        <div class="timer-display" id="timerDisplay">25:00</div>
        <div class="timer-controls">
          <button class="btn-start" id="startBtn" data-i18n="startBtn"></button>
          <button class="btn-reset" id="resetBtn" data-i18n="resetBtn"></button>
        </div>
        <div class="focus-log"><h3 data-i18n="focusLogTitle"></h3><div id="focusLogList"></div></div>
      </div>
    </section>

    <section id="view-exams" class="hidden">
      <div class="exam-form">
        <div class="field"><label data-i18n="examNameLabel"></label><input type="text" id="examName"></div>
        <div class="field"><label data-i18n="examDateLabel"></label><input type="date" id="examDate"></div>
        <div class="field"><label data-i18n="examTopicsLabel"></label><input type="number" id="examTopics" min="1" value="5"></div>
        <button class="btn-add" id="examAddBtn" data-i18n="examCreateBtn"></button>
      </div>
      <div class="exam-list" id="examList"></div>
    </section>

    <section id="view-notes" class="hidden">
      <div class="notes-layout">
        <div class="notes-toolbar">
          <h4 data-i18n="writingModeLabel"></h4>
          <div class="tpl-options" id="modeOptions">
            <button data-mode="pen" class="active" data-i18n="modePen"></button>
            <button data-mode="text" data-i18n="modeText"></button>
          </div>
          <h4 data-i18n="paperTypeLabel"></h4>
          <div class="tpl-options" id="tplOptions">
            <button data-tpl="lined" class="active" data-i18n="paperLined"></button>
            <button data-tpl="dot" data-i18n="paperDot"></button>
            <button data-tpl="grid" data-i18n="paperGrid"></button>
            <button data-tpl="cornell" data-i18n="paperCornell"></button>
          </div>
          <h4 data-i18n="penColorLabel"></h4>
          <div class="pen-colors" id="penColors">
            <div class="pen-swatch selected" style="background:#2C2A4A" data-color="#2C2A4A"></div>
            <div class="pen-swatch" style="background:#C98D89" data-color="#C98D89"></div>
            <div class="pen-swatch" style="background:#3A4A6B" data-color="#3A4A6B"></div>
            <div class="pen-swatch" style="background:#8CA79A" data-color="#8CA79A"></div>
            <div class="pen-swatch" style="background:#C9A24B" data-color="#C9A24B"></div>
          </div>
          <div id="penSizeWrap" class="pen-size"><span data-i18n="penSizeLabel"></span><input type="range" id="penSize" min="1" max="8" value="2"></div>
          <div id="fontSizeWrap" class="pen-size hidden">
            <span data-i18n="fontSizeLabel"></span>
            <select id="fontSize">
              <option value="16" data-i18n="fontSmall"></option>
              <option value="22" selected data-i18n="fontMedium"></option>
              <option value="30" data-i18n="fontLarge"></option>
            </select>
          </div>
          <input class="notes-title-input" id="noteTitle" data-i18n-placeholder="pageTitlePlaceholder" value="">
          <button class="tool-btn" id="eraserBtn" data-i18n="eraserBtn"></button>
          <button class="tool-btn" id="clearCanvasBtn" data-i18n="clearPage"></button>
          <button class="tool-btn primary" id="saveNoteBtn" data-i18n="savePage"></button>
          <button class="tool-btn" id="downloadNoteBtn" data-i18n="downloadImage"></button>
          <button class="tool-btn" id="printNoteBtn" data-i18n="printPage"></button>
        </div>
        <div class="page-frame">
          <div class="page-canvas-wrap" id="canvasWrap">
            <canvas id="bgCanvas" width="620" height="826"></canvas>
            <canvas id="drawCanvas" width="620" height="826"></canvas>
          </div>
        </div>
        <div class="saved-notes">
          <h4 data-i18n="savedPagesTitle"></h4>
          <div id="savedNotesList"></div>
        </div>
      </div>
    </section>

    <section id="view-subjects" class="hidden"><div class="subjects-grid" id="subjectsGrid"></div></section>

    <section id="view-stats" class="hidden">
      <div class="heatmap-wrap">
        <h3 style="font-family:'Cormorant Garamond', serif; margin-top:0;" data-i18n="heatmapTitle"></h3>
        <div class="heatmap-grid" id="heatmapGrid"></div>
        <div class="heat-legend">
          <span data-i18n="heatLess"></span>
          <div class="heat-cell" style="background:var(--cream)"></div>
          <div class="heat-cell" style="background:var(--accent-light)"></div>
          <div class="heat-cell" style="background:var(--accent); opacity:0.5"></div>
          <div class="heat-cell" style="background:var(--accent)"></div>
          <span data-i18n="heatMore"></span>
        </div>
      </div>
    </section>

    <datalist id="subjectsDatalist"></datalist>
    <footer data-i18n="footerText"></footer>
  </main>
</div>

<script>
/* ================= I18N ================= */
const RTL_LANGS = ['ar','ur'];
const LOCALE_MAP = { ar:'ar', en:'en', fr:'fr', es:'es', de:'de', tr:'tr', ur:'ur', hi:'hi', id:'id', pt:'pt', ru:'ru', zh:'zh-Hans', ja:'ja' };

const I18N = {
ar:{navWeek:"الأسبوعي",navMonth:"الشهري",navFocus:"وضع التركيز",navExams:"عد الاختبارات",navNotes:"أوراق الكتابة",navSubjects:"المواد",navStats:"الإحصائيات",colorLabel:"اللون",cardTasksDone:"مهام منجزة",cardFocusMin:"دقيقة تركيز",cardStreak:"أيام متتالية",cardExams:"اختبار قادم",taskTitleLabel:"عنوان المهمة",taskSubjectLabel:"المادة",taskDayLabel:"اليوم",taskDateLabel:"التاريخ",taskPriorityLabel:"الأولوية",priorityLow:"عادية",priorityMedium:"متوسطة",priorityHigh:"عالية",addBtn:"إضافة",noTasks:"لا توجد مهام",toggleDone:"إنجاز",toggleUndo:"تراجع",deleteBtn:"حذف",focusSubjectPlaceholder:"اكتب اسم المادة",minutesLabel:"دقيقة",startBtn:"ابدأ",stopBtn:"إيقاف",resetBtn:"إعادة",focusLogTitle:"آخر جلسات التركيز",noSessions:"لا توجد جلسات بعد",examNameLabel:"اسم المادة / الاختبار",examDateLabel:"تاريخ الاختبار",examTopicsLabel:"عدد المواضيع المتبقية",examCreateBtn:"أنشئ خطة",noExams:"لا توجد اختبارات مضافة",daysLeft:"يوم متبقي",examExpired:"انتهى",deletePlan:"حذف الخطة",writingModeLabel:"طريقة الكتابة",modePen:"قلم / يد / سيالة",modeText:"كتابة بالكيبورد",paperTypeLabel:"نوع الورقة",paperLined:"مسطّرة",paperDot:"نقطية",paperGrid:"مربعات",paperCornell:"كورنيل",penColorLabel:"لون القلم / النص",penSizeLabel:"السُمك",fontSizeLabel:"حجم الخط",fontSmall:"صغير",fontMedium:"متوسط",fontLarge:"كبير",pageTitlePlaceholder:"عنوان الصفحة...",eraserBtn:"ممحاة",clearPage:"مسح الصفحة",savePage:"حفظ الصفحة",downloadImage:"تنزيل كصورة",printPage:"طباعة",savedPagesTitle:"الصفحات المحفوظة",noSavedPages:"لا توجد صفحات محفوظة",subjectsNone:"أضف مهامًا أو جلسات تركيز لتظهر موادك هنا",doneOfTotal:"{done} من {total} مهمة منجزة",minutesFocus:"{m} دقيقة تركيز",heatmapTitle:"خريطة النشاط (آخر 12 أسبوع)",heatLess:"أقل",heatMore:"أكثر",footerText:"يتم حفظ بياناتك تلقائيًا في هذا التطبيق",levelLabel:"المستوى {n}",xpSub:"{x} / 100 نقطة",streakLine:"سلسلة: {n} يوم",pageWeek:"اللوحة الأسبوعية",pageMonth:"التقويم الشهري",pageFocus:"وضع التركيز",pageExams:"عدّاد الاختبارات الذكي",pageNotes:"أوراق الكتابة",pageSubjects:"المواد الدراسية",pageStats:"الإحصائيات",noNoteTitle:"ملاحظاتي",examMeta:"{topics} موضوع · تاريخ الاختبار {date}"},
en:{navWeek:"Weekly",navMonth:"Monthly",navFocus:"Focus Mode",navExams:"Exam Countdown",navNotes:"Writing Pages",navSubjects:"Subjects",navStats:"Statistics",colorLabel:"Color",cardTasksDone:"Tasks done",cardFocusMin:"Focus minutes",cardStreak:"Day streak",cardExams:"Upcoming exams",taskTitleLabel:"Task title",taskSubjectLabel:"Subject",taskDayLabel:"Day",taskDateLabel:"Date",taskPriorityLabel:"Priority",priorityLow:"Low",priorityMedium:"Medium",priorityHigh:"High",addBtn:"Add",noTasks:"No tasks",toggleDone:"Done",toggleUndo:"Undo",deleteBtn:"Delete",focusSubjectPlaceholder:"Type subject name",minutesLabel:"min",startBtn:"Start",stopBtn:"Stop",resetBtn:"Reset",focusLogTitle:"Recent focus sessions",noSessions:"No sessions yet",examNameLabel:"Subject / exam name",examDateLabel:"Exam date",examTopicsLabel:"Topics remaining",examCreateBtn:"Create plan",noExams:"No exams added",daysLeft:"days left",examExpired:"Passed",deletePlan:"Delete plan",writingModeLabel:"Writing mode",modePen:"Pen / hand / stylus",modeText:"Type with keyboard",paperTypeLabel:"Paper type",paperLined:"Lined",paperDot:"Dot grid",paperGrid:"Graph",paperCornell:"Cornell",penColorLabel:"Pen / text color",penSizeLabel:"Thickness",fontSizeLabel:"Font size",fontSmall:"Small",fontMedium:"Medium",fontLarge:"Large",pageTitlePlaceholder:"Page title...",eraserBtn:"Eraser",clearPage:"Clear page",savePage:"Save page",downloadImage:"Download image",printPage:"Print",savedPagesTitle:"Saved pages",noSavedPages:"No saved pages",subjectsNone:"Add tasks or focus sessions to see subjects here",doneOfTotal:"{done} of {total} tasks done",minutesFocus:"{m} focus minutes",heatmapTitle:"Activity heatmap (last 12 weeks)",heatLess:"Less",heatMore:"More",footerText:"Your data is saved automatically in this app",levelLabel:"Level {n}",xpSub:"{x} / 100 points",streakLine:"Streak: {n} days",pageWeek:"Weekly Board",pageMonth:"Monthly Calendar",pageFocus:"Focus Mode",pageExams:"Smart Exam Countdown",pageNotes:"Writing Pages",pageSubjects:"Subjects",pageStats:"Statistics",noNoteTitle:"My Notes",examMeta:"{topics} topics · exam date {date}"},
fr:{navWeek:"Semaine",navMonth:"Mois",navFocus:"Mode Focus",navExams:"Compte à rebours",navNotes:"Pages d'écriture",navSubjects:"Matières",navStats:"Statistiques",colorLabel:"Couleur",cardTasksDone:"Tâches faites",cardFocusMin:"Minutes de focus",cardStreak:"Jours de suite",cardExams:"Examens à venir",taskTitleLabel:"Titre de la tâche",taskSubjectLabel:"Matière",taskDayLabel:"Jour",taskDateLabel:"Date",taskPriorityLabel:"Priorité",priorityLow:"Basse",priorityMedium:"Moyenne",priorityHigh:"Haute",addBtn:"Ajouter",noTasks:"Aucune tâche",toggleDone:"Fait",toggleUndo:"Annuler",deleteBtn:"Supprimer",focusSubjectPlaceholder:"Nom de la matière",minutesLabel:"min",startBtn:"Démarrer",stopBtn:"Arrêter",resetBtn:"Réinitialiser",focusLogTitle:"Sessions récentes",noSessions:"Aucune session",examNameLabel:"Matière / examen",examDateLabel:"Date de l'examen",examTopicsLabel:"Sujets restants",examCreateBtn:"Créer un plan",noExams:"Aucun examen ajouté",daysLeft:"jours restants",examExpired:"Passé",deletePlan:"Supprimer le plan",writingModeLabel:"Mode d'écriture",modePen:"Stylo / main / stylet",modeText:"Clavier",paperTypeLabel:"Type de papier",paperLined:"Ligné",paperDot:"Pointillé",paperGrid:"Quadrillé",paperCornell:"Cornell",penColorLabel:"Couleur",penSizeLabel:"Épaisseur",fontSizeLabel:"Taille du texte",fontSmall:"Petit",fontMedium:"Moyen",fontLarge:"Grand",pageTitlePlaceholder:"Titre de la page...",eraserBtn:"Gomme",clearPage:"Effacer",savePage:"Enregistrer",downloadImage:"Télécharger",printPage:"Imprimer",savedPagesTitle:"Pages enregistrées",noSavedPages:"Aucune page enregistrée",subjectsNone:"Ajoutez des tâches pour voir vos matières ici",doneOfTotal:"{done} sur {total} tâches faites",minutesFocus:"{m} minutes de focus",heatmapTitle:"Carte d'activité (12 dernières semaines)",heatLess:"Moins",heatMore:"Plus",footerText:"Vos données sont enregistrées automatiquement",levelLabel:"Niveau {n}",xpSub:"{x} / 100 points",streakLine:"Série : {n} jours",pageWeek:"Tableau hebdomadaire",pageMonth:"Calendrier mensuel",pageFocus:"Mode Focus",pageExams:"Compte à rebours intelligent",pageNotes:"Pages d'écriture",pageSubjects:"Matières",pageStats:"Statistiques",noNoteTitle:"Mes notes",examMeta:"{topics} sujets · examen le {date}"},
es:{navWeek:"Semanal",navMonth:"Mensual",navFocus:"Modo Enfoque",navExams:"Cuenta atrás",navNotes:"Hojas de escritura",navSubjects:"Materias",navStats:"Estadísticas",colorLabel:"Color",cardTasksDone:"Tareas hechas",cardFocusMin:"Minutos de enfoque",cardStreak:"Días seguidos",cardExams:"Exámenes próximos",taskTitleLabel:"Título de tarea",taskSubjectLabel:"Materia",taskDayLabel:"Día",taskDateLabel:"Fecha",taskPriorityLabel:"Prioridad",priorityLow:"Baja",priorityMedium:"Media",priorityHigh:"Alta",addBtn:"Añadir",noTasks:"Sin tareas",toggleDone:"Hecho",toggleUndo:"Deshacer",deleteBtn:"Eliminar",focusSubjectPlaceholder:"Escribe la materia",minutesLabel:"min",startBtn:"Iniciar",stopBtn:"Detener",resetBtn:"Reiniciar",focusLogTitle:"Sesiones recientes",noSessions:"Sin sesiones aún",examNameLabel:"Materia / examen",examDateLabel:"Fecha del examen",examTopicsLabel:"Temas restantes",examCreateBtn:"Crear plan",noExams:"Sin exámenes añadidos",daysLeft:"días restantes",examExpired:"Pasado",deletePlan:"Eliminar plan",writingModeLabel:"Modo de escritura",modePen:"Lápiz / mano / lápiz óptico",modeText:"Escribir con teclado",paperTypeLabel:"Tipo de papel",paperLined:"Rayado",paperDot:"Puntos",paperGrid:"Cuadrícula",paperCornell:"Cornell",penColorLabel:"Color",penSizeLabel:"Grosor",fontSizeLabel:"Tamaño de letra",fontSmall:"Pequeño",fontMedium:"Mediano",fontLarge:"Grande",pageTitlePlaceholder:"Título de la página...",eraserBtn:"Borrador",clearPage:"Borrar página",savePage:"Guardar página",downloadImage:"Descargar imagen",printPage:"Imprimir",savedPagesTitle:"Páginas guardadas",noSavedPages:"Sin páginas guardadas",subjectsNone:"Añade tareas o sesiones para ver tus materias",doneOfTotal:"{done} de {total} tareas hechas",minutesFocus:"{m} minutos de enfoque",heatmapTitle:"Mapa de actividad (últimas 12 semanas)",heatLess:"Menos",heatMore:"Más",footerText:"Tus datos se guardan automáticamente",levelLabel:"Nivel {n}",xpSub:"{x} / 100 puntos",streakLine:"Racha: {n} días",pageWeek:"Tablero semanal",pageMonth:"Calendario mensual",pageFocus:"Modo Enfoque",pageExams:"Cuenta atrás inteligente",pageNotes:"Hojas de escritura",pageSubjects:"Materias",pageStats:"Estadísticas",noNoteTitle:"Mis notas",examMeta:"{topics} temas · examen el {date}"},
de:{navWeek:"Wöchentlich",navMonth:"Monatlich",navFocus:"Fokus-Modus",navExams:"Prüfungs-Countdown",navNotes:"Schreibseiten",navSubjects:"Fächer",navStats:"Statistik",colorLabel:"Farbe",cardTasksDone:"Erledigte Aufgaben",cardFocusMin:"Fokus-Minuten",cardStreak:"Tage in Folge",cardExams:"Bevorstehende Prüfungen",taskTitleLabel:"Aufgabentitel",taskSubjectLabel:"Fach",taskDayLabel:"Tag",taskDateLabel:"Datum",taskPriorityLabel:"Priorität",priorityLow:"Niedrig",priorityMedium:"Mittel",priorityHigh:"Hoch",addBtn:"Hinzufügen",noTasks:"Keine Aufgaben",toggleDone:"Erledigt",toggleUndo:"Rückgängig",deleteBtn:"Löschen",focusSubjectPlaceholder:"Fach eingeben",minutesLabel:"Min",startBtn:"Start",stopBtn:"Stopp",resetBtn:"Zurücksetzen",focusLogTitle:"Letzte Sitzungen",noSessions:"Noch keine Sitzungen",examNameLabel:"Fach / Prüfung",examDateLabel:"Prüfungsdatum",examTopicsLabel:"Verbleibende Themen",examCreateBtn:"Plan erstellen",noExams:"Keine Prüfungen hinzugefügt",daysLeft:"Tage übrig",examExpired:"Vorbei",deletePlan:"Plan löschen",writingModeLabel:"Schreibmodus",modePen:"Stift / Hand / Eingabestift",modeText:"Tastatureingabe",paperTypeLabel:"Papierart",paperLined:"Liniert",paperDot:"Punktraster",paperGrid:"Kariert",paperCornell:"Cornell",penColorLabel:"Farbe",penSizeLabel:"Dicke",fontSizeLabel:"Schriftgröße",fontSmall:"Klein",fontMedium:"Mittel",fontLarge:"Groß",pageTitlePlaceholder:"Seitentitel...",eraserBtn:"Radiergummi",clearPage:"Seite leeren",savePage:"Seite speichern",downloadImage:"Bild herunterladen",printPage:"Drucken",savedPagesTitle:"Gespeicherte Seiten",noSavedPages:"Keine gespeicherten Seiten",subjectsNone:"Füge Aufgaben oder Sitzungen hinzu",doneOfTotal:"{done} von {total} erledigt",minutesFocus:"{m} Fokus-Minuten",heatmapTitle:"Aktivitätskarte (letzte 12 Wochen)",heatLess:"Weniger",heatMore:"Mehr",footerText:"Deine Daten werden automatisch gespeichert",levelLabel:"Level {n}",xpSub:"{x} / 100 Punkte",streakLine:"Serie: {n} Tage",pageWeek:"Wochenplan",pageMonth:"Monatskalender",pageFocus:"Fokus-Modus",pageExams:"Intelligenter Countdown",pageNotes:"Schreibseiten",pageSubjects:"Fächer",pageStats:"Statistik",noNoteTitle:"Meine Notizen",examMeta:"{topics} Themen · Prüfung am {date}"},
tr:{navWeek:"Haftalık",navMonth:"Aylık",navFocus:"Odak Modu",navExams:"Sınav Sayacı",navNotes:"Yazı Sayfaları",navSubjects:"Dersler",navStats:"İstatistikler",colorLabel:"Renk",cardTasksDone:"Tamamlanan görev",cardFocusMin:"Odak dakikası",cardStreak:"Ardışık gün",cardExams:"Yaklaşan sınav",taskTitleLabel:"Görev başlığı",taskSubjectLabel:"Ders",taskDayLabel:"Gün",taskDateLabel:"Tarih",taskPriorityLabel:"Öncelik",priorityLow:"Düşük",priorityMedium:"Orta",priorityHigh:"Yüksek",addBtn:"Ekle",noTasks:"Görev yok",toggleDone:"Tamamlandı",toggleUndo:"Geri al",deleteBtn:"Sil",focusSubjectPlaceholder:"Ders adını yaz",minutesLabel:"dk",startBtn:"Başlat",stopBtn:"Durdur",resetBtn:"Sıfırla",focusLogTitle:"Son oturumlar",noSessions:"Henüz oturum yok",examNameLabel:"Ders / sınav adı",examDateLabel:"Sınav tarihi",examTopicsLabel:"Kalan konu sayısı",examCreateBtn:"Plan oluştur",noExams:"Sınav eklenmedi",daysLeft:"gün kaldı",examExpired:"Geçti",deletePlan:"Planı sil",writingModeLabel:"Yazma modu",modePen:"Kalem / el / stylus",modeText:"Klavye ile yaz",paperTypeLabel:"Kağıt türü",paperLined:"Çizgili",paperDot:"Noktalı",paperGrid:"Kareli",paperCornell:"Cornell",penColorLabel:"Renk",penSizeLabel:"Kalınlık",fontSizeLabel:"Yazı boyutu",fontSmall:"Küçük",fontMedium:"Orta",fontLarge:"Büyük",pageTitlePlaceholder:"Sayfa başlığı...",eraserBtn:"Silgi",clearPage:"Sayfayı temizle",savePage:"Sayfayı kaydet",downloadImage:"Resim indir",printPage:"Yazdır",savedPagesTitle:"Kayıtlı sayfalar",noSavedPages:"Kayıtlı sayfa yok",subjectsNone:"Derslerinizi görmek için görev ekleyin",doneOfTotal:"{total} görevden {done} tamamlandı",minutesFocus:"{m} odak dakikası",heatmapTitle:"Etkinlik haritası (son 12 hafta)",heatLess:"Az",heatMore:"Çok",footerText:"Verileriniz otomatik olarak kaydedilir",levelLabel:"Seviye {n}",xpSub:"{x} / 100 puan",streakLine:"Seri: {n} gün",pageWeek:"Haftalık Pano",pageMonth:"Aylık Takvim",pageFocus:"Odak Modu",pageExams:"Akıllı Sınav Sayacı",pageNotes:"Yazı Sayfaları",pageSubjects:"Dersler",pageStats:"İstatistikler",noNoteTitle:"Notlarım",examMeta:"{topics} konu · sınav tarihi {date}"},
ur:{navWeek:"ہفتہ وار",navMonth:"ماہانہ",navFocus:"فوکس موڈ",navExams:"امتحان کاؤنٹ ڈاؤن",navNotes:"تحریری صفحات",navSubjects:"مضامین",navStats:"اعداد و شمار",colorLabel:"رنگ",cardTasksDone:"مکمل کام",cardFocusMin:"فوکس منٹ",cardStreak:"مسلسل دن",cardExams:"آنے والے امتحانات",taskTitleLabel:"کام کا عنوان",taskSubjectLabel:"مضمون",taskDayLabel:"دن",taskDateLabel:"تاریخ",taskPriorityLabel:"ترجیح",priorityLow:"کم",priorityMedium:"درمیانہ",priorityHigh:"زیادہ",addBtn:"شامل کریں",noTasks:"کوئی کام نہیں",toggleDone:"مکمل",toggleUndo:"واپس",deleteBtn:"حذف کریں",focusSubjectPlaceholder:"مضمون کا نام لکھیں",minutesLabel:"منٹ",startBtn:"شروع",stopBtn:"روکیں",resetBtn:"ری سیٹ",focusLogTitle:"حالیہ سیشنز",noSessions:"ابھی کوئی سیشن نہیں",examNameLabel:"مضمون / امتحان کا نام",examDateLabel:"امتحان کی تاریخ",examTopicsLabel:"باقی موضوعات",examCreateBtn:"منصوبہ بنائیں",noExams:"کوئی امتحان شامل نہیں",daysLeft:"دن باقی",examExpired:"ختم",deletePlan:"منصوبہ حذف کریں",writingModeLabel:"لکھنے کا طریقہ",modePen:"قلم / ہاتھ / اسٹائلس",modeText:"کی بورڈ سے لکھیں",paperTypeLabel:"کاغذ کی قسم",paperLined:"لائنڈ",paperDot:"ڈاٹ گرڈ",paperGrid:"گرڈ",paperCornell:"کورنیل",penColorLabel:"رنگ",penSizeLabel:"موٹائی",fontSizeLabel:"فونٹ سائز",fontSmall:"چھوٹا",fontMedium:"درمیانہ",fontLarge:"بڑا",pageTitlePlaceholder:"صفحہ کا عنوان...",eraserBtn:"ایریزر",clearPage:"صفحہ صاف کریں",savePage:"صفحہ محفوظ کریں",downloadImage:"تصویر ڈاؤن لوڈ کریں",printPage:"پرنٹ",savedPagesTitle:"محفوظ شدہ صفحات",noSavedPages:"کوئی محفوظ صفحہ نہیں",subjectsNone:"مضامین دیکھنے کے لیے کام شامل کریں",doneOfTotal:"{total} میں سے {done} کام مکمل",minutesFocus:"{m} فوکس منٹ",heatmapTitle:"سرگرمی نقشہ (آخری 12 ہفتے)",heatLess:"کم",heatMore:"زیادہ",footerText:"آپ کا ڈیٹا خودکار طور پر محفوظ ہوتا ہے",levelLabel:"سطح {n}",xpSub:"{x} / 100 پوائنٹس",streakLine:"سلسلہ: {n} دن",pageWeek:"ہفتہ وار بورڈ",pageMonth:"ماہانہ کیلنڈر",pageFocus:"فوکس موڈ",pageExams:"ذہین امتحان کاؤنٹ ڈاؤن",pageNotes:"تحریری صفحات",pageSubjects:"مضامین",pageStats:"اعداد و شمار",noNoteTitle:"میرے نوٹس",examMeta:"{topics} موضوعات · امتحان کی تاریخ {date}"},
hi:{navWeek:"साप्ताहिक",navMonth:"मासिक",navFocus:"फोकस मोड",navExams:"परीक्षा उलटी गिनती",navNotes:"लेखन पृष्ठ",navSubjects:"विषय",navStats:"आंकड़े",colorLabel:"रंग",cardTasksDone:"पूर्ण कार्य",cardFocusMin:"फोकस मिनट",cardStreak:"लगातार दिन",cardExams:"आगामी परीक्षा",taskTitleLabel:"कार्य शीर्षक",taskSubjectLabel:"विषय",taskDayLabel:"दिन",taskDateLabel:"तारीख",taskPriorityLabel:"प्राथमिकता",priorityLow:"कम",priorityMedium:"मध्यम",priorityHigh:"उच्च",addBtn:"जोड़ें",noTasks:"कोई कार्य नहीं",toggleDone:"पूर्ण",toggleUndo:"वापस",deleteBtn:"हटाएं",focusSubjectPlaceholder:"विषय लिखें",minutesLabel:"मिनट",startBtn:"शुरू करें",stopBtn:"रोकें",resetBtn:"रीसेट",focusLogTitle:"हाल के सत्र",noSessions:"अभी कोई सत्र नहीं",examNameLabel:"विषय / परीक्षा नाम",examDateLabel:"परीक्षा तिथि",examTopicsLabel:"शेष विषय",examCreateBtn:"योजना बनाएं",noExams:"कोई परीक्षा नहीं जोड़ी",daysLeft:"दिन शेष",examExpired:"समाप्त",deletePlan:"योजना हटाएं",writingModeLabel:"लेखन मोड",modePen:"पेन / हाथ / स्टाइलस",modeText:"कीबोर्ड से लिखें",paperTypeLabel:"पेपर प्रकार",paperLined:"रेखांकित",paperDot:"डॉट ग्रिड",paperGrid:"ग्रिड",paperCornell:"कॉर्नेल",penColorLabel:"रंग",penSizeLabel:"मोटाई",fontSizeLabel:"फॉन्ट आकार",fontSmall:"छोटा",fontMedium:"मध्यम",fontLarge:"बड़ा",pageTitlePlaceholder:"पृष्ठ शीर्षक...",eraserBtn:"मिटाने वाला",clearPage:"पृष्ठ साफ़ करें",savePage:"पृष्ठ सहेजें",downloadImage:"छवि डाउनलोड करें",printPage:"प्रिंट करें",savedPagesTitle:"सहेजे गए पृष्ठ",noSavedPages:"कोई सहेजा पृष्ठ नहीं",subjectsNone:"विषय देखने के लिए कार्य जोड़ें",doneOfTotal:"{total} में से {done} कार्य पूर्ण",minutesFocus:"{m} फोकस मिनट",heatmapTitle:"गतिविधि मानचित्र (पिछले 12 सप्ताह)",heatLess:"कम",heatMore:"अधिक",footerText:"आपका डेटा स्वचालित रूप से सहेजा जाता है",levelLabel:"स्तर {n}",xpSub:"{x} / 100 अंक",streakLine:"लगातार: {n} दिन",pageWeek:"साप्ताहिक बोर्ड",pageMonth:"मासिक कैलेंडर",pageFocus:"फोकस मोड",pageExams:"स्मार्ट परीक्षा उलटी गिनती",pageNotes:"लेखन पृष्ठ",pageSubjects:"विषय",pageStats:"आंकड़े",noNoteTitle:"मेरे नोट्स",examMeta:"{topics} विषय · परीक्षा तिथि {date}"},
id:{navWeek:"Mingguan",navMonth:"Bulanan",navFocus:"Mode Fokus",navExams:"Hitung Mundur Ujian",navNotes:"Halaman Tulisan",navSubjects:"Mata Pelajaran",navStats:"Statistik",colorLabel:"Warna",cardTasksDone:"Tugas selesai",cardFocusMin:"Menit fokus",cardStreak:"Hari berturut-turut",cardExams:"Ujian mendatang",taskTitleLabel:"Judul tugas",taskSubjectLabel:"Mata pelajaran",taskDayLabel:"Hari",taskDateLabel:"Tanggal",taskPriorityLabel:"Prioritas",priorityLow:"Rendah",priorityMedium:"Sedang",priorityHigh:"Tinggi",addBtn:"Tambah",noTasks:"Tidak ada tugas",toggleDone:"Selesai",toggleUndo:"Batal",deleteBtn:"Hapus",focusSubjectPlaceholder:"Tulis nama mata pelajaran",minutesLabel:"mnt",startBtn:"Mulai",stopBtn:"Berhenti",resetBtn:"Atur ulang",focusLogTitle:"Sesi terbaru",noSessions:"Belum ada sesi",examNameLabel:"Mata pelajaran / ujian",examDateLabel:"Tanggal ujian",examTopicsLabel:"Topik tersisa",examCreateBtn:"Buat rencana",noExams:"Belum ada ujian",daysLeft:"hari tersisa",examExpired:"Lewat",deletePlan:"Hapus rencana",writingModeLabel:"Mode menulis",modePen:"Pena / tangan / stylus",modeText:"Ketik dengan keyboard",paperTypeLabel:"Jenis kertas",paperLined:"Bergaris",paperDot:"Titik",paperGrid:"Kotak",paperCornell:"Cornell",penColorLabel:"Warna",penSizeLabel:"Ketebalan",fontSizeLabel:"Ukuran font",fontSmall:"Kecil",fontMedium:"Sedang",fontLarge:"Besar",pageTitlePlaceholder:"Judul halaman...",eraserBtn:"Penghapus",clearPage:"Bersihkan halaman",savePage:"Simpan halaman",downloadImage:"Unduh gambar",printPage:"Cetak",savedPagesTitle:"Halaman tersimpan",noSavedPages:"Belum ada halaman tersimpan",subjectsNone:"Tambahkan tugas untuk melihat mata pelajaran",doneOfTotal:"{done} dari {total} tugas selesai",minutesFocus:"{m} menit fokus",heatmapTitle:"Peta aktivitas (12 minggu terakhir)",heatLess:"Sedikit",heatMore:"Banyak",footerText:"Data Anda disimpan otomatis",levelLabel:"Level {n}",xpSub:"{x} / 100 poin",streakLine:"Runtutan: {n} hari",pageWeek:"Papan Mingguan",pageMonth:"Kalender Bulanan",pageFocus:"Mode Fokus",pageExams:"Hitung Mundur Cerdas",pageNotes:"Halaman Tulisan",pageSubjects:"Mata Pelajaran",pageStats:"Statistik",noNoteTitle:"Catatan Saya",examMeta:"{topics} topik · ujian {date}"},
pt:{navWeek:"Semanal",navMonth:"Mensal",navFocus:"Modo Foco",navExams:"Contagem de Provas",navNotes:"Páginas de Escrita",navSubjects:"Matérias",navStats:"Estatísticas",colorLabel:"Cor",cardTasksDone:"Tarefas concluídas",cardFocusMin:"Minutos de foco",cardStreak:"Dias seguidos",cardExams:"Próximas provas",taskTitleLabel:"Título da tarefa",taskSubjectLabel:"Matéria",taskDayLabel:"Dia",taskDateLabel:"Data",taskPriorityLabel:"Prioridade",priorityLow:"Baixa",priorityMedium:"Média",priorityHigh:"Alta",addBtn:"Adicionar",noTasks:"Sem tarefas",toggleDone:"Feito",toggleUndo:"Desfazer",deleteBtn:"Excluir",focusSubjectPlaceholder:"Digite a matéria",minutesLabel:"min",startBtn:"Iniciar",stopBtn:"Parar",resetBtn:"Reiniciar",focusLogTitle:"Sessões recentes",noSessions:"Nenhuma sessão ainda",examNameLabel:"Matéria / prova",examDateLabel:"Data da prova",examTopicsLabel:"Tópicos restantes",examCreateBtn:"Criar plano",noExams:"Nenhuma prova adicionada",daysLeft:"dias restantes",examExpired:"Passou",deletePlan:"Excluir plano",writingModeLabel:"Modo de escrita",modePen:"Caneta / mão / caneta digital",modeText:"Digitar no teclado",paperTypeLabel:"Tipo de papel",paperLined:"Pautado",paperDot:"Pontilhado",paperGrid:"Quadriculado",paperCornell:"Cornell",penColorLabel:"Cor",penSizeLabel:"Espessura",fontSizeLabel:"Tamanho da fonte",fontSmall:"Pequeno",fontMedium:"Médio",fontLarge:"Grande",pageTitlePlaceholder:"Título da página...",eraserBtn:"Borracha",clearPage:"Limpar página",savePage:"Salvar página",downloadImage:"Baixar imagem",printPage:"Imprimir",savedPagesTitle:"Páginas salvas",noSavedPages:"Nenhuma página salva",subjectsNone:"Adicione tarefas para ver suas matérias",doneOfTotal:"{done} de {total} tarefas concluídas",minutesFocus:"{m} minutos de foco",heatmapTitle:"Mapa de atividade (últimas 12 semanas)",heatLess:"Menos",heatMore:"Mais",footerText:"Seus dados são salvos automaticamente",levelLabel:"Nível {n}",xpSub:"{x} / 100 pontos",streakLine:"Sequência: {n} dias",pageWeek:"Painel Semanal",pageMonth:"Calendário Mensal",pageFocus:"Modo Foco",pageExams:"Contagem Inteligente",pageNotes:"Páginas de Escrita",pageSubjects:"Matérias",pageStats:"Estatísticas",noNoteTitle:"Minhas Notas",examMeta:"{topics} tópicos · prova em {date}"},
ru:{navWeek:"Неделя",navMonth:"Месяц",navFocus:"Режим фокуса",navExams:"Обратный отсчёт",navNotes:"Страницы для письма",navSubjects:"Предметы",navStats:"Статистика",colorLabel:"Цвет",cardTasksDone:"Выполнено задач",cardFocusMin:"Минут фокуса",cardStreak:"Дней подряд",cardExams:"Ближайшие экзамены",taskTitleLabel:"Название задачи",taskSubjectLabel:"Предмет",taskDayLabel:"День",taskDateLabel:"Дата",taskPriorityLabel:"Приоритет",priorityLow:"Низкий",priorityMedium:"Средний",priorityHigh:"Высокий",addBtn:"Добавить",noTasks:"Нет задач",toggleDone:"Готово",toggleUndo:"Отменить",deleteBtn:"Удалить",focusSubjectPlaceholder:"Введите предмет",minutesLabel:"мин",startBtn:"Старт",stopBtn:"Стоп",resetBtn:"Сброс",focusLogTitle:"Недавние сессии",noSessions:"Пока нет сессий",examNameLabel:"Предмет / экзамен",examDateLabel:"Дата экзамена",examTopicsLabel:"Осталось тем",examCreateBtn:"Создать план",noExams:"Экзамены не добавлены",daysLeft:"дней осталось",examExpired:"Прошёл",deletePlan:"Удалить план",writingModeLabel:"Режим письма",modePen:"Ручка / рука / стилус",modeText:"Ввод с клавиатуры",paperTypeLabel:"Тип бумаги",paperLined:"Линованная",paperDot:"Точечная сетка",paperGrid:"Клетка",paperCornell:"Корнелл",penColorLabel:"Цвет",penSizeLabel:"Толщина",fontSizeLabel:"Размер шрифта",fontSmall:"Маленький",fontMedium:"Средний",fontLarge:"Большой",pageTitlePlaceholder:"Заголовок страницы...",eraserBtn:"Ластик",clearPage:"Очистить страницу",savePage:"Сохранить страницу",downloadImage:"Скачать изображение",printPage:"Печать",savedPagesTitle:"Сохранённые страницы",noSavedPages:"Нет сохранённых страниц",subjectsNone:"Добавьте задачи, чтобы увидеть предметы",doneOfTotal:"{done} из {total} задач выполнено",minutesFocus:"{m} минут фокуса",heatmapTitle:"Карта активности (последние 12 недель)",heatLess:"Меньше",heatMore:"Больше",footerText:"Ваши данные сохраняются автоматически",levelLabel:"Уровень {n}",xpSub:"{x} / 100 очков",streakLine:"Серия: {n} дней",pageWeek:"Недельная доска",pageMonth:"Месячный календарь",pageFocus:"Режим фокуса",pageExams:"Умный обратный отсчёт",pageNotes:"Страницы для письма",pageSubjects:"Предметы",pageStats:"Статистика",noNoteTitle:"Мои заметки",examMeta:"{topics} тем · экзамен {date}"},
zh:{navWeek:"每周",navMonth:"每月",navFocus:"专注模式",navExams:"考试倒计时",navNotes:"书写页面",navSubjects:"科目",navStats:"统计",colorLabel:"颜色",cardTasksDone:"已完成任务",cardFocusMin:"专注分钟",cardStreak:"连续天数",cardExams:"即将考试",taskTitleLabel:"任务标题",taskSubjectLabel:"科目",taskDayLabel:"日期",taskDateLabel:"日期",taskPriorityLabel:"优先级",priorityLow:"低",priorityMedium:"中",priorityHigh:"高",addBtn:"添加",noTasks:"没有任务",toggleDone:"完成",toggleUndo:"撤销",deleteBtn:"删除",focusSubjectPlaceholder:"输入科目名称",minutesLabel:"分钟",startBtn:"开始",stopBtn:"停止",resetBtn:"重置",focusLogTitle:"最近的专注记录",noSessions:"暂无记录",examNameLabel:"科目 / 考试名称",examDateLabel:"考试日期",examTopicsLabel:"剩余主题数",examCreateBtn:"创建计划",noExams:"未添加考试",daysLeft:"天剩余",examExpired:"已过期",deletePlan:"删除计划",writingModeLabel:"书写方式",modePen:"手写笔 / 手指 / 触控笔",modeText:"键盘输入",paperTypeLabel:"纸张类型",paperLined:"横线",paperDot:"点阵",paperGrid:"方格",paperCornell:"康奈尔笔记",penColorLabel:"颜色",penSizeLabel:"粗细",fontSizeLabel:"字体大小",fontSmall:"小",fontMedium:"中",fontLarge:"大",pageTitlePlaceholder:"页面标题...",eraserBtn:"橡皮擦",clearPage:"清空页面",savePage:"保存页面",downloadImage:"下载图片",printPage:"打印",savedPagesTitle:"已保存页面",noSavedPages:"没有已保存的页面",subjectsNone:"添加任务或专注记录以查看科目",doneOfTotal:"已完成 {done}/{total} 个任务",minutesFocus:"{m} 分钟专注",heatmapTitle:"活动热力图（近12周）",heatLess:"少",heatMore:"多",footerText:"您的数据会自动保存",levelLabel:"等级 {n}",xpSub:"{x} / 100 分",streakLine:"连续：{n} 天",pageWeek:"每周看板",pageMonth:"月历",pageFocus:"专注模式",pageExams:"智能考试倒计时",pageNotes:"书写页面",pageSubjects:"科目",pageStats:"统计",noNoteTitle:"我的笔记",examMeta:"{topics} 个主题 · 考试日期 {date}"},
ja:{navWeek:"週間",navMonth:"月間",navFocus:"フォーカスモード",navExams:"試験カウントダウン",navNotes:"書き込みページ",navSubjects:"科目",navStats:"統計",colorLabel:"カラー",cardTasksDone:"完了タスク",cardFocusMin:"集中分数",cardStreak:"連続日数",cardExams:"近日試験",taskTitleLabel:"タスク名",taskSubjectLabel:"科目",taskDayLabel:"曜日",taskDateLabel:"日付",taskPriorityLabel:"優先度",priorityLow:"低",priorityMedium:"中",priorityHigh:"高",addBtn:"追加",noTasks:"タスクなし",toggleDone:"完了",toggleUndo:"元に戻す",deleteBtn:"削除",focusSubjectPlaceholder:"科目名を入力",minutesLabel:"分",startBtn:"開始",stopBtn:"停止",resetBtn:"リセット",focusLogTitle:"最近のセッション",noSessions:"まだセッションがありません",examNameLabel:"科目 / 試験名",examDateLabel:"試験日",examTopicsLabel:"残りのトピック数",examCreateBtn:"計画を作成",noExams:"試験は追加されていません",daysLeft:"日残り",examExpired:"終了",deletePlan:"計画を削除",writingModeLabel:"書き込み方法",modePen:"ペン / 手書き / スタイラス",modeText:"キーボード入力",paperTypeLabel:"用紙タイプ",paperLined:"罫線",paperDot:"ドット方眼",paperGrid:"方眼",paperCornell:"コーネル式",penColorLabel:"色",penSizeLabel:"太さ",fontSizeLabel:"文字サイズ",fontSmall:"小",fontMedium:"中",fontLarge:"大",pageTitlePlaceholder:"ページタイトル...",eraserBtn:"消しゴム",clearPage:"ページを消去",savePage:"ページを保存",downloadImage:"画像をダウンロード",printPage:"印刷",savedPagesTitle:"保存済みページ",noSavedPages:"保存済みページはありません",subjectsNone:"タスクを追加すると科目が表示されます",doneOfTotal:"{total}件中{done}件完了",minutesFocus:"集中{m}分",heatmapTitle:"アクティビティマップ（過去12週間）",heatLess:"少ない",heatMore:"多い",footerText:"データは自動的に保存されます",levelLabel:"レベル {n}",xpSub:"{x} / 100 ポイント",streakLine:"連続：{n}日",pageWeek:"週間ボード",pageMonth:"月間カレンダー",pageFocus:"フォーカスモード",pageExams:"スマート試験カウントダウン",pageNotes:"書き込みページ",pageSubjects:"科目",pageStats:"統計",noNoteTitle:"マイノート",examMeta:"{topics}トピック · 試験日{date}"}
};

let currentLang = 'ar';
function t(key, vars){
  let s = (I18N[currentLang] && I18N[currentLang][key]) || (I18N.en[key]) || key;
  if(vars) Object.keys(vars).forEach(k=>{ s = s.replace('{'+k+'}', vars[k]); });
  return s;
}
function applyStaticI18n(){
  document.querySelectorAll('[data-i18n]').forEach(el=>{ el.textContent = t(el.dataset.i18n); });
  document.querySelectorAll('[data-i18n-placeholder]').forEach(el=>{ el.placeholder = t(el.dataset.i18nPlaceholder); });
  document.getElementById('htmlRoot').dir = RTL_LANGS.includes(currentLang) ? 'rtl' : 'ltr';
  document.getElementById('htmlRoot').lang = currentLang;
  // rebuild day select options
  const taskDay = document.getElementById('taskDay');
  const prevVal = taskDay.value;
  taskDay.innerHTML = '';
  for(let i=0;i<7;i++){
    const opt = document.createElement('option');
    opt.value = i;
    opt.textContent = dayName(i);
    taskDay.appendChild(opt);
  }
  if(prevVal!=='') taskDay.value = prevVal;
}
function dayName(idx){
  const d = new Date(2024,0,7+idx); // a known Sunday=idx0.. adjust
  return new Intl.DateTimeFormat(LOCALE_MAP[currentLang]||'en', {weekday:'long'}).format(d);
}
function monthName(m,y){
  const d = new Date(y, m, 1);
  return new Intl.DateTimeFormat(LOCALE_MAP[currentLang]||'en', {month:'long'}).format(d);
}
function dowShort(idx){
  const d = new Date(2024,0,7+idx);
  return new Intl.DateTimeFormat(LOCALE_MAP[currentLang]||'en', {weekday:'short'}).format(d);
}

/* ================= App state ================= */
const THEMES = { rose:{accent:'#C98D89',light:'#F6E7E4'}, sage:{accent:'#8CA79A',light:'#E7EFEA'}, navy:{accent:'#3A4A6B',light:'#E7EBF2'}, gold:{accent:'#C9A24B',light:'#F5EEDC'} };
let tasks=[], sessions=[], exams=[], savedNotes=[];
let currentView='week';
let calMonth=new Date().getMonth(), calYear=new Date().getFullYear();
let timerInterval=null, timerSeconds=25*60, timerRunning=false;

function todayStr(){ return new Date().toISOString().slice(0,10); }
function setTheme(name){
  const th = THEMES[name]||THEMES.rose;
  document.documentElement.style.setProperty('--accent', th.accent);
  document.documentElement.style.setProperty('--accent-light', th.light);
  document.querySelectorAll('.swatch').forEach(s=>s.classList.toggle('selected', s.dataset.theme===name));
}
async function loadAll(){
  try{ const r=await window.storage.get('sp-tasks', false); tasks=r?JSON.parse(r.value):[]; }catch(e){ tasks=[]; }
  try{ const r=await window.storage.get('sp-sessions', false); sessions=r?JSON.parse(r.value):[]; }catch(e){ sessions=[]; }
  try{ const r=await window.storage.get('sp-exams', false); exams=r?JSON.parse(r.value):[]; }catch(e){ exams=[]; }
  try{ const r=await window.storage.get('sp-notes', false); savedNotes=r?JSON.parse(r.value):[]; }catch(e){ savedNotes=[]; }
  try{ const r=await window.storage.get('sp-theme', false); setTheme(r?r.value:'rose'); }catch(e){ setTheme('rose'); }
  try{ const r=await window.storage.get('sp-lang', false); currentLang = r?r.value:'ar'; }catch(e){ currentLang='ar'; }
  document.getElementById('langSelect').value = currentLang;
  applyStaticI18n();
  setPageTitle();
  renderAll();
  drawBackground(currentTpl);
  renderSavedNotes();
}
async function saveTasks(){ try{ await window.storage.set('sp-tasks', JSON.stringify(tasks), false); }catch(e){} }
async function saveSessions(){ try{ await window.storage.set('sp-sessions', JSON.stringify(sessions), false); }catch(e){} }
async function saveExams(){ try{ await window.storage.set('sp-exams', JSON.stringify(exams), false); }catch(e){} }
async function saveTheme(n){ try{ await window.storage.set('sp-theme', n, false); }catch(e){} }
async function saveNotesList(){ try{ await window.storage.set('sp-notes', JSON.stringify(savedNotes), false); }catch(e){} }
async function saveLang(n){ try{ await window.storage.set('sp-lang', n, false); }catch(e){} }

function escapeHtml(str){ const d=document.createElement('div'); d.textContent=str; return d.innerHTML; }

/* ---------- Gamification ---------- */
function computeXP(){ return tasks.filter(t=>t.done).length*10 + sessions.reduce((a,s)=>a+s.minutes,0)*2; }
function computeStreak(){
  const active=new Set();
  tasks.filter(t=>t.done&&t.date).forEach(t=>active.add(t.date));
  sessions.forEach(s=>active.add(s.date));
  let streak=0, d=new Date();
  while(true){ const ds=d.toISOString().slice(0,10); if(active.has(ds)){streak++; d.setDate(d.getDate()-1);} else break; }
  return streak;
}
function renderGamification(){
  const xp=computeXP(); const level=Math.floor(xp/100)+1; const inLevel=xp%100;
  document.getElementById('levelLabel').textContent = t('levelLabel',{n:level});
  document.getElementById('xpFill').style.width = inLevel+'%';
  document.getElementById('xpSub').textContent = t('xpSub',{x:inLevel});
  const streak = computeStreak();
  document.getElementById('streakLine').textContent = t('streakLine',{n:streak});
  document.getElementById('cardTasksDone').textContent = tasks.filter(t=>t.done).length;
  document.getElementById('cardFocusMin').textContent = sessions.reduce((a,s)=>a+s.minutes,0);
  document.getElementById('cardStreak').textContent = streak;
  document.getElementById('cardExams').textContent = exams.filter(e=>e.date>=todayStr()).length;
}

/* ---------- Week board (dayIndex 0-6, Sunday=0) ---------- */
function renderWeek(){
  const board=document.getElementById('board'); board.innerHTML='';
  for(let i=0;i<7;i++){
    const col=document.createElement('div'); col.className='day-col';
    const head=document.createElement('div'); head.className='day-head'; head.textContent=dayName(i); col.appendChild(head);
    const body=document.createElement('div'); body.className='day-body';
    const dayTasks=tasks.filter(x=>x.dayIndex===i);
    if(dayTasks.length===0){ const note=document.createElement('div'); note.className='empty-note'; note.textContent=t('noTasks'); body.appendChild(note); }
    else{
      dayTasks.forEach(tk=>{
        const card=document.createElement('div'); card.className='task-card priority-'+tk.priority+(tk.done?' done':'');
        card.innerHTML=`<div class="task-title">${escapeHtml(tk.title)}</div><div class="task-subject">${escapeHtml(tk.subject||'')}</div>
          <div class="task-actions"><button class="toggle">${tk.done?t('toggleUndo'):t('toggleDone')}</button><button class="del">${t('deleteBtn')}</button></div>`;
        card.querySelector('.toggle').onclick=()=>toggleTask(tk.id);
        card.querySelector('.del').onclick=()=>deleteTask(tk.id);
        body.appendChild(card);
      });
    }
    col.appendChild(body); board.appendChild(col);
  }
}

/* ---------- Month ---------- */
function renderMonth(){
  document.getElementById('monthLabel').textContent = `${monthName(calMonth,calYear)} ${calYear}`;
  const grid=document.getElementById('calendarGrid'); grid.innerHTML='';
  for(let i=0;i<7;i++){ const el=document.createElement('div'); el.className='cal-dow'; el.textContent=dowShort(i); grid.appendChild(el); }
  const firstDay=new Date(calYear,calMonth,1).getDay();
  const daysInMonth=new Date(calYear,calMonth+1,0).getDate();
  for(let i=0;i<firstDay;i++){ const e=document.createElement('div'); e.className='cal-cell empty'; grid.appendChild(e); }
  for(let d=1; d<=daysInMonth; d++){
    const cell=document.createElement('div'); cell.className='cal-cell';
    const dateStr=`${calYear}-${String(calMonth+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
    const num=document.createElement('div'); num.className='num'; num.textContent=d; cell.appendChild(num);
    tasks.filter(x=>x.date===dateStr).forEach(x=>{ const mt=document.createElement('div'); mt.className='mini-task'; mt.textContent=x.title; cell.appendChild(mt); });
    grid.appendChild(cell);
  }
}

/* ---------- Subjects ---------- */
function renderSubjects(){
  const grid=document.getElementById('subjectsGrid'); grid.innerHTML='';
  const map={};
  tasks.forEach(x=>{ const s=x.subject||'-'; if(!map[s]) map[s]={total:0,done:0,minutes:0}; map[s].total++; if(x.done) map[s].done++; });
  sessions.forEach(s=>{ const name=s.subject||'-'; if(!map[name]) map[name]={total:0,done:0,minutes:0}; map[name].minutes+=s.minutes; });
  const names=Object.keys(map);
  if(names.length===0){ grid.innerHTML = `<div class="empty-note">${t('subjectsNone')}</div>`; }
  else{
    names.forEach(name=>{
      const info=map[name]; const card=document.createElement('div'); card.className='subject-card';
      card.innerHTML=`<div class="sname">${escapeHtml(name)}</div><div class="scount">${t('doneOfTotal',{done:info.done,total:info.total})}</div><div class="stime">${t('minutesFocus',{m:info.minutes})}</div>`;
      grid.appendChild(card);
    });
  }
  const datalist=document.getElementById('subjectsDatalist');
  datalist.innerHTML='';
  names.forEach(n=>{ const opt=document.createElement('option'); opt.value=n; datalist.appendChild(opt); });
}

/* ---------- Focus timer ---------- */
function formatTime(sec){ const m=Math.floor(sec/60).toString().padStart(2,'0'); const s=(sec%60).toString().padStart(2,'0'); return `${m}:${s}`; }
document.getElementById('focusDuration').oninput=(e)=>{ if(!timerRunning){ let v=parseInt(e.target.value)||1; timerSeconds=v*60; document.getElementById('timerDisplay').textContent=formatTime(timerSeconds);} };
document.querySelectorAll('.preset-btns button').forEach(b=>{
  b.onclick=()=>{ document.getElementById('focusDuration').value=b.dataset.min; if(!timerRunning){ timerSeconds=parseInt(b.dataset.min)*60; document.getElementById('timerDisplay').textContent=formatTime(timerSeconds);} };
});
document.getElementById('startBtn').onclick=()=>{
  if(timerRunning){ clearInterval(timerInterval); timerRunning=false; document.getElementById('startBtn').textContent=t('startBtn'); }
  else{
    timerRunning=true; document.getElementById('startBtn').textContent=t('stopBtn');
    timerInterval=setInterval(()=>{
      timerSeconds--; document.getElementById('timerDisplay').textContent=formatTime(timerSeconds);
      if(timerSeconds<=0){ clearInterval(timerInterval); timerRunning=false; document.getElementById('startBtn').textContent=t('startBtn'); logFocusSession(); }
    },1000);
  }
};
document.getElementById('resetBtn').onclick=()=>{
  clearInterval(timerInterval); timerRunning=false; document.getElementById('startBtn').textContent=t('startBtn');
  timerSeconds=(parseInt(document.getElementById('focusDuration').value)||25)*60;
  document.getElementById('timerDisplay').textContent=formatTime(timerSeconds);
};
function logFocusSession(){
  const subject=document.getElementById('focusSubject').value.trim()||'-';
  const minutes=parseInt(document.getElementById('focusDuration').value)||25;
  sessions.push({date:todayStr(), subject, minutes});
  saveSessions(); renderFocusLog(); renderGamification(); renderSubjects();
}
function renderFocusLog(){
  const list=document.getElementById('focusLogList'); list.innerHTML='';
  const recent=sessions.slice(-8).reverse();
  if(recent.length===0){ list.innerHTML=`<div class="empty-note">${t('noSessions')}</div>`; return; }
  recent.forEach(s=>{ const row=document.createElement('div'); row.className='log-row'; row.innerHTML=`<span>${escapeHtml(s.subject)}</span><span>${s.minutes} ${t('minutesLabel')} · ${s.date}</span>`; list.appendChild(row); });
}

/* ---------- Exams ---------- */
function renderExams(){
  const list=document.getElementById('examList'); list.innerHTML='';
  if(exams.length===0){ list.innerHTML=`<div class="empty-note">${t('noExams')}</div>`; return; }
  exams.sort((a,b)=>a.date.localeCompare(b.date));
  exams.forEach(ex=>{
    const daysLeft=Math.ceil((new Date(ex.date)-new Date(todayStr()))/(1000*60*60*24));
    const card=document.createElement('div'); card.className='exam-card';
    card.innerHTML=`<div class="etitle">${escapeHtml(ex.name)}</div><div class="ecount">${daysLeft>=0?daysLeft+' '+t('daysLeft'):t('examExpired')}</div>
      <div class="emeta">${t('examMeta',{topics:ex.topics,date:ex.date})}</div>
      <button class="del">${t('deletePlan')}</button>`;
    card.querySelector('.del').onclick=()=>deleteExam(ex.id);
    list.appendChild(card);
  });
}
document.getElementById('examAddBtn').onclick=()=>{
  const name=document.getElementById('examName').value.trim();
  const date=document.getElementById('examDate').value;
  const topics=parseInt(document.getElementById('examTopics').value)||1;
  if(!name||!date) return;
  exams.push({id:Date.now().toString(), name, date, topics});
  const start=new Date(); start.setHours(0,0,0,0);
  const end=new Date(date);
  const dayCount=Math.max(1, Math.floor((end-start)/(1000*60*60*24)));
  const perDayGap=Math.max(1, Math.floor(dayCount/topics));
  for(let i=0;i<topics;i++){
    const d=new Date(start); d.setDate(d.getDate()+Math.min(dayCount-1, i*perDayGap+1));
    const dateStr=d.toISOString().slice(0,10);
    tasks.push({ id:Date.now().toString()+i, title:`${name} #${i+1}`, subject:name, dayIndex:d.getDay(), date:dateStr, priority:'high', done:false });
  }
  document.getElementById('examName').value=''; document.getElementById('examDate').value='';
  saveExams(); saveTasks(); renderAll();
};
function deleteExam(id){ exams=exams.filter(e=>e.id!==id); saveExams(); renderExams(); renderGamification(); }

/* ---------- Heatmap ---------- */
function renderHeatmap(){
  const grid=document.getElementById('heatmapGrid'); grid.innerHTML='';
  const activity={};
  tasks.filter(x=>x.done&&x.date).forEach(x=>{ activity[x.date]=(activity[x.date]||0)+1; });
  sessions.forEach(s=>{ activity[s.date]=(activity[s.date]||0)+1; });
  const days=84; const today=new Date(); const cellsByDate=[];
  for(let i=days-1;i>=0;i--){ const d=new Date(today); d.setDate(d.getDate()-i); cellsByDate.push(d.toISOString().slice(0,10)); }
  cellsByDate.forEach(ds=>{
    const count=activity[ds]||0;
    const cell=document.createElement('div'); cell.className='heat-cell';
    let opacity=0; if(count===1) opacity=0.35; else if(count===2) opacity=0.65; else if(count>=3) opacity=1;
    if(count>0){ cell.style.background='var(--accent)'; cell.style.opacity=opacity; }
    grid.appendChild(cell);
  });
}

/* ---------- Task CRUD ---------- */
function toggleTask(id){ const x=tasks.find(k=>k.id===id); if(x){ x.done=!x.done; saveTasks(); renderAll(); } }
function deleteTask(id){ tasks=tasks.filter(k=>k.id!==id); saveTasks(); renderAll(); }
document.getElementById('addBtn').onclick=()=>{
  const title=document.getElementById('taskTitle').value.trim();
  const subject=document.getElementById('taskSubject').value.trim();
  const dayIndex=parseInt(document.getElementById('taskDay').value);
  const date=document.getElementById('taskDate').value;
  const priority=document.getElementById('taskPriority').value;
  if(!title) return;
  tasks.push({id:Date.now().toString(), title, subject, dayIndex, date, priority, done:false});
  document.getElementById('taskTitle').value=''; document.getElementById('taskSubject').value=''; document.getElementById('taskDate').value='';
  saveTasks(); renderAll();
};

/* ---------- Notes: pen (pointer events) + keyboard text ---------- */
const bgCanvas=document.getElementById('bgCanvas');
const drawCanvas=document.getElementById('drawCanvas');
const bgCtx=bgCanvas.getContext('2d');
const drawCtx=drawCanvas.getContext('2d');
const canvasWrap=document.getElementById('canvasWrap');
let currentTpl='lined', currentMode='pen';
let penColor='#2C2A4A', penSize=2, isErasing=false, drawing=false, lastX=0, lastY=0, activePointerId=null;

function getAccent(){ return getComputedStyle(document.documentElement).getPropertyValue('--accent').trim()||'#C98D89'; }

function drawBackground(tpl){
  const w=bgCanvas.width, h=bgCanvas.height;
  bgCtx.clearRect(0,0,w,h); bgCtx.fillStyle='#FFFDF9'; bgCtx.fillRect(0,0,w,h);
  const accent=getAccent();
  bgCtx.fillStyle=accent; bgCtx.globalAlpha=0.5;
  bgCtx.beginPath(); bgCtx.arc(w-24,24,5,0,Math.PI*2); bgCtx.fill();
  bgCtx.beginPath(); bgCtx.arc(w-40,24,3,0,Math.PI*2); bgCtx.fill();
  bgCtx.globalAlpha=1;
  bgCtx.strokeStyle='#E7E1D6'; bgCtx.lineWidth=1.5;
  bgCtx.beginPath(); bgCtx.moveTo(24,70); bgCtx.lineTo(w-24,70); bgCtx.stroke();
  if(tpl==='lined'){
    bgCtx.strokeStyle='#E9E4D8'; bgCtx.lineWidth=1;
    for(let y=100;y<h-24;y+=32){ bgCtx.beginPath(); bgCtx.moveTo(24,y); bgCtx.lineTo(w-24,y); bgCtx.stroke(); }
  } else if(tpl==='dot'){
    bgCtx.fillStyle='#DCD5C4';
    for(let y=96;y<h-24;y+=24){ for(let x=24;x<w-24;x+=24){ bgCtx.beginPath(); bgCtx.arc(x,y,1.3,0,Math.PI*2); bgCtx.fill(); } }
  } else if(tpl==='grid'){
    bgCtx.strokeStyle='#EAE4D8'; bgCtx.lineWidth=1;
    for(let y=88;y<h-24;y+=24){ bgCtx.beginPath(); bgCtx.moveTo(24,y); bgCtx.lineTo(w-24,y); bgCtx.stroke(); }
    for(let x=24;x<w-24;x+=24){ bgCtx.beginPath(); bgCtx.moveTo(x,88); bgCtx.lineTo(x,h-24); bgCtx.stroke(); }
  } else if(tpl==='cornell'){
    bgCtx.strokeStyle=accent; bgCtx.lineWidth=1.5;
    bgCtx.beginPath(); bgCtx.moveTo(160,88); bgCtx.lineTo(160,h-140); bgCtx.stroke();
    bgCtx.beginPath(); bgCtx.moveTo(24,h-140); bgCtx.lineTo(w-24,h-140); bgCtx.stroke();
    bgCtx.strokeStyle='#EAE4D8';
    for(let y=110;y<h-140;y+=30){ bgCtx.beginPath(); bgCtx.moveTo(170,y); bgCtx.lineTo(w-24,y); bgCtx.stroke(); }
  }
  bgCtx.fillStyle='#2C2A4A'; bgCtx.font="26px 'Caveat', cursive"; bgCtx.textAlign='right';
  bgCtx.fillText(document.getElementById('noteTitle').value || t('noNoteTitle'), w-24, 50);
}
function clearDrawingLayer(){ drawCtx.clearRect(0,0,drawCanvas.width,drawCanvas.height); }
function getCanvasPos(e){
  const rect=drawCanvas.getBoundingClientRect();
  const scaleX=drawCanvas.width/rect.width, scaleY=drawCanvas.height/rect.height;
  return { x:(e.clientX-rect.left)*scaleX, y:(e.clientY-rect.top)*scaleY };
}
function getWrapCssPos(e){
  const rect=canvasWrap.getBoundingClientRect();
  return { x:e.clientX-rect.left, y:e.clientY-rect.top };
}

function pointerDownHandler(e){
  if(currentMode==='text'){ openTextInput(e); return; }
  drawing=true; activePointerId=e.pointerId;
  drawCanvas.setPointerCapture(e.pointerId);
  const p=getCanvasPos(e); lastX=p.x; lastY=p.y;
  e.preventDefault();
}
function pointerMoveHandler(e){
  if(!drawing || e.pointerId!==activePointerId) return;
  const p=getCanvasPos(e);
  drawCtx.strokeStyle = isErasing ? '#FFFDF9' : penColor;
  drawCtx.lineWidth = isErasing ? penSize*6 : penSize;
  drawCtx.lineCap='round';
  drawCtx.beginPath(); drawCtx.moveTo(lastX,lastY); drawCtx.lineTo(p.x,p.y); drawCtx.stroke();
  lastX=p.x; lastY=p.y;
  e.preventDefault();
}
function pointerUpHandler(e){ if(e.pointerId===activePointerId){ drawing=false; activePointerId=null; } }
drawCanvas.addEventListener('pointerdown', pointerDownHandler);
drawCanvas.addEventListener('pointermove', pointerMoveHandler);
drawCanvas.addEventListener('pointerup', pointerUpHandler);
drawCanvas.addEventListener('pointercancel', pointerUpHandler);
drawCanvas.addEventListener('pointerleave', pointerUpHandler);

function openTextInput(e){
  const pos=getWrapCssPos(e);
  const fontSize=parseInt(document.getElementById('fontSize').value)||22;
  const ta=document.createElement('textarea');
  ta.className='text-overlay-input';
  ta.style.left=pos.x+'px'; ta.style.top=pos.y+'px';
  ta.style.color=penColor; ta.style.fontSize=fontSize+'px';
  ta.dir = RTL_LANGS.includes(currentLang) ? 'rtl' : 'ltr';
  canvasWrap.appendChild(ta);
  ta.focus();
  function commit(){
    const val=ta.value;
    if(val.trim()!==''){
      const rectWrap=canvasWrap.getBoundingClientRect();
      const scaleX=drawCanvas.width/rectWrap.width, scaleY=drawCanvas.height/rectWrap.height;
      const cx=pos.x*scaleX, cy=pos.y*scaleY;
      drawCtx.fillStyle=penColor;
      drawCtx.font=(fontSize*scaleY)+"px 'Tajawal', sans-serif";
      drawCtx.textAlign = ta.dir==='rtl' ? 'right':'left';
      const lines=val.split('\n');
      const lineHeight=fontSize*scaleY*1.3;
      lines.forEach((line,i)=>{
        const anchorX = ta.dir==='rtl' ? cx+ (ta.offsetWidth*scaleX) : cx;
        drawCtx.fillText(line, anchorX, cy + lineHeight*(i+1));
      });
    }
    if(ta.parentNode) ta.parentNode.removeChild(ta);
  }
  ta.addEventListener('blur', commit, {once:true});
  ta.addEventListener('keydown', (ev)=>{ if(ev.key==='Escape'){ ta.removeEventListener('blur',commit); ta.remove(); } });
}

document.querySelectorAll('#modeOptions button').forEach(btn=>{
  btn.onclick=()=>{
    document.querySelectorAll('#modeOptions button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    currentMode=btn.dataset.mode;
    document.getElementById('penSizeWrap').classList.toggle('hidden', currentMode!=='pen');
    document.getElementById('fontSizeWrap').classList.toggle('hidden', currentMode!=='text');
    drawCanvas.style.cursor = currentMode==='text' ? 'text' : 'crosshair';
  };
});
document.querySelectorAll('#tplOptions button').forEach(btn=>{
  btn.onclick=()=>{
    document.querySelectorAll('#tplOptions button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active'); currentTpl=btn.dataset.tpl; drawBackground(currentTpl);
  };
});
document.querySelectorAll('.pen-swatch').forEach(sw=>{
  sw.onclick=()=>{ document.querySelectorAll('.pen-swatch').forEach(s=>s.classList.remove('selected')); sw.classList.add('selected'); penColor=sw.dataset.color; isErasing=false; document.getElementById('eraserBtn').classList.remove('primary'); };
});
document.getElementById('penSize').oninput=(e)=>{ penSize=parseInt(e.target.value); };
document.getElementById('eraserBtn').onclick=()=>{ isErasing=!isErasing; document.getElementById('eraserBtn').classList.toggle('primary', isErasing); };
document.getElementById('clearCanvasBtn').onclick=()=>{ clearDrawingLayer(); };
document.getElementById('noteTitle').addEventListener('input', ()=>drawBackground(currentTpl));

function mergedDataURL(){
  const tmp=document.createElement('canvas'); tmp.width=bgCanvas.width; tmp.height=bgCanvas.height;
  const tctx=tmp.getContext('2d'); tctx.drawImage(bgCanvas,0,0); tctx.drawImage(drawCanvas,0,0);
  return tmp.toDataURL('image/png');
}
document.getElementById('saveNoteBtn').onclick=()=>{
  const title=document.getElementById('noteTitle').value.trim()||t('noNoteTitle');
  savedNotes.unshift({id:Date.now().toString(), title, image:mergedDataURL(), date:todayStr()});
  if(savedNotes.length>30) savedNotes=savedNotes.slice(0,30);
  saveNotesList(); renderSavedNotes();
};
document.getElementById('downloadNoteBtn').onclick=()=>{
  const link=document.createElement('a'); const title=document.getElementById('noteTitle').value.trim()||t('noNoteTitle');
  link.download=title+'.png'; link.href=mergedDataURL(); link.click();
};
document.getElementById('printNoteBtn').onclick=()=>{
  const dataUrl=mergedDataURL(); const w=window.open('','_blank');
  w.document.write(`<html><head><title>Print</title></head><body style="margin:0"><img src="${dataUrl}" style="width:100%"></body></html>`);
  w.document.close(); w.onload=()=>w.print();
};
function renderSavedNotes(){
  const list=document.getElementById('savedNotesList'); list.innerHTML='';
  if(savedNotes.length===0){ list.innerHTML=`<div class="empty-note">${t('noSavedPages')}</div>`; return; }
  savedNotes.forEach(n=>{
    const thumb=document.createElement('div'); thumb.className='saved-thumb';
    thumb.innerHTML=`<img src="${n.image}"><div class="cap"><span>${escapeHtml(n.title)}</span><button class="del">${t('deleteBtn')}</button></div>`;
    thumb.querySelector('img').onclick=()=>{
      const img=new Image(); img.onload=()=>{ bgCtx.clearRect(0,0,bgCanvas.width,bgCanvas.height); bgCtx.drawImage(img,0,0); clearDrawingLayer(); }; img.src=n.image;
    };
    thumb.querySelector('.del').onclick=(ev)=>{ ev.stopPropagation(); savedNotes=savedNotes.filter(x=>x.id!==n.id); saveNotesList(); renderSavedNotes(); };
    list.appendChild(thumb);
  });
}

/* ---------- Nav, theme, language ---------- */
function setPageTitle(){
  const map={ week:'pageWeek', month:'pageMonth', focus:'pageFocus', exams:'pageExams', notes:'pageNotes', subjects:'pageSubjects', stats:'pageStats' };
  document.getElementById('pageTitle').textContent = t(map[currentView]);
}
document.querySelectorAll('.nav button').forEach(btn=>{
  btn.onclick=()=>{
    document.querySelectorAll('.nav button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    currentView=btn.dataset.view;
    ['week','month','focus','exams','notes','subjects','stats'].forEach(v=>document.getElementById('view-'+v).classList.toggle('hidden', currentView!==v));
    setPageTitle();
    if(currentView==='notes') drawBackground(currentTpl);
  };
});
document.querySelectorAll('.swatch').forEach(sw=>{
  sw.onclick=()=>{ setTheme(sw.dataset.theme); saveTheme(sw.dataset.theme); renderHeatmap(); drawBackground(currentTpl); };
});
document.getElementById('langSelect').onchange=(e)=>{
  currentLang=e.target.value; saveLang(currentLang);
  applyStaticI18n(); setPageTitle(); renderAll(); drawBackground(currentTpl); renderSavedNotes();
};
document.getElementById('prevMonth').onclick=()=>{ calMonth--; if(calMonth<0){calMonth=11; calYear--;} renderMonth(); };
document.getElementById('nextMonth').onclick=()=>{ calMonth++; if(calMonth>11){calMonth=0; calYear++;} renderMonth(); };

function renderAll(){ renderWeek(); renderMonth(); renderSubjects(); renderFocusLog(); renderExams(); renderHeatmap(); renderGamification(); }

function updateTodayBadge(){
  document.getElementById('todayBadge').textContent = new Intl.DateTimeFormat(LOCALE_MAP[currentLang]||'en', {weekday:'long', year:'numeric', month:'long', day:'numeric'}).format(new Date());
}
const _origApplyStaticI18n = applyStaticI18n;
applyStaticI18n = function(){ _origApplyStaticI18n(); updateTodayBadge(); };

document.getElementById('timerDisplay').textContent = formatTime(timerSeconds);
loadAll();
</script>
</body>
</html>
# Student-panner
