
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roblox Studio Ideas Bot</title>
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
            max-width: 800px;
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
            transition: border-color 0.3s;
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
            transition: background-color 0.3s;
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
            transition: all 0.3s;
        }

        .suggestion-btn:hover {
            background-color: #3b82f6;
            color: white;
        }

        /* Custom scrollbar for messages */
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

        .messages::-webkit-scrollbar-thumb:hover {
            background: #2563eb;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎮 Roblox Studio Ideas Bot</h1>
            <p>Get creative ideas for your next Roblox game!</p>
        </div>

        <div class="chat-container">
            <div class="messages" id="messages"></div>
            <div class="typing-indicator" id="typingIndicator">Bot is typing...</div>
            <div class="suggestions" id="suggestions"></div>
            <div class="input-area">
                <input 
                    type="text" 
                    id="userInput" 
                    placeholder="Ask for Roblox Studio game ideas..."
                    onkeypress="handleKeyPress(event)"
                >
                <button onclick="sendMessage()" id="sendBtn">Send</button>
            </div>
        </div>
    </div>

    <script>
        const messagesDiv = document.getElementById('messages');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');
        const typingIndicator = document.getElementById('typingIndicator');
        const suggestionsDiv = document.getElementById('suggestions');

        // Game ideas database
        const gameIdeas = {
            obby: [
                "🏃 Speed Runner Obby: Create an obstacle course where players race against the clock! Add checkpoints, moving platforms, and fun obstacles like spinning hammers and disappearing floors.",
                "🌈 Rainbow Obby Adventure: Make a colorful obby where each level is a different color! Add rainbow bridges, color-changing platforms, and collect stars along the way.",
                "🚀 Space Obby: Build an obby in space! Add floating asteroids to jump on, rocket boosters, and gravity-switching zones where you can walk on walls!"
            ],
            simulator: [
                "🐾 Pet Collection Simulator: Create a game where players can collect cute pets! Add different pets to find, feeding mechanics, and a cozy pet home to decorate.",
                "🍕 Pizza Shop Simulator: Run your own pizza restaurant! Players can make pizzas, serve customers, upgrade their shop, and unlock new toppings.",
                "🏝️ Island Builder Simulator: Players start on a small island and can expand it! Add trees to plant, buildings to unlock, and islands to discover."
            ],
            adventure: [
                "🗺️ Treasure Hunt Adventure: Create a big map with hidden treasures! Add a treasure map, mysterious caves, and puzzles to solve to find the ultimate treasure.",
                "🏰 Castle Quest: Build a magical castle with different rooms to explore! Add friendly NPCs, secret passages, and quests to complete.",
                "🌲 Forest Explorer: Make a mysterious forest to explore! Add cute animals, hidden paths, berry collecting, and a treehouse base."
            ],
            racing: [
                "🏎️ Kart Racing Track: Design a fun racing track with loops and jumps! Add power-ups like speed boost and shields, plus different karts to unlock.",
                "🛹 Skateboard Park Challenge: Create a skateboard park where players do tricks! Add ramps, rails to grind, and a scoring system for cool tricks.",
                "🚗 Car Customizer Race: Make a racing game where players customize their cars first! Add paint colors, decals, spoilers, and then race on cool tracks."
            ],
            tycoon: [
                "🏪 Shop Tycoon: Build and grow your own store! Start small and upgrade to add more items, decorations, and employees.",
                "🎢 Theme Park Tycoon: Create your own amusement park! Add rides, food stands, decorations, and make visitors happy.",
                "🏗️ City Builder Tycoon: Start with one building and grow a whole city! Add houses, shops, parks, and roads to connect everything."
            ],
            roleplay: [
                "🏡 House Roleplay: Create a neighborhood where players can have their own houses! Add furniture to buy, pets to adopt, and fun activities.",
                "🎒 School Roleplay: Build a school with different classrooms! Add lockers, a cafeteria, playground, and different roles like student and teacher.",
                "🏥 Hospital Roleplay: Make a friendly hospital! Players can be doctors, nurses, or patients. Add different rooms and medical tools."
            ],
            tower_defense: [
                "🗼 Castle Defense: Protect your castle from silly monsters! Place towers that shoot, freeze, or bounce enemies away. Each tower has special powers!",
                "🌸 Garden Defense: Defend your garden from mischievous bugs! Use flower towers, water sprayers, and ladybug helpers.",
                "🏖️ Beach Defense: Stop crabs and sea creatures from taking your sandcastle! Build sand towers, use water balloons, and recruit seagull helpers."
            ],
            horror: [
                "👻 Friendly Ghost House: A spooky but not-too-scary game! Explore a mansion with silly ghosts who play pranks. Find hidden candies and solve light puzzles.",
                "🌙 Moonlight Mystery: A nighttime adventure in a forest with glowing creatures! Not scary, just mysterious. Find magical fireflies and discover secrets.",
                "🎃 Pumpkin Patch Adventure: Visit a magical pumpkin patch at night! Meet friendly scarecrows, collect glowing pumpkins, and solve easy riddles."
            ],
            general: [
                "🎨 Creative Building Showcase: Make a place where players can build anything! Give them different blocks, colors, and tools to create.",
                "⚽ Sports Stadium: Create a fun sports game! Could be soccer, basketball, or make up your own sport with special power-ups.",
                "🎪 Mini Games Hub: Build a game with lots of small games inside! Add a colorful lobby and different game portals to jump through.",
                "🎵 Music Dance Party: Create a disco with colored lights! Players can dance, change music, and collect dance moves.",
                "🧩 Puzzle Palace: Make a castle full of fun puzzles! Add mazes, matching games, and riddles to solve."
            ]
        };

        const tips = [
            "💡 Pro tip: Start simple! Build one feature at a time and test it before adding more.",
            "💡 Remember: Use colors that go well together to make your game look amazing!",
            "💡 Helpful hint: Add checkpoints in your game so players don't have to start over if they fail.",
            "💡 Fun idea: Add sounds and music to make your game more exciting!",
            "💡 Smart tip: Watch YouTube tutorials to learn new building techniques!",
            "💡 Pro tip: Test your game yourself before showing friends - you'll find ways to make it better!",
            "💡 Remember: Even the best game creators started as beginners. Keep practicing!"
        ];

        const quickSuggestions = [
            "Obby ideas",
            "Simulator games",
            "Racing games",
            "Adventure games",
            "Tycoon ideas",
            "Give me a tip"
        ];

        // Initialize
        addMessage('bot', '👋 Hi! I can help you come up with fun ideas for Roblox Studio! What kind of game would you like to make? Adventure? Racing? Pet simulator? Let me know!');
        showSuggestions();

        function showSuggestions() {
            suggestionsDiv.innerHTML = '';
            quickSuggestions.forEach(suggestion => {
                const btn = document.createElement('button');
                btn.className = 'suggestion-btn';
                btn.textContent = suggestion;
                btn.onclick = () => {
                    userInput.value = suggestion;
                    sendMessage();
                };
                suggestionsDiv.appendChild(btn);
            });
        }

        function addMessage(type, text) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${type}-message`;
            messageDiv.textContent = text;
            messagesDiv.appendChild(messageDiv);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        function handleKeyPress(event) {
            if (event.key === 'Enter' && !sendBtn.disabled) {
                sendMessage();
            }
        }

        function getRandomItem(array) {
            return array[Math.floor(Math.random() * array.length)];
        }

        function generateResponse(message) {
            const lowerMessage = message.toLowerCase();

            // Check for keywords
            if (lowerMessage.includes('obby') || lowerMessage.includes('obstacle')) {
                return getRandomItem(gameIdeas.obby);
            } else if (lowerMessage.includes('simulator') || lowerMessage.includes('sim')) {
                return getRandomItem(gameIdeas.simulator);
            } else if (lowerMessage.includes('adventure') || lowerMessage.includes('explore')) {
                return getRandomItem(gameIdeas.adventure);
            } else if (lowerMessage.includes('race') || lowerMessage.includes('racing') || lowerMessage.includes('car')) {
                return getRandomItem(gameIdeas.racing);
            } else if (lowerMessage.includes('tycoon') || lowerMessage.includes('build') && lowerMessage.includes('grow')) {
                return getRandomItem(gameIdeas.tycoon);
            } else if (lowerMessage.includes('roleplay') || lowerMessage.includes('rp') || lowerMessage.includes('house')) {
                return getRandomItem(gameIdeas.roleplay);
            } else if (lowerMessage.includes('tower') || lowerMessage.includes('defense') || lowerMessage.includes('defend')) {
                return getRandomItem(gameIdeas.tower_defense);
            } else if (lowerMessage.includes('scary') || lowerMessage.includes('horror') || lowerMessage.includes('spooky')) {
                return getRandomItem(gameIdeas.horror);
            } else if (lowerMessage.includes('tip') || lowerMessage.includes('help') || lowerMessage.includes('advice')) {
                return getRandomItem(tips);
            } else if (lowerMessage.includes('hello') || lowerMessage.includes('hi') || lowerMessage.includes('hey')) {
                return "👋 Hello! I'm excited to help you create awesome Roblox games! What type of game interests you? I know about obbys, simulators, racing games, adventures, tycoons, roleplay, tower defense, and more!";
            } else if (lowerMessage.includes('thank')) {
                return "😊 You're welcome! Have fun building your game! Remember, the most important thing is to be creative and have fun!";
            } else {
                // Default: give a random general idea
                return getRandomItem(gameIdeas.general);
            }
        }

        async function sendMessage() {
            const message = userInput.value.trim();
            if (!message) return;

            // Disable input while processing
            userInput.disabled = true;
            sendBtn.disabled = true;
            typingIndicator.classList.add('active');

            // Add user message
            addMessage('user', message);
            userInput.value = '';

            // Simulate thinking time
            await new Promise(resolve => setTimeout(resolve, 1000 + Math.random() * 1000));

            // Generate response
            const response = generateResponse(message);
            addMessage('bot', response);

            // Re-enable input
            typingIndicator.classList.remove('active');
            userInput.disabled = false;
            sendBtn.disabled = false;
            userInput.focus();
        }
    </script>
</body>
</html>
