<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fun Pinyin Learning</title>
    <style>
        body {
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background-color: #f0f8ff;
            color: #333;
            line-height: 1.6;
            padding: 0;
            margin: 0;
        }

        header {
            background-color: #ff9966;
            color: white;
            text-align: center;
            padding: 1rem;
            margin-bottom: 2rem;
        }

        h1, h2 {
            color: #ff5e62;
        }

        main {
            max-width: 1000px;
            margin: 0 auto;
            padding: 0 1rem;
        }

        .category {
            background-color: white;
            border-radius: 10px;
            padding: 1rem;
            margin-bottom: 2rem;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        .buttons-container {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 1rem;
        }

        .pinyin-btn {
            background-color: #88d8b0;
            border: none;
            border-radius: 50px;
            color: white;
            padding: 10px 15px;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
        }

        .pinyin-btn:hover {
            background-color: #ff6b6b;
            transform: scale(1.1);
        }

        #info-panel {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            max-width: 500px;
            width: 80%;
            z-index: 100;
            display: none;
        }

        #info-panel.visible {
            display: block;
        }

        #close-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: #888;
        }

        .examples {
            margin-top: 1rem;
            padding-top: 1rem;
            border-top: 1px dashed #ccc;
        }

        @media (max-width: 600px) {
            .buttons-container {
                justify-content: center;
            }
            
            .pinyin-btn {
                padding: 8px 12px;
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>Fun Pinyin Learning</h1>
        <p>A fun way for English-speaking kids to learn Chinese Pinyin!</p >
    </header>
    
    <main>
        <section class="category" id="initials">
            <h2>Initials (声母)</h2>
            <div class="buttons-container"></div>
        </section>
        
        <section class="category" id="simple-finals">
            <h2>Simple Finals (单韵母)</h2>
            <div class="buttons-container"></div>
        </section>
        
        <section class="category" id="compound-finals">
            <h2>Compound Finals (复韵母)</h2>
            <div class="buttons-container"></div>
        </section>
        
        <section class="category" id="nasal-finals">
            <h2>Nasal Finals (鼻韵母)</h2>
            <div class="buttons-container"></div>
        </section>
        
        <section class="category" id="tones">
            <h2>Tones (声调)</h2>
            <div class="buttons-container"></div>
        </section>
    </main>
    
    <div id="info-panel" class="hidden">
        <button id="close-btn">×</button>
        <h3 id="pinyin-display"></h3>
        <p id="english-sound"></p >
        <p id="description"></p >
        <div class="examples">
            <h4>Examples:</h4>
            <ul id="examples-list"></ul>
        </div>
    </div>
    
    <script>
        // Pinyin data with English associations
        const pinyinData = {
            initials: [
                { pinyin: "b", english: "like 'b' in 'boy'", desc: "Unaspirated 'b' sound", examples: ["bàba (dad)", "bǐ (pen)"] },
                { pinyin: "p", english: "like 'p' in 'pie'", desc: "Aspirated 'p' sound", examples: ["píngguǒ (apple)", "pà (afraid)"] },
                { pinyin: "m", english: "like 'm' in 'mom'", desc: "'m' sound", examples: ["māma (mom)", "mǐ (rice)"] },
                { pinyin: "f", english: "like 'f' in 'fish'", desc: "'f' sound", examples: ["fēijī (airplane)", "fàn (rice)"] },
                { pinyin: "d", english: "like 'd' in 'dog'", desc: "Unaspirated 'd' sound", examples: ["dà (big)", "dōngxi (thing)"] },
                { pinyin: "t", english: "like 't' in 'top'", desc: "Aspirated 't' sound", examples: ["tùzi (rabbit)", "tiān (day)"] },
                { pinyin: "n", english: "like 'n' in 'no'", desc: "'n' sound", examples: ["nǎinai (grandma)", "nǐ (you)"] },
                { pinyin: "l", english: "like 'l' in 'love'", desc: "'l' sound", examples: ["lǎoshī (teacher)", "lǜ (green)"] },
                { pinyin: "g", english: "like 'g' in 'go'", desc: "Unaspirated 'g' sound", examples: ["gǒu (dog)", "gōngzuò (work)"] },
                { pinyin: "k", english: "like 'k' in 'kite'", desc: "Aspirated 'k' sound", examples: ["kāfēi (coffee)", "kěshì (but)"] },
                { pinyin: "h", english: "like 'h' in 'hello'", desc: "'h' sound", examples: ["hǎo (good)", "hē (drink)"] },
                { pinyin: "j", english: "like 'j' in 'jeep' but softer", desc: "Soft 'j' sound", examples: ["jiā (home)", "jīntiān (today)"] },
                { pinyin: "q", english: "like 'ch' in 'cheese' but softer", desc: "Soft 'ch' sound", examples: ["qī (seven)", "qǐng (please)"] },
                { pinyin: "x", english: "like 'sh' in 'sheep' but softer", desc: "Soft 'sh' sound", examples: ["xièxie (thank you)", "xīngqī (week)"] },
                { pinyin: "zh", english: "like 'j' in 'jump'", desc: "'j' sound with tongue curled", examples: ["zhōngguó (China)", "zhè (this)"] },
                { pinyin: "ch", english: "like 'ch' in 'church'", desc: "'ch' sound with tongue curled", examples: ["chī (eat)", "chá (tea)"] },
                { pinyin: "sh", english: "like 'sh' in 'shoe'", desc: "'sh' sound with tongue curled", examples: ["shū (book)", "shénme (what)"] },
                { pinyin: "r", english: "like 'r' in 'rain' but softer", desc: "Soft 'r' sound", examples: ["rì (sun/day)", "rè (hot)"] },
                { pinyin: "z", english: "like 'ds' in 'kids'", desc: "'ds' sound", examples: ["zǎo (morning)", "zìjǐ (self)"] },
                { pinyin: "c", english: "like 'ts' in 'cats'", desc: "'ts' sound", examples: ["cì (time)", "cài (vegetable)"] },
                { pinyin: "s", english: "like 's' in 'sun'", desc: "'s' sound", examples: ["sān (three)", "sūzhōu (Suzhou)"] },
                { pinyin: "y", english: "like 'y' in 'yes'", desc: "Consonant 'y' sound", examples: ["yī (one)", "yáng (sheep)"] },
                { pinyin: "w", english: "like 'w' in 'we'", desc: "Consonant 'w' sound", examples: ["wǒ (I/me)", "wǔ (five)"] }
            ],
            simpleFinals: [
                { pinyin: "a", english: "like 'a' in 'father'", desc: "Open 'a' sound", examples: ["mā (mom)", "bà (dad)"] },
                { pinyin: "o", english: "like 'o' in 'or'", desc: "Round 'o' sound", examples: ["bō (wave)", "mó (rub)"] },
                { pinyin: "e", english: "like 'e' in 'her'", desc: "Neutral 'e' sound", examples: ["hē (drink)", "lè (happy)"] },
                { pinyin: "i", english: "like 'ee' in 'see'", desc: "Long 'ee' sound", examples: ["nǐ (you)", "lì (strength)"] },
                { pinyin: "u", english: "like 'oo' in 'food'", desc: "Round 'oo' sound", examples: ["shū (book)", "hǔ (tiger)"] },
                { pinyin: "ü", english: "like French 'u' in 'tu'", desc: "Pursed lips 'u' sound", examples: ["nǚ (female)", "lǜ (green)"] }
            ],
            compoundFinals: [
                { pinyin: "ai", english: "like 'i' in 'hi'", desc: "'a' to 'i' glide", examples: ["hǎi (sea)", "lái (come)"] },
                { pinyin: "ei", english: "like 'ay' in 'say'", desc: "'e' to 'i' glide", examples: ["hēi (black)", "méi (not have)"] },
                { pinyin: "ao", english: "like 'ow' in 'how'", desc: "'a' to 'o' glide", examples: ["hǎo (good)", "māo (cat)"] },
                { pinyin: "ou", english: "like 'o' in 'go'", desc: "'o' to 'u' glide", examples: ["gǒu (dog)", "zhōu (week)"] },
                { pinyin: "ia", english: "like 'ya' in 'yacht'", desc: "'i' to 'a' glide", examples: ["jiā (home)", "xià (down)"] },
                { pinyin: "ie", english: "like 'ye' in 'yes'", desc: "'i' to 'e' glide", examples: ["xiè (thank)", "jié (festival)"] },
                { pinyin: "ua", english: "like 'wa' in 'water'", desc: "'u' to 'a' glide", examples: ["wà (socks)", "huā (flower)"] },
                { pinyin: "uo", english: "like 'wo' in 'won't'", desc: "'u' to 'o' glide", examples: ["duō (many)", "guò (pass)"] },
                { pinyin: "üe", english: "like 'ue' in French 'duel'", desc: "'ü' to 'e' glide", examples: ["xuě (snow)", "yuè (moon)"] },
                { pinyin: "iao", english: "like 'yow' in 'meow'", desc: "'i' to 'ao' glide", examples: ["xiǎo (small)", "niǎo (bird)"] },
                { pinyin: "iou", english: "like 'yo' in 'yoyo'", desc: "'i' to 'ou' glide (often written as 'iu')", examples: ["liú (stay)", "qiú (ball)"] },
                { pinyin: "uai", english: "like 'why'", desc: "'u' to 'ai' glide", examples: ["kuài (fast)", "huài (bad)"] },
                { pinyin: "uei", english: "like 'way'", desc: "'u' to 'ei' glide (often written as 'ui')", examples: ["huì (can)", "duì (correct)"] }
            ],
            nasalFinals: [
                { pinyin: "an", english: "like 'an' in 'fan'", desc: "'a' + nasal 'n'", examples: ["hàn (Chinese)", "bān (class)"] },
                { pinyin: "en", english: "like 'en' in 'pen'", desc: "'e' + nasal 'n'", examples: ["hěn (very)", "rén (person)"] },
                { pinyin: "in", english: "like 'een' in 'seen'", desc: "'i' + nasal 'n'", examples: ["xīn (new)", "jīn (gold)"] },
                { pinyin: "un", english: "like 'oon' in 'soon'", desc: "'u' + nasal 'n'", examples: ["hún (marry)", "lún (wheel)"] },
                { pinyin: "ün", english: "like French 'un'", desc: "'ü' + nasal 'n'", examples: ["yún (cloud)", "jūn (army)"] },
                { pinyin: "ang", english: "like 'ang' in 'sang'", desc: "'a' + nasal 'ng'", examples: ["fáng (house)", "táng (sugar)"] },
                { pinyin: "eng", english: "like 'ung' in 'lung'", desc: "'e' + nasal 'ng'", examples: ["fēng (wind)", "dēng (light)"] },
                { pinyin: "ing", english: "like 'ing' in 'sing'", desc: "'i' + nasal 'ng'", examples: ["míng (bright)", "qīng (light color)"] },
                { pinyin: "ong", english: "like 'ong' in 'song'", desc: "'o' + nasal 'ng'", examples: ["hóng (red)", "gōng (work)"] },
                { pinyin: "ian", english: "like 'yen'", desc: "'i' to 'an' glide", examples: ["tiān (sky)", "jiàn (see)"] },
                { pinyin: "uan", english: "like 'wan' in 'want'", desc: "'u' to 'an' glide", examples: ["guān (close)", "huàn (change)"] },
                { pinyin: "üan", english: "like 'yuan'", desc: "'ü' to 'an' glide", examples: ["quán (all)", "xuǎn (choose)"] },
                { pinyin: "iang", english: "like 'yang'", desc: "'i' to 'ang' glide", examples: ["liǎng (two)", "xiǎng (want)"] },
                { pinyin: "uang", english: "like 'wang'", desc: "'u' to 'ang' glide", examples: ["huáng (yellow)", "zhuāng (dress)"] },
                { pinyin: "ueng", english: "like 'weng'", desc: "'u' to 'eng' glide", examples: ["wèng (jar)"] },
                { pinyin: "iong", english: "like 'yong'", desc: "'i' to 'ong' glide", examples: ["xiōng (older brother)", "qióng (poor)"] }
            ],
            tones: [
                { pinyin: "ā", english: "High and flat (like singing 'la')", desc: "First tone - high and level", examples: ["mā (mom)", "bā (eight)"] },
                { pinyin: "á", english: "Rising (like asking 'What?')", desc: "Second tone - rising", examples: ["má (hemp)", "bá (pull out)"] },
                { pinyin: "ǎ", english: "Dipping (like saying 'well...')", desc: "Third tone - falls then rises", examples: ["mǎ (horse)", "bǎ (hold)"] },
                { pinyin: "à", english: "Falling (like a command 'Stop!')", desc: "Fourth tone - sharp fall", examples: ["mà (scold)", "bà (dad)"] },
                { pinyin: "a", english: "Light and short (no mark)", desc: "Neutral tone - short and light", examples: ["ma (question particle)", "ba (suggestion particle)"] }
            ]
        };

        // Initialize the app when DOM is loaded
        document.addEventListener('DOMContentLoaded', function() {
            // Create buttons for each category
            createButtons('initials', pinyinData.initials);
            createButtons('simple-finals', pinyinData.simpleFinals);
            createButtons('compound-finals', pinyinData.compoundFinals);
            createButtons('nasal-finals', pinyinData.nasalFinals);
            createButtons('tones', pinyinData.tones);
            
            // Set up close button for info panel
            document.getElementById('close-btn').addEventListener('click', function() {
                document.getElementById('info-panel').classList.remove('visible');
            });
        });

        // Function to create buttons for a category
        function createButtons(categoryId, items) {
            const container = document.querySelector(`#${categoryId} .buttons-container`);
            
            items.forEach(item => {
                const button = document.createElement('button');
                button.className = 'pinyin-btn';
                button.textContent = item.pinyin;
                
                button.addEventListener('click', function() {
                    showInfo(item);
                });
                
                container.appendChild(button);
            });
        }

        // Function to show information in the panel
        function showInfo(item) {
            document.getElementById('pinyin-display').textContent = item.pinyin;
            document.getElementById('english-sound').textContent = `Sounds like: ${item.english}`;
            document.getElementById('description').textContent = item.desc;
            
            const examplesList = document.getElementById('examples-list');
            examplesList.innerHTML = '';
            
            item.examples.forEach(example => {
                const li = document.createElement('li');
                li.textContent = example;
                examplesList.appendChild(li);
            });
            
            document.getElementById('info-panel').classList.add('visible');
        }
    </script>
</body>
</html>
