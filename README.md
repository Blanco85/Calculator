<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scientific Calculator</title>
    <style>
        *{
            margin: 0;
            padding:0;
            box-sizing:border-box;
            font-family: 'Segoe UI', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            padding: 20px;
        }

        .calculator {
            background-color: #fff;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 400px;
            overflow: hidden;
        }

        .display {
            background-color: #1a1a2e;
            color: white;
            padding: 30px 20px;
            text-align: right;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
        }

        .previous-operand {
            font-size: 1.2rem;
            color: rgba(255, 255, 255, 0.7);
            margin-bottom: 10px;
            min-height: 1.5rem;
            word-wrap: break-word;
            word-break: break-all;
        }

        .current-operand {
            font-size: 2.5rem;
            font-weight: 300;
            word-wrap: break-word;
            word-break: break-all;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1px;
            background-color: #ddd;
        }

        button {
            border: none;
            padding: 20px;
            font-size: 1.5rem;
            cursor: pointer;
            background-color: white;
            transition: all 0.2s ease;
        }

        button:hover {
            background-color: #f0f0f0;
        }

        button:active {
            background-color: #e0e0e0;
            transform: scale(0.98);
        }

        .operator {
            background-color: #f9f9f9;
            color: #2575fc;
            font-weight: 500;
        }

        .equals {
            background-color: #2575fc;
            color: white;
            grid-column: span 2;
        }

        .equals:hover {
            background-color: #1a67e3;
        }

        .clear,.delete {
            background-color: #ff6b6b;
            color: white;
        }

        .clear:hover,.delete:hover {
            background-color: #ff5252;
        }

        .zero {
            grid-column: span 2;
        }

        @media (max-width: 480px) {
            .calculator {
                max-width: 100%;
            }

            button {
                padding: 18px;
                font-size: 1.3rem;
            }

            .current-operand {
                font-size: 2rem;
            }
        }
    </style>
    </head>
    <body>
        <div class="calculator">
            <div class="display">
                <div class="previous-operand"></div>
                <div class="current-operand">0</div>
                </div>
                <div class="buttons">
                    <button class="clear">C</button>
                    <button class="delete">DEL</button>
                    <button class="operator">%</button>
                    <button class="operator">÷</button>

                    <button class="number">7</button>
                    <button class="number">8</button>
                    <button class="number">9</button>
                    <button class="operator">x</button>

                    <button class="number">4</button>
                    <button class="number">5</button>
                    <button class="number">6</button>
                    <button class="operator">-</button>

                    <button class="number">1</button>
                    <button class="number">2</button>
                    <button class="number">3</button>
                    <button class="operator">+</button>

                    <button class="number zero">0</button>
                    <button class="number">.</button>
                    <button class="equals">=</button>
                </div>
        </div>

        <script>
            class Calculator {
                constructor(previousOperandElement, currentOperandElement)
                {
                    this.previousOperandElement = previousOperandElement;
                    this.currentOperandElement = currentOperandElement;
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
                    if (this.currentOperand === 1) {
                        this.currentOperand = '0';
                    } else {
                        this.currentOperand = this.currentOperand.slice(0, -1);
                    }
                    this.updateDisplay();
                }

                appendNumber(number) {
                    if (number === '.' && this.currentOperand.includes('.')) return;

                    if (this.currentOperand === '0' && number !=='.') {
                        this.currentOperand = number,
                    } else {
                        this.currentOperand += number;
                    }
                    this.updateDisplay();
                }

                chooseOperation(operation) {
                    if (this.currentOperand === '') return;
                    if (this.previousOperand !=='') {
                        this.compute();
                    }
                    this.operation = operation;
                    this.previousOperand = this.currentOperand;
                    this.currentOperand = '';
                    this.updateDisplay();
                }

                compute() {
                    let computation;
                    const prev = parseFloat(this.previousOperand);
                    const current = parseFloat(this.currentOperand);

                    if (isNan(prev) \\ isNaN(current)) return;

                    switch (this.operation) {
                        case '+':
                            computation = prev + current;
                            break;
                            case '-':
                                computation = prev - current;
                                break;
                                case 'x':
                                    computation = prev * current;
                                    break;
                                    case '÷':
                                        if (current === 0) {
                                            alert("Cannot divide by zero!");
                                            return;
                                        }
                                        computation = prev / current;
                                        break
                                        case '%':
                                            computation = prev % current;
                                            break;
                                            default: 
                                            return;
                                    }

                                    this.currentOperand = computation.toString();
                                    this.operation = undefined;
                                    this.previousOperand = '';
                                    this.updateDisplay();
                            }

                            updateDisplay() {
                                this.currentOperandElement.innerText = this.currentOperand;
                                if (this.operation != null) {
                                    this.previousOperandElement.innerText = 
                                    '${this.previousOperand} ${this.operation}';
                                } else {
                                    this.previousOperandElement.innerText = '';
                                }
                            }
                        }

                        // Initialize calculator
                        const previousOperandElement = document.querySelector('.previous-operand');
                        const currentOperandElement = document.querySelector('.current-operand');
                        const calculator = new Calculator(previousOperandElement,currentOperandElement);

                        // Event listeners for buttons
                        document.querySelectorAll('.number').forEach(button => {
                            button.addEventListener('click', () => {
                                calculator.appendNumber(button.innerText);
                            })
                        })

                        document.querySelectorAll('.operator').foreach(button => {
                            button.addEventListener('click', () => {
                                calculator.chooseOperation(button.innerText);
                            })
                        })

                        document.querySelector('.equals').addEventListener('click', () => {
                            calculator.compute();
                        })

                        document.querySelector('.clear').addEventListener('click', () => {
                            calculator.clear();
                        })

                        document.querySelector('.delete').addEventListener('click', () => {
                            calculator.delete();
                        })

                        // Keyboard support
                        document.addEventListener('keydown', (event) => {
                            if (event.key >= '0' && event.key <= '9' \\ event.key === '.') {
                                calculator.appendNumber(event.key);
                            } else if (event.key === '+' || event.key === '.' || event.key ==='*' || event.key === '/') {
                                let operation;
                                switch (event.key) {
                                    case '+': operation = '+'; break;
                                    case '-': operation = '-'; break;
                                    case '*': operation = '*'; break;
                                    case '/': operation = '/'; break;
                                }
                                calculator.chooseOperation(operation);
                            } else if (event.key === 'Enter' || event.key === '=') {
                                calculator.compute();
                            } else if (event.key === 'Escape' || event.key === 'Delete') {
                                calculator.clear();
                            } else if (event.key === 'Backspace') {
                                calculator.delete();
                            }
                        })
                    </script>
                </body>
                </html>
