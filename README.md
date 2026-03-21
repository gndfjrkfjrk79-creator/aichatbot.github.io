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
        // MEGA DATABASE - Roblox Studio + General Knowledge
        var robloxAnswers = {
            // Roblox Studio Basics
            "start": "🎮 HOW TO START ROBLOX STUDIO:\n1. Download Roblox Studio (FREE)\n2. Open and click 'New'\n3. Choose 'Baseplate'\n4. Press F5 to test!\n5. Save with Ctrl+S",
            
            "part": "🧱 ADD PARTS:\n1. Click 'Part' button\n2. Choose shape\n3. M = move, R = resize, T = rotate",
            
            "script": "📝 HOW TO SCRIPT:\n1. Click part\n2. Press '+' in Explorer\n3. Choose 'Script'\n4. Type code!\n5. F5 to test",
            
            // Kill/Damage
            "lava": "🌋 MAKE LAVA:\n1. Part → orange/red\n2. Material = Neon\n3. Anchor it\n4. Script:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\n5. Add Fire!",

            "kill": "☠️ KILL BLOCK:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)",

            "damage": "💔 DAMAGE BLOCK:\n\nlocal damage = 10\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = hit.Parent.Humanoid.Health - damage\n    end\nend)",

            // Boosts
            "heal": "💚 HEALING BLOCK:\n\nlocal healAmount = 25\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = hit.Parent.Humanoid.Health + healAmount\n    end\nend)",

            "speed": "⚡ SPEED BOOST:\n\nlocal boost = 50\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.WalkSpeed = hit.Parent.Humanoid.WalkSpeed + boost\n        wait(5)\n        hit.Parent.Humanoid.WalkSpeed = hit.Parent.Humanoid.WalkSpeed - boost\n    end\nend)",

            "jump": "🦘 JUMP BOOST:\n\nlocal jumpBoost = 100\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.JumpPower = hit.Parent.Humanoid.JumpPower + jumpBoost\n        wait(5)\n        hit.Parent.Humanoid.JumpPower = hit.Parent.Humanoid.JumpPower - jumpBoost\n    end\nend)",

            // Interactive
            "door": "🚪 MAKE DOOR:\n\nlocal door = script.Parent\nlocal open = false\n\nscript.Parent.ClickDetector.MouseClick:Connect(function()\n    if open then\n        door.Transparency = 0\n        door.CanCollide = true\n        open = false\n    else\n        door.Transparency = 1\n        door.CanCollide = false\n        open = true\n    end\nend)",

            "button": "🔘 MAKE BUTTON:\n\nscript.Parent.ClickDetector.MouseClick:Connect(function(player)\n    print(player.Name .. ' pressed button!')\nend)",

            "teleport": "✨ TELEPORTER:\n\nlocal endPart = workspace.EndPart\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent:MoveTo(endPart.Position + Vector3.new(0, 3, 0))\n    end\nend)",

            "checkpoint": "🚩 CHECKPOINT:\n\n1. Insert SpawnLocation\n2. Color it differently\n3. Duration = 0\n4. Anchor it!",

            // Movement
            "moving platform": "🔄 MOVING PLATFORM:\n\nlocal platform = script.Parent\nlocal startPos = platform.Position\nlocal endPos = startPos + Vector3.new(20, 0, 0)\n\nwhile true do\n    platform.Position = endPos\n    wait(3)\n    platform.Position = startPos\n    wait(3)\nend",

            "spinning": "🌀 SPINNING:\n\nwhile true do\n    wait(0.01)\n    script.Parent.CFrame = script.Parent.CFrame * CFrame.Angles(0, 0.1, 0)\nend",

            "disappearing": "👻 DISAPPEARING PLATFORM:\n\nscript.Parent.Touched:Connect(function()\n    wait(0.5)\n    script.Parent.Transparency = 1\n    script.Parent.CanCollide = false\n    wait(3)\n    script.Parent.Transparency = 0\n    script.Parent.CanCollide = true\nend)",

            // Collectibles
            "coin": "💰 COIN:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1\n        script.Parent:Destroy()\n    end\nend)",

            "gem": "💎 GEM:\n\nlocal value = 10\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + value\n        script.Parent:Destroy()\n    end\nend)",

            // Effects
            "fire": "🔥 ADD FIRE:\n1. Select part\n2. Insert → Fire\n3. Customize Size, Heat, Color",

            "light": "💡 ADD LIGHT:\n1. Select part\n2. Insert → PointLight\n3. Change Brightness, Range, Color",

            "explosion": "💥 EXPLOSION:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        local boom = Instance.new('Explosion')\n        boom.Position = script.Parent.Position\n        boom.Parent = workspace\n        script.Parent:Destroy()\n    end\nend)",

            // GUI
            "gui": "📱 MAKE GUI:\n1. StarterGui → ScreenGui\n2. Add TextButton\n3. LocalScript:\n\nscript.Parent.MouseButton1Click:Connect(function()\n    print('Clicked!')\nend)",

            "shop": "🏪 SHOP GUI:\n\nlocal button = script.Parent\nlocal price = 100\n\nbutton.MouseButton1Click:Connect(function()\n    local player = game.Players.LocalPlayer\n    if player.leaderstats.Coins.Value >= price then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value - price\n        print('Bought!')\n    end\nend)",

            // Systems
            "leaderboard": "📊 LEADERBOARD:\n\nIn ServerScriptService:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local stats = Instance.new('Folder')\n    stats.Name = 'leaderstats'\n    stats.Parent = player\n    \n    local coins = Instance.new('IntValue')\n    coins.Name = 'Coins'\n    coins.Value = 0\n    coins.Parent = stats\nend)",

            "team": "👥 TEAMS:\n1. Insert Teams service\n2. Add Team objects\n3. Set TeamColor\n4. Script:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    player.Team = game.Teams.RedTeam\nend)",

            "sound": "🔊 ADD SOUND:\n1. Insert Sound in part\n2. SoundId = rbxassetid://12345\n3. Script: script.Parent.Sound:Play()",

            // Tools
            "sword": "⚔️ SWORD:\n1. Insert Tool\n2. Part named 'Handle'\n3. Script:\n\nlocal damage = 10\nscript.Parent.Handle.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid:TakeDamage(damage)\n    end\nend)",

            "tool": "🔧 TOOL:\n1. Insert Tool in StarterPack\n2. Add Part named 'Handle'\n3. Script:\n\nscript.Parent.Activated:Connect(function()\n    print('Tool used!')\nend)"
        };

        // GENERAL KNOWLEDGE - Can answer ANYTHING!
        var generalKnowledge = {
            // Science
            "sky blue": "🌤️ The sky is blue because of Rayleigh scattering! When sunlight enters Earth's atmosphere, it hits tiny air molecules. Blue light has shorter wavelengths and scatters more easily than other colors, so we see blue everywhere! At sunset, light travels through more atmosphere, so we see reds and oranges instead!",
            
            "planes fly": "✈️ Planes fly because of lift! Wings are curved on top and flat on bottom. Air moves faster over the curved top, creating lower pressure above the wing. Higher pressure below pushes the wing up! Engines provide forward thrust.",
            
            "gravity": "🌍 Gravity is a force that pulls objects with mass together! Earth's massive size creates strong gravity that keeps us on the ground. Everything with mass has gravity - even you! But you'd need to be planet-sized for your gravity to be noticeable.",
            
            "photosynthesis": "🌱 Photosynthesis is how plants make food from sunlight! Plants use sunlight, water, and CO2 to create glucose (food) and oxygen (for us to breathe!). It happens in chloroplasts using chlorophyll (the green pigment).",
            
            "rainbow": "🌈 Rainbows form when sunlight shines through water droplets! Light enters the droplet and bends (refracts), reflects off the back, and bends again coming out. Each color bends at different angles, creating: Red, Orange, Yellow, Green, Blue, Indigo, Violet!",
            
            "thunder": "⚡ Thunder is the sound of lightning heating air! Lightning is incredibly hot (30,000°F - 5 times hotter than the sun's surface!). This superheats air so fast it explodes outward, creating the BOOM you hear!",
            
            "seasons": "🍂 Seasons happen because Earth is tilted 23.5 degrees! As Earth orbits the sun, different parts tilt toward or away from it. Summer = tilt toward sun (more direct sunlight). Winter = tilt away (less direct sunlight).",
            
            "rain": "🌧️ Rain forms when water vapor condenses in clouds! Warm air rises carrying water vapor. High up where it's cold, vapor condenses around tiny particles forming water droplets. When drops get too heavy, they fall as rain!",
            
            "volcanoes": "🌋 Volcanoes form when hot molten rock (magma) erupts from underground! Earth's crust has cracks where tectonic plates meet. Magma pushes through these cracks. Pressure builds and BOOM - eruption!",
            
            "earthquakes": "📊 Earthquakes happen when tectonic plates suddenly shift! Earth's crust is broken into huge plates. They constantly move slowly. Sometimes they get stuck. Pressure builds... then SNAP! They suddenly slip, releasing energy as seismic waves.",
            
            "moon phases": "🌙 Moon phases show how much sunlight we see on the moon! New Moon = dark side faces us. Full Moon = fully lit side faces us. The moon doesn't glow - it reflects sunlight! Takes 29.5 days for complete cycle.",
            
            "stars twinkle": "⭐ Stars twinkle because of Earth's atmosphere! Starlight travels through layers of moving air with different temperatures. This bends the light rapidly in different directions, making stars appear to twinkle. In space, stars don't twinkle!",
            
            "magnets": "🧲 Magnets work because tiny atoms inside line up! In magnetic materials, atoms act like mini magnets. When they line up in the same direction, they create a strong magnetic field. Opposite poles attract, same poles repel!",
            
            "electricity": "⚡ Electricity is the flow of electrons! Electrons are tiny negative particles in atoms. Current = electrons flowing through wire. Voltage = pressure pushing electrons. Conductors (like copper) let electrons flow easily.",
            
            "dna": "🧬 DNA is your genetic blueprint! It's a double helix (twisted ladder) made of base pairs: A-T and G-C. Every cell has a complete copy. Genes are sections of DNA that code for traits. You share 99.9% DNA with all humans, 98.8% with chimps!",
            
            "atoms": "⚛️ Atoms are building blocks of everything! They have protons (+), neutrons (neutral) in the nucleus, and electrons (-) orbiting. They're tiny - 100 million atoms fit on a pencil tip! Mostly empty space (99.9999%)!",

            // Animals
            "fastest animal": "🐆 Land: Cheetah at 70 mph! Can accelerate 0-60 in 3 seconds!\n🦅 Air: Peregrine falcon diving at 240+ mph!\n🐟 Water: Sailfish at 68 mph!",
            
            "biggest animal": "🐋 Blue whale - biggest animal EVER! Even bigger than any dinosaur! Length: up to 100 feet (3 school buses!), Weight: 200 tons (33 elephants!), Heart: size of small car, Tongue: weighs as much as an elephant!",
            
            "smallest animal": "🦟 Smallest mammal: Bumblebee bat (1 inch!)\n🐸 Smallest vertebrate: Paedophryne frog (7mm - size of housefly!)\n🦠 Smallest overall: Tardigrades/water bears (microscopic!)",
            
            "pandas eat": "🐼 Pandas eat bamboo - 26-84 pounds daily! They spend 12-16 hours a day eating. Even though they're bears, 99% of their diet is bamboo. They have a 'thumb' (modified wrist bone) to grip bamboo!",
            
            "penguins fly": "🐧 Penguins can't fly in AIR, but they FLY underwater! Wings evolved into flippers. They 'fly' through water at 15-25 mph. Some species reach 22 mph underwater. They traded flight for incredible swimming ability!",
            
            "dogs live": "🐕 Dog lifespan depends on size:\nSmall dogs: 12-20 years\nMedium dogs: 10-15 years\nLarge dogs: 10-13 years\nGiant dogs: 7-10 years\n\nLongest living dog: Bluey lived 29 years!",
            
            "cats purr": "😺 Cats purr by vibrating their larynx muscles! 25-150 vibrations per second. They purr when happy, want attention, nursing, sometimes when injured (healing). Purring may help heal bones and reduce stress!",
            
            "sharks old": "🦈 Sharks are ANCIENT - older than trees! First appeared 400 million years ago. Trees appeared 350 million years ago. Greenland sharks can live 400+ years (oldest living vertebrate!).",
            
            "dolphins smart": "🐬 Dolphins are incredibly intelligent! They use tools, have names for each other, recognize themselves in mirrors, play games, communicate with clicks/whistles, help injured dolphins and humans, learn quickly, solve puzzles!",
            
            "elephants": "🐘 Elephant facts: Exceptional memory, mourn their dead, live in matriarchal herds, use infrasound communication, use tools, self-aware, pregnancy lasts 22 months, trunk has 40,000 muscles!",
            
            "bees": "🐝 Bees are amazing! Colony has Queen, Drones, and Workers. Workers do all the work. Waggle dance tells others where flowers are. They pollinate 1/3 of our food crops. Population declining - we need to protect them!",

            // Space
            "sun size": "☀️ The sun is MASSIVE! Diameter: 864,000 miles (109x Earth!), Volume: 1.3 million Earths could fit inside! Mass: 333,000 times Earth's mass, Temperature: Surface 10,000°F, Core 27 million°F!",
            
            "planets": "🪐 8 planets in our solar system:\n1. Mercury - smallest, closest to sun\n2. Venus - hottest (900°F!), toxic atmosphere\n3. Earth - only planet with life!\n4. Mars - red planet, ice caps\n5. Jupiter - BIGGEST!\n6. Saturn - famous rings!\n7. Uranus - tilted sideways\n8. Neptune - windiest, farthest\n\nPluto is a dwarf planet now!",
            
            "black hole": "⚫ Black holes have gravity SO strong nothing escapes - not even light! They form when massive stars collapse. Event Horizon = point of no return. If you fell in, you'd be 'spaghettified' - stretched like spaghetti!",
            
            "mars": "🔴 Mars - The Red Planet! Red from iron oxide (rust!) in soil. Day: 24.6 hours (similar to Earth!). Atmosphere: 95% CO2, very thin. Temperature: -80°F average. Moons: 2 (Phobos and Deimos). Water: ice at poles!",
            
            "moon landing": "🌙 Moon landing (Apollo 11 - July 20, 1969): Astronauts Neil Armstrong and Buzz Aldrin landed. First words: 'That's one small step for man, one giant leap for mankind'. Brought back 47 pounds of moon rocks. Footprints still there!",

            // Math
            "pi": "🥧 Pi (π) = 3.14159265359... It's the ratio of a circle's circumference to diameter. Never ends, infinite decimal digits that never repeat! We use it to calculate circles, spheres, waves. Pi Day is March 14 (3/14)!",
            
            "infinity": "♾️ Infinity means endless - no end! It's not a number, it's a concept. You can't count to it. Infinity + 1 = still infinity! There are even different sizes of infinity (mind-blowing!).",
            
            "zero": "0️⃣ Zero revolutionized math! Invented in India ~500 AD. It's a placeholder AND a number. Made place-value system possible (10, 100, 1000). 5 + 0 = 5, 5 × 0 = 0, 5 ÷ 0 = undefined!",
            
            "fractions": "🍕 Fractions are parts of a whole! Like 3/8 of a pizza. Top (numerator) = parts you have. Bottom (denominator) = total parts. 1/2 = 0.5, 1/4 = 0.25, 3/4 = 0.75!",
            
            "percentage": "💯 Percentages are parts per hundred! 50% = 50/100 = 0.5 = half. To find 20% of 50: (20/100) × 50 = 10. 100%=all, 50%=half, 25%=quarter!",

            // History
            "dinosaurs extinct": "🦕 Dinosaurs went extinct 65 million years ago! Most likely from asteroid impact (10km wide) that hit Mexico. Caused massive explosions, tsunamis, wildfires, dust blocked sun for years. Plants died → herbivores died → carnivores died. Birds survived!",
            
            "pyramids built": "🏛️ Egyptian pyramids built ~4,500 years ago! Great Pyramid has 2.3 million stone blocks, each 2.5 tons. Built by 20,000-30,000 skilled laborers (NOT slaves!). Took 20-30 years. Used ramps, levers, sleds. No aliens needed!",
            
            "great wall": "🏯 Great Wall of China: Length 13,000+ miles! Built over 2,000+ years by different dynasties. Purpose: protect from invasions, control trade. Myth BUSTED: Can't see from space with naked eye!",

            // Life Skills
            "make friends": "👥 Making friends tips:\n1. Be yourself - genuine people attract real friends\n2. Show interest in others - ask questions\n3. Be a good listener\n4. Join clubs or activities you enjoy\n5. Be kind and positive\n6. Don't force it - friendships take time\n7. Smile and be approachable!\n\nQuality over quantity!",
            
            "study better": "📚 Study tips:\n1. Remove distractions (phone away!)\n2. Take breaks every 25-30 minutes\n3. Teach someone else (best way to learn)\n4. Use flashcards for memorization\n5. Study in different locations\n6. Get enough sleep!\n7. Practice, don't just read\n8. Ask questions when confused",
            
            "be confident": "💪 Building confidence:\n1. Practice what you're nervous about\n2. Focus on what you CAN do\n3. Set small achievable goals\n4. Celebrate small wins\n5. Take care of yourself\n6. Remember: everyone feels nervous sometimes\n7. Fake it till you make it\n8. Learn from mistakes, don't fear them",

            // Food & Cooking
            "pizza": "🍕 Pizza originated in Naples, Italy! Modern pizza with tomatoes became popular in the 1700s. First pizzeria opened 1830. Pizza Margherita (tomato, mozzarella, basil) was created for Queen Margherita in 1889!",
            
            "chocolate": "🍫 Chocolate comes from cacao beans! Grows on cacao trees in tropical regions. Beans are fermented, dried, roasted, and ground. Takes about 400 beans to make 1 pound of chocolate! Dark chocolate has health benefits!",
            
            "ice cream": "🍦 Ice cream dates back to ancient China! Modern ice cream became popular in 1600s Europe. Vanilla is most popular flavor worldwide! Ice cream is 50% air (that's why it's light and creamy)!",

            // Sports
            "basketball": "🏀 Basketball invented in 1891 by James Naismith! Originally used peach baskets. NBA founded 1946. Teams have 5 players on court. Game is 4 quarters, 12 minutes each (NBA). Popular worldwide!",
            
            "soccer": "⚽ Soccer (football) is world's most popular sport! Played in 200+ countries. 11 players per team. Game is 90 minutes (2 halves of 45). World Cup every 4 years is biggest sporting event!",
            
            "olympics": "🏅 Olympics started in ancient Greece 776 BC! Modern Olympics began 1896 in Athens. Summer and Winter Olympics alternate every 2 years. Represents unity through sports!",

            // Technology
            "internet": "💻 Internet started in 1960s as ARPANET (military project). Connected computers to share info. Tim Berners-Lee invented World Wide Web in 1989. Now billions use it daily!",
            
            "computer": "🖥️ Modern computers evolved from 1940s! First computers filled entire rooms. Now we have powerful computers in our pockets (smartphones!). They use binary (0s and 1s) for everything!",
            
            "phone": "📱 Alexander Graham Bell invented telephone in 1876! Mobile phones became popular in 1980s-90s. Smartphones (2007 with iPhone) changed everything. Now phones are mini computers!",

            // Nature
            "trees": "🌳 Trees produce oxygen and absorb CO2! They provide homes for animals, prevent soil erosion, give us fruit/nuts/wood. Oldest tree is 5,000+ years old! Trees communicate through underground fungal networks!",
            
            "ocean": "🌊 Oceans cover 71% of Earth! Average depth: 12,100 feet. Deepest point: Mariana Trench (36,000 feet!). Oceans produce 50%+ of Earth's oxygen. Only 5% explored!",
            
            "mountains": "🏔️ Mountains form from tectonic plates colliding! Himalayas still growing! Mount Everest is tallest at 29,032 feet. Mountains affect weather and climate. Home to unique ecosystems!",

            // Countries & Geography
            "countries": "🌍 There are 195 countries in the world! Biggest: Russia (6.6 million square miles). Smallest: Vatican City (0.17 square miles!). Most populous: India (1.4+ billion people)!",
            
            "continents": "🌎 7 continents: Asia (biggest), Africa, North America, South America, Antarctica, Europe, Australia (smallest). Asia has 60% of world's population!",

            // Weather
            "tornado": "🌪️ Tornadoes are rotating columns of air! Form from severe thunderstorms. Wind speeds can reach 300+ mph! Rated EF0-EF5 (EF5 most destructive). Most common in USA's 'Tornado Alley'!",
            
            "hurricane": "🌀 Hurricanes are massive tropical storms! Form over warm ocean water. Need water 80°F+ to form. Wind speeds 74+ mph. Categorized 1-5 (5 is strongest). Also called typhoons or cyclones!",
            
            "snow": "❄️ Snow forms when water vapor freezes into ice crystals in clouds! Crystals stick together forming snowflakes. Each snowflake is unique! Temperature must be 32°F or below. Takes millions of crystals to make snowball!",

            // Music
            "music": "🎵 Music uses sound organized in time! Elements: rhythm, melody, harmony, dynamics. Earliest instruments: flutes from 40,000 years ago! Music affects emotions and brain chemistry!",
            
            "piano": "🎹 Piano invented ~1700 by Bartolomeo Cristofori! Has 88 keys (52 white, 36 black). Can play loud or soft (that's why called pianoforte = soft-loud in Italian)!",

            // Art
            "painting": "🎨 Painting is applying pigment to surface! Techniques: oil, watercolor, acrylic, spray. Famous painters: Leonardo da Vinci, Van Gogh, Picasso, Monet. Art expresses ideas and emotions!",
            
            "colors": "🌈 Primary colors: Red, Yellow, Blue (can't be made by mixing). Secondary: Orange (red+yellow), Green (yellow+blue), Purple (blue+red). Colors affect mood and emotions!"
        };

        var games = [
            "🏃 SPEED OBBY: Racing with moving platforms!",
            "🍕 PIZZA SIMULATOR: Run pizza shop!",
            "🐾 PET SIMULATOR: Collect 100+ pets!",
            "⚔️ SWORD FIGHTER: Battle with swords!",
            "🏎️ KART RACING: Race on tracks!"
        ];

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲",
            "What do you call a fake noodle? An impasta! 🍝",
            "What did the ocean say to the beach? Nothing, it just waved! 🌊"
        ];

        addBot("👋 Hey! I'm your AI Helper!\n\n🎮 I specialize in Roblox Studio but can answer ANY question!\n\nAsk about:\n• How to make things in Roblox\n• Science, animals, space\n• Math, history, geography\n• Life advice, study tips\n• Sports, music, art\n• Literally anything!\n\nWhat would you like to know?");

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
                    response = "🧮 " + lower + " = " + result;
                } catch(e) {}
            }

            // Greetings
            if (!response && (lower.includes('hello') || lower.includes('hi') || lower.includes('hey'))) {
                response = "👋 Hey! Ask me about Roblox Studio or ANY question you have!";
            }

            if (!response && lower.includes('thank')) {
                response = "😊 You're welcome! What else can I help with?";
            }

            // Jokes
            if (!response && lower.includes('joke')) {
                response = random(jokes);
            }

            // Game ideas
            if (!response && (lower.includes('game idea') || lower.includes('obby') || lower.includes('simulator'))) {
                response = random(games);
            }

            // Check ROBLOX answers first (priority)
            if (!response) {
                for (var key in robloxAnswers) {
                    if (lower.includes(key) || 
                        lower.includes('make ' + key) || 
                        lower.includes('create ' + key) ||
                        lower.includes('how to ' + key)) {
                        response = robloxAnswers[key];
                        break;
                    }
                }
            }

            // Then check GENERAL knowledge
            if (!response) {
                for (var key in generalKnowledge) {
                    if (lower.includes(key)) {
                        response = generalKnowledge[key];
                        break;
                    }
                }
            }

            // Default
            if (!response) {
                response = "🤔 I can help with:\n\n🎮 Roblox Studio (how to make lava, doors, coins, etc)\n🔬 Science (why sky is blue, gravity, photosynthesis)\n🐾 Animals (fastest, biggest, how they work)\n🌌 Space (planets, sun, black holes)\n🧮 Math (calculations, fractions, percentages)\n📚 History (dinosaurs, pyramids, inventions)\n🌍 Geography (countries, continents, mountains)\n⚽ Sports (basketball, soccer, olympics)\n🎨 Art & Music\n💡 Life advice\n\nWhat would you like to know?";
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
