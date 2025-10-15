# simplecalculator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Final Calculator with History</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: flex-start;
      height: 100vh;
      padding-top: 40px;
      background: linear-gradient(135deg, #74ebd5, #ACB6E5);
      font-family: Arial, sans-serif;
    }

    .container {
      display: flex;
      gap: 20px;
    }

    .calculator {
      width: 280px;
      background: #222;
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 5px 20px rgba(0,0,0,0.4);
    }

    .display {
      width: 100%;
      height: 60px;
      background: #000;
      color: #0f0;
      font-size: 2rem;
      text-align: right;
      padding: 10px;
      border-radius: 10px;
      margin-bottom: 15px;
      overflow-x: auto;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
    }

    button {
      height: 60px;
      font-size: 1.2rem;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: 0.2s;
    }

    button:hover {
      opacity: 0.8;
    }

    .num {
      background: #333;
      color: #fff;
    }

    .op {
      background: #ff9800;
      color: #fff;
    }

    .clear {
      background: #f44336;
      color: #fff;
      grid-column: span 2;
    }

    .equal {
      background: #4caf50;
      color: #fff;
      grid-column: span 2;
    }

    /* History section */
    .history {
      width: 220px;
      background: #fff;
      border-radius: 15px;
      padding: 15px;
      box-shadow: 0 5px 20px rgba(0,0,0,0.4);
      overflow-y: auto;
      max-height: 340px;
    }

    .history h3 {
      margin: 0 0 10px;
      text-align: center;
      font-size: 1.2rem;
      color: #222;
    }

    .history-item {
      font-size: 0.95rem;
      padding: 5px;
      border-bottom: 1px solid #ddd;
      color: #333;
      cursor: pointer;
    }

    .clear-history {
      margin-top: 10px;
      width: 100%;
      background: #f44336;
      color: white;
      border: none;
      padding: 8px;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Calculator -->
    <div class="calculator">
      <div id="display" class="display">0</div>
      <div class="buttons">
        <button class="clear" onclick="clearDisplay()">C</button>
        <button class="op" onclick="appendValue('/')">÷</button>
        <button class="op" onclick="appendValue('*')">×</button>
        <button class="op" onclick="backspace()">⌫</button>

        <button class="num" onclick="appendValue('7')">7</button>
        <button class="num" onclick="appendValue('8')">8</button>
        <button class="num" onclick="appendValue('9')">9</button>
        <button class="op" onclick="appendValue('-')">-</button>

        <button class="num" onclick="appendValue('4')">4</button>
        <button class="num" onclick="appendValue('5')">5</button>
        <button class="num" onclick="appendValue('6')">6</button>
        <button class="op" onclick="appendValue('+')">+</button>

        <button class="num" onclick="appendValue('1')">1</button>
        <button class="num" onclick="appendValue('2')">2</button>
        <button class="num" onclick="appendValue('3')">3</button>
        <button class="equal" onclick="calculate()">=</button>

        <button class="num" onclick="appendValue('0')" style="grid-column: span 2;">0</button>
        <button class="num" onclick="appendValue('.')">.</button>
      </div>
    </div>

    <!-- History -->
    <div class="history" id="history">
      <h3>History</h3>
      <button class="clear-history" onclick="clearHistory()">Clear History</button>
    </div>
  </div>

  <script>
    const display = document.getElementById('display');
    const historyBox = document.getElementById('history');

    function appendValue(value) {
      let lastChar = display.innerText.slice(-1);

      // prevent double operators
      if ("+-*/".includes(lastChar) && "+-*/".includes(value)) {
        return;
      }

      if (display.innerText === "0" || display.innerText === "Error") {
        display.innerText = value;
      } else {
        display.innerText += value;
      }
    }

    function clearDisplay() {
      display.innerText = "0";
    }

    function backspace() {
      display.innerText = display.innerText.slice(0, -1) || "0";
    }

    function calculate() {
      try {
        let expression = display.innerText;
        let result = eval(expression);
        display.innerText = result;

        // Save to history
        addToHistory(expression + " = " + result);
      } catch {
        display.innerText = "Error";
      }
    }

    function addToHistory(entry) {
      let div = document.createElement("div");
      div.className = "history-item";
      div.innerText = entry;

      // ✅ Click to reuse old result
      div.onclick = () => {
        let result = entry.split("=")[1].trim();
        display.innerText = result;
      };

      historyBox.insertBefore(div, historyBox.querySelector(".clear-history"));
      historyBox.scrollTop = historyBox.scrollHeight;
    }

    function clearHistory() {
      // remove all history items but keep heading & button
      historyBox.innerHTML = "<h3>History</h3><button class='clear-history' onclick='clearHistory()'>Clear History</button>";
    }

    // Keyboard support
    document.addEventListener("keydown", (event) => {
      if (!isNaN(event.key) || "+-*/.".includes(event.key)) {
        appendValue(event.key);
      } else if (event.key === "Enter") {
        calculate();
      } else if (event.key === "Backspace") {
        backspace();
      } else if (event.key === "Escape") {
        clearDisplay();
      }
    });
  </script>
</body>
</html>
