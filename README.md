<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TO THE MATH - 스마트 암산 연습기</title>
<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :root {
    --primary-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
    --secondary-gradient: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
    --accent-color: #667eea;
    --text-primary: #2c3e50;
    --text-secondary: #7f8c8d;
    --bg-primary: #ffffff;
    --bg-secondary: #f8f9fa;
    --bg-tertiary: rgba(255, 255, 255, 0.95);
    --border-color: #e0e6ed;
    --shadow-light: 0 10px 30px rgba(0,0,0,0.1);
    --shadow-medium: 0 20px 60px rgba(0,0,0,0.15);
    --shadow-heavy: 0 30px 80px rgba(0,0,0,0.2);
    --glass-bg: rgba(255, 255, 255, 0.1);
    --glass-border: rgba(255, 255, 255, 0.2);
  }

  [data-theme="dark"] {
    --primary-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
    --secondary-gradient: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
    --accent-color: #667eea;
    --text-primary: #ecf0f1;
    --text-secondary: #bdc3c7;
    --bg-primary: #1a1a1a;
    --bg-secondary: #2c2c2c;
    --bg-tertiary: rgba(44, 44, 44, 0.95);
    --border-color: #34495e;
    --shadow-light: 0 10px 30px rgba(0,0,0,0.3);
    --shadow-medium: 0 20px 60px rgba(0,0,0,0.4);
    --shadow-heavy: 0 30px 80px rgba(0,0,0,0.5);
    --glass-bg: rgba(0, 0, 0, 0.2);
    --glass-border: rgba(255, 255, 255, 0.1);
  }

  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background: var(--bg-primary);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    color: var(--text-primary);
    transition: all 0.3s ease;
    position: relative;
    overflow-x: hidden;
  }

  body::before {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--primary-gradient);
    opacity: 0.1;
    z-index: -1;
  }

  .cosmic-bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -2;
    background: radial-gradient(circle at 20% 80%, rgba(102, 126, 234, 0.3) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(118, 75, 162, 0.3) 0%, transparent 50%),
                radial-gradient(circle at 40% 40%, rgba(240, 147, 251, 0.2) 0%, transparent 50%);
    animation: cosmicShift 20s ease-in-out infinite;
  }

  @keyframes cosmicShift {
    0%, 100% { transform: rotate(0deg) scale(1); }
    50% { transform: rotate(180deg) scale(1.1); }
  }

  .floating-particles {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: -1;
  }

  .particle {
    position: absolute;
    width: 4px;
    height: 4px;
    background: var(--accent-color);
    border-radius: 50%;
    opacity: 0.3;
    animation: float 15s infinite ease-in-out;
  }

  .particle:nth-child(1) { top: 10%; left: 10%; animation-delay: 0s; }
  .particle:nth-child(2) { top: 20%; left: 80%; animation-delay: 2s; }
  .particle:nth-child(3) { top: 80%; left: 20%; animation-delay: 4s; }
  .particle:nth-child(4) { top: 60%; left: 60%; animation-delay: 6s; }
  .particle:nth-child(5) { top: 30%; left: 40%; animation-delay: 8s; }

  @keyframes float {
    0%, 100% { transform: translateY(0px) translateX(0px); }
    25% { transform: translateY(-30px) translateX(20px); }
    50% { transform: translateY(-60px) translateX(-20px); }
    75% { transform: translateY(-30px) translateX(10px); }
  }

  .header {
    background: var(--primary-gradient);
    color: white;
    padding: 40px 60px;
    border-radius: 30px;
    font-size: 48px;
    font-weight: 900;
    letter-spacing: 4px;
    text-shadow: 0 4px 8px rgba(0,0,0,0.3);
    box-shadow: var(--shadow-heavy);
    margin-bottom: 40px;
    position: relative;
    overflow: hidden;
    backdrop-filter: blur(10px);
  }

  .header::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
    animation: shimmer 3s infinite;
  }

  @keyframes shimmer {
    0% { left: -100%; }
    100% { left: 100%; }
  }

  .theme-toggle {
    position: fixed;
    top: 30px;
    right: 30px;
    z-index: 1000;
    background: var(--glass-bg);
    border: 2px solid var(--glass-border);
    border-radius: 50px;
    padding: 15px 20px;
    cursor: pointer;
    backdrop-filter: blur(20px);
    transition: all 0.3s ease;
    box-shadow: var(--shadow-light);
  }

  .theme-toggle:hover {
    transform: scale(1.05);
    box-shadow: var(--shadow-medium);
  }

  .theme-toggle-icon {
    font-size: 24px;
    transition: transform 0.3s ease;
  }

  .container {
    max-width: 900px;
    width: 100%;
    background: var(--bg-tertiary);
    backdrop-filter: blur(20px);
    padding: 60px;
    border-radius: 40px;
    box-shadow: var(--shadow-heavy);
    border: 2px solid var(--glass-border);
    position: relative;
    overflow: hidden;
  }

  .container::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: conic-gradient(from 0deg, transparent, var(--accent-color), transparent);
    opacity: 0.03;
    animation: rotate 20s linear infinite;
  }

  @keyframes rotate {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  .settings {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 30px;
    margin-bottom: 50px;
    padding: 40px;
    background: var(--glass-bg);
    border-radius: 25px;
    border: 2px solid var(--glass-border);
    box-shadow: var(--shadow-light);
    backdrop-filter: blur(15px);
    position: relative;
    z-index: 1;
  }

  .setting-group {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .setting-group label {
    font-weight: 700;
    color: var(--text-primary);
    font-size: 18px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .setting-group input, .setting-group select {
    padding: 16px 20px;
    border: 2px solid var(--border-color);
    border-radius: 15px;
    font-size: 16px;
    background: var(--bg-primary);
    color: var(--text-primary);
    transition: all 0.3s ease;
    box-shadow: var(--shadow-light);
  }

  .setting-group input:focus, .setting-group select:focus {
    outline: none;
    border-color: var(--accent-color);
    box-shadow: 0 0 0 4px rgba(102, 126, 234, 0.2);
    transform: translateY(-2px);
  }

  .digits-selector {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
  }

  .digit-btn {
    padding: 12px 20px;
    border: 2px solid var(--border-color);
    border-radius: 12px;
    background: var(--bg-primary);
    color: var(--text-primary);
    cursor: pointer;
    transition: all 0.3s ease;
    font-weight: 600;
    min-width: 80px;
    text-align: center;
  }

  .digit-btn:hover {
    border-color: var(--accent-color);
    transform: translateY(-2px);
    box-shadow: var(--shadow-light);
  }

  .digit-btn.active {
    background: var(--accent-color);
    color: white;
    border-color: var(--accent-color);
  }

  .controls {
    display: flex;
    justify-content: center;
    gap: 25px;
    margin: 50px 0;
    flex-wrap: wrap;
    position: relative;
    z-index: 1;
  }

  .btn {
    background: var(--primary-gradient);
    color: white;
    border: none;
    padding: 20px 40px;
    font-size: 18px;
    font-weight: 700;
    cursor: pointer;
    border-radius: 20px;
    transition: all 0.3s ease;
    box-shadow: var(--shadow-medium);
    position: relative;
    overflow: hidden;
    min-width: 150px;
  }

  .btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
    transition: left 0.5s;
  }

  .btn:hover::before {
    left: 100%;
  }

  .btn:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-heavy);
  }

  .btn:active {
    transform: translateY(-2px);
  }

  .btn-success {
    background: linear-gradient(135deg, #27ae60 0%, #2ecc71 100%);
  }

  .btn-danger {
    background: linear-gradient(135deg, #e74c3c 0%, #c0392b 100%);
  }

  .btn-info {
    background: linear-gradient(135deg, #3498db 0%, #2980b9 100%);
  }

  .problem-area {
    background: var(--glass-bg);
    padding: 50px;
    border-radius: 30px;
    margin: 40px 0;
    border: 2px solid var(--glass-border);
    box-shadow: var(--shadow-medium);
    backdrop-filter: blur(15px);
    position: relative;
    z-index: 1;
    text-align: center;
  }

  .problem-display {
    font-size: 72px;
    font-weight: 900;
    margin: 30px 0;
    background: var(--primary-gradient);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-shadow: 0 2px 4px rgba(0,0,0,0.1);
    letter-spacing: 3px;
    line-height: 1.2;
  }

  .answer-input {
    font-size: 36px;
    padding: 25px 35px;
    border: 3px solid var(--border-color);
    border-radius: 20px;
    width: 300px;
    text-align: center;
    margin: 30px 0;
    background: var(--bg-primary);
    color: var(--text-primary);
    font-weight: 700;
    transition: all 0.3s ease;
    box-shadow: var(--shadow-light);
  }

  .answer-input:focus {
    outline: none;
    border-color: var(--accent-color);
    box-shadow: 0 0 0 6px rgba(102, 126, 234, 0.2);
    transform: translateY(-3px);
  }

  .timer-container {
    margin: 30px 0;
    position: relative;
  }

  .timer-bar {
    width: 100%;
    height: 40px;
    background: var(--bg-secondary);
    border-radius: 25px;
    overflow: hidden;
    box-shadow: inset 0 4px 8px rgba(0,0,0,0.1);
    border: 2px solid var(--border-color);
    position: relative;
  }

  .timer-fill {
    height: 100%;
    background: var(--primary-gradient);
    width: 100%;
    transition: width 0.1s linear;
    border-radius: 25px;
    position: relative;
    box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
  }

  .timer-fill::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 60%;
    background: linear-gradient(to bottom, rgba(255,255,255,0.4), transparent);
    border-radius: 25px 25px 0 0;
  }

  .timer-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-weight: 700;
    font-size: 16px;
    text-shadow: 0 2px 4px rgba(0,0,0,0.5);
    z-index: 1;
  }

  .result-display {
    font-size: 28px;
    margin-top: 25px;
    font-weight: 700;
    padding: 20px;
    border-radius: 15px;
    min-height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
  }

  .result-display.correct {
    background: linear-gradient(135deg, rgba(39, 174, 96, 0.15) 0%, rgba(46, 204, 113, 0.15) 100%);
    color: #27ae60;
    border: 2px solid rgba(39, 174, 96, 0.3);
    box-shadow: 0 10px 30px rgba(39, 174, 96, 0.2);
  }

  .result-display.incorrect {
    background: linear-gradient(135deg, rgba(231, 76, 60, 0.15) 0%, rgba(192, 57, 43, 0.15) 100%);
    color: #e74c3c;
    border: 2px solid rgba(231, 76, 60, 0.3);
    box-shadow: 0 10px 30px rgba(231, 76, 60, 0.2);
  }

  .statistics {
    margin-top: 50px;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 30px;
    padding: 40px;
    background: var(--glass-bg);
    border-radius: 25px;
    border: 2px solid var(--glass-border);
    box-shadow: var(--shadow-medium);
    backdrop-filter: blur(15px);
    position: relative;
    z-index: 1;
  }

  .stat-card {
    text-align: center;
    padding: 30px;
    background: var(--bg-primary);
    border-radius: 20px;
    box-shadow: var(--shadow-light);
    border: 2px solid var(--border-color);
    transition: transform 0.3s ease;
  }

  .stat-card:hover {
    transform: translateY(-5px);
    box-shadow: var(--shadow-medium);
  }

  .stat-number {
    font-size: 42px;
    font-weight: 900;
    color: var(--accent-color);
    margin-bottom: 10px;
    background: var(--primary-gradient);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .stat-label {
    font-size: 16px;
    color: var(--text-secondary);
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .difficulty-selector {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: 15px;
    margin-top: 20px;
  }

  .difficulty-btn {
    padding: 15px 20px;
    border: 2px solid var(--border-color);
    border-radius: 15px;
    background: var(--bg-primary);
    color: var(--text-primary);
    cursor: pointer;
    transition: all 0.3s ease;
    font-weight: 600;
    text-align: center;
    font-size: 14px;
  }

  .difficulty-btn:hover {
    border-color: var(--accent-color);
    transform: translateY(-2px);
    box-shadow: var(--shadow-light);
  }

  .difficulty-btn.active {
    background: var(--accent-color);
    color: white;
    border-color: var(--accent-color);
  }

  .modal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.7);
    backdrop-filter: blur(10px);
    z-index: 2000;
    align-items: center;
    justify-content: center;
  }

  .modal-content {
    background: var(--bg-primary);
    padding: 40px;
    border-radius: 25px;
    max-width: 600px;
    width: 90%;
    max-height: 80vh;
    overflow-y: auto;
    box-shadow: var(--shadow-heavy);
    border: 2px solid var(--border-color);
    position: relative;
  }

  .modal-close {
    position: absolute;
    top: 20px;
    right: 25px;
    font-size: 30px;
    cursor: pointer;
    color: var(--text-secondary);
    transition: color 0.3s ease;
  }

  .modal-close:hover {
    color: var(--text-primary);
  }

  .streak-indicator {
    position: absolute;
    top: 20px;
    left: 20px;
    background: var(--primary-gradient);
    color: white;
    padding: 10px 20px;
    border-radius: 20px;
    font-weight: 700;
    font-size: 14px;
    box-shadow: var(--shadow-light);
  }

  @media (max-width: 768px) {
    .container {
      padding: 40px 25px;
      margin: 20px;
    }
    
    .settings {
      grid-template-columns: 1fr;
      padding: 30px;
    }
    
    .controls {
      flex-direction: column;
      align-items: center;
    }
    
    .problem-display {
      font-size: 48px;
    }
    
    .answer-input {
      width: 250px;
      font-size: 28px;
    }
    
    .header {
      font-size: 32px;
      padding: 30px 40px;
    }
    
    .theme-toggle {
      top: 20px;
      right: 20px;
    }
  }
</style>
</head>
<body>
  <div class="cosmic-bg"></div>
  <div class="floating-particles">
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
  </div>

  <div class="theme-toggle" onclick="toggleTheme()">
    <span class="theme-toggle-icon">🌙</span>
  </div>

  <div class="streak-indicator" id="streakIndicator">
    🔥 연속 정답: 0
  </div>

  <header class="header">
    TO THE MATH
  </header>

  <div class="container">
    <div class="settings">
      <div class="setting-group">
        <label>⏱️ 문제 간격 (초)</label>
        <input type="number" id="timeInterval" value="3.0" min="0.5" max="30" step="0.1">
      </div>
      
      <div class="setting-group">
        <label>🔢 자릿수 선택</label>
        <div class="digits-selector">
          <div class="digit-btn" data-digits="1">1 (한자리)</div>
          <div class="digit-btn active" data-digits="2">2 (두자리)</div>
          <div class="digit-btn" data-digits="3">3 (세자리)</div>
          <div class="digit-btn" data-digits="4">4 (네자리)</div>
        </div>
      </div>
      
      <div class="setting-group">
        <label>➕ 연산 종류</label>
        <select id="operationType">
          <option value="add">➕ 더하기</option>
          <option value="subtract">➖ 빼기</option>
          <option value="multiply">✖️ 곱하기</option>
          <option value="divide">➗ 나누기</option>
          <option value="mixed">🔄 혼합</option>
        </select>
      </div>

      <div class="setting-group">
        <label>🎯 난이도 설정</label>
        <div class="difficulty-selector">
          <div class="difficulty-btn" data-difficulty="easy">쉬움</div>
          <div class="difficulty-btn active" data-difficulty="normal">보통</div>
          <div class="difficulty-btn" data-difficulty="hard">어려움</div>
          <div class="difficulty-btn" data-difficulty="expert">전문가</div>
        </div>
      </div>
    </div>

    <div class="controls">
      <button class="btn btn-success" onclick="startPractice()">🚀 연습 시작</button>
      <button class="btn btn-danger" onclick="stopPractice()">⏹️ 연습 중단</button>
      <button class="btn btn-info" onclick="resetStats()">🔄 통계 초기화</button>
      <button class="btn" onclick="showHelp()">❓ 도움말</button>
    </div>

    <div class="problem-area">
      <div class="problem-display" id="problemDisplay">시작 버튼을 눌러주세요</div>
      <input type="text" class="answer-input" id="answerInput" placeholder="답을 입력하세요" disabled>
      
      <div class="timer-container">
        <div class="timer-bar">
          <div class="timer-fill" id="timerFill"></div>
          <div class="timer-text" id="timerText">준비하세요</div>
        </div>
      </div>
      
      <div class="result-display" id="resultDisplay"></div>
    </div>

    <div class="statistics">
      <div class="stat-card">
        <div class="stat-number" id="totalProblems">0</div>
        <div class="stat-label">총 문제</div>
      </div>
      <div class="stat-card">
        <div class="stat-number" id="correctAnswers">0</div>
        <div class="stat-label">정답 수</div>
      </div>
      <div class="stat-card">
        <div class="stat-number" id="accuracyRate">0%</div>
        <div class="stat-label">정답률</div>
      </div>
      <div class="stat-card">
        <div class="stat-number" id="averageTime">0.0s</div>
        <div class="stat-label">평균 시간</div>
      </div>
    </div>
  </div>

  <div class="modal" id="helpModal">
    <div class="modal-content">
      <span class="modal-close" onclick="closeHelp()">&times;</span>
      <h2>📚 사용법 안내</h2>
      <h3>🎯 기본 사용법</h3>
      <p>1. 원하는 설정을 선택하세요 (시간 간격, 자릿수, 연산 종류, 난이도)</p>
      <p>2. "연습 시작" 버튼을 클릭하세요</p>
      <p>3. 문제가 나타나면 답을 입력하고 Enter를 누르세요</p>
      <p>4. 시간이 초과되면 자동으로 다음 문제로 넘어갑니다</p>
      
      <h3>🔧 설정 옵션</h3>
      <p><strong>자릿수:</strong> 1(한자리)부터 4(네자리)까지 선택 가능</p>
      <p><strong>연산 종류:</strong> 더하기, 빼기, 곱하기, 나누기, 혼합 중 선택</p>
      <p><strong>난이도:</strong> 쉬움(큰 수), 보통(중간 수), 어려움(작은 수), 전문가(복잡한 계산)</p>
      
      <h3>🎨 테마 설정</h3>
      <p>우측 상단의 달/해 아이콘을 클릭하여 다크/라이트 모드를 전환할 수 있습니다.</p>
      
      <h3>📊 통계 정보</h3>
      <p>총 문제 수, 정답 수, 정답률, 평균 응답 시간을 실시간으로 확인할 수 있습니다.</p>
    </div>
  </div>

  <script>
    // 전역 변수
    let currentProblem = null;
    let currentAnswer = null;
    let practiceTimer = null;
    let countdownTimer = null;
    let isRunning = false;
    let selectedDigits = 2;
    let selectedDifficulty = 'normal';
    let currentStreak = 0;
    
    // 통계 변수
    let stats = {
      total: 0,
      correct: 0,
      totalTime: 0,
      responses: []
    };

    // 테마 토글
    function toggleTheme() {
      const body = document.body;
      const themeIcon = document.querySelector('.theme-toggle-icon');
      
      if (body.getAttribute('data-theme') === 'dark') {
        body.removeAttribute('data-theme');
        themeIcon.textContent = '🌙';
        localStorage.setItem('theme', 'light');
      } else {
        body.setAttribute('data-theme', 'dark');
        themeIcon.textContent = '☀️';
        localStorage.setItem('theme', 'dark');
      }
    }

    
    // 테마 초기화
    function initTheme() {
      const savedTheme = localStorage.getItem('theme');
      const body = document.body;
      const themeIcon = document.querySelector('.theme-toggle-icon');
      
      if (savedTheme === 'dark') {
        body.setAttribute('data-theme', 'dark');
        themeIcon.textContent = '☀️';
      } else {
        body.removeAttribute('data-theme');
        themeIcon.textContent = '🌙';
      }
    }

    // 자릿수 선택
    function selectDigits(digits) {
      selectedDigits = parseInt(digits);
      document.querySelectorAll('.digit-btn').forEach(btn => {
        btn.classList.remove('active');
      });
      document.querySelector(`[data-digits="${digits}"]`).classList.add('active');
    }

    // 난이도 선택
    function selectDifficulty(difficulty) {
      selectedDifficulty = difficulty;
      document.querySelectorAll('.difficulty-btn').forEach(btn => {
        btn.classList.remove('active');
      });
      document.querySelector(`[data-difficulty="${difficulty}"]`).classList.add('active');
    }

    function getNumberRange(digits, difficulty) {
  const baseMin = Math.pow(10, digits - 1);      // 예: 100
  const baseMax = Math.pow(10, digits) - 1;      // 예: 999
  const rangeSize = baseMax - baseMin + 1;

  switch (difficulty) {
    case 'easy':
      // 자리수는 맞지만, 최소값 근처 작은 숫자
      return {
        min: baseMin,
        max: baseMin + Math.floor(rangeSize * 0.1) // 상위 10% 이내
      };
    case 'normal':
      // 중간대
      const midStart = baseMin + Math.floor(rangeSize * 0.4);
      return {
        min: midStart,
        max: midStart + Math.floor(rangeSize * 0.2)
      };
    case 'hard':
      // 높은 숫자대
      const highStart = baseMin + Math.floor(rangeSize * 0.8);
      return {
        min: highStart,
        max: baseMax
      };
    case 'expert':
    default:
      // 전체 범위
      return {
        min: baseMin,
        max: baseMax
      };
  }
}


    // 랜덤 숫자 생성
    function generateRandomNumber(digits, difficulty) {
      const range = getNumberRange(digits, difficulty);
      return Math.floor(Math.random() * (range.max - range.min + 1)) + range.min;
    }

    // 문제 생성
    function generateProblem() {
      const operationType = document.getElementById('operationType').value;
      let operation;
      
      if (operationType === 'mixed') {
        const operations = ['add', 'subtract', 'multiply', 'divide'];
        operation = operations[Math.floor(Math.random() * operations.length)];
      } else {
        operation = operationType;
      }

      let num1, num2, answer, display;
      
      switch (operation) {
        case 'add':
          num1 = generateRandomNumber(selectedDigits, selectedDifficulty);
          num2 = generateRandomNumber(selectedDigits, selectedDifficulty);
          answer = num1 + num2;
          display = `${num1} + ${num2}`;
          break;
          
        case 'subtract':
          num1 = generateRandomNumber(selectedDigits, selectedDifficulty);
          num2 = generateRandomNumber(selectedDigits, selectedDifficulty);
          if (num1 < num2) [num1, num2] = [num2, num1]; // 음수 방지
          answer = num1 - num2;
          display = `${num1} - ${num2}`;
          break;
          
        case 'multiply':
          const smallerDigits = Math.max(1, selectedDigits - 1);
          num1 = generateRandomNumber(selectedDigits, selectedDifficulty);
          num2 = generateRandomNumber(smallerDigits, selectedDifficulty);
          answer = num1 * num2;
          display = `${num1} × ${num2}`;
          break;
          
        case 'divide':
          num2 = generateRandomNumber(Math.max(1, selectedDigits - 1), selectedDifficulty);
          answer = generateRandomNumber(selectedDigits, selectedDifficulty);
          num1 = num2 * answer;
          display = `${num1} ÷ ${num2}`;
          break;
      }

      return { display, answer };
    }

    // 연습 시작
    function startPractice() {
      if (isRunning) return;
      
      isRunning = true;
      document.getElementById('answerInput').disabled = false;
      document.getElementById('answerInput').focus();
      
      nextProblem();
    }

    // 연습 중단
    function stopPractice() {
      isRunning = false;
      clearTimeout(practiceTimer);
      clearInterval(countdownTimer);
      
      document.getElementById('problemDisplay').textContent = '시작 버튼을 눌러주세요';
      document.getElementById('answerInput').disabled = true;
      document.getElementById('answerInput').value = '';
      document.getElementById('resultDisplay').textContent = '';
      document.getElementById('timerText').textContent = '준비하세요';
      document.getElementById('timerFill').style.width = '100%';
    }

    // 다음 문제
    function nextProblem() {
      if (!isRunning) return;
      
      const problem = generateProblem();
      currentProblem = problem.display;
      currentAnswer = problem.answer;
      
      document.getElementById('problemDisplay').textContent = currentProblem;
      document.getElementById('answerInput').value = '';
      document.getElementById('resultDisplay').textContent = '';
      document.getElementById('answerInput').focus();
      
      startTimer();
    }

    // 타이머 시작
    function startTimer() {
      const timeLimit = parseFloat(document.getElementById('timeInterval').value) * 1000;
      const startTime = Date.now();
      
      document.getElementById('timerText').textContent = `${(timeLimit / 1000).toFixed(1)}초`;
      
      countdownTimer = setInterval(() => {
        const elapsed = Date.now() - startTime;
        const remaining = Math.max(0, timeLimit - elapsed);
        const percentage = (remaining / timeLimit) * 100;
        
        document.getElementById('timerFill').style.width = `${percentage}%`;
        document.getElementById('timerText').textContent = `${(remaining / 1000).toFixed(1)}초`;
        
        if (remaining <= 0) {
          clearInterval(countdownTimer);
          handleTimeout();
        }
      }, 100);
    }

    // 시간 초과 처리
    function handleTimeout() {
      const resultDisplay = document.getElementById('resultDisplay');
      resultDisplay.textContent = `시간 초과! 정답: ${currentAnswer}`;
      resultDisplay.className = 'result-display incorrect';
      
      stats.total++;
      stats.responses.push({ time: parseFloat(document.getElementById('timeInterval').value), correct: false });
      currentStreak = 0;
      
      updateStats();
      updateStreak();
      
      practiceTimer = setTimeout(() => {
        nextProblem();
      }, 1500);
    }

    // 답안 체크
    function checkAnswer() {
      const userAnswer = parseInt(document.getElementById('answerInput').value);
      const resultDisplay = document.getElementById('resultDisplay');
      
      if (isNaN(userAnswer)) return;
      
      clearInterval(countdownTimer);
      clearTimeout(practiceTimer);
      
      const timeLimit = parseFloat(document.getElementById('timeInterval').value);
      const timerFill = document.getElementById('timerFill');
      const remainingPercentage = parseFloat(timerFill.style.width);
      const timeUsed = timeLimit * (1 - remainingPercentage / 100);
      
      stats.total++;
      stats.totalTime += timeUsed;
      stats.responses.push({ time: timeUsed, correct: userAnswer === currentAnswer });
      
      if (userAnswer === currentAnswer) {
        resultDisplay.textContent = '정답입니다! 🎉';
        resultDisplay.className = 'result-display correct';
        stats.correct++;
        currentStreak++;
      } else {
        resultDisplay.textContent = `오답! 정답: ${currentAnswer}`;
        resultDisplay.className = 'result-display incorrect';
        currentStreak = 0;
      }
      
      updateStats();
      updateStreak();
      
      practiceTimer = setTimeout(() => {
        nextProblem();
      }, 1500);
    }

    // 통계 업데이트
    function updateStats() {
      document.getElementById('totalProblems').textContent = stats.total;
      document.getElementById('correctAnswers').textContent = stats.correct;
      
      const accuracy = stats.total > 0 ? (stats.correct / stats.total * 100).toFixed(1) : 0;
      document.getElementById('accuracyRate').textContent = `${accuracy}%`;
      
      const avgTime = stats.responses.length > 0 ? 
        (stats.totalTime / stats.responses.length).toFixed(1) : 0;
      document.getElementById('averageTime').textContent = `${avgTime}s`;
    }

    // 연속 정답 업데이트
    function updateStreak() {
      document.getElementById('streakIndicator').textContent = `🔥 연속 정답: ${currentStreak}`;
    }

    // 통계 초기화
    function resetStats() {
      if (confirm('통계를 초기화하시겠습니까?')) {
        stats = {
          total: 0,
          correct: 0,
          totalTime: 0,
          responses: []
        };
        currentStreak = 0;
        updateStats();
        updateStreak();
      }
    }

    // 도움말 표시
    function showHelp() {
      document.getElementById('helpModal').style.display = 'flex';
    }

    // 도움말 닫기
    function closeHelp() {
      document.getElementById('helpModal').style.display = 'none';
    }

    // 이벤트 리스너
    document.addEventListener('DOMContentLoaded', function() {
      initTheme();
      
      // 자릿수 선택 이벤트
      document.querySelectorAll('.digit-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          selectDigits(btn.dataset.digits);
        });
      });
      
      // 난이도 선택 이벤트
      document.querySelectorAll('.difficulty-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          selectDifficulty(btn.dataset.difficulty);
        });
      });
      
      // 답안 입력 이벤트
      document.getElementById('answerInput').addEventListener('keypress', function(e) {
        if (e.key === 'Enter' && isRunning) {
          checkAnswer();
        }
      });
      
      // 모달 외부 클릭 시 닫기
      document.getElementById('helpModal').addEventListener('click', function(e) {
        if (e.target === this) {
          closeHelp();
        }
      });
    });

    // ESC 키로 모달 닫기
    document.addEventListener('keydown', function(e) {
      if (e.key === 'Escape') {
        closeHelp();
      }
    });
  </script>
</body>
</html>
