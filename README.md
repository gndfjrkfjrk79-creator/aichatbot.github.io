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
        // MEGA DATABASE - 100+ ROBLOX GAME IDEAS!
        var games = {
            obby: [
                "🏃 SPEED RUNNER OBBY: Ultimate racing challenge! Moving platforms that accelerate, boost pads, slow zones, checkpoints every 10, difficulties Easy to INSANE, leaderboard for fastest times, secret shortcuts, rainbow trail on completion!",
                "🌈 RAINBOW COLOR OBBY: Each level different color! RED=fire/lava, BLUE=ice/slippery, GREEN=nature/vines, YELLOW=lightning/electric, PURPLE=gravity flip, ORANGE=bouncy, final level mixes all!",
                "🚀 SPACE OBBY: Journey from Earth to Black Hole! Zero gravity sections, asteroid jumping, rocket boosters, alien NPCs give hints, collect stars for shop, unlock spaceship skins and trails, planet-themed levels!",
                "🏰 MEDIEVAL CASTLE OBBY: Escape dungeon to rooftop! Swinging axes, arrow traps, moving stone platforms, knight patrols, friendly dragon race at top, medieval shop with armor/sword skins, secret treasure rooms!",
                "🌊 UNDERWATER OBBY: 50 levels surface to ocean floor! Swimming mechanics, oxygen bubbles, avoid sharks/jellyfish, coral reefs, shipwrecks, submarine checkpoints, collect pearls, bioluminescent levels!",
                "🎪 CIRCUS OBBY: Greatest show! Tightrope walking, cannon launchers, trampoline zones, spinning wheels, dodge juggling balls, clown NPCs cheer, unlock circus outfits and confetti effects!",
                "❄️ ICE MOUNTAIN OBBY: Climb frozen peak! Slippery ice physics, avalanche escape sequences, ice caves, yeti encounters, hot cocoa checkpoints, skiing sections, snowball dodge areas!",
                "🏜️ DESERT PYRAMID OBBY: Ancient Egypt theme! Sand traps, mummy NPCs, hieroglyph puzzles, scarab collecting, pharaoh's treasure at top, sandstorm sections, oasis checkpoints!",
                "🌋 VOLCANO OBBY: Active volcano! Lava rising sections, heat shield power-ups, eruption escape sequences, magma monsters, obsidian platforms, gem collecting, firefighter checkpoints!",
                "🎃 HALLOWEEN OBBY: Spooky fun! Haunted house sections, friendly ghosts, pumpkin platforms, candy collecting, witch NPCs, spider web obstacles, graveyard maze, trick-or-treat checkpoints!",
                "🎄 CHRISTMAS OBBY: Winter wonderland! Elf workshops, candy cane paths, snowflake platforms, present collecting, Santa's sleigh race, reindeer jumps, gingerbread house sections!",
                "🌲 JUNGLE OBBY: Wild adventure! Vine swinging, waterfall jumps, quicksand pits, monkey helpers, ancient ruins, treasure hunting, animal crossing sections, treehouse checkpoints!",
                "🏙️ CITY PARKOUR OBBY: Urban challenge! Rooftop jumping, crane climbing, billboard hopping, subway sections, traffic dodging, skyscraper scaling, neon night mode!",
                "🌌 GALAXY OBBY: Cosmic journey! Wormhole teleports, nebula mazes, comet surfing, black hole gravity, planet hopping, star collecting, UFO encounters, alien technology!",
                "🦖 DINOSAUR OBBY: Prehistoric adventure! T-Rex chase sections, pterodactyl flying, tar pit avoiding, volcano eruptions, caveman checkpoints, fossil collecting, time portal finale!"
            ],
            
            simulator: [
                "🐾 MEGA PET SIMULATOR: 100+ pets common to mythical! 3-stage evolution, custom homes with decorations, mini-games (fetch/agility/races), breeding for rare combos, pet abilities, daily care, trading system, pet accessories shop!",
                "🍕 PIZZA EMPIRE: Build restaurant empire! Start with small stand, 50+ toppings, hire chefs/drivers/cashiers, upgrade ovens, multiple locations, custom pizza creator, delivery mini-game, pizza contests, food truck expansion!",
                "⚔️ SWORD MASTER: 200+ legendary swords! Train stats (strength/speed/defense), battle training dummies, quest system, dungeon raids, forge swords from materials, enchantment system, PvP arena, armor sets, special combos!",
                "🏝️ ISLAND BUILDER: Start tiny, expand island! Plant crops (wheat/corn/fruits/palms), build houses/shops/parks/beaches, attract tourists, unlock new islands (tropical/volcanic/arctic), fishing, treasure hunting, hire workers!",
                "🧙 MAGIC ACADEMY: 50+ spells across 5 schools! Attend classes (Potions/Charms/Defense), collect wands with powers, familiar pets boost magic, homework quests, wizard duels, spell combinations, house competitions!",
                "💎 MINING TYCOON: Dig for riches! 50+ gem types (ruby/diamond/emerald/mythical), upgrade pickaxe/drill/dynamite, hire miners for auto-mining, craft jewelry, unlock locations (volcano/ice cave/crystal cavern), prestige system!",
                "🏋️ GYM SIMULATOR: Get super strong! Train in gym (weights/treadmill/boxing), level up strength/speed/endurance, unlock exercises/equipment, compete in competitions, hire trainer, healthy food shop, unlock gyms in cities!",
                "🎨 ART STUDIO: Become master artist! Paint on canvas with 20+ colors, sell artwork, unlock styles/tools, gallery for displays, art contests, commission system, upgrade studio with easels/lighting!",
                "☕ CAFE SIMULATOR: Run coffee shop! Make espresso/cappuccino/lattes, serve pastries, hire baristas, upgrade machines, morning rush challenges, loyal customer system, expand to chain, latte art competitions!",
                "🍦 ICE CREAM SHOP: Scoop paradise! 50+ flavors to unlock, create sundaes/cones/shakes, serve customers, unlock toppings, ice cream truck, compete in contests, seasonal flavors, decorate shop!",
                "🎮 GAME DEV SIMULATOR: Build game empire! Create games, hire programmers/artists/designers, publish games, earn from players, unlock game engines, attend conventions, win awards, grow studio!",
                "🚗 CAR WASH TYCOON: Cleaning empire! Wash/wax/detail cars, upgrade equipment, hire workers, expand locations, unlock vehicle types (cars/trucks/buses/planes!), VIP service, car show events!",
                "🏥 HOSPITAL SIMULATOR: Run medical center! Hire doctors/nurses, treat patients, upgrade equipment, research cures, expand wings, emergency room chaos, ambulance service, save lives!",
                "🎸 MUSIC STUDIO: Rock star life! Learn instruments, write songs, record albums, perform concerts, hire band members, upgrade studio, tour world, win Grammys, fan club system!",
                "🌾 FARM SIMULATOR: Country life! Plant crops, raise animals (cows/chickens/pigs), sell produce, upgrade equipment, expand farm, seasonal changes, farmers market, harvest festivals!",
                "🎬 MOVIE STUDIO: Hollywood mogul! Direct films, hire actors, build sets, special effects, release movies, box office tracking, win Oscars, franchise building, theme park attractions!",
                "🏪 SUPERMARKET TYCOON: Retail empire! Stock shelves, hire cashiers, expand store, multiple departments (produce/bakery/deli), manage inventory, promotions, loyalty cards, delivery service!",
                "🦸 SUPERHERO ACADEMY: Train heroes! Unlock powers (flight/strength/speed/invisibility), save city from villains, hero costume creator, team up missions, upgrade powers, hero rankings!",
                "🍔 FOOD TRUCK EMPIRE: Mobile restaurant! Multiple food trucks (tacos/burgers/sushi), travel to events, unlock recipes, hire drivers, upgrade trucks, compete in food festivals!",
                "🎪 CARNIVAL TYCOON: Build fairground! Set up rides, game booths, food stands, hire staff, prizes management, seasonal events, fireworks shows, expand to traveling circus!"
            ],
            
            adventure: [
                "🗺️ TREASURE ISLAND QUEST: Ultimate hunt! Massive island with 20+ locations, find map pieces, solve riddles/puzzles, explore caves/temples, ancient traps (arrows/boulders), boss Guardian, hidden rooms, map reveals as you explore!",
                "🏰 KINGDOM QUEST RPG: Save the realm! Choose Knight/Wizard/Archer/Rogue, 30+ quests from NPCs, battle system with skills, explore forests/caves/villages/castles, collect gear, level 1-50, final boss Dark Sorcerer!",
                "🌲 ENCHANTED FOREST: Magical journey! Meet fairies/unicorns/talking trees, 15 forest zones, gather herbs/berries, craft potions/spells, animal companions, build treehouse hideout, seasonal events, restore forest magic!",
                "🏴‍☠️ PIRATE ADVENTURE: Sail the seas! Captain your ship, visit 10 islands, treasure maps to buried gold, naval battles with pirates, recruit crew, upgrade ship (cannons/sails), sea monsters, trade ports!",
                "🏔️ MOUNTAIN EXPEDITION: Climb the peak! Multi-day journey, survival mechanics (warmth/energy/hunger), scenic viewpoints, weather challenges (storms/avalanches), wildlife encounters, upgrade gear, photograph sights!",
                "🌋 VOLCANO ESCAPE: Island erupting! Build raft to escape, gather supplies as lava flows, evacuate with friends co-op, rescue animals, hot zones to avoid, earthquake events, epic escape finale!",
                "🏺 ANCIENT TEMPLE: Explore ruins! Hieroglyph puzzles, trap rooms (spikes/arrows/rolling stones), treasure chambers, torch lighting in darkness, multiple endings based on choices, decode ancient story!",
                "🎭 MYSTERY MANSION: Detective work! 30-room haunted house, collect clues/evidence, interview ghost NPCs, puzzle rooms with riddles, secret passages behind bookcases, piece together mystery, multiple suspects!",
                "🏛️ ROMAN EMPIRE: Historical adventure! Become gladiator/senator/merchant, explore ancient Rome (Colosseum/Forum/Pantheon), chariot races, political intrigue, build reputation, multiple storylines!",
                "🦖 DINOSAUR ISLAND: Prehistoric survival! Explore island with dinosaurs, tame dinos as mounts, gather resources, build camps, discover fossils, escape T-Rex chases, solve extinction mystery!",
                "🌊 ATLANTIS QUEST: Lost city underwater! Dive deep, discover ruins, unlock ancient technology, befriend merfolk, solve puzzles to raise Atlantis, collect artifacts, sea creature battles!",
                "🏜️ DESERT CARAVAN: Cross the dunes! Lead caravan across vast desert, manage supplies (water/food), sandstorms, bandit attacks, discover oases, ancient ruins, reach legendary city!",
                "❄️ ARCTIC EXPEDITION: North Pole adventure! Dog sled racing, igloo building, aurora borealis viewing, polar bear encounters (friendly!), ice fishing, discover lost research station!",
                "🌴 TROPICAL PARADISE: Island hopping! Visit 20 islands each unique, solve island-specific mysteries, build bridges between islands, volcano island, haunted island, treasure island!",
                "🎪 TIME TRAVELER: Through history! Start present day, travel to Ancient Egypt/Medieval times/Wild West/Future, complete missions in each era, collect historical artifacts, prevent time paradoxes!"
            ],
            
            racing: [
                "🏎️ TURBO KART RACING: Ultimate championship! 20 tracks (city/beach/volcano/space/jungle/underwater), 50+ karts, power-ups (rockets/shields/boost/oil/lightning), customize paint/decals/wheels, championship mode, time trials, multiplayer!",
                "🛹 SKATE PARK PRO: Extreme skating! Massive park with 10 sections, trick system (kickflip/ollie/grind/manual), combo multiplier, S-K-A-T-E letters, create custom parks, 30+ boards, sponsored challenges!",
                "🏁 DRAG RACING: Quarter-mile mayhem! Perfect launch timing, gear shift mechanics, nitrous boost, tune cars (engine/tires/transmission), 40+ cars, underground story, pink slip races, custom wraps!",
                "🚁 HELICOPTER RACING: Sky competition! Choppers/planes/jets, ring checkpoint courses through clouds, barrel rolls/loops, weather challenges, 15 aircraft, canyon racing, stunt mode!",
                "🏇 HORSE RACING: Derby excitement! Train horses, 10 breeds, jump obstacles, betting system, breeding mechanics, jockey outfits, famous tracks, bonding affects performance!",
                "🚤 BOAT RACING: Water speed! Jet skis/speedboats/yachts, ocean/river/lake tracks, wave physics affects handling, trick jumps off ramps, marine obstacles, underwater tunnels!",
                "🏃 PARKOUR RACE: Free-running! Wall runs, precision jumps, vaults, city rooftops, time attack mode, multiplayer racing, unlock movement abilities, style points, create custom courses!",
                "🚂 TRAIN RACING: Railroad madness! Control locomotives, switch tracks for shortcuts, coal management (speed vs fuel), 10 historical trains, mountain passes/city routes, weather affects tracks!",
                "🦖 DINOSAUR RACING: Prehistoric speed! Race different dinosaurs (T-Rex/Raptor/Pterodactyl/Triceratops), each has unique abilities, prehistoric tracks through jungles/volcanoes, dino customization!",
                "🎢 ROLLER COASTER RACING: Thrill ride competition! Race on custom coaster tracks, loops/corkscrews/drops, speed management, multiplayer, build your own tracks, theme park settings!",
                "🚀 ROCKET RACING: Space speed! Fly rockets through space, planetary rings, asteroid fields, wormhole shortcuts, upgrade rockets, zero-gravity sections, cosmic tracks!",
                "🏍️ MOTORCYCLE RACING: Bike championship! Dirt bikes/sport bikes/choppers, motocross tracks, highway racing, stunts and wheelies, customize bikes, gang territory races!",
                "🎿 DOWNHILL SKIING: Alpine racing! Ski down mountains, slalom gates, avoid obstacles, speed sections, trick jumps, different mountains (Alps/Rockies/Japan), weather conditions!",
                "🛼 ROLLER DERBY: Rink racing! Roller skate around track, bump opponents, power-ups, team mode, trick systems, customize skates/outfits, different rink types!",
                "🏐 BEACH RACING: Coastal speed! Race on beaches, jet ski sections, sandcastle obstacles, boardwalk areas, surfboard segments, volleyball courts, tiki bars!"
            ],
            
            tycoon: [
                "🏪 MEGA MALL TYCOON: Shopping empire! Start 1 shop expand to 50+, different stores (clothing/food/electronics/toys/sports), hire employees/security, parking with valet, food court, theater/arcade, seasonal events!",
                "🎢 THEME PARK EMPIRE: Amusement paradise! 30+ rides, custom roller coaster builder, food stands/games, performers/mascots, queue management, cleanliness matters, fireworks shows, seasonal themes!",
                "🏗️ CITY BUILDER: Metropolis maker! Village to mega city, zone residential/commercial/industrial, infrastructure (roads/power/water), 100+ buildings, budget/taxes, public services, transportation!",
                "🍔 RESTAURANT CHAIN: Food empire! Multiple chains, menu customization, drive-thru/dine-in, hire/train staff, marketing campaigns, compete with rivals, expand to new cities!",
                "🏨 RESORT TYCOON: Luxury paradise! Beachfront resort, room types (standard to presidential), amenities (pool/spa/restaurant/bar), events (weddings/conferences), guest reviews affect reputation!",
                "🎮 ARCADE EMPIRE: Retro gaming! 50+ arcade cabinets, ticket prizes, claw machines, VR section, snack bar, tournaments, maintain/repair machines, nostalgic themes!",
                "🏭 FACTORY TYCOON: Industrial production! Build production lines, raw materials to products, conveyor automation, hire workers, research technologies, fulfil orders, expand factory!",
                "🏪 GAS STATION EMPIRE: Fuel business! Pumps, convenience store, car wash, repairs, hire attendants, expand locations, loyalty programs, truck stop services!",
                "🏥 HOSPITAL EMPIRE: Medical network! Build hospitals, hire doctors/nurses, research cures, emergency rooms, upgrade equipment, ambulances, expand departments!",
                "🎬 MOVIE THEATER CHAIN: Cinema empire! Multiple theaters, different movie types, concessions, comfortable seats, IMAX screens, VIP lounges, premiere events!",
                "🏊 WATER PARK TYCOON: Splash paradise! Water slides, wave pools, lazy rivers, splash pads, lifeguard stations, cabanas, food stands, seasonal operations!",
                "🎳 BOWLING ALLEY: Strike business! Multiple lanes, arcade section, food/drinks, cosmic bowling nights, leagues, tournaments, pro shop, party rooms!",
                "🏋️ GYM CHAIN: Fitness empire! Equipment variety, personal trainers, classes (yoga/spin/kickboxing), juice bar, locker rooms, multiple locations, membership tiers!",
                "🏡 REAL ESTATE MOGUL: Property empire! Buy/sell/rent houses, renovate properties, property management, market fluctuations, commercial properties, become billionaire!",
                "🚗 CAR DEALERSHIP: Auto sales! New/used cars, test drives, financing, trade-ins, service department, expand brands, luxury section, custom orders!"
            ],
            
            fighting: [
                "🥊 SUPERHERO BATTLE ARENA: Comic combat! Create heroes with superpowers (flight/strength/laser eyes/invisibility), punch/kick/special moves, different arenas (city/volcano/space), unlock costumes/abilities, hero rankings!",
                "⚔️ MEDIEVAL COMBAT: Knights duel! Swords/shields/axes/maces, blocking system, different weapon types, armor upgrades (leather/chainmail/plate), tournament mode, jousting, castle sieges!",
                "🤖 ROBOT BATTLE: Mech warfare! Build battle robots, customize parts/weapons/colors (lasers/missiles/saws), arena with hazards, weight classes, tournament mode, team battles!",
                "🥋 NINJA DOJO: Shinobi combat! Learn ninja moves, stealth kills, throwing stars/kunai, katana fighting, wall-running, smoke bombs, shadow clones, ninja rankings!",
                "🦖 DINOSAUR BATTLE: Prehistoric combat! Control dinosaurs (T-Rex strong/Raptor fast/Pterodactyl flies/Stego tank), unique abilities, prehistoric arenas, dino customization, evolution system!",
                "🧙 WIZARD DUEL: Magic combat! Cast spells (fireball/ice blast/lightning/shield), wand dueling, potion throwing, familiar pets help fight, arena hazards, wizard tournaments!",
                "🦸 ANIME FIGHTER: Manga-style combat! Anime characters with special abilities, energy attacks, transformation modes, aerial combos, story mode, unlock characters, ranked matches!",
                "👽 ALIEN INVASION: Sci-fi combat! Humans vs Aliens, different alien species with unique abilities, futuristic weapons (plasma/laser/ray guns), space station battles!",
                "🐉 DRAGON RIDERS: Aerial combat! Ride dragons into battle, dragon abilities (fire breath/ice breath/lightning), dragon customization, sky battles, ground combat mode!",
                "🎭 SUPERHERO VS VILLAIN: Good vs Evil! Choose hero or villain side, iconic powers, team battles, destructible environments, city battles, unlock legendary characters!",
                "🥊 BOXING CHAMPIONSHIP: Prize fights! Create boxer, train stats, learn combos/special punches, dodge/block/counter, career mode, championship belts, custom gloves!",
                "🗡️ SAMURAI SHOWDOWN: Honor combat! Samurai sword fighting, stance system (aggressive/defensive/balanced), honor points, dojo battles, feudal Japan setting!",
                "👊 STREET FIGHTER: Urban brawl! Street fighting styles (boxing/kickboxing/wrestling/MMA), underground tournaments, rival gangs, reputation system, unlock moves!",
                "🦁 ANIMAL ARENA: Beast battles! Control different animals (lion/bear/gorilla/wolf/eagle), unique animal abilities, natural environments, food chain mechanics!",
                "⚡ ELEMENTAL WARRIORS: Power combat! Control elements (fire/water/earth/air/lightning), element combinations, environmental advantages, elemental transformations!"
            ],
            
            survival: [
                "🏝️ STRANDED ISLAND: Ultimate survival! Start with nothing, gather resources (wood/stone/fruit), craft tools/weapons, build shelter from storms, hunt/fish, fresh water management, explore for secrets, signal fire!",
                "❄️ ARCTIC SURVIVAL: Frozen wilderness! Temperature critical, build igloo shelters, ice fishing, craft warm clothing, blizzard survival, polar bear encounters, northern lights navigation, limited daylight!",
                "🌋 VOLCANO ISLAND ESCAPE: Eruption! Island volcano erupting, build raft to escape, gather supplies as lava flows, co-op evacuation, rescue animals, hot zones, earthquakes, ash clouds!",
                "🌵 DESERT SURVIVAL: Extreme heat! Find oasis for water, build shade shelters, cacti resources, sandstorms, scorpion/snake dangers, camel companion, mirages, night freezing cold!",
                "🌲 WILDERNESS CAMPING: Forest survival! Set up camp, gather firewood, cook on campfire, pitch tent before dark, wildlife encounters, hiking for resources, stream water, storm prep!",
                "🏚️ ABANDONED CITY: Urban survival! Scavenge buildings for supplies, avoid hazards, build safe zone base, limited resources, help other survivors, grow food in gardens, defend from threats!",
                "🌊 OCEAN RAFT: Lost at sea! Expand raft with debris, catch fish, collect rainwater, sharks circle raft, island hopping, craft equipment, weather storms, navigate by stars!",
                "🏔️ MOUNTAIN SURVIVAL: Alpine challenge! Survive mountain conditions, altitude sickness, climbing gear needed, avalanche danger, find shelter in caves, hunt mountain goats, reach peak!",
                "🦖 DINOSAUR SURVIVAL: Jurassic danger! Survive with dinosaurs, build secure base, tame friendly dinos, avoid predators (T-Rex/Raptors), gather resources, craft weapons, discover escape!",
                "👽 ALIEN PLANET: Extraterrestrial survival! Crash land on alien world, strange creatures, unusual resources, toxic atmosphere (need suit), alien plants, build escape ship!",
                "🧟 ZOMBIE APOCALYPSE: Undead survival! Survive zombie hordes, fortify base, craft weapons, find survivors, limited ammo, food/water scarcity, rescue missions, find cure!",
                "🌪️ STORM CHASER: Disaster survival! Survive natural disasters (tornadoes/hurricanes/earthquakes), emergency shelter, rescue others, weather prediction, supply management!",
                "🏕️ CAMPING TRIP GONE WRONG: Nature survival! Lost in woods, find way back, build emergency shelter, avoid predators, forage for food, signal for rescue, night dangers!",
                "☢️ NUCLEAR WASTELAND: Post-apocalypse! Survive radiation zones, wear protective gear, scavenge bunkers, mutant creatures, contaminated water, find underground shelter, rebuild!",
                "🌑 MOON BASE: Space survival! Survive on moon base, oxygen management, grow food hydroponically, repair equipment, meteor showers, explore craters, return to Earth!"
            ],
            
            puzzle: [
                "🧩 PUZZLE PALACE: Brain challenge castle! 100+ puzzles, different types (jigsaw/logic/pattern/memory), unlock rooms, increasing difficulty, time trials, leaderboards!",
                "🔍 MYSTERY DETECTIVE: Solve cases! Find clues, interrogate suspects, puzzle mini-games, crime scene investigation, connect evidence, multiple cases, detective rank!",
                "🎯 ESCAPE ROOM: Ultimate escapes! 20+ themed rooms, find keys/codes, solve riddles, use items together, hidden objects, time pressure, co-op mode!",
                "🔢 NUMBER QUEST: Math adventure! Solve number puzzles to progress, different operations (+/-/×/÷), pattern recognition, unlock math powers, boss battles using math!",
                "🎨 COLOR MIXING LAB: Science puzzles! Mix colors to solve puzzles, learn color theory, create specific shades, timed challenges, unlock new colors, art science!",
                "🧠 BRAIN TEASER PARK: Mind games! Each ride is puzzle (memory/pattern/logic/word), level up intelligence, unlock new attractions, compete with friends!",
                "🔐 LOCKSMITH: Master locks! Pick locks with mini-games, different lock types (combination/key/electronic), break into safes, heist missions, unlock legendary vaults!",
                "🎲 MAZE MASTER: Labyrinth challenge! Navigate complex mazes, moving walls, trap avoidance, time limits, collect keys, unlock harder mazes, 3D mazes!",
                "💡 INVENTION LAB: Engineering puzzles! Build machines to solve problems, physics-based, chain reactions, Rube Goldberg machines, unlock parts!",
                "🗺️ MAP PUZZLES: Geography challenge! Piece together maps, learn countries/capitals, geographic features, cultural facts, world tour, unlock continents!"
            ],
            
            roleplay: [
                "🏡 NEIGHBORHOOD LIFE: Suburban roleplay! Own customizable house, 20+ furniture items, adopt pets, jobs (teacher/doctor/chef/artist), neighborhood events (BBQ/garage sales), drive cars, visit friends!",
                "🎒 ULTIMATE SCHOOL: Complete experience! Student or teacher role, 10 classrooms, take classes (Math/Science/Art/PE), cafeteria, playground, lockers, school events (dances/sports day), clubs!",
                "🏥 MEDICAL CENTER: Hospital roleplay! Roles (doctor/nurse/patient/surgeon), departments (ER/surgery/pediatrics), medical tools, patient care mini-games, ambulance service, pharmacy/lab!",
                "🌆 CITY LIFE: Urban living! Apartments/houses, 20+ job options, shopping mall, restaurants/cafes, public transport (bus/subway/taxi), parks, police/firefighter roles, city events!",
                "🏖️ BEACH RESORT: Tropical paradise! Beach houses/hotels, water activities (swimming/surfing/diving), beach cafe jobs, volleyball/sandcastles, boat rentals, sunset parties, seashell collecting!",
                "🏰 ROYAL CASTLE: Medieval fantasy! Roles (king/queen/knight/wizard/peasant), throne room, jousting tournaments, royal feasts, castle defense, medieval jobs, coronation ceremonies!",
                "🚀 SPACE STATION: Futuristic roleplay! Astronaut/scientist/engineer roles, zero gravity sections, space missions/repairs, alien encounters, research labs, spaceship hangars, galactic travel!",
                "🎪 CIRCUS LIFE: Big top roleplay! Roles (ringmaster/acrobat/clown/magician), performance shows, practice/train skills, animal care, costume customization, travel to cities!",
                "🏙️ OFFICE BUILDING: Corporate life! Work in skyscraper, different jobs (CEO/manager/employee), meetings, deadlines, promotions, office politics, lunch breaks, coffee breaks!",
                "🏕️ SUMMER CAMP: Outdoor adventure! Camper or counselor, cabins, campfire activities, crafts, hiking, canoeing, talent shows, camp games, make friends!",
                "🏛️ MUSEUM: Educational roleplay! Work as guide/curator/security, different exhibits (dinosaurs/art/space/history), school field trips, special events, artifact restoration!",
                "✈️ AIRPORT: Travel roleplay! Pilot/flight attendant/passenger/security, check-in process, board planes, in-flight service, different destinations, luggage handling!",
                "🏪 RETAIL LIFE: Store roleplay! Work as cashier/manager/stocker, help customers, restock shelves, handle complaints, inventory, sales events, employee of month!",
                "🎬 MOVIE SET: Film roleplay! Actor/director/crew/producer, film scenes, costume/makeup, script reading, premiere events, paparazzi, awards shows!",
                "🎵 MUSIC VENUE: Concert life! Musician/roadie/security/fan, perform shows, backstage access, meet and greets, tour bus, record studio, fan club!"
            ],
            
            horror: [
                "👻 FRIENDLY GHOST MANSION: Light spooky! Explore mansion with silly pranking ghosts, find hidden candies, light puzzles, more funny than scary, cartoon ghosts!",
                "🌙 MOONLIGHT MYSTERY: Nighttime adventure! Forest with glowing creatures, mysterious but not scary, find magical fireflies, discover secrets, peaceful exploration!",
                "🎃 PUMPKIN PATCH: Halloween fun! Magical pumpkin patch, friendly scarecrows, collect glowing pumpkins, solve easy riddles, autumn vibes, not scary!",
                "🦇 BATTY'S MANSION: Cute bat tour! Friendly bat Batty gives mansion tour, dark but fun, silly surprises, treasure hunting, adorable spooky!",
                "🕷️ SPIDER WEB MAZE: Friendly spider! Giant web maze, friendly spider helps you, glowing markers, fun music, collect web coins!",
                "🏚️ OLD LIBRARY: Mysterious books! Explore ancient library, friendly book characters, solve literary puzzles, magical stories come alive, enchanted but safe!",
                "🌫️ FOG FOREST: Misty adventure! Navigate foggy forest, mysterious sounds but friendly creatures, lantern lighting, discover hidden paths, atmospheric but safe!",
                "🎭 THEATER MYSTERY: Phantom story! Old theater with friendly phantom, solve theater mysteries, perform plays, backstage secrets, dramatic but fun!",
                "🏰 CASTLE AT NIGHT: Moonlit castle! Explore castle at night, friendly night guards, owl companions, stargazing, mysterious but safe, treasure hunt!",
                "👁️ MYSTERIOUS MUSEUM: After hours! Museum at night, exhibits come alive (friendly!), solve museum mysteries, historical characters talk, educational and mysterious!"
            ]
        };

        // Knowledge base (same as before)
        var knowledge = {
            "sky blue": "🌤️ The sky is blue because of Rayleigh scattering! Sunlight hits air molecules and blue light scatters more than other colors.",
            "planes fly": "✈️ Planes fly because of lift! Wings are shaped so air moves faster over the top, creating lower pressure that lifts the plane!",
            "gravity": "🌍 Gravity is a force that pulls objects together! Earth's gravity keeps us on the ground and makes things fall.",
            "fastest animal": "🐆 Land: Cheetah 70mph! Air: Peregrine falcon 240+mph! Water: Sailfish 68mph!",
            "biggest animal": "🐋 Blue whale - biggest ever! Up to 100 feet long, 200 tons!",
            "pandas eat": "🐼 Pandas eat bamboo - 26-84 pounds daily! They spend 12-16 hours eating!",
            "planets": "🪐 8 planets: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune!",
            "black hole": "⚫ Black hole is where gravity is SO strong nothing escapes - not even light!",
            "photosynthesis": "🌱 Plants make food from sunlight! They use sunlight, water, CO2 to create sugar and oxygen!",
            "dinosaurs": "🦖 Dinosaurs went extinct 65 million years ago from asteroid impact!",
            "moon": "🌙 Moon phases show how much sunlight we see reflected! Takes 29.5 days for full cycle."
        };

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲",
            "What do you call a fake noodle? An impasta! 🍝",
            "What did the ocean say to the beach? Nothing, it just waved! 🌊"
        ];

        var facts = [
            "🦈 Sharks are older than trees! 400 million years vs 350 million!",
            "🍯 Honey never spoils! 3,000-year-old honey is still edible!",
            "🐙 Octopuses have 3 hearts and blue blood!",
            "⚡ Lightning is 5 times hotter than the sun's surface!"
        ];

        addBot("👋 Hey! I'm your Roblox AI Helper!\n\n🎮 I have 100+ ROBLOX GAME IDEAS!\n\nCategories:\n• Obby (15 ideas)\n• Simulator (20 ideas)\n• Adventure (15 ideas)\n• Racing (15 ideas)\n• Tycoon (15 ideas)\n• Fighting (15 ideas)\n• Survival (15 ideas)\n• Puzzle (10 ideas)\n• Roleplay (15 ideas)\n• Horror (10 ideas)\n\nJust ask for any category!");

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
                response = "👋 Hey! What kind of game idea do you want?";
            }

            if (!response && lower.includes('thank')) {
                response = "😊 You're welcome! Want another game idea?";
            }

            if (!response && (lower.includes('joke') || lower.includes('funny'))) {
                response = random(jokes);
            }

            if (!response && lower.includes('fact')) {
                response = random(facts);
            }

            // GAME IDEAS - Check specific categories
            if (!response && lower.includes('obby')) {
                response = random(games.obby);
            }
            if (!response && lower.includes('simulator')) {
                response = random(games.simulator);
            }
            if (!response && lower.includes('adventure')) {
                response = random(games.adventure);
            }
            if (!response && (lower.includes('racing') || lower.includes('race') || lower.includes('car'))) {
                response = random(games.racing);
            }
            if (!response && (lower.includes('tycoon') || lower.includes('business'))) {
                response = random(games.tycoon);
            }
            if (!response && (lower.includes('fighting') || lower.includes('battle'))) {
                response = random(games.fighting);
            }
            if (!response && lower.includes('survival')) {
                response = random(games.survival);
            }
            if (!response && lower.includes('puzzle')) {
                response = random(games.puzzle);
            }
            if (!response && lower.includes('roleplay')) {
                response = random(games.roleplay);
            }
            if (!response && (lower.includes('horror') || lower.includes('scary'))) {
                response = random(games.horror);
            }

            // General game idea
            if (!response && (lower.includes('game') || lower.includes('roblox') || lower.includes('idea'))) {
                var allGames = [].concat(
                    games.obby, games.simulator, games.adventure, games.racing, 
                    games.tycoon, games.fighting, games.survival, games.puzzle, 
                    games.roleplay, games.horror
                );
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
                response = "🎮 I have 100+ game ideas!\n\nAsk for:\n• Obby ideas\n• Simulator ideas\n• Adventure ideas\n• Racing ideas\n• Tycoon ideas\n• Fighting ideas\n• Survival ideas\n• Puzzle ideas\n• Roleplay ideas\n• Horror ideas\n\nOr ask science questions!";
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
            
