# ONLYDRAW – Real-Time Collaborative Intelligent Whiteboard

ONLYDRAW is a browser-based **real-time collaborative whiteboard** that allows multiple users to create, edit, and interact with drawing elements on a shared canvas.

The project represents canvas content as **structured drawing objects** instead of treating the canvas as a single image. This enables operations such as **selection, movement, resizing, deletion, styling, and collaborative synchronization**.

The current implementation focuses on building the core collaborative drawing and interaction system using **React, HTML Canvas, Yjs, Rough.js, perfect-freehand, Zustand, and y-websocket**.

---

## 📌 Project Overview

Traditional online whiteboards can face challenges related to **real-time collaboration, object manipulation, and extensibility**.

ONLYDRAW addresses these challenges by providing:

- 🎨 A real-time shared whiteboard
- 🧩 Structured canvas objects
- ✏️ Multiple drawing tools
- 🖱️ Object selection and manipulation
- 📦 Custom hit-testing
- 🔄 CRDT-based collaborative state
- ✨ Sketch-style rendering
- 🖌️ Smooth freehand drawing
- 👥 Participant awareness
- ↩️ Undo/redo functionality

The project is designed as a foundation that can later be extended with **AI-powered voice interaction, persistent boards, authentication, infinite canvas functionality, and rich-text editing**.

---

## 🎯 Objectives

The main objectives of ONLYDRAW are:

1. Build a browser-based collaborative whiteboard.
2. Allow multiple users to work on the same canvas in real time.
3. Represent drawings as structured objects rather than a single image.
4. Support different drawing primitives and freehand input.
5. Provide direct manipulation such as selection, movement, resizing, and deletion.
6. Implement collaborative state using Yjs and CRDT principles.
7. Provide natural and sketch-style visual rendering.
8. Maintain a separation between local UI state and shared document state.
9. Build a foundation for future AI-assisted whiteboard interaction.

---

# ✨ Features

## 1. 🤝 Collaborative Whiteboard

Multiple users can work on the same canvas while changes are synchronized between connected clients.

The collaborative document is maintained using **Yjs**, while **y-websocket** provides the synchronization mechanism between connected clients.

---

## 2. 🎨 Drawing Tools

The current implementation supports:

- ▭ Rectangle
- ◯ Ellipse / Circle
- ╱ Line
- ✏️ Freehand Drawing

---

## 3. 🧩 Structured Object Model

Instead of storing the whiteboard as one image, every drawing is represented as a **structured object**.

A conceptual drawing element may contain:

```text
Element
├── ID
├── Type
├── Position
├── Dimensions / Geometry
├── Stroke
├── Fill
└── Drawing-specific Data
```

For example, a rectangle can be represented as:

```text
{
    type: "rectangle",
    x: 100,
    y: 150,
    width: 200,
    height: 120,
    stroke: "#000000",
    fill: "transparent"
}
```

This structured representation allows individual objects to be manipulated without modifying the entire canvas.

---

## 4. 🖱️ Object Selection and Manipulation

Drawing objects can be individually interacted with through:

- Selection
- Movement
- Resizing
- Deletion
- Styling

Custom **hit-testing** is used to determine which object the user is interacting with.

Bounding boxes are used to assist with selection and resizing operations.

---

## 5. ✏️ Freehand Drawing

Freehand input is implemented using **perfect-freehand** to generate smooth and natural-looking strokes.

Instead of simply storing raw mouse positions, the system processes pointer input to produce a smoother stroke representation.

This allows freehand drawings to maintain a more natural handwritten appearance.

---

## 6. ✨ Sketch-Style Rendering

**Rough.js** is used for rendering geometric shapes with a hand-drawn/sketch-like appearance.

This provides a more natural visual style compared with perfectly rigid geometric shapes.

Rough.js can be used for primitives such as:

- Rectangles
- Ellipses
- Lines
- Other geometric elements

---

## 7. 🔄 Real-Time Collaboration

ONLYDRAW uses **Yjs**, a CRDT-based framework, to maintain shared document state.

The basic collaboration flow is:

```text
User Action
     ↓
Local Drawing State
     ↓
Yjs Shared Document
     ↓
y-websocket
     ↓
Other Connected Clients
     ↓
Canvas Update
```

Because Yjs uses CRDT principles, concurrent changes from multiple users can be merged while maintaining a consistent shared state.

---

## 8. 👥 Participant Awareness

The system can maintain awareness information about connected participants.

This provides a foundation for collaborative features such as:

- User presence
- Cursor positions
- Active participants
- Collaboration indicators

---

## 9. ↩️ Undo / Redo

Undo and redo functionality is supported using the collaborative document's history mechanisms.

Yjs provides an **UndoManager** that can track changes made to shared data and allow users to undo or redo operations.

---

## 10. ⚡ Local UI State

**Zustand** is used for managing local application/UI state.

This helps separate:

```text
Local UI State
      +
Shared Collaborative State
```

For example, temporary UI information such as the currently selected tool or selected object can be handled locally, while drawing elements that need to be synchronized are maintained through the collaborative Yjs document.

---

# 🏗️ Technology Stack

| Technology | Purpose |
|------------|---------|
| **React** | Frontend UI and component management |
| **HTML Canvas** | Drawing and rendering surface |
| **Yjs** | Collaborative shared document state |
| **y-websocket** | Real-time synchronization |
| **Rough.js** | Sketch-style shape rendering |
| **perfect-freehand** | Smooth freehand strokes |
| **Zustand** | Local UI/application state |
| **JavaScript** | Application logic |

---

# 🧠 System Architecture

The high-level architecture can be represented as:

```text
                 ┌──────────────────────┐
                 │      React UI        │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Canvas / Tools     │
                 └──────────┬───────────┘
                            │
              ┌─────────────┴─────────────┐
              ▼                           ▼
      ┌───────────────┐           ┌────────────────┐
      │    Zustand    │           │      Yjs       │
      │ Local UI State│           │ Shared State   │
      └───────────────┘           └───────┬────────┘
                                          │
                                          ▼
                                  ┌───────────────┐
                                  │ y-websocket   │
                                  │ Synchronizer  │
                                  └───────┬───────┘
                                          │
                           ┌──────────────┴──────────────┐
                           ▼                             ▼
                    ┌──────────────┐              ┌──────────────┐
                    │   Client A   │              │   Client B   │
                    └──────────────┘              └──────────────┘
```

---

# 🔍 Object Interaction Flow

When a user interacts with a drawing object:

```text
Pointer Event
     ↓
Determine Pointer Position
     ↓
Hit-Testing
     ↓
Identify Target Object
     ↓
Selection / Movement / Resize
     ↓
Update Object Geometry
     ↓
Update Shared State
     ↓
Synchronize Through Yjs
     ↓
Render Updated Canvas
```

---

# 📐 Example: Rectangle Interaction

A rectangle can be represented using:

```text
x
y
width
height
stroke
fill
```

When the user draws a rectangle:

```text
Mouse Down
    ↓
Store Starting Point
    ↓
Mouse Move
    ↓
Calculate Width & Height
    ↓
Create / Update Rectangle Object
    ↓
Render Rectangle
    ↓
Mouse Up
    ↓
Store Final Geometry
```

For resizing, the system identifies the selected object's **resize handle**, calculates the new geometry based on pointer movement, and updates the object's dimensions.

---

# 🧩 Current Implementation

The current implementation focuses primarily on the **core collaborative drawing system**, including:

- Canvas-based drawing
- Basic drawing primitives
- Freehand drawing
- Structured drawing objects
- Object selection
- Object manipulation
- Hit-testing
- Sketch-style rendering
- Collaborative synchronization
- Participant awareness
- Undo/redo
- Local UI state management

---

# 🚀 Future Scope

The architecture provides a foundation for several future improvements.

### 🤖 AI-Powered Interaction

Voice commands could allow users to interact with the whiteboard using natural language.

For example:

```text
"Draw a red rectangle in the top-right corner."
```

The command could be processed by an AI system and converted into structured drawing operations.

---

### 💾 Persistent Boards

Future versions can support saving and loading boards using a backend database.

This would allow users to:

- Save boards
- Reopen previous sessions
- Share boards
- Maintain drawing history

---

### 🔐 Authentication

Authentication can be introduced to support:

- User accounts
- Private boards
- Shared boards
- Permission management

---

### ♾️ Infinite Canvas

The current canvas can be extended into an infinite workspace with:

- Zooming
- Panning
- Coordinate transformations
- Large-scale drawing support

---

### 📝 Rich Text Editing

Text elements can be introduced as structured objects that support:

- Formatting
- Font size
- Font family
- Alignment
- Editing

---

# 📌 Conclusion

ONLYDRAW demonstrates how a collaborative whiteboard can be designed around a **structured object model and real-time CRDT-based synchronization** rather than treating the canvas as a static image.

By combining **React, Canvas, Yjs, y-websocket, Rough.js, perfect-freehand, and Zustand**, the project provides a foundation for building an extensible collaborative drawing environment.

The architecture also leaves room for future **AI-assisted interaction, persistence, authentication, infinite canvas capabilities, and richer editing features**.
