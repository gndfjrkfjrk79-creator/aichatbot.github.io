<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roblox Game Idea Generator</title>
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
            <p>Game ideas + answers to any question!</p>
        </div>

        <div class="chat-container">
            <div class="messages" id="messages"></div>
            <div class="typing-indicator" id="typingIndicator">Thinking...</div>
            <div class="suggestions" id="suggestions"></div>
            <div class="input-area">
                <input 
                    type="text" 
                    id="userInput" 
                    placeholder="Ask for game ideas or any question..."
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

        // MASSIVE Roblox game ideas database
        const gameIdeas = {
            obby: [
                "🏃 **SPEED RUNNER OBBY**: Ultimate racing challenge!\n• Moving platforms that speed up over time\n• Boost pads and slow zones\n• Checkpoints every 10 obstacles\n• Difficulties: Easy, Medium, Hard, INSANE\n• Leaderboard for fastest times\n• Secret shortcuts for skilled players\n• Rainbow completion trail effect",
                
                "🌈 **RAINBOW COLOR OBBY**: Every level is a different color!\n• RED: Fire obstacles and lava jumps\n• BLUE: Slippery ice platforms\n• GREEN: Nature vines and leaf platforms\n• YELLOW: Lightning strikes and electric barriers\n• PURPLE: Gravity flip zones (walk on ceiling!)\n• Mix all colors for final challenge",
                
                "🚀 **SPACE OBBY ADVENTURE**: Journey through the galaxy!\n• Start on Earth, end at Black Hole\n• Zero gravity floating sections\n• Jump between asteroids\n• Rocket booster power-ups\n• Alien NPCs give helpful hints\n• Collect stars for space shop\n• Unlock spaceship skins and trails",
                
                "🏰 **MEDIEVAL CASTLE OBBY**: Climb the tower!\n• Escape dungeon and reach rooftop\n• Swinging axes and arrow traps\n• Knight NPCs patrol the castle\n• Friendly dragon race at the top\n• Collect gold coins for medieval shop\n• Unlock armor skins and sword trails\n• Secret treasure rooms to discover"
            ],

            simulator: [
                "🐾 **ULTIMATE PET SIMULATOR**: Complete pet paradise!\n• Collect 100+ unique pets (common to mythical)\n• Pet evolution system (3 stages)\n• Build custom pet homes with decorations\n• Mini-games: fetch, agility, pet races\n• Breeding for rare combinations\n• Pet abilities and special powers\n• Daily care: feeding, bathing, playing\n• Trading system with friends",
                
                "🍕 **PIZZA EMPIRE TYCOON**: Build restaurant empire!\n• Start with small pizza stand\n• 50+ toppings to unlock\n• Hire chefs, delivery drivers, cashiers\n• Upgrade ovens for faster cooking\n• Expand to multiple locations\n• Custom pizza creator\n• Delivery mini-game\n• Pizza-making competitions",
                
                "⚔️ **SWORD MASTER SIMULATOR**: Become legendary warrior!\n• Collect 200+ legendary swords\n• Train stats: Strength, Speed, Defense\n• Battle training dummies for XP\n• Quest system from village NPCs\n• Dungeon raids with epic bosses\n• Forge new swords from materials\n• PvP arena battles\n• Unlock armor sets and special moves",
                
                "🏝️ **ISLAND BUILDER EMPIRE**: Create paradise!\n• Start on tiny island, expand\n• Plant crops: wheat, corn, fruits, trees\n• Build houses, shops, parks, beaches\n• Attract tourists for income\n• Unlock new islands (tropical, volcanic, arctic)\n• Fishing and treasure hunting\n• Hire workers to automate\n• Weather system affects gameplay"
            ],

            adventure: [
                "🗺️ **TREASURE HUNT QUEST**: Epic treasure adventure!\n• Massive map with hidden treasures\n• Find treasure map pieces\n• Solve riddles and puzzles\n• Explore mysterious caves and temples\n• Ancient traps: arrows, rolling boulders\n• Boss battle with Ancient Guardian\n• Secret treasure rooms\n• Map reveals as you explore",
                
                "🏰 **KINGDOM QUEST RPG**: Save the kingdom!\n• Choose class: Knight, Wizard, Archer, Rogue\n• 30+ quests from different NPCs\n• Battle system with skills and combos\n• Explore forests, caves, villages, castles\n• Collect weapons, armor, potions\n• Level up system (1-50)\n• Final boss: Dark Sorcerer\n• Party system for friends",
                
                "🌲 **ENCHANTED FOREST**: Magical journey!\n• Meet mystical creatures: fairies, unicorns, talking trees\n• 15 different forest zones to explore\n• Gather magical herbs and berries\n• Craft potions and spells\n• Animal companion system\n• Build treehouse hideout\n• Seasonal events and changes\n• Mystery story to uncover",
                
                "🏴‍☠️ **PIRATE ADVENTURE**: Sail the seven seas!\n• Captain your own pirate ship\n• Visit 10 different islands\n• Treasure maps lead to buried gold\n• Naval battles with enemy pirates\n• Recruit crew members\n• Upgrade ship: cannons, sails, hull\n• Sea monster encounters\n• Legendary treasure finale"
            ],

            racing: [
                "🏎️ **TURBO KART RACING**: Ultimate kart championship!\n• 20 unique tracks (city, beach, volcano, space)\n• 50+ karts to collect\n• Power-ups: rockets, shields, speed boost, oil slick\n• Customize: paint, decals, wheels, spoilers\n• Championship mode with points\n• Time trial with ghost racers\n• Multiplayer races (8 players)\n• Drift mechanics and shortcuts",
                
                "🛹 **SKATE PARK PRO**: Extreme skateboarding!\n• Massive skate park with multiple sections\n• Trick system: kickflip, ollie, grind, manual\n• Combo multiplier system\n• Collect S-K-A-T-E letters\n• Create custom skate parks\n• Unlock 30+ boards and styles\n• Sponsored challenges\n• Competition mode with rankings",
                
                "🏁 **DRAG RACING**: Quarter-mile racing!\n• Perfect launch reaction timing\n• Gear shift mechanics\n• Nitrous oxide boost system\n• Tune cars: engine, tires, transmission\n• 40+ cars to unlock\n• Underground racing story mode\n• Pink slip races (winner takes car)\n• Custom wraps and underglow"
            ],

            tycoon: [
                "🏪 **MEGA MALL TYCOON**: Shopping empire!\n• Start with 1 shop, expand to 50+\n• Different store types: clothing, food, electronics, toys\n• Hire employees and security guards\n• Parking lot with valet service\n• Food court with multiple restaurants\n• Movie theater and arcade\n• Seasonal sales and events\n• Customer satisfaction system",
                
                "🎢 **THEME PARK EMPIRE**: Build amusement park!\n• Design 30+ different rides\n• Custom roller coaster builder\n• Food stands and game booths\n• Hire performers and mascots\n• Queue line management\n• Park cleanliness matters\n• Nightly fireworks shows\n• Seasonal themes (Halloween, Christmas)",
                
                "🏗️ **CITY BUILDER**: Grow your metropolis!\n• Start as village, become mega city\n• Zone areas: residential, commercial, industrial\n• Build infrastructure: roads, power, water\n• Unlock 100+ building types\n• Manage budget and taxes\n• Public services: police, fire, hospital\n• Transportation: buses, subway, trains\n• Citizen happiness meter"
            ]
        };

        // General knowledge for ANY question
        const knowledge = {
            science: {
                'sky blue': '🌤️ The sky is blue because of Rayleigh scattering! Sunlight hits air molecules, and blue light scatters more than other colors because it has shorter wavelengths. At sunset, light travels through more atmosphere, so we see reds and oranges!',
                'planes fly': '✈️ Planes fly because of lift! Wings are shaped so air moves faster over the top than the bottom. This creates lower pressure on top, lifting the plane up! Engines push it forward, and the wing shape keeps it airborne.',
                'gravity': '🌍 Gravity is the force that pulls objects together! Earth\'s gravity keeps us on the ground and makes things fall. Everything with mass has gravity - even you! But Earth is so massive, its gravity is very strong.',
                'photosynthesis': '🌱 Photosynthesis is how plants make food from sunlight! They use sunlight, water, and CO2 to create sugar (food) and oxygen (for us to breathe!). It happens in chloroplasts with chlorophyll (the green stuff)!',
                'rainbow': '🌈 Rainbows form when sunlight shines through water droplets! Light bends going into the droplet, reflects off the back, then bends again coming out. Each color bends differently, creating the rainbow: red, orange, yellow, green, blue, indigo, violet!'
            },

            animals: {
                'fastest animal': '🐆 The cheetah is fastest on land at 70 mph! But the peregrine falcon can dive at 240+ mph - fastest overall! In water, the sailfish swims 68 mph!',
                'biggest animal': '🐋 The blue whale is the biggest animal EVER - even bigger than dinosaurs! They grow up to 100 feet long and weigh 200 tons (as heavy as 33 elephants!). Their heart is the size of a small car!',
                'pandas eat': '🐼 Pandas eat bamboo - about 26 to 84 pounds every day! They spend 12-16 hours a day eating. Even though they\'re bears, 99% of their diet is bamboo!',
                'penguins fly': '🐧 Penguins can\'t fly in air, but they "fly" underwater! They use wings as flippers to swim super fast - some reach 22 mph! They\'re amazing swimmers!',
                'dogs live': '🐕 Dogs usually live 10-13 years, depending on breed! Smaller dogs often live longer (12-16 years) while bigger dogs live shorter (8-12 years). With good care and love, dogs live happy long lives!'
            },

            space: {
                'sun size': '☀️ The sun is MASSIVE! It\'s 109 times wider than Earth! You could fit 1.3 million Earths inside it! Even though it\'s 93 million miles away, we still feel its heat!',
                'planets': '🪐 Our solar system has 8 planets: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, and Neptune! Pluto is now a dwarf planet.',
                'black hole': '⚫ A black hole is where gravity is SO strong that nothing can escape - not even light! They form when massive stars collapse. We call them "black" because they don\'t let light out.',
                'moon phases': '🌙 Moon phases happen because we see different amounts of sunlight on the moon! New Moon (dark), Crescent, Quarter (half), Gibbous, Full Moon. It takes 29.5 days for a full cycle!'
            },

            math: {
                'pi': '🥧 Pi (π) is approximately 3.14159... It\'s the ratio of a circle\'s circumference to its diameter. It goes on forever without repeating! We use it to calculate circles and spheres. Pi Day is March 14!',
                'fractions': '🍕 Fractions are parts of a whole! Like if you cut a pizza into 8 slices and eat 3, you ate 3/8 of the pizza! Top number = parts you have, Bottom number = total parts.',
                'percentage': '💯 Percentages are parts out of 100! 50% means 50 out of 100 (half). 100% = all, 50% = half, 25% = quarter, 0% = none. To find 20% of 50: (20/100) × 50 = 10!'
            },

            jokes: [
                "Why don't scientists trust atoms? Because they make up everything! 😄",
                "What do you call a bear with no teeth? A gummy bear! 🐻",
                "Why did the scarecrow win an award? He was outstanding in his field! 🌾",
                "What do you call a dinosaur that crashes his car? Tyrannosaurus Wrecks! 🦖",
                "What did the ocean say to the beach? Nothing, it just waved! 🌊",
                "Why did the bicycle fall over? It was two-tired! 🚲",
                "What do you call a fake noodle? An impasta! 🍝",
                "Why don't eggs tell jokes? They'd crack up! 🥚"
            ],

            facts: [
                "🦈 Sharks have been around longer than trees! Sharks: 400 million years, Trees: 350 million years!",
                "🍯 Honey never spoils! 3,000-year-old honey found in Egyptian tombs is still edible!",
                "🐙 Octopuses have 3 hearts and blue blood! Two pump blood to gills, one to the body!",
                "⚡ Lightning is 5 times hotter than the surface of the sun!",
                "🧠 Your brain uses 20% of your body's energy but is only 2% of your weight!"
            ]
        };

        const suggestions = [
            "🎮 Obby idea",
            "🐾 Simulator idea",
            "🏰 Adventure idea",
            "🏎️ Racing idea",
            "😂 Tell a joke",
            "🌟 Fun fact"
        ];

        // Show welcome message
        addMessage('bot', '👋 Hey! I\'m your Roblox AI Helper!\n\nI can help with:\n🎮 Roblox game ideas (obbys, simulators, adventures, racing, tycoons)\n🔬 Science questions\n🐾 Animal facts\n🌌 Space stuff\n🧮 Math help\n😂 Jokes and fun facts\n\nWhat would you like?');
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

        function calculateMath(message) {
            const cleanMsg = message.toLowerCase()
                .replace(/what is|what's|calculate|equals/gi, '')
                .replace(/plus/gi, '+')
                .replace(/minus/gi, '-')
                .replace(/times/gi, '*')
                .replace(/divided by/gi, '/')
                .trim();
            
            if (/^[\d\s+\-*/().]+$/.test(cleanMsg)) {
                try {
                    const result = eval(cleanMsg);
                    return '🧮 ' + cleanMsg + ' = ' + result;
                } catch (e) {
                    return null;
                }
            }
            return null;
        }

        function generateResponse(message) {
            const lower = message.toLowerCase();

            // Try math first
            const mathResult = calculateMath(message);
            if (mathResult) return mathResult;

            // Greetings
            if (lower.includes('hello') || lower.includes('hi') || lower.includes('hey')) {
                return "👋 Hey! What's up? Want a Roblox game idea or have a question?";
            }

            // Thanks
            if (lower.includes('thank')) {
                return "😊 You're welcome! Need anything else?";
            }

            // Jokes
            if (lower.includes('joke') || lower.includes('funny')) {
                return getRandomItem(knowledge.jokes);
            }

            // Fun facts
            if (lower.includes('fun fact') || lower.includes('fact')) {
                return getRandomItem(knowledge.facts);
            }

            // ROBLOX GAME IDEAS - Priority!
            if (lower.includes('obby') || lower.includes('obstacle')) {
                return getRandomItem(gameIdeas.obby);
            }
            if (lower.includes('simulator') || lower.includes('sim')) {
                return getRandomItem(gameIdeas.simulator);
            }
            if (lower.includes('adventure') || lower.includes('quest') || lower.includes('explore')) {
                return getRandomItem(gameIdeas.adventure);
            }
            if (lower.includes('racing') || lower.includes('race') || lower.includes('car')) {
                return getRandomItem(gameIdeas.racing);
            }
            if (lower.includes('tycoon') || lower.includes('business') || lower.includes('empire')) {
                return getRandomItem(gameIdeas.tycoon);
            }
            if (lower.includes('game') || lower.includes('roblox') || lower.includes('idea')) {
                const allGames = Object.values(gameIdeas).flat();
                return getRandomItem(allGames);
            }

            // Science questions
            for (let key in knowledge.science) {
                if (lower.includes(key)) {
                    return knowledge.science[key];
                }
            }

            // Animal questions
            for (let key in knowledge.animals) {
                if (lower.includes(key)) {
                    return knowledge.animals[key];
                }
            }

            // Space questions
            for (let key in knowledge.space) {
                if (lower.includes(key)) {
                    return knowledge.space[key];
                }
            }

            // Math questions
            for (let key in knowledge.math) {
                if (lower.includes(key)) {
                    return knowledge.math[key];
                }
            }

            // Default helpful response
            return "🤔 I can help with:\n\n🎮 Roblox game ideas (just ask for 'game idea'!)\n🔬 Science questions (why is the sky blue?)\n🐾 Animal facts\n🌌 Space stuff\n🧮 Math help (or just type math like '5+3')\n😂 Jokes and fun facts\n\nWhat would you like to know?";
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

        sendBtn.addEventListener('click', sendMessage);
        userInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>
            
