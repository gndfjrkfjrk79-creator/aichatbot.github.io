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
                    placeholder="Ask me anything about Roblox Studio..."
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

        // Expanded game ideas database
        const gameIdeas = {
            obby: [
                "🏃 Speed Runner Obby: Create an obstacle course where players race against the clock! Add checkpoints, moving platforms, and fun obstacles like spinning hammers and disappearing floors. You could also add a leaderboard to show the fastest times!",
                "🌈 Rainbow Obby Adventure: Make a colorful obby where each level is a different color! Add rainbow bridges, color-changing platforms, and collect stars along the way. Each color could have a different theme!",
                "🚀 Space Obby: Build an obby in space! Add floating asteroids to jump on, rocket boosters, and gravity-switching zones where you can walk on walls! Don't forget to add cool space music!",
                "🌊 Water World Obby: Create an obby that takes place underwater and above water! Add swimming sections, water slides, diving boards, and boat platforms to jump between.",
                "🎪 Circus Obby: Make a circus-themed obby with tightropes, trampolines, cannon launchers, and spinning circus wheels! Add clown NPCs to cheer players on."
            ],
            simulator: [
                "🐾 Pet Collection Simulator: Create a game where players can collect cute pets! Add different pets to find, feeding mechanics, and a cozy pet home to decorate. Players can also train their pets to do tricks!",
                "🍕 Pizza Shop Simulator: Run your own pizza restaurant! Players can make pizzas, serve customers, upgrade their shop, and unlock new toppings. Add a delivery system too!",
                "🏝️ Island Builder Simulator: Players start on a small island and can expand it! Add trees to plant, buildings to unlock, and islands to discover. Include fishing and treasure hunting!",
                "⚔️ Knight Training Simulator: Train to become a knight! Players can practice sword fighting, complete quests, upgrade armor, and protect the kingdom from silly dragons.",
                "🧙 Magic School Simulator: Attend a school of magic! Learn spells, collect wands, brew potions, and complete magical homework. Add different magical subjects like flying and potion-making!"
            ],
            adventure: [
                "🗺️ Treasure Hunt Adventure: Create a big map with hidden treasures! Add a treasure map, mysterious caves, and puzzles to solve to find the ultimate treasure. Include riddles and secret passages!",
                "🏰 Castle Quest: Build a magical castle with different rooms to explore! Add friendly NPCs who give quests, secret passages, hidden keys, and a dragon in the dungeon (a friendly one!).",
                "🌲 Forest Explorer: Make a mysterious forest to explore! Add cute animals, hidden paths, berry collecting, and a treehouse base. Include a day/night cycle for extra atmosphere!",
                "🏔️ Mountain Climbing Adventure: Create a big mountain to climb! Add different paths, camping spots, wildlife to observe, and a beautiful view at the top with a flag to plant.",
                "🏺 Ancient Temple Explorer: Explore an ancient temple full of mysteries! Add trap rooms (fun ones!), ancient puzzles, treasure rooms, and hieroglyphics that tell a story."
            ],
            racing: [
                "🏎️ Kart Racing Track: Design a fun racing track with loops and jumps! Add power-ups like speed boost and shields, plus different karts to unlock. Create shortcuts for skilled players!",
                "🛹 Skateboard Park Challenge: Create a skateboard park where players do tricks! Add ramps, rails to grind, and a scoring system for cool tricks. Include combo multipliers!",
                "🚗 Car Customizer Race: Make a racing game where players customize their cars first! Add paint colors, decals, spoilers, and then race on cool tracks with different weather.",
                "🏇 Horse Racing Derby: Race cute horses through meadows and forests! Players can train their horses, brush them, and feed them carrots for speed boosts.",
                "🚁 Helicopter Racing: Race helicopters through obstacle courses in the sky! Add rings to fly through, boost pads on clouds, and different helicopters to unlock."
            ],
            tycoon: [
                "🏪 Shop Tycoon: Build and grow your own store! Start small and upgrade to add more items, decorations, and employees. Expand to multiple shops!",
                "🎢 Theme Park Tycoon: Create your own amusement park! Add rides, food stands, decorations, and make visitors happy. Design custom roller coasters!",
                "🏗️ City Builder Tycoon: Start with one building and grow a whole city! Add houses, shops, parks, and roads to connect everything. Add a subway system for extra fun!",
                "🍔 Restaurant Chain Tycoon: Build a restaurant empire! Start with one small restaurant and expand across the map. Hire chefs, create menus, and serve happy customers.",
                "🎮 Game Studio Tycoon: Run your own game development studio! Hire programmers, designers, and artists. Create games and watch them succeed!"
            ],
            roleplay: [
                "🏡 Neighborhood Roleplay: Create a neighborhood where players can have their own houses! Add furniture to buy, pets to adopt, jobs to work, and fun activities like barbecues.",
                "🎒 School Roleplay: Build a school with different classrooms! Add lockers, a cafeteria, playground, library, and different roles like student, teacher, and principal.",
                "🏥 Hospital Roleplay: Make a friendly hospital! Players can be doctors, nurses, or patients. Add different rooms, medical tools, and maybe even a gift shop!",
                "🏨 Hotel Roleplay: Run a fancy hotel! Players can be guests, staff, or the manager. Add a pool, restaurant, fancy rooms, and a lobby with a check-in desk.",
                "🌆 City Life Roleplay: Create a whole city to roleplay in! Add apartments, jobs, stores, a park, and transportation. Players can be anything from chefs to firefighters!"
            ],
            tower_defense: [
                "🗼 Castle Defense: Protect your castle from silly monsters! Place towers that shoot, freeze, or bounce enemies away. Each tower has special powers! Add knights and archers.",
                "🌸 Garden Defense: Defend your garden from mischievous bugs! Use flower towers, water sprayers, and ladybug helpers. Make it colorful and fun!",
                "🏖️ Beach Defense: Stop crabs and sea creatures from taking your sandcastle! Build sand towers, use water balloons, and recruit seagull helpers.",
                "🍭 Candy Land Defense: Protect your candy kingdom! Use lollipop shooters, chocolate walls, and gummy bear soldiers against candy-stealing ants.",
                "🏔️ Snow Fort Defense: Defend your snow fort from snowball-throwing penguins! Build ice towers, snowman guards, and use avalanche traps!"
            ],
            horror_light: [
                "👻 Friendly Ghost House: A spooky but not-too-scary game! Explore a mansion with silly ghosts who play pranks. Find hidden candies and solve light puzzles. More funny than scary!",
                "🌙 Moonlight Mystery: A nighttime adventure in a forest with glowing creatures! Not scary, just mysterious. Find magical fireflies and discover secrets.",
                "🎃 Pumpkin Patch Adventure: Visit a magical pumpkin patch at night! Meet friendly scarecrows, collect glowing pumpkins, and solve easy riddles.",
                "🦇 Batty's Mansion Tour: A cute bat named Batty gives you a tour of his mansion! It's dark but fun, with silly surprises and treasure to find.",
                "🕷️ Spider's Web Maze: Navigate a giant spider web (the spider is friendly and helps you!). Find your way through with glowing markers and fun music."
            ],
            fighting: [
                "🥊 Super Hero Battle Arena: Create characters with super powers! Add punch, kick, and special power moves. Make different arenas like a city or volcano.",
                "⚔️ Medieval Sword Fighting: Knights battle with swords and shields! Add blocking, different sword types, and armor upgrades. Include a tournament mode!",
                "🤖 Robot Battle Bots: Build and battle robots! Players customize their robots with different parts, weapons, and colors. Add an arena with hazards.",
                "🥋 Ninja Training Dojo: Learn ninja moves and battle other ninjas! Add stealth moves, throwing stars, and wall-climbing abilities.",
                "🦖 Dinosaur Battle: Control different dinosaurs and battle! Each dinosaur has special abilities. T-Rex is strong, Raptor is fast, and Pterodactyl can fly!"
            ],
            puzzle: [
                "🧩 Puzzle Palace: Make a castle full of fun puzzles! Add mazes, matching games, riddles, and color-matching challenges.",
                "🔍 Mystery Detective Game: Players solve mysteries by finding clues! Add magnifying glass tool, clue notebook, and different cases to solve.",
                "🎯 Brain Teaser Park: An amusement park where each ride is a puzzle! Add memory games, pattern matching, and logic puzzles.",
                "🔢 Number Quest Adventure: Make math fun! Players solve number puzzles to unlock doors, find treasures, and defeat silly monsters with math.",
                "🎨 Color Mixing Lab: A science lab where players mix colors to solve puzzles! Learn color theory while having fun creating the right shades."
            ],
            survival: [
                "🏝️ Island Survival: Players are stranded on an island! Add crafting, building shelters, finding food, and coconut trees. Keep it fun and not too hard!",
                "❄️ Arctic Explorer: Survive in a snowy world! Build igloos, catch fish, and stay warm by campfires. Add friendly polar bears and penguins!",
                "🌋 Volcano Escape: The volcano is erupting! Work with friends to build boats, collect supplies, and escape the island before lava flows!",
                "🌵 Desert Adventure: Survive in the desert! Find oases, ride camels, build shade, and discover ancient pyramids with treasures.",
                "🌲 Forest Camp: Set up camp in the woods! Gather sticks for fire, set up tents, roast marshmallows, and go on nature hikes. Very peaceful and fun!"
            ]
        };

        // HOW-TO guides
        const howToGuides = {
            start: "🎮 To start making games in Roblox Studio:\n1. Download Roblox Studio (it's free!)\n2. Open it and click 'New'\n3. Choose a template or start with 'Baseplate'\n4. Use the Explorer window to add parts\n5. Press F5 to test your game!\n\nStart simple - try making a platform to stand on first!",
            
            parts: "🧱 To add parts in Roblox Studio:\n1. Click the 'Part' button in the Home tab\n2. Or click Model tab → Part\n3. Choose shape: Block, Sphere, Cylinder, or Wedge\n4. Use the Move tool to position it\n5. Use the Scale tool to change size\n6. Change color in Properties window!\n\nParts are the building blocks of everything!",
            
            script: "📝 To add scripts in Roblox Studio:\n1. Click the part you want to script\n2. Press the '+' in Explorer next to the part\n3. Choose 'Script' (for server) or 'LocalScript' (for player only)\n4. Type your Lua code inside!\n5. Press F5 to test it!\n\nStart with simple scripts like changing colors or printing messages!",
            
            colors: "🎨 To change colors in Roblox Studio:\n1. Select the part you want to color\n2. Look at Properties window (usually on right)\n3. Find 'BrickColor' or 'Color'\n4. Click it to open color picker\n5. Choose your favorite color!\n\nYou can also use the 'Material' property to make things look like wood, metal, grass, etc!",
            
            testing: "🎮 To test your game:\n1. Press F5 to start playing\n2. Move with WASD keys\n3. Jump with Space\n4. Look around with your mouse\n5. Press F5 again to stop testing\n\nAlways test your game often to make sure everything works!",
            
            publish: "🌟 To publish your game:\n1. Click File → Publish to Roblox\n2. Give your game a cool name\n3. Write a description\n4. Choose if it's public or private\n5. Click 'Create' or 'Update'\n\nMake sure to test it thoroughly first! You can update it anytime.",
            
            movement: "🏃 To make things move with scripts:\n1. Add a Script to the part\n2. Use: part.Position = Vector3.new(x, y, z)\n3. Or use: part.CFrame = CFrame.new(x, y, z)\n4. Put it in a loop to keep moving!\n5. Add wait() to control speed\n\nExample: while true do wait(0.1) part.Position = part.Position + Vector3.new(1,0,0) end",
            
            teleport: "✨ To make a teleporter:\n1. Create two parts (one for start, one for end)\n2. Add a Script to the start part\n3. Detect when player touches it\n4. Move the player to the end part\n5. Make it glow so players know it's special!\n\nUse touched event and CFrame to move the player!",
            
            coins: "💰 To make collectible coins:\n1. Create a coin part (cylinder works great!)\n2. Add a Script to it\n3. Use .Touched event\n4. When player touches, add to their score\n5. Make the coin disappear after collecting!\n\nYou can use Leaderstats to show the score!",
            
            sounds: "🔊 To add sounds:\n1. Find a sound on Roblox website\n2. Copy its Sound ID number\n3. In Studio, insert a Sound object\n4. Paste the ID in SoundId property\n5. Use script to play: sound:Play()\n\nSounds make games way more fun and exciting!",
            
            gui: "📱 To make a GUI (menu/buttons):\n1. Click on StarterGui in Explorer\n2. Insert a ScreenGui\n3. Add TextLabel for text or TextButton for buttons\n4. Use Properties to change size, color, and text\n5. Add LocalScript to make buttons do things!\n\nGUIs are great for health bars, shops, and menus!",
            
            lighting: "💡 To change lighting:\n1. Find 'Lighting' in Explorer\n2. Change TimeOfDay for day/night (try '14:00:00' for day)\n3. Change Brightness (higher = brighter)\n4. Add fog with FogEnd and FogColor\n5. Try ClockTime for cool effects!\n\nGood lighting makes your game look amazing!",
            
            spawn: "🎯 To make a spawn point:\n1. Insert a SpawnLocation (Model tab)\n2. Move it where you want players to start\n3. Change its color in Properties\n4. Set Duration to 0 for instant spawn\n5. Make sure it's not floating!\n\nYou can have multiple spawns for different teams!"
        };

        // Tips database
        const tips = [
            "💡 Start simple! Build one feature at a time and test it before adding more.",
            "💡 Use colors that go well together to make your game look amazing!",
            "💡 Add checkpoints in your game so players don't have to start over if they fail.",
            "💡 Sounds and music make games more exciting - add them!",
            "💡 Watch YouTube tutorials to learn new building techniques!",
            "💡 Test your game yourself before showing friends.",
            "💡 Even the best creators started as beginners. Keep practicing!",
            "💡 Use the scale tool to make big platforms quickly!",
            "💡 Copy and paste parts to build faster (Ctrl+C then Ctrl+V)!",
            "💡 Press F11 for fullscreen to see more of your game!",
            "💡 Save your game often with Ctrl+S!",
            "💡 Use groups to organize parts together!",
            "💡 The rotate tool helps make stairs and ramps!",
            "💡 Try different materials like wood, metal, and grass!",
            "💡 Lighting can change the whole mood of your game!",
            "💡 Add a waiting lobby for multiplayer games!",
            "💡 Use transparent parts to create invisible walls!",
            "💡 Anchor parts that shouldn't fall (check Anchored in Properties)!",
            "💡 Play other people's games to get ideas!",
            "💡 Ask friends to test your game and give feedback!"
        ];

        // Scripting help
        const scriptingHelp = {
            basic: "📜 Lua scripting basics for Roblox:\n\n-- This is a comment\nprint('Hello!') -- Shows message in output\n\nwait(1) -- Waits 1 second\n\nwhile true do\n    -- Code here repeats forever\n    wait(0.1)\nend\n\nVariables:\nlocal myNumber = 10\nlocal myText = 'Hello'\nlocal myPart = game.Workspace.Part\n\nStart with simple print statements to learn!",
            
            beginner: "🎯 Easy script examples:\n\n1. Make part disappear when touched:\nscript.Parent.Touched:Connect(function()\n    script.Parent.Transparency = 1\nend)\n\n2. Change color every second:\nwhile true do\n    script.Parent.BrickColor = BrickColor.Random()\n    wait(1)\nend\n\n3. Make part glow:\nlocal light = Instance.new('PointLight')\nlight.Parent = script.Parent\nlight.Brightness = 5\n\nCopy these and try them!",
            
            variables: "📦 Variables store information!\n\nThink of variables like boxes that hold things:\n\nlocal score = 0  -- Box holding a number\nlocal playerName = 'Alex'  -- Box holding text\nlocal isGameActive = true  -- Box holding true/false\n\nYou can change what's in the box:\nscore = score + 10  -- Add 10 to score\nplayerName = 'Sam'  -- Change the name\n\nUse 'local' to create new boxes!",
            
            functions: "🎪 Functions are like recipes - they do a specific job!\n\nlocal function sayHello()\n    print('Hello!')\nend\n\nsayHello()  -- This runs the function\n\nFunctions with inputs:\nlocal function addNumbers(a, b)\n    return a + b\nend\n\nlocal result = addNumbers(5, 3)  -- result is 8\n\nFunctions help organize your code!",
            
            events: "⚡ Events happen when something occurs!\n\nTouched Event (when something touches a part):\nscript.Parent.Touched:Connect(function(hit)\n    print('Something touched me!')\nend)\n\nClick Event (when player clicks):\nscript.Parent.ClickDetector.MouseClick:Connect(function()\n    print('Player clicked me!')\nend)\n\nEvents make your game interactive!",
            
            loops: "🔄 Loops repeat code!\n\nWhile loop (repeats while true):\nwhile true do\n    print('Forever!')\n    wait(1)\nend\n\nFor loop (repeats specific times):\nfor i = 1, 10 do\n    print('Count:', i)\nend\n\nFor loop through list:\nfor i, player in pairs(game.Players:GetPlayers()) do\n    print(player.Name)\nend\n\nLoops are super useful!"
        };

        // Building techniques
        const buildingTips = {
            basic: "🏗️ Basic building tips:\n\n1. Start with large parts, add details later\n2. Use the grid (View → Show Grid) for alignment\n3. Group parts: Ctrl+G to move them together\n4. Duplicate with Ctrl+D for faster building\n5. Use snap to grid for neat buildings\n6. Try different materials for variety\n7. Add details like windows and doors last\n\nPractice makes perfect!",
            
            terrain: "🏔️ Using Terrain:\n\n1. Home tab → Editor → Edit\n2. Choose Generate for instant terrain\n3. Use Grow tool to add terrain\n4. Use Erode tool to remove terrain\n5. Paint tool changes terrain type\n6. Try grass, sand, rock, snow!\n7. Make mountains, valleys, and islands\n\nTerrain makes natural-looking environments!",
            
            detail: "✨ Adding cool details:\n\n1. Use different sized parts for variety\n2. Add Decals for pictures on walls\n3. Use PointLights to make things glow\n4. Add Smoke or Fire effects\n5. Try different materials (wood, metal, etc)\n6. Use thin parts as decorations\n7. Add plants from toolbox!\n\nDetails make your game special!",
            
            organizing: "📁 Organizing your game:\n\n1. Use Folders in workspace\n2. Name everything clearly\n3. Group related parts together\n4. Put scripts in ServerScriptService\n5. Keep assets in ReplicatedStorage\n6. Use Teams for team games\n7. Comment your scripts!\n\nGood organization helps you find things!",
            
            models: "🎁 Using the Toolbox:\n\n1. Click View → Toolbox\n2. Search for what you need\n3. Click to insert into game\n4. Check scripts before using!\n5. Customize models with your colors\n6. Some models may have viruses - be careful!\n7. You can make your own models too!\n\nToolbox has tons of free stuff!",
            
            advanced: "🚀 Advanced building:\n\n1. Use CSGUnion to combine parts\n2. Use CSGSubtract to cut holes\n3. Try using CFrames for rotation\n4. Use TweenService for smooth movement\n5. Create custom shapes with unions\n6. Use constraints for physics\n7. Try path generation for AI\n\nThese are pro techniques!"
        };

        const quickSuggestions = [
            "Game ideas",
            "How to start",
            "Scripting help",
            "Building tips",
            "Make it fun",
            "Random idea"
        ];

        // Initialize
        addMessage('bot', '👋 Hi! I\'m your Roblox Studio helper! I can help you with:\n\n🎮 Game ideas for any genre\n🔧 How to use Roblox Studio\n📜 Scripting and coding help\n🏗️ Building techniques\n💡 Tips and tricks\n\nWhat would you like to know?');
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

        function containsAny(text, keywords) {
            return keywords.some(keyword => text.includes(keyword));
        }

        function generateResponse(message) {
            const lowerMessage = message.toLowerCase();

            // Greetings
            if (containsAny(lowerMessage, ['hello', 'hi', 'hey', 'sup', 'yo'])) {
                return "👋 Hey there! I'm super excited to help you with Roblox Studio! What would you like to know about? I can help with game ideas, building, scripting, or just general tips!";
            }

            // Thanks
            if (containsAny(lowerMessage, ['thank', 'thanks', 'thx', 'ty'])) {
                return "😊 You're welcome! I'm always here to help! Have fun building your awesome game! Let me know if you need anything else!";
            }

            // Goodbye
            if (containsAny(lowerMessage, ['bye', 'goodbye', 'see you', 'cya', 'later'])) {
                return "👋 Goodbye! Have an amazing time creating your game! Come back anytime you need help. Happy building! 🎮";
            }

            // How are you
            if (containsAny(lowerMessage, ['how are you', 'whats up', 'what\'s up'])) {
                return "I'm doing great! 😊 I'm excited to help you make awesome Roblox games! What are you working on today?";
            }

            // HOW TO questions
            if (containsAny(lowerMessage, ['how to start', 'how do i start', 'getting started', 'begin', 'first time'])) {
                return howToGuides.start;
            }
            if (containsAny(lowerMessage, ['how to add part', 'how to make part', 'how to place', 'create part'])) {
                return howToGuides.parts;
            }
            if (containsAny(lowerMessage, ['how to script', 'how to code', 'add script', 'make script', 'scripting'])) {
                return howToGuides.script;
            }
            if (containsAny(lowerMessage, ['how to color', 'change color', 'how to paint', 'make it colorful'])) {
                return howToGuides.colors;
            }
            if (containsAny(lowerMessage, ['how to test', 'how to play', 'test game', 'try my game'])) {
                return howToGuides.testing;
            }
            if (containsAny(lowerMessage, ['how to publish', 'upload game', 'share game', 'make it public'])) {
                return howToGuides.publish;
            }
            if (containsAny(lowerMessage, ['how to move', 'make it move', 'movement', 'animate'])) {
                return howToGuides.movement;
            }
            if (containsAny(lowerMessage, ['how to teleport', 'make teleporter', 'teleportation'])) {
                return howToGuides.teleport;
            }
            if (containsAny(lowerMessage, ['how to make coins', 'collectible', 'pickup', 'collect'])) {
                return howToGuides.coins;
            }
            if (containsAny(lowerMessage, ['how to add sound', 'how to add music', 'sound', 'audio', 'music'])) {
                return howToGuides.sounds;
            }
            if (containsAny(lowerMessage, ['how to make gui', 'how to make menu', 'buttons', 'interface', 'ui'])) {
                return howToGuides.gui;
            }
            if (containsAny(lowerMessage, ['how to change lighting', 'make it dark', 'make it bright', 'day night'])) {
                return howToGuides.lighting;
            }
            if (containsAny(lowerMessage, ['how to spawn', 'spawn point', 'where players start', 'respawn'])) {
                return howToGuides.spawn;
            }

            // SCRIPTING questions
            if (containsAny(lowerMessage, ['learn lua', 'lua basics', 'scripting basics', 'coding basics'])) {
                return scriptingHelp.basic;
            }
            if (containsAny(lowerMessage, ['easy script', 'simple script', 'beginner script', 'first script'])) {
                return scriptingHelp.beginner;
            }
            if (containsAny(lowerMessage, ['what are variables', 'how do variables', 'variable'])) {
                return scriptingHelp.variables;
            }
            if (containsAny(lowerMessage, ['what are functions', 'how do functions', 'function'])) {
                return scriptingHelp.functions;
            }
            if (containsAny(lowerMessage, ['what are events', 'how do events', 'event', 'touched'])) {
                return scriptingHelp.events;
            }
            if (containsAny(lowerMessage, ['what are loops', 'how do loops', 'loop', 'while', 'for loop'])) {
                return scriptingHelp.loops;
            }

            // BUILDING questions
            if (containsAny(lowerMessage, ['building tips', 'building help', 'how to build', 'build better'])) {
                return buildingTips.basic;
            }
            if (containsAny(lowerMessage, ['terrain', 'mountains', 'hills', 'landscape'])) {
                return buildingTips.terrain;
            }
            if (containsAny(lowerMessage, ['add details', 'make it look good', 'decoration', 'decorate'])) {
                return buildingTips.detail;
            }
            if (containsAny(lowerMessage, ['organize', 'organization', 'clean up', 'messy'])) {
                return buildingTips.organizing;
            }
            if (containsAny(lowerMessage, ['toolbox', 'free models', 'assets', 'download'])) {
                return buildingTips.models;
            }
            if (containsAny(lowerMessage, ['advanced', 'pro tips', 'expert'])) {
                return buildingTips.advanced;
            }

            // GAME IDEAS by type
            if (containsAny(lowerMessage, ['obby', 'obstacle'])) {
                return getRandomItem(gameIdeas.obby);
            }
            if (containsAny(lowerMessage, ['simulator', 'sim', 'clicking'])) {
                return getRandomItem(gameIdeas.simulator);
            }
            if (containsAny(lowerMessage, ['adventure', 'explore', 'quest'])) {
                return getRandomItem(gameIdeas.adventure);
            }
            if (containsAny(lowerMessage, ['race', 'racing', 'car', 'speed'])) {
                return getRandomItem(gameIdeas.racing);
            }
            if (containsAny(lowerMessage, ['tycoon', 'build', 'grow', 'business'])) {
                return getRandomItem(gameIdeas.tycoon);
            }
            if (containsAny(lowerMessage, ['roleplay', 'rp', 'life'])) {
                return getRandomItem(gameIdeas.roleplay);
            }
            if (containsAny(lowerMessage, ['tower defense', 'defense', 'defend', 'tower'])) {
                return getRandomItem(gameIdeas.tower_defense);
            }
            if (containsAny(lowerMessage, ['scary', 'horror', 'spooky', 'creepy'])) {
                return getRandomItem(gameIdeas.horror_light);
            }
            if (containsAny(lowerMessage, ['fighting', 'battle', 'combat', 'pvp', 'fight'])) {
                return getRandomItem(gameIdeas.fighting);
            }
            if (containsAny(lowerMessage, ['puzzle', 'brain', 'riddle', 'mystery'])) {
                return getRandomItem(gameIdeas.puzzle);
            }
            if (containsAny(lowerMessage, ['survival', 'survive', 'island'])) {
                return getRandomItem(gameIdeas.survival);
            }

            // General game questions
            if (containsAny(lowerMessage, ['game idea', 'game ideas', 'what game', 'what should i make', 'give me idea'])) {
                const allIdeas = [...gameIdeas.obby, ...gameIdeas.simulator, ...gameIdeas.adventure, 
                                 ...gameIdeas.racing, ...gameIdeas.tycoon, ...gameIdeas.roleplay,
                                 ...gameIdeas.tower_defense, ...gameIdeas.fighting, ...gameIdeas.puzzle];
                return getRandomItem(allIdeas);
            }

            if (containsAny(lowerMessage, ['make fun', 'make it better', 'improve', 'make it good'])) {
                return "🎮 Here's how to make your game more fun:\n\n1. Add music and sound effects\n2. Give players goals or quests\n3. Add unlockables and achievements\n4. Make it colorful and exciting\n5. Add rewards for playing\n6. Test it with friends\n7. Add Easter eggs and secrets\n8. Make controls smooth\n9. Add a tutorial for new players\n10. Keep updating with new content!\n\nThe most important thing: make something YOU would want to play!";
            }

            if (containsAny(lowerMessage, ['tip', 'tips', 'advice', 'help', 'stuck'])) {
                return getRandomItem(tips);
            }

            if (containsAny(lowerMessage, ['popular', 'trending', 'most played', 'best games'])) {
                return "🔥 Popular game types on Roblox right now:\n\n1. Simulators (pet, clicking, training)\n2. Obbys (obstacle courses)\n3. Roleplay games (school, city, house)\n4. Tycoons (building empires)\n5. Tower Defense\n6. Fighting/Battle games\n7. Racing games\n8. Horror/Mystery games\n9. Survival games\n10. Adventure quests\n\nBut remember - make what YOU think is fun! Original ideas can become super popular too!";
            }

            if (containsAny(lowerMessage, ['difficulty', 'hard', 'easy', 'level'])) {
                return "⚖️ Balancing game difficulty:\n\n🟢 Easy: For beginners\n- Clear instructions\n- Generous checkpoints\n- Simple controls\n- Lots of help\n\n🟡 Medium: Main game\n- Some challenge\n- Fair obstacles\n- Learnable patterns\n- Occasional tough spots\n\n🔴 Hard: For experts\n- Real challenges\n- Fewer checkpoints\n- Requires skill\n- Feels rewarding!\n\nTip: Start easy and get harder gradually. Test with friends!";
            }

            if (containsAny(lowerMessage, ['multiplayer', 'friends', 'players', 'co-op', 'together'])) {
                return "👥 Making multiplayer games:\n\n1. Add spawn points for all players\n2. Use Teams for team games\n3. Add player name tags\n4. Create leaderboards\n5. Make sure enough space for everyone\n6. Add chat commands\n7. Consider co-op challenges\n8. Add player vs player options\n9. Make it fun solo too!\n10. Test with friends!\n\nMultiplayer games are more fun to play with friends!";
            }

            if (containsAny(lowerMessage, ['money', 'coins', 'currency', 'shop', 'store'])) {
                return "💰 Adding currency/shop system:\n\n1. Use Leaderstats to show money\n2. Give money for completing tasks\n3. Create a shop GUI\n4. Add items to buy (power-ups, cosmetics)\n5. Use IntValues to track money\n6. Save money with DataStore\n7. Make earning money fun!\n8. Add daily rewards\n9. Don't make it too grindy\n10. Test the economy!\n\nMoney systems make players want to keep playing!";
            }

            if (containsAny(lowerMessage, ['animation', 'animate', 'move characters', 'make them move'])) {
                return "🎭 Adding animations:\n\n1. Use Roblox Animation Editor (Plugins)\n2. Create keyframes for movement\n3. Save animation and get ID\n4. Load in script: local anim = humanoid:LoadAnimation(animation)\n5. Play it: anim:Play()\n6. Adjust speed: anim:AdjustSpeed(2)\n7. Stop it: anim:Stop()\n\nAnimations make characters come alive! Try walking, jumping, and dancing animations first!";
            }

            if (containsAny(lowerMessage, ['mobile', 'phone', 'tablet', 'touch'])) {
                return "📱 Making games mobile-friendly:\n\n1. Add touch controls (on-screen buttons)\n2. Make UI bigger for easy tapping\n3. Test on mobile (Device Emulator)\n4. Use UserInputService for touch detection\n5. Don't use keyboard-only controls\n6. Keep it simple\n7. Optimize for lower performance\n8. Test button sizes!\n\nMany Roblox players use mobile!";
            }

            // Random idea
            if (containsAny(lowerMessage, ['random', 'surprise', 'anything'])) {
                const categories = Object.keys(gameIdeas);
                const randomCategory = categories[Math.floor(Math.random() * categories.length)];
                return "🎲 Here's a random idea for you!\n\n" + getRandomItem(gameIdeas[randomCategory]);
            }

            // What is Roblox Studio
            if (containsAny(lowerMessage, ['what is roblox studio', 'what is studio', 'tell me about'])) {
                return "🎮 Roblox Studio is a FREE program where you can create your own Roblox games!\n\nYou can:\n✨ Build 3D worlds\n📝 Write code (Lua programming)\n🎨 Design characters and objects\n🎵 Add sounds and music\n🎮 Make any game you imagine!\n\nIt's like having your own game development studio! Millions of people use it to make games. Download it from roblox.com!";
            }

            // What can I make
            if (containsAny(lowerMessage, ['what can i make', 'what can i create', 'possibilities'])) {
                return "🌟 You can make ANYTHING in Roblox Studio!\n\nPopular types:\n🏃 Obbys & parkour\n🎮 Simulators\n🏰 Adventures & RPGs\n🏎️ Racing games\n🏗️ Tycoons\n🎭 Roleplay\n⚔️ Fighting games\n🧩 Puzzles\n🌍 Survival\n🎪 Mini-games\n\nAnd much more! If you can imagine it, you can build it! What sounds fun to you?";
            }

            // Encouragement
            if (containsAny(lowerMessage, ['give up', 'too hard', 'can\'t do', 'impossible', 'difficult'])) {
                return "💪 Don't give up! Every expert was once a beginner!\n\n🌟 Remember:\n- Start with small, simple projects\n- Watch tutorials when stuck\n- Ask for help (that's what I'm here for!)\n- Practice makes perfect\n- Even simple games can be fun\n- Take breaks when frustrated\n- Celebrate small wins!\n\nYou've got this! What part is giving you trouble? I can help!";
            }

            // Default - general helpful response
            return "🤔 I'm not sure about that specific question, but I can help you with:\n\n💡 Game ideas (obby, simulator, racing, adventure, etc.)\n🔧 How to use Roblox Studio tools\n📜 Scripting and coding help\n🏗️ Building techniques\n💎 Making your game fun\n🎮 Testing and publishing\n\nWhat would you like to know more about?";
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
            
