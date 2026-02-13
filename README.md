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
            font-size: 2.8em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.2em;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
        }

        .stats {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .stat-box {
            background: rgba(255,255,255,0.2);
            padding: 15px 25px;
            border-radius: 10px;
            color: white;
            text-align: center;
        }

        .stat-box h3 {
            font-size: 2em;
            margin-bottom: 5px;
        }

        .stat-box p {
            font-size: 0.9em;
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
            <h1>🎮 Roblox Game Idea Generator</h1>
            <p>Get unlimited creative game ideas for Roblox Studio!</p>
        </div>

        <div class="stats">
            <div class="stat-box">
                <h3 id="ideaCount">150+</h3>
                <p>Game Ideas</p>
            </div>
            <div class="stat-box">
                <h3>15+</h3>
                <p>Categories</p>
            </div>
            <div class="stat-box">
                <h3 id="generatedCount">0</h3>
                <p>Ideas Generated</p>
            </div>
        </div>

        <div class="chat-container">
            <div class="messages" id="messages"></div>
            <div class="typing-indicator" id="typingIndicator">Generating ideas...</div>
            <div class="suggestions" id="suggestions"></div>
            <div class="input-area">
                <input 
                    type="text" 
                    id="userInput" 
                    placeholder="What type of game do you want to make?"
                    onkeypress="handleKeyPress(event)"
                >
                <button onclick="sendMessage()" id="sendBtn">Get Idea</button>
            </div>
        </div>
    </div>

    <script>
        const messagesDiv = document.getElementById('messages');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');
        const typingIndicator = document.getElementById('typingIndicator');
        const suggestionsDiv = document.getElementById('suggestions');
        let generatedIdeas = 0;

        // MASSIVE game ideas database - 150+ ideas!
        const gameIdeas = {
            obby: [
                "🏃 **MEGA SPEED OBBY**: Create the ultimate speed challenge! Add:\n• Moving platforms that speed up over time\n• Boost pads and slow zones\n• Checkpoints every 10 obstacles\n• Different difficulties: Easy (Green), Medium (Yellow), Hard (Red), INSANE (Black)\n• Leaderboard showing fastest times\n• Secret shortcut paths for skilled players\n• Rainbow trail effect for completing levels",
                
                "🌈 **COLOR FUSION OBBY**: Every 5 levels, colors change with unique mechanics!\n• RED Level: Fire obstacles, lava jumps\n• BLUE Level: Ice platforms (slippery!)\n• GREEN Level: Nature theme with vines and leaves\n• YELLOW Level: Lightning strikes and electric barriers\n• PURPLE Level: Gravity switches, walk on ceiling\n• Mix levels for ultimate challenge!",
                
                "🚀 **SPACE ADVENTURE OBBY**: Journey through the galaxy!\n• Start on Earth, end at a Black Hole\n• Each planet = different stage (Mars, Jupiter, Saturn)\n• Zero gravity sections - float between asteroids\n• Rocket boosters as power-ups\n• Alien NPCs that give hints\n• Collect stars for shop currency\n• Buy spaceship skins and trails",
                
                "🏰 **MEDIEVAL CASTLE OBBY**: Climb the tallest castle!\n• Start in dungeon, escape to rooftop\n• Swinging axes, arrow traps, moving walls\n• Knight NPCs patrol and give quests\n• Dragon boss at top (friendly race!)\n• Collect gold coins for medieval shop\n• Unlock armor skins and sword trails\n• Secret treasure rooms with rare items",
                
                "🌊 **UNDERWATER OCEAN OBBY**: Deep sea diving adventure!\n• 50 levels from surface to ocean floor\n• Swimming mechanics with oxygen bubbles\n• Avoid sharks, jellyfish, and whirlpools\n• Beautiful coral reefs and shipwrecks\n• Submarine checkpoints\n• Collect pearls to unlock sea creature pets\n• Bioluminescent levels (glow in the dark!)",
                
                "🎪 **CIRCUS SPECTACULAR OBBY**: The greatest show on earth!\n• Tightrope walking sections\n• Cannon launchers between platforms\n• Trampoline bounce areas\n• Spinning circus wheels\n• Juggling ball obstacles to dodge\n• Clown NPCs cheer and give tips\n• Unlock circus outfits and confetti effects",
                
                "🏔️ **MOUNTAIN CLIMBER OBBY**: Scale the highest peak!\n• Start at base camp, climb to summit\n• Ice climbing sections with slippery rocks\n• Avalanche escape sequences\n• Camp checkpoints to rest\n• Weather changes (snow, wind, clear)\n• Collect mountain gear as rewards\n• Plant flag at the top for achievement!",
                
                "🎮 **RETRO ARCADE OBBY**: Nostalgic 8-bit themed levels!\n• Pixelated platforms and obstacles\n• Classic game references in each level\n• Pac-Man maze section\n• Space Invaders dodge sequence\n• Tetris block jumping puzzle\n• 8-bit music and sound effects\n• Unlock retro game character skins"
            ],

            simulator: [
                "🐾 **ULTIMATE PET PARADISE**: The most complete pet simulator!\n• 100+ unique pets to collect (common to mythical)\n• Pet evolution system (3 stages each)\n• Build custom pet home with decorations\n• Mini-games: fetch, agility course, pet races\n• Breeding system for rare combinations\n• Pet abilities: some find coins faster, others give XP boost\n• Daily pet care: feeding, bathing, playing\n• Trade pets with friends\n• Pet accessories shop: hats, glasses, wings",
                
                "🍕 **PIZZA EMPIRE TYCOON**: Build a pizza restaurant empire!\n• Start with small pizza stand\n• 50+ toppings to unlock\n• Hire staff: chefs, delivery drivers, cashiers\n• Upgrade ovens for faster cooking\n• Expand to multiple restaurants\n• Custom pizza creator for special orders\n• Delivery mini-game (drive to houses)\n• Compete in pizza-making contests\n• Unlock food truck and catering business",
                
                "⚔️ **LEGENDARY SWORD MASTER**: Train to be the greatest warrior!\n• Collect 200+ legendary swords\n• Train stats: Strength, Speed, Defense\n• Battle training dummies for XP\n• Quest system from village NPCs\n• Dungeon raids with bosses\n• Forge new swords by combining materials\n• Enchantment system for special powers\n• PvP arena for player battles\n• Unlock armor sets and special moves",
                
                "🏝️ **ISLAND EMPIRE BUILDER**: Create your dream island paradise!\n• Start on tiny island, expand by buying land\n• Plant crops: wheat, corn, fruits, trees\n• Build houses, shops, parks, beaches\n• Attract tourists for income\n• Unlock new islands (volcanic, tropical, arctic)\n• Fishing and treasure hunting mini-games\n• Hire workers to automate tasks\n• Weather system affects crops\n• Create roads and transportation",
                
                "🧙 **MAGIC ACADEMY SIMULATOR**: Master the mystical arts!\n• Learn 50+ spells across 5 schools of magic\n• Attend classes: Potions, Charms, Transfiguration\n• Collect wands with different powers\n• Familiar pets that boost magic\n• Complete homework quests for XP\n• Duel other students in wizard battles\n• Unlock spell combinations\n• Explore secret chambers in the castle\n• House system with team competitions",
                
                "💎 **GEM MINING EMPIRE**: Dig for fortune!\n• Mine gems from underground caves\n• 50+ gem types (ruby, diamond, emerald, rare ones)\n• Upgrade pickaxe, drill, and equipment\n• Hire miners to auto-mine\n• Sell gems or craft jewelry\n• Unlock new mining locations (volcano, ice cave)\n• Discover ancient artifacts\n• Prestige system for rebirth bonuses\n• Build above-ground gem shop",
                
                "🎨 **ART STUDIO CREATOR**: Become a master artist!\n• Paint on canvas with 20+ colors\n• Sell artwork for coins\n• Unlock new art styles and tools\n• Gallery to display your best work\n• Art contests with other players\n• Commission system (NPCs request art)\n• Upgrade studio with better easels, lighting\n• Learn techniques: watercolor, oil, digital\n• Frame and decorate your gallery",
                
                "🏋️ **GYM TRAINING SIMULATOR**: Get super strong!\n• Train in gym: weights, treadmill, boxing\n• Level up: Strength, Speed, Endurance\n• Unlock new exercises and equipment\n• Compete in competitions (lifting, running)\n• Hire personal trainer for bonuses\n• Healthy food shop affects stats\n• Unlock gyms in different cities\n• Create custom workout routines\n• Achievement system for milestones"
            ],

            adventure: [
                "🗺️ **LOST TEMPLE EXPEDITION**: Explore ancient ruins!\n• Massive temple with 20+ rooms to discover\n• Solve hieroglyph puzzles to open doors\n• Dodge ancient traps: arrows, falling rocks, spikes\n• Collect artifacts for museum rewards\n• Boss battle: Ancient Guardian statue\n• Hidden treasure rooms with rare loot\n• Map system that fills in as you explore\n• Torch lighting mechanic in dark areas\n• Multiple endings based on choices",
                
                "🏰 **KINGDOM QUEST RPG**: Save the kingdom from darkness!\n• Create character: Knight, Wizard, Archer, or Rogue\n• 30+ quests from different NPCs\n• Battle system with skills and magic\n• Explore forests, caves, villages, castles\n• Collect weapons, armor, and potions\n• Level up system (1-50)\n• Final boss: Dark Sorcerer in his tower\n• Side quests for rare rewards\n• Party system (team with friends)",
                
                "🌲 **ENCHANTED FOREST ADVENTURE**: Magical woodland journey!\n• Meet mystical creatures: fairies, unicorns, talking trees\n• 15 different forest areas to explore\n• Gather magical berries and herbs\n• Craft potions and spells\n• Animal companion system (fox, owl, deer)\n• Build treehouse hideout\n• Seasonal events (spring flowers, autumn leaves)\n• Mystery story: Why is the forest magic fading?\n• Restore magic by completing quests",
                
                "🏔️ **MOUNTAIN PEAK ADVENTURE**: Conquer the summit!\n• Multi-day journey with camps\n• Survival mechanics: warmth, energy, hunger\n• Beautiful scenic viewpoints\n• Weather challenges: storms, avalanches\n• Wildlife encounters (eagles, mountain goats)\n• Climbing gear progression\n• Photograph rare sights for rewards\n• Secret cave systems with treasures\n• Achievement for reaching summit at sunrise",
                
                "🏴‍☠️ **PIRATE TREASURE HUNT**: Sail the seven seas!\n• Captain your own pirate ship\n• Visit 10 different islands\n• Treasure maps lead to buried gold\n• Naval battles with enemy pirates\n• Recruit crew members\n• Upgrade ship: cannons, sails, hull\n• Sea monster encounters (kraken!)\n• Trade goods between ports\n• Legendary treasure as final goal",
                
                "🌵 **DESERT OASIS QUEST**: Journey across the dunes!\n• Cross vast desert to reach paradise oasis\n• Ride camels for faster travel\n• Find water sources to survive\n• Ancient pyramids to explore\n• Sandstorm events (take shelter!)\n• Desert bandits and friendly nomads\n• Discover lost city buried in sand\n• Solve sphinx riddles\n• Collect golden scarabs",
                
                "❄️ **FROZEN KINGDOM ADVENTURE**: Explore the icy realm!\n• Ice palace with frozen rooms\n• Sliding ice puzzles\n• Befriend snow creatures\n• Restore warmth to the frozen land\n• Ice magic abilities\n• Snowflake collecting\n• Aurora borealis viewing spots\n• Hot spring rest areas\n• Epic boss: Ice Dragon",
                
                "🎭 **MYSTERY MANSION DETECTIVE**: Solve the case!\n• Explore 30-room haunted mansion\n• Collect clues and evidence\n• Interview ghost NPCs\n• Puzzle rooms with riddles\n• Secret passages behind bookcases\n• Piece together the mystery story\n• Multiple suspects and endings\n• Detective tools: magnifying glass, notepad\n• Unlock true ending with all clues"
            ],

            racing: [
                "🏎️ **TURBO KART CHAMPIONSHIP**: Ultimate kart racing!\n• 20 unique racing tracks (city, beach, volcano, space)\n• 50+ karts to unlock and collect\n• Power-ups: rockets, shields, speed boost, oil slick\n• Customize karts: paint, decals, wheels, spoilers\n• Championship mode (10 races, points system)\n• Time trial with ghost racing\n• Multiplayer races (8 players)\n• Drift mechanics for sharp turns\n• Secret shortcuts on each track",
                
                "🛹 **EXTREME SKATE PARK**: Pull off insane tricks!\n• Massive skate park with multiple sections\n• Trick system: kickflip, ollie, grind, manual\n• Combo multiplier (chain tricks together)\n• S-K-A-T-E letters to collect\n• Create custom skate parks\n• Unlock new boards and styles\n• Sponsored challenges for rewards\n• Compete in competitions\n• Film mode (record your best runs)",
                
                "🏁 **DRAG RACING LEGENDS**: Quarter-mile mayhem!\n• Reaction time start (perfect launch)\n• Shift timing mechanics\n• Nitrous oxide boost system\n• Tune cars: engine, tires, transmission\n• 40+ cars from different eras\n• Underground racing story mode\n• Pink slip races (winner takes car)\n• Customize with wraps and underglow\n• Dyno shop to test performance",
                
                "🚁 **SKY RACE CHALLENGE**: Aerial racing action!\n• Helicopter, plane, and jet races\n• Ring checkpoint courses through clouds\n• Barrel roll and loop-de-loop tricks\n• Weather challenges (storms, wind)\n• 15 aircraft to unlock\n• Canyon racing (tight spaces!)\n• Dogfight mode (race while dodging)\n• Stunt challenges\n• Unlock legendary aircraft",
                
                "🏇 **DERBY DAY RACING**: Horse racing excitement!\n• Train and care for horses\n• 10 different horse breeds\n• Jump obstacles on race course\n• Betting system for rewards\n• Breed horses for perfect racer\n• Jockey outfit customization\n• Famous tracks to compete on\n• Horse bonding affects performance\n• Kentucky Derby style championship",
                
                "🚤 **WATER RACING MANIA**: Speed across the waves!\n• Jet skis, speedboats, and yachts\n• Ocean, river, and lake tracks\n• Wave physics affect handling\n• Trick jumps off ramps\n• Marine animal obstacles\n• Underwater tunnel sections\n• Weather: calm, choppy, stormy\n• Customize boats with colors and flags\n• Pirate ship race special event",
                
                "🏃 **PARKOUR SPRINT RACE**: Free-running competition!\n• Race while doing parkour moves\n• Wall runs, precision jumps, vaults\n• City rooftop courses\n• Time attack mode\n• Multiplayer racing\n• Unlock new movement abilities\n• Style points for clean runs\n• Create custom parkour courses\n• World record leaderboards",
                
                "🚂 **RAILROAD RACING**: Train racing madness!\n• Control powerful locomotives\n• Switch tracks to take shortcuts\n• Coal management (speed vs fuel)\n• 10 historical trains to unlock\n• Mountain passes and city routes\n• Passenger vs freight trains\n• Rail yard customization\n• Weather affects track conditions\n• Cross-country championship"
            ],

            tycoon: [
                "🏪 **MEGA MALL EMPIRE**: Build the ultimate shopping center!\n• Start with 1 small shop, expand to 50+ stores\n• Different store types: clothing, food, electronics, toys\n• Hire employees and security guards\n• Parking lot and valet service\n• Food court with multiple restaurants\n• Movie theater and arcade\n• Seasonal sales and events\n• Upgrade decorations and lighting\n• Customer satisfaction affects profits",
                
                "🎢 **THEME PARK TYCOON DELUXE**: Create the happiest place!\n• Design 30+ different rides\n• Custom roller coaster builder\n• Food stands, game booths, gift shops\n• Hire performers and mascots\n• Queue line management\n• Park cleanliness matters\n• Fireworks show at night\n• Seasonal themes (Halloween, Christmas)\n• VIP pass system for extra income",
                
                "🏗️ **CITY BUILDER PRO**: Grow from village to metropolis!\n• Zone areas: residential, commercial, industrial\n• Build infrastructure: roads, power, water\n• Unlock 100+ building types\n• Manage budget and taxes\n• Disaster events: storms, fires\n• Public services: police, fire, hospital\n• Public transportation: buses, subway, trains\n• Citizen happiness meter\n• Landmarks and monuments",
                
                "🍔 **FAST FOOD EMPIRE**: Become restaurant royalty!\n• Multiple food chains to manage\n• Menu customization\n• Drive-thru and dine-in\n• Hire and train staff\n• Marketing campaigns\n• Compete with rival restaurants\n• Expand to new cities\n• Food quality vs speed balance\n• Secret menu items for loyal customers",
                
                "🏨 **LUXURY RESORT TYCOON**: Build 5-star paradise!\n• Beachfront resort management\n• Room types: standard to presidential suite\n• Amenities: pool, spa, restaurant, bar\n• Events: weddings, conferences\n• Staff management\n• Guest reviews affect reputation\n• Expand to multiple properties\n• Activities: surfing, diving, golf\n• VIP guest special requests",
                
                "🎮 **ARCADE EMPIRE**: Retro gaming business!\n• 50+ arcade cabinets to place\n• Ticket redemption prizes\n• Claw machines and skill games\n• VR gaming section\n• Snack bar\n• Tournament hosting\n• Maintain and repair machines\n• Nostalgic decor themes\n• Attract different customer types",
                
                "🏭 **FACTORY TYCOON**: Industrial production!\n• Build production lines\n• Raw materials to finished products\n• Automate with conveyor belts\n• Hire workers for different tasks\n• Research new technologies\n• Fulfil orders for profit\n• Expand factory floor space\n• Efficiency upgrades\n• Supply chain management",
                
                "🎬 **MOVIE STUDIO MOGUL**: Hollywood success!\n• Produce different film genres\n• Hire actors, directors, crew\n• Build movie sets\n• Special effects department\n• Script selection\n• Marketing and premieres\n• Box office tracking\n• Awards and recognition\n• Franchise development"
            ],

            roleplay: [
                "🏡 **DREAM NEIGHBORHOOD**: Perfect suburban life!\n• Own customizable house\n• 20+ furniture items\n• Adopt pets (dogs, cats, birds)\n• Jobs: teacher, doctor, chef, artist\n• Neighborhood events: BBQ, garage sales\n• Drive cars around town\n• Visit friends' houses\n• Seasons change decorations\n• Local shops to visit",
                
                "🎒 **ULTIMATE SCHOOL LIFE**: Complete school experience!\n• Choose student or teacher role\n• 10 different classrooms\n• Take classes: Math, Science, Art, PE\n• Cafeteria with food choices\n• Recess playground activities\n• Lockers to customize\n• School events: dances, sports day\n• Clubs: drama, music, sports\n• Report cards and achievements",
                
                "🏥 **MEDICAL CENTER RP**: Hospital simulation!\n• Roles: doctor, nurse, patient, surgeon\n• Different departments: ER, surgery, pediatrics\n• Medical tools and equipment\n• Patient care mini-games\n• Ambulance service\n• Pharmacy and lab\n• Medical uniforms customization\n• Emergency scenarios\n• Hospital upgrades",
                
                "🌆 **CITY LIFE SIMULATOR**: Urban living!\n• Apartments and houses\n• 20+ job options\n• Shopping mall and stores\n• Restaurants and cafes\n• Public transport: bus, subway, taxi\n• Parks and entertainment\n• Police and firefighter roles\n• City events and festivals\n• Day/night cycle affects activities",
                
                "🏖️ **BEACH RESORT LIFE**: Tropical paradise!\n• Beach houses and hotels\n• Water activities: swimming, surfing, diving\n• Beach cafe jobs\n• Volleyball and sandcastle building\n• Boat rentals\n• Sunset parties\n• Seashell collecting\n• Lifeguard role\n• Tropical outfits",
                
                "🏰 **ROYAL CASTLE RP**: Medieval fantasy!\n• Roles: king, queen, knight, wizard, peasant\n• Throne room and great hall\n• Jousting tournaments\n• Royal feasts\n• Castle defense from dragons\n• Medieval jobs and trades\n• Coronation ceremonies\n• Explore dungeon and towers\n• Royal decree system",
                
                "🚀 **SPACE STATION LIFE**: Futuristic RP!\n• Astronaut, scientist, engineer roles\n• Zero gravity sections\n• Space missions and repairs\n• Alien encounters (friendly!)\n• Research labs\n• Spaceship hangars\n• Galactic travel\n• Space suits and equipment\n• Communication with Earth",
                
                "🎪 **CIRCUS PERFORMERS**: Under the big top!\n• Roles: ringmaster, acrobat, clown, magician\n• Performance shows for audience\n• Practice and train skills\n• Animal care (friendly circus animals)\n• Costume customization\n• Travel to different cities\n• Circus tent customization\n• Ticket sales management\n• Applause rating system"
            ],

            tower_defense: [
                "🗼 **CASTLE SIEGE DEFENSE**: Protect the kingdom!\n• 15 tower types: archers, cannons, wizards, catapults\n• 50 waves of enemies getting harder\n• Upgrade towers (3 levels each)\n• Special abilities: freeze, poison, fire\n• Different enemy types: goblins, orcs, trolls, dragons\n• Boss waves every 10 levels\n• Strategic tower placement\n• Castle health meter\n• Co-op mode with friends",
                
                "🌸 **GARDEN GUARDIAN**: Protect your plants!\n• Flower towers shoot petals\n• Sunflowers generate energy\n• Venus flytraps eat enemies\n• Defend against bugs and pests\n• Cute, colorful graphics\n• Water your towers to heal them\n• Seasons change tower abilities\n• Butterfly helpers as power-ups\n• Unlock exotic plants",
                
                "🏖️ **BEACH DEFENSE BATTLE**: Save the sandcastle!\n• Sand towers, water cannons, seashell launchers\n• Enemy crabs, jellyfish, and sea monsters\n• Tides affect gameplay\n• Recruit seagulls and dolphins\n• Build moats and barriers\n• Treasure chest rewards\n• Surfboard patrols\n• Lighthouse gives vision\n• Coconut catapult special attack",
                
                "🍭 **SWEET SHOP DEFENSE**: Candy battle!\n• Lollipop shooters and gummy cannons\n• Chocolate walls and caramel traps\n• Ants and mice steal candy\n• Cotton candy clouds slow enemies\n• Upgrade with sugar coins\n• Candy cane barriers\n• Boss: Giant Gummy Bear\n• Ice cream freeze ability\n• Soda pop explosive traps",
                
                "🏔️ **ARCTIC FORTRESS**: Snow and ice warfare!\n• Snowball turrets and ice spikes\n• Penguin warriors help defend\n• Avalanche special attack\n• Enemy: polar bears, yetis, snow monsters\n• Igloo barracks spawn defenders\n• Northern lights power boost\n• Blizzard slows all enemies\n• Hot cocoa heals towers\n• Ice sculpture maze paths",
                
                "🌋 **VOLCANO DEFENSE**: Lava and fire!\n• Lava cannons and fire towers\n• Rock golems as ground troops\n• Magma moat around base\n• Enemies: ice monsters (weak to fire)\n• Eruption ultimate ability\n• Obsidian walls\n• Dragon ally flyovers\n• Geothermal energy system\n• Ash cloud concealment",
                
                "🏙️ **CITY DEFENDER**: Urban warfare!\n• Police towers, SWAT teams\n• Rooftop snipers\n• Road blocks and barriers\n• Helicopter support\n• Enemy: zombies, aliens, robots\n• Upgrade city defenses\n• Skyscraper vantage points\n• Emergency services backup\n• Evacuation missions",
                
                "🌲 **FOREST FORTRESS**: Nature's defense!\n• Tree towers with vine lassos\n• Rock throwing positions\n• Animal allies: bears, wolves\n• Defend ancient tree\n• Enemies: loggers and machines\n• Root barrier system\n• Bird's eye view scouting\n• Mushroom poison clouds\n• Seasons affect strategy"
            ],

            survival: [
                "🏝️ **STRANDED ISLAND SURVIVAL**: Ultimate island challenge!\n• Start with nothing on deserted island\n• Gather resources: wood, stone, fruit\n• Craft tools and weapons\n• Build shelter from storms\n• Hunt animals and fish\n• Fresh water management\n• Explore island for secrets\n• Signal fire for rescue\n• Day/night survival cycle\n• Weather survival challenges",
                
                "❄️ **ARCTIC SURVIVAL**: Frozen wilderness!\n• Temperature management critical\n• Build igloo shelters\n• Ice fishing for food\n• Craft warm clothing\n• Blizzard survival\n• Polar bear encounters\n• Northern lights navigation\n• Limited daylight planning\n• Frozen lake dangers\n• Rescue mission goal",
                
                "🌋 **VOLCANO ISLAND ESCAPE**: Race against eruption!\n• Island volcano is erupting!\n• Build raft to escape\n• Gather supplies while lava flows\n• Evacuate with friends (co-op)\n• Timed survival\n• Rescue animals too\n• Hot zones to avoid\n• Earthquake events\n• Ash cloud visibility\n• Epic escape sequence",
                
                "🌵 **DESERT SURVIVAL**: Extreme heat challenge!\n• Find oasis for water\n• Build shade shelters\n• Cacti provide resources\n• Sandstorms hit randomly\n• Scorpion and snake dangers\n• Camel companion\n• Mirages confuse navigation\n• Night is freezing cold\n• Ancient ruins shelter\n• Rescue caravan arrives eventually",
                
                "🌲 **WILDERNESS CAMPING**: Forest survival!\n• Set up camp site\n• Gather firewood and kindling\n• Cook food on campfire\n• Pitch tent before dark\n• Wildlife encounters\n• Hiking for resources\n• Stream for fresh water\n• Storm preparation\n• S'mores mini-game\n• Nature photography spots",
                
                "🏚️ **ABANDONED CITY**: Urban survival!\n• Scavenge buildings for supplies\n• Avoid hazards and dangers\n• Build safe zone base\n• Limited resources\n• Other survivors to help\n• Grow food in gardens\n• Defend from threats\n• Explore different districts\n• Vehicle restoration\n• Community rebuilding",
                
                "🌊 **OCEAN RAFT SURVIVAL**: Lost at sea!\n• Expand raft with debris\n• Catch fish for food\n• Collect rainwater\n• Shark circle the raft\n• Island hopping\n• Craft better equipment\n• Weather the storms\n• Navigation by stars\n• Whale encounters\n• Find inhabited island",
                
                "🏔️ **MOUNTAIN EXPEDITION**: Alpine
            
