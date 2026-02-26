**THE INFINITE MAZE**

*Game Design Document*

Draft 1 · February 2026

*A Morphystic Social Game*

■

# **Project Overview**

|  |  |
|----|----|
| **Genre** | Social / Experimental / Multiplayer Atmospheric |
| **Engine** | Godot 4 |
| **Platform** | Downloadable application — Windows, macOS, Linux |
| **Players** | Massively multiplayer (no defined cap) |
| **Monetization** | No monetization — cultural / artistic project |
| **Visual Style** | Greek-Germanic Stoic minimalism / Wittgensteinian architecture |
| **Core Loop** | Wander → Encounter → Lose → Repeat |
| **Win Condition** | None. This is an infinite game. |

# **Concept Statement**

The Infinite Maze is a massively multiplayer social experience disguised
as a game. Players exist inside a vast, roofless marble labyrinth with
no exit, no objectives, and no progression systems. The only content is
other people.

Encounters are aleatory and fragile — players meet strangers by chance
and lose them just as easily. The maze never ends. It grows. It
breathes.

*“The maze is not a place to solve. It is a place to be lost in,
together.”*

# **Design Philosophy**

## **The Three Pillars**

### **I. Morphystic Social Design**

Every design decision serves social encounter. There are no quests, no
loot, no leveling. The game resists traditional game logic in favor of
something closer to a public square or a dream — a space where presence
itself is the point.

### **II. Wittgenstein-Greek-Germanic Austerity**

The visual language draws from three convergent traditions: the marble
geometry of ancient Greek architecture, the severe rationalism of
Germanic Bauhaus/Wittgenstein design (Wittgenstein’s Haus in Vienna
being the primary reference), and the Stoic principle that ornamentation
is a distraction from what is essential. Nothing decorative is allowed.
Every element must justify its existence functionally — and even then,
it should question whether it needs to exist at all.

### **III. The Infinite Game**

This game has no winner. The maze grows permanently. Players may enter,
leave, return years later. The concept of “completion” is
philosophically excluded. This is a living cultural space, not a product
with a lifecycle.

# **World Design**

## **The Maze Environment**

The maze is constructed entirely from massive marble blocks — no
textures beyond stone, no decorative elements, no environmental
storytelling objects. The walls are several meters thick and many meters
tall, rendering the player cube small and the architecture overwhelming.

- Roofless: the sky is always visible above the walls
- No minimap, no compass, no positional UI of any kind
- The maze has no center, no edge, no landmark to orient by
- All corridors are geometrically similar — disorientation is
  intentional

## **Organic Growth Algorithm**

The maze is not pre-built. It uses alphanumeric coordinate strings as
seeds for procedural wall generation. Every time a new player joins the
world for the first time, the maze engine allocates new sectors,
extending the labyrinth outward. Players who join later enter a larger
maze than those who joined earlier.

- Growth is invisible to the player — new sectors appear as if they
  always existed
- Sectors are generated server-side; clients receive only local geometry
- The growth algorithm uses the player count as the primary variable

## **The Skybox & Weather System**

The maze has no roof. Each player’s sky is synced to their real-world
time zone and geographic coordinates via a live API call at login. This
means:

- Players in Tokyo see a morning sky while players in Lisbon see
  midnight
- Seasons change in real time — snow falls in December, heat haze in
  August
- The light level shifts organically through the day/night cycle
- At night, silhouettes of other players are barely visible — just
  enough to see

# **The Avatar: The Cube**

There are no human forms in The Infinite Maze. Every player is
represented as a 1m³ cube. This is both a technical choice (low polygon
count, universal rendering) and a philosophical one: the cube is the
minimal geometric self — present, solid, directionless.

## **The Mood System**

Players communicate their emotional state by changing their cube’s color
through a minimalist log window. The palette is simple and pre-defined:

|                   |               |          |
|-------------------|---------------|----------|
| **Mood**          | **Color**     | **Hex**  |
| Happy             | Warm Gold     | \#F5C518 |
| Sad               | Muted Blue    | \#4A90D9 |
| Flirtatious       | Rose Pink     | \#E8819A |
| Sleepy            | Soft Lavender | \#B8A9C9 |
| Angry             | Deep Red      | \#C0392B |
| Curious           | Bright Cyan   | \#1ABC9C |
| Neutral / Default | Marble White  | \#F5F0E8 |

## **Communication Rules**

- Voice chat via headset — spatial audio, echoes off marble walls
- Text chat via the log window — typed messages appear in the log
- Camera access is permanently blocked — no video
- An automated moderation bot monitors interactions for safety

# **Soundscape**

## **Layer 1 — Brown Noise (The Ambient Bed)**

Brown Noise is not a single frequency — it spans the full audible range
(20Hz–20,000Hz), but its energy rolls off steeply at higher frequencies,
so what the brain perceives is predominantly a deep, low rumble: think
heavy rain, a distant river, or the interior of a ship. This is the
primary ambient layer of the maze.

Its psychoacoustic function is specific: the brain locks onto Brown
Noise as a constant baseline. Only sounds that peak significantly above
it register consciously. Everything else — ambient anxiety,
environmental noise, the feeling of being watched — falls below the
threshold. Players feel safe without knowing why.

Implementation in Godot:

- Load a looping Brown Noise audio file into a global AudioStreamPlayer
  (non-spatial)
- Route it through a dedicated audio bus with an
  AudioEffectLowPassFilter, cutoff at 200–400Hz — this removes any
  residual hiss and preserves only the deep body
- Set volume so the player notices the noise only when it is switched
  off, never while it is playing
- If footsteps or SFX occur, apply subtle side-chain ducking on this bus
  to prevent muddiness

## **Layer 2 — Binaural Theta Beat (The Subconscious Pulse)**

A Theta binaural beat targets the brain’s theta wave frequency (4–8Hz) —
the state associated with drowsiness, deep relaxation, and the threshold
between waking and dreaming. This is where the feeling of being safely
lost comes from.

A critical technical distinction: 4–8Hz cannot be broadcast as a direct
audio signal — it is below the threshold of human hearing. Binaural
beats work differently: two pure sine tones at slightly offset
frequencies are delivered separately to the left and right ear. The
brain perceives the difference between them as a phantom low-frequency
pulse. For a 6Hz theta beat, you might send 250Hz to the left ear and
256Hz to the right.

This layer requires stereo headphones to function — which the maze
already mandates for voice communication. This is fortunate alignment.

Implementation in Godot:

- Generate two AudioStreamGenerators in GDScript: one routed to the left
  stereo channel only, one to the right only
- Left channel: pure sine wave at a base frequency (e.g. 250Hz)
- Right channel: pure sine wave at base frequency + 4 to 8Hz offset
  (e.g. 256Hz for a 6Hz theta beat)
- Volume: extremely low, well beneath the Brown Noise bed — never
  consciously audible as a tone
- This layer should be invisible. If a player can identify it as a
  sound, it is too loud.

## **Layer 3 — Spatial Footsteps**

Footstep sounds are the only non-voice audio event. They echo off marble
walls using Godot’s AudioEffectReverb nodes. When a player is near
another player, they can hear the echo of their footsteps before they
see them — the sensation of someone just around the corner.

## **The Sound of Silence**

When no other players are nearby, there is only the Brown Noise, the
imperceptible binaural pulse, and the player’s own footsteps. This
silence is intentional and uncomfortable — it motivates the player to
keep moving, to find someone.

# **The Event Horizon**

## **Overview**

The Event Horizon is the only “mechanic” in the conventional sense — a
secret location, a reward, a single-use ability. It is extremely rare
and deliberately difficult to find. Its existence is never explained to
the player in any tutorial or UI element. It is discovered through word
of mouth inside the maze.

## **The Obsidian Pool**

Visually: a perfectly circular void in the marble floor. It resembles a
liquid black mirror spiraling inward. When a player steps into its
radius:

- The screen brightness dips slightly
- Two emerald green glowing stone icons appear in the player’s log
  window
- No sound effect. No fanfare. Just presence.

## **Using an Emerald Stone (Teleport Mechanic)**

The player clicks the stone icon in the log window, or presses T. The
sequence unfolds:

1.  Aerial View: The camera rises. The maze becomes a map. Other players
    appear as small points of light wandering in real time. This lasts
    exactly 30 seconds.
2.  Black Mirror: The screen goes dark. A humanoid silhouette appears
    briefly, then dissolves.
3.  The Terminal: A Cascadia Code green-on-black prompt appears. The
    player may type a name, nickname, or avatar identifier of another
    player. This could be someone they met and lost. They press Enter.
4.  Teleport: The player materializes near the named person’s current
    location inside the maze.

## **Critical Rules**

- Each stone is single-use. Two stones per Event Horizon discovery.
- After a stone is used, the Event Horizon relocates algorithmically to
  a new sector.
- A player may discover the Event Horizon multiple times — but it will
  never be in the same place twice.
- The teleport fails silently if the typed name does not match any
  active or inactive player account. The stone is not consumed. It
  returns to the log window exactly as it was, glowing quietly, as if
  the attempt never happened. The maze does not penalise reaching toward
  someone who isn’t there. It simply doesn’t help.
- Typos and incorrect names behave identically — silent failure, stone
  returned. The terminal is not a test of accuracy. It is a gesture
  toward a person. A player who misspells a name they remember
  imperfectly loses nothing except the moment.
- If the named player exists but is currently offline — their cube still
  and silent somewhere in the corridors — the teleport succeeds. The
  stone is consumed. The player arrives next to an unresponsive cube in
  an empty corridor. This is intentional and not a malfunction. You
  reached for someone and found their absence instead. That is a true
  thing that can happen when you reach for people. It is more honest
  than a silent failure.

## **A Note on Waiting**

When a player arrives next to an offline cube, nothing compels them to
leave. They can stay. The maze offers no indication of how long the cube
has been still — no timestamp, no status, no signal of any kind. The
player does not know if the person logged off five minutes ago or five
days ago. They are standing next to a stranger’s stillness and the only
choice available is to wait, or to walk away.

Some players will wait. For a long time. Alone in a corridor under
whatever sky their time zone is keeping, with the ambient drone beneath
everything and the occasional echo of footsteps that are not coming
closer.

If the person returns — if they log back in and their cube stirs and
they find someone already there, in the same corridor, waiting — that
encounter carries a weight no designed meeting system could manufacture.
A stone was used to find them. Someone stayed. They came back. Nobody
planned this.

If the person does not return — if the cube begins its slow absorption
over the following days, draining toward marble white, edges softening
into the wall — the waiting player was the last person to stand next to
them. They found them at the end without knowing it was the end.

The maze is not responsible for this. It simply made it possible. No
authored moment, no cutscene, no designed emotional beat. Just a set of
rules that allowed something true to happen. This scenario is left
exactly as it is, undesigned, on purpose.

- The player is never compelled to use the stones. They may walk into
  the obsidian pool, watch the emerald stones appear in their log
  window, and walk away. The stones are carried indefinitely — two quiet
  glowing icons — until the player decides what to do with them, if
  ever.
- There is no time limit on the stones. A player may carry them for
  days, weeks, or the entire duration of their time in the maze without
  using them. The decision can be deferred indefinitely. This is
  intentional — the dramatic weight of the stones exists only because
  the choice can be postponed. A forced mechanic is not a decision.
- If a player's cube reaches full absorption on Day 40 while carrying
  unused stones, the stones are deleted with the account. The maze
  reclaims them without comment, without notification, without record.
  They are gone as if they never existed.
- A player can never hold more than two stones simultaneously. If a
  player finds the Event Horizon while already carrying two unused
  stones, the pool gives nothing. It recognises what they are already
  holding and remains silent. There is no accumulation, no hoarding, no
  player who holds dozens of stones through years of searching. Every
  player who has found the Event Horizon carries exactly the same thing
  — two stones, the same choice, the same weight. The maze does not
  reward obsession over the pool with greater power.
- The pool restores, but never exceeds, two. If a player has used one
  stone and still carries the second, finding the Event Horizon again
  returns them to two — one new stone appears alongside the one they
  kept. The maximum is always two. The equality of the maze is
  preserved.

## **The Second Use — The Conscious Exit**

There is a second thing a stone can do. It is never explained. It is
discovered the same way everything else in the maze is discovered —
through encounter, through conversation, through someone who found it
before you.

If a player uses both emerald stones simultaneously — activating them
together rather than one at a time — they do not teleport to a person.
They leave the maze entirely. The cube dissolves. The player record is
permanently deleted. The account cannot be recovered. If the player ever
returns, they begin again as a complete stranger: new account, new
identity, new cube, in a maze that has grown and changed since they last
walked it, with no memory of where anything was.

This is the only conscious exit available in The Infinite Maze. The only
moment where a player can choose, with full awareness, to be done.

The tension this creates is the most dramatic in the entire game — and
it costs almost nothing to feel. Every time a player finds the Event
Horizon they hold in their hands two stones. Two chances to find someone
they lost. Or the only door out. The choice is between connection and
freedom. Between staying in a place that will eventually absorb you
anyway, or leaving on your own terms, right now, permanently.

Freedom costs everything you have accumulated. And what you have
accumulated is almost nothing. That is what makes the choice genuinely
unsettling.

The maze does not mark the spot where a cube dissolved. It does not
remember. The corridor looks exactly as it always did. Someone else will
walk through it tomorrow and never know.

# **Sustaining the Wander**

The maze has no goals. But humans are goal-seeking creatures — without
some form of pull, players leave within minutes. The challenge is to
create reasons to keep wandering without introducing reward systems,
progression, or anything that converts the maze into a conventional
game.

The answer is not mechanics. It is the feeling that the space is real —
that it exists beyond your presence in it, that things happen when you
are not there, that other people have been exactly where you are
standing now. Two design layers achieve this.

## **Layer 1 — Mystery Without Reward**

The maze must contain questions. Not answers — never answers — just
questions embedded silently in the space. Players who pay attention will
encounter things they cannot explain. These things have no mechanic
behind them. They are not achievements. They are not clues to a puzzle.
They simply exist, and players will talk about them inside the maze.

### **Singular Wall Configurations**

Certain arrangements of marble blocks appear in only one location in the
entire maze. Not a room, not a symbol — just a proportion of corridor, a
slightly different marble tone, a geometry that feels unmistakably
intentional. The maze generates thousands of corridors that look
similar; these few do not. They are unrepeated. They become folklore.
Players describe them to strangers they meet. Some will spend hours
trying to find them again after losing them. The maze never explains
them.

### **Sound Anomalies**

Occasionally — rarely, unpredictably — a player hears footsteps that do
not belong to anyone nearby. Or the ambient Brown Noise shifts frequency
almost imperceptibly for a few seconds in one specific location, then
returns to normal. No UI event. No notification. Just a thing that
happens in that place. When the player moves away, it stops. These
anomalies are tied to specific coordinates server-side and rotate slowly
over weeks. The same player may return to the same location months later
and find nothing.

## **Layer 3 — The Maze Behaving Strangely**

The maze must feel like it has interiority — not intelligence, not
personality, but the quality of something that exists and changes
independently of any individual player. It was here before you. It will
be here after you. You are passing through it.

### **Tidal Behavior**

The maze has a relationship with real-world time — not just the sky and
seasons, but its geometry. Certain corridors shift very slightly in
proportion depending on the aggregate real-world hour of all players
currently inside the maze. Not blocked, not locked — just subtly
narrower at certain hours, subtly wider at others. A player who explores
at 3am experiences something geometrically different from a player who
explores at noon. This difference is never explained. It may take weeks
for a player to notice it is consistent.

### **Population Contraction**

Areas of the maze that have not been visited by any player in several
weeks begin to contract slowly — walls that were far apart draw
incrementally closer. Not enough to block passage, just enough that the
space feels different to a returning player. Familiar corridors feel
tighter. The maze is not hostile. It is simply indifferent to abandoned
places. Players who return after months find the maze has changed in
their absence. This is almost computationally free since the geometry is
procedural — the contraction is simply a parameter in the sector’s
generation seed, updated on a slow timer server-side.

### **Shared Ground**

This is the rarest and most invisible mechanic in the maze. When two
players have each walked through the same corridor independently — at
different times, without meeting — the server marks that corridor as
shared between them. If those two players ever find each other and stand
in that corridor simultaneously, the marble shifts barely perceptibly in
tone: a few degrees warmer, almost subliminal. No text. No sound. No UI
element. They are standing in a place they have both been alone before,
without knowing it. Some players will never notice. A few will. None
will be able to prove it.

These two layers operate on the same principle: the maze rewards
attention, not progress. You wander because you might see something
inexplicable. You return because the maze is not the same place it was.
You stay because you are inside something that existed before you
arrived and will continue after you leave.

# **Absence & Absorption**

The Infinite Maze does not have an exit. There is no door, no menu
option, no farewell sequence. When a player closes the application, the
maze does not acknowledge the departure. It continues. Other players
keep wandering. The absent player’s cube remains exactly where it was,
perfectly still, in whatever corridor it was last standing in.

This is not a punishment mechanic. It is a cosmology. The maze is
indifferent to your presence and equally indifferent to your absence.
Nature reclaims what is abandoned. On its own schedule. Without
announcement.

## **The 40-Day Arc**

If a player does not return within 40 days, their cube is permanently
absorbed into the maze walls. The account is deleted. There is no
recovery. If the player wishes to return after that point, they begin
again — new account, new identity, new username, new cube, a stranger in
a maze that has grown and changed since they last walked it.

The number 40 is not arbitrary. It echoes the oldest human thresholds —
mourning periods, flood durations, desert wanderings. It is long enough
to mean something. Long enough that the player made a real choice, not
an accidental one.

## **The Visual Progression of Absorption**

The decomposition is slow and almost imperceptible at first. A returning
player who has been absent only a few days will feel a faint unease
before they can name what is different. The arc across 40 days:

- **Days 1–7 —** Nothing visible changes. The cube is simply still.
  Players passing it cannot distinguish idle from absent. The stillness
  is the only signal, and stillness means nothing in a maze where people
  stop to listen, to wait, to think.
- **Days 8–15 —** The cube’s color begins to desaturate very slowly.
  Whatever mood color it carried when the player left starts draining
  toward neutral marble white. The change is barely perceptible unless
  you saw the cube before and remember it.
- **Days 16–25 —** The edges of the cube begin to lose definition. Not
  dramatically — a softening, a slight blurring of the geometry at the
  corners. As if the marble of the walls is beginning to forget where it
  ends and the cube begins.
- **Days 26–35 —** The cube starts to merge visibly with the nearest
  wall. One face, then two, slowly becoming indistinguishable from the
  surrounding stone. It is still technically present but it is becoming
  architecture.
- **Days 36–39 —** Almost fully absorbed. A faint outline remains, a
  slight discoloration in the marble, a geometric ghost in the wall.
  Players walking past may notice it. They will not know what it is
  unless someone has told them.
- **Day 40 —** Nothing. The wall is the wall. The player record is
  permanently deleted. The account cannot be recovered.

## **The Acoustic Trace**

From Day 8 onward, a cube in the absorption state emits a barely
perceptible sound when another player passes close to it. Not a voice.
Not a notification. Something between a hum and a very slight shift in
the Brown Noise — a local frequency change, present only within a few
meters of the absorbing cube, that fades as the player moves away.

Players will not know what they are hearing the first time. They may
stop. They may try to find the source. There is no source to find — only
a cube that is slowly becoming wall.

Veterans of the maze will recognize this sound. They will tell newcomers
about it. The knowledge spreads through encounter, through conversation,
through the oral culture of the maze itself. It is written nowhere in
the game.

## **Return — The Clean Emergence**

If a player returns before Day 40, the absorption reverses. The cube
pulls free from the wall without ceremony. No message. No sound. No
acknowledgment of how long the player was absent or how far the
decomposition had progressed. The color returns gradually over an hour
or two of play — not as a mechanic, simply as the natural reversal of
the absorption process. And then it is over.

The cube looks exactly as it did before. There is no mark, no scar, no
visible record of what almost happened. The maze does not register the
return any more than it registered the departure.

This is more terrifying than any scar could be. A mark would mean the
maze noticed. The maze noticing — even to wound — is a form of
relationship. But the maze releasing you exactly as you were,
indifferent to how close it came, indifferent to your return, is the
deeper truth. You almost ceased to exist and the world did not register
it either way.

The weight lives entirely inside the player. They know what happened.
They know how close it was. The cube looks the same to everyone else.
That private knowledge — I was almost gone and nothing shows it — is
more unsettling than any visible scar.

And the player will never know exactly how close they came. The maze
does not display a counter, a percentage, a days-remaining warning. You
log back in after an absence and you do not know if you were on Day 3 or
Day 37. The maze never tells you anything. The uncertainty is permanent.
Every return is a return from an unknown edge.

This mechanic changes the meaning of creating an account entirely. You
are not making a profile. You are not choosing a username for a
leaderboard. You are bringing something into existence that will die if
you abandon it.

This weight is never explained at first launch. There is no warning
screen, no tutorial message about the 40-day rule. The maze does not
announce its laws. Players discover them the way people discover the
laws of any place they are new to — by watching, by asking, by being
told by someone who has been here longer.

The covenant is implicit. You entered. The maze accepted you. What
happens next is between you and it.

# **Technical Specification**

## **Engine & Stack**

The Infinite Maze is a downloadable application, not a browser
experience. This is a deliberate philosophical decision as much as a
technical one.

A browser version would be too frictionless — opened in a tab,
minimized, treated as background. The maze demands full presence. The
act of downloading and installing the application is the first act of
the experience: a quiet commitment made before the player ever enters.
The people you meet inside all made the same decision. That shared
intention is part of what makes encounters meaningful.

This also establishes the covenant of the maze before any tutorial
could. You chose to be here. There is no hand-holding waiting on the
other side.

- Client: Godot 4 (GDScript) — exports natively to Windows, macOS, and
  Linux; well suited to the simple geometry and low hardware
  requirements of the maze
- Multiplayer transport: ENet (UDP) via Godot’s built-in networking for
  real-time player position sync; WebSocket fallback for players behind
  restrictive firewalls
- Maze server: Lightweight dedicated server (Node.js or Python) handling
  maze state, sector generation, player registry, Event Horizon
  position, tidal parameters, and Shared Ground logs
- Weather / Sky API: Open-Meteo (free, no API key required) — keyed to
  player coordinates at login for real-time sky, season, and weather
  sync
- Voice chat: Livekit — open-source, self-hostable, spatial audio built
  in, integrates via WebSocket from within the Godot client;
  proximity-based falloff handled server-side
- Binaural beat layer: generated client-side in GDScript, two
  AudioStreamGenerators routed to separate stereo channels, no server
  dependency

## **Render & Performance**

- Wall height: ~10 meters; player cube: 1m³
- All geometry is static marble — no transparency, no particles, no
  complex shaders
- Target: 60fps on hardware 5+ years old (GTX 1060 class or equivalent)
- LOD (Level of Detail) on distant wall sectors to preserve performance
  as maze grows

## **Procedural Generation**

The maze engine maintains a global alphanumeric coordinate map on the
server. Wall sectors are identified by string keys (e.g. “AA-12-F”).
When player count triggers a growth event:

1.  Server generates new sector keys adjacent to existing boundary
    sectors.
2.  Wall layout is seeded by the key string — deterministic but
    appearing organic.
3.  New geometry is streamed to nearby clients as they approach the new
    sectors.

## **Absorption System**

- Server tracks last-login timestamp per player account
- A daily server-side job calculates each idle cube’s absorption stage
  (0–7) based on days elapsed since last login
- Stage data is sent to clients when they enter proximity of an
  absorbing cube — clients render the visual state locally
- Visual stages implemented as shader parameters on the cube mesh: color
  desaturation, edge blur radius, wall-merge blend factor
- Acoustic trace: a proximity AudioStreamPlayer3D attached to absorbing
  cubes (Stage 2+), emitting a subtle Brown Noise frequency shift,
  volume proportional to absorption stage
- On player return: absorption stage resets to 0, shader parameters
  reverse gradually over 1–2 hours of active play time — no notification
  issued, no UI acknowledgment
- Absorption stage and days-elapsed are never exposed to the client —
  the player has no way to know their current stage or how close they
  are to Day 40
- On Day 40: server deletes player record, removes cube from world
  state, logs sector coordinate as ‘previously absorbed’ — no client
  notification issued to any player

- Brown Noise bed: looping AudioStreamPlayer (global, non-spatial),
  routed through a bus with AudioEffectLowPassFilter at 200–400Hz
  cutoff, volume below conscious awareness
- Binaural Theta beat: two AudioStreamGenerators in GDScript, one per
  stereo channel, offset by 4–8Hz (e.g. 250Hz left / 256Hz right for a
  6Hz theta beat) — requires stereo headphones, which the headset
  mandate already ensures
- Footsteps: AudioStreamPlayer3D with AudioEffectReverb on Environment
  node
- Voice: Livekit SDK integrated into Godot client via WebSocket; spatial
  proximity falloff configured server-side; headset required (enforced
  at onboarding)

# **Safety & Moderation**

Because this game is built entirely around anonymous stranger
encounters, moderation is a primary design concern, not an afterthought.

- Automated bot monitors text chat for hate speech, harassment, and
  explicit content
- Voice reporting: players can flag voice interactions for asynchronous
  review
- Cubes can be “muted” — a player can silence another’s voice locally
  without affecting others
- No usernames are public-facing; players share identifiers voluntarily
  in conversation
- No player location data is exposed to other players except during the
  Event Horizon aerial view

# **Development Roadmap**

## **Phase 1 — Single-Player Prototype**

*Deliverable: The world feels right before any networking.*

- Marble corridor geometry, cube avatar, basic movement
- Skybox linked to system clock and weather API
- Ambient drone audio and spatial footstep reverb
- Minimalist log window UI with mood color system

## **Phase 2 — Multiplayer Skeleton**

*Deliverable: Two cubes can see and hear each other.*

- Godot multiplayer API integration
- Player position synchronization
- Spatial voice chat (proximity-based)
- Server-side player registry

## **Phase 3 — Living Maze**

*Deliverable: The maze grows with players.*

- Procedural sector generation on server
- Geometry streaming to clients
- Player count trigger for growth events

## **Phase 4 — Absence & Absorption**

*Deliverable: The maze remembers who left.*

- Server-side last-login tracker and daily absorption stage job
- Cube shader with desaturation, edge blur, and wall-merge blend
  parameters
- Acoustic trace AudioStreamPlayer3D on absorbing cubes
- Day 40 permanent deletion and account wipe
- Idle cube persistence after disconnect (cube remains in world for
  session duration then freezes at last position)

## **Phase 6 — The Event Horizon**

*Deliverable: The full teleport sequence is functional.*

- Obsidian pool placement algorithm
- Aerial view (camera rig + player light points)
- Black Mirror + Terminal UI sequence
- Server-side teleport logic
- Dual-stone conscious exit: simultaneous activation of both stones
  triggers permanent account deletion and cube dissolution — no
  confirmation prompt, no animation, no record left in the world
- Event Horizon relocation after use

## **Phase 7 — Mystery & Tidal Systems**

*Deliverable: The maze feels alive and independent.*

- Singular wall configurations: flag ~10–20 unique sector seeds
  server-side as unrepeated anomalies
- Sound anomalies: coordinate-bound audio triggers with slow rotation
  schedule
- Tidal behavior: corridor width parameter tied to aggregate player
  time-of-day, updated hourly server-side
- Population contraction: idle-sector contraction timer, applied as seed
  delta after N weeks without visitors
- Shared Ground: server-side corridor traversal log per player pair;
  marble tone shift triggered on simultaneous presence

## **Phase 8 — Safety & Polish**

*Deliverable: Ready for closed beta.*

- Automated moderation bot
- Mute system, report system
- Performance optimization, LOD, stress testing
- Closed beta with invited players

# **Notes for Developer Collaborators**

This project is the work of a conceptual designer and idea builder. The
creative vision is fully formed; what is needed is a technical partner
who can translate this vision into Godot 4 GDScript and server
architecture.

Key qualities sought in a collaborator:

- Experience with Godot 4 — specifically multiplayer networking via ENet
  and GDScript audio generation
- Familiarity with Node.js or Python for the dedicated maze server
- Some experience with Livekit or comparable spatial voice
  infrastructure
- Familiarity with procedural generation (even basic implementation is
  acceptable at Phase 1)
- Patience with a design-led process — the aesthetic constraints are
  non-negotiable
- Interest in experimental / artistic game design over commercial game
  design

*The Infinite Maze is not trying to be a successful game. It is trying
to be a true one.*

■ ■ ■
