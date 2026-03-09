# **Yong Yun Jing**
### Game Developer · Multi-Disciplinary Designer


I'm a game developer with a BA in User Experience and Game Design and a Diploma in Visual Effects, with a background on programming and a specialization in general design. My background sits at the intersection of technical implementation and user-focused design — I care about how games feel to play, and I care equally about how they're built.

Most of my experience has come from working on student game projects in multidisciplinary teams alongside designers, engineers, and artists — on both commercial engines and fully custom-built ones. That last part tends to be where things get interesting. When standard tools aren't available, I lean into building the infrastructure that makes development possible: pipelines, tooling workflows, and systems that let the rest of the team focus on making the game good.

Outside of gameplay systems, I have a strong interest in solving production problems through tooling and data-driven design. If there's a bottleneck in the workflow, I'd rather fix it properly than work around it indefinitely.

In the following portfolio showcase, I've mostly extracted the most exciting and crucial achievements of each project. However, I also play a role in other aspects of the game development cycle, which would be too much to write out in full. **I'd love to chat to talk more about any points you might be interested in particular!**

[Check out my website](https://yongyunjing.com)

## Technical Skills

- **Languages:** C#, C++
- **Engines** Unity
- **Specialisations:** Art Implementation for Games, Rapid Gameplay Prototyping, Workflow Development

## Featured Projects

Take a look at what I've been building. Each project reflects a different set of constraints, team structures, and technical challenges — hopefully something here catches your eye.

| Project | Type |
|---|---|
| [Moledy](#moledy) | 2D Arcade Platformer · Custom Engine |
| [Clustercluck](#clustercluck) | 2-Player Party Game with a Twist |

## Other Projects

I've contributed to these projects as well, but they did not have as many challenges or learning points as the featured projects. Neverthelessly, do check them out as well!

| Project | Type |
|---|---|
| [Bubble Thoughts](https://4ngrypanda.itch.io/bubble-thoughts) | Casual Typing Game · Learnt Godot in 2 days |
| [Super Sidekick Speed-dating Sim](https://4ngrypanda.itch.io/super-sidekick-speed-dating-sim) | 2 Player Casual Competitive Sidekick Recruitment |
| [Encore of Faces](https://4ngrypanda.itch.io/encore-of-faces) | Pinball to defeat the bosses |

# Moledy
### 2D Arcade-Style Platformer Developed in Custom C++ Engine
[![](Moledy-Cover.png)](https://youtu.be/rfO5-KV_KQ0?si=jJjIUZFnygxD8jpj)
[Watch the Trailer](https://youtu.be/rfO5-KV_KQ0?si=jJjIUZFnygxD8jpj)

[Download Now](https://drive.google.com/file/d/1JNr8h3tT3QLvNpH09Q4uW34RAhisFPuY/view?usp=drive_link)

## Overview

Moledy is a time-based arcade platformer I developed with a multidisciplinary student team, built entirely on a **custom C++ game engine**, with Unity used as a initial prototyping engine. The core loop puts players underground, digging through destructible terrain, attacking enemies and dodging hazards, all under constant pressure to set a new time record.

Working without a commercial framework meant that a lot of things most teams take for granted — tile editors, level pipelines, asset instancing — simply didn't exist out of the box. That gap ended up defining a huge portion of my work on this project.

| | |
|---|---|
| **Genre** | 2D Arcade Platformer |
| **Engine** | Custom Engine with C++ Scripting Support |
| **Team** | 2 Designers · 6 Engineers |
| **My Role** | Design Lead / Gameplay Programmer / Technical Artist|

## Languages & Tools

| Tool | Purpose |
|---|---|
| C++ | Custom engine gameplay scripting |
| C# | Rapid prototyping in Unity |
| Tiled Map Editor | Visual level authoring |
| JSON | Runtime level data format |
| XInput API | Controller input mapping |

## What I Built

These are the core systems I personally designed and implemented:

### 🟫 Tile-Based Digging Mechanic
The signature mechanic of the game. Implemented destructible terrain traversal using the engine's C++ scripting layer. The engine didn't support disabling tile objects at runtime, so I worked around this by **setting tile dimensions to `(0, 0)` and disabling their colliders** — effectively making them disappear without touching the engine's core. A small hack, but it worked cleanly and held up throughout the full playtest cycle.

### 🎮 Character Movement & Jump Controller
Built the player's movement and jump behaviour using the engine's `Physics2D` framework. This had to be tuned iteratively throughout the project as there were significant difference between the custom engine's Physics as compared to Unity's.

### 🕹️ Controller Input System
Integrated the **XInput library** to support gamepad play, mapping each player action to the appropriate input. Getting this hooked into gameplay state cleanly took more wiring than expected, but the result was a much more natural feel for the game's fast-paced mechanics.

### 🔄 Gameplay Loop & State Logic
Implemented the main gameplay states — roaming, digging, combat and level interactions. This was the main part of the development that brought everything else together.

### 🚪 Scene Transitions & Menus
Collaborated with the engineering team to handle transitions between menus, the tutorial, and gameplay levels — making sure state was properly cleaned up and handed off between scenes.

## The Big Problem: Level Creation

Early in development we encountered a siginificant obstruction to our rapid iterations.

The custom engine had **no tile mapping tools, and no tile instancing support**. The only way to build a level was to place every single tile manually inside the engine itself, using a level editor that was prone to mistakes (especially because undo wasn't a thing!)

**Without a pipeline:**

Designer intent  →  [manual tile placement, one by one]  → Engine

- ☠️ hours per level
- ☠️ impossible to iterate
- ☠️ playtesting nearly blocked

This wasn't sustainable and we were burning design time on busywork instead of actually building the level.

## The Solution: A Custom Level Authoring Pipeline

I designed and built an external workflow to fix this entirely.

| Tiled Map Editor | C# Conversion Tool | Custom Engine |
|-------|-----|------------|
| Designer visually builds levels with native tile placement | Parses Tiled Datafile and restructures it to JSON format  | Loads JSON at runtime and loads level for play |

**Step by step:**

1. **Design in Tiled** — Designers used the Tiled Map Editor to visually lay out levels using a grid and the game's actual art assets. No engine knowledge required.

2. **Convert with C#** — I wrote a C# tool that parsed Tiled's exported map format, restructured the data to match the engine's runtime expectations, and output a lightweight JSON file.

3. **Load at runtime** — The engine read the JSON on scene load and reconstructed the level dynamically, with zero manual tile placement needed.

The whole pipeline required **no modifications to the custom engine itself**, simply supporting the existing architecture that was already in place.

## Impact

The change was immediate and significant:

- ✅ Level iteration went from days to minutes
- ✅ Designers could focus on pacing and layout, not manual work
- ✅ The single largest production bottleneck was gone
- ✅ The project remained feasible within the timeline

Without this pipeline, I'm not sure we would have actually been able to ship a level that was more complex than just a rectangle. The time saved went directly into playtesting and tuning — which is where it needed to go.

## Reflection

Moledy taught me a lot about the hidden infrastructure costs of custom engine development. It's easy to underestimate how much a commercial engine quietly does for you. Building (or in this case, *working around*) that infrastructure ourselves made every system feel earned.

The level pipeline in particular is something I'm proud of — not because it was technically complex, but because it solved a real problem cleanly, with minimal disruption to the rest of the team's workflow.

---

# Clustercluck
### Cooperative Party Game with a Chaotic PvP Twist
[![](Clustercluck-Cover.png)](https://www.youtube.com/watch?v=l_QdnzvIQ8s)

[Watch the Trailer](https://www.youtube.com/watch?v=l_QdnzvIQ8s)

[Download Now](https://4ngrypanda.itch.io/clustercluck)

## Overview

Clustercluck is a cooperative party game for two players where you play as chickens running a fast-paced nugget restaurant — serving nuggets to an endless line of very hungry chicken customers.

Players rush around the kitchen performing tasks such as collecting raw chicken, grinding it, cooking nuggets, and serving orders before customers lose patience. The game focuses on **communication, timing, and coordination**, similar to games like *Overcooked*.

Just when players think they've mastered the workflow, the game introduces a twist: the cooperative kitchen suddenly turns into a **PvP battle**, where the last remaining chicken cooks their former teammate and serves them to the waiting customers.

The project was developed in **Unity using C#**, with a team of designers, programmers, and artists collaborating to deliver a polished, narrative-driven party game experience.

| | |
|---|---|
| **Genre** | Cooperative Party Game |
| **Engine** | Unity |
| **Team** | 10 Members |
| **My Role** | Core Programmer / Technical Artist / Gameplay Implementation |

## Languages & Tools

| Tool | Purpose |
|---|---|
| Unity | Game engine and scene management |
| C# | Gameplay programming |
| Unity Input System | Player input and interaction |
| Aseprite | Pixel art asset creation (art team) |

## What I Built

These are the main systems and contributions I worked on during development.

### 🎮 Controller Input System

I implemented the controller input system to support gamepad play, expanding the player experience beyond basic keyboard controls.

This involved mapping player actions to controller inputs and ensuring that movement, interaction, and gameplay actions felt intuitive and responsive. Because the game relies heavily on fast coordination between two players, controller responsiveness was critical to making the gameplay feel satisfying.

### 🧩 Gameplay Implementation & Tech Art

I worked closely with the art team to integrate their assets into the game and transform static artwork into a functioning gameplay experience.

This involved:

- Implementing interactive kitchen stations
- Integrating character sprites and animations
- Building interactive gameplay objects
- Ensuring the visual presentation remained consistent during gameplay

The role required a blend of programming and technical art, translating the artists' work into a cohesive interactive environment.

### 🔄 Game States & Flow

I collaborated with another programmer to implement the main game flow and progression states, including:

- Active kitchen gameplay
- Task completion and food serving
- Customer interaction loops
- Transition into the final PvP phase

This system ensured the cooperative gameplay transitioned smoothly into the game's surprise competitive finale.

## The Big Challenge: Making Controls Feel Good

Clustercluck is fundamentally about **fast player coordination**. If movement and interactions feel even slightly awkward, the entire experience breaks down.

While implementing controller support was technically straightforward, the real challenge was **tuning the responsiveness and feedback** of player actions so that interactions felt natural during chaotic gameplay.

Small details mattered:

- How quickly players could pick up items
- How interactions triggered near kitchen stations
- How clearly the game communicated successful actions

Because the gameplay loop is built around rapid interactions, the difference between *working controls* and *good controls* was huge.

## The Solution: Iterative Input & Feedback Tuning

To address this, I focused on refining the interaction layer between player input and gameplay systems.

This included:

- Fine-tuning interaction detection around kitchen stations
- Adjusting input responsiveness for player movement and actions
- Improving feedback when actions succeeded or failed

Rather than adding complexity, the goal was to **make the controls invisible** — players should be thinking about cooking and coordinating with their partner, not fighting the controls.

## Impact

These improvements helped shape the overall feel of the game:

- ✅ Gameplay interactions became faster and more intuitive
- ✅ Controller support enabled a better couch-co-op experience
- ✅ Players could focus on coordination rather than control friction
- ✅ The cooperative chaos of the kitchen felt smooth and responsive

Even though the game mechanics themselves were intentionally simple, polishing the player experience helped make the gameplay feel engaging and satisfying.

## Reflection

Clustercluck reinforced how important **player feel** is in multiplayer games.

Even simple mechanics can become frustrating if input responsiveness and feedback aren't carefully tuned. By focusing on interaction design and controller responsiveness, we were able to turn a straightforward gameplay loop into something that felt much more dynamic during play.

Working as both a programmer and technical artist also gave me a deeper appreciation for the bridge between art and gameplay — making sure that visual assets don't just look good, but **function well within the interactive system**.
