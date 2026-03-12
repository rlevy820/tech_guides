# React

React keeps a "pretend version" of your webpage in memory. When something changes—like a number or text—it updates that pretend version first. Then React compares that pretend version to what’s actually on your screen. If it notices a difference, it only changes that exact part on your real webpage. This way, it doesn’t have to rebuild the whole page—just the part that changed.

If you want to build with React, you’ll typically start by defining components. A component is just a piece of your interface, like a button or a form. React lets you break the UI down into these parts and then combine them. Each component reacts to changes in data and updates the interface.

In a real web front-end, you have many interactive elements. State is what lets those elements respond. For example, a form input’s state holds what you type, a modal’s state controls whether it’s open or not, and a list’s state might hold items you’ve fetched. Instead of manually manipulating the page, you let the state drive what’s displayed. This makes complex interfaces manageable and predictable.

We’ll build a basic to-do list where you can add tasks.

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
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');

  return (
    <div>
      <input 
        value={newTask}
        onChange={(e) => setNewTask(e.target.value)}
        placeholder="Add a task"
      />
      <button onClick={() => setTasks([...tasks, newTask])}>
        Add Task
      </button>
      <ul>
        {tasks.map((task, index) => (
          <li key={index}>{task}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```
### State

We start by importing a function called `useState` from the `react` library. `useState` is a function that takes in any data type parameter and returns an array with two elements: (1) the current state, which is equal to the parameter you gave to the function (a number, string, list, object, etc), (2) a function that you call when you want to change the state (you are calling (2) to change (1)).

You can think of state as a special kind of variable tied to the component’s behavior. Unlike regular variables, when you update state using the provided function (like setTasks or setNewTask), React re-renders the component. So, state holds dynamic data that React watches and reacts to.

Think about whats going to change on the browser, and thats how you think about what state variables you'll need. You identify state variables based on what data you expect to change—whether through user input, events, or data fetched from somewhere. If it’s dynamic and the UI should reflect those changes, you manage it with state. Static elements or data that never change don’t need state. If you have a to-do list, the list of tasks is state because it changes when you add or remove tasks. In contrast, a static title like "My To-Do List" doesn’t change, so it’s not state. The list of tasks changes as you add new items, and the text input changes as the user types. Both require state to keep the UI in sync with those changes.

`const [tasks, setTasks] = useState([]);` calls the function `useState([])` which returns two items which we are naming as `tasks` for the state and `setTasks` as the function we call to update `tasks`. Since and empty list `[]` is passed in as a parameter to `useState()`, `tasks` is initialized as an empty list.

When we want to add the first item to the list, we call `setTasks([...tasks, newTask])`. This basically says "add all elements of the list (`...tasks`) and append the `newTask`".

`...tasks` equals the full current value of `tasks`.

The line const `[newTask, setNewTask] = useState('');` creates a piece of state called `newTask`, starting as an empty string. This is where we store what the user is currently typing into the input field. The `setNewTask` function allows us to update that text as the user types. When the input changes, we call `setNewTask(e.target.value)` to keep newTask in sync with what's in the text box.

The `e` is the event object passed in by the input field when you type. `e.target` is the element that triggered the event (in this case, the input box). `e.target.value` is the current text inside that input box. So when we call `setNewTask(e.target.value)`, we’re updating the `newTask` state to match what’s currently typed in the input.


### Return

The return statement in a React component is where we define what should be displayed on the screen. In this case, App is a component, and whatever we return is the user interface—what the user will see and interact with. So, inside that return, we’re essentially describing the structure and elements of the page—like text boxes, buttons, and lists—built from HTML-like elements.

Inside that return, we place the elements we need. The input field is where the user types. The button triggers an action when clicked. The list (the `ul` and `li` elements) shows all tasks. Each of these elements is connected to the state we set up—the input shows what’s being typed, the button adds to the list, and the list renders each task.

#### Input

The input element is where the user types a task. Its `value` attribute is tied to the `newTask` state, meaning whatever `newTask` is, that’s what you see inside the input. When the user types, the `onChange` event fires. This event takes what the user typed (`e.target.value`) and updates `newTask`, ensuring the input always matches what’s stored in that state.

#### Button

The button is a clickable element that triggers an action. When the user clicks it, it runs the function tied to its `onClick`. In this case, we take the current `newTask`, add it to the tasks array, and update the state with the new list.

#### List

The list is rendered from the `tasks` state. Each time tasks changes, React re-renders the list. We use a `map` function to go through each task and display it as a list item. Each item has a unique `key` (here we use the index). So, when you add a task, the list updates immediately, reflecting the current tasks state.

## User Interactions Beyond Adding To A List

We’ve already demonstrated how to add items to a list by updating state. Now we’ll extend this by allowing users to remove or mark tasks as complete. These interactions will show how we modify state in different ways, making our component more dynamic and responsive to user input. Let’s walk through removing a task first.

### Removing a task

First, we’ll add a “remove” button next to each task. When clicked, it will remove that task from the tasks array by creating a new array without that task and updating state.

Update the `<ul>` element to be the following:

```{JavaScript}
<ul>
  {tasks.map((task, index) => (
    <li key={index}>
      {task} 
      <button onClick={() => {
        const updatedTasks = tasks.filter((_, i) => i !== index);
        setTasks(updatedTasks);
      }}>
        Remove
      </button>
    </li>
  ))}
</ul>
```

We've added a `<button>` object next to the `{task}` name. Since we want the `tasks` state variable to be changed to the same list only without that item, we create a variable `updatedTasks` equal to the current `tasks` list filtered to only include items not at the current index. The `filter` function takes in `(_, i)` where, since we don't care about the current item and only it's index, we use `_` for the first parameter.

### Marking tasks complete

To mark tasks as complete, we’ll store more information per task. Instead of just using a simple string `"new task 1"`, we’ll use an object `{ description: newTask, completed: false }` (where `newTask = "new task 1"`) for each task. Each object will have a description and a completed flag. Then we’ll add a button to toggle completion.

We'll change `<button onClick={() => setTasks([...tasks, newTask])}>` to the following:

```{JavaScript}
<button onClick={() => {
  setTasks([...tasks, { description: newTask, completed: false }]);
  setNewTask('');
}}>
```

This ensures each new task is an object with a description and a completed status (starting as false). After adding, we clear the input by calling `setNewTask('')`.

Now, let’s modify how we display each task. We’ll check the `completed` property. If it’s true, we can style the task differently—like crossing it out. After that, we’ll add a button to toggle the `completed` status.

```{JavaScript}
<ul>
  {tasks.map((task, index) => (
    <li key={index} style={{ textDecoration: task.completed ? 'line-through' : 'none' }}>
      {task.description}
      <button onClick={() => {
        const updatedTasks = tasks.map((t, i) => 
          i === index ? { ...t, completed: !t.completed } : t
        );
        setTasks(updatedTasks);
      }}>
        {task.completed ? 'Undo' : 'Complete'}
      </button>
      <button onClick={() => {
        const updatedTasks = tasks.filter((_, i) => i !== index);
        setTasks(updatedTasks);
      }}>
        Remove
      </button>
    </li>
  ))}
</ul>
```

```{JavaScript}
<button onClick={() => {
    const updatedTasks = tasks.map((t, i) => 
        i === index ? { ...t, completed: !t.completed } : t
    );
    setTasks(updatedTasks);
    }}>
    {task.completed ? 'Undo' : 'Complete'}
</button>
```

This code is added to each item. The text of the button is `"Undo"` if `task.completed` is `true` and `"Complete"` if `false`. When you click the button, it maps through all the tasks (`t` is the task state and `i` is the index) and runs the following expression `i === index ? { ...t, completed: !t.completed } : t`: if `i === index` meaning if we're mapping over the current item where the button was clicked next to, set the `updatedTasks` variable to the object `{ ...t, completed: !t.completed }` (`...t` takes all the parts of the object (in this case it's only the description, so we could have also just used `{ description: t, completed: !t.completed }`)), but `...t` is good practice if you have more object features. So we take all of what `t` currently is, and change `completed` equal to `!t.completed` the opposite of what the completed value is currently. If `i === index` is false, just set it to `t` (the current value unmodified). Once we've set `updatedTasks` correctly, we call `setTasks` to change the state of the task.

## Storing Tasks with PostgreSQL

At this point, we are storing tasks in-memory in React, but if we refresh the page, our tasks disappear. We'll use a PostgreSQL database and a simple Express (using Node.js) backend to handle storing and retrieving tasks. Then we’ll connect our frontend to that backend so tasks stay even after a page reload.

### Setting up the backend server

We’ll install Express, connect to PostgreSQL using a library like `pg`, and create routes to add, fetch, and delete tasks. Once that’s done, we’ll connect it to the React frontend.

In production, it's common to keep the frontend and backend as separate services or projects. Typically, you'd have a top-level directory—like a root project folder—and inside that, you’d have one folder for the frontend (your React app) and another for the backend (your Express or FastAPI server). Then, you deploy them separately—often with the frontend served by a static host (like Netlify or Vercel) and the backend hosted on a separate server (like Heroku, AWS, or Render).

Currently we have our `my-app` folder holding our React frontend. Lets create a better folder structure to separate our front end and backend.

1. Move up one directory with `cd ..`
2. Make the parent folder with `mkdir my-full-stack-app`
3. Create the frontend parent folder with `mkdir my-full-stack-app/frontend`
4. Create the backend parent folder with `mkdir my-full-stack-app/backend`
5. Move the React front end in that folder with `mv my-app my-full-stack-app/frontend/`
6. Enter into the root directory with `cd my-full-stack-app`

So the folder structure becomes:

```
my-full-stack-app
├── frontend
│   └── my-app
└── backend
```

Now we can begin working in the backend folder.

1. Enter into the backend folder with `cd backend`
2. Initialize a new default Node project using `npm init -y`
3. Install Express and pg by running `npm install express pg`
4. Install TypeScript development dependencies `npm install -D typescript tsx @types/node @types/express`
5. Create the TypeScript config file with `npx tsc --init`

You should see:

```
backend
├── node_modules/
└── package-lock.json
└── package.json
```

The `package.json` file is where all your project’s metadata and dependencies live—so it tracks what libraries you installed and other project info. The `package-lock.json` locks the exact versions of those packages so everyone working on the project uses the same versions. And the `node_modules` folder is where all the installed packages sit. Now that we have those, we can create the server file and set up our first route.

Create a file in the `backend/` folder called `index.js`