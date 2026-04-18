# BeKindRewind Store - Unified Implementation Spec

## ROOM GEOMETRY & LAYOUT

### Dimensions (FINAL)
- **Width**: 60 units (X: -30 to +30)
- **Depth**: 40 units (Z: -20 to +20)
- **Ceiling Height**: 4.5 units (Y: 0 to 4.5)
- **Floor**: Y = 0

### Player Spawn & Movement
- **Start Position**: Z = 35 (outside store facing entrance)
- **Start Direction**: Facing negative Z (into store)
- **Player Height**: 1.7 units (camera Y position)
- **Walk Speed**: 0.04 units per frame
- **Player Collision Radius**: 0.4 units
- **Mouse Sensitivity**: 0.001
- **Collision Method**: Sliding (test X and Z axes separately)
- **Camera Bob**: Sine wave while moving (subtle amplitude)

### Entrance & Zones
- **Front Clear Zone**: No shelves in first 4 units (Z: -20 to -16)
- **Checkout Counter**: Left side near entrance, low desk (simple box geometry)
- **Shelf Layout**: 6 aisles running front-to-back, double-sided facing each other

## SHELF SYSTEM

### Aisle Shelves (6 total)
**Positions**: Evenly spaced at X = [-25, -15, -5, 5, 15, 25] (10-unit intervals)

**Physical Dimensions Per Aisle:**
- **Height**: 1.5 units max (not towering)
- **Depth**: 0.4 units (extends from shelf center ±0.2)
- **Length**: Runs front-to-back, Z = -14 to +18 (32 units)
- **Walkway Between Aisles**: 2 units (5 units per aisle spacing - 1.5 width = 3.5 sides = ~2 walkway)

**Shelf Levels (3 per aisle)**: Y = [0.3, 0.9, 1.5]

**Geometry Structure Per Aisle:**
- Support posts at Z = [-10, -2, 6, 14] (vertical elements, dark gray)
- 3 horizontal wooden shelves at each level (warm wood #8B5E3C)
- Dark navy backing panel (#003087) behind shelf unit
- Shelf width: 1.5 units wide (accommodates the box cases)

### Wall Shelves

**Back Wall** (Z = +20, shelf front):
- Runs full width X = [-30 to +30]
- 3 shelf levels: Y = [0.3, 0.9, 1.5]
- Depth: 0.4 units
- Navy backing panels
- Wooden shelves

**Left Wall** (X = -30, shelf front facing right):
- Runs full length Z = [-20 to +20]
- 3 shelf levels: Y = [0.3, 0.9, 1.5]
- Depth: 0.4 units
- Navy backing panels
- Wooden shelves

**Right Wall** (X = +30, shelf front facing left):
- Runs full length Z = [-20 to +20]
- 3 shelf levels: Y = [0.3, 0.9, 1.5]
- Depth: 0.4 units
- Navy backing panels
- Wooden shelves

## DVD CASE PLACEMENT

### Box Geometry Specifications
- **Dimensions**: 0.38W × 0.55H × 0.05D units
- **Placement**: Packed tightly side-by-side on shelves
- **Gap Between Cases**: 0.01 units
- **Orientation**: Upright (vertical on shelf surface)
- **Y Position on Shelf**: Shelf level + 0.275 (half case height 0.55/2, to sit on shelf)

### Color Assignment by Genre
Genres (in order placed on aisles): Horror, Drama, Action, Thriller, Sci-Fi, Comedy, Animation, Romance, Documentary, Western

Base colors with ±20% random brightness variation per case:
- **Horror**: #4a0a0a (dark red)
- **Thriller**: #0a0a4a (dark blue)
- **Action**: #4a2a0a (dark orange)
- **Drama**: #3a3a0a (dark gold)
- **Sci-Fi**: #0a3a3a (dark teal)
- **Comedy**: #3a3a00 (dark yellow)
- **Romance**: #4a0a2a (dark pink)
- **Animation**: #0a3a0a (dark green)
- **Documentary**: #2a2a2a (dark grey - default)
- **Western**: #2a2a2a (dark grey - default)

### Genre Assignment Per Aisle
Map genres to aisles by placement order (6 aisles → first 6 genres):
1. X = -25: Horror
2. X = -15: Drama
3. X = -5: Action
4. X = 5: Thriller
5. X = 15: Sci-Fi
6. X = 25: Comedy

Additional genres (Animation, Romance, Documentary, Western) distributed on wall shelves or as overflow.

### Rendering Process
- Generate all cases at start (do NOT load from CSV yet)
- Solid MeshBasicMaterial, no textures
- Log to console: "Total DVD cases placed: [COUNT]"
- Random brightness: `color * (0.8 + Math.random() * 0.4)` per case

## VISUAL ELEMENTS

### Floor
- **Material**: Dark grey tiles (#2a2a2a)
- **Pattern**: Subtle grout lines (slight darker grey gaps between tiles, ~0.05 units)
- **Y Position**: 0
- **Coverage**: Full store area X: [-30, 30], Z: [-20, 20]

### Walls

**Back Wall** (Z ≥ +19.5):
- Lower section (60% height, Y: 0 to 2.7): Navy #003087
- Upper section (40% height, Y: 2.7 to 4.5): Blockbuster yellow #FFD600
- Dividing stripe (Y ≈ 2.7, height 0.1): White
- Shelves mounted on this wall

**Front Wall** (Z ≤ -19.5):
- Lower section: Navy #003087
- Upper section: Yellow #FFD600
- Dividing stripe: White
- Opening for entrance (center, approximately 10 units wide)

**Left Wall** (X ≤ -29.5):
- Lower section: Navy #003087
- Upper section: Yellow #FFD600
- Dividing stripe: White
- Shelves mounted on this wall

**Right Wall** (X ≥ +29.5):
- Lower section: Navy #003087
- Upper section: Yellow #FFD600
- Dividing stripe: White
- Shelves mounted on this wall

### Ceiling
- **Material**: Off-white drop ceiling (#cccccc)
- **Y Position**: 4.5
- **Pattern**: Drop ceiling tiles with subtle grid lines (6×6 unit tiles)
- **Details**: Grid lines slightly darker (#aaaaaa), 0.05 unit thick

### Genre Signs
- **One sign per aisle** at the ENTRANCE END of each aisle (positioned at Z ≈ -12)
- **Style**: Street sign - mounted perpendicular to view as player approaches aisle
- **Dimensions**: 3 units wide × 1.5 units tall
- **Background**: Dark navy (#003087)
- **Text**: Blockbuster yellow (#FFD600), bold, centered
- **Rotation**: Faces toward aisle entrance
- **Font Size**: Large, readable from 8+ units away

**Sign Genres (per aisle X position):**
- X = -25: "HORROR"
- X = -15: "DRAMA"
- X = -5: "ACTION"
- X = 5: "THRILLER"
- X = 15: "SCI-FI"
- X = 25: "COMEDY"

### Checkout Counter
- **Location**: Left side near entrance, Z ≈ -17, X ≈ -10
- **Dimensions**: 4 units wide × 2 units deep × 1 unit tall
- **Material**: Wood color (#8B5E3C)
- **Purpose**: Visual element only, not functional

## LIGHTING SYSTEM

### Ambient Base Light
- **Color**: Warm dim (#333322)
- **Intensity**: 0.4

### Fluorescent Strip Lights
- **One per aisle** mounted at ceiling just below Y = 4.5
- **Position**: Centered at each aisle's X coordinate, Z = 0 (middle of aisle)
- **Properties**: Point light, warm white color, moderate intensity
- **Shadow**: Enabled with 256×256 shadow maps

### Aisle-Specific Color Influence
Each aisle adds a subtle colored ambient light influence. As player moves between aisles, transition lighting over ~2 seconds (lerp effect):

- **Horror Aisle** (X = -25): Slight green tint
- **Drama Aisle** (X = -15): Warm dim gold
- **Action Aisle** (X = -5): Cool white, high contrast
- **Thriller Aisle** (X = 5): Cool blue
- **Sci-Fi Aisle** (X = 15): Cool blue-white
- **Comedy Aisle** (X = 25): Warm yellow
- **Default** (transition zones): Neutral white

### Light Positioning Details
- No lights cast shadows from poster cases (lighting affects them minimally)
- Lights positioned to avoid z-fighting with ceiling geometry
- Fluorescent strips flicker subtly (vary intensity ±5% over 0.5 second cycles) for atmosphere

## INTERACTION & UI

### HUD Elements
- **Crosshair**: Center screen, yellow circle (#FFD600)
- **Instructions**: Bottom left, "Click to explore | WASD to move" (desktop)
- **Mobile Instructions**: "Left drag to move | Right drag to look"
- **Back Button**: Top left, "← Back to Collection", links to index.html

### Hover Interaction
- **Hover Over Case**: Display genre name at bottom center
- **Display Duration**: Fade in/out smoothly
- **Styling**: Yellow text (#FFD600) on dark background

### Click Interaction
- **Detail Panel**: Full-screen overlay, dark navy background (#0f1628), yellow border/text (#FFD600)
- **Panel Content**:
  - Placeholder "Film Title"
  - Genre label
  - "DVD Case" text
  - Simple close button or click-outside to close
- **No actual movie data loaded yet** (placeholder text only)

### Controls

**Desktop:**
- **WASD**: Movement (W=forward, A=left, S=back, D=right, relative to view)
- **Mouse**: Look around with pointer lock
- **Click**: Request pointer lock OR interact with cases
- **ESC**: Release pointer lock

**Mobile:**
- **Left half of screen touch + drag**: Virtual joystick (movement)
- **Right half of screen drag**: Look around
- **Tap case**: Interact

## TECHNICAL REQUIREMENTS

### Z-Fighting Prevention
- Floor at Y = 0
- Carpet tiles at Y = 0.01 (sits on floor)
- Shelf surfaces at Y = [0.3, 0.9, 1.5]
- DVD cases at Y = shelf_level + 0.275
- Wall backing panels at Z = wall_position ± 0.02
- Ceiling at Y = 4.5
- Ceiling tiles at Y = 4.45

### Geometry Derivation
- All collider positions derived from visual shelf positions
- No separate hardcoded collision geometry
- Collision boxes calculated from shelf dimensions and positions
- Boundary collision at room edges with 0.4 unit player radius margin

### Performance Considerations
- Use MeshBasicMaterial for DVD cases (no complex shading)
- Limit lighting to 1 ambient + 6 fluorescent strip lights
- No texture loading for initial implementation
- Batch cases into single geometry where possible
- LOD or culling not needed for this small environment

## IMPLEMENTATION ORDER

1. **Environment Setup**: Scene, camera, renderer, basic colors
2. **Floor & Ceiling**: Base planes, tile patterns, lighting
3. **Walls**: All 4 walls with color sections and stripes
4. **Shelving System**: Aisle shelves, wall shelves, supports, backing panels
5. **Genre Signs**: Canvas textures, positioned per aisle
6. **Lighting**: Ambient + fluorescent strips + color influences
7. **DVD Cases**: Generate all cases with genre colors and brightness variation
8. **Checkout Counter**: Simple geometry near entrance
9. **Movement System**: Camera, controls, collision detection, lerp lighting
10. **Interaction**: Hover detection, detail panel, crosshair
11. **Mobile Support**: Joystick, touch look
12. **Polish**: Camera bob, transitions, console logging

## VERIFICATION CHECKLIST

### Visual
- [ ] No z-fighting visible on any surfaces
- [ ] No floating DVD cases (all sitting on shelves)
- [ ] DVD cases tightly packed with 0.01 gap
- [ ] Wall colors match Blockbuster branding
- [ ] Ceiling grid visible and subtle
- [ ] Genre signs readable and well-positioned
- [ ] Checkout counter visible and proportional
- [ ] Lighting smooth across aisles with color transitions

### Functional
- [ ] Player can walk through all aisles without clipping
- [ ] Collision sliding works smoothly (separate X/Z testing)
- [ ] Hover over cases shows genre label
- [ ] Click cases opens detail panel
- [ ] ESC/click-outside closes detail panel
- [ ] Mobile controls responsive (joystick + look)
- [ ] Camera bob smooth while walking
- [ ] Pointer lock works on desktop
- [ ] Window resize handled

### Performance
- [ ] Smooth 60 FPS maintained
- [ ] No console errors
- [ ] Cases logged: "Total DVD cases placed: [COUNT]"
- [ ] All geometry loaded without lag

## IMPLEMENTATION NOTES

- **Three.js**: r128 from CDN (unchanged)
- **No Texture Loading**: Initial implementation uses solid colors only
- **No CSV Data**: Hardcoded case generation, placeholder film titles
- **Canvas Textures Only**: Genre signs use canvas text, no image files
- **Blockbuster Branding**: Colors strictly #FFD600 (yellow), #003087 (navy), #2a2a2a (dark floor)
- **No Cyan**: Explicitly avoid cyan color anywhere
- **Warm Atmosphere**: Use warm colors (#333322 ambient, #8B5E3C wood) when not using branded colors
- **Commit**: To feature branch `claude/plan-store-rebuild-YASCw` (per system requirements)

