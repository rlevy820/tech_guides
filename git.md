# Git

Git is a technology that lets you keep version control of your projects and collaborate with teammates. Think of it like Google Docs version history — but for code.

I'm going to explain Git through the metaphor of writing a book. First, how you'd use Git writing a book alone (solo dev), then how you'd use it writing a book with other people (collaborative dev).

---

## Setting Up the Metaphor

| Metaphor | Git Equivalent |
|---|---|
| Writing a full book | Building a full codebase |
| Writing a chapter | Building a feature |
| The main draft | The `main` branch |
| A working draft | A feature branch |
| Version history | Git commits |

### Local vs. Remote vs. GitHub

Before diving in, there are three "places" you need to have a mental model for:

- **Local** — your machine. This is where you actually write. Your branches, your commits, your changes all live here first.
- **Remote** — a shared version of the project hosted on a server. This is the single source of truth that everyone syncs to.
- **GitHub** — the platform that hosts your remote. It's where you push your work so others can see it, and where you pull others' work down to your machine.

Think of it like this: your **local** is your personal notebook. **GitHub** is the shared filing cabinet in the office. You write in your notebook, and when something is ready, you file it. When you want to see what a teammate filed, you go check the cabinet.

Before anything, you need to initialize Git in your project folder:

```bash
git init
```

This tells Git to start tracking changes in that directory. Everything from here on lives inside that folder.

---

## Writing a Book by Yourself (Solo Dev)

### Branches are places you can write

When you `git init`, Git creates a default branch called `main`. Think of `main` as your official, clean draft — the version you'd hand to a publisher.

The rule of thumb: **you never write directly in `main`.** Instead, you create a separate branch for each chapter (feature) you're working on, write there, and merge it into `main` when it's ready.

### Example: Drafting Chapter One

You want to start writing Chapter One, so you create a branch for it:

```bash
git checkout -b chapter-one
```

This creates a new branch called `chapter-one` and switches you into it. Now you're writing in your own isolated draft — `main` is untouched.

You write for a while, and you want to save your progress. That's a **commit**:

```bash
git add .
git commit -m "Draft intro scene for chapter one"
```

`git add .` stages all your changes (tells Git what to include), and `git commit` saves a snapshot of that moment with a message describing what you did.

### Mid-chapter idea: a Prologue

While working on Chapter One, you get an idea for a Prologue. You don't want to mix that work into your Chapter One draft, so you create a new branch off of `main`:

```bash
git checkout main
git checkout -b prologue
```

You write the prologue, commit your work, and decide it's ready to become part of the official draft. Here's how you bring it into `main`:

```bash
# First, make sure main is up to date
git checkout main

# Merge the prologue branch into main
git merge prologue
```

`main` now includes the prologue. You can go back to Chapter One:

```bash
git checkout chapter-one
```

Your Chapter One draft is exactly where you left it — the prologue work never interfered with it.

### The solo dev loop, summarized:

```bash
git checkout -b chapter-two        # Start a new branch for new work
# ...write, write, write...
git add .                          # Stage your changes
git commit -m "Finished chapter two climax"   # Save a snapshot
git checkout main                  # Switch to main
git merge chapter-two              # Bring the finished chapter into main
```

---

## Writing a Book with Other People (Collaborative Dev)

Now imagine you have a co-author: Jim. Here's the key thing to understand about collaborative development — **you and Jim are working asynchronously.** You're not on a call together. You're not checking in every five minutes. You're both heads-down in your own local environments, writing independently, and neither of you automatically knows what the other is doing.

This is why Git + GitHub exists in the first place.

The setup looks like this:

- **Your local** — your machine. Your branches, your commits, your drafts.
- **Jim's local** — his machine. His branches, his commits, his drafts.
- **GitHub (the remote)** — the shared filing cabinet. The single source of truth. Neither of you can see what the other is doing until one of you pushes to GitHub.

When you first get the project, you **clone** the remote repo to your local machine:

```bash
git clone <repo-url>
```

Jim does the same on his machine. Now you both have a local copy, and you're both connected to the same remote on GitHub.

### Example: A Logic Conflict in Chapter One

You and Jim both decide to work on Chapter One. Neither of you tells the other — you're async. You each create a branch on your own machine and start writing:

```bash
# You, on your machine
git checkout -b chapter-one

# Jim, on his machine (at the same time, independently)
git checkout -b chapter-one
```

You write: *"The dog was given away because it kept biting."*

Jim writes, independently, with no idea what you wrote: *"The dog would never be given away — the family loved it too much."*

Jim finishes first. He commits his work locally, then **pushes** it up to GitHub so it becomes part of the shared remote:

```bash
# Jim
git add .
git commit -m "Add chapter one - dog subplot"
git push origin chapter-one
# (Jim opens a pull request on GitHub and it gets merged into main)
```

At this point, `main` on GitHub now contains Jim's version. **You have no idea this happened.** You've been writing on your local machine the whole time.

Now you're ready to submit your work. But here's the problem: if you just pushed your version of Chapter One directly, you'd be overwriting Jim's changes on the remote — because your local copy of `main` is still the old version, before Jim merged anything.

This is exactly why you **pull before you merge.** You need to ask GitHub: *"What's changed since I last looked?"*

```bash
git checkout main
git pull origin main
```

`git pull` fetches the latest version of `main` from GitHub and updates your local copy. Now you can see Jim's changes — for the first time — right there on your machine.

You go back to your branch and merge `main` into it. This brings Jim's changes into your draft so you can reconcile them before submitting:

```bash
git checkout chapter-one
git merge main
```

Git tries to combine both versions — and flags a **merge conflict** because two sentences in the same spot contradict each other. It'll look something like this in your file:

```
<<<<<<< HEAD (your version)
The dog was given away because it kept biting.
=======
The dog would never be given away — the family loved it too much.
>>>>>>> main (Jim's version)
```

This is Git saying: *"I don't know which version to keep. You decide."*

You call Jim, talk it through, and agree to keep his version (with a small tweak for your plot). You edit the file to reflect the resolved version, removing all the conflict markers, then commit the resolution:

```bash
git add .
git commit -m "Resolve merge conflict in chapter one - keep Jim's dog subplot"
```

Now your branch has both your changes and Jim's changes, cleanly reconciled. You push it up to GitHub:

```bash
git push origin chapter-one
```

And open a pull request to merge it into `main`.

### Why the order matters

The reason you pull `main` → merge into your branch → then push (rather than just pushing directly) is because **you're async.** You never know what got merged into `main` while you were heads-down writing. Pulling first means you're always reconciling on your local machine — where you can actually read and fix conflicts — before anything touches the shared remote. If you pushed without pulling, you'd either overwrite someone else's work or GitHub would reject your push outright because your history is behind.

### The collaborative dev loop, summarized:

```bash
git checkout -b my-feature         # Start your own branch (local)
# ...do your work...
git add .
git commit -m "Description of what you did"   # Save locally

git checkout main
git pull origin main               # Sync your local main with GitHub (the remote)

git checkout my-feature
git merge main                     # Bring those remote changes into your local branch
# (resolve any conflicts if needed)

git push origin my-feature         # Push your branch up to GitHub
# (open a pull request on GitHub to merge into main)
```

---

## Quick Reference

| Command | What it does |
|---|---|
| `git init` | Start tracking a folder with Git |
| `git clone <url>` | Copy a remote repo to your machine |
| `git checkout -b <name>` | Create and switch to a new branch |
| `git add .` | Stage all changes |
| `git commit -m "..."` | Save a snapshot with a message |
| `git push origin <branch>` | Push your branch to the remote |
| `git pull origin main` | Pull the latest version of main |
| `git merge <branch>` | Merge a branch into your current one |