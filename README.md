# calculator-project
**Working Calculator (HTML, CSS, JavaScript):** Designed and developed a responsive web-based calculator to perform basic arithmetic operations using HTML for structure, CSS for styling, and JavaScript for logic and user interaction.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }

        .calculator {
            background: #fff;
            border-radius: 20px;
            padding: 24px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            width: 320px;
        }

        .display {
            background: #f7f8fa;
            border-radius: 12px;
            padding: 24px;
            margin-bottom: 20px;
            text-align: right;
            min-height: 80px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
        }

        .previous-operand {
            color: #888;
            font-size: 16px;
            min-height: 20px;
            margin-bottom: 4px;
        }

        .current-operand {
            color: #222;
            font-size: 32px;
            font-weight: 500;
            word-wrap: break-word;
            word-break: break-all;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
        }

        button {
            border: none;
            border-radius: 12px;
            font-size: 20px;
            font-weight: 500;
            padding: 24px;
            cursor: pointer;
            transition: all 0.2s;
            background: #f7f8fa;
            color: #222;
        }

        button:hover {
            background: #e9ecef;
            transform: translateY(-2px);
        }

        button:active {
            transform: translateY(0);
        }

        .operator {
            background: #667eea;
            color: white;
        }

        .operator:hover {
            background: #5568d3;
        }

        .equals {
            background: #764ba2;
            color: white;
            grid-row: span 2;
        }

        .equals:hover {
            background: #653a8a;
        }

        .clear {
            background: #ff6b6b;
            color: white;
        }

        .clear:hover {
            background: #ff5252;
        }

        .zero {
            grid-column: span 2;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display">
            <div class="previous-operand" id="previous"></div>
            <div class="current-operand" id="current">0</div>
        </div>
        <div class="buttons">
            <button class="clear" onclick="calc.clear()">C</button>
            <button onclick="calc.delete()">DEL</button>
            <button class="operator" onclick="calc.chooseOperation('÷')">÷</button>
            <button class="operator" onclick="calc.chooseOperation('×')">×</button>
            <button onclick="calc.appendNumber('7')">7</button>
            <button onclick="calc.appendNumber('8')">8</button>
            <button onclick="calc.appendNumber('9')">9</button>
            <button class="operator" onclick="calc.chooseOperation('-')">-</button>
            <button onclick="calc.appendNumber('4')">4</button>
            <button onclick="calc.appendNumber('5')">5</button>
            <button onclick="calc.appendNumber('6')">6</button>
            <button class="operator" onclick="calc.chooseOperation('+')">+</button>
            <button onclick="calc.appendNumber('1')">1</button>
            <button onclick="calc.appendNumber('2')">2</button>
            <button onclick="calc.appendNumber('3')">3</button>
            <button class="equals" onclick="calc.compute()">=</button>
            <button class="zero" onclick="calc.appendNumber('0')">0</button>
            <button onclick="calc.appendNumber('.')">.</button>
        </div>
    </div>

    <script>
        class Calculator {
            constructor(previousElement, currentElement) {
                this.previousElement = previousElement;
                this.currentElement = currentElement;
                this.clear();
            }

            clear() {
                this.currentOperand = '0';
                this.previousOperand = '';
                this.operation = undefined;
                this.updateDisplay();
            }

            delete() {
                if (this.currentOperand === '0') return;
                this.currentOperand = this.currentOperand.toString().slice(0, -1);
                if (this.currentOperand === '' || this.currentOperand === '-') {
                    this.currentOperand = '0';
                }
                this.updateDisplay();
            }

            appendNumber(number) {
                if (number === '.' && this.currentOperand.includes('.')) return;
                if (this.currentOperand === '0' && number !== '.') {
                    this.currentOperand = number;
                } else {
                    this.currentOperand = this.currentOperand.toString() + number.toString();
                }
                this.updateDisplay();
            }

            chooseOperation(operation) {
                if (this.currentOperand === '') return;
                if (this.previousOperand !== '') {
                    this.compute();
                }
                this.operation = operation;
                this.previousOperand = this.currentOperand;
                this.currentOperand = '0';
                this.updateDisplay();
            }

            compute() {
                let computation;
                const prev = parseFloat(this.previousOperand);
                const current = parseFloat(this.currentOperand);
                if (isNaN(prev) || isNaN(current)) return;

                switch (this.operation) {
                    case '+':
                        computation = prev + current;
                        break;
                    case '-':
                        computation = prev - current;
                        break;
                    case '×':
                        computation = prev * current;
                        break;
                    case '÷':
                        computation = prev / current;
                        break;
                    default:
                        return;
                }

                this.currentOperand = computation;
                this.operation = undefined;
                this.previousOperand = '';
                this.updateDisplay();
            }

            updateDisplay() {
                this.currentElement.textContent = this.currentOperand;
                if (this.operation != null) {
                    this.previousElement.textContent = `${this.previousOperand} ${this.operation}`;
                } else {
                    this.previousElement.textContent = '';
                }
            }
        }

        const previousElement = document.getElementById('previous');
        const currentElement = document.getElementById('current');
        const calc = new Calculator(previousElement, currentElement);
    </script>
</body>
</html>