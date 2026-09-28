# REFACTOR STAGE 1: Safe Copy-Paste Patch

**Goal**: Extract all magic numbers into a CONFIG object. Zero behavior changes.

## Instructions

1. Open `index (2).html` in your editor
2. Find the line: `const TAU=Math.PI*2, clamp=...` (around line 88)
3. **DELETE** everything from `const TAU=...` through `const TEAM_COLORS=["#3b82ff","#ef4444"];` (lines 88–204)
4. **PASTE** the entire content below at that location
5. Save and test in browser. The game should play identically.

---

## Copy-Paste Content Below

```javascript
/* ============================================
   CONFIGURATION: All Magic Numbers & Constants
   ============================================ */

const CONFIG = {
  /* Arena Layout */
  arena: {
    width: 1800,
    height: 1100,
    fighterRadius: 28,
    boundaryPadding: 55,
    centerX: 900,
    centerY: 550,
    spawnPoints: [
      [330, 300],   // Team 0, Fighter 0
      [330, 800],   // Team 0, Fighter 1
      [1470, 300],  // Team 1, Fighter 0
      [1470, 800]   // Team 1, Fighter 1
    ],
    nextRoundSpawns: [
      [260, 380],   // Team 0, Fighter 0
      [260, 720],   // Team 0, Fighter 1
      [1540, 380],  // Team 1, Fighter 0
      [1540, 720]   // Team 1, Fighter 1
    ],
    obstacles: [
      { x: 760, y: 150, w: 280, h: 55 },
      { x: 760, y: 895, w: 280, h: 55 },
      { x: 260, y: 430, w: 80, h: 240 },
      { x: 1460, y: 430, w: 80, h: 240 },
      { x: 585, y: 325, w: 85, h: 85 },
      { x: 1130, y: 690, w: 85, h: 85 },
      { x: 860, y: 500, w: 80, h: 100 }
    ]
  },

  /* Fighter Style Stats */
  styles: {
    "Kung Fu": {
      speed: 190,
      range: 70,
      damage: 8,
      heavy: 18,
      hp: 100,
      attackRate: 0.32,
      color: "#35e67a"
    },
    "Wrestler": {
      speed: 105,
      range: 74,
      damage: 12,
      heavy: 30,
      hp: 135,
      attackRate: 0.65,
      color: "#f0b52d"
    },
    "Boxer": {
      speed: 145,
      range: 125,
      damage: 10,
      heavy: 27,
      hp: 110,
      attackRate: 0.52,
      color: "#3297ff"
    },
    "Brawler": {
      speed: 125,
      range: 82,
      damage: 16,
      heavy: 38,
      hp: 118,
      attackRate: 0.75,
      color: "#ff3d3d"
    },
    "Martial Arts": {
      speed: 155,
      range: 100,
      damage: 12,
      heavy: 25,
      hp: 108,
      attackRate: 0.48,
      color: "#b65cff"
    }
  },

  /* Combat System Mechanics */
  combat: {
    hpMultiplier: 1.35,
    
    // Attack windows (time before attacking is allowed)
    baseAttackWindow: {
      "Kung Fu": 0.16,
      "Boxer": 0.18,
      "Wrestler": 0.22,
      "Brawler": 0.24,
      "Martial Arts": 0.18,
      default: 0.2
    },

    // Attack cooldown (recovery after attacking)
    recoveryGap: {
      "Kung Fu": 0.88,
      "Boxer": 0.78,
      "Wrestler": 1.16,
      "Brawler": 1.04,
      "Martial Arts": 0.92,
      default: 0.95
    },

    // Minimum damage per move (prevents harmless pokes)
    floorDamage: {
      JAB: 18,
      PALM: 20,
      MIXED: 20,
      COUNTER: 24,
      CROSS: 26,
      KICK: 29,
      "SPIN KICK": 44,
      HEAVY: 31,
      RUSH: 27,
      GRAPPLE: 34,
      FIREBALL: 38,
      "SONIC BLAST": 38,
      SHOCKWAVE: 41,
      "ICE FREEZE": 39,
      "SEISMIC SLAM": 44,
      "FAULTLINE": 38,
      "SCRAP CANNON": 42,
      "VOID NEEDLE": 46,
      "JUMP KICK": 36,
      "SUPER PUNCH": 40,
      "IRON CROSS": 35,
      "POWER SLAM": 44,
      "GROUND BREAKER": 46,
      "SHADOW FLURRY": 42
    },

    // Cooldowns
    specialCooldown: 1.85,
    
    // Knockdown mechanics
    knockdownBaseDuration: 0.72,
    knockdownDamageScaling: 0.72,
    knockdownChanceSpecial: 0.34,
    knockdownChanceSpinKick: 0.26,
    knockdownChanceHeavy: 0.18,
    
    // Guard mechanics
    guardBlockPercent: 0.38,
    grapplePinDuration: 0.85,
    grappleDuration: 0.50,
    
    // Counter window (DUEL mode only)
    counterWindowDuration: 0.34,
    
    // Combo system
    maxComboHits: 4,
    comboCooldownDuel: 1.55,
    comboCooldownBrawl: 1.15,
    
    // Frozen effect (ice projectile)
    frozenBaseDuration: 1.1,
    frozenDamageScaling: 0.018,
    frozenMaxDuration: 2.4,
    
    // Partial decapitation
    headDetachChanceSpinKick: 0.30,
    headDetachChanceSpecial: 0.18,
    headDetachChanceNormal: 0.12,
    headDetachHpThreshold: 0.28,
    
    // Team combo damage multipliers
    teamComboMultiplierBrawl: 1.58,
    teamComboMultiplierDuel: 1.35,
    
    // Finisher mechanics
    finisherHpThreshold: 0.32,
    finisherDamageReduction: 0.16,
    
    // Accuracy multipliers (for specific styles)
    precisionCounterBoost: 1.18,
    boxerCounterBoost: 1.5,
    martialArtsCounterBoost: 1.4,
    kungFuKickBoost: 1.2,
    
    // Knockback mechanics
    heavyKnockbackThreshold: 24,
    heavyKnockbackForce: 0.5,
    lightKnockbackForce: 0.82,
    knockbackBaseForce: 90,
    knockbackDamageScaling: 3.4,
    lightKnockbackBase: 25,
    lightKnockbackScaling: 1.4,
    
    // Distance thresholds
    distanceRangeExtension: 62,
    distanceAttackSpecialThreshold: 680,
    distanceDuelCounterReady: 230,
    distanceDuelCloseFighting: 420,
    distanceDuelClosePositioning: 170,
    distanceCloseRangeThreshold: 145,
    distanceFarThreshold: 150
  },

  /* DUEL vs BRAWL Combat Modes */
  combatMode: {
    defaultMode: "DUEL",
    
    // Phase durations
    duelBaseDuration: 5.6,
    duelDurationVariance: 3.6,
    brawlBaseDuration: 4.2,
    brawlDurationVariance: 2.2,
    
    // AI targeting
    brawlConvergenceChance: 0.78,
    
    // Attack frequency multipliers
    brawlAttackSpeedMultiplier: 0.60,
    duelAttackSpeedMultiplier: 1.18,
    comboAttackSpeedMultiplierDuel: 0.62,
    
    // Special move chances
    brawlSpecialChance: 0.64,
    duelCounterSpecialChance: 0.66,
    duelComboStage3Chance: 0.72,
    duelComboStage1to2Chance: 0.10,
    duelNoComboChance: 0.24,
    
    // Combat positioning gaps
    duelGapMinimum: 92,
    duelGapMaximum: 142,
    duelGapRangeMultiplier: 0.86,
    brawlGapMinimum: 62,
    brawlGapMaximum: 104,
    brawlGapRangeMultiplier: 0.68,
    
    // Speed adjustments
    duelCloseRangeSpeedMult: 0.68,
    brawlCloseRangeSpeedMult: 0.82,
    farRangeSpeedBoost: 1.18,
    guardSpeedMultiplier: 0.62,
    attackSpeedMultiplier: 0.48,
    seekCloseRangeSpeedMult: 0.35,
    
    // Feint (fake attack) mechanics
    feintTriggerChance: 0.035,
    feintDuration: 0.22,
    
    // Duel-specific AI
    dodgeChanceKungFu: 0.5,
    feintChanceKungFu: 0.5
  },

  /* Camera Settings */
  camera: {
    defaultX: 900,
    defaultY: 550,
    defaultZoom: 0.62,
    minZoom: 0.38,
    maxZoom: 0.85,
    zoomSpanMinimum: 500,
    zoomSpanPaddingX: 360,
    zoomSpanPaddingY: 260,
    zoomRatioPadding: 0.67,
    lerpDamping: 0.001
  },

  /* Visual Effects */
  fx: {
    // Screen shake
    shakeDecayRate: 30,
    
    // Particles
    impactSparkCountSmall: 10,
    impactSparkCountLarge: 24,
    impactDebrisCountSmall: 3,
    impactDebrisCountLarge: 8,
    particleFriction: 0.96,
    particleLifeSmallMin: 0.25,
    particleLifeSmallMax: 0.55,
    particleLifeLargeMin: 0.4,
    particleLifeLargeMax: 0.8,
    particleSizeSmallMin: 1.5,
    particleSizeSmallMax: 2.5,
    particleSizeLargeMin: 3,
    particleSizeLargeMax: 7,
    
    // Impact rings
    ringExpandSpeed: 9,
    ringDuration: 0.45,
    
    // Floating text
    floatRiseSpeed: 28,
    
    // Detached head physics
    headGravity: 720,
    headFrictionExponent: 0.985,
    headRotationFrictionExponent: 0.995,
    headBounceElasticity: 0.46,
    headBounceFriction: 0.76,
    headRotationBounceFriction: 0.92,
    
    // Projectiles
    projectileSpeedIce: 430,
    projectileSpeedQuake: 390,
    projectileSpeedVoid: 610,
    projectileSpeedDefault: 520,
    projectileLifetime: 1.7,
    projectileHitRadius: 30
  },

  /* Hit Stop (Freeze Frame) Durations */
  hitStop: {
    light: 0.045,
    medium: 0.065,
    heavy: 0.075,
    special: 0.11,
    precisionCounter: 0.13,
    block: 0.035,
    doubleTeamStart: 0.10,
    doubleTeamFinal: 0.24,
    modeSwitch: 0.06
  },

  /* Screen Shake Intensities */
  shake: {
    lightHit: 4,
    mediumHit: 7,
    heavyHit: 9,
    specialHit: 12,
    knockdown: 15,
    finisherStart: 24,
    finisherPhase1: 34,
    finisherPhase2: 42,
    doubleTeamEntry: 30,
    doubleTeamPhase1: 26,
    doubleTeamPhase2: 34,
    doubleTeamFinal: 42,
    headDetach: 21,
    headBounce: 8,
    projectileIce: 9,
    projectileDefault: 13,
    brawlModeSwitch: 18
  },

  /* Match Timing (milliseconds) */
  timing: {
    introDelay: 2000,
    finisherTotalDuration: 2400,
    finisherPhase1Delay: 1250,
    finisherPhase2Delay: 1900,
    teamAssaultTotalDuration: 2700,
    teamAssaultPhase1Delay: 420,
    teamAssaultPhase2Delay: 900,
    teamAssaultPhase3Delay: 1450,
    teamAssaultPhase4Delay: 2050,
    victoryDisplayDuration: 3200,
    centerMessageDuration: 600,
    modeAnnouncementDuration: 700,
    finisherTextClearTime: 850
  },

  /* AI Decision Making */
  ai: {
    // Target selection refresh rates
    targetRefreshStageBase: 1.15,
    targetRefreshBrawlBase: 0.55,
    targetRefreshDuel: 1.8,
    
    // Enemy priority scoring
    damageWeightPercent: 0.45,
    isolatedEnemyWeight: 55,
    behindEnemyWeight: 15,
    
    // Movement interpolation
    velocityDamping: 7,
    rotationDamping: 10,
    rotationFastDamping: 8,
    staggerRotDamping: 8,
    staggerVelocityDamping: 12,
    bodyLeanMaximum: 0.14,
    bodyLeanDamping: 7,
    
    // Combat rhythm
    stepPhaseScaling: 0.055,
    walkSpeedThreshold: 8,
    walkAnimationBoost: 1.5,
    
    // Physical separation
    separationMultiplier: 0.52,
    separationTeamDrag: 0.82,
    separationMinDistance: 0.01
  },

  /* UI and Logging */
  logging: {
    maxLogEntries: 5000,
    scrollAutoThreshold: 30,
    enableAIDebugging: true
  },

  /* Team Configuration */
  teams: {
    teamCount: 2,
    fightersPerTeam: 2,
    colors: ["#3b82ff", "#ef4444"],
    fighterNames: {
      0: ["K-9", "SABLE"],
      1: ["BRUISER", "VOLT"]
    }
  },

  /* Round and Match Rules */
  rounds: {
    winsRequiredToWinBout: 2,
    maxRoundsInBout: 3
  }
};

/* ============================================
   UTILITY FUNCTIONS
   ============================================ */

const TAU = Math.PI * 2;

/**
 * Clamp a value between a minimum and maximum.
 * @param {number} value - The value to clamp
 * @param {number} minimum - Minimum bound
 * @param {number} maximum - Maximum bound
 * @returns {number} - Clamped value
 */
function clamp(value, minimum, maximum) {
  return Math.max(minimum, Math.min(maximum, value));
}

/**
 * Linear interpolation between two values.
 * @param {number} a - Start value
 * @param {number} b - End value
 * @param {number} t - Interpolation factor (0-1)
 * @returns {number} - Interpolated value
 */
function lerp(a, b, t) {
  return a + (b - a) * t;
}

/**
 * Linear interpolation between two angles (accounts for wrapping).
 * @param {number} a - Start angle (radians)
 * @param {number} b - End angle (radians)
 * @param {number} t - Interpolation factor (0-1)
 * @returns {number} - Interpolated angle
 */
function lerpAngle(a, b, t) {
  return a + normalizeAngle(b - a) * t;
}

/**
 * Calculate Euclidean distance between two points.
 * @param {Object} pointA - Point {x, y}
 * @param {Object} pointB - Point {x, y}
 * @returns {number} - Distance
 */
function distance(pointA, pointB) {
  return Math.hypot(pointA.x - pointB.x, pointA.y - pointB.y);
}

/**
 * Calculate angle from point A to point B.
 * @param {Object} pointA - Origin {x, y}
 * @param {Object} pointB - Target {x, y}
 * @returns {number} - Angle in radians
 */
function angle(pointA, pointB) {
  return Math.atan2(pointB.y - pointA.y, pointB.x - pointA.x);
}

/**
 * Normalize an angle to be within -π to π range.
 * @param {number} angleRadians - Angle in radians
 * @returns {number} - Normalized angle
 */
function normalizeAngle(angleRadians) {
  return Math.atan2(Math.sin(angleRadians), Math.cos(angleRadians));
}

/**
 * Generate a random number between min (inclusive) and max (exclusive).
 * @param {number} min - Minimum value
 * @param {number} max - Maximum value
 * @returns {number} - Random value
 */
function random(min, max) {
  return min + Math.random() * (max - min);
}

/**
 * Randomly select an element from an array.
 * @param {Array} array - Array to pick from
 * @returns {*} - Random element
 */
function randomPick(array) {
  return array[(Math.random() * array.length) | 0];
}

/**
 * Escape special HTML characters for safe DOM insertion.
 * @param {string} str - String to escape
 * @returns {string} - Escaped string
 */
function escapeHtml(str) {
  return String(str).replace(/[&<>"]/g, (char) => ({
    "&": "&amp;",
    "<": "&lt;",
    ">": "&gt;",
    '"': "&quot;"
  }[char]));
}

/**
 * Separate a circle from a rectangle using collision response.
 * Used for pushing fighters away from arena obstacles.
 * @param {Object} circleCenter - Circle center {x, y}
 * @param {number} circleRadius - Circle radius
 * @param {Object} rect - Rectangle {x, y, w, h}
 * @returns {boolean} - True if collision was resolved
 */
function circleRectanglePush(circleCenter, circleRadius, rect) {
  // Find closest point on rectangle to circle center
  const closestX = clamp(circleCenter.x, rect.x, rect.x + rect.w);
  const closestY = clamp(circleCenter.y, rect.y, rect.y + rect.h);
  
  // Calculate distance and direction
  let deltaX = circleCenter.x - closestX;
  let deltaY = circleCenter.y - closestY;
  const distanceToRect = Math.hypot(deltaX, deltaY);
  
  // Check for collision
  if (distanceToRect < circleRadius) {
    if (distanceToRect < 0.001) {
      // Ambiguous corner case: push along closest edge
      const distToLeft = Math.abs(circleCenter.x - rect.x);
      const distToRight = Math.abs(circleCenter.x - (rect.x + rect.w));
      const distToTop = Math.abs(circleCenter.y - rect.y);
      const distToBottom = Math.abs(circleCenter.y - (rect.y + rect.h));
      const minDist = Math.min(distToLeft, distToRight, distToTop, distToBottom);

      if (minDist === distToLeft) {
        circleCenter.x = rect.x - circleRadius;
      } else if (minDist === distToRight) {
        circleCenter.x = rect.x + rect.w + circleRadius;
      } else if (minDist === distToTop) {
        circleCenter.y = rect.y - circleRadius;
      } else {
        circleCenter.y = rect.y + rect.h + circleRadius;
      }
    } else {
      // Normal case: push away from closest point
      circleCenter.x = closestX + (deltaX / distanceToRect) * circleRadius;
      circleCenter.y = closestY + (deltaY / distanceToRect) * circleRadius;
    }
    return true;
  }
  return false;
}

/* ============================================
   ROBOT CLASS
   ============================================ */

/**
 * Fighter robot in the arena.
 * 
 * State Machine:
 *   IDLE → SEEK → (ATTACK | GRAPPLE | COUNTER | GUARD | EVADE)
 *   
 *   - IDLE: Not yet spawned or knocked down
 *   - SEEK: Approaching opponent
 *   - ATTACK: Executing an attack sequence
 *   - GRAPPLE: Attempting or being grappled
 *   - COUNTER: Responding to opponent attack
 *   - GUARD: Defensive stance
 *   - EVADE: Dodging or backing away
 *   - RETREAT: Frozen or severely disadvantaged
 * 
 * @class
 * @param {string} name - Fighter name (e.g., "K-9", "BRUISER")
 * @param {number} team - Team index (0 = Blue, 1 = Red)
 * @param {string} style - Fighting style (must exist in CONFIG.styles)
 * @param {number} spawnIndex - Spawn point index (0-3)
 */
class Robot {
  constructor(name, team, style, spawnIndex) {
    const styleStats = CONFIG.styles[style];
    const [spawnX, spawnY] = CONFIG.arena.spawnPoints[spawnIndex];

    /* ====== IDENTITY ====== */
    this.name = name;
    this.team = team;
    this.style = style;

    /* ====== POSITION & VELOCITY ====== */
    this.x = spawnX;
    this.y = spawnY;
    this.vx = 0;
    this.vy = 0;
    this.angle = team === 0 ? 0 : Math.PI;

    /* ====== HEALTH ====== */
    this.hp = styleStats.hp * CONFIG.combat.hpMultiplier;
    this.maxHp = styleStats.hp * CONFIG.combat.hpMultiplier;
    this.radius = CONFIG.arena.fighterRadius;

    /* ====== CORE STATE MACHINE ====== */
    this.state = "IDLE";
    this.stateTime = 0;
    this.target = null;

    /* ====== ACTION TIMERS & COOLDOWNS ====== */
    this.cool = 0;              // Attack cooldown (time before next attack allowed)
    this.attack = 0;            // Current attack duration remaining
    this.attackKind = "";       // Name of attack being performed
    this.special = 0;           // Special move cooldown
    this.guardTime = 0;         // Active guard duration
    this.guard = 0;             // Guard state flag (0 or 1)

    /* ====== ATTACK DETAILS ====== */
    this.attackTarget = null;   // Intended target
    this.attackDuration = 0;    // Total duration of attack animation
    this.attackPhase = 0;       // Current frame of attack (0 to duration)
    this.attackContact = 0;     // Frame when damage occurs
    this.attackBase = 0;        // Base damage before modifiers
    this.attackReach = 0;       // How far the attack extends
    this.attackSpecial = false; // Is this a special move?
    this.pendingHit = false;    // Hit has been calculated but not applied
    this.attackResolved = false; // Hit has been fully resolved

    /* ====== DEFENSE & REACTIONS ====== */
    this.dodge = 0;             // Dodge duration (evasion active)
    this.dodgeDir = 0;          // Dodge direction (-1, 0, or 1)
    this.hitFlash = 0;          // Hit flash animation duration
    this.hitReact = 0;          // Hit reaction (knockback) duration
    this.hitDir = 0;            // Direction of incoming hit
    this.stagger = 0;           // Stagger (stumble) duration
    this.recoil = 0;            // Recoil from landing heavy attack
    this.grapple = 0;           // Grapple engagement state
    this.pinned = 0;            // Pinned in grapple duration

    /* ====== KNOCKDOWN & STATUS ====== */
    this.knockdown = 0;         // Knockdown duration (on ground)
    this.knockdownDir = 0;      // Direction of spin during knockdown
    this.knockdownSpin = 0;     // Current spin rotation
    this.frozen = 0;            // Frozen/ice effect duration
    this.invuln = 0;            // Invulnerability frames (during cinematics)

    /* ====== ARMOR & SPECIAL STATES ====== */
    this.headDetached = false;  // Head assembly partially detached
    this.partialDecap = 0;      // Partial decap animation duration
    this.disorient = 0;         // Disorientation from head damage
    this.headSwing = 0;         // Direction head swings

    /* ====== MOVEMENT & ANIMATION ====== */
    this.walk = 0;              // Walk animation state
    this.stepPhase = 0;         // Step cycle phase
    this.speedNow = 0;          // Current velocity magnitude
    this.bodyLean = 0;          // Body lean (forward/backward tilt)

    /* ====== COMBO SYSTEM ====== */
    this.comboHits = 0;         // Current combo hit count (0-4)
    this.comboTimer = 0;        // Time remaining to extend combo
    this.comboChain = 0;        // Combo chain counter
    this.feint = 0;             // Feint (fake attack) duration

    /* ====== COUNTER MECHANICS ====== */
    this.counterWindow = 0;     // Time remaining to execute counter
    this.counterFlash = 0;      // Counter success flash effect

    /* ====== STATISTICS ====== */
    this.damageDone = 0;        // Total damage dealt this match
    this.damageTaken = 0;       // Total damage received this match
    this.roundDamage = 0;       // Damage dealt this round
    this.attempts = 0;          // Total attacks attempted
    this.landed = 0;            // Attacks that connected
    this.missed = 0;            // Attacks that missed
    this.kos = 0;               // Number of KOs achieved

    /* ====== STATE TRACKING (for debugging) ====== */
    this.states = {
      IDLE: 0,      // Seconds spent idle
      SEEK: 0,      // Seconds spent seeking/approaching
      ATTACK: 0,    // Seconds spent attacking
      EVADE: 0,     // Seconds spent evading
      RETREAT: 0,   // Seconds spent retreating
      GRAPPLE: 0,   // Seconds spent grappling
      COUNTER: 0    // Seconds spent countering
    };

    this.lastReason = "Booting tactical system";
    this.lastHit = -9;

    /* ====== MISCELLANEOUS ====== */
    this.poseSeed = Math.random();  // Random seed for pose variation
    this.specialType = "";          // Name of current special move
    this.airborne = 0;              // Is this attack airborne?
  }

  /**
   * Check if this robot is alive.
   * @returns {boolean} - True if hp > 0
   */
  alive() {
    return this.hp > 0;
  }

  /**
   * Change the robot's state with a reason for logging.
   * Only changes if newState differs from current state.
   * 
   * @param {string} newState - New state name (must be key in this.states)
   * @param {string} reason - Reason for state change (logged for debugging)
   */
  setState(newState, reason = "") {
    if (this.state !== newState) {
      this.state = newState;
      this.stateTime = 0;
      this.lastReason = reason;
      logAI(this);
    }
  }
}

/* ============================================
   TEAM COLORS & GLOBAL CONSTANTS
   ============================================ */

const TEAM_COLORS = CONFIG.teams.colors;
```

---

## Verification Checklist

After pasting, verify:

- [ ] Game starts without errors
- [ ] Fighters spawn at correct locations
- [ ] Combat feels identical to original
- [ ] No console errors
- [ ] LOG output matches original logging

---

## Next Stage (Not Done Yet)

Once this stage is working, Stage 2 will:
- Extract all AI decision logic
- Extract all combat resolution logic
- Extract all rendering functions
- Add comprehensive JSDoc comments throughout

**Do NOT proceed to Stage 2 until Stage 1 is tested and working.**
