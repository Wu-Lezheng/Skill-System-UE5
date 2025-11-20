# Skill-System-UE5
An RPG skill system built using Unreal Engine 5.5. It has 4 implemented skills and different types of enemies for demo purposes. The relevant files are listed in the **`latest release`** on the right.

## Skills Implemented

### Dual Breath: 
Creates an electric blast which slows enemies, followed by a wave of fire that launches forward and deals damage over time.
- Damage type: magical
- Can ignore spell immunity: false
- Dispel level needed: weak dispel
- Cooldown: 10s
- MP consumed: 135/150/165/180 (by levels)
- Burn damage: 20/40/60/80 (by levels); Burn tick: 1s
- Move slow: × 0.75/0.7/0.65/0.6 (by levels); Attack slow: - 25/30/35/40 (by levels)
- Duration: 5s

### Ice Path:
Creates a path of ice that stuns and damages enemies that touches it.
- Damage type: magical
- Can ignore spell immunity: false
- Dispel level needed: strong dispel
- Cooldown: 20/17/14/11s (by levels)
- MP consumed: 100
- Path formation delay: 0.2s; Path duration: 5/5.5/6/6.5s (by levels)
- Stun duration: 1.25/1.5/1.75/2.0s
- Damage: 50

### Desolate [passive]
Deals additional damage whenever an enemy without allied units within a radius is damaged. 
- Damage type: pure
- Can ignore spell immunity: true
- Dispel level needed: cannot be dispelled
- Damage: 25/40/55/70 (by levels)
- Radius: 350

### Aphotic Shield
Creates an all damage barrier that absorbs a set amount of damage and bursts when it expires or is destroyed, dealling damage equal to the amount absorbed. Has strong dispel effect upon activation.
- Damage type: magical
- Can ignore spell immunity: false
- Dispel level needed: weak dispel
- Cooldown: 12/10/8/6s (by levels)
- MP consumed: 110/120/130/140 (by levels)
- Duration: 12s
- Radius: 400
- Max damage to absorb: 120/150/180/210 (by levels)

## Enemies
- A stationary enemy
- A moving enemy
- An insolated enemy
- An enemy shooting projectiles that deals damage to the player on hit
- An enemy shooting projectiles that inflicts burn (weak dispel needed to remove), dealing damage over time
- An enemy shooting projectiles that inflicts stun (strong dispel needed to remove), disabling player movement

## System Architecture
- **Skill:** Reads skill information from data table upon creation; can be attached to character mesh sockets and spawn/destroy/control skill effects [e.g., BP_DualBreath]
- **Skill Effect:** Results in various outcomes on some target(s), such as damage and status effects, based on certain condition(s), such as collision [e.g., BP_FireWall]
- **Status:** Data object; contains information such as dispel level [e.g., Burn]
- **Skill Manager (actor component):** Manages the creation, addition, and removal of skills for characters
- **Tag Manager (actor component):** Manages the identity and identity comparison for characters, such as enemy factions
- **Status Manager (actor component):** Manages the creation, addition, update, and removal of status for characters
- **Effect Provider (blueprint interface):** Implemented by entities that can cause effects on others, such as the player, enemies, and skills; usually for passive effects such as `OnDealDamage`, `OnReceiveDamage`, and `OnInflictBurn`
- **Effect Receiver (blueprint interface):** Implemented by entities that can receive effects from others; usually for toggling status effects and dealing damage

## Utils
- Skills acquire points for the player to gain skills
- Skill upgrade point to level up all possessed skills
- HP restore point to get back full HP
- MP restore point to get back full MP
- HUD to display player state and control
