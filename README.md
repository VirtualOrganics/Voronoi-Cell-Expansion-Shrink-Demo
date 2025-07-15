# Voronoi Cell Expansion-Shrink Demo

An interactive 3D visualization demonstrating how Voronoi cells expand and shrink by moving their generator points. This demo provides clear insight into the fundamental mechanics of Voronoi diagram manipulation.

🔗 **[Live Demo](https://virtualorganics.github.io/Voronoi-Cell-Expansion-Shrink-Demo/)**

## Overview

This demo illustrates a core concept in computational geometry: **how moving a generator point affects the size and shape of its corresponding Voronoi cell**. By visualizing this relationship in real-time, users can develop an intuitive understanding of:

- The relationship between generator points and cell boundaries
- How cells compete for space in a Voronoi diagram
- The dynamics of cell expansion and contraction

## Key Features

### 🎯 Focused Visualization
- **Single Cell Manipulation**: Control one cell at a time to clearly see cause and effect
- **Visual Feedback**: The active cell changes color (blue for shrinking, red for expanding)
- **Generator Points**: Yellow dots show the actual generator positions
- **Movement Vectors**: Arrows indicate the direction and magnitude of movement

### 🎛️ Interactive Controls
- **Change Slider** (-10 to +10): Control the expansion/shrink factor
  - Negative values: Cell shrinks (blue)
  - Positive values: Cell expands (red)
- **Live Update**: Toggle real-time computation
- **Opacity Controls**: Adjust visibility of selected vs unselected cells
- **Central Button**: Automatically select the most central cell

## How It Works

### The Method

1. **Generator Movement**: When you adjust the slider, the generator point of the selected cell moves:
   - **Expansion** (positive values): Generator moves away from the cell's centroid
   - **Shrinkage** (negative values): Generator moves toward the cell's centroid

2. **Voronoi Recomputation**: As the generator moves, the Voronoi diagram is recalculated, causing:
   - The selected cell to grow or shrink
   - Neighboring cells to adjust their boundaries accordingly

3. **Visual Feedback**: 
   - Cell color indicates state (blue/red/grey)
   - Arrow shows movement direction
   - Real-time updates show the immediate effect

### Mathematical Foundation

The expansion/shrink factor controls the generator displacement:

```
new_position = original_position + (original_position - centroid) * factor * scale
```

Where:
- `original_position`: Initial generator location
- `centroid`: Geometric center of the Voronoi cell
- `factor`: User-controlled value (-10 to +10)
- `scale`: Scaling constant for reasonable movement

## Usage

1. **Select a Cell**: 
   - Click "Central" to auto-select the most central cell
   - Or manually enter a cell index in "Active Cell"

2. **Adjust the Change Slider**:
   - Move left (negative) to shrink the cell
   - Move right (positive) to expand the cell

3. **Observe the Effect**:
   - Watch the generator point move
   - See the cell boundaries adjust
   - Notice how neighboring cells respond

4. **Fine-tune Visibility**:
   - Adjust "Selection" opacity for the active cell
   - Adjust "Unselected" opacity for other cells

## Technical Details

### Implementation
- **3D Voronoi Computation**: Uses periodic Delaunay triangulation via WebAssembly
- **Real-time Visualization**: Three.js for 3D rendering
- **Efficient Updates**: Only recomputes when necessary

### Visualization Elements
- **Voronoi Cells**: 3D polyhedra with adjustable transparency
- **Delaunay Edges**: Yellow lines showing the dual triangulation
- **Generator Points**: Small spheres at cell centers
- **Movement Vectors**: Directional arrows showing displacement

## Applications

This demonstration is valuable for:

- **Education**: Teaching Voronoi diagram concepts
- **Research**: Understanding cell dynamics in computational geometry
- **Development**: Prototyping Voronoi-based algorithms
- **Visualization**: Creating intuitive geometric demonstrations

## Controls Reference

| Control | Description | Default |
|---------|-------------|---------|
| Change | Expansion/shrink factor | -2.00 |
| Live Update | Real-time computation | Checked |
| Vectors | Show movement arrows | Checked |
| Show Generators | Display generator points | Checked |
| Active Cell | Index of selected cell | Auto |
| Selection | Opacity of active cell | 0.80 |
| Unselected | Opacity of other cells | 0.20 |

## Building from Source

```bash
# Clone the repository
git clone https://github.com/VirtualOrganics/Voronoi-Cell-Expansion-Shrink-Demo.git
cd Voronoi-Cell-Expansion-Shrink-Demo

# Serve locally
python3 -m http.server 8000

# Open in browser
open http://localhost:8000
```

## Acknowledgments

This demo is built upon:
- **[Fabric-of-Space-X](https://github.com/VirtualOrganics/Fabric-of-Space-X)**: The parent project for Voronoi analysis
- **Geogram** by Bruno Levy: Core geometric algorithms
- **Three.js**: 3D visualization framework
- **Emscripten**: WebAssembly compilation

## License

MIT License - See LICENSE file for details

---

**Explore the fundamental mechanics of Voronoi cell manipulation through interactive visualization** 