<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roblox AI Helper</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #1e3a8a;
            min-height: 100vh;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: '6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            font-size: 150px;
            font-weight: bold;
            color: rgba(59, 130, 246, 0.1);
            line-height: 1.5;
            word-wrap: break-word;
            z-index: 0;
            pointer-events: none;
            letter-spacing: 50px;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 20px;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.1em;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
        }

        .chat-container {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .messages {
            height: 500px;
            overflow-y: auto;
            border: 2px solid #bfdbfe;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 8px;
            background-color: #eff6ff;
        }

        .message {
            margin: 15px 0;
            padding: 12px 15px;
            border-radius: 8px;
            animation: fadeIn 0.3s ease-in;
            white-space: pre-wrap;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .user-message {
            background-color: #2563eb;
            color: white;
            margin-left: 50px;
            text-align: right;
        }

        .bot-message {
            background-color: white;
            border: 2px solid #93c5fd;
            margin-right: 50px;
            color: #1e40af;
        }

        .input-area {
            display: flex;
            gap: 10px;
        }

        #userInput {
            flex: 1;
            padding: 12px;
            border: 2px solid #3b82f6;
            border-radius: 8px;
            font-size: 16px;
        }

        #userInput:focus {
            outline: none;
            border-color: #2563eb;
        }

        #sendBtn {
            padding: 12px 30px;
            background-color: #2563eb;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
        }

        #sendBtn:hover {
            background-color: #1d4ed8;
        }

        #sendBtn:disabled {
            background-color: #93c5fd;
            cursor: not-allowed;
        }

        .typing-indicator {
            display: none;
            padding: 10px;
            color: #1e40af;
            font-style: italic;
        }

        .typing-indicator.active {
            display: block;
        }

        .suggestions {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 15px;
        }

        .suggestion-btn {
            padding: 8px 15px;
            background-color: #dbeafe;
            color: #1e40af;
            border: 2px solid #3b82f6;
            border-radius: 20px;
            cursor: pointer;
            font-size: 14px;
        }

        .suggestion-btn:hover {
            background-color: #3b82f6;
            color: white;
        }

        .messages::-webkit-scrollbar {
            width: 8px;
        }

        .messages::-webkit-scrollbar-track {
            background: #dbeafe;
            border-radius: 4px;
        }

        .messages::-webkit-scrollbar-thumb {
            background: #3b82f6;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎮 Roblox AI Helper</h1>
            <p>Ask me anything about Roblox, games, or life!</p>
        </div>

        <div class="chat-container">
            <div class="messages" id="messages"></div>
            <div class="typing-indicator" id="typingIndicator">Thinking...</div>
            <div class="suggestions" id="suggestions"></div>
            <div class="input-area">
                <input 
                    type="text" 
                    id="userInput" 
                    placeholder="Ask me anything..."
                >
                <button id="sendBtn">Send</button>
            </div>
        </div>
    </div>

    <script>
        const messagesDiv = document.getElementById('messages');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');
        const typingIndicator = document.getElementById('typingIndicator');
        const suggestionsDiv = document.getElementById('suggestions');

        const gameIdeas = [
            "🏃 SPEED OBBY: Race through moving platforms, boost pads, and secret shortcuts! Add a leaderboard for fastest times.",
            "🍕 PIZZA SIMULATOR: Run your own pizza shop! Make pizzas, serve customers, and upgrade your restaurant.",
            "🏰 CASTLE ADVENTURE: Explore a magical castle with quests, hidden rooms, and a friendly dragon boss!",
            "🏎️ RACING GAME: Build awesome race tracks with loops, jumps, and power-ups like speed boosts!",
            "🐾 PET COLLECTOR: Collect and train 100+ pets! Build pet homes and compete in pet races.",
            "🗺️ TREASURE HUNT: Create a huge map with hidden treasures, puzzles, and mysterious caves!",
            "🏪 SHOP TYCOON: Start with a small shop and grow it into a huge mall empire!",
            "⚔️ SWORD FIGHTER: Battle with different swords, unlock cool armor, and fight bosses!",
            "🌈 RAINBOW OBBY: Each level is a different color with unique obstacles and challenges!",
            "🚀 SPACE EXPLORER: Journey through space, visit planets, and discover alien friends!"
        ];

        const answers = {
            'sky blue': 'The sky is blue because of something called Rayleigh scattering! Sunlight hits air molecules, and blue light scatters more than other colors. That\'s why we see blue everywhere!',
            'planes fly': 'Planes fly because of lift! Wings are shaped so air moves faster over the top, creating lower pressure. This lifts the plane up while engines push it forward!',
            'gravity': 'Gravity is a force that pulls objects together! Earth\'s gravity keeps us on the ground. Everything with mass has gravity - even you!',
            'fastest animal': 'The cheetah is fastest on land at 70 mph! But the peregrine falcon can dive at 240+ mph, making it the fastest animal overall!',
            'biggest animal': 'The blue whale is the biggest animal ever - even bigger than dinosaurs! They can be 100 feet long and weigh 200 tons!',
            'pandas eat': 'Pandas eat bamboo - about 26 to 84 pounds every day! They spend 12-16 hours a day eating. Bamboo is basically their whole diet!',
            'make friends': 'Tips for making friends:\n1. Be yourself\n2. Show interest in others\n3. Join activities you enjoy\n4. Be kind and positive\n5. Don\'t force it - friendships take time!',
            'study better': 'Study tips:\n1. Remove distractions\n2. Take breaks every 25 minutes\n3. Teach someone else what you learned\n4. Use flashcards\n5. Get enough sleep!',
            'planets': 'There are 8 planets: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, and Neptune! Pluto is now a dwarf planet.',
            'black hole': 'A black hole is where gravity is SO strong that nothing can escape - not even light! They form when massive stars collapse.'
        };

        const jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲",
            "What do you call a fake noodle? An impasta! 🍝",
            "What did the ocean say to the beach? Nothing, it just waved! 🌊"
        ];

        const suggestions = [
            "Game idea",
            "Tell me a joke",
            "Why is the sky blue?",
            "How do I make friends?",
            "Fun fact",
            "Study tips"
        ];

        // Show initial message
        addMessage('bot', '👋 Hey! I\'m your AI helper! I can help with:\n\n🎮 Roblox game ideas\n🔬 Science questions\n😂 Jokes and fun facts\n📚 Study and life advice\n\nWhat would you like to know?');
        showSuggestions();

        function showSuggestions() {
            suggestionsDiv.innerHTML = '';
            suggestions.forEach(sug => {
                const btn = document.createElement('button');
                btn.className = 'suggestion-btn';
                btn.textContent = sug;
                btn.onclick = function() {
                    userInput.value = sug;
                    sendMessage();
                };
                suggestionsDiv.appendChild(btn);
            });
        }

        function addMessage(type, text) {
            const messageDiv = document.createElement('div');
            messageDiv.className = 'message ' + type + '-message';
            messageDiv.textContent = text;
            messagesDiv.appendChild(messageDiv);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        function getRandomItem(array) {
            return array[Math.floor(Math.random() * array.length)];
        }

        function generateResponse(message) {
            const lower = message.toLowerCase();

            // Greetings
            if (lower.includes('hello') || lower.includes('hi') || lower.includes('hey')) {
                return "👋 Hey there! What can I help you with today?";
            }

            // Thanks
            if (lower.includes('thank')) {
                return "😊 You're welcome! Happy to help anytime!";
            }

            // Jokes
            if (lower.includes('joke') || lower.includes('funny')) {
                return getRandomItem(jokes);
            }

            // Game ideas
            if (lower.includes('game') || lower.includes('idea') || lower.includes('obby') || lower.includes('simulator') || lower.includes('roblox')) {
                return getRandomItem(gameIdeas);
            }

            // Math
            if (lower.match(/\d+\s*[\+\-\*\/]\s*\d+/)) {
                try {
                    const result = eval(lower.replace(/[^0-9+\-*/().]/g, ''));
                    return '🧮 ' + lower + ' = ' + result;
                } catch(e) {
                    // continue to other checks
                }
            }

            // Check answers database
            for (let key in answers) {
                if (lower.includes(key)) {
                    return answers[key];
                }
            }

            // Fun facts
            if (lower.includes('fun fact') || lower.includes('fact')) {
                const facts = [
                    "🦈 Sharks are older than trees! They've been around for 400 million years!",
                    "🍯 Honey never spoils! 3,000-year-old honey is still edible!",
                    "🐙 Octopuses have 3 hearts and blue blood!",
                    "⚡ Lightning is 5 times hotter than the surface of the sun!"
                ];
                return getRandomItem(facts);
            }

            // Default
            return "🤔 I'm not sure about that, but I can help with:\n\n🎮 Roblox game ideas\n🔬 Science questions (ask me why the sky is blue!)\n😂 Jokes\n📚 Study tips\n\nWhat would you like to know?";
        }

        function sendMessage() {
            const message = userInput.value.trim();
            if (!message) return;

            userInput.disabled = true;
            sendBtn.disabled = true;
            typingIndicator.classList.add('active');

            addMessage('user', message);
            userInput.value = '';

            setTimeout(function() {
                const response = generateResponse(message);
                addMessage('bot', response);

                typingIndicator.classList.remove('active');
                userInput.disabled = false;
                sendBtn.disabled = false;
                userInput.focus();
            }, 1000);
        }

        // Click button
        sendBtn.addEventListener('click', sendMessage);

        // Press Enter
        userInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>
            
