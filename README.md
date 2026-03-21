<html>
<head>
    <title>AI Chatbot</title>
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
            white-space: pre-wrap;
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
        <h1>AI Chatbot</h1>
        
        <div class="chat">
            <div id="messages"></div>
            
            <input type="text" id="input" placeholder="Ask me anything...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        // ROBLOX-FOCUSED DATABASE
        var answers = {
            // Roblox Studio - PRIORITY
            "roblox": "I'm here to help with Roblox! I can teach you how to build games, script in Lua, create GUIs, make obbys, add sounds, create tools, and much more. What would you like to learn about Roblox Studio?",
            
            "start": "To get started with Roblox Studio:\n\n1. Download Roblox Studio for free from the Roblox website\n2. Open it and click 'New'\n3. Choose 'Baseplate' for a blank canvas\n4. Press F5 to test your game anytime\n5. Save regularly with Ctrl+S\n\nWhat would you like to build first?",
            
            "part": "To add parts in Roblox Studio:\n\n1. Click the 'Part' button in the Home tab\n2. Choose your shape: Block, Sphere, Cylinder, or Wedge\n3. Use the Move tool (M key) to position it\n4. Use the Scale tool (R key) to resize it\n5. Use the Rotate tool (T key) to turn it\n\nYou can copy parts with Ctrl+C and paste with Ctrl+V. Would you like to know more about building?",
            
            "script": "Here's how to add scripts in Roblox Studio:\n\n1. Select the part you want to add a script to\n2. Click the '+' button in the Explorer window next to the part\n3. Choose 'Script' for server-side code or 'LocalScript' for client-side\n4. The script editor will open where you can write Lua code\n5. Press F5 to test your script\n\nWould you like an example script to get started?",
            
            "lava": "Here's how to create a lava block that kills players:\n\n1. Insert a Part into your workspace\n2. Set the color to orange or red (BrickColor in Properties)\n3. Set Material to 'Neon' for a glowing effect\n4. Make sure it's Anchored (check the box in Properties)\n5. Add a Script to the part with this code:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\nOptionally, add a Fire object inside the part for visual flames! Would you like to know how to make other types of blocks?",

            "kill": "To create a kill block:\n\nAdd this script to any part:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\nThis detects when a player touches the part and sets their health to 0. Make the block red so players know it's dangerous!",

            "damage": "Here's how to make a damage block:\n\nlocal damage = 10  -- Change this number for more/less damage\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = hit.Parent.Humanoid.Health - damage\n    end\nend)\n\nThis removes 10 health each time the player touches it. You can adjust the damage variable to any number you want.",

            "heal": "To create a healing block:\n\nlocal healAmount = 25\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = hit.Parent.Humanoid.Health + healAmount\n    end\nend)\n\nMake the block green so players know it heals them. You can change healAmount to heal more or less health.",

            "speed": "Here's how to make a speed boost pad:\n\nlocal speedBoost = 50\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.WalkSpeed = hit.Parent.Humanoid.WalkSpeed + speedBoost\n        wait(5)\n        hit.Parent.Humanoid.WalkSpeed = hit.Parent.Humanoid.WalkSpeed - speedBoost\n    end\nend)\n\nThis gives players a speed boost for 5 seconds. The default WalkSpeed is 16, so this makes them run at 66 speed!",

            "jump": "To create a jump boost pad:\n\nlocal jumpBoost = 100\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.JumpPower = hit.Parent.Humanoid.JumpPower + jumpBoost\n        wait(5)\n        hit.Parent.Humanoid.JumpPower = hit.Parent.Humanoid.JumpPower - jumpBoost\n    end\nend)\n\nDefault JumpPower is 50, so this makes them jump much higher for 5 seconds!",

            "door": "Here's how to make a clickable door:\n\n1. Create your door part\n2. Add a ClickDetector to it\n3. Add this script:\n\nlocal door = script.Parent\nlocal open = false\n\nscript.Parent.ClickDetector.MouseClick:Connect(function()\n    if open then\n        door.Transparency = 0\n        door.CanCollide = true\n        open = false\n    else\n        door.Transparency = 1\n        door.CanCollide = false\n        open = true\n    end\nend)\n\nClicking the door toggles it between open (invisible) and closed (visible).",

            "button": "To create a button:\n\n1. Make a button-shaped part\n2. Add a ClickDetector to it\n3. Add this script:\n\nscript.Parent.ClickDetector.MouseClick:Connect(function(player)\n    print(player.Name .. ' pressed the button!')\n    -- Add what you want to happen here\nend)\n\nYou can make the button change color, activate a door, or trigger any event you want!",

            "teleport": "Here's how to make a teleporter:\n\n1. Create two parts - a teleporter entrance and exit\n2. Name the exit part 'EndPart' in the Explorer\n3. Add this script to the entrance part:\n\nlocal endPart = workspace.EndPart\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent:MoveTo(endPart.Position + Vector3.new(0, 3, 0))\n    end\nend)\n\nMake both parts glow with a PointLight for a cool effect!",

            "checkpoint": "To make checkpoints:\n\n1. Go to the Model tab and click 'Spawn' (or insert a SpawnLocation)\n2. Move it to where you want players to respawn\n3. Change its color in Properties (different color for each checkpoint)\n4. Set Duration to 0 for instant respawn\n5. Make sure it's Anchored\n\nPlayers will respawn at the last checkpoint they touched!",

            "moving platform": "Here's how to make a moving platform:\n\nlocal platform = script.Parent\nlocal startPos = platform.Position\nlocal endPos = startPos + Vector3.new(20, 0, 0)  -- Moves 20 studs in X direction\n\nwhile true do\n    platform.Position = endPos\n    wait(3)\n    platform.Position = startPos\n    wait(3)\nend\n\nFor smooth movement, use TweenService instead of direct position changes!",

            "spinning": "To make a part spin continuously:\n\nwhile true do\n    wait(0.01)\n    script.Parent.CFrame = script.Parent.CFrame * CFrame.Angles(0, 0.1, 0)\nend\n\nChange the 0.1 value to adjust rotation speed. Higher = faster spinning.",

            "disappearing": "Here's how to make a disappearing platform:\n\nscript.Parent.Touched:Connect(function()\n    wait(0.5)  -- Wait half second before disappearing\n    script.Parent.Transparency = 1\n    script.Parent.CanCollide = false\n    wait(3)  -- Wait 3 seconds\n    script.Parent.Transparency = 0\n    script.Parent.CanCollide = true\nend)\n\nThe platform disappears when touched and reappears after 3 seconds.",

            "coin": "To create collectible coins:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1\n        script.Parent:Destroy()\n    end\nend)\n\nMake the coin yellow and add a spinning script for a classic coin effect!",

            "gem": "For collectible gems worth more points:\n\nlocal gemValue = 10\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + gemValue\n        script.Parent:Destroy()\n    end\nend)\n\nMake gems different colors and values for variety!",

            "fire": "To add fire effects:\n\n1. Select your part\n2. Click the '+' in Explorer next to the part\n3. Choose 'Fire'\n4. Customize in Properties:\n   - Size: how big the flames are\n   - Heat: how high flames rise\n   - Color: flame color\n\nGreat for lava, torches, campfires, or adding danger to obstacles!",

            "light": "To add lighting effects:\n\n1. Select your part\n2. Insert a PointLight\n3. Customize in Properties:\n   - Brightness: light intensity\n   - Range: how far the light reaches\n   - Color: light color\n\nUse lights to make parts glow, create atmosphere, or highlight important areas!",

            "explosion": "Here's how to create an explosion:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        local explosion = Instance.new('Explosion')\n        explosion.Position = script.Parent.Position\n        explosion.Parent = workspace\n        script.Parent:Destroy()\n    end\nend)\n\nThe explosion affects nearby players and parts!",

            "gui": "To create a GUI (screen interface):\n\n1. In StarterGui, insert a ScreenGui\n2. Add a TextButton or TextLabel inside it\n3. Customize text, size, and color in Properties\n4. For clickable buttons, add a LocalScript:\n\nscript.Parent.MouseButton1Click:Connect(function()\n    print('Button clicked!')\n    -- Add your code here\nend)\n\nGUIs are great for health bars, shops, menus, and instructions!",

            "shop": "To create a shop GUI:\n\nlocal button = script.Parent\nlocal price = 100\n\nbutton.MouseButton1Click:Connect(function()\n    local player = game.Players.LocalPlayer\n    if player.leaderstats.Coins.Value >= price then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value - price\n        print('Item purchased!')\n        -- Give the player their item here\n    else\n        print('Not enough coins!')\n    end\nend)\n\nThis checks if the player has enough coins before purchasing.",

            "leaderboard": "To create a leaderboard:\n\nIn ServerScriptService, add this script:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local leaderstats = Instance.new('Folder')\n    leaderstats.Name = 'leaderstats'\n    leaderstats.Parent = player\n    \n    local coins = Instance.new('IntValue')\n    coins.Name = 'Coins'\n    coins.Value = 0\n    coins.Parent = leaderstats\nend)\n\nThis creates a leaderboard that shows on the right side of the screen!",

            "team": "To create teams:\n\n1. Insert the Teams service in Explorer\n2. Add Team objects inside Teams\n3. Set TeamColor for each team\n4. Name your teams (Red Team, Blue Team, etc.)\n5. Use this script to assign players:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    player.Team = game.Teams.RedTeam\nend)\n\nTeams are great for competitive games!",

            "sound": "To add sounds:\n\n1. Find a sound on the Roblox website and copy its ID number\n2. Insert a Sound object into a part\n3. Paste the ID in SoundId property: rbxassetid://12345\n4. Use this script to play it:\n\nscript.Parent.Sound:Play()\n\nSet Looped = true for background music!",

            "sword": "To create a basic sword:\n\n1. Insert a Tool in StarterPack\n2. Add a Part inside it named 'Handle'\n3. Shape it like a sword\n4. Add this script to the Tool:\n\nlocal tool = script.Parent\nlocal damage = 10\n\ntool.Handle.Touched:Connect(function(hit)\n    if tool.Parent:IsA('Model') and hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid:TakeDamage(damage)\n    end\nend)\n\nPlayers can swing it to damage others!",

            "tool": "To create a basic tool:\n\n1. Insert a Tool in StarterPack\n2. Add a Part named 'Handle' inside it\n3. Add a Script to the Tool:\n\nlocal tool = script.Parent\n\ntool.Activated:Connect(function()\n    print('Tool activated!')\n    -- Add what the tool does here\nend)\n\nTools can be anything: swords, flashlights, building tools, etc!",

            // General knowledge - still available but less prominent
            "sky blue": "The sky appears blue due to Rayleigh scattering. When sunlight enters Earth's atmosphere, it collides with air molecules. Blue light has a shorter wavelength and scatters more than other colors, which is why we see blue throughout the sky. During sunset, light travels through more atmosphere, causing reds and oranges to dominate.",
            
            "gravity": "Gravity is a fundamental force that attracts objects with mass toward each other. Earth's gravity keeps us grounded and gives objects weight. The strength of gravity depends on mass and distance - more massive objects have stronger gravity, and gravity weakens with distance.",
            
            "planets": "Our solar system has 8 planets: Mercury (smallest, closest to sun), Venus (hottest), Earth (only planet with life), Mars (red planet), Jupiter (largest), Saturn (famous rings), Uranus (tilted sideways), and Neptune (windiest, farthest). Pluto is now classified as a dwarf planet.",
            
            "photosynthesis": "Photosynthesis is the process plants use to convert sunlight into energy. They take in carbon dioxide from air and water from soil, using sunlight to create glucose (sugar) for food and release oxygen as a byproduct. This happens in chloroplasts containing chlorophyll, the green pigment in plants.",
            
            "dinosaurs extinct": "Dinosaurs went extinct approximately 65 million years ago, most likely due to a massive asteroid impact in what is now Mexico. This caused widespread devastation: tsunamis, wildfires, and dust blocking sunlight for years. Plants died, then herbivores, then carnivores. Birds, which evolved from theropod dinosaurs, survived.",
            
            "fastest animal": "The cheetah is the fastest land animal, reaching speeds up to 70 mph. However, the peregrine falcon is the fastest animal overall, diving at speeds exceeding 240 mph. In water, the sailfish swims at about 68 mph."
        };

        var gameIdeas = [
            "Here's a Roblox game idea: SPEED OBBY - Create an obby with moving platforms that accelerate over time, boost pads, slow zones, and checkpoints every 10 obstacles. Include difficulty levels from Easy to INSANE, with a leaderboard for fastest times and secret shortcuts for skilled players!",
            
            "Game idea: MEGA PET SIMULATOR - Players collect 100+ pets ranging from common to mythical, with a 3-stage evolution system. Include mini-games like fetch, agility courses, and races. Add breeding mechanics for rare combinations and a trading system!",
            
            "Try this: PIZZA EMPIRE - Players start with a small pizza stand and build a restaurant empire. Include 50+ toppings, hiring staff (chefs, drivers, cashiers), upgrading equipment, and expanding to multiple locations. Add a delivery mini-game and pizza-making competitions!",
            
            "How about: TREASURE ISLAND QUEST - Create a massive island with 20+ locations where players find treasure map pieces, solve riddles and puzzles, and explore caves and temples. Include ancient traps, a boss Guardian, and hidden treasure rooms!",
            
            "Consider: TURBO KART RACING - Design 20 unique tracks (city, beach, volcano, space, jungle, underwater) with 50+ customizable karts. Add power-ups like rockets, shields, and speed boosts. Include championship mode, time trials, and 8-player multiplayer races!"
        ];

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything!",
            "What do you call a bear with no teeth? A gummy bear!",
            "Why did the bicycle fall over? It was two-tired!",
            "What do you call a fake noodle? An impasta!",
            "What did the ocean say to the beach? Nothing, it just waved!"
        ];

        addBot("Hello! I'm an AI assistant specializing in Roblox Studio development. I can help you learn how to build games, write scripts, create mechanics, and bring your game ideas to life.\n\nI can also answer general questions about science, math, history, and more.\n\nWhat would you like to know?");

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

            // Math calculation
            if (lower.match(/\d+\s*[\+\-\*\/]\s*\d+/)) {
                try {
                    var result = eval(lower.replace(/[^0-9+\-*/().]/g, ''));
                    response = "The answer is " + result + ".";
                } catch(e) {}
            }

            // Greetings
            if (!response && (lower.includes('hello') || lower.includes('hi') || lower.includes('hey'))) {
                response = "Hello! How can I assist you today? I specialize in Roblox Studio development but can help with other topics as well.";
            }

            if (!response && lower.includes('thank')) {
                response = "You're welcome! Feel free to ask if you need anything else.";
            }

            // Jokes
            if (!response && lower.includes('joke')) {
                response = random(jokes);
            }

            // Game ideas
            if (!response && (lower.includes('game idea') || lower.includes('obby') || lower.includes('simulator'))) {
                response = random(gameIdeas);
            }

            // Check answers database
            if (!response) {
                for (var key in answers) {
                    if (lower.includes(key) || 
                        lower.includes('make ' + key) || 
                        lower.includes('create ' + key) ||
                        lower.includes('how to ' + key)) {
                        response = answers[key];
                        break;
                    }
                }
            }

            // Default response
            if (!response) {
                response = "I can help you with:\n\n• Roblox Studio development (building, scripting, GUIs, game mechanics)\n• Game ideas and design concepts\n• Science, math, and general knowledge questions\n\nWhat would you like to learn about?";
            }

            setTimeout(function() {
                addBot(response);
            }, 500);
        }

        document.getElementById('input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                send();
            }
        });
    </script>
</body>
</html>













































































