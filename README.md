# Voronoi Cell Expansion-Shrink Demo

An interactive 3D visualization demonstrating the fundamental mechanics of how Voronoi cells physically expand and shrink through generator point movement. This demo provides clear insight into the mathematical relationship between generator positions and cell boundaries.

🔗 **[Live Demo](https://virtualorganics.github.io/Voronoi-Cell-Expansion-Shrink-Demo/)**

## 🎯 Overview

This demo distills the core concept from the [Fabric-of-Space-X](https://github.com/VirtualOrganics/Fabric-of-Space-X) project: **how moving a generator point affects the size and shape of its corresponding Voronoi cell**. By visualizing this relationship in real-time, users can develop an intuitive understanding of:

- The direct relationship between generator points and cell boundaries
- How cells compete for space in a Voronoi diagram
- The dynamics of cell expansion and contraction
- The physical mesh transformation process

## 🔧 How Cell Expansion-Shrink Works

### The Fundamental Principle

In a Voronoi diagram, each cell represents the region of space closest to its generator point. When you move a generator:
- **Away from its cell's center** → The cell EXPANDS
- **Toward its cell's center** → The cell SHRINKS

This demo makes this abstract concept tangible by letting you control the movement in real-time.

### 🔬 The Physical Movement Phase

The expansion-shrink system translates a simple control value into actual mesh deformation through a precise mathematical process:

#### **Step 1: Calculate Cell Centroid**

For each Voronoi cell, we find its geometric center:

```javascript
// Collect all vertices of the Voronoi cell
const cellVertices = cell.faces.flatMap(face => face.vertices);

// Calculate centroid (average position)
const centroid = {
  x: cellVertices.reduce((sum, v) => sum + v.x, 0) / cellVertices.length,
  y: cellVertices.reduce((sum, v) => sum + v.y, 0) / cellVertices.length,
  z: cellVertices.reduce((sum, v) => sum + v.z, 0) / cellVertices.length
};
```

The centroid represents the "heart" of the cell - the average position of all its boundary vertices.

#### **Step 2: Determine Movement Direction**

Based on the user's input (the Change slider value):

```javascript
// Calculate vector FROM generator TO centroid
const direction = {
  x: centroid.x - generator.x,
  y: centroid.y - generator.y,
  z: centroid.z - generator.z
};

// Normalize to unit vector
const magnitude = Math.sqrt(direction.x² + direction.y² + direction.z²);
const unitDirection = {
  x: direction.x / magnitude,
  y: direction.y / magnitude,
  z: direction.z / magnitude
};
```

This gives us the direction the generator needs to move to shrink the cell.

#### **Step 3: Apply the Movement**

The actual position change based on the slider value:

```javascript
// changeValue: -10 to +10 from slider
// Negative = shrink (move toward centroid)
// Positive = expand (move away from centroid)

const moveAmount = changeValue * scale * deltaTime;

// Apply movement
generator.x -= unitDirection.x * moveAmount;  // Note: minus for correct direction
generator.y -= unitDirection.y * moveAmount;
generator.z -= unitDirection.z * moveAmount;
```

#### **Step 4: Why This Changes the Mesh**

The KEY insight - moving the generator changes ALL neighboring cells:

1. **Generator moves outward** → Its Voronoi cell EXPANDS
   - The generator claims more "territory"
   - Neighboring cells get compressed
   - Cell boundaries shift outward
   
2. **Generator moves inward** → Its Voronoi cell SHRINKS
   - The generator claims less "territory"
   - Neighboring cells expand to fill the space
   - Cell boundaries shift inward

### Visual Example

```
BEFORE (neutral state):          AFTER (expansion, Change = +5):
    N1 -------- N2                   N1 ------ N2
    |     •G    |                    | •G'     |
    |     ↓     |                    | ↖       |
    |     C     |                    |   C     |
    N3 -------- N4                   N3 ------ N4

G = Generator                    G' = New generator position
C = Centroid                     Cell expanded, neighbors compressed
```

## 🔄 The Complete Transform Pipeline

When you adjust the Change slider, here's exactly what happens each frame:

```javascript
1. getUserInput()          → changeValue = -2.0 (from slider)
2. calculateCentroid()     → centroid = (0.5, 0.6, 0.5)
3. computeDirection()      → direction = normalized(centroid - generator)
4. moveGenerator()         → generator += direction * changeValue * scale
5. recomputeDelaunay()     → new tetrahedra from moved generator
6. rebuildVoronoi()        → new cells from new tetrahedra
7. updateVisualization()   → render new geometry with colors
```

This creates the smooth animation where:
- The selected cell "breathes" in response to slider changes
- Neighboring cells automatically adjust
- The entire mesh remains mathematically consistent

## 📊 Mathematical Details

### The Expansion-Shrink Equation

The complete movement equation for the generator point:

```
Δp = -(C - p) × factor × scale × dt

Where:
- p = current generator position
- C = cell centroid position
- factor = user-controlled value (-10 to +10)
- scale = scaling constant (0.001 for reasonable movement)
- dt = frame time delta
```

### Key Properties

1. **Linear Response**: Movement is directly proportional to the slider value
2. **Direction Consistency**: Always moves along the generator-centroid axis
3. **Frame-rate Independence**: Uses deltaTime for consistent animation
4. **Reversible**: Moving the slider back returns to original state

## 🎮 Interactive Controls

### Core Controls

| Control | Description | Range | Default |
|---------|-------------|-------|---------|
| **Change** | Expansion/shrink factor | -10 to +10 | -2.00 |
| **Live Update** | Real-time computation toggle | On/Off | On |
| **Central** | Auto-select central cell | Button | - |

### Visualization Options

| Control | Description | Range | Default |
|---------|-------------|-------|---------|
| **Vectors** | Show movement arrows | On/Off | On |
| **Show Generators** | Display generator points | On/Off | On |
| **Selection** | Active cell opacity | 0-1 | 0.80 |
| **Unselected** | Other cells opacity | 0-1 | 0.20 |

### Visual Indicators

- **Cell Colors**:
  - 🔵 Blue: Shrinking (negative Change value)
  - 🔴 Red: Expanding (positive Change value)
  - ⚫ Grey: Neutral (Change value = 0)

- **Yellow Elements**:
  - Generator points (small spheres)
  - Delaunay edges (connectivity lines)
  - Movement vectors (when enabled)

## 🏗️ Technical Implementation

### Core Technologies

- **3D Voronoi Computation**: WebAssembly module using Geogram library
- **Real-time Visualization**: Three.js for 3D rendering
- **Efficient Updates**: Only recomputes when necessary
- **Periodic Boundaries**: Proper handling of wrapped space

### Visualization Elements

1. **Voronoi Cells**: 3D polyhedra with adjustable transparency
2. **Delaunay Edges**: Yellow lines showing the dual triangulation
3. **Generator Points**: Small spheres (radius 0.0075) at cell centers
4. **Movement Vectors**: Directional arrows with logarithmic scaling

### Performance Optimizations

- Pre-allocated geometry buffers
- Efficient mesh updates
- Frame-rate independent animation
- Selective recomputation based on Live Update toggle

## 🎯 Use Cases

### Educational

- **Teaching Voronoi Concepts**: Visual demonstration of generator-cell relationship
- **Interactive Learning**: Hands-on exploration of geometric principles
- **Mathematical Visualization**: See abstract concepts in action

### Research & Development

- **Algorithm Prototyping**: Test Voronoi-based algorithms
- **Parameter Exploration**: Understand sensitivity to generator movement
- **Visualization Tool**: Create demonstrations for papers/presentations

### Creative Applications

- **Generative Art**: Use as inspiration for Voronoi-based designs
- **Animation Reference**: Understand cell dynamics for procedural animation
- **Pattern Generation**: Explore emergent patterns from simple rules

## 🚀 Running Locally

```bash
# Clone the repository
git clone https://github.com/VirtualOrganics/Voronoi-Cell-Expansion-Shrink-Demo.git
cd Voronoi-Cell-Expansion-Shrink-Demo

# Serve locally (Python 3)
python3 -m http.server 8000

# Or using Node.js
npx http-server -p 8000

# Open in browser
open http://localhost:8000
```

## 🧩 Understanding the Code

### Key Files

- `index.html` - Main demo interface and controls
- `src/js/DelaunayComputation.js` - Core Voronoi/Delaunay engine
- `src/js/Visualizer.js` - Three.js rendering logic
- `dist/delaunay.js` - WebAssembly module (compiled from Geogram)

### Generator Movement Implementation

The movement logic in the demo:

```javascript
// When slider changes
function updateGeneratorPosition(cellIndex, changeValue) {
    // Get current positions
    const generator = generators[cellIndex];
    const centroid = calculateCentroid(cells[cellIndex]);
    
    // Calculate movement
    const direction = normalize(subtract(centroid, generator));
    const movement = scale(direction, -changeValue * 0.001);
    
    // Apply movement
    generator.add(movement);
    
    // Trigger recomputation
    if (liveUpdate) {
        recomputeVoronoi();
    }
}
```

## 🤝 Contributing

We welcome contributions! Areas for enhancement:

- Additional visualization modes
- Performance optimizations
- Educational annotations
- Alternative control schemes
- Export functionality

## 📄 License

MIT License - See LICENSE file for details

## 🙏 Acknowledgments

This demo is built upon:
- **[Fabric-of-Space-X](https://github.com/VirtualOrganics/Fabric-of-Space-X)**: The parent project for comprehensive Voronoi analysis
- **Geogram** by Bruno Levy: Core geometric algorithms
- **Three.js**: 3D visualization framework
- **Emscripten**: WebAssembly compilation

Special thanks to the computational geometry community for the foundational algorithms that make this visualization possible.

---

**Explore the fundamental mechanics of Voronoi cell manipulation through interactive visualization** 