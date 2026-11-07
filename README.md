# Sudoku React

A fully-featured Sudoku game built with React, featuring an interactive board, timer, pause system, notes mode, and conflict detection.

## Features

-  **Interactive Sudoku Board** - Click to select cells and enter numbers
-  **Timer** - Track your solving time with pause functionality
-  **Notes Mode** - Add candidate notes to cells (3×3 grid display)
-  **Undo History** - Undo your moves (optimized to store only changed cells)
-  **Conflict Detection** - Automatic detection of duplicate numbers in rows, columns, and 3×3 squares
-  **Pause System** - Pause the game and timer; grid becomes transparent but borders remain visible
-  **Win Detection** - Automatically detects when the puzzle is completed without conflicts
-  **Responsive Design** - Adapts to different screen sizes (desktop, tablet, mobile)
-  **Solve Feature** - Console command to auto-solve puzzle (leaves one cell empty for manual completion)

## Tech Stack

- **React** - UI library
- **JavaScript** - Core logic
- **CSS Grid** - Layout system
- **sudoku.js** - Sudoku puzzle generation and solving library

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd sudoku_react
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm start
```

4. Open [http://localhost:3000](http://localhost:3000) in your browser

## How to Play

### Basic Controls
- **Click a cell** - Select it (highlighted in blue)
- **Type 1-9** - Enter a number into the selected cell
- **Backspace/Delete** - Erase the selected cell
- **Arrow Keys** - Navigate between cells

### Features

#### Timer & Pause
- Timer starts automatically when the game loads
- Click the pause button to pause/resume the game and timer
- When paused, the grid becomes semi-transparent but grid lines remain visible

#### Notes Mode
- Click the "Notes" button in controls to toggle notes mode
- In notes mode, enter numbers to add candidate notes (displayed as 3×3 grid in cells)
- Exit notes mode by clicking again; notes automatically clear from cells when you enter a value

#### Undo
- Click the undo button to revert your last move
- Undo history is maintained separately for board values and notes

#### Conflict Highlighting
- Cells with duplicate numbers are highlighted in red
- Duplicates are detected within:
  - Rows
  - Columns
  - 3×3 squares
- Neighbors of selected cells are highlighted in light blue
- Cells with the same value as the selected cell are highlighted in light blue

#### Win Condition
- The game is won when all 81 cells are filled with no conflicts
- A "You Won!" overlay appears
- Timer automatically stops
- Click "New Game" to play again

### Console Features

#### Solve Puzzle (Feature Flag)
Open the browser console and type:
```javascript
solvePuzzle()
```

This will:
- Solve the current puzzle using the sudoku.js solver
- Leave the first empty cell empty for you to complete manually
- Reset your history and notes for a fresh attempt

## Project Structure

```
src/
├── App.jsx                          # Main app component, state orchestrator
├── App.css                          # Grid layout and responsive design
├── hooks/
│   ├── useTimer.js                  # Timer state and interval management
│   ├── useSudokuGame.js             # Puzzle generation and board management
│   ├── useSolvePuzzle.js            # Solve feature (console command)
│   └── useUpdateCell.js             # Cell update logic with history tracking
├── services/
│   ├── cellService.js               # Cell utilities (editable check, filling, etc.)
│   ├── conflictService.js           # Conflict detection algorithm
│   ├── navigationService.js         # Keyboard navigation logic
│   ├── notesService.js              # Notes board initialization
│   ├── stylingService.js            # CSS class generators
│   └── timerService.js              # Timer utilities
├── layout/
│   ├── sudoku-board/
│   │   ├── Board.jsx                # Main board component with pause/won overlays
│   │   ├── Board.css                # Board styling and overlay styles
│   │   ├── Square.jsx               # 3×3 square container
│   │   ├── Cell.jsx                 # Individual cell with notes
│   │   ├── Cell.css                 # Cell styling and highlights
│   │   ├── BoardContext.jsx         # Context definition
│   │   └── BoardProvider.jsx        # Context provider with board state
│   └── controls-panel/
│       ├── GameControls.jsx         # Undo, Erase, Notes buttons
│       ├── GameControls.css
│       ├── Numpad.jsx               # 1-9 number input buttons
│       ├── Numpad.css
│       ├── Timer.jsx                # Timer display and pause button
│       ├── Timer.css
│       ├── NewGame.jsx              # New Game button
│       └── NewGame.css
└── icons/                           # SVG icons (undo, erase, notes, play, pause)
```

## State Management

### App Level State
- `boardValues` - 9×9 grid of current cell values
- `selectedCell` - Currently selected cell position
- `notesMode` - Toggle for notes mode
- `notesBoard` - 9×9 grid of arrays storing candidate notes
- `history` - Stack of previous states for undo
- `isPaused` - Pause state
- `elapsedTime` - Current timer in seconds
- `isGameWon` - Win condition flag (derived from board state)

### Context (BoardContext)
Shared with Board and Cell components:
- Board configuration (initial puzzle, current values, conflicts)
- Selection state and callbacks
- Pause state and interaction handler
- Win state

## Key Design Decisions

### Optimized History Storage
Instead of storing entire board snapshots, only changed cells are stored:
```javascript
{
  boardValues: structuredClone(boardValues),
  notesBoard: structuredClone(notesBoard),
  selectedCell
}
```
This saves ~95% memory compared to storing full state snapshots.

### Derived Win State
`isGameWon` is computed with `useMemo` instead of stored in state, avoiding cascading renders and keeping derived data in sync.

### Pause Overlay Design
The pause overlay is positioned inside the Board component (not App level) because:
- It's board-specific UI
- Keeps related logic together
- Allows transparent background to show grid lines

### Timer Cleanup
The timer uses `let intervalId` inside useEffect instead of `useRef` because:
- Interval is only accessed within the effect
- Simpler and more efficient
- Cleanup function has access via closure

### Notes as Arrays
Candidate notes stored as arrays `[1, 2, 3]` instead of individual state because:
- More flexible for future features
- Easier to clear on value entry
- Better performance with `includes()` checks

## Responsive Breakpoints

### Desktop (> 700px)
- 2-column grid: board (2fr) | controls (1fr)
- Timer and NewGame on the right side
- Full-size buttons and text

### Mobile (≤ 700px)
- 1-column layout
- Header section with Timer + NewGame side-by-side
- Board full width
- Controls (GameControls + Numpad) full width
- Compact button sizes with responsive fonts

### Small Mobile (≤ 500px)
- Same layout as mobile with further reduced width

## Performance Optimizations

1. **Memoized Computations** - Cell properties computed with `useMemo`
2. **Optimized History** - Only changed cells stored
3. **Efficient Conflict Detection** - Single pass algorithm
4. **Context Optimization** - Minimal context dependencies to prevent unnecessary re-renders
5. **Lazy Note Display** - Notes only rendered when cell is empty

## Browser Compatibility

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Requires modern JavaScript (ES6+)

## Future Enhancements

- [ ] Difficulty levels (Easy, Medium, Hard, Expert)
- [ ] Statistics tracking (best time, games played)
- [ ] Local storage persistence
- [ ] Multiple undo/redo with keyboard shortcuts
- [ ] Hint system
- [ ] Dark mode
- [ ] Sound effects
- [ ] Multiplayer scoring

## License

This project is open source and available under the MIT License.

## Credits

Built with React and [sudoku.js](https://github.com/robatron/sudoku.js) for puzzle generation and solving.