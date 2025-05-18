<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<title>Fufufafa</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #f5f0e6, #d6b8a2);
    max-width: 600px;
    margin: 40px auto;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.15);
    color: #333;
  }
  h1 {
    text-align: center;
    color: #a74942;
    margin-bottom: 25px;
    font-weight: 700;
  }
  #quiz-container {
    background: #fff5f2;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 0 4px 10px rgba(167,73,66,0.4);
  }
  .question-area {
    display: flex;
    align-items: center;
    gap: 20px;
    margin-bottom: 20px;
  }
  #hutao-img {
    width: 120px;
    border-radius: 15px;
    border: none;
   
  }
  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-15px); }
  }
  #question {
    font-size: 20px;
    font-weight: 600;
    flex: 1;
    color: #7b3f36;
  }
  .options button {
    width: 100%;
    margin: 8px 0;
    padding: 12px;
    font-size: 16px;
    border: none;
    border-radius: 12px;
    background-color: #f9d6cf;
    color: #a74942;
    cursor: pointer;
    box-shadow: 0 3px 6px rgba(167,73,66,0.2);
    transition: background-color 0.3s ease;
  }
  .options button:hover {
    background-color: #d97c6a;
    color: #fff;
  }
  #progress {
    height: 10px;
    background-color: #e9c8bb;
    border-radius: 8px;
    overflow: hidden;
    margin-bottom: 20px;
  }
  #progress-bar {
    height: 100%;
    width: 0%;
    background-color: #a74942;
    transition: width 0.3s ease;
  }
  #result {
    margin-top: 30px;
    font-size: 22px;
    font-weight: 700;
    text-align: center;
    color: #a74942;
  }
  #share {
    margin-top: 15px;
    text-align: center;
  }
  #share button {
    background-color: #a74942;
    color: white;
    border: none;
    padding: 12px 24px;
    border-radius: 15px;
    cursor: pointer;
    font-size: 16px;
    box-shadow: 0 3px 6px rgba(167,73,66,0.5);
    transition: background-color 0.3s ease;
  }
  #share button:hover {
    background-color: #6b2b23;
  }
</style>
</head>
<body>

<h1>🔮 Hidup joko....</h1>

<div id="quiz-container">
  <div id="progress"><div id="progress-bar"></div></div>

  <div class="question-area">
    <img id="hutao-img" src="https://www.pngall.com/wp-content/uploads/14/Hu-Tao-PNG-Free-Image.png" alt="Hu Tao" />
    <div id="question">Pertanyaan akan muncul di sini...</div>
  </div>

  <div class="options" id="options"></div>
</div>

<div id="result"></div>
<div id="share" style="display:none;">
  <button onclick="copyResult()">📋 Salin & Bagikan Hasil</button>
</div>

<script>
  const questions = [
    {
      text: "Apa yang kamu lakukan saat waktu luang?",
      options: [
        { text: "Membaca buku", types: ["Reflektif", "Analitis"] },
        { text: "Menulis cerita", types: ["Kreatif"] },
        { text: "Hangout sama teman", types: ["Sosial"] },
        { text: "Jalan-jalan spontan", types: ["Petualang"] }
      ]
    },
    {
      text: "Bagaimana kamu menghadapi masalah?",
      options: [
        { text: "Diam dan berpikir dulu", types: ["Reflektif"] },
        { text: "Menganalisis dan mencari solusi", types: ["Analitis"] },
        { text: "Curhat ke teman", types: ["Sosial", "Empatik"] },
        { text: "Langsung bertindak", types: ["Petualang", "Praktis"] }
      ]
    },
    {
      text: "Kamu lebih suka suasana...",
      options: [
        { text: "Tenang dan damai", types: ["Reflektif"] },
        { text: "Rapi dan terstruktur", types: ["Analitis"] },
        { text: "Rame dan hidup", types: ["Sosial"] },
        { text: "Petualangan dan kejutan", types: ["Petualang"] }
      ]
    },
    {
      text: "Dalam kelompok, kamu biasanya...",
      options: [
        { text: "Mengamati dan mendengarkan", types: ["Reflektif"] },
        { text: "Mengarahkan dan menyusun rencana", types: ["Analitis"] },
        { text: "Menghibur dan bersosialisasi", types: ["Sosial"] },
        { text: "Mendorong aksi dan tantangan", types: ["Petualang"] }
      ]
    },
    {
      text: "Gaya belajarmu lebih ke arah...",
      options: [
        { text: "Sendiri dan mandiri", types: ["Reflektif"] },
        { text: "Mencatat dan menyusun poin", types: ["Analitis"] },
        { text: "Diskusi dan tanya jawab", types: ["Sosial"] },
        { text: "Praktik langsung", types: ["Petualang"] }
      ]
    }
    // Kamu bisa tambah sampai 20 pertanyaan di sini
  ];

  const allTypes = ["Reflektif", "Analitis", "Sosial", "Kreatif", "Petualang", "Empatik", "Praktis"];
  let currentQuestion = 0;
  let scores = {};
  allTypes.forEach(t => scores[t] = 0);

  function showQuestion() {
    const q = questions[currentQuestion];
    document.getElementById("question").textContent = q.text;

    const optionsDiv = document.getElementById("options");
    optionsDiv.innerHTML = "";

    q.options.forEach(option => {
      const btn = document.createElement("button");
      btn.textContent = option.text;
      btn.onclick = () => {
        option.types.forEach(t => scores[t]++);
        currentQuestion++;
        updateProgress();
        if (currentQuestion < questions.length) {
          showQuestion();
        } else {
          showResult();
        }
      };
      optionsDiv.appendChild(btn);
    });
  }

  function updateProgress() {
    const progressPercent = (currentQuestion / questions.length) * 100;
    document.getElementById("progress-bar").style.width = progressPercent + "%";
  }

  function showResult() {
    document.getElementById("quiz-container").style.display = "none";
    const resultDiv = document.getElementById("result");

    const sortedScores = Object.entries(scores).sort((a,b) => b[1] - a[1]);
    const topType = sortedScores[0][0];
    const topScore = sortedScores[0][1];

    const descriptions = {
      "Reflektif": "🌱 Si Reflektif: tenang, introspektif, suka menyendiri.",
      "Analitis": "🔍 Si Analitis: logis, rasional, suka struktur dan perencanaan.",
      "Sosial": "💬 Si Sosial: ramah, komunikatif, senang berkumpul.",
      "Kreatif": "🎨 Si Kreatif: imajinatif, ekspresif, suka hal baru.",
      "Petualang": "🚀 Si Petualang: spontan, aktif, suka tantangan.",
      "Empatik": "❤️ Si Empatik: peduli, peka, suka menolong.",
      "Praktis": "🛠️ Si Praktis: realistis, efisien, suka selesaikan masalah langsung."
    };

    resultDiv.innerHTML = `<strong>Hasilmu:</strong><br>${descriptions[topType]} (nilai: ${topScore})`;
    document.getElementById("share").style.display = "block";
  }

  function copyResult() {
    const resultText = document.getElementById("result").innerText;
    navigator.clipboard.writeText(resultText).then(() => {
      alert("Hasil berhasil disalin! Kamu bisa bagikan ke teman.");
    });
  }

  showQuestion();
  updateProgress();
</script>

</body>
</html>
