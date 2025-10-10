<!DOCTYPE html>
<html lang="zh-Hant">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>147測驗｜單一檔案版</title>
    <style>
      /* 回首頁浮動按鈕 */
      #homeBtn {
        position: absolute;
        top: 100px; /* 深色模式按鈕在 20px，這裡放在它下方 */
        right: 20px;
        padding: 10px 20px;
        background: #28a745;
        color: #fff;
        border: none;
        border-radius: 8px;
        text-decoration: none;
        font-size: 1em;
        box-shadow: 0 4px 12px rgba(0,0,0,.15);
        transition: background .3s, transform .2s, box-shadow .2s;
        z-index: 1000;
      }
      #homeBtn:hover { background: #218838; transform: translateY(-2px); box-shadow: 0 6px 16px rgba(0,0,0,.25); }
      body.dark #homeBtn { background: #2e7d32; }
      body.dark #homeBtn:hover { background: #25652a; }

      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 40px;
        font-size: 1.6em;
        background: #f0f4f8;
        color: #000;
        transition: background-color 0.5s, color 0.5s;
      }
      body.dark { background: #121212; color: #fff; }
      #container {
        max-width: 1200px; margin: auto; background: #fff; padding: 40px;
        border-radius: 20px; box-shadow: 0 0 10px rgba(0,0,0,.1);
        transition: background-color .5s, color .5s;
      }
      body.dark #container { background: #1e1e1e; }
      .hidden { display: none; }
      h1, h2 { text-align: center; font-size: 2.4em; }
      #rules { background: #e9ecef; padding: 20px; margin-bottom: 40px; border-radius: 10px; font-size: 1.2em; }
      body.dark #rules { background: #333; }
      .btn {
        background: #007bff; color: #fff; border: none; padding: 20px 40px;
        margin: 10px; border-radius: 10px; cursor: pointer; font-size: 1.2em;
        transition: background-color .3s;
      }
      .btn:hover { background: #0056b3; }
      #leaveBtn { background: #dc3545; }
      #progress, #timer { font-weight: bold; font-size: 1.4em; }
      .progress-container { background: #ddd; height: 10px; border-radius: 5px; margin-top: 20px; overflow: hidden; }
      body.dark .progress-container { background: #555; }
      .progress-bar { height: 100%; width: 0; background: #007bff; transition: width .6s ease; }
      .question { margin: 40px 0 20px; font-size: 1.8em; }
      .options label { display: block; margin-bottom: 16px; font-size: 1.4em; }
      table { width: 100%; border-collapse: collapse; margin-top: 40px; font-size: 1.2em; }
      th, td { border: 1px solid #ccc; padding: 16px; text-align: left; }
      tr.wrong { background-color: #ffe6e6; }
      body.dark tr.wrong { background-color: #661111; }
      #darkModeToggle {
        position: absolute; top: 20px; right: 20px; padding: 10px 20px;
        background: #333; color: #fff; border: none; border-radius: 8px; cursor: pointer; font-size: 1em;
        transition: background .3s;
      }
      #darkModeToggle:hover { background: #555; }
    </style>
  </head>
  <body>
    <!-- 吃飽太閒做的網站，能幫到你我覺得很開心 -->
    <button id="darkModeToggle">深色模式 / Dark Mode</button>
    <a id="homeBtn" href="https://hisausage7.github.io/147test/" title="回首頁">🏠 回首頁</a>

    <div id="container">
      <div id="welcome">
        <h1>147測驗M2-工科賽</h1>
        <div id="rules">
          <p><strong>考試注意事項 / Exam Rules:</strong></p>
          <p>1. 請輸入姓名後才能開始作答。 / You must enter your name to start the quiz.</p>
          <p>2. 考試限時80分鐘，自動倒數。 / The quiz is timed for 80 minutes, countdown starts immediately.</p>
          <p>3. 作答途中可隨時點擊「離開考試」提前結束。 / You can click "Leave Quiz" anytime to finish early.</p>
          <p>4. 完成後會自動顯示所有答題結果與成績。 / Results and scores will be displayed after completion.</p>
          <p>5. 答對題目顯示O，答錯題目顯示X。 / Correct answers will show O, incorrect answers will show X.</p>
          <p>6. 測驗過程為亂序出題。 / The test process was chaotic and disordered in setting questions.</p>
          <p>!版權及源代碼所有-航機系008沈崑宸!</p>
          <p>!僅作為自我測驗使用!</p>
        </div>
        <input type="text" id="nameInput" placeholder="輸入姓名 / Enter your name"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1.4em;" />
        <input type="number" id="questionLimit" placeholder="輸入題數,至多5題 / Enter number of questions"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1.4em;" />
        <button id="startBtn" class="btn">開始測驗 / Start Quiz</button>
      </div>

      <div id="quiz" class="hidden">
        <div>
          <span id="welcomeName"></span>
          <span id="timer" style="float:right">80:00</span>
        </div>
        <div id="progress">題數: <span id="current">0</span> / <span id="total">0</span></div>
        <div class="progress-container"><div id="progressBar" class="progress-bar"></div></div>
        <div class="question" id="questionText"></div>
        <div class="options" id="options"></div>
        <button id="prevBtn" class="btn" style="background:#6c757d">上一題 / Previous</button>
        <button id="leaveBtn" class="btn">離開考試 / Leave Quiz</button>
      </div>

      <div id="results" class="hidden">
        <h2>測驗結果 / Results</h2>
        <div>
          <button id="retryBtn" class="btn">重新開始 / Retry</button>
          <button id="showWrongBtn" class="btn" style="background:#ffc107">只看錯題 / Wrong Only</button>
          <button id="showAllBtn" class="btn">看全部結果 / Show All</button>
        </div>
        <table>
          <thead>
            <tr><th>題目</th><th>您的答案</th><th>正確答案</th><th>結果</th></tr>
          </thead>
          <tbody id="resultsBody"></tbody>
        </table>
        <div id="scoreSummary" style="text-align:center;margin-top:20px;font-size:1.2em"></div>
      </div>
    </div>

    <script>
      document.addEventListener("DOMContentLoaded", function () {
        const questions = [
          {
    question: "An atom that gains one or more additional electrons is called （獲得一個或多個電子的原子被稱為）",
    options: [
      "A. a negative ion（負離子）",
      "B. a positive ion（正離子）",
      "C. an isotope（同位素）",
      "D. an ion（離子）"
    ],
    answer: "A"
  },
  {
    question: "A millilitre is equal to （1 毫升等於）",
    options: [
      "A. one million litres（1 百萬公升）",
      "B. one millionth of a litre（1 百萬分之 1 公升）",
      "C. one thousandth of a litre（1 千分之 1 公升）",
      "D. ten thousand of a litre（一萬公升）"
    ],
    answer: "C"
  },
  {
    question: "The SI unit of acceleration is the （加速度的國際單位為）",
    options: [
      "A. metre per second squared (m/s²)",
      "B. metre per second (m/s)",
      "C. square metre (m²)",
      "D. metre (m)"
    ],
    answer: "A"
  },
  {
    question: "Ten kilograms is expressed numerically as （10 公斤重量以數字表達）",
    options: [
      "A. 1 Mg",
      "B. 10 K",
      "C. 10 kg",
      "D. 100 K"
    ],
    answer: "C"
  },
  {
    question: "What is the temperature conversion formula between Celsius and Fahrenheit？（攝氏溫度℃與華氏溫度℉換算的關係為何？）",
    options: [
      "A. ℃ = ℉",
      "B. ℃ = 1.8℉",
      "C. ℃ = (℉ - 32) × 5/9",
      "D. ℃ = (℉ + 32) × 9/5"
    ],
    answer: "C"
  }
        ];

        function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
          }
          return array;
        }

        let shuffledQuestions;
        let current = 0, total = 0, timer = 80 * 60, interval, answers = [];

        function fmtTime(s) {
          const m = Math.floor(s / 60).toString().padStart(2, "0");
          const sec = (s % 60).toString().padStart(2, "0");
          return m + ":" + sec;
        }

        const startBtn = document.getElementById("startBtn");
        const prevBtn = document.getElementById("prevBtn");
        const leaveBtn = document.getElementById("leaveBtn");
        const retryBtn = document.getElementById("retryBtn");
        const showWrongBtn = document.getElementById("showWrongBtn");
        const showAllBtn = document.getElementById("showAllBtn");
        const darkModeToggle = document.getElementById("darkModeToggle");
        const progressBar = document.getElementById("progressBar");

        startBtn.addEventListener("click", () => {
          const n = document.getElementById("nameInput").value.trim();
          const qLimit = parseInt(document.getElementById("questionLimit").value);

          if (!n) return alert("請輸入姓名 / Enter your name");
          if (!qLimit || qLimit <= 0) return alert("請輸入要作答的題數,最多5題 / Enter number of questions");

          shuffledQuestions = shuffle([...questions]).slice(0, Math.min(qLimit, questions.length));
          total = shuffledQuestions.length;
          current = 0; answers = []; timer = 80 * 60;

          document.getElementById("welcome").classList.add("hidden");
          document.getElementById("quiz").classList.remove("hidden");
          document.getElementById("welcomeName").innerText = "歡迎: " + n;
          document.getElementById("total").innerText = total;
          updateProgress();

          interval = setInterval(() => {
            if (timer > 0) {
              timer--;
              document.getElementById("timer").innerText = fmtTime(timer);
            } else {
              clearInterval(interval);
              finish();
            }
          }, 1000);

          showQ();
        });

        prevBtn.addEventListener("click", () => {
          if (current > 0) {
            current--;
            answers.pop();
            showQ();
          }
        });

        leaveBtn.addEventListener("click", finish);
        retryBtn.addEventListener("click", () => location.reload());
        showWrongBtn.addEventListener("click", filterResultsWrong);
        showAllBtn.addEventListener("click", showAllResults);
        darkModeToggle.addEventListener("click", () => document.body.classList.toggle("dark"));

        function showQ() {
          if (current >= total) return finish();
          document.getElementById("current").innerText = current + 1;
          updateProgress();

          const q = shuffledQuestions[current];
          document.getElementById("questionText").innerText = q.question;

          const optDiv = document.getElementById("options");
          optDiv.innerHTML = "";

          let optionsWithFlag = q.options.map((option) => ({
            text: option,
            isAnswer: option.charAt(0) === q.answer,
          }));
          optionsWithFlag = shuffle(optionsWithFlag);

          optionsWithFlag.forEach((opt) => {
            const lbl = document.createElement("label");
            const rd = document.createElement("input");
            rd.type = "radio";
            rd.name = "opt";
            rd.value = opt.text;
            rd.onchange = () => {
              answers.push({
                q,
                selectedText: opt.text,
                correctText: q.options.find((optItem) => optItem.charAt(0) === q.answer),
                correct: opt.isAnswer,
              });
              current++;
              showQ();
            };
            lbl.append(rd, " ", opt.text);
            optDiv.append(lbl);
          });
        }

        function updateProgress() {
          const percent = (current / total) * 100;
          progressBar.style.width = percent + "%";
        }

        function finish() {
          clearInterval(interval);
          document.getElementById("quiz").classList.add("hidden");
          document.getElementById("results").classList.remove("hidden");
          renderResults(answers);
          document.getElementById("scoreSummary").innerText =
            `答對 ${answers.filter((a) => a.correct).length} 題 / 已作答 ${answers.length} 題 / 共 ${total} 題`;
          window.scrollTo({ top: 0, behavior: "smooth" });
        }

        function renderResults(data) {
          const tb = document.getElementById("resultsBody");
          tb.innerHTML = "";
          data.forEach((a) => {
            const tr = document.createElement("tr");
            tr.innerHTML = `
              <td>${a.q.question}</td>
              <td>${a.selectedText}</td>
              <td>${a.correctText}</td>
              <td>${a.correct ? "O" : "X"}</td>
            `;
            if (!a.correct) tr.classList.add("wrong");
            tb.append(tr);
          });
        }

        function filterResultsWrong() { renderResults(answers.filter((a) => !a.correct)); }
        function showAllResults() { renderResults(answers); }
      });
    </script>
  </body>
</html>
