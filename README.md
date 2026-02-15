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
            
            <input type="text" id="input" placeholder="Ask me anything...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        // ROBLOX STUDIO TUTORIALS - HOW TO DO THINGS!
        var studioTutorials = {
            "start": "🎮 HOW TO START WITH ROBLOX STUDIO:\n\n1. Download Roblox Studio (FREE at roblox.com)\n2. Open it and click 'New'\n3. Choose a template or 'Baseplate'\n4. Use Explorer window to see your game parts\n5. Press F5 to test your game!\n6. Save often with Ctrl+S\n\nStart simple - make a platform to stand on first!",
            
            "add part": "🧱 HOW TO ADD PARTS:\n\n1. Click 'Part' button in Home tab (or Model tab)\n2. Choose shape: Block, Sphere, Cylinder, Wedge\n3. Use Move tool (M key) to position it\n4. Use Scale tool (R key) to resize it\n5. Use Rotate tool (T key) to turn it\n6. Change color in Properties window!\n\nTIP: Copy with Ctrl+C, paste with Ctrl+V!",
            
            "script": "📝 HOW TO ADD SCRIPTS:\n\n1. Click the part you want to script\n2. Press '+' button in Explorer next to the part\n3. Choose 'Script' (for server) or 'LocalScript' (for player)\n4. Type your Lua code inside!\n5. Press F5 to test!\n\nExample script:\nprint('Hello World!')\nwait(2)\nscript.Parent.BrickColor = BrickColor.Random()\n\nStart simple and build up!",
            
            "color": "🎨 HOW TO CHANGE COLORS:\n\n1. Select the part you want to color\n2. Look at Properties window (right side)\n3. Find 'BrickColor' or 'Color'\n4. Click it to open color picker\n5. Choose your favorite color!\n\nTIP: You can also change 'Material' to make things look like wood, metal, grass, etc!",
            
            "move": "🏃 HOW TO MAKE THINGS MOVE:\n\nSimple movement script:\n\nwhile true do\n    wait(0.1)\n    script.Parent.Position = script.Parent.Position + Vector3.new(0.1, 0, 0)\nend\n\nSpinning script:\n\nwhile true do\n    wait(0.01)\n    script.Parent.CFrame = script.Parent.CFrame * CFrame.Angles(0, 0.1, 0)\nend\n\nUse TweenService for smooth movement!",
            
            "teleport": "✨ HOW TO MAKE A TELEPORTER:\n\n1. Create 2 parts (start and end teleporter)\n2. Add Script to start part\n3. Use this code:\n\nlocal endPart = game.Workspace.EndPart\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        hit.Parent:MoveTo(endPart.Position)\n    end\nend)\n\nMake them glow with PointLight for cool effect!",
            
            "gui": "📱 HOW TO MAKE GUI (MENUS/BUTTONS):\n\n1. Click StarterGui in Explorer\n2. Insert a ScreenGui\n3. Add TextLabel for text OR TextButton for buttons\n4. Use Properties to change size, color, text\n5. Add LocalScript to make buttons work!\n\nButton click script:\nlocal button = script.Parent\nbutton.MouseButton1Click:Connect(function()\n    print('Button clicked!')\nend)\n\nGUIs are great for health bars, shops, menus!",
            
            "sound": "🔊 HOW TO ADD SOUNDS:\n\n1. Find a sound on Roblox website\n2. Copy the Sound ID number\n3. In Studio, insert a Sound object into part\n4. Paste ID in SoundId property: rbxassetid://12345\n5. Use script to play:\n\nlocal sound = script.Parent.Sound\nsound:Play()\n\nTo loop: sound.Looped = true\nTo stop: sound:Stop()\n\nSounds make games WAY more fun!",
            
            "spawn": "🎯 HOW TO MAKE SPAWN POINT:\n\n1. Go to Model tab\n2. Click 'Spawn' (or insert SpawnLocation)\n3. Move it where you want players to start\n4. Change its color in Properties\n5. Set Duration to 0 for instant respawn\n6. Make sure it's Anchored!\n\nYou can have multiple spawns for different teams!",
            
            "lighting": "💡 HOW TO CHANGE LIGHTING:\n\n1. Find 'Lighting' in Explorer\n2. Change these properties:\n   • TimeOfDay: '14:00:00' for day, '00:00:00' for night\n   • Brightness: Higher = brighter (default 2)\n   • Ambient: Background light color\n   • OutdoorAmbient: Outdoor light color\n3. For fog: Change FogEnd and FogColor\n4. Try ClockTime for cool effects!\n\nGood lighting makes your game look AMAZING!",
            
            "team": "👥 HOW TO CREATE TEAMS:\n\n1. Insert 'Teams' service in Explorer\n2. Add Team objects inside it\n3. Set TeamColor for each team\n4. Name your teams (Red Team, Blue Team, etc)\n5. SpawnLocations can have TeamColor too!\n\nScript to assign player to team:\ngame.Players.PlayerAdded:Connect(function(player)\n    player.Team = game.Teams.RedTeam\nend)\n\nGreat for team games!",
            
            "leaderboard": "📊 HOW TO MAKE LEADERBOARD:\n\nAdd this script to ServerScriptService:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local leaderstats = Instance.new('Folder')\n    leaderstats.Name = 'leaderstats'\n    leaderstats.Parent = player\n    \n    local coins = Instance.new('IntValue')\n    coins.Name = 'Coins'\n    coins.Value = 0\n    coins.Parent = leaderstats\nend)\n\nThis shows on right side of screen!\nAdd more IntValues for different stats!",
            
            "collect": "💰 HOW TO MAKE COLLECTIBLE COINS:\n\n1. Create a coin part (cylinder works great!)\n2. Make it yellow/gold colored\n3. Add this Script to the coin:\n\nlocal coin = script.Parent\n\ncoin.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1\n        coin:Destroy()\n    end\nend)\n\nMake it spin for extra coolness!",
            
            "tool": "🔧 HOW TO CREATE TOOLS:\n\n1. Insert a 'Tool' object\n2. Add a Part inside the Tool (this is the handle)\n3. Name the part 'Handle'\n4. Add a Script to the Tool\n5. Put Tool in StarterPack\n\nBasic tool script:\nlocal tool = script.Parent\n\ntool.Activated:Connect(function()\n    print('Tool used!')\nend)\n\nTools can be swords, flashlights, anything!",
            
            "animation": "🎭 HOW TO ADD ANIMATIONS:\n\n1. Use Animation Editor plugin\n2. Create keyframes for movement\n3. Save animation and get Animation ID\n4. Load in script:\n\nlocal humanoid = character:WaitForChild('Humanoid')\nlocal animator = humanoid:WaitForChild('Animator')\nlocal animation = Instance.new('Animation')\nanimation.AnimationId = 'rbxassetid://12345'\nlocal animTrack = animator:LoadAnimation(animation)\nanimTrack:Play()\n\nAnimations make characters come alive!",
            
            "camera": "📷 HOW TO CONTROL CAMERA:\n\nIn LocalScript:\n\nlocal camera = workspace.CurrentCamera\nlocal player = game.Players.LocalPlayer\n\n-- First person\ncamera.CameraType = Enum.CameraType.Scriptable\ncamera.CFrame = player.Character.Head.CFrame\n\n-- Lock to part\ncamera.CameraSubject = workspace.Part\n\n-- Zoom limits\nplayer.CameraMaxZoomDistance = 50\nplayer.CameraMinZoomDistance = 10\n\nCamera control = cinematic games!",
            
            "datastore": "💾 HOW TO SAVE PLAYER DATA:\n\nIn ServerScriptService:\n\nlocal DataStoreService = game:GetService('DataStoreService')\nlocal myDataStore = DataStoreService:GetDataStore('MyData')\n\n-- Save data\ngame.Players.PlayerRemoving:Connect(function(player)\n    local success, err = pcall(function()\n        myDataStore:SetAsync(player.UserId, player.leaderstats.Coins.Value)\n    end)\nend)\n\n-- Load data\ngame.Players.PlayerAdded:Connect(function(player)\n    local data = myDataStore:GetAsync(player.UserId)\n    if data then\n        player.leaderstats.Coins.Value = data\n    end\nend)\n\nSaves coins, levels, items!",
            
            "publish": "🌟 HOW TO PUBLISH YOUR GAME:\n\n1. Make sure your game works (test with F5!)\n2. Click File → Publish to Roblox\n3. Give your game a cool name\n4. Write a description (tell people what it is!)\n5. Choose if it's public or private\n6. Click 'Create' or 'Update'\n7. Set game icon and thumbnails in game settings\n8. Share the link with friends!\n\nTest thoroughly before publishing!",
            
            "test": "🎮 HOW TO TEST YOUR GAME:\n\n• Press F5 to play in Studio\n• Press F8 to stop playing\n• Use WASD to move\n• Space to jump\n• Mouse to look around\n\nAdvanced testing:\n• F6 = Test with multiple players\n• F7 = Test as server only\n• View → Output to see errors/prints\n\nALWAYS test before showing friends!",
            
            "anchor": "⚓ HOW TO ANCHOR PARTS:\n\n1. Select the part\n2. Look at Properties window\n3. Find 'Anchored' checkbox\n4. Check it to anchor (part won't fall!)\n5. Uncheck to make it fall with gravity\n\nWHEN TO ANCHOR:\n• Buildings, floors, walls\n• Platforms that shouldn't move\n• Decorations\n\nWHEN NOT TO ANCHOR:\n• Moving platforms (use scripts instead)\n• Objects players should push\n• Falling objects",
            
            "group": "📁 HOW TO GROUP PARTS:\n\n1. Select multiple parts (hold Ctrl and click)\n2. Right-click and choose 'Group'\n3. OR press Ctrl+G\n4. This creates a Model\n5. Name the model in Properties\n6. Now you can move all parts together!\n\nUNGROUP: Ctrl+U\n\nGrouping = easier building!",
            
            "transparent": "👻 HOW TO MAKE PARTS TRANSPARENT:\n\n1. Select the part\n2. Find 'Transparency' in Properties\n3. Change value:\n   • 0 = fully visible\n   • 0.5 = half transparent\n   • 1 = invisible (but still there!)\n4. Use CanCollide = false to make players walk through\n\nGreat for:\n• Invisible walls/barriers\n• Glass windows\n• Ghost effects\n• Secret passages!",
            
            "resize": "📏 HOW TO RESIZE PARTS:\n\nMethod 1 - Scale Tool:\n1. Press R key (or click Scale in Home tab)\n2. Drag the handles to resize\n3. Hold Shift for uniform scaling\n\nMethod 2 - Properties:\n1. Select part\n2. Find 'Size' in Properties\n3. Change X, Y, Z values\n   • X = width\n   • Y = height  \n   • Z = depth\n\nTIP: Keep grid snap on for neat sizes!"
        };

        // Keep all game ideas from before
        var games = {
            obby: [
                "🏃 SPEED RUNNER OBBY: Ultimate racing challenge! Moving platforms that accelerate, boost pads, slow zones, checkpoints every 10, difficulties Easy to INSANE, leaderboard for fastest times, secret shortcuts, rainbow trail!",
                "🌈 RAINBOW COLOR OBBY: Each level different color! RED=fire/lava, BLUE=ice/slippery, GREEN=nature/vines, YELLOW=lightning, PURPLE=gravity flip, ORANGE=bouncy!",
                "🚀 SPACE OBBY: Journey from Earth to Black Hole! Zero gravity sections, asteroid jumping, rocket boosters, alien NPCs, collect stars, planet-themed levels!",
                "🏰 MEDIEVAL CASTLE OBBY: Escape dungeon to rooftop! Swinging axes, arrow traps, knight patrols, dragon race at top, medieval shop!",
                "🌊 UNDERWATER OBBY: 50 levels surface to ocean floor! Swimming mechanics, oxygen bubbles, avoid sharks/jellyfish, coral reefs, submarine checkpoints!"
            ],
            simulator: [
                "🐾 MEGA PET SIMULATOR: 100+ pets! 3-stage evolution, custom homes, mini-games, breeding, pet abilities, daily care, trading!",
                "🍕 PIZZA EMPIRE: Restaurant empire! 50+ toppings, hire staff, upgrade ovens, multiple locations, delivery mini-game, competitions!",
                "⚔️ SWORD MASTER: 200+ legendary swords! Train stats, battle dummies, quest system, dungeon raids, forge swords, PvP arena!"
            ]
        };

        var knowledge = {
            "sky blue": "🌤️ The sky is blue because of Rayleigh scattering! Sunlight hits air molecules and blue light scatters more.",
            "planes fly": "✈️ Planes fly because of lift! Wings create lower pressure on top, lifting the plane!",
            "gravity": "🌍 Gravity pulls objects together! Earth's gravity keeps us on the ground.",
            "fastest animal": "🐆 Land: Cheetah 70mph! Air: Peregrine falcon 240+mph!",
            "planets": "🪐 8 planets: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune!"
        };

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲"
        ];

        addBot("👋 Hey! I'm your Roblox AI Helper!\n\n🎮 I can help with:\n• Game ideas (120+ ideas!)\n• How to use Roblox Studio\n• Scripting tutorials\n• Building tips\n• Science questions\n• Jokes and fun facts\n\nWhat would you like to know?");

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
                response = "👋 Hey! Want game ideas or help with Roblox Studio?";
            }

            if (!response && lower.includes('thank')) {
                response = "😊 You're welcome! Need anything else?";
            }

            // Jokes
            if (!response && lower.includes('joke')) {
                response = random(jokes);
            }

            // ROBLOX STUDIO TUTORIALS - Priority!
            if (!response) {
                for (var key in studioTutorials) {
                    if (lower.includes(key) || 
                        (key === 'start' && (lower.includes('how to start') || lower.includes('getting started') || lower.includes('begin'))) ||
                        (key === 'add part' && (lower.includes('add part') || lower.includes('create part') || lower.includes('make part'))) ||
                        (key === 'script' && (lower.includes('how to script') || lower.includes('add script') || lower.includes('code'))) ||
                        (key === 'color' && (lower.includes('change color') || lower.includes('paint'))) ||
                        (key === 'move' && (lower.includes('make move') || lower.includes('movement'))) ||
                        (key === 'teleport' && lower.includes('teleport')) ||
                        (key === 'gui' && (lower.includes('make gui') || lower.includes('menu') || lower.includes('button'))) ||
                        (key === 'sound' && (lower.includes('add sound') || lower.includes('music') || lower.includes('audio'))) ||
                        (key === 'spawn' && (lower.includes('spawn point') || lower.includes('respawn'))) ||
                        (key === 'lighting' && lower.includes('lighting')) ||
                        (key === 'team' && lower.includes('team')) ||
                        (key === 'leaderboard' && (lower.includes('leaderboard') || lower.includes('leaderstats'))) ||
                        (key === 'collect' && (lower.includes('collectible') || lower.includes('coin'))) ||
                        (key === 'tool' && lower.includes('tool')) ||
                        (key === 'animation' && lower.includes('animation')) ||
                        (key === 'camera' && lower.includes('camera')) ||
                        (key === 'datastore' && (lower.includes('save') || lower.includes('data'))) ||
                        (key === 'publish' && (lower.includes('publish') || lower.includes('upload'))) ||
                        (key === 'test' && (lower.includes('test') || lower.includes('play'))) ||
                        (key === 'anchor' && lower.includes('anchor')) ||
                        (key === 'group' && lower.includes('group')) ||
                        (key === 'transparent' && (lower.includes('transparent') || lower.includes('invisible'))) ||
                        (key === 'resize' && (lower.includes('resize') || lower.includes('size')))
                    ) {
                        response = studioTutorials[key];
                        break;
                    }
                }
            }

            // Game ideas
            if (!response && lower.includes('obby')) {
                response = random(games.obby);
            }
            if (!response && lower.includes('simulator')) {
                response = random(games.simulator);
            }
            if (!response && (lower.includes('game') || lower.includes('idea'))) {
                var allGames = [].concat(games.obby, games.simulator);
                response = random(allGames);
            }

            // Knowledge
            if (!response) {
                for (var key in knowledge) {
                    if (lower.includes(key)) {
                        response = knowledge[key];
                        break;
                    }
                }
            }

            // Default
            if (!response) {
                response = "🎮 I can help with:\n\n• Game ideas (ask for 'obby idea')\n• How to start Roblox Studio\n• How to add parts/scripts\n• How to change colors\n• How to make things move\n• How to make teleporters\n• How to make GUI/menus\n• How to add sounds\n• How to make teams/leaderboards\n• Science questions\n\nWhat would you like to know?";
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
            
