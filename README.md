# envelop.html
// Option: create and open a data: URL for the supplied HTML
const html = `<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Birthday Countdown</title>

<link href="https://fonts.googleapis.com/css2?family=Pacifico&display=swap" rel="stylesheet">

<style>
body {
    margin: 0;
    height: 100vh;
    overflow: hidden;
    background: linear-gradient(135deg, #fde2e4, #dbeafe);
    font-family: 'Pacifico', cursive;
}

/* Envelope */
.overlay {
    position: fixed;
    inset: 0;
    display: flex;
    justify-content: center;
    align-items: center;
}

.envelope {
    width: 240px;
    height: 150px;
    background: #f9a8d4;
    border-radius: 12px;
    cursor: pointer;
    position: relative;
    box-shadow: 0 15px 40px rgba(0,0,0,0.2);
}

.flap {
    position: absolute;
    top: 0;
    left: 0;
    border-left: 120px solid transparent;
    border-right: 120px solid transparent;
    border-top: 75px solid #f472b6;
}

/* Confetti */
.confetti {
    position: fixed;
    width: 8px;
    height: 14px;
    animation: fall 3s linear forwards;
}

@keyframes fall {
    from { transform: translateY(0) rotate(0deg); }
    to { transform: translateY(110vh) rotate(360deg); }
}

/* Emojis */
.float {
    position: fixed;
    bottom: -20px;
    animation: rise 6s linear forwards;
}

@keyframes rise {
    to { transform: translateY(-110vh) scale(1.6); opacity: 0; }
}

/* Center text */
.center {
    position: fixed;
    inset: 0;
    display: none;
    justify-content: center;
    align-items: center;
    text-align: center;
}

.text {
    font-size: 4rem;
    color: #0671B7;
}

.countdown {
    font-size: 5.5rem;
    color: #0671B7;
}

/* Close */
.close {
    position: fixed;
    top: 15px;
    right: 20px;
    font-size: 26px;
    cursor: pointer;
    display: none;
}
</style>
</head>

<body>

<div class="overlay" id="overlay">
    <div class="envelope" id="envelope">
        <div class="flap"></div>
    </div>
</div>

<div class="center" id="center">
    <div id="display" class="text"></div>
</div>

<div class="close" id="close">✕</div>

<script>
const envelope = document.getElementById("envelope");
const overlay = document.getElementById("overlay");
const center = document.getElementById("center");
const display = document.getElementById("display");
const closeBtn = document.getElementById("close");

const emojis = ["💜","💜","💗","🤍","🎉","💗","💙","🎈"];

function nextBirthday() {
    const now = new Date();
    let y = now.getFullYear();
    let d = new Date(y, 0, 20);
    if (d < now) d = new Date(y + 1, 0, 20);
    return d;
}
const birthday = nextBirthday();

/* Confetti burst */
function confettiBurst() {
    for (let i = 0; i < 120; i++) {
        const c = document.createElement("div");
        c.className = "confetti";
        c.style.left = Math.random() * 100 + "vw";
        c.style.top = "-10px";
        c.style.background = \`hsl(\${Math.random()*360},100%,70%)\`;
        c.style.animationDuration = 2 + Math.random() * 2 + "s";
        document.body.appendChild(c);
        setTimeout(() => c.remove(), 4000);
    }
}

/* Floating emojis */
setInterval(() => {
    const e = document.createElement("div");
    e.className = "float";
    e.textContent = emojis[Math.floor(Math.random() * emojis.length)];
    e.style.left = Math.random() * 100 + "vw";
    e.style.fontSize = 20 + Math.random() * 30 + "px";
    document.body.appendChild(e);
    setTimeout(() => e.remove(), 6000);
}, 250);

/* Countdown */
function startCountdown() {
    display.className = "countdown";
    setInterval(() => {
        const diff = birthday - new Date();
        const d = Math.floor(diff / 86400000);
        const h = Math.floor(diff / 3600000) % 24;
        const m = Math.floor(diff / 60000) % 60;
        const s = Math.floor(diff / 1000) % 60;
        display.textContent = \`\${d}d \${h}h \${m}m \${s}s\`;
    }, 1000);
}

/* Click flow */
envelope.onclick = () => {
    overlay.style.display = "none";
    center.style.display = "flex";
    closeBtn.style.display = "block";

    confettiBurst();

    display.className = "text";
    display.textContent = "Birthday Countdown";

    setTimeout(() => {
        display.textContent = "Birthday Countdown";
    }, 500);


    setTimeout(() => {
        startCountdown();
    }, 2000);
};

closeBtn.onclick = () => location.reload();
</script>

</body>
</html>`;

// Create a data URL and open it in a new tab
const dataUrl = "data:text/html;charset=utf-8," + encodeURIComponent(html);
window.open(dataUrl, "_blank");

// Also append an anchor to the page so you can copy the link
const a = document.createElement('a');
a.href = dataUrl;
a.textContent = 'Open Birthday Countdown (data URL)';
a.style.position = 'fixed';
a.style.bottom = '10px';
a.style.left = '10px';
a.style.zIndex = 99999;
a.style.background = '#fff';
a.style.padding = '6px 10px';
a.style.border = '1px solid #ccc';
a.style.borderRadius = '4px';
a.style.boxShadow = '0 2px 6px rgba(0,0,0,0.15)';
document.body.appendChild(a);
