# Space Towers - Game Planning Document

A space-themed tower defense game built as a single-page browser application.

## Tech Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| Language | TypeScript | Type safety, better DX |
| Runtime | Node.js | Build tooling, dev server |
| Framework | React 18+ | UI components, state management |
| Build Tool | Vite | Fast HMR, optimized builds |
| Rendering | Canvas 2D API | Game rendering, performance |
| Styling | CSS Modules or Tailwind | UI styling |
| Testing | Vitest + React Testing Library | Unit/integration tests |

## Architecture Overview

```
src/
├── main.tsx                 # App entry point
├── App.tsx                  # Root component
├── components/              # React UI components
│   ├── Game.tsx            # Main game container
│   ├── HUD.tsx             # Heads-up display (resources, wave info)
│   ├── TowerPanel.tsx      # Tower selection/purchase UI
│   ├── GameOver.tsx        # End game screen
│   └── MainMenu.tsx        # Start screen
├── game/                    # Game engine (pure TypeScript, no React)
│   ├── Engine.ts           # Main game loop, canvas management
│   ├── Map.ts              # Map/path definitions
│   ├── Tower.ts            # Tower base class and types
│   ├── Enemy.ts            # Enemy base class and types
│   ├── Projectile.ts       # Projectile system
│   ├── Wave.ts             # Wave spawning logic
│   └── types.ts            # Shared game types
├── hooks/                   # React hooks
│   └── useGameEngine.ts    # Hook to bridge React <-> Game Engine
├── assets/                  # Static assets (sprites, sounds)
└── utils/                   # Shared utilities
```

## Core Game Mechanics

### Game Loop

1. **Update Phase**: Process game state (towers fire, enemies move, projectiles travel)
2. **Render Phase**: Draw current state to canvas
3. **Target**: 60 FPS using `requestAnimationFrame`

### Economy

- **Starting Credits**: 100
- **Earn Credits**: Destroying enemies
- **Spend Credits**: Building/upgrading towers
- **Lives**: Start with 20, lose 1 per enemy reaching the end

### Towers (MVP: 4 Types)

| Tower | Cost | Damage | Range | Fire Rate | Special |
|-------|------|--------|-------|-----------|---------|
| **Laser Turret** | 50 | 10 | Medium | Fast | Basic, reliable |
| **Plasma Cannon** | 100 | 40 | Medium | Slow | High single-target damage |
| **Tesla Coil** | 150 | 15 | Short | Medium | Chain lightning (hits 3 enemies) |
| **Missile Battery** | 200 | 25 | Long | Slow | Splash damage |

### Enemies (MVP: 4 Types)

| Enemy | HP | Speed | Reward | Special |
|-------|-----|-------|--------|---------|
| **Scout Drone** | 30 | Fast | 10 | Basic enemy |
| **Assault Bot** | 80 | Medium | 25 | Standard enemy |
| **Heavy Mech** | 200 | Slow | 50 | Tanky |
| **Swarm** | 15 | Very Fast | 5 | Spawns in groups of 5 |

### Wave System

- Waves increase in difficulty (more enemies, stronger types)
- Short break between waves for tower placement
- Boss wave every 5 waves (special enemy with high HP)

### Map

- Single map for MVP
- Pre-defined path that enemies follow
- Grid-based tower placement (towers cannot block path)
- Visual: Space station corridor/platform aesthetic

## State Management

### Game State (managed by Engine)

```typescript
interface GameState {
  phase: 'menu' | 'playing' | 'paused' | 'wave-break' | 'game-over';
  credits: number;
  lives: number;
  wave: number;
  towers: Tower[];
  enemies: Enemy[];
  projectiles: Projectile[];
  selectedTower: TowerType | null;
}
```

### React Integration

- Game engine runs independently from React render cycle
- React components observe game state via hooks
- UI actions (buy tower, pause) dispatch to engine
- Canvas rendering handled entirely by engine

## MVP Feature List

### Phase 1: Foundation
- [ ] Project setup (Vite + React + TypeScript)
- [ ] Basic canvas rendering
- [ ] Game loop with fixed timestep
- [ ] Map with defined path
- [ ] Enemy pathfinding along path

### Phase 2: Core Gameplay
- [ ] Tower placement system
- [ ] Tower targeting and firing
- [ ] Projectile system
- [ ] Enemy damage and destruction
- [ ] Credit economy

### Phase 3: Game Flow
- [ ] Wave spawning system
- [ ] Lives and game over
- [ ] Wave break intervals
- [ ] All 4 tower types
- [ ] All 4 enemy types

### Phase 4: UI/UX
- [ ] HUD (credits, lives, wave number)
- [ ] Tower selection panel
- [ ] Main menu
- [ ] Game over screen
- [ ] Pause functionality

### Phase 5: Polish
- [ ] Basic sound effects
- [ ] Visual feedback (damage, explosions)
- [ ] Tower range indicators
- [ ] Enemy health bars
- [ ] Wave preview

## Future Enhancements (Post-MVP)

- Tower upgrades (3 levels per tower)
- Multiple maps
- Endless mode
- Local high scores (localStorage)
- Tower special abilities
- More enemy types (shields, flying, spawners)
- Difficulty settings

## Development Guidelines

### Code Quality
- Strict TypeScript (`strict: true`)
- ESLint for code style
- Prettier for formatting
- All game logic must be unit testable (no DOM dependencies in `game/`)

### Performance Targets
- 60 FPS with 50+ enemies on screen
- < 100ms initial load time (code-split if needed)
- Efficient collision detection (spatial partitioning if needed)

### Testing Strategy
- Unit tests for game logic (towers, enemies, wave spawning)
- Integration tests for React components
- Manual playtesting for balance

## Getting Started

```bash
# Install dependencies
npm install

# Start dev server
npm run dev

# Run tests
npm test

# Build for production
npm run build
```

## Design Notes

### Visual Style
- Dark space background with stars
- Neon/holographic UI elements
- Glowing projectiles and effects
- Grid overlay for tower placement

### Color Palette
- Background: Deep space blue (#0a0a1a)
- UI Accent: Cyan (#00ffff)
- Danger: Red (#ff3366)
- Success: Green (#00ff88)
- Credits: Gold (#ffcc00)
