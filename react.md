# React

React keeps a "pretend version" of your webpage in memory. When something changes—like a number or text—it updates that pretend version first. Then React compares that pretend version to what’s actually on your screen. If it notices a difference, it only changes that exact part on your real webpage. This way, it doesn’t have to rebuild the whole page—just the part that changed.

If you want to build with React, you’ll typically start by defining components. A component is just a piece of your interface, like a button or a form. React lets you break the UI down into these parts and then combine them. Each component reacts to changes in data and updates the interface.

In a real web front-end, you have many interactive elements. State is what lets those elements respond. For example, a form input’s state holds what you type, a modal’s state controls whether it’s open or not, and a list’s state might hold items you’ve fetched. Instead of manually manipulating the page, you let the state drive what’s displayed. This makes complex interfaces manageable and predictable.

## Setup

We will use Vite to help set up the project. Vite handles the build and development environment—making it fast and efficient. React itself is what you write inside the src/ folder—your components, state, and UI logic. In short: Vite is the tool scaffolding your project; React is the library you use to build the UI inside that structure.

1. Open the terminal and run `npm create vite@latest my-app -- --template react`
2. Navigate into the project folder with `cd my-app` and run `npm install` to ensure dependencies are ready
3. Run `npm run dev` to launch the development server

When you run `npm create vite@latest`, it sets up a standard project structure. At the top level, you have a project folder. Inside, there’s a `package.json` file—this lists the project name, version, and dependencies. There's also a `vite.config.js` (or .ts) that configures the development build. The source code sits in a `src/` folder—this is where your React components go. Usually, `src/main.jsx` (or .tsx) is the entry point. Components can be organized in `src/` subfolders. You’ll also have a `public/` folder for static assets like images. Finally, you’ll run the project with `npm run dev`. In summary: configuration at the top, source code in `src/`, and static files in `public/`.

## Creating a Component

Next, we’ll define a simple React component. Inside your `src/` folder, open `App.jsx` and replace the code with the following:

```{JavaScript}
import { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default App;
```