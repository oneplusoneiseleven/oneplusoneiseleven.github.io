<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Text Gamble</title>
  <style>
    body {
      background-color: #111;
      color: #0f0;
      font-family: monospace;
      padding: 20px;
    }
    #output {
      white-space: pre-wrap;
    }
    input, button {
      background-color: #222;
      color: #0f0;
      border: 1px solid #0f0;
      font-family: monospace;
      padding: 5px;
    }
    button:hover {
      background-color: #0f0;
      color: #000;
    }
  </style>
</head>
<body>

<div id="output"></div>
<br>
<input type="number" id="bet" placeholder="Amount to gamble">
<button onclick="gamble()">Gamble</button>
<button onclick="reset()">Reset</button>

<script>
  let money = 100;
  let startMoney = 100;
  let loansTaken = 0;
  const output = document.getElementById("output");

  function log(text) {
    output.textContent += text + "\n";
    output.scrollTop = output.scrollHeight;
  }

  function reset() {
    money = 100;
    startMoney = 100;
    loansTaken = 0;
    output.textContent = "";
    log("You have $100.");
  }

  function gamble() {
    const input = document.getElementById("bet");
    let toGamble = parseInt(input.value);
    input.value = "";

    if (isNaN(toGamble) || toGamble <= 0) {
      log("Enter a valid amount.");
      return;
    }

    if (toGamble > money) {
      if (toGamble <= (money * 1.5 + 1)) {
        let loan = toGamble - money;
        loansTaken += loan;
        log(`You took a loan of $${loan} to afford this bet.`);
        money = toGamble;
      } else {
        log("Bet too high. You can't borrow that much.");
        return;
      }
    }

    let rng = Math.random() < 0.5 ? 0 : 1;
    if (rng === 1) {
      money += toGamble;
      log(`You won! You now have $${money}.`);
    } else {
      money -= toGamble;
      log(`You lost. You now have $${money}.`);
    }

    if (money <= 0) {
      log("You're broke. Reset to try again.");
    }

    log(`Net profit: $${money - startMoney - loansTaken}`);
  }

  // Start game
  reset();
</script>

</body>
</html>
