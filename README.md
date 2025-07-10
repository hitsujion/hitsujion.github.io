# hitsujion.github.io
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>MBTI職業診断</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>MBTI風 職業診断</h1>
  <div id="quiz-container"></div>
  <button onclick="nextQuestion()">次へ</button>
  <div id="result"></div>
  <script src="script.js"></script>
</body>
</html>
body {
  font-family: sans-serif;
  text-align: center;
  padding: 20px;
}

#quiz-container {
  margin: 20px auto;
  max-width: 500px;
}

button {
  padding: 10px 20px;
  margin: 10px;
}
const questions = [
  { q: "一人で過ごすのが好き", type: "I" },
  { q: "直感より事実を重視する", type: "S" },
  { q: "論理的に考える", type: "T" },
  { q: "計画を立てて行動する", type: "J" }
];

const answers = {};
let current = 0;

function showQuestion() {
  const container = document.getElementById("quiz-container");
  if (current >= questions.length) {
    showResult();
    return;
  }

  const q = questions[current];
  container.innerHTML = `
    <h2>${q.q}</h2>
    <button onclick="answer('${q.type}')">はい</button>
    <button onclick="answer('${getOpposite(q.type)}')">いいえ</button>
  `;
}

function getOpposite(type) {
  const opposites = { I: "E", E: "I", S: "N", N: "S", T: "F", F: "T", J: "P", P: "J" };
  return opposites[type];
}

function answer(type) {
  answers[type] = (answers[type] || 0) + 1;
  current++;
  showQuestion();
}

function getMBTI() {
  const pairs = [["E", "I"], ["S", "N"], ["T", "F"], ["J", "P"]];
  return pairs.map(([a, b]) => (answers[a] > answers[b] ? a : b)).join('');
}

function showResult() {
  const mbti = getMBTI();
  const resultDiv = document.getElementById("result");
  const job = getJobByMBTI(mbti);
  resultDiv.innerHTML = `<h2>あなたのタイプは ${mbti}</h2><p>おすすめ職業: ${job}</p>`;
}

function getJobByMBTI(type) {
  const jobMap = {
    INTP: "AIエンジニア、研究職、データサイエンティスト",
    ENFP: "企画職、マーケティング、イベントプランナー",
    ISFJ: "看護師、保育士、サポート職",
    ENTJ: "経営者、プロジェクトマネージャー、戦略コンサル"
    // 他のMBTIタイプも追加できます
  };
  return jobMap[type] || "情報不足（ごめんなさい）";
}

showQuestion();
