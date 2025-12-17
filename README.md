<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Exercise: Business & Behavior</title>
    <style>
        :root {
            --primary-color: #008080;
            --secondary-color: #20b2aa;
            --accent-color: #3cb371;
            --light-bg: #f0fff0;
            --dark-text: #333;
            --success-color: #28a745;
            --error-color: #dc3545;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0; padding: 0;
            background-color: var(--light-bg);
            color: var(--dark-text);
            line-height: 1.6;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            padding: 15px 20px;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
        }

        .score-display {
            position: absolute;
            top: 15px; right: 20px;
            font-weight: bold;
            background-color: rgba(255, 255, 255, 0.2);
            padding: 5px 10px; border-radius: 5px;
        }

        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .nav-buttons {
            display: flex; justify-content: center;
            gap: 10px; margin-bottom: 20px; flex-wrap: wrap; 
        }

        .nav-button {
            padding: 10px 15px; border: none; border-radius: 8px;
            cursor: pointer; background-color: var(--secondary-color);
            color: white; font-weight: bold; flex-grow: 1; min-width: 150px;
        }

        .nav-button.active { background-color: var(--primary-color); }

        .exercise-section {
            background-color: white; padding: 25px;
            border-radius: 12px; box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }

        h2 { color: var(--primary-color); border-bottom: 3px solid var(--secondary-color); padding-bottom: 10px; }

        .vocab-item { display: flex; padding: 15px 0; border-bottom: 1px dashed #ccc; }
        .word-col { flex: 0 0 180px; font-size: 1.1em; font-weight: bold; color: var(--primary-color); }
        .details-col { flex: 1; }
        .context { font-style: italic; color: var(--accent-color); display: block; }

        .word-bank { display: flex; flex-wrap: wrap; gap: 10px; padding: 15px; margin-bottom: 20px; border-radius: 8px; background: linear-gradient(45deg, var(--secondary-color), var(--accent-color)); }
        .word-bank-item { background: white; padding: 5px 10px; border-radius: 5px; font-weight: bold; }

        .gap-sentence { margin-bottom: 15px; padding: 10px; border-left: 5px solid var(--secondary-color); background: #f9f9f9; }
        .gap-input { width: 140px; padding: 5px; border: 2px solid #ccc; border-radius: 4px; text-align: center; }

        .matching-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .match-item { padding: 12px; border: 2px solid #ddd; border-radius: 8px; cursor: pointer; }
        .matched { background-color: var(--success-color) !important; color: white !important; }

        .mic-btn { background: var(--primary-color); color: white; border: none; padding: 8px; border-radius: 50%; cursor: pointer; }
    </style>
</head>
<body>

    <header>
        <h1>English Vocabulary Interactive Practice</h1>
        <div class="score-display">Score: <span id="current-score">0</span> / 24</div>
    </header>

    <div class="container">
        <div class="nav-buttons">
            <button class="nav-button active" onclick="showSection('vocab-list-section', this)">1. 📚 Vocabulary List</button>
            <button class="nav-button" onclick="showSection('writing-gaps-section', this)">2. ✍️ Fill in Gaps</button>
            <button class="nav-button" onclick="showSection('matching-section', this)">3. 🔗 Match Definitions</button>
            <button class="nav-button" onclick="showSection('pronunciation-section', this)">4. 🗣️ Pronunciation</button>
        </div>

        <div id="vocab-list-section" class="exercise-section">
            <h2>📚 Vocabulary List</h2>
            <div id="vocab-list-content"></div>
        </div>

        <div id="writing-gaps-section" class="exercise-section" style="display: none;">
            <h2>✍️ Fill in the Gaps (Writing)</h2>
            <div class="word-bank" id="writing-word-bank"></div>
            <div id="writing-gaps-content"></div>
            <button onclick="checkWritingGaps()" style="background: var(--success-color); color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer;">Check Answers</button>
        </div>

        <div id="matching-section" class="exercise-section" style="display: none;">
            <h2>🔗 Match Definitions</h2>
            <div class="matching-grid" id="matching-grid"></div>
        </div>

        <div id="pronunciation-section" class="exercise-section" style="display: none;">
            <h2>🗣️ Pronunciation Practice</h2>
            <div id="pronunciation-content"></div>
        </div>
    </div>

    <script>
        const vocabData = [
            {
                word: "word-of-mouth",
                context: "marketing/communication",
                meaning: "Information passed from person to person by oral communication.",
                example: "We don't spend much on ads; most of our clients come through **word-of-mouth**.",
                writingSentence: "Positive **___** is the most powerful marketing tool for a small business.",
                definition: "The passing of information from person to person by oral communication.",
                pronSentence: "The news spread by **___**."
            },
            {
                word: "autonomously",
                context: "working independently",
                meaning: "Acting independently or having the freedom to do so.",
                example: "As a senior engineer, I am expected to work **autonomously** on my projects.",
                writingSentence: "The new robot can navigate the warehouse **___** without human help.",
                definition: "In a way that is self-governing or independent.",
                pronSentence: "She prefers to work **___**."
            },
            {
                word: "behavior",
                context: "manner of acting",
                meaning: "The way in which one acts or conducts oneself, especially toward others.",
                example: "The teacher noticed a change in the student's **behavior** after the holidays.",
                writingSentence: "We are studying consumer **___** to improve our website design.",
                definition: "The way in which an animal or person acts in response to a situation.",
                pronSentence: "Good **___** should be rewarded."
            },
            {
                word: "loft",
                context: "architecture/housing",
                meaning: "A large, open area in a warehouse or factory converted into an apartment.",
                example: "They live in a beautiful **loft** in the industrial district.",
                writingSentence: "The artist uses the upper **___** of the barn as a painting studio.",
                definition: "A room or space directly under the roof of a house or other building.",
                pronSentence: "The stairs lead up to the **___**."
            },
            {
                word: "stray",
                context: "animals/wandering",
                meaning: "To move away aimlessly from a group or from the right course.",
                example: "Be careful not to **stray** from the main path while hiking.",
                writingSentence: "The organization rescues **___** cats and finds them new homes.",
                definition: "A person or thing that is separated from a group or has no home.",
                pronSentence: "Don't let the dog **___** too far."
            },
            {
                word: "once",
                context: "frequency or conditional",
                meaning: "On one occasion or only one time; as soon as.",
                example: "I visit my parents **once** a week.",
                writingSentence: "**___** the contract is signed, we can begin the work.",
                definition: "At a single point in time; formerly; as soon as.",
                pronSentence: "I have only been there **___**."
            },
            {
                word: "trade fair",
                context: "business/exhibition",
                meaning: "An exhibition at which businesses in a particular industry promote their products.",
                example: "We are traveling to Germany for an international **trade fair** next month.",
                writingSentence: "Attending a **___** is a great way to meet potential suppliers.",
                definition: "An event held to bring together members of a particular industry to display products.",
                pronSentence: "The **___** was very crowded this year."
            },
            {
                word: "enable",
                context: "capability/permission",
                meaning: "To give someone or something the authority or means to do something.",
                example: "This software will **enable** us to track our expenses more efficiently.",
                writingSentence: "New technologies **___** people to work from almost anywhere.",
                definition: "To make something possible or to provide the means for something.",
                pronSentence: "Smartphones **___** instant communication."
            }
        ];

        let score = 0;
        let writingResults = new Array(vocabData.length).fill(false);
        let matchResults = new Array(vocabData.length).fill(false);
        let pronResults = new Array(vocabData.length).fill(false);

        function showSection(id, btn) {
            document.querySelectorAll('.exercise-section').forEach(s => s.style.display = 'none');
            document.getElementById(id).style.display = 'block';
            document.querySelectorAll('.nav-button').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        }

        function setup() {
            // Vocab List
            const list = document.getElementById('vocab-list-content');
            list.innerHTML = vocabData.map(i => `
                <div class="vocab-item">
                    <div class="word-col">${i.word}</div>
                    <div class="details-col">
                        <span class="context">(${i.context})</span>
                        <p>${i.meaning}<br><small>Example: ${i.example}</small></p>
                    </div>
                </div>
            `).join('');

            // Writing
            const bank = document.getElementById('writing-word-bank');
            bank.innerHTML = vocabData.map(i => `<div class="word-bank-item">${i.word}</div>`).join('');
            const gaps = document.getElementById('writing-gaps-content');
            gaps.innerHTML = vocabData.map((i, idx) => {
                const parts = i.writingSentence.split('___');
                return `<div class="gap-sentence">${parts[0]}<input type="text" class="gap-input" data-idx="${idx}" data-ans="${i.word.toLowerCase()}">${parts[1]}<span id="fb-${idx}"></span></div>`;
            }).join('');

            // Matching
            const grid = document.getElementById('matching-grid');
            const words = vocabData.map((i, idx) => ({t: i.word, id: idx, type: 'w'}));
            const defs = vocabData.map((i, idx) => ({t: i.definition, id: idx, type: 'd'}));
            const items = [...words, ...defs].sort(() => Math.random() - 0.5);
            grid.innerHTML = items.map(i => `<div class="match-item" data-id="${i.id}" data-type="${i.type}" onclick="handleMatch(this)">${i.t}</div>`).join('');

            // Pronunciation
            const pron = document.getElementById('pronunciation-content');
            pron.innerHTML = vocabData.map((i, idx) => `
                <div class="gap-sentence">
                    ${i.pronSentence.replace(i.word, '_____')} 
                    <button class="mic-btn" onclick="testPron('${i.word}', ${idx})">🎤</button>
                    <span id="pron-fb-${idx}">Click to say "${i.word}"</span>
                </div>
            `).join('');
        }

        function checkWritingGaps() {
            document.querySelectorAll('.gap-input').forEach(input => {
                const idx = input.dataset.idx;
                if (input.value.trim().toLowerCase() === input.dataset.ans) {
                    writingResults[idx] = true;
                    document.getElementById(`fb-${idx}`).textContent = " ✅";
                } else {
                    document.getElementById(`fb-${idx}`).textContent = " ❌";
                }
            });
            updateScore();
        }

        let selected = null;
        function handleMatch(el) {
            if (el.classList.contains('matched')) return;
            if (!selected) {
                selected = el; el.style.background = '#c8e6c9';
            } else {
                if (selected.dataset.id === el.dataset.id && selected.dataset.type !== el.dataset.type) {
                    selected.classList.add('matched'); el.classList.add('matched');
                    matchResults[el.dataset.id] = true; updateScore();
                }
                selected.style.background = ''; selected = null;
            }
        }

        function testPron(word, idx) {
            const rec = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            rec.lang = 'en-US';
            rec.start();
            rec.onresult = (e) => {
                const say = e.results[0][0].transcript.toLowerCase();
                if (say.includes(word.toLowerCase().replace('-', ' '))) {
                    document.getElementById(`pron-fb-${idx}`).textContent = "✅ Correct!";
                    pronResults[idx] = true; updateScore();
                } else {
                    document.getElementById(`pron-fb-${idx}`).textContent = `❌ Heard: "${say}"`;
                }
            };
        }

        function updateScore() {
            const s = writingResults.filter(x => x).length + matchResults.filter(x => x).length + pronResults.filter(x => x).length;
            document.getElementById('current-score').textContent = s;
        }

        window.onload = setup;
    </script>
</body>
</html>
