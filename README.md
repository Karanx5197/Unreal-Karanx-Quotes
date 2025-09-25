# Unreal-Karanx-Quotes
My Quotes Web site 

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Unreal Karan Quotes</title>

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Monoton&family=Pacifico&family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body, html { width: 100%; height: 100%; font-family: 'Poppins', sans-serif; overflow-x: hidden; background: #000; color: #fff; }

  /* Landing Page */
  #landing {
    display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh;
    background: #000;
    z-index: 2;
  }
  #landing h1 { font-family: 'Monoton', cursive; font-size: 60px; margin-bottom: 40px; }
  .category-btn {
    background: linear-gradient(90deg,#ff6600,#ffcc00);
    padding: 15px 40px; margin: 15px; font-size: 20px;
    border: none; border-radius: 10px; cursor: pointer;
    color: #fff; font-weight: bold;
    transition: transform 0.3s;
  }
  .category-btn:hover { transform: scale(1.1); }

  /* Quotes Section */
  #quotes-section { display: none; padding: 30px; text-align: center; position: relative; z-index: 2; }
  #back-btn {
    background: linear-gradient(90deg,#ff6600,#ffcc00);
    padding: 10px 20px; 
    margin-bottom: 20px;
    font-size: 16px;
    border: none; border-radius: 8px;
    cursor: pointer; color: #fff; font-weight: bold;
    transition: transform 0.3s;
  }
  #back-btn:hover { transform: scale(1.05); }

  .quote-box {
    position: relative;
    background: rgba(255,255,255,0.05); 
    margin: 20px auto; 
    padding: 25px; 
    border-radius: 15px; 
    max-width: 800px; 
    box-shadow: 0 0 15px rgba(255,255,255,0.1); 
    opacity: 0;               
    transform: translateY(20px);
    transition: opacity 0.8s ease, transform 0.8s ease; 
  }
  .quote-box.show {
    opacity: 1;
    transform: translateY(0);
  }
  .quote-text { font-size: 20px; font-style: italic; margin-bottom: 10px; }
  .writer { font-family: 'Pacifico', cursive; font-size: 18px; color: #ffcc00; text-align: right; }

  /* NEW badge */
  .new-badge {
    position: absolute;
    top: 15px;
    right: 15px;
    background: #ff6600;
    color: #fff;
    font-size: 12px;
    font-weight: bold;
    padding: 3px 8px;
    border-radius: 5px;
  }

  /* Sparkle effect */
  #sparkle-canvas { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 1; pointer-events: none; }

  footer { text-align: center; padding: 20px; color: #aaa; margin-top: 50px; }
</style>
</head>
<body>

<!-- Sparkle Canvas -->
<canvas id="sparkle-canvas"></canvas>

<!-- Landing Page -->
<div id="landing">
  <h1>Unreal Karan</h1>
  <p style="margin-bottom:30px; color:#bbb; font-size:18px;">Choose a topic:</p>
  <button class="category-btn" data-category="friendship">Friendship</button>
  <button class="category-btn" data-category="love">Love</button>
  <button class="category-btn" data-category="life">Life</button>
</div>

<!-- Quotes Section -->
<div id="quotes-section"></div>

<footer>© 2025 Unreal Karan | All Rights Reserved</footer>

<script>
const landing = document.getElementById('landing');
const quotesSection = document.getElementById('quotes-section');
const buttons = document.querySelectorAll('.category-btn');

// Quotes Data
const quotes = {
  love: [
    "तुम्हारी मुस्कान मेरे दिन को रोशन कर देती है।",
    "प्यार वो नहीं जो बस आँखों में दिखे, बल्कि जो दिल में महसूस हो।",
    "तुमसे दूर रहकर भी तुम्हारा एहसास हर पल मेरे साथ है।",
    "सच्चा प्यार कभी खत्म नहीं होता, बस गहरा होता जाता है।",
    "तुम मेरी खामोशी में भी गूँजते हो।",
    "दिल से दिल का रिश्ता शब्दों से नहीं, एहसासों से बँधा होता है।",
    "तुम्हारे बिना ये दुनिया अधूरी सी लगती है।",
    "कभी-कभी एक नजर हज़ार बातें कह जाती है।",
    "प्यार वो है जो हर कमी को पूरी कर देता है।",
    "तुम मेरे ख्वाबों की हकीकत हो।",
    "दिल के पास रहने के लिए दूरी मायने नहीं रखती।",
    "तुम्हारे साथ हर लम्हा खास है।",
    "सच्चा प्यार वो है जो वक्त और हालात से प्रभावित न हो।",
    "तुम मेरी जिंदगी की सबसे खूबसूरत वजह हो।",
    "प्यार में सिर्फ एक एहसास काफी है — समझने के लिए।",
    "तुम्हारे बिना मेरी धड़कन अधूरी है।",
    "तुम्हारे होंठों की मुस्कान मेरे दिल की धड़कन बढ़ा देती है।",
    "प्यार का मतलब सिर्फ ‘आई लव यू’ नहीं, बल्कि ‘मैं हमेशा तुम्हारे साथ हूँ’ है।",
    "तुम मेरी जिंदगी में आए और सब कुछ खूबसूरत बना दिया।",
    "सच्चा प्यार वही है जो बिना शर्त के साथ दे।",
    "तुम्हारे साथ बिताए हर पल मेरे लिए अनमोल हैं।",
    "प्यार में ना तो समय मायने रखता है, ना दूरी।",
    "तुम मेरी खुशियों की वजह हो, और मेरी मजबूती भी।",
    "दिल से दिल का रिश्ता शब्दों से नहीं, एहसासों से बँधा होता है।",
    "तुम्हारी यादें मेरी तन्हाई को महक देती हैं।",
    "प्यार में डर नहीं होता, सिर्फ विश्वास और उम्मीद होती है।",
    "तुम मेरी सबसे बड़ी ताकत और सबसे खूबसूरत कमजोरी हो।",
    "प्यार में कोई नियम नहीं, बस दिल की सुनो।",
    "तुम्हारे बिना मेरा हर दिन अधूरा है।",
    "सच्चा प्यार कभी खत्म नहीं होता, वो हमेशा दिल में रहता है।",
    "तुम मेरी जिंदगी का सबसे सुंदर हिस्सा हो।",
    "प्यार में छोटा या बड़ा कोई फर्क नहीं, बस सच्चा होना चाहिए।",
    "तुमसे बातें करना मेरे लिए सबसे प्यारा पल होता है।",
    "सच्चा प्यार हमारे सपनों को हकीकत में बदल देता है।",
    "तुम्हारे बिना मेरी धड़कन रुक सी जाती है।",
    "प्यार वो है जो हमारे फासलों को भी मिटा दे।",
    "तुम मेरी जिंदगी की वो मिठास हो जो हर दिन मीठा बना दे।",
    "तुम मेरे ख्यालों में हमेशा रहते हो।",
    "सच्चा प्यार वही है जो हमारी कमियों को भी स्वीकार करे।",
    "तुम मेरे लिए वो खास हो जिसे मैं हमेशा पास रखना चाहता हूँ।",
    "प्यार में शब्द कम और एहसास ज्यादा होते हैं।",
    "तुम्हारे बिना मेरी हँसी भी फीकी लगती है।",
    "प्यार का मतलब सिर्फ मिलना नहीं, समझना भी है।",
    "तुम मेरी जिंदगी की वो खुशी हो जो कभी खत्म न हो।",
    "सच्चा प्यार हमारी आत्मा को जोड़ देता है।",
    "तुम्हारे साथ बिताया हर पल मेरे लिए यादगार है।",
    "प्यार में दूरी सिर्फ एक चुनौती है, बाधा नहीं।",
    "तुम्हारे बिना मेरा दिन अधूरा है।",
    "सच्चा प्यार हमारे अंदर की अच्छाई को बाहर लाता है।",
    "तुम मेरी जिंदगी का वो हिस्सा हो जिसे मैं हमेशा संभालकर रखना चाहता हूँ।"
  ],
  life: [
    "कभी-कभी सब कुछ ठीक लगता है, लेकिन अंदर से खालीपन होता है।",
    "लोग बदल जाते हैं, रिश्ते भी बदल जाते हैं, और हमारी उम्मीदें भी।",
    "जिंदगी में कभी-कभी सही समय पर सही लोग नहीं मिलते।",
    "दुनिया में सबकुछ दिखावे का है, सचाई कम ही मिलती है।",
    "जो चीज़ हम चाहते हैं, वह हमेशा हाथ में नहीं आती।",
    "कभी-कभी हँसना बस दर्द को छुपाने का तरीका होता है।",
    "हमेशा दूसरों को खुश करने की कोशिश में खुद भूल जाते हैं।",
    "लोग हमेशा वही समझते हैं जो दिखता है, जो अंदर है वो कोई नहीं जानता।",
    "हर कोई दोस्त नहीं होता, और हर कोई परिवार नहीं होता।",
    "जिंदगी की सबसे बड़ी सीख यही है कि हर चीज़ अस्थायी है।",
    "हम सोचते हैं कि समय ठहर जाएगा, पर समय हमेशा चलता है।",
    "कभी-कभी अकेलापन सबसे अच्छा साथी होता है।",
    "जो लोग दूर चले गए, उनकी यादें अक्सर सबसे ज्यादा दर्द देती हैं।",
    "सपने वही सच होते हैं, जिनके लिए हम मेहनत करते हैं।",
    "कुछ लोग हमारी जिंदगी में सिर्फ सबक सीखाने आते हैं।",
    "खुद को बदलने से ही जिंदगी बदलती है, किसी और को बदलने से नहीं।",
    "कभी-कभी छोटी खुशियाँ ही सबसे बड़ी होती हैं।",
    "हमेशा वही लोग याद आते हैं, जो हमारे लिए छोड़ गए।",
    "जो चीज़ आसानी से मिलती हैं, उसकी कद्र कम होती है।",
    "ज़िन्दगी का सबसे बड़ा सच यही है कि कुछ भी हमेशा नहीं रहता।",
    "जो लोग सच में हमारे हैं, वो किसी हालात में नहीं छोड़ते।",
    "कभी-कभी हमें खुद को रोकना पड़ता है, सिर्फ लोगों की बातों के लिए।",
    "हर रिश्ता आसान नहीं होता, हर दोस्त सच्चा नहीं होता।",
    "हमेशा दूसरों की खुशी में खुश रहना आसान नहीं होता।",
    "कुछ दिन ऐसे भी आते हैं जब कुछ करने का मन नहीं होता।",
    "जो यादें दर्द देती हैं, वही हमारी सबसे बड़ी सीख बनती हैं।",
    "हमेशा वही लोग याद आते हैं, जिनकी हम पर कोई उम्मीद थी।",
    "कभी-कभी लोग बदलते हैं, बिना वजह नहीं, पर वजह भी नहीं बताते।",
    "जो लोग खुश रहते हैं, उनके पास हमेशा एक वजह होती है।",
    "कभी-कभी मुस्कान सबसे बड़ी ताकत होती है।",
    "जो चीज़ें खो जाती हैं, उनकी कद्र हम तब समझते हैं जब बहुत देर हो जाती है।",
    "ज़िन्दगी में हर किसी का अपना सफर होता है।",
    "कुछ लोग हमारी जिंदगी में सिर्फ testing के लिए आते हैं।",
    "हमेशा वही लोग साथ रहते हैं, जो बिना कहे समझ जाते हैं।",
    "जो दर्द नहीं दिखता, उसका मतलब यह नहीं कि वह नहीं है।",
    "कभी-कभी खुशियाँ छोटी होती हैं, लेकिन उनका असर बड़ा होता है।",
    "जो लोग दूर जाते हैं, उनकी कमी हमेशा महसूस होती है।",
    "हर कोई हमारी तरह महसूस नहीं करता, इसलिए सबको समझाना जरूरी नहीं।",
    "कभी-कभी समय खुद सबसे बड़ा healer होता है।",
    "जो लोग हमारी जिंदगी में आते हैं, उनका असर हमेशा रहता है।",
    "कभी-कभी हमें खुद से ही लड़ना पड़ता है।",
    "जो चीज़ें मुश्किल से मिलती हैं, वही ज्यादा खास होती हैं।",
    "कभी-कभी लोग सिर्फ testing के लिए आते हैं, permanent के लिए नहीं।",
    "हमेशा वही लोग याद आते हैं, जो हमारी जिंदगी में मिसिंग फील करवाते हैं।",
    "कुछ लोग सिर्फ memories छोड़कर चले जाते हैं।",
    "कभी-कभी किसी का silence भी बहुत कुछ कह जाता है।",
    "हमेशा सबको खुश नहीं रखा जा सकता, खुद से भी compromise नहीं करना चाहिए।",
    "जो लोग सच में हमारे हैं, वो किसी भी मुश्किल में साथ रहते हैं।",
    "कभी-कभी छोटी बातें ही जिंदगी बदल देती हैं।",
    "हमारी सबसे बड़ी ताकत हमारी खुद की सोच होती है।"
  ],
  friendship: [
    "सच्चा दोस्त वही है, जो मुश्किल समय में साथ दे।",
    "दोस्ती में दूरी मायने नहीं रखती, एहसास मायने रखते हैं।",
    "वक़्त बदलता है, लोग बदलते हैं, लेकिन सच्चे दोस्त नहीं बदलते।",
    "दोस्ती में झूठ की कोई जगह नहीं होती।",
    "कुछ लोग हमारी जिंदगी में सिर्फ यादों के लिए आते हैं।",
    "दोस्त वो हैं जो हमारे हँसते हुए और रोते हुए दोनों समय साथ रहते हैं।",
    "दोस्ती का मतलब सिर्फ साथ नहीं, समझना भी है।",
    "कभी-कभी दोस्त हमारी सबसे बड़ी ताकत बन जाते हैं।",
    "सच्चा दोस्त वही है जो बिना कहे हमारी सोच समझ जाए।",
    "कुछ दोस्त सिर्फ testing के लिए आते हैं, लेकिन जो permanent हैं वो सच्चे दोस्त होते हैं।",
    "दोस्ती में competition नहीं, companionship चाहिए।",
    "जो दोस्त मुश्किल में साथ छोड़ दें, वो कभी सच्चे नहीं थे।",
    "सच्चा दोस्त हमारी चुप्पी में भी समझ जाता है।",
    "दोस्ती में लड़ाई होती है, पर सच्चाई हमेशा जीतती है।",
    "कभी-कभी हम अपनी problems सिर्फ दोस्तों से share कर पाते हैं।",
    "जो दोस्त यादों में हमेशा रहते हैं, उनका कोई मूल्य नहीं होता।",
    "दोस्ती में ना rules, ना restrictions — बस trust चाहिए।",
    "सच्चा दोस्त वही है जो हमारे लिए ख़ुशियाँ और दुख दोनों में खड़ा रहे।",
    "कुछ लोग हमारी जिंदगी में testing के लिए आते हैं, कुछ permanent के लिए।",
    "दोस्ती हमारी strength है, weakness नहीं।",
    "वक़्त और distance सिर्फ friendship को और मजबूत बनाते हैं।",
    "सच्चे दोस्त वही हैं जो बिना वजह परेशान न करें।",
    "दोस्त वही है जो हमारी छोटी-छोटी खुशियों में भी खुश रहे।",
    "कभी-कभी silence भी friendship का proof होता है।",
    "जो दोस्त हमारी limits समझ लें, वो सबसे खास होते हैं।",
    "सच्चा दोस्त वो है जो हमारी गलती होने पर भी सच बोले।",
    "दोस्ती में show-off नहीं, सिर्फ real bonding चाहिए।",
    "कभी-कभी दोस्त हमारी सबसे बड़ी therapy बन जाते हैं।",
    "दोस्त वही है जो हमारी कमी देखकर भी हमें accept करे।",
    "सच्ची दोस्ती हमारी memories में हमेशा जिंदा रहती है।",
    "कुछ दोस्त सिर्फ adventure के लिए आते हैं, कुछ जिंदगी भर के लिए।",
    "कभी-कभी एक laugh ही हमारी friendship की सबसे बड़ी strength होती है।",
    "दोस्त वही है जो हमारी गलतियों पर हंसकर सिखा भी दे।",
    "सच्ची दोस्ती में ego नहीं, सिर्फ understanding होती है।",
    "जो दोस्त हमारे साथ silent भी रह सकते हैं, वो सबसे अच्छे दोस्त हैं।",
    "दोस्ती हमारी safe place होती है, stress relief नहीं।",
    "कभी-कभी दोस्त हमारी पहचान बन जाते हैं।",
    "सच्चा दोस्त वही है जो हमारी success में jealous न हो, बल्कि celebrate करे।",
    "दोस्ती में सिर्फ quantity नहीं, quality मायने रखती है।",
    "जो दोस्त हमेशा support करें, वो हमारी जिंदगी में permanent होते हैं।",
    "सच्ची दोस्ती हमारी खुशियों और दुख दोनों में हमें समझती है।"
  ]
};

// Event Listeners
buttons.forEach(btn => {
  btn.addEventListener('click', () => {
    const category = btn.getAttribute('data-category');
    landing.style.display = 'none';
    displayQuotes(category);
    startSparkle();
  });
});

// Display Quotes with Scroll Fade Effect, Back Button & NEW badge
function displayQuotes(category) {
  quotesSection.innerHTML = `<button id="back-btn">← Back</button>`;
  quotesSection.style.display = 'block';

  const backBtn = document.getElementById('back-btn');
  backBtn.addEventListener('click', () => {
    quotesSection.style.display = 'none';
    landing.style.display = 'flex';
  });

  const today = new Date();
  const formattedDate = `${today.getDate()}/${today.getMonth()+1}/${today.getFullYear()}`;

  quotes[category].forEach(q => {
    const box = document.createElement('div');
    box.classList.add('quote-box');
    box.innerHTML = `
      <p class="quote-text">"${q}"</p>
      <p class="writer">- Karan</p>
      <span class="new-badge">NEW • ${formattedDate}</span>
    `;
    quotesSection.appendChild(box);
  });

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if(entry.isIntersecting){
        entry.target.classList.add('show');
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.1 });

  const allBoxes = document.querySelectorAll('.quote-box');
  allBoxes.forEach(box => observer.observe(box));
}

/* Sparkle Effect */
const canvas = document.getElementById('sparkle-canvas');
const ctx = canvas.getContext('2d');
let particles = [];

function resizeCanvas() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

class Particle {
  constructor() {
    this.x = Math.random() * canvas.width;
    this.y = Math.random() * canvas.height;
    this.size = Math.random() * 3 + 1;
    this.speedX = Math.random() * 1 - 0.5;
    this.speedY = Math.random() * 1 - 0.5;
    this.alpha = Math.random();
  }
  update() { this.x += this.speedX; this.y += this.speedY;
    if(this.x < 0 || this.x > canvas.width) this.speedX = -this.speedX;
    if(this.y < 0 || this.y > canvas.height) this.speedY = -this.speedY;
  }
  draw() { ctx.fillStyle = `rgba(255,255,255,${this.alpha})`;
    ctx.beginPath(); ctx.arc(this.x,this.y,this.size,0,Math.PI*2); ctx.fill(); }
}

function startSparkle() {
  particles = [];
  for(let i=0;i<200;i++){ particles.push(new Particle()); }
  animateSparkle();
}

function animateSparkle() {
  ctx.clearRect(0,0,canvas.width,canvas.height);
  particles.forEach(p => { p.update(); p.draw(); });
  requestAnimationFrame(animateSparkle);
}
                                                                      /* 
  🔹 DAILY NEW QUOTES ADD GUIDE 🔹

  Use the following format to add new quotes to your website:

  quotes['love'].unshift("Yahan apna new love quote likho");
  quotes['life'].unshift("Yahan apna new life quote likho");
  quotes['friendship'].unshift("Yahan apna new friendship quote likho");

  ✅ Example:

  quotes['love'].unshift("तुम हमेशा मेरी जिंदगी में रोशनी की तरह रहोगे।");
  quotes['life'].unshift("हर नया दिन एक नई उम्मीद लेकर आता है।");
  quotes['friendship'].unshift("सच्चा दोस्त वही है जो हमेशा साथ दे।");

  ⚠️ Reminder:
  - Ye code **script tag ke andar** hona chahiye.
  - Reload karne par ye automatically quotes section me add ho jayega.
  - NEW badge aur aaj ka date bhi automatically show hoga.
*/

</script>

</body>
</html>

