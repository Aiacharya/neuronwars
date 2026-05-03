# Neuron Wars — YouTube Playable Game (MVP)

## Goal
Build a lightweight State.io-style strategy game, deliverable as a single
self-contained HTML file for YouTube Playables certification. Player vs AI
node-conquest with a powers system.

## Hard constraints
- Single `index.html`, target <500KB, hard ceiling 2MB
- Zero external resources: no CDN, no Google Fonts, no remote assets
- Vanilla HTML/CSS/JS only — no frameworks, no build tools
- Mobile-first, touch input, must hit 60 FPS on mid-range Android
- Match length: 60 seconds

## Workspace structure

```
neuron-wars/
├── README.md          # full spec (this document)
├── index.html         # the deliverable, evolves through phases
└── notes/
    ├── ai-spec.md     # threat-scoring formulas + tuning notes
    └── test-plan.md   # FPS, load time, file size checks
```

## Locked design spec

### Map (8 nodes)
- 2 player start (bottom)
- 2 AI start (top)
- 2 power nodes — hex glyph, brighter halo, equidistant from both starts
- 2 neutral filler nodes

### Combat
- Fixed 50% troop send (no slider — keeps UX tight for 60s loop)
- Touch: drag from owned node to target node = send troops
- Production: linear ramp tied to node max capacity
- Capture: incoming troops > defenders = capture, surplus becomes garrison

### Powers (both player and AI)

| Power  | Effect                                             |
|--------|----------------------------------------------------|
| Surge  | Target node produces 3× for 5s                    |
| Shield | Target node immune to attacks for 3s              |
| Blitz  | All your nodes send 50% in one synchronized wave  |

### Power earning (hybrid)
- Time tick: +1 charge every 25s
- Power-node capture: +1 instant charge
- Inventory cap: 2 charges
- UX: three icon buttons at bottom. Tap icon to arm → tap target node → consume charge.

### Smart AI (every 1.5s)
Threat-scoring formula:

```
score = target_value × production_rate
      − target_troops × 1.2
      − distance × 0.05
      + frontline_bonus
      + player_weakness_bonus
```

Defensive override: if any AI node has enemy nearby with > my_troops + 15,
reinforce instead of attacking.

### AI power priority (deterministic, in order)
1. Threatened node? → Shield it
2. AI troop total ≥ player + 30 AND 2 charges held? → Blitz
3. Just captured a high-production node? → Surge it
4. Else hoard

---

## Build phases

### Phase 1 (v0.1) — START HERE
Create workspace + skeleton `index.html`:
- Canvas sized to viewport (mobile portrait)
- 8 hardcoded node positions (will iterate)
- Render nodes with owner colors + troop counts
- Game loop scaffold (`requestAnimationFrame` + delta time)
- Touch handler stub (logs taps to console, no logic)
- Inline CSS only, dark background, flat vector aesthetic
- NO combat, NO AI, NO powers yet

**Acceptance:** opens in browser, shows 8 colored circles with numbers, 60 FPS confirmed.

### Phase 2 (v0.2) — Combat
Drag-to-attack, particle troop flow, capture logic, production tick.
Dumb AI placeholder (attack nearest weakest).

### Phase 3 (v0.3) — Smart AI
Replace dumb AI with threat-scoring formula. Console-log AI decisions for tuning.

### Phase 4 (v0.4) — Powers
3 powers, charge system, AI power priority rules, bottom button UI.

### Phase 5 (v0.5) — Ship
Minify, run file-size pass, verify <500KB, FPS test on real Android device.

---

## Status

| Version | Phase     | Status  |
|---------|-----------|---------|
| v0.1    | Skeleton  | ✅ Done |
| v0.2    | Combat    | ✅ Done |
| v0.3    | Smart AI  | pending |
| v0.4    | Powers    | pending |
| v0.5    | Ship      | pending |
