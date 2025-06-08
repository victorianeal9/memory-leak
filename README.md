# Connection-Lost
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Connection: A Memory Loop</title>
<style>
  body {
    margin: 0;
    padding: 20px;
    font-family: Arial, sans-serif;
    background: #000;
    color: #00ff00;
    overflow: hidden;
    cursor: crosshair;
    user-select: none;
  }
  .memory-counter {
    position: fixed;
    top: 20px;
    right: 20px;
    font-size: 16px;
    color: #ff0000;
    background: #000;
    padding: 6px 10px;
    border-radius: 5px;
    border: 2px solid #ff0000;
    z-index: 1000;
  }
  .instruction {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 16px;
    color: #ffff00;
    background: rgba(0,0,0,0.85);
    padding: 15px 20px;
    border-radius: 8px;
    max-width: 600px;
    text-align: center;
    user-select: none;
    box-shadow: 0 0 15px #ff0;
    z-index: 1000;
  }
  .text-fragment {
    position: absolute;
    font-size: 22px;
    max-width: 550px;
    color: #00ff00;
    text-shadow:
      -1px -1px 0 #000,
      1px -1px 0 #000,
      -1px 1px 0 #000,
      1px 1px 0 #000;
    opacity: 1 !important; /* force visible */
    pointer-events: none;
    font-family: 'Courier New', Courier, monospace;
    line-height: 1.4;
    user-select: none;
    font-weight: normal;
    z-index: 10;
  }
  .final-message {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 48px;
    color: #fff;
    text-align: center;
    opacity: 0;
    transition: opacity 3s ease-in;
    z-index: 2000;
    font-weight: bold;
    background-color: rgba(0,0,0,0.75);
    padding: 30px 40px;
    border-radius: 10px;
    user-select: none;
    pointer-events: none;
  }
  .final-message.visible {
    opacity: 1;
  }
</style>
</head>
<body>

<div class="memory-counter" aria-live="polite" aria-atomic="true">
  CONNECTION: <span id="memory-count">900</span> MB
</div>

<div class="instruction" aria-live="polite">
  Move slowly through the spaces between memory.<br />
  Each gesture costs what cannot be replaced.<br />
  When memory runs out, he is gone.<br /><br />
  Use mouse move, click or press <kbd>Enter</kbd> or <kbd>Spacebar</kbd> to reveal memories.
</div>

<div class="final-message" id="final-message" role="alert" aria-live="assertive" aria-atomic="true">
  CONNECTION<br />LOST
</div>

<script>
  const poemLines = [
    "Dad looks up when I walk in.",
    "His eyes still hold recognition.",
    "Good morning pet, he says clearly.",
    "We sit together with our tea.",
    "He tells me about the cattle.",
    "The same stories, but I listen.",
    "His hands still know how to gesture.",
    "I am still his child.",
    "The kitchen holds our history.",
    "This is how mornings used to be.",
    "He asks about my work.",
    "Remembers I teach at the school.",
    "Calls me by my childhood name.",
    "Shows me photos we've seen before.",
    "Still talks about fixing mum's car.",
    "Still worries about driving in the rain.",
    "I make him another cup of tea.",
    "He thanks me like I'm a guest.",
    "Where are my keys? he asks again.",
    "I help him look in the same places.",
    "He's sure someone moved them.",
    "His confidence is wavering now.",
    "Asks what day it is.",
    "Forgets I just told him.",
    "Stares at me like I'm familiar.",
    "But can't quite place me.",
    "Calls me Margaret. Mum's name.",
    "I don't correct him anymore.",
    "He's talking to someone not here.",
    "Asks when dinner will be ready.",
    "It's only ten in the morning.",
    "He can't work the kettle anymore.",
    "Stands confused in his own kitchen.",
    "I guide him to his chair.",
    "He looks grateful but lost.",
    "Asks if I know where he lives.",
    "We're sitting in his living room.",
    "He doesn't recognise the photos.",
  ];

  const memoryCountEl = document.getElementById('memory-count');
  const finalMessage = document.getElementById('final-message');

  let memoryLeft = 900;
  const totalLines = poemLines.length;
  const memoryCost = Math.floor(memoryLeft / totalLines);
  let currentIndex = 0;

  function createFragment(text) {
    const frag = document.createElement('div');
    frag.className = 'text-fragment';
    frag.textContent = text;

    // Position safely inside viewport
    const padding = 60;
    const maxWidth = 550;
    const x = Math.random() * (window.innerWidth - maxWidth - padding * 2) + padding;
    const y = Math.random() * (window.innerHeight - 50 - padding * 2) + padding;
    frag.style.left = `${x}px`;
    frag.style.top = `${y}px`;

    document.body.appendChild(frag);
  }

  function revealNextLine() {
    if(currentIndex >= poemLines.length) return;

    createFragment(poemLines[currentIndex]);
    currentIndex++;

    memoryLeft -= memoryCost;
    if(memoryLeft < 0) memoryLeft = 0;
    memoryCountEl.textContent = memoryLeft;

    if(memoryLeft === 0 || currentIndex === poemLines.length) {
      finalMessage.classList.add('visible');
      document.body.style.cursor = 'default';
      window.removeEventListener('click', onClick);
      window.removeEventListener('keydown', onKeyDown);
    }
  }

  function onClick() {
    revealNextLine();
  }

  function onKeyDown(e) {
    if(e.key === "Enter" || e.key === " ") {
      e.preventDefault();
      revealNextLine();
    }
  }

  window.addEventListener('click', onClick);
  window.addEventListener('keydown', onKeyDown);
</script>

</body>
</html>
