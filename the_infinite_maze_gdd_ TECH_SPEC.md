# THE INFINITE MAZE
## Game Design Document (GDD)

---

# 1. Overview

## 1.1 Title
THE INFINITE MAZE

## 1.2 Genre
Online Persistent / Massively Multiplayer / Exploration Simulation

## 1.3 Concept Summary
This game is an online experience where players exist as minimal entities (cubes) exploring a dynamically generated, infinite maze structure.

There are no explicit objectives, reward systems, or ranking mechanisms. Players exist within the space, move through it, encounter others, or disappear.

---

# 2. World Design & Philosophy

## 2.1 Basic Structure
- Space constructed from unified marble texture
- No ceiling
- Fixed skybox environment
- Infinitely expanding structure

## 2.2 Design Principles
- No player-favoring design
- No explicit rewards
- No quests
- No experience points
- No persistent progression systems

The game centers on the core experiences of "presence," "encounter," and "choice."

---

# 3. Player Specifications

## 3.1 Player Definition
Players exist within the maze as "Player Cubes."

### Attributes
- Shape: Cubic
- No facial expressions
- No individual decorations
- Fixed size

## 3.2 Displayable Elements
- Color change functionality (emotional expression feature)
- Player names are hidden by default

## 3.3 Controls
- Movement (forward/back/left/right)
- Camera rotation
- No jumping
- No attacks
- No inventory (except Event Horizon-related items)

---

# 4. Map Generation System

## 4.1 Generation Method
- Server-side dynamic generation
- Sector-based expansion
- Expands outward when new players join

## 4.2 Structural Characteristics
- Random branching corridors
- Dead ends present
- Loop structures present
- No central hub

## 4.3 Persistence
- Maze exists continuously
- Not optimized for individual players
- No changes based on player history

---

# 5. Multiplayer Specifications

## 5.1 Server Structure
- Centrally managed server
- Always-on synchronization

## 5.2 Encounter Specifications
- Players visible within the same sector
- No voice chat
- No text chat
- Non-verbal contact only

## 5.3 Collision
- Collision detection enabled
- No physical interaction (cannot push)

---

# 6. Account Management

## 6.1 Login
- Account creation required
- Session management enabled

## 6.2 Inactivity Processing
- Automatic deletion after 40 days from last login

## 6.3 Absorption Effect
Gradual visual changes:
1. Color desaturation
2. Edge softening
3. Integration into wall mesh

After deletion, recovery is impossible.

---

# 7. Event Horizon

## 7.1 Definition
Circular void structures randomly generated within the maze.

## 7.2 Discovery Conditions
- Found through exploration by chance
- No explicit markers

## 7.3 Emerald Stone System
Upon discovery, 2 stones are granted to the player.

### Usage Methods

#### Using 1 Stone
- Transition to aerial view mode
- Simple full map display
- Other players shown as light points
- Time limit: 30 seconds
- Name input functionality
- If target player exists, warp to nearby location

#### Using 2 Stones Simultaneously
- Immediate permanent account deletion
- Player cube immediately disappears
- Recovery impossible

No confirmation screen.

---

# 8. UI / UX

## 8.1 HUD
- Minimal display
- No permanent map display
- No status display

## 8.2 Logs
- System notifications minimized
- No display of remaining inactivity days

---

# 9. Sound Design

- Echoing footsteps
- Minimal ambient sound
- No BGM

---

# 10. Technical Requirements (Initial Assumptions)

- Engine: TBD (Unity / Unreal assumed)
- Online synchronization required
- Dynamic map generation algorithm needed
- Low-load mesh integration processing

---

# 11. Monetization Policy (Provisional)

- Base play: paid or one-time purchase
- No microtransactions
- No advertisements

---

# 12. Core Experience Definition

The purpose of this game lies in the following experiences, not rewards or achievements:

- Existence in purposeless space
- Chance encounters
- Unmapped structure
- Choice of voluntary disappearance

---

End of Document
