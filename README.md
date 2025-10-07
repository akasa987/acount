# acount
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>くじ引きアプリ</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      margin-top: 100px;
    }

    #result {
      margin-top: 30px;
      font-size: 2rem;
      font-weight: bold;
      color: #0077cc;
    }

    button {
      font-size: 1.5rem;
      padding: 15px 30px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <p>おみくじ<br>ボタンをクリックしてください。</p>
    <div class="wrapper-button">
      <button id="button" class="button">おみくじを引く</button>
    </div>
    <p id="result"></p>
    <p id="myElement"></p>
  </div>

  <script>
    // 景品リスト（確率と在庫数）
    const prizes = [
      { name: "スイッチ2", rate: 3, stock: 4 },
      { name: "吉", rate: 7, stock: 1 },
      { name: "中吉", rate: 10, stock: 1 },
      { name: "小吉", rate: 10, stock: 1 },
      { name: "末吉", rate: 15, stock: 1 },
      { name: "凶", rate: 20, stock: 1 },
      { name: "ポケットティッシュ", rate: 35, stock: 1 }
    ];

    let drawCount = 0;
    const maxDraws = 10;

    window.onload = function () {
      document.getElementById("button").onclick = function () {
        const resultElement = document.getElementById("result");
        const messageElement = document.getElementById("myElement");

        if (drawCount >= maxDraws) {
          messageElement.textContent = 'もうくじは引けません。';
          return;
        }

        // 在庫のある景品だけを対象に抽選
        const availablePrizes = prizes.filter(p => p.stock > 0);

        if (availablePrizes.length === 0) {
          messageElement.textContent = 'すべての景品が終了しました。';
          return;
        }

        // 合計確率（在庫のあるものだけ）
        const totalRate = availablePrizes.reduce((sum, p) => sum + p.rate, 0);
        const rand = Math.random() * totalRate;

        let cumulative = 0;
        for (let prize of availablePrizes) {
          cumulative += prize.rate;
          if (rand < cumulative) {
            resultElement.textContent = `結果：${prize.name}`;
            prize.stock--; // 在庫を1つ減らす
            drawCount++;
            messageElement.textContent = `残り回数：${maxDraws - drawCount}`;
            return;
          }
        }
      }
    };
  </script>
</body>
</html>
