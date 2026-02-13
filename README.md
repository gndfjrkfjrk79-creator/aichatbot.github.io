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
            <h1>🎮 Roblox Game Idea Generator</h1>
            <p>Get unlimited creative game ideas for Roblox Studio!</p>
        </div>

        <div class="chat-container">
            <div class="messages" id="messages"></div>
            <div class="typing-indicator" id="typingIndicator">Generating idea...</div>
            <div class="suggestions" id="suggestions"></div>
            <div class="input-area">
                <input 
                    type="text" 
                    id="userInput" 
                    placeholder="What type of Roblox game do you want to make?"
                >
                <button id="sendBtn">Get Idea</button>
            </div>
        </div>
    </div>

    <script>
        const messagesDiv = document.getElementById('messages');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');
        const typingIndicator = document.getElementById('typingIndicator');
        const suggestionsDiv = document.getElementById('suggestions');

        // HUGE database of Roblox game ideas (100+ ideas!)
        const gameIdeas = {
            obby: [
                "🏃 **SPEED RUNNER OBBY**: Ultimate racing obby!\n• Moving platforms that speed up\n• Boost pads and slow zones\n• Checkpoints every 10 obstacles\n• Difficulties: Easy, Medium, Hard, INSANE\n• Leaderboard for fastest times\n• Secret shortcuts for pros\n• Rainbow trail when you finish\n• Speed multipliers as you progress",
                
                "🌈 **RAINBOW COLOR OBBY**: Each level = different color!\n• RED Level: Fire obstacles, lava jumps, heat damage\n• BLUE Level: Slippery ice platforms, frozen sections\n• GREEN Level: Nature vines, leaf platforms, tree jumping\n• YELLOW Level: Lightning strikes, electric barriers\n• PURPLE Level: Gravity flip zones, upside-down sections\n• ORANGE Level: Bouncy platforms, trampoline zones\n• Mix all colors for final boss level",
                
                "🚀 **SPACE OBBY ADVENTURE**: Journey through the galaxy!\n• Start on Earth, end at Black Hole\n• Zero gravity floating sections\n• Jump between asteroids\n• Rocket booster power-ups\n• Alien NPCs give hints and checkpoints\n• Collect stars for space shop\n• Unlock spaceship skins and trails\n• Planet-themed levels (Mars, Jupiter, Saturn)\n• Meteor dodge sections",
                
                "🏰 **MEDIEVAL CASTLE OBBY**: Climb the tower!\n• Start in dungeon, escape to rooftop\n• Swinging axes and arrow traps\n• Moving stone platforms\n• Knight NPCs patrol and give quests\n• Dragon race at the top (friendly!)\n• Collect gold coins for medieval shop\n• Unlock armor skins and sword trails\n• Secret treasure rooms\n• Castle courtyard parkour section",
                
                "🌊 **UNDERWATER OCEAN OBBY**: Deep sea adventure!\n• 50 levels from surface to ocean floor\n• Swimming mechanics with oxygen bubbles\n• Avoid sharks, jellyfish, whirlpools\n• Beautiful coral reef sections\n• Shipwreck exploration\n• Submarine checkpoints\n• Collect pearls for sea shop\n• Unlock sea creature pets\n• Bioluminescent night levels",
                
                "🎪 **CIRCUS SPECTACULAR OBBY**: Greatest show!\n• Tightrope walking sections\n• Cannon launchers between platforms\n• Trampoline bounce zones\n• Spinning circus wheels\n• Dodge juggling balls\n• Clown NPCs cheer you on\n• Unlock circus outfits and confetti effects\n• Trapeze swing sections\n• Funhouse mirror maze level"
            ],

            simulator: [
                "🐾 **MEGA PET SIMULATOR**: Ultimate pet collection!\n• Collect 100+ unique pets (common to mythical)\n• 3-stage pet evolution system\n• Build and decorate custom pet homes\n• Mini-games: fetch, agility course, pet races\n• Breeding system for rare combinations\n• Pet abilities (some find coins faster, others XP boost)\n• Daily pet care: feeding, bathing, playing\n• Trading system with other players\n• Pet accessories shop: hats, wings, trails\n• Legendary pets with special powers",
                
                "🍕 **PIZZA EMPIRE TYCOON**: Restaurant empire!\n• Start with tiny pizza stand\n• 50+ toppings to unlock\n• Hire chefs, delivery drivers, cashiers\n• Upgrade ovens for faster cooking\n• Expand to multiple restaurants\n• Custom pizza creator\n• Delivery mini-game (drive to houses)\n• Pizza-making contests\n• Unlock food truck\n• Catering business expansion",
                
                "⚔️ **SWORD MASTER SIMULATOR**: Legendary warrior!\n• Collect 200+ legendary swords\n• Train stats: Strength, Speed, Defense, Magic\n• Battle training dummies for XP\n• Quest system from village NPCs\n• Dungeon raids with epic bosses\n• Forge new swords from materials\n• Enchantment system for powers\n• PvP arena battles\n• Unlock armor sets and capes\n• Special sword combos and abilities",
                
                "🏝️ **ISLAND EMPIRE BUILDER**: Build paradise!\n• Start on tiny island, expand by buying land\n• Plant crops: wheat, corn, fruits, palm trees\n• Build houses, shops, parks, beaches\n• Attract tourists for income\n• Unlock new islands (volcanic, tropical, arctic)\n• Fishing and treasure hunting mini-games\n• Hire workers to automate tasks\n• Weather affects crops\n• Build bridges between islands",
                
                "🧙 **MAGIC ACADEMY SIMULATOR**: Master magic!\n• Learn 50+ spells across 5 schools\n• Attend classes: Potions, Charms, Transfiguration, Defense\n• Collect wands with different powers\n• Familiar pets boost magic abilities\n• Complete homework quests for XP\n• Wizard duels with other students\n• Unlock powerful spell combinations\n• Explore secret castle chambers\n• House system with competitions",
                
                "💎 **MINING TYCOON**: Dig for riches!\n• Mine gems from underground caves\n• 50+ gem types (ruby, diamond, emerald, mythical)\n• Upgrade pickaxe, drill, dynamite\n• Hire miners for auto-mining\n• Sell gems or craft jewelry\n• Unlock new locations: volcano, ice cave, crystal cavern\n• Discover ancient artifacts\n• Prestige system for rebirth bonuses\n• Build gem shop empire"
            ],

            adventure: [
                "🗺️ **TREASURE ISLAND QUEST**: Ultimate treasure hunt!\n• Massive island map with 20+ locations\n• Find treasure map pieces\n• Solve riddles and puzzles\n• Explore mysterious caves and temples\n• Dodge ancient traps: arrows, rolling boulders\n• Boss battle: Ancient Guardian\n• Hidden treasure rooms with rare loot\n• Map reveals as you explore\n• Collect artifacts for rewards\n• Secret underwater cave",
                
                "🏰 **KINGDOM QUEST RPG**: Save the realm!\n• Create character: Knight, Wizard, Archer, Rogue\n• 30+ quests from different NPCs\n• Battle system with skills and combos\n• Explore forests, caves, villages, castles\n• Collect weapons, armor, potions\n• Level up system (1-50)\n• Final boss: Dark Sorcerer in tower\n• Side quests for legendary items\n• Party system (play with friends)",
                
                "🌲 **ENCHANTED FOREST**: Magical journey!\n• Meet mystical creatures: fairies, unicorns, talking trees\n• 15 different forest zones\n• Gather magical berries and herbs\n• Craft potions and spells\n• Animal companion system\n• Build treehouse hideout\n• Seasonal events (spring flowers, autumn leaves)\n• Mystery: Why is forest magic fading?\n• Restore magic through quests",
                
                "🏴‍☠️ **PIRATE ADVENTURE**: Sail the seas!\n• Captain your own pirate ship\n• Visit 10 different islands\n• Treasure maps lead to buried gold\n• Naval battles with enemy pirates\n• Recruit crew members\n• Upgrade ship: cannons, sails, hull\n• Sea monster encounters (kraken!)\n• Trade goods between ports\n• Legendary treasure finale",
                
                "🏔️ **MOUNTAIN EXPEDITION**: Climb the peak!\n• Multi-day journey with camps\n• Survival: warmth, energy, hunger\n• Beautiful scenic viewpoints\n• Weather challenges: storms, avalanches\n• Wildlife: eagles, mountain goats\n• Upgrade climbing gear\n• Photograph rare sights\n• Secret cave systems\n• Plant flag at summit"
            ],

            racing: [
                "🏎️ **TURBO KART RACING**: Ultimate kart championship!\n• 20 unique tracks: city, beach, volcano, space, jungle\n• 50+ karts to collect and unlock\n• Power-ups: rocket boost, shield, oil slick, lightning\n• Customize: paint, decals, wheels, spoilers, neon lights\n• Championship mode (10 races, points system)\n• Time trial with ghost racers\n• Multiplayer races (8 players)\n• Drift mechanics for sharp turns\n• Secret shortcuts on each track\n• Unlock legendary karts",
                
                "🛹 **SKATE PARK PRO**: Extreme skating!\n• Massive skate park with 10 sections\n• Trick system: kickflip, ollie, grind, manual, heelflip\n• Combo multiplier (chain tricks)\n• Collect S-K-A-T-E letters\n• Create custom skate parks\n• Unlock 30+ boards and styles\n• Sponsored challenges\n• Competitions with rankings\n• Film mode (record runs)\n• Street skating in city",
                
                "🏁 **DRAG RACING LEGENDS**: Quarter mile!\n• Reaction time perfect launch\n• Gear shift timing mechanics\n• Nitrous oxide boost button\n• Tune cars: engine, tires, transmission, weight\n• 40+ cars from different eras\n• Underground racing story mode\n• Pink slip races (winner takes car)\n• Custom wraps and underglow\n• Dyno shop performance testing",
                
                "🚁 **AIR RACE EXTREME**: Sky racing!\n• Helicopters, planes, jets\n• Ring checkpoint courses through clouds\n• Barrel rolls and loop-de-loops\n• Weather challenges: storms, wind\n• 15 aircraft to unlock\n• Canyon racing (tight spaces!)\n• Stunt challenges\n• Dogfight race mode\n• Unlock legendary aircraft"
            ],

            tycoon: [
                "🏪 **MEGA MALL TYCOON**: Shopping empire!\n• Start with 1 shop, expand to 50+ stores\n• Store types: clothing, food, electronics, toys, sports\n• Hire employees and security\n• Parking lot with valet service\n• Food court with multiple restaurants\n• Movie theater and arcade\n• Seasonal sales events\n• Upgrade decorations and lighting\n• Customer satisfaction meter\n• Expand to multiple malls",
                
                "🎢 **THEME PARK EMPIRE**: Amusement paradise!\n• Design 30+ different rides\n• Custom roller coaster builder\n• Food stands and game booths\n• Hire performers and mascots\n• Queue line management\n• Park cleanliness affects reviews\n• Nightly fireworks shows\n• Seasonal themes (Halloween, Christmas)\n• VIP fast pass system\n• Water park expansion",
                
                "🏗️ **CITY
            
