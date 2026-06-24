import random

# Global list of items allowed for attacking
VALID_WEAPONS = ["Iron Sword", "Iron Dagger", "Steel Axe", "Hunting Bow", "Iron Spear"]

while True:
    print("\n===============================")
    print("           FIRST KING          ")
    print("===============================")
    print(" Explore the 7 Great Kingdoms: ")
    print(" 1. Blackrun (The Iron Bastion)")
    print("===============================")
    print("Play (New Game)")
    print("Credits")
    print("Exit")

    menu = input("Select an option: ")

    # --- INITIALIZE DEFAULT PLAYER DATA ---
    player_hp = 100
    player_mana = 100  
    
    magic_level = 15
    destruction_level = 15  
    guerison_level = 15     
    one_handed_level = 15
    two_handed_level = 15
    archery_level = 15
    pickpocket_level = 15  
    lockpick_level = 15    
    
    toughness = 1
    vigor = 1
    is_poisoned = False
    potions_count = 0
    mana_potions_count = 2  
    player_gold = 0
    lockpicks_count = 5    
    
    inventory_weapons = []
    equipped_armor = {"shield": 0, "chest": 0, "helmet": 0, "gloves": 0, "boots": 0}
    defense = 0  
    action = "0"

    if menu == "Play" or menu.strip().lower() == "play":
        name = input("Enter your name: ")
        print(f"Welcome, {name}!")
        game_over = False
        
        print("\n...")
        print("Everything goes dark...")
        print("\nYou slowly open your eyes. The air is cold and damp.")
        print(f"You realize you, {name}, are locked in a dimly lit prison cell in Blackrun.")
        print("Heavy footsteps echo down the corridor.")
        print("Two armored guards open your cell door and grab you by the arms.")
        print("'Move, prisoner,' one growls. 'It's time.'")
        print("With no choice, you follow the guards through the dark stone hallways...")
        print("Until you finally arrive at the execution room.")
        
        print("\nThe executioner raises his axe. The crowd falls silent.")
        print("\n[!] SUDDENLY, A DEAFENING ROAR RIPS THROUGH THE AIR!")
        print("The ground shakes violently! A massive dragon crashes through the roof, crushing the executioner.")
        print("Chaos erupts. The guards scream and run away. The building is collapsing around you!")
        
        print("\nWhat do you do?")
        print("1. Run out the smashed front door into the city streets (The Grot).")
        print("2. Dive into a heavy iron trapdoor leading down into the darkness (The Dungeon).")
        
        action = input("Choose 1 or 2: ")
        
        # ==========================================================
        # --- PATH 1: THE GROT (CRAB INFESTATION) ---
        # ==========================================================
        if action == "1":
            print("\nYou sprint for the front door, but a blast of dragon fire blocks your path!")
            print("Forced to turn back, you find a hidden tunnel that leads deep into a rocky Grot.")
            print("Inside the Grot, you stumble upon the fallen body of a royal troop.")
            
            loot_prompt = input("\nDo you want to loot the body of the royal troop? (yes/no): ").lower()
            if loot_prompt in ["yes", "y"]:
                body_items = ["Iron Sword", "Steel Shield", "Iron Dagger", "Heal Potion", "Blackrun Armor", "Blackrun Helmet", "Blackrun Gloves", "Blackrun Boots"]
                while True:
                    print("\n--- [LOOTING BODY] ---")
                    if not body_items:
                        print("[ The body is completely empty. ]")
                        break
                    for idx, item in enumerate(body_items, 1):
                        print(f"{idx}. Take {item}")
                    print("0. Done Looting (Exit Container)")
                    
                    loot_choice = input("Select an item number to take: ")
                    if loot_choice == "0": break
                    
                    if loot_choice.isdigit() and 1 <= int(loot_choice) <= len(body_items):
                        selected_item = body_items.pop(int(loot_choice) - 1)
                        print(f"[+] You took: {selected_item}")
                        
                        if selected_item == "Heal Potion": potions_count += 1
                        elif selected_item in VALID_WEAPONS: inventory_weapons.append(selected_item)
                        elif selected_item == "Steel Shield" and equipped_armor["shield"] < 1:
                            equipped_armor["shield"] = 1
                            print("[🛡️] Equipped: Steel Shield (+1 Def)")
                        elif selected_item == "Blackrun Armor" and equipped_armor["chest"] < 2:
                            equipped_armor["chest"] = 2
                            print("[👕] Equipped: Blackrun Armor (+2 Def)")
                        elif selected_item == "Blackrun Helmet" and equipped_armor["helmet"] < 1:
                            equipped_armor["helmet"] = 1
                            print("[🪖] Equipped: Blackrun Helmet (+1 Def)")
                        elif selected_item == "Blackrun Gloves" and equipped_armor["gloves"] < 0.5:
                            equipped_armor["gloves"] = 0.5
                            print("[🧤] Equipped: Blackrun Gloves (+0.5 Def)")
                        elif selected_item == "Blackrun Boots" and equipped_armor["boots"] < 0.5:
                            equipped_armor["boots"] = 0.5
                            print("[🥾] Equipped: Blackrun Boots (+0.5 Def)")
                    else:
                        print("[-] Invalid choice!")

            defense = sum(equipped_armor.values())
            print(f"\nTotal Active Defense Bonus: +{defense}")
            
            print("\n--- ENEMY ENCOUNTER IN THE GROT ---")
            print("The ground clicks and scrapes. Suddenly, a massive, mutated Cave Crab bursts from the shadows!")
            enemy_hp = 30
            
            # First Fight: Cave Crab
            while enemy_hp > 0 and player_hp > 0:
                print(f"\nYour HP: {player_hp} | Cave Crab HP: {enemy_hp} | Potions: {potions_count}")
                battle_choice = input("1. Attack | 2. Magic | 3. Potion | 4. Run | 5. Check Stats: ")
                
                if battle_choice == "1":
                    if not inventory_weapons:
                        print("[-] No weapons! Fists hit for 2 damage.")
                        enemy_hp -= 2
                    else:
                        print("\n--- CHOOSE YOUR WEAPON ---")
                        for idx, w in enumerate(inventory_weapons, 1):
                            print(f"{idx}. {w}")
                        weap_sel = input("Select weapon number: ")
                        if weap_sel.isdigit() and 1 <= int(weap_sel) <= len(inventory_weapons):
                            weapon = inventory_weapons[int(weap_sel) - 1]
                            enemy_hp -= 10  
                            if weapon in ["Iron Sword", "Iron Dagger", "Iron Spear"]:
                                one_handed_level += 1
                                print(f"[+] Hit with {weapon}! One-Handed Level Up to Lv {one_handed_level}!")
                        else:
                            print("[-] Invalid selection! You fumbled your weapon attack.")
                
                elif battle_choice == "2":
                    if player_mana >= 10:
                        player_mana -= 10
                        magic_level += 1
                        print("\n--- CHOOSE A SPELL ---")
                        print("1. Fire Strike (Destruction)")
                        print("2. Fast Heal (Guérison)")
                        spell_choice = input("Select a spell: ")
                        
                        if spell_choice == "1":
                            destruction_level += 1
                            enemy_hp -= 10
                            print(f"[🔥] Fire Strike deals -10 HP! Destruction Level Up to Lv {destruction_level}!")
                        elif spell_choice == "2":
                            guerison_level += 1
                            player_hp = min(100, player_hp + 20)
                            print(f"[✨] Fast Heal restores +20 HP! Guérison Level Up to Lv {guerison_level}!")
                        else:
                            print("[-] Invalid spell choice! Mana lost.")
                    else:
                        print("[-] Out of Mana!")
                
                elif battle_choice == "3":
                    if potions_count > 0:
                        potions_count -= 1
                        player_hp = min(100, player_hp + 20)
                        print(f"[🧪] Recovered health! HP: {player_hp}")
                    else:
                        print("[-] Out of potions!")
                
                elif battle_choice == "4":
                    print("\nYou safely fled from the Crab!")
                    break
                
                elif battle_choice == "5":
                    print(f"\n--- {name}'s STATS ---")
                    print(f"Health: {player_hp}/100 | Mana: {player_mana}/100")
                    print(f"One-Handed: Lv {one_handed_level} | Two-Handed: Lv {two_handed_level} | Archery: Lv {archery_level}")
                    print(f"Destruction: Lv {destruction_level} | Guérison: Lv {guerison_level} | Magic Base: Lv {magic_level}")
                    print(f"Lockpicking: Lv {lockpick_level} | Pickpocket: Lv {pickpocket_level}")
                    print("-------------------")
                    continue

                if enemy_hp > 0 and battle_choice != "4":
                    player_hp -= 1  
                    print(f"[-] The crab snaps at you! (-1 HP)")
                if player_hp <= 0: game_over = True

            # --- CRAB REINFORCEMENTS IN THE GROT ---
            if not game_over and battle_choice != "4":
                print("\n[⚠️] Watch out! Two smaller, aggressive Shore Crabs scuttle out from the wet gravel to avenge their pack!")
                reinforcements_hp = 30  
                while reinforcements_hp > 0 and player_hp > 0:
                    print(f"\nYour HP: {player_hp} | Shore Crabs HP: {reinforcements_hp} | Potions: {potions_count}")
                    battle_choice = input("1. Attack | 2. Magic | 3. Potion | 5. Check Stats: ")
                    
                    if battle_choice == "1":
                        if not inventory_weapons:
                            print("[-] No weapons! Fists hit for 2 damage.")
                            reinforcements_hp -= 2
                        else:
                            print("\n--- CHOOSE YOUR WEAPON ---")
                            for idx, w in enumerate(inventory_weapons, 1):
                                print(f"{idx}. {w}")
                            weap_sel = input("Select weapon number: ")
                            if weap_sel.isdigit() and 1 <= int(weap_sel) <= len(inventory_weapons):
                                weapon = inventory_weapons[int(weap_sel) - 1]
                                reinforcements_hp -= 10
                                if weapon in ["Iron Sword", "Iron Dagger", "Iron Spear"]:
                                    one_handed_level += 1
                                    print(f"[+] Hit with {weapon}! One-Handed Level Up to Lv {one_handed_level}!")
                            else:
                                print("[-] Invalid selection! You fumbled your weapon attack.")
                                
                    elif battle_choice == "2":
                        if player_mana >= 10:
                            player_mana -= 10
                            magic_level += 1
                            print("\n--- CHOOSE A SPELL ---")
                            print("1. Fire Strike (Destruction)")
                            print("2. Fast Heal (Guérison)")
                            spell_choice = input("Select a spell: ")
                            
                            if spell_choice == "1":
                                destruction_level += 1
                                reinforcements_hp -= 10
                                print(f"[🔥] Flame blast boils the crabs! Destruction Level up to {destruction_level}!")
                            elif spell_choice == "2":
                                guerison_level += 1
                                player_hp = min(100, player_hp + 20)
                                print(f"[✨] Fast Heal restores +20 HP! Guérison Level Up to Lv {guerison_level}!")
                            else:
                                print("[-] Invalid spell choice! Mana lost.")
                        else:
                            print("[-] Out of Mana!")
                            
                    elif battle_choice == "3" and potions_count > 0:
                        potions_count -= 1
                        player_hp = min(100, player_hp + 20)
                        print("[🧪] Restored health.")
                    elif battle_choice == "5":
                        print(f"\nHealth: {player_hp}/100 | Destruction: Lv {destruction_level}")
                        continue

                    if reinforcements_hp > 0:
                        player_hp -= 1  
                        print("[-] The shore crabs pinch your ankles! (-1 HP)")
                    if player_hp <= 0: game_over = True

            # --- GROT PATH REPETITIVE LOCKPICKING LOOP ---
            if not game_over:
                print("\nNear the edge of the Grot pool, you find an old moss-covered locked chest.")
                chest_opened = False
                
                while not chest_opened and lockpicks_count > 0:
                    print(f"\nYou currently have {lockpicks_count} lockpicks (crochets) left.")
                    chest_choice = input("Do you want to try to pick the lock? (yes/no): ").lower()
                    
                    if chest_choice in ["yes", "y"]:
                        pick_attempt = random.randint(1, 100)
                        print("\n*Click, scrape, twist...*")
                        if pick_attempt <= 25:
                            lockpicks_count -= 1
                            print(f"[-] Snap! Lockpick broke.")
                        else:
                            lockpick_level += 1
                            player_gold += 150
                            chest_opened = True
                            print(f"[🔑] SUCCESS! Lockpicking Level Up to Lv {lockpick_level}! Found 150 Gold!")
                    else:
                        print("You step away from the chest.")
                        break
                
                if lockpicks_count == 0 and not chest_opened:
                    print("[-] Out of lockpicks! You cannot attempt to unlock the chest anymore.")

            if not game_over:
                print(f"\nYou successfully navigated the Grot and escaped Blackrun with {player_gold} Gold!")
                break

        # ==========================================================
        # --- PATH 2: THE DUNGEON OF BLACKRUN ---
        # ==========================================================
        elif action == "2":
            print("\nYou lift the trapdoor and dive down into the Dungeon corridors.")
            print("Resting against a brick wall is the body of a dead royal guard troop.")
            
            loot_prompt = input("\nDo you want to loot the dead guard troop? (yes/no): ").lower()
            if loot_prompt in ["yes", "y"]:
                body_items = ["Steel Axe", "Hunting Bow", "Heal Potion", "Blackrun Armor", "Blackrun Helmet", "Blackrun Gloves", "Blackrun Boots"]
                while True:
                    print("\n--- [LOOTING GUARD] ---")
                    if not body_items: 
                        print("[ The body is completely empty. ]")
                        break
                    for idx, item in enumerate(body_items, 1): print(f"{idx}. Take {item}")
                    print("0. Done Looting")
                    loot_choice = input("Select number: ")
                    if loot_choice == "0": break
                    
                    if loot_choice.isdigit() and 1 <= int(loot_choice) <= len(body_items):
                        selected_item = body_items.pop(int(loot_choice) - 1)
                        print(f"[+] Took: {selected_item}")
                        if selected_item == "Heal Potion": potions_count += 1
                        elif selected_item in VALID_WEAPONS: inventory_weapons.append(selected_item)
                        elif selected_item == "Blackrun Armor" and equipped_armor["chest"] < 2:
                            equipped_armor["chest"] = 2
                            print("[👕] Equipped: Blackrun Armor (+2 Def)")
                        elif selected_item == "Blackrun Helmet" and equipped_armor["helmet"] < 1:
                            equipped_armor["helmet"] = 1
                            print("[🪖] Equipped: Blackrun Helmet (+1 Def)")
                        elif selected_item == "Blackrun Gloves" and equipped_armor["gloves"] < 0.5:
                            equipped_armor["gloves"] = 0.5
                            print("[🧤] Equipped: Blackrun Gloves (+0.5 Def)")
                        elif selected_item == "Blackrun Boots" and equipped_armor["boots"] < 0.5:
                            equipped_armor["boots"] = 0.5
                            print("[🥾] Equipped: Blackrun Boots (+0.5 Def)")

            defense = sum(equipped_armor.values())
            print(f"\nTotal Active Defense Bonus: +{defense}")
            
            print("\nA hostile Blackrun Jailer stands near the exit gate, his back turned to you.")
            
            stole_keys = False
            steal_choice = input("Do you want to try to pickpocket the dungeon keys from his belt? (yes/no): ").lower()
            
            if steal_choice in ["yes", "y"]:
                steal_roll = random.randint(1, 100)
                if steal_roll <= 30:
                    print("[-] Caught pickpocketing! The Jailer whirls around!")
                else:
                    pickpocket_level += 1
                    stole_keys = True
                    print(f"[💰] SUCCESS! Pickpocketing Level Up to Lv {pickpocket_level}!")
            
            if stole_keys:
                print("\nYou slip past the gate and avoid combat entirely!")
                enemy_hp = 0
            else:
                enemy_hp = 30  

            while enemy_hp > 0 and player_hp > 0:
                print(f"\n--- YOUR COMBAT MENU ---")
                print(f"Your HP: {player_hp} | Jailer HP: {enemy_hp} | Potions: {potions_count}")
                battle_choice = input("1. Attack | 2. Magic | 3. Potion | 4. Run | 5. Check Stats: ")
                
                if battle_choice == "1":
                    if not inventory_weapons:
                        enemy_hp -= 2
                        print("[-] Fists deal 2 damage.")
                    else:
                        print("\n--- CHOOSE YOUR WEAPON ---")
                        for idx, w in enumerate(inventory_weapons, 1):
                            print(f"{idx}. {w}")
                        weap_sel = input("Select weapon number: ")
                        if weap_sel.isdigit() and 1 <= int(weap_sel) <= len(inventory_weapons):
                            weapon = inventory_weapons[int(weap_sel) - 1]
                            enemy_hp -= 10
                            if weapon == "Steel Axe":
                                two_handed_level += 1
                                print(f"[🪓] Hit! Two-Handed Level Up to Lv {two_handed_level}!")
                            elif weapon == "Hunting Bow":
                                archery_level += 1
                                print(f"[🏹] Hit! Archery Level Up to Lv {archery_level}!")
                        else:
                            print("[-] Invalid weapon selection!")
                
                elif battle_choice == "2":
                    if player_mana >= 10:
                        player_mana -= 10
                        magic_level += 1
                        print("\n--- CHOOSE A SPELL ---")
                        print("1. Fire Strike (Destruction)")
                        print("2. Fast Heal (Guérison)")
                        spell_choice = input("Select a spell: ")
                        
                        if spell_choice == "1":
                            destruction_level += 1
                            enemy_hp -= 10
                            print(f"[🔥] Fire Strike deals -10 HP! Destruction Level Up to Lv {destruction_level}!")
                        elif spell_choice == "2":
                            guerison_level += 1
                            player_hp = min(100, player_hp + 20)
                            print(f"[✨] Fast Heal restores +20 HP! Guérison Level Up to Lv {guerison_level}!")
                        else:
                            print("[-] Invalid spell choice! Mana lost.")
                    else:
                        print("[-] Out of Mana!")
                
                elif battle_choice == "3" and potions_count > 0:
                    potions_count -= 1
                    player_hp = min(100, player_hp + 20)
                    print("[🧪] Restored health.")
                
                elif battle_choice == "4":
                    print("\nYou fled from combat!")
                    break
                
                elif battle_choice == "5":
                    print(f"\n--- {name}'s STATS ---")
                    print(f"Health: {player_hp}/100 | Mana: {player_mana}/100")
                    print(f"One-Handed: Lv {one_handed_level} | Two-Handed: Lv {two_handed_level} | Archery: Lv {archery_level}")
                    print(f"Destruction: Lv {destruction_level} | Guérison: Lv {guerison_level} | Magic Base: Lv {magic_level}")
                    print(f"Lockpicking: Lv {lockpick_level} | Pickpocket: Lv {pickpocket_level}")
                    print("-------------------")
                    continue

                if enemy_hp > 0 and battle_choice != "4":
                    player_hp -= 1  
                    print("[-] The jailer strikes back! (-1 HP)")
                if player_hp <= 0: game_over = True

            # --- TWO ELITE GUARDS REINFORCEMENTS (Weapon and Magic choice added here) ---
            if not game_over and not stole_keys:
                print("\n[⚠️] Reinforcements! Two Elite Guards sprint around the corner with toxic spears!")
                elites_hp = 30  
                while elites_hp > 0 and player_hp > 0:
                    if is_poisoned:
                        player_hp -= 1  
                        print(f"[🤢] Poison drains -1 HP! Current HP: {player_hp}")
                        if player_hp <= 0:
                            game_over = True
                            break

                    print(f"\nYour HP: {player_hp} | Elites HP: {elites_hp} | Potions: {potions_count}")
                    battle_choice = input("1. Attack | 2. Magic | 3. Potion | 5. Check Stats: ")
                    
                    if battle_choice == "1":
                        if not inventory_weapons:
                            print("[-] No weapons! Fists hit for 2 damage.")
                            elites_hp -= 2
                        else:
                            print("\n--- CHOOSE YOUR WEAPON ---")
                            for idx, w in enumerate(inventory_weapons, 1):
                                print(f"{idx}. {w}")
                            weap_sel = input("Select weapon number: ")
                            if weap_sel.isdigit() and 1 <= int(weap_sel) <= len(inventory_weapons):
                                weapon = inventory_weapons[int(weap_sel) - 1]
                                elites_hp -= 10
                                if weapon in ["Iron Sword", "Iron Dagger", "Iron Spear"]:
                                    one_handed_level += 1
                                    print(f"[+] Hit with {weapon}! One-Handed Level Up to Lv {one_handed_level}!")
                                elif weapon == "Steel Axe":
                                    two_handed_level += 1
                                    print(f"[🪓] Hit! Two-Handed Level Up to Lv {two_handed_level}!")
                                elif weapon == "Hunting Bow":
                                    archery_level += 1
                                    print(f"[🏹] Hit! Archery Level Up to Lv {archery_level}!")
                            else:
                                print("[-] Invalid weapon selection!")
                                
                    elif battle_choice == "2":
                        if player_mana >= 10:
                            player_mana -= 10
                            magic_level += 1
                            print("\n--- CHOOSE A SPELL ---")
                            print("1. Fire Strike (Destruction)")
                            print("2. Fast Heal (Guérison)")
                            spell_choice = input("Select a spell: ")
                            
                            if spell_choice == "1":
                                destruction_level += 1
                                elites_hp -= 10
                                print(f"[🔥] Destruction Spell hits the elites! Level up to {destruction_level}!")
                            elif spell_choice == "2":
                                guerison_level += 1
                                player_hp = min(100, player_hp + 20)
                                print(f"[✨] Fast Heal restores +20 HP! Guérison Level Up to Lv {guerison_level}!")
                            else:
                                print("[-] Invalid spell choice! Mana lost.")
                        else:
                            print("[-] Out of Mana!")
                            
                    elif battle_choice == "3" and potions_count > 0:
                        potions_count -= 1
                        player_hp = min(100, player_hp + 25)
                        is_poisoned = False
                        print("[🧪] Restored health and cured poison!")
                    elif battle_choice == "5":
                        print(f"\nHealth: {player_hp}/100 | Destruction: Lv {destruction_level}")
                        continue

                    if elites_hp > 0:
                        player_hp -= 1  
                        if not is_poisoned and random.randint(1, 2) == 1:
                            is_poisoned = True
                            print("[⚠️] POISONED! The elite spear poisons you!")
                    if player_hp <= 0: game_over = True

            # --- DUNGEON PATH REPETITIVE LOCKPICKING LOOP ---
            if not game_over:
                print("\nIn the corner of the room, you spot an old, heavily locked iron chest.")
                chest_opened = False
                
                while not chest_opened and lockpicks_count > 0:
                    print(f"\nYou currently have {lockpicks_count} lockpicks (crochets) left.")
                    chest_choice = input("Do you want to try to pick the lock? (yes/no): ").lower()
                    
                    if chest_choice in ["yes", "y"]:
                        pick_attempt = random.randint(1, 100)
                        print("\n*Click, scrape, twist...*")
                        if pick_attempt <= 25:
                            lockpicks_count -= 1
                            print(f"[-] Snap! Lockpick broke.")
                        else:
                            lockpick_level += 1
                            player_gold += 150
                            chest_opened = True
                            print(f"[🔑] SUCCESS! Lockpicking Level Up to Lv {lockpick_level}! Found 150 Gold!")
                    else:
                        print("You step away from the chest.")
                        break
                
                if lockpicks_count == 0 and not chest_opened:
                    print("[-] Out of lockpicks! You cannot attempt to unlock the chest anymore.")

            if not game_over:
                print(f"\nYou exit into the outside air! You escaped Blackrun entirely with {player_gold} Gold!")
                break

        if game_over:
            print("\n☠️ YOU DIED ☠️")
            restart = input("Would you like to restart Chapter 1? (yes/no): ").lower()
            if restart != "yes": break

    elif menu == "Credits":
        print("\n--- CREDITS ---\nProgrammer: Neomatrix\nSystems: Complete Chapter 1 Final Build")
        input("\nPress Enter to return to menu...")
    elif menu == "Exit":
        print("Goodbye!")
        break
