<!DOCTYPE html>
<html>
<head>
    <title>Roblox AI Helper</title>
    <style>
        body {
            font-family: Arial;
            background: #1e3a8a;
            padding: 20px;
            margin: 0;
        }
        
        body::before {
            content: '6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            font-size: 300px;
            font-weight: bold;
            color: rgba(59, 130, 246, 0.15);
            z-index: 0;
            pointer-events: none;
            line-height: 1.2;
            letter-spacing: 80px;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }
        
        h1 {
            color: white;
            text-align: center;
        }
        
        .chat {
            background: white;
            border-radius: 10px;
            padding: 20px;
        }
        
        #messages {
            height: 400px;
            overflow-y: scroll;
            border: 2px solid #3b82f6;
            padding: 10px;
            margin-bottom: 20px;
            background: #eff6ff;
        }
        
        .message {
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
        }
        
        .user {
            background: #2563eb;
            color: white;
            text-align: right;
        }
        
        .bot {
            background: white;
            border: 2px solid #93c5fd;
            color: #1e40af;
        }
        
        input {
            width: 70%;
            padding: 10px;
            border: 2px solid #3b82f6;
            border-radius: 5px;
            font-size: 16px;
        }
        
        button {
            width: 25%;
            padding: 10px;
            background: #2563eb;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        
        button:hover {
            background: #1d4ed8;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎮 Roblox AI Helper</h1>
        
        <div class="chat">
            <div id="messages"></div>
            
            <input type="text" id="input" placeholder="Ask me anything...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        // Game ideas
        var games = [
            "🏃 SPEED OBBY: Race through moving platforms with boost pads, checkpoints, and secret shortcuts! Add a leaderboard for fastest times.",
            "🍕 PIZZA SIMULATOR: Run your own pizza shop! Make pizzas, serve customers, hire staff, and expand your restaurant empire.",
            "🏰 CASTLE ADVENTURE: Explore a magical castle with quests, puzzles, hidden rooms, and a friendly dragon boss at the top!",
            "🏎️ KART RACING: Build awesome tracks with loops and jumps! Add power-ups like speed boost, shields, and oil slicks.",
            "🐾 PET COLLECTOR: Collect 100+ pets, train them, build pet homes, and compete in pet races!",
            "🗺️ TREASURE HUNT: Create a huge map with hidden treasures, solve puzzles, explore caves, and find the ultimate treasure!",
            "🏪 MALL TYCOON: Start with one shop and grow it into a massive shopping mall with restaurants, arcades, and more!",
            "⚔️ SWORD FIGHTER: Battle with different swords, unlock cool armor, complete quests, and fight epic bosses!",
            "🌈 RAINBOW OBBY: Each level is a different color! RED=fire obstacles, BLUE=ice platforms, GREEN=nature, YELLOW=lightning!",
            "🚀 SPACE EXPLORER: Journey through space, visit different planets, meet aliens, and discover the galaxy!"
        ];

        // Answers to questions
        var answers = {
            "sky blue": "The sky is blue because of Rayleigh scattering! Sunlight hits air molecules and blue light scatters more than other colors.",
            "planes fly": "Planes fly because of lift! Wings are shaped so air moves faster over the top, creating lower pressure that lifts the plane up!",
            "gravity": "Gravity is a force that pulls objects together! Earth's gravity keeps us on the ground and makes things fall down.",
            "fastest animal": "The cheetah is fastest on land at 70 mph! But the peregrine falcon can dive at 240+ mph!",
            "biggest animal": "The blue whale is the biggest animal ever - even bigger than dinosaurs! They can be 100 feet long!",
            "pandas eat": "Pandas eat bamboo - about 26 to 84 pounds every day! They spend 12-16 hours a day eating it!",
            "planets": "There are 8 planets: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, and Neptune!",
            "black hole": "A black hole is where gravity is SO strong that nothing can escape - not even light!",
            "photosynthesis": "Photosynthesis is how plants make food from sunlight! They use sunlight, water, and CO2 to create sugar and oxygen."
        };

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲",
            "What do you call a fake noodle? An impasta! 🍝",
            "What did the ocean say to the beach? Nothing, it just waved! 🌊"
        ];

        var facts = [
            "🦈 Sharks have been around longer than trees! Sharks: 400 million years, Trees: 350 million years!",
            "🍯 Honey never spoils! 3,000-year-old honey is still edible!",
            "🐙 Octopuses have 3 hearts and blue blood!",
            "⚡ Lightning is 5 times hotter than the surface of the sun!"
        ];

        // Show welcome message
        addBot("👋 Hey! I'm your Roblox AI Helper!\n\nI can help with:\n🎮 Roblox game ideas\n🔬 Science questions\n🐾 Animal facts\n🌌 Space stuff\n😂 Jokes and fun facts\n\nWhat would you like?");

        function addBot(text) {
            var div = document.createElement('div');
            div.className = 'message bot';
            div.innerText = text;
            document.getElementById('messages').appendChild(div);
            document.getElementById('messages').scrollTop = 999999;
        }

        function addUser(text) {
            var div = document.createElement('div');
            div.className = 'message user';
            div.innerText = text;
            document.getElementById('messages').appendChild(div);
            document.getElementById('messages').scrollTop = 999999;
        }

        function random(arr) {
            return arr[Math.floor(Math.random() * arr.length)];
        }

        function send() {
            var input = document.getElementById('input');
            var msg = input.value.trim();
            if (!msg) return;

            addUser(msg);
            input.value = '';

            var lower = msg.toLowerCase();
            var response = "";

            // Check for math
            if (lower.match(/\d+\s*[\+\-\*\/]\s*\d+/)) {
                try {
                    var result = eval(lower.replace(/[^0-9+\-*/().]/g, ''));
                    response = "🧮 " + lower + " = " + result;
                } catch(e) {}
            }

            // Greetings
            if (!response && (lower.includes('hello') || lower.includes('hi') || lower.includes('hey'))) {
                response = "👋 Hey! What can I help you with?";
            }

            // Thanks
            if (!response && lower.includes('thank')) {
                response = "😊 You're welcome! Need anything else?";
            }

            // Jokes
            if (!response && (lower.includes('joke') || lower.includes('funny'))) {
                response = random(jokes);
            }

            // Facts
            if (!response && (lower.includes('fact') || lower.includes('fun fact'))) {
                response = random(facts);
            }

            // Game ideas
            if (!response && (lower.includes('game') || lower.includes('obby') || lower.includes('simulator') || lower.includes('roblox') || lower.includes('idea'))) {
                response = random(games);
            }

            // Check answers
            if (!response) {
                for (var key in answers) {
                    if (lower.includes(key)) {
                        response = answers[key];
                        break;
                    }
                }
            }

            // Default
            if (!response) {
                response = "🤔 I can help with:\n\n🎮 Roblox game ideas\n🔬 Science questions\n🐾 Animal facts\n😂 Jokes and fun facts\n\nJust ask!";
            }

            setTimeout(function() {
                addBot(response);
            }, 500);
        }

        // Enter key
        document.getElementById('input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                send();
            }
        });
    </script>
</body>
</html>
            
