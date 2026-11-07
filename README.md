# Colorful Rectangle Editor

A React application for creating, editing, and managing colored rectangles with a modern, responsive UI.

## Features

- **Box Inserter**: Create new colored rectangles with RGB values
  - Input R, G, B values (0-255 range validation)
  - Insert random button to generate random colors
  - Real-time preview of the color before inserting
  - Maximum 9 boxes limit

- **Box Editor**: Edit selected rectangles
  - Update R, G, B values with live preview
  - Apply changes to persist updates
  - View detailed box information (RGB, ID, creation time, box number)

- **Box List**: Display all created rectangles
  - Click to select and edit
  - Responsive grid layout (3 columns desktop, 2 tablets, 1 mobile)

- **Responsive Design**: Adapts to all screen sizes
  - Desktop: 3 boxes per row, side-by-side editor/inserter
  - Tablet: 2 boxes per row, stacked layout
  - Mobile: 1 box per row, vertical layout

## Tech Stack

- **React** - UI library
- **Redux** - Global state management
- **react-hook-form** - Form handling and validation
- **CSS3** - Styling with media queries for responsiveness

## Installation

1. Clone the repository:
```bash
git clone https://github.com/rzamfira/colorful_rectangle_editor.git
cd colorful_rectangle
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

The app will open in your browser at `http://localhost:5173`

## Project Structure

```
src/
├── components/
│   ├── boxInserter.jsx       # Component for creating new boxes
│   ├── boxInserter.css       # Styling for inserter
│   ├── boxEditor.jsx         # Component for editing selected box
│   ├── boxEditor.css         # Styling for editor
│   ├── boxList.jsx           # Component displaying all boxes
│   └── boxList.css           # Styling for box list
├── redux/
│   ├── store.js              # Redux store configuration
│   ├── reducer.js            # Main reducer with state logic
│   ├── actions.js            # Action creators
│   └── actionTypes.js        # Action type constants
├── App.jsx                   # Main app component
├── App.css                   # App styling and layout
└── main.jsx                  # Entry point
```

## Architecture

### Global State (Redux)

**State shape:**
```javascript
{
  boxes: [
    {
      id: "box-1",
      r: 255,
      g: 100,
      b: 50,
      creationTime: "2024-11-05T12:00:00.000Z"
    },
    // ... more boxes
  ],
  selectedBox: { /* currently selected box or null */ }
}
```

**Actions:**
- `ADD_BOX` - Add new box to list
- `SELECT_BOX` - Select box for editing
- `UPDATE_BOX_COLOR` - Update selected box's RGB colors

### Local State (React)

- **BoxEditor**: 
  - `showModal` - Modal visibility toggle (useState)
  - Form values - Managed by react-hook-form
  - Preview colors - Watched from form using `watch()`

- **BoxInserter**:
  - Form values - Managed by react-hook-form

### Data Flow

1. **Adding a box**: 
   - User fills form → Validates with react-hook-form → Dispatches ADD_BOX → Reducer generates ID and timestamp → Box added to Redux state

2. **Selecting a box**:
   - User clicks box in list → Dispatches SELECT_BOX → Redux updates selectedBox → BoxEditor receives via useSelector → Form resets with box colors

3. **Editing colors**:
   - User types in inputs → Form values update → `watch()` detects changes → Preview square updates in real-time
   - User clicks Apply → Dispatches UPDATE_BOX_COLOR with new colors → Reducer updates both boxes array and selectedBox

## Key Concepts Learned

### Redux
- Centralized state management with store, reducer, and actions
- Dispatch actions to update state predictably
- Use `useSelector` hook to read state in components

### React Hook Form
- Form validation with register and validation rules
- Form state tracking with `watch()` for real-time updates
- Handle form submission with `handleSubmit()`

### React Hooks
- `useState` for local component state
- `useEffect` for side effects (form reset)
- `useSelector` for Redux state reading

### Responsive Design
- Media queries for different screen sizes
- Flexible layouts with CSS Grid and Flexbox
- Font and spacing adjustments for readability on mobile

## Features Breakdown

### Box Inserter
- RGB inputs with number validation (0-255)
- "Insert random" button generates random RGB values
- Preview shows color before insertion
- Checks max 9 boxes limit with user feedback
- Form resets after successful insertion

### Box Editor
- Live preview updates using react-hook-form's `watch()`
- Info modal displays box metadata
- Apply button saves changes to Redux
- Clear visual feedback with colored cards

### Box List
- Grid layout adapts to screen size
- Click to select box for editing
- Boxes display their RGB color
- Ordered by creation

## Validation

- **R, G, B inputs**: Must be numbers between 0-255
- **Max boxes**: Limited to 9 rectangles
- **Required fields**: All RGB values required before inserting

## Browser Support

Works on all modern browsers:
- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers

## Future Enhancements

- Export/Import box configurations
- Undo/Redo functionality
- Delete box feature
- Box naming
- Color picker input
- Local storage persistence

