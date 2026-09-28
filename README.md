EN VERSION, RU VERSION IS ON https://github.com/AlexFurkay/DeskSaw-VLLM

Introducing a desktop pet powered by a local LLM [Ollama]!
The character lives on your desktop, reacts to what you do with it and what's happening on your screen,
and speaks — not with pre-recorded lines, but with unique, non-repeating phrases.
**This is a non-commercial fan project. The characters and some of the assets are taken from the game Gunsaw**.

  **Requirements**
- Windows (macOS/Linux builds will come later),
- https://ollama.com/download, installed and running (can be minimized to the tray),
- A GPU with 6+ GB of VRAM for comfortable performance with the default model (*qwen3-vl:8b-instruct*),

The model can be downloaded manually with one command in the windows CMD, or with one command in-game as well.
List of models that will work in the game and their specs (a stronger model = better lines = needs a stronger PC)

1) `qwen3-vl:8b-instruct` - the stock model - 8GB VRAM, CPU no worse than i5-8400, 6-10GB DDR4, GPU no lower than GTX 1070ti, model weight 6.1GB

   >   download command: ollama run qwen3-vl:8b-instruct

   >   settings: aiCreativity 0.9, aiRepetitionGuard 1.2


3) `ministral-3:14b` - a stronger model - noticeably more lively and literary speech - from 10GB VRAM, CPU no worse than i5-9400, 12-16GB DDR4, GPU no lower than RTX 3060 12GB, model weight 9.1GB. Can also run on a more modest GPU (e.g. GTX 1070 Ti), but noticeably slower: generating the initial batch of lines can take around 800 seconds instead of 120

   >   download command: ollama run ministral-3:14b

   >   settings: aiCreativity 0.6, aiRepetitionGuard 1.1

DeskSaw ships as two separate versions — Russian and English, these are two different builds,
so changing the language means downloading the other build. Download the one you need from the releases section.

  **First launch**

Just run the `.exe`, everything else is created automatically on first start — nothing needs to be copied manually.

  **Core mechanics**

Mood, hunger, sleep, health - four basic stats of the character, they affect how it behaves and what it says.
Mood drops from hunger/tiredness/rough handling, and rises from petting and feeding.

  ***To pet or not to pet? - Yes***

Above a certain mood threshold the character reacts normally, but if you upset it enough — it starts rejecting affection, and may hiss/growl instead of the usual reaction. Feeding, sleep, and the reaction to getting up/falling asleep are also tied to the current state. All the main stats — **`fullness`**, **`tiredness`**, **`health`** and **`mood`** — can be seen by hovering the cursor over the character's head while holding LCTRL (no click needed).

  **Health and injuries**

The character has health (100hp). It drops from hard falls or from being hit by thrown objects. Throwing with the mouse is also deals damage when the throw is hard enough. Several escalating pain-reaction tiers with matching visible damage, plus a final line when health hits zero.

- *At 0 health* the character dies: the body falls, closes its eyes, and after saying its last line, goes silent until the application is restarted. This is a deliberate design choice, not a bug.
- *Below 20 health*, the character, even when trying to get up, collapses again — it's very weak, but stays conscious.
- The character's body visually shows damage — the lower the health, the more body parts look injured.
- **Healing** — the `bandage`, `syringe` and `medkit` items restore 5/15/30 health respectively. All items can be spawned via the console (`items` section); to apply healing, drag the item onto the character and it will figure out what to do with it. *Healing and feeding won't help a dead character...*
- Health slowly regenerates on its own over time.

**Screen reactions**

Once every 35–100 seconds (configurable) the character comments on what's on your desktop based on a screenshot.
The tone of the reaction **changes depending on the character's current health and mood** — the worse it's doing, the more tiredness, pain or low spirits noticeably comes through in the line, without overriding the reaction to the screen itself.

`Screenshots are sent directly to your own LLM model, only within the local system!`

  **Skins**

Each character (skin) is a separate folder, fully independent from the others, with its own set of textures, lore, sounds and translated lines (`TRANSLATION.json`). New skins that I add in future updates can be downloaded and installed by simply unzipping and moving over a single folder! After that (*and after restarting the application*), they appear in the `Skins` section automatically — nothing else needs to be downloaded or installed separately. Before spawning a new character, the old one needs to be removed with a dedicated console command (`Console` section).

`I'll add all the skins, but if I get the sense in any way that you, my dear and best
 users, want a particular skin sooner - just let me know however you can!`

  **How to install a character made by another author**

If someone shared their own character (a folder with textures, personality and quotes) — installing it is simple, done by hand, no launcher needed:

1. Download and **unpack** the character's archive (e.g. `Rex.zip`).
2. Find the game's save folder (`%appdata%\...\desksawPRE-RELEASE\`, if you haven't opened it in a while — `CONFIG.json` and `SAVE.json` are in there too).
3. Drag the character's folder itself (e.g. `Rex`) as a whole into the `skin\` subfolder.
4. Restart the game.

Done — the textures, personality, quotes and sound (if the author added them) will be picked up automatically. Nothing needs to be compiled or entered in the console.

*`Almost like magic`*

**What should be inside the character's folder:**
```
Rex/
├── experimentHead.png (and other textures)
├── TRANSLATION.json   (required - without it you'll only get the basic, extremely dull and weak lines)
├── lore.txt           (required - without it the personality will be empty and the character won't feel like anything)
└── sounds/            (optional - without it the default Expie sound will be used)
    ├── whine/         (regular sounds)
    ├── bark/          (aggressive sounds: hissing, barking, screaming)
    └── speech.ogg
```

>**In TRANSLATION.json each quote pool should have more than 40 lines! Otherwise, because of the**
>**thin sample, the character will respond incorrectly - the more quotes there are and the more sensible**
>**they are, the better. Overly convoluted quotes, grammar mistakes, and a mismatch with the quote's tone also**
>**cause glitches in the LLM's logic, given that the stock setup uses a fairly weak model that can**
>**only string letters into words and words into sentences. You reap what you sow**


  **Console**

Opens with `Ctrl + right click` on the character.

| Command | What it does |
|---|---|
| `help` | List of all commands |
| `toggleAI` | toggle AI on/off - the app will work like the original DeskSaw |
| `toggleProfanity` | Allow/forbid swearing in lines |
| `toggleFlirt` | Allow/forbid light flirting when mood is very high |
| `toggleDebugTags` | Show the source of every line (`[pet]`, `[screen]`, etc.) - useful for debugging |
| `toggleAiLog` | Log every generated (and rejected) line to a file - useful for debugging |
| `aiStatus` | Current state of all AI settings + list of downloaded models |
| `aiCreativity <0.0-1.5>` | randomness of responses, directly affects absolutely every line. Higher value = more varied, see the current recommended settings above in the **Requirements** section |
| `aiRepetitionGuard <1.0-2.0>` | anti-repetition protection. Higher value = lower chance of repeating itself, see the current recommended settings above in the **Requirements** section |
| `aiModel <name>` | switch the Ollama model (it will download it itself if not already present, type the model name carefully. It's best to download via the command line, where you can see progress and other useful info and models) |
| `banwords` | manage the list of banned words/phrases. I don't recommend adding regular words, since the models start hunting for synonyms and hallucinating. Example of good banwords: *Session*, *University*, *Work*, *Shower*, *Diabetes*, *Sinyavino Village*. Example of bad banwords: *Claws*, *Again*, *Hi*, *Hurts*, *How are you*, *<Country>*, *You*, *you*|
| `visionInterval <min> <max>` | How often the character comments on the screen, in seconds. First number, space, second number. If you set the minimum to 5 or so - the AI model won't be able to keep up with the lines and will start seriously choking. You don't want that anyway. For movies I set it to 65 200, for games 45 100 |
| `petSize <number>` | Character size (default 4.0), applies to newly spawned characters or after a restart |
| `spawn <name>` | Spawn an item (`bread`, `bandage`, `syringe`, `medkit`, etc.). It's easier to do this by opening the dropdown at the top - Items, and just clicking the one you want |
| `spawnExpie` | Spawn a new character. You can spawn characters without removing the old ones — they won't get in each other's way. However, more characters means more resource usage, and even on very powerful PCs spawning several characters will seriously challenge any benchmark. **I have no idea what happens if you spawn more than one critter**, and if I ever want to find out, my PC will explode |
| `despawnPet` | Remove the current pet from the screen (no one gets hurt, promise) |
| `clearItems <entity/object/name>` | remove items/pets from the screen. Just typing clearItems will remove only all items |
| `reloadLore` | Re-read the lore file without restarting the game, useful if you like fine-tuning your wording to nail the character's essence through the model's "eyes". A guide on writing lore will be published later, but trust me, *you don't want to go there* |
| `resizeConsole <width> <height>` | Console window size. You can also just drag the window from the bottom-right corner |
| `nukeData` | reset absolutely everything (save, settings) — be careful, the command name means exactly what it says |

## Having issues?

- **AI reactions are very slow or don't show up** — check that the `ollama` app is running, and that the computer has enough free resources (close heavy background programs like games/streams while testing).
- **Closed literally everything - still empty/slow** — check Task Manager; if CPU, RAM and GPU are all maxed out, the bad news is your PC can't handle the AI model. One fix - use a smaller model.
- **Something is lagging** — some VPNs intercept even local traffic (`127.0.0.1`). Add the address `127.0.0.1:11434` or the game's `.exe` itself to your VPN's exceptions
