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
        <h1>🎮 Roblox AI Helper</h1>
        
        <div class="chat">
            <div id="messages"></div>
            
            <input type="text" id="input" placeholder="Ask how to make anything...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        // MEGA ROBLOX STUDIO TUTORIALS - 100+ HOW-TOs!
        var answers = {
            // Basic Getting Started
            "start": "🎮 HOW TO START:\n1. Download Roblox Studio (FREE)\n2. Open and click 'New'\n3. Choose 'Baseplate'\n4. Press F5 to test!\n5. Save with Ctrl+S",
            
            "part": "🧱 HOW TO ADD PARTS:\n1. Click 'Part' button in Home tab\n2. Choose shape (Block/Sphere/Cylinder)\n3. Press M to move\n4. Press R to resize\n5. Press T to rotate",
            
            "script": "📝 HOW TO SCRIPT:\n1. Click the part\n2. Press '+' in Explorer\n3. Choose 'Script'\n4. Type code!\n5. Press F5 to test",
            
            // Kill/Damage Blocks
            "lava": "🌋 MAKE LAVA BLOCK:\n1. Insert Part\n2. Color = orange/red\n3. Material = Neon\n4. Anchor it!\n5. Add Script:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\n6. Add Fire object for flames!",

            "kill": "☠️ MAKE KILL BLOCK:\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.Health = 0\n    end\nend)\n\nTip: Make it red so players know it's dangerous!",

            "damage": "💔 MAKE DAMAGE BLOCK:\n\nlocal damage = 10\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.Health = humanoid.Health - damage\n    end\nend)",

            "poison": "☠️ MAKE POISON BLOCK:\n\nlocal damage = 5\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        for i = 1, 10 do\n            wait(1)\n            humanoid.Health = humanoid.Health - damage\n        end\n    end\nend)\n\nDamages over time!",

            "spike": "⚡ MAKE SPIKE TRAP:\n\n1. Make a spike-shaped part (or use wedges)\n2. Color it gray/silver\n3. Add Script:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\nMake it look sharp!",

            // Healing/Boost
            "heal": "💚 MAKE HEALING BLOCK:\n\nlocal healAmount = 25\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.Health = humanoid.Health + healAmount\n    end\nend)\n\nMake it green!",

            "speed": "⚡ MAKE SPEED BOOST:\n\nlocal speedBoost = 50\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.WalkSpeed = humanoid.WalkSpeed + speedBoost\n        wait(5)\n        humanoid.WalkSpeed = humanoid.WalkSpeed - speedBoost\n    end\nend)\n\n5 second speed boost!",

            "jump": "🦘 MAKE JUMP BOOST:\n\nlocal jumpBoost = 100\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.JumpPower = humanoid.JumpPower + jumpBoost\n        wait(5)\n        humanoid.JumpPower = humanoid.JumpPower - jumpBoost\n    end\nend)",

            // Interactive Objects
            "door": "🚪 MAKE DOOR:\n\n1. Create door part\n2. Add ClickDetector\n3. Add Script:\n\nlocal door = script.Parent\nlocal open = false\n\nscript.Parent.ClickDetector.MouseClick:Connect(function()\n    if open then\n        door.Transparency = 0\n        door.CanCollide = true\n        open = false\n    else\n        door.Transparency = 1\n        door.CanCollide = false\n        open = true\n    end\nend)",

            "button": "🔘 MAKE BUTTON:\n\n1. Create button part\n2. Add ClickDetector\n3. Add Script:\n\nscript.Parent.ClickDetector.MouseClick:Connect(function(player)\n    print(player.Name .. ' pressed the button!')\n    -- Add what happens here\nend)\n\nChange color when clicked!",

            "checkpoint": "🚩 MAKE CHECKPOINT:\n\n1. Insert SpawnLocation part\n2. Change color (each checkpoint different color)\n3. Set properties:\n   • Duration = 0\n   • Neutral = true\n4. Make sure it's Anchored!\n\nPlayers respawn here when they touch it!",

            "teleport": "✨ MAKE TELEPORTER:\n\n1. Create 2 parts (start and end)\n2. Name one 'EndPart'\n3. Add to start part:\n\nlocal endPart = workspace.EndPart\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent:MoveTo(endPart.Position + Vector3.new(0, 3, 0))\n    end\nend)\n\nMake them glow!",

            // Movement
            "moving platform": "🔄 MAKE MOVING PLATFORM:\n\nlocal platform = script.Parent\nlocal startPos = platform.Position\nlocal endPos = startPos + Vector3.new(20, 0, 0)\n\nwhile true do\n    platform.Position = endPos\n    wait(3)\n    platform.Position = startPos\n    wait(3)\nend\n\nUse TweenService for smooth movement!",

            "spinning": "🌀 MAKE SPINNING PART:\n\nwhile true do\n    wait(0.01)\n    script.Parent.CFrame = script.Parent.CFrame * CFrame.Angles(0, 0.1, 0)\nend\n\nChange 0.1 for speed!",

            "bouncy": "🦘 MAKE BOUNCY PART:\n\n1. Select part\n2. Properties → CustomPhysicalProperties\n3. Check the box\n4. Set Elasticity = 1.5 or higher!\n\nSuper bouncy!",

            "disappearing": "👻 MAKE DISAPPEARING PLATFORM:\n\nlocal platform = script.Parent\n\nplatform.Touched:Connect(function()\n    wait(0.5)\n    platform.Transparency = 1\n    platform.CanCollide = false\n    wait(3)\n    platform.Transparency = 0\n    platform.CanCollide = true\nend)",

            // Collectibles
            "coin": "💰 MAKE COLLECTIBLE COIN:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1\n        script.Parent:Destroy()\n    end\nend)\n\nMake it yellow and spinning!",

            "gem": "💎 MAKE GEM:\n\nlocal gemValue = 10\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + gemValue\n        script.Parent:Destroy()\n    end\nend)\n\nWorth more than coins!",

            "key": "🔑 MAKE KEY:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        local hasKey = Instance.new('BoolValue')\n        hasKey.Name = 'HasKey'\n        hasKey.Parent = player\n        script.Parent:Destroy()\n    end\nend)\n\nCheck if player has key to unlock doors!",

            // Effects
            "fire": "🔥 ADD FIRE:\n\n1. Select part\n2. Insert → Fire\n3. Customize:\n   • Size = flame size\n   • Heat = height\n   • Color = flame color\n\nGreat for torches, lava!",

            "smoke": "💨 ADD SMOKE:\n\n1. Select part\n2. Insert → Smoke\n3. Customize:\n   • Size = smoke size\n   • Opacity = thickness\n   • RiseVelocity = speed\n\nGreat for chimneys!",

            "sparkles": "✨ ADD SPARKLES:\n\n1. Select part\n2. Insert → Sparkles\n3. Change SparkleColor in Properties\n\nMagical effects!",

            "light": "💡 ADD LIGHT:\n\n1. Select part\n2. Insert → PointLight\n3. Customize:\n   • Brightness = intensity\n   • Range = how far\n   • Color = light color\n\nMake things glow!",

            "explosion": "💥 MAKE EXPLOSION:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        local explosion = Instance.new('Explosion')\n        explosion.Position = script.Parent.Position\n        explosion.Parent = workspace\n        script.Parent:Destroy()\n    end\nend)",

            // Tools & Weapons
            "sword": "⚔️ MAKE SWORD:\n\n1. Insert Tool\n2. Add Part inside named 'Handle'\n3. Shape it like sword\n4. Add Script to Tool:\n\nlocal tool = script.Parent\nlocal damage = 10\n\ntool.Activated:Connect(function()\n    -- Sword swing animation here\nend)\n\ntool.Handle.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid:TakeDamage(damage)\n    end\nend)",

            "tool": "🔧 MAKE TOOL:\n\n1. Insert Tool in StarterPack\n2. Add Part inside named 'Handle'\n3. Add Script:\n\nlocal tool = script.Parent\n\ntool.Activated:Connect(function()\n    print('Tool used!')\n    -- Add tool function here\nend)",

            "gun": "🔫 MAKE GUN:\n\n1. Create Tool with Handle\n2. Add Script:\n\nlocal tool = script.Parent\nlocal damage = 20\n\ntool.Activated:Connect(function()\n    local ray = Ray.new(tool.Handle.Position, tool.Handle.CFrame.LookVector * 100)\n    local hit, position = workspace:FindPartOnRay(ray)\n    \n    if hit and hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid:TakeDamage(damage)\n    end\nend)",

            // GUI
            "gui": "📱 MAKE GUI:\n\n1. StarterGui → Insert ScreenGui\n2. Add TextButton\n3. Change text in Properties\n4. Add LocalScript:\n\nscript.Parent.MouseButton1Click:Connect(function()\n    print('Button clicked!')\nend)",

            "shop": "🏪 MAKE SHOP GUI:\n\n1. Create ScreenGui\n2. Add Frame for shop window\n3. Add TextButtons for items\n4. LocalScript:\n\nlocal button = script.Parent\nlocal price = 100\n\nbutton.MouseButton1Click:Connect(function()\n    local player = game.Players.LocalPlayer\n    if player.leaderstats.Coins.Value >= price then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value - price\n        print('Item bought!')\n    end\nend)",

            "health bar": "❤️ MAKE HEALTH BAR:\n\nIn StarterGui → ScreenGui:\n1. Add Frame (background)\n2. Add Frame inside (health bar)\n3. LocalScript:\n\nlocal player = game.Players.LocalPlayer\nlocal character = player.Character\nlocal humanoid = character:WaitForChild('Humanoid')\nlocal healthBar = script.Parent\n\nhumanoid.HealthChanged:Connect(function()\n    healthBar.Size = UDim2.new(humanoid.Health / humanoid.MaxHealth, 0, 1, 0)\nend)",

            // Sounds & Music
            "sound": "🔊 ADD SOUND:\n\n1. Find sound ID on Roblox\n2. Insert Sound into part\n3. SoundId = rbxassetid://12345\n4. Script to play:\n\nscript.Parent.Sound:Play()\n\nTo loop: Sound.Looped = true",

            "music": "🎵 ADD BACKGROUND MUSIC:\n\n1. Insert Sound in Workspace\n2. Set SoundId to music ID\n3. Looped = true\n4. Playing = true\n5. Volume = 0.5\n\nAuto-plays when game starts!",

            "footstep": "👣 CUSTOM FOOTSTEP SOUNDS:\n\nIn StarterCharacterScripts:\n\nlocal character = script.Parent\nlocal humanoid = character:WaitForChild('Humanoid')\nlocal rootPart = character:WaitForChild('HumanoidRootPart')\n\nhumanoid.Running:Connect(function(speed)\n    if speed > 0 then\n        -- Play footstep sound\n    end\nend)",

            // Advanced
            "leaderboard": "📊 MAKE LEADERBOARD:\n\nIn ServerScriptService:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local stats = Instance.new('Folder')\n    stats.Name = 'leaderstats'\n    stats.Parent = player\n    \n    local coins = Instance.new('IntValue')\n    coins.Name = 'Coins'\n    coins.Value = 0\n    coins.Parent = stats\nend)",

            "team": "👥 MAKE TEAMS:\n\n1. Insert Teams service\n2. Add Team objects\n3. Set TeamColor\n4. Script to assign:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    player.Team = game.Teams.RedTeam\nend)",

            "datastore": "💾 SAVE PLAYER DATA:\n\nlocal DataStore = game:GetService('DataStoreService'):GetDataStore('MyData')\n\ngame.Players.PlayerRemoving:Connect(function(player)\n    DataStore:SetAsync(player.UserId, player.leaderstats.Coins.Value)\nend)\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local data = DataStore:GetAsync(player.UserId)\n    if data then\n        player.leaderstats.Coins.Value = data\n    end\nend)",

            "morph": "🎭 MAKE MORPH:\n\n1. Create model of character\n2. Add Humanoid to model\n3. Create TouchPart\n4. Script:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        local morph = game.ReplicatedStorage.Morph:Clone()\n        player.Character = morph\n        morph.Parent = workspace\n    end\nend)",

            // Building
            "build": "🏗️ BUILDING TIPS:\n\n1. Start with large parts\n2. Use grid snap\n3. Group parts (Ctrl+G)\n4. Copy/paste (Ctrl+C, Ctrl+V)\n5. Use different materials\n6. Add lighting\n7. Test often (F5)!",

            "anchor": "⚓ ANCHOR PARTS:\n\n1. Select part\n2. Properties → Anchored\n3. Check the box\n\nAnchored = doesn't fall!",

            // Science
            "sky blue": "🌤️ Sky is blue because of Rayleigh scattering!",
            "gravity": "🌍 Gravity pulls objects together!",
            "planets": "🪐 8 planets: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune!"
        };

        var games = [
            "🏃 SPEED OBBY: Racing with moving platforms!",
            "🍕 PIZZA SIMULATOR: Run pizza shop!",
            "🐾 PET SIMULATOR: Collect 100+ pets!",
            "⚔️ SWORD FIGHTER: Battle with swords!",
            "🏎️ KART RACING: Race on 20 tracks!"
        ];

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲"
        ];

        addBot("👋 I can teach you how to make ANYTHING in Roblox Studio!\n\nJust ask:\n• How to make lava\n• How to make a door\n• How to make coins\n• How to make teleporter\n• How to make moving platform\n• How to make speed boost\n• How to make GUI\n• How to make sword\n• And much more!\n\nWhat do you want to make?");

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

            // Math
            if (lower.match(/\d+\s*[\+\-\*\/]\s*\d+/)) {
                try {
                    var result = eval(lower.replace(/[^0-9+\-*/().]/g, ''));
                    response = "🧮 " + lower + " = " + result;
                } catch(e) {}
            }

            // Greetings
            if (!response && (lower.includes('hello') || lower.includes('hi') || lower.includes('hey'))) {
                response = "👋 Hey! Tell me what you want to make in Roblox Studio!";
            }

            if (!response && lower.includes('thank')) {
                response = "😊 You're welcome! What else do you want to make?";
            }

            // Jokes
            if (!response && lower.includes('joke')) {
                response = random(jokes);
            }

            // Game ideas
            if (!response && (lower.includes('game idea') || lower.includes('obby') || lower.includes('simulator'))) {
                response = random(games);
            }

            // Check answers database for ALL "how to make" questions
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

            // Default
            if (!response) {
                response = "I can teach you how to make:\n\n🌋 Lava/kill blocks\n💚 Healing blocks\n⚡ Speed/jump boosts\n🚪 Doors & buttons\n✨ Teleporters\n🔄 Moving platforms\n💰 Coins & gems\n🔥 Fire & effects\n⚔️ Swords & tools\n📱 GUIs & shops\n🔊 Sounds & music\n📊 Leaderboards\n\nWhat do you want to make?";
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
