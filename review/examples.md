# Ten examples next to their source turn

Each example: where it comes from, the raw Metamon turn it was built from, what the original Showdown replay log shows for that turn, the exact model input, and the target. The log lines keep battle actions only; `pN` marks the point-of-view player's side.

## 1. Lead (first turn)

- **Example:** `train.jsonl` line 36038
- **Battle:** `smogtours-gen1ou-708225` (smogtours, 08-06-2023), point of view `quickclaw83695` (rating Unrated, LOSS), Showdown turn 1, step 0 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2023/08/smogtours-gen1ou-708225_Unrated_quickclaw83695_vs_mareep93864_08-06-2023_LOSS.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-708225
- **Label:** Metamon index 2 = move Surf; Showdown log shows `p2`: move Surf → **agrees**

**Source turn (Metamon file):**
```text
actions[0] = 2    (next action, actions[1] = 3)
forced_switch = False
player_active_pokemon = starmie  hp_pct=1.000  status=nostatus  effect=noeffect  types='psychic water'
  moves as stored: surf (PP 24/24), thunderwave (PP 32/32), recover (PP 32/32), psychic (PP 16/16)
  Metamon move order (index 0-3): 0=psychic, 1=recover, 2=surf, 3=thunderwave
available_switches as stored: exeggutor 1.00 nostatus, snorlax 1.00 nostatus, chansey 1.00 nostatus, lapras 1.00 nostatus, tauros 1.00 nostatus
  Metamon switch order (index 4-8): 4=chansey, 5=exeggutor, 6=lapras, 7=snorlax, 8=tauros
opponent_active_pokemon = gengar  hp_pct=1.000  status=nostatus  effect=noeffect  revealed moves=[]
player_prev_move = nomove   opponent_prev_move = nomove   opponents_remaining = 6
```
**Showdown log, turn 1 (you are p2):**
```text
|turn|1
|switch|p1a: Jynx|Jynx|333/333
|move|p2a: Starmie|Surf|p1a: Jynx
|-damage|p1a: Jynx|224/333
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Starmie (Psychic/Water), HP 100%, healthy; base HP 60, Atk 75, Def 85, Spc 100, Spe 115
Opponent's active Pokemon: Gengar (Ghost/Poison), HP 100%, healthy; base HP 60, Atk 65, Def 60, Spc 130, Spe 110
Opponent's revealed moves: none yet
Last moves: you used nothing; the opponent used nothing.
Pokemon left: you 6, opponent 6.

Legal actions:
A0 = use Psychic: Psychic, power 90, accuracy 100%, PP 16/16, 2x vs Gengar
A1 = use Recover: Normal, status move, PP 32/32
A2 = use Surf: Water, power 95, accuracy 100%, PP 24/24, 1x vs Gengar
A3 = use Thunder Wave: Electric, status move, accuracy 100%, PP 32/32
A4 = switch to Chansey (Normal), HP 100%, healthy; base HP 250, Atk 5, Def 5, Spc 105, Spe 50; moves: Counter, Ice Beam, Soft-Boiled, Thunder Wave
A5 = switch to Exeggutor (Grass/Psychic), HP 100%, healthy; base HP 95, Atk 95, Def 85, Spc 125, Spe 55; moves: Explosion, Mega Drain, Psychic, Sleep Powder
A6 = switch to Lapras (Ice/Water), HP 100%, healthy; base HP 130, Atk 85, Def 80, Spc 95, Spe 60; moves: Blizzard, Body Slam, Confuse Ray, Thunderbolt
A7 = switch to Snorlax (Normal), HP 100%, healthy; base HP 160, Atk 110, Def 65, Spc 65, Spe 30; moves: Body Slam, Hyper Beam, Reflect, Rest
A8 = switch to Tauros (Normal), HP 100%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Blizzard, Body Slam, Earthquake, Hyper Beam
```
**Target (assistant):** `{"action_id":"A2"}`

## 2. Forced switch after a faint

- **Example:** `train.jsonl` line 20638
- **Battle:** `smogtours-gen1ou-924991` (smogtours, 04-11-2026), point of view `bide85657` (rating Unrated, LOSS), Showdown turn 29, step 30 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2026/04/3763-smogtours-gen1ou-924991_Unrated_bide85657_vs_encore90411_04-11-2026_LOSS.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-924991
- **Label:** Metamon index 7 = switch to Tauros; Showdown log shows `p1`: switch to Tauros → **agrees**

**Source turn (Metamon file):**
```text
actions[30] = 7    (next action, actions[31] = 6)
forced_switch = True
player_active_pokemon = snorlax  hp_pct=0.000  status=fnt  effect=noeffect  types='normal notype'
  moves as stored: bodyslam (PP 23/24), hyperbeam (PP 7/8), selfdestruct (PP 7/8), earthquake (PP 16/16)
  Metamon move order (index 0-3): 0=bodyslam, 1=earthquake, 2=hyperbeam, 3=selfdestruct
available_switches as stored: jolteon 1.00 slp, slowbro 0.80 slp, exeggutor 0.73 par, tauros 1.00 nostatus
  Metamon switch order (index 4-8): 4=exeggutor, 5=jolteon, 6=slowbro, 7=tauros
opponent_active_pokemon = starmie  hp_pct=0.000  status=fnt  effect=noeffect  revealed moves=[]
player_prev_move = selfdestruct   opponent_prev_move = nomove   opponents_remaining = 4
```
**Showdown log, turn 29 (you are p1):**
```text
|turn|29
|switch|p2a: Starmie|Starmie|100/100
|move|p1a: Snorlax|Self-Destruct|p2a: Starmie
|-damage|p2a: Starmie|0 fnt
|faint|p2a: Starmie
|faint|p1a: Snorlax
|switch|p2a: Chansey|Chansey|100/100 par
|switch|p1a: Tauros|Tauros|100/100
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your Snorlax fainted: choose which Pokemon to send in.
Opponent's active Pokemon: Starmie (Psychic/Water), HP 0%, fainted; base HP 60, Atk 75, Def 85, Spc 100, Spe 115
Opponent's revealed moves: none yet
Last moves: you used Self-Destruct; the opponent used nothing.
Pokemon left: you 4, opponent 4.

Legal actions:
A4 = switch to Exeggutor (Grass/Psychic), HP 73%, paralyzed; base HP 95, Atk 95, Def 85, Spc 125, Spe 55; moves: Explosion, Psychic, Sleep Powder, Stun Spore
A5 = switch to Jolteon (Electric), HP 100%, asleep; base HP 65, Atk 65, Def 60, Spc 110, Spe 130; moves: Double Kick, Pin Missile, Thunderbolt, Thunder Wave
A6 = switch to Slowbro (Psychic/Water), HP 80%, asleep; base HP 95, Atk 75, Def 110, Spc 80, Spe 30; moves: Amnesia, Rest, Surf, Thunder Wave
A7 = switch to Tauros (Normal), HP 100%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Blizzard, Body Slam, Earthquake, Rest
```
**Target (assistant):** `{"action_id":"A7"}`

## 3. Voluntary switch

- **Example:** `train.jsonl` line 22135
- **Battle:** `smogtours-gen1ou-772500` (smogtours, 06-06-2024), point of view `timerball45388` (rating Unrated, LOSS), Showdown turn 32, step 32 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2024/06/smogtours-gen1ou-772500_Unrated_timerball45388_vs_detect19012_06-06-2024_LOSS.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-772500
- **Label:** Metamon index 6 = switch to Gengar; Showdown log shows `p2`: switch to Gengar → **agrees**

**Source turn (Metamon file):**
```text
actions[32] = 6    (next action, actions[33] = -1)
forced_switch = False
player_active_pokemon = exeggutor  hp_pct=0.470  status=nostatus  effect=noeffect  types='grass psychic'
  moves as stored: psychic (PP 9/16), sleeppowder (PP 24/24), stunspore (PP 48/48), explosion (PP 8/8)
  Metamon move order (index 0-3): 0=explosion, 1=psychic, 2=sleeppowder, 3=stunspore
available_switches as stored: alakazam 0.69 par, gengar 0.70 slp, chansey 0.86 nostatus, tauros 1.00 nostatus
  Metamon switch order (index 4-8): 4=alakazam, 5=chansey, 6=gengar, 7=tauros
opponent_active_pokemon = exeggutor  hp_pct=0.090  status=nostatus  effect=noeffect  revealed moves=['sleeppowder']
player_prev_move = psychic   opponent_prev_move = sleeppowder   opponents_remaining = 5
```
**Showdown log, turn 32 (you are p2):**
```text
|turn|32
|switch|p2a: Gengar|Gengar|70/100 slp
|move|p1a: Exeggutor|Psychic|p2a: Gengar
|-supereffective|p2a: Gengar
|-damage|p2a: Gengar|30/100 slp
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Exeggutor (Grass/Psychic), HP 47%, healthy; base HP 95, Atk 95, Def 85, Spc 125, Spe 55
Opponent's active Pokemon: Exeggutor (Grass/Psychic), HP 9%, healthy, boosts -1 Spc; base HP 95, Atk 95, Def 85, Spc 125, Spe 55
Opponent's revealed moves: Sleep Powder
Last moves: you used Psychic; the opponent used Sleep Powder.
Pokemon left: you 5, opponent 5.

Legal actions:
A0 = use Explosion: Normal, power 170, accuracy 100%, PP 8/8, 1x vs Exeggutor
A1 = use Psychic: Psychic, power 90, accuracy 100%, PP 9/16, 0.5x vs Exeggutor
A2 = use Sleep Powder: Grass, status move, accuracy 75%, PP 24/24
A3 = use Stun Spore: Grass, status move, accuracy 75%, PP 48/48
A4 = switch to Alakazam (Psychic), HP 69%, paralyzed; base HP 55, Atk 50, Def 45, Spc 135, Spe 120; moves: Psychic, Recover, Seismic Toss, Thunder Wave
A5 = switch to Chansey (Normal), HP 86%, healthy; base HP 250, Atk 5, Def 5, Spc 105, Spe 50; moves: Ice Beam, Seismic Toss, Soft-Boiled, Thunder Wave
A6 = switch to Gengar (Ghost/Poison), HP 70%, asleep; base HP 60, Atk 65, Def 60, Spc 130, Spe 110; moves: Explosion, Hypnosis, Psychic, Thunderbolt
A7 = switch to Tauros (Normal), HP 100%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Body Slam, Earthquake, Hyper Beam, Thunderbolt
```
**Target (assistant):** `{"action_id":"A6"}`

## 4. Super-effective attack chosen

- **Example:** `train.jsonl` line 24966
- **Battle:** `gen1ou-989175740` (ladder, 10-08-2019), point of view `lickitung63255` (rating 1549, WIN), Showdown turn 33, step 32 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2019/10/gen1ou-989175740_1549_lickitung63255_vs_ampharos78981_10-08-2019_WIN.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/gen1ou-989175740
- **Label:** Metamon index 0 = move Blizzard; Showdown log shows `p1`: move Blizzard → **agrees**

**Source turn (Metamon file):**
```text
actions[32] = 0    (next action, actions[33] = 0)
forced_switch = False
player_active_pokemon = jynx  hp_pct=0.800  status=par  effect=noeffect  types='ice psychic'
  moves as stored: blizzard (PP 6/8), psychic (PP 14/16), counter (PP 32/32), lovelykiss (PP 16/16)
  Metamon move order (index 0-3): 0=blizzard, 1=counter, 2=lovelykiss, 3=psychic
available_switches as stored: chansey 0.30 par, cloyster 0.17 nostatus, starmie 0.63 nostatus, rhydon 0.39 nostatus, tauros 1.00 nostatus
  Metamon switch order (index 4-8): 4=chansey, 5=cloyster, 6=rhydon, 7=starmie, 8=tauros
opponent_active_pokemon = dragonite  hp_pct=1.000  status=nostatus  effect=noeffect  revealed moves=['bodyslam', 'surf', 'thunderbolt']
player_prev_move = blizzard   opponent_prev_move = thunderbolt   opponents_remaining = 3
```
**Showdown log, turn 33 (you are p1):**
```text
|turn|33
|move|p2a: Dragonite|Thunderbolt|p1a: Jynx
|-damage|p1a: Jynx|55/100 par
|move|p1a: Jynx|Blizzard|p2a: Dragonite
|-supereffective|p2a: Dragonite
|-damage|p2a: Dragonite|0 fnt
|faint|p2a: Dragonite
|switch|p2a: kekreisi|Jolteon|100/100
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Jynx (Ice/Psychic), HP 80%, paralyzed; base HP 65, Atk 50, Def 35, Spc 95, Spe 95
Opponent's active Pokemon: Dragonite (Dragon/Flying), HP 100%, healthy; base HP 91, Atk 134, Def 95, Spc 100, Spe 80
Opponent's revealed moves: Body Slam, Surf, Thunderbolt
Last moves: you used Blizzard; the opponent used Thunderbolt.
Pokemon left: you 6, opponent 3.

Legal actions:
A0 = use Blizzard: Ice, power 120, accuracy 90%, PP 6/8, 4x vs Dragonite
A1 = use Counter: Fighting, returns double the damage of the last Normal/Fighting hit, accuracy 100%, PP 32/32, priority -1, type matchup does not change damage
A2 = use Lovely Kiss: Normal, status move, accuracy 75%, PP 16/16
A3 = use Psychic: Psychic, power 90, accuracy 100%, PP 14/16, 1x vs Dragonite
A4 = switch to Chansey (Normal), HP 30%, paralyzed; base HP 250, Atk 5, Def 5, Spc 105, Spe 50; moves: Ice Beam, Sing, Soft-Boiled, Thunder Wave
A5 = switch to Cloyster (Ice/Water), HP 17%, healthy; base HP 50, Atk 95, Def 180, Spc 85, Spe 70; moves: Blizzard, Clamp, Explosion, Hyper Beam
A6 = switch to Rhydon (Ground/Rock), HP 39%, healthy; base HP 105, Atk 130, Def 120, Spc 45, Spe 40; moves: Body Slam, Earthquake, Rock Slide, Substitute
A7 = switch to Starmie (Psychic/Water), HP 63%, healthy; base HP 60, Atk 75, Def 85, Spc 100, Spe 115; moves: Blizzard, Recover, Thunderbolt, Thunder Wave
A8 = switch to Tauros (Normal), HP 100%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Blizzard, Body Slam, Earthquake, Hyper Beam
```
**Target (assistant):** `{"action_id":"A0"}`

## 5. Status move chosen

- **Example:** `train.jsonl` line 15849
- **Battle:** `smogtours-gen1ou-306474` (smogtours, 07-30-2017), point of view `ledyba34034` (rating Unrated, LOSS), Showdown turn 23, step 24 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2017/07/smogtours-gen1ou-306474_Unrated_ledyba34034_vs_goldberry51359_07-30-2017_LOSS.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-306474
- **Label:** Metamon index 2 = move Soft-Boiled; Showdown log shows `p2`: move Soft-Boiled → **agrees**

**Source turn (Metamon file):**
```text
actions[24] = 2    (next action, actions[25] = 2)
forced_switch = False
player_active_pokemon = chansey  hp_pct=0.563  status=nostatus  effect=noeffect  types='normal notype'
  moves as stored: thunderwave (PP 31/32), softboiled (PP 15/16), icebeam (PP 16/16), seismictoss (PP 32/32)
  Metamon move order (index 0-3): 0=icebeam, 1=seismictoss, 2=softboiled, 3=thunderwave
available_switches as stored: alakazam 0.77 slp, tauros 0.38 nostatus, persian 1.00 nostatus
  Metamon switch order (index 4-8): 4=alakazam, 5=persian, 6=tauros
opponent_active_pokemon = snorlax  hp_pct=0.725  status=par  effect=noeffect  revealed moves=['bodyslam', 'counter', 'earthquake']
player_prev_move = softboiled   opponent_prev_move = bodyslam   opponents_remaining = 4
```
**Showdown log, turn 23 (you are p2):**
```text
|turn|23
|move|p2a: Chansey|Soft-Boiled|p2a: Chansey
|-heal|p2a: Chansey|703/703
|move|p1a: Snorlax|Body Slam|p2a: Chansey
|-crit|p2a: Chansey
|-damage|p2a: Chansey|167/703
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Chansey (Normal), HP 56%, healthy; base HP 250, Atk 5, Def 5, Spc 105, Spe 50
Opponent's active Pokemon: Snorlax (Normal), HP 72%, paralyzed; base HP 160, Atk 110, Def 65, Spc 65, Spe 30
Opponent's revealed moves: Body Slam, Counter, Earthquake
Last moves: you used Soft-Boiled; the opponent used Body Slam.
Pokemon left: you 4, opponent 4.

Legal actions:
A0 = use Ice Beam: Ice, power 95, accuracy 100%, PP 16/16, 1x vs Snorlax
A1 = use Seismic Toss: Fighting, fixed damage equal to level (100), accuracy 100%, PP 32/32, type matchup does not change damage
A2 = use Soft-Boiled: Normal, status move, PP 15/16
A3 = use Thunder Wave: Electric, status move, accuracy 100%, PP 31/32
A4 = switch to Alakazam (Psychic), HP 77%, asleep; base HP 55, Atk 50, Def 45, Spc 135, Spe 120; moves: Psychic, Recover, Seismic Toss, Thunder Wave
A5 = switch to Persian (Normal), HP 100%, healthy; base HP 65, Atk 70, Def 60, Spc 65, Spe 115; moves: Bubble Beam, Hyper Beam, Slash, Thunderbolt
A6 = switch to Tauros (Normal), HP 38%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Blizzard, Body Slam, Earthquake, Hyper Beam
```
**Target (assistant):** `{"action_id":"A2"}`

## 6. Own active at low HP

- **Example:** `train.jsonl` line 11115
- **Battle:** `smogtours-gen1ou-224029` (smogtours, 12-14-2016), point of view `poliwhirl93333` (rating Unrated, WIN), Showdown turn 28, step 27 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2016/12/smogtours-gen1ou-224029_Unrated_poliwhirl93333_vs_combusken74868_12-14-2016_WIN.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-224029
- **Label:** Metamon index 0 = move Psychic; Showdown log shows `p2`: move Psychic → **agrees**

**Source turn (Metamon file):**
```text
actions[27] = 0    (next action, actions[28] = 0)
forced_switch = False
player_active_pokemon = alakazam  hp_pct=0.064  status=par  effect=noeffect  types='notype psychic'
  moves as stored: thunderwave (PP 31/32), psychic (PP 13/16), seismictoss (PP 31/32), recover (PP 31/32)
  Metamon move order (index 0-3): 0=psychic, 1=recover, 2=seismictoss, 3=thunderwave
available_switches as stored: golem 1.00 nostatus, chansey 0.89 nostatus, snorlax 1.00 nostatus, tauros 1.00 nostatus, persian 1.00 nostatus
  Metamon switch order (index 4-8): 4=chansey, 5=golem, 6=persian, 7=snorlax, 8=tauros
opponent_active_pokemon = chansey  hp_pct=1.000  status=par  effect=noeffect  revealed moves=['icebeam', 'thunderwave']
player_prev_move = psychic   opponent_prev_move = icebeam   opponents_remaining = 6
```
**Showdown log, turn 28 (you are p2):**
```text
|turn|28
|switch|p1a: Tauros|Tauros|353/353
|move|p2a: Alakazam|Psychic|p1a: Tauros
|-damage|p1a: Tauros|199/353
|-unboost|p1a: Tauros|spd|1
|-unboost|p1a: Tauros|spa|1
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Alakazam (Psychic), HP 6%, paralyzed; base HP 55, Atk 50, Def 45, Spc 135, Spe 120
Opponent's active Pokemon: Chansey (Normal), HP 100%, paralyzed; base HP 250, Atk 5, Def 5, Spc 105, Spe 50
Opponent's revealed moves: Ice Beam, Thunder Wave
Last moves: you used Psychic; the opponent used Ice Beam.
Pokemon left: you 6, opponent 6.

Legal actions:
A0 = use Psychic: Psychic, power 90, accuracy 100%, PP 13/16, 1x vs Chansey
A1 = use Recover: Normal, status move, PP 31/32
A2 = use Seismic Toss: Fighting, fixed damage equal to level (100), accuracy 100%, PP 31/32, type matchup does not change damage
A3 = use Thunder Wave: Electric, status move, accuracy 100%, PP 31/32
A4 = switch to Chansey (Normal), HP 89%, healthy; base HP 250, Atk 5, Def 5, Spc 105, Spe 50; moves: Ice Beam, Soft-Boiled, Thunderbolt, Thunder Wave
A5 = switch to Golem (Ground/Rock), HP 100%, healthy; base HP 80, Atk 110, Def 130, Spc 55, Spe 45; moves: Body Slam, Earthquake, Explosion, Rock Slide
A6 = switch to Persian (Normal), HP 100%, healthy; base HP 65, Atk 70, Def 60, Spc 65, Spe 115; moves: Bubble Beam, Hyper Beam, Slash, Thunderbolt
A7 = switch to Snorlax (Normal), HP 100%, healthy; base HP 160, Atk 110, Def 65, Spc 65, Spe 30; moves: Body Slam, Hyper Beam, Self-Destruct, Surf
A8 = switch to Tauros (Normal), HP 100%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Blizzard, Body Slam, Earthquake, Hyper Beam
```
**Target (assistant):** `{"action_id":"A0"}`

## 7. Opponent boosted or behind an effect

- **Example:** `train.jsonl` line 35436
- **Battle:** `smogtours-gen1ou-496011` (smogtours, 04-26-2020), point of view `horsea94141` (rating Unrated, WIN), Showdown turn 22, step 21 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2020/04/smogtours-gen1ou-496011_Unrated_horsea94141_vs_enpowerpoison39624_04-26-2020_WIN.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-496011
- **Label:** Metamon index 0 = move Blizzard; Showdown log shows `p1`: move Blizzard → **agrees**

**Source turn (Metamon file):**
```text
actions[21] = 0    (next action, actions[22] = 5)
forced_switch = False
player_active_pokemon = lapras  hp_pct=1.000  status=nostatus  effect=noeffect  types='ice water'
  moves as stored: blizzard (PP 7/8), sing (PP 24/24), thunderbolt (PP 24/24), bodyslam (PP 24/24)
  Metamon move order (index 0-3): 0=blizzard, 1=bodyslam, 2=sing, 3=thunderbolt
available_switches as stored: starmie 1.00 nostatus, exeggutor 1.00 par, tauros 1.00 nostatus, chansey 1.00 nostatus, snorlax 1.00 nostatus
  Metamon switch order (index 4-8): 4=chansey, 5=exeggutor, 6=snorlax, 7=starmie, 8=tauros
opponent_active_pokemon = snorlax  hp_pct=0.650  status=nostatus  effect=reflect  revealed moves=['reflect', 'bodyslam', 'rest']
player_prev_move = blizzard   opponent_prev_move = rest   opponents_remaining = 5
```
**Showdown log, turn 22 (you are p1):**
```text
|turn|22
|switch|p2a: Slowbro|Slowbro|100/100
|move|p1a: Lapras|Blizzard|p2a: Slowbro|[miss]
|-miss|p1a: Lapras
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Lapras (Ice/Water), HP 100%, healthy; base HP 130, Atk 85, Def 80, Spc 95, Spe 60
Opponent's active Pokemon: Snorlax (Normal), HP 65%, healthy, Reflect up; base HP 160, Atk 110, Def 65, Spc 65, Spe 30
Opponent's revealed moves: Body Slam, Reflect, Rest
Last moves: you used Blizzard; the opponent used Rest.
Pokemon left: you 6, opponent 5.

Legal actions:
A0 = use Blizzard: Ice, power 120, accuracy 90%, PP 7/8, 1x vs Snorlax
A1 = use Body Slam: Normal, power 85, accuracy 100%, PP 24/24, 1x vs Snorlax
A2 = use Sing: Normal, status move, accuracy 55%, PP 24/24
A3 = use Thunderbolt: Electric, power 95, accuracy 100%, PP 24/24, 1x vs Snorlax
A4 = switch to Chansey (Normal), HP 100%, healthy; base HP 250, Atk 5, Def 5, Spc 105, Spe 50; moves: Reflect, Seismic Toss, Soft-Boiled, Thunder Wave
A5 = switch to Exeggutor (Grass/Psychic), HP 100%, paralyzed; base HP 95, Atk 95, Def 85, Spc 125, Spe 55; moves: Explosion, Mega Drain, Psychic, Sleep Powder
A6 = switch to Snorlax (Normal), HP 100%, healthy; base HP 160, Atk 110, Def 65, Spc 65, Spe 30; moves: Body Slam, Reflect, Rest, Self-Destruct
A7 = switch to Starmie (Psychic/Water), HP 100%, healthy; base HP 60, Atk 75, Def 85, Spc 100, Spe 115; moves: Blizzard, Psychic, Recover, Thunder Wave
A8 = switch to Tauros (Normal), HP 100%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Blizzard, Body Slam, Earthquake, Hyper Beam
```
**Target (assistant):** `{"action_id":"A0"}`

## 8. Late game (two or fewer Pokemon left)

- **Example:** `train.jsonl` line 1672
- **Battle:** `gen1ou-2322723538` (ladder, 03-18-2025), point of view `armorfossil10172` (rating 1441, WIN), Showdown turn 41, step 45 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2025/03/gen1ou-2322723538_1441_armorfossil10172_vs_drapion55815_03-18-2025_WIN.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/gen1ou-2322723538
- **Label:** Metamon index 1 = move Earthquake; Showdown log shows `p1`: move Earthquake → **agrees**

**Source turn (Metamon file):**
```text
actions[45] = 1    (next action, actions[46] = 1)
forced_switch = False
player_active_pokemon = rhydon  hp_pct=1.000  status=nostatus  effect=noeffect  types='ground rock'
  moves as stored: earthquake (PP 16/16), bodyslam (PP 24/24), rockslide (PP 16/16), substitute (PP 16/16)
  Metamon move order (index 0-3): 0=bodyslam, 1=earthquake, 2=rockslide, 3=substitute
available_switches as stored: 
  Metamon switch order (index 4-8): 
opponent_active_pokemon = alakazam  hp_pct=0.520  status=nostatus  effect=noeffect  revealed moves=['thunderwave', 'psychic', 'seismictoss']
player_prev_move = nomove   opponent_prev_move = psychic   opponents_remaining = 2
```
**Showdown log, turn 41 (you are p1):**
```text
|turn|41
|move|p2a: Alakazam|Psychic|p1a: Rhydon
|-crit|p1a: Rhydon
|-damage|p1a: Rhydon|1/100
|-unboost|p1a: Rhydon|spa|1
|-unboost|p1a: Rhydon|spd|1
|move|p1a: Rhydon|Earthquake|p2a: Alakazam
|-damage|p2a: Alakazam|0 fnt
|faint|p2a: Alakazam
|switch|p2a: Snorlax|Snorlax|49/100 par
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Rhydon (Ground/Rock), HP 100%, healthy; base HP 105, Atk 130, Def 120, Spc 45, Spe 40
Opponent's active Pokemon: Alakazam (Psychic), HP 52%, healthy; base HP 55, Atk 50, Def 45, Spc 135, Spe 120
Opponent's revealed moves: Psychic, Seismic Toss, Thunder Wave
Last moves: you used nothing; the opponent used Psychic.
Pokemon left: you 1, opponent 2.

Legal actions:
A0 = use Body Slam: Normal, power 85, accuracy 100%, PP 24/24, 1x vs Alakazam
A1 = use Earthquake: Ground, power 100, accuracy 100%, PP 16/16, 1x vs Alakazam
A2 = use Rock Slide: Rock, power 75, accuracy 90%, PP 16/16, 1x vs Alakazam
A3 = use Substitute: Normal, status move, PP 16/16
```
**Target (assistant):** `{"action_id":"A1"}`

## 9. Validation split

- **Example:** `val.jsonl` line 1962
- **Battle:** `smogtours-gen1ou-153810` (smogtours, 04-23-2016), point of view `chimecho62545` (rating Unrated, WIN), Showdown turn 85, step 88 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2016/04/smogtours-gen1ou-153810_Unrated_chimecho62545_vs_denpowersteel85750_04-23-2016_WIN.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-153810
- **Label:** Metamon index 3 = move Thunder Wave; Showdown log shows `p2`: move Thunder Wave → **agrees**

**Source turn (Metamon file):**
```text
actions[88] = 3    (next action, actions[89] = 1)
forced_switch = False
player_active_pokemon = zapdos  hp_pct=1.000  status=nostatus  effect=noeffect  types='electric flying'
  moves as stored: thunderwave (PP 29/32), thunder (PP 14/16), thunderbolt (PP 21/24), drillpeck (PP 32/32)
  Metamon move order (index 0-3): 0=drillpeck, 1=thunder, 2=thunderbolt, 3=thunderwave
available_switches as stored: snorlax 1.00 nostatus
  Metamon switch order (index 4-8): 4=snorlax
opponent_active_pokemon = snorlax  hp_pct=0.870  status=nostatus  effect=reflect  revealed moves=['bodyslam', 'rest', 'reflect']
player_prev_move = thunderwave   opponent_prev_move = bodyslam   opponents_remaining = 2
```
**Showdown log, turn 85 (you are p2):**
```text
|turn|85
|move|p2a: Zapdos|Thunder Wave|p1a: Snorlax
|-status|p1a: Snorlax|par
|move|p1a: Snorlax|Body Slam|p2a: Zapdos
|-crit|p2a: Zapdos
|-damage|p2a: Zapdos|40/100
|-status|p2a: Zapdos|par
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Zapdos (Electric/Flying), HP 100%, healthy; base HP 90, Atk 90, Def 85, Spc 125, Spe 100
Opponent's active Pokemon: Snorlax (Normal), HP 87%, healthy, Reflect up; base HP 160, Atk 110, Def 65, Spc 65, Spe 30
Opponent's revealed moves: Body Slam, Reflect, Rest
Last moves: you used Thunder Wave; the opponent used Body Slam.
Pokemon left: you 2, opponent 2.

Legal actions:
A0 = use Drill Peck: Flying, power 80, accuracy 100%, PP 32/32, 1x vs Snorlax
A1 = use Thunder: Electric, power 120, accuracy 70%, PP 14/16, 1x vs Snorlax
A2 = use Thunderbolt: Electric, power 95, accuracy 100%, PP 21/24, 1x vs Snorlax
A3 = use Thunder Wave: Electric, status move, accuracy 100%, PP 29/32
A4 = switch to Snorlax (Normal), HP 100%, healthy; base HP 160, Atk 110, Def 65, Spc 65, Spe 30; moves: Body Slam, Hyper Beam, Rest, Self-Destruct
```
**Target (assistant):** `{"action_id":"A3"}`

## 10. Test split

- **Example:** `test.jsonl` line 1128
- **Battle:** `smogtours-gen1ou-270404` (smogtours, 04-13-2017), point of view `misdreavus32098` (rating Unrated, WIN), Showdown turn 12, step 11 in the file
- **Source file:** `gen1ou.tar.gz::gen1ou/2017/04/smogtours-gen1ou-270404_Unrated_misdreavus32098_vs_liechiberry15209_04-13-2017_WIN.json.lz4`
- **Replay:** https://replay.pokemonshowdown.com/smogtours-gen1ou-270404
- **Label:** Metamon index 3 = move Sleep Powder; Showdown log shows `p2`: move Sleep Powder → **agrees**

**Source turn (Metamon file):**
```text
actions[11] = 3    (next action, actions[12] = 5)
forced_switch = False
player_active_pokemon = exeggutor  hp_pct=0.384  status=par  effect=noeffect  types='grass psychic'
  moves as stored: psychic (PP 16/16), sleeppowder (PP 24/24), explosion (PP 8/8), hyperbeam (PP 8/8)
  Metamon move order (index 0-3): 0=explosion, 1=hyperbeam, 2=psychic, 3=sleeppowder
available_switches as stored: alakazam 1.00 slp, chansey 1.00 par, snorlax 0.48 nostatus, tauros 1.00 nostatus, lapras 1.00 nostatus
  Metamon switch order (index 4-8): 4=alakazam, 5=chansey, 6=lapras, 7=snorlax, 8=tauros
opponent_active_pokemon = snorlax  hp_pct=0.602  status=nostatus  effect=reflect  revealed moves=['reflect', 'bodyslam']
player_prev_move = nomove   opponent_prev_move = bodyslam   opponents_remaining = 6
```
**Showdown log, turn 12 (you are p2):**
```text
|turn|12
|switch|p1a: Jynx|Jynx|258/333 par
|move|p2a: Exeggutor|Sleep Powder|p1a: Jynx
|-fail|p1a: Jynx
```
**Model input (user message):**
```text
Pokemon Showdown, Gen 1 OU. Choose your action.
Your active Pokemon: Exeggutor (Grass/Psychic), HP 38%, paralyzed; base HP 95, Atk 95, Def 85, Spc 125, Spe 55
Opponent's active Pokemon: Snorlax (Normal), HP 60%, healthy, Reflect up; base HP 160, Atk 110, Def 65, Spc 65, Spe 30
Opponent's revealed moves: Body Slam, Reflect
Last moves: you used nothing; the opponent used Body Slam.
Pokemon left: you 6, opponent 6.

Legal actions:
A0 = use Explosion: Normal, power 170, accuracy 100%, PP 8/8, 1x vs Snorlax
A1 = use Hyper Beam: Normal, power 150, accuracy 90%, PP 8/8, 1x vs Snorlax
A2 = use Psychic: Psychic, power 90, accuracy 100%, PP 16/16, 1x vs Snorlax
A3 = use Sleep Powder: Grass, status move, accuracy 75%, PP 24/24
A4 = switch to Alakazam (Psychic), HP 100%, asleep; base HP 55, Atk 50, Def 45, Spc 135, Spe 120; moves: Psychic, Reflect, Seismic Toss, Thunder Wave
A5 = switch to Chansey (Normal), HP 100%, paralyzed; base HP 250, Atk 5, Def 5, Spc 105, Spe 50; moves: Ice Beam, Soft-Boiled, Thunderbolt, Thunder Wave
A6 = switch to Lapras (Ice/Water), HP 100%, healthy; base HP 130, Atk 85, Def 80, Spc 95, Spe 60; moves: Blizzard, Body Slam, Confuse Ray, Thunderbolt
A7 = switch to Snorlax (Normal), HP 48%, healthy; base HP 160, Atk 110, Def 65, Spc 65, Spe 30; moves: Body Slam, Earthquake, Hyper Beam, Rest
A8 = switch to Tauros (Normal), HP 100%, healthy; base HP 75, Atk 100, Def 95, Spc 70, Spe 110; moves: Blizzard, Body Slam, Earthquake, Hyper Beam
```
**Target (assistant):** `{"action_id":"A3"}`

The system message is identical in every example:

```text
Select one action ID from the provided legal-action list. Return only a JSON object with the single key "action_id". Do not add an explanation.
```
