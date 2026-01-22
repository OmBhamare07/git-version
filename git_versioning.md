# Git Versioning: Understanding Areas and Reverting Changes

## Introduction
Git is a **distributed version control system** that helps developers track changes in source code, manage multiple versions, and collaborate efficiently. One of the most important concepts in Git is understanding how files move between different **areas (states)** and how changes can be reverted or reset safely.

This document explains:
- The **three main areas in Git**
- How files move between these areas
- Commands to restore files from **Staging → Working Directory**
- Commands to move changes from **Commit History → Working Directory** using different strategies

---

## The Three Areas in Git

[![ver.png](https://i.postimg.cc/BbrrxtxT/ver.png)](https://postimg.cc/zHk2Nz2v)

### 1. Working Directory (Current Area)
- This is where you actively work on files.
- Any changes made to files are first reflected here.
- Files in this area are **not yet tracked for commit**.

Example:
- Editing a file using a code editor
- Creating a new file

[![ver1.png](https://i.postimg.cc/vHc0LQTb/ver1.png)](https://postimg.cc/BXf5Jf2V)
- git status is untracked

---

### 2. Staging Area 
- This area acts as a **preview zone** before committing changes.
- Files added using `git add` are placed here.
- It allows selective commits.

Purpose:
- Decide exactly what goes into the next commit

[![ver2.png](https://i.postimg.cc/jj0KYgmb/ver2.png)](https://postimg.cc/w1VnhV9f)
- git status is A (added)

---

### 3. Commit Area 
- This is the permanent record of changes.
- Each commit creates a snapshot with a unique **commit ID**.
- Commits are stored safely in Git history.

[![ver3.png](https://i.postimg.cc/qq9D6Y8J/ver3.png)](https://postimg.cc/QKJJRf4w)

---

## Flow of Files Between Git Areas

Working Directory → Staging Area → Commit Area

Commands used:
- `git add` → Working to Staging
- `git commit` → Staging to Commit

---

## 1. Sending File from Staging Area to Working Directory

### Scenario
A file was added to the staging area using `git add`, but you want to **remove it from staging without deleting changes**.

### Command
```
git restore --staged <index.html>
```

### Explanation
- Removes the file from the **staging area**
- Keeps the changes in the **working directory**
- Useful when a file was added by mistake

### Example Workflow
1. Modify a file
2. Run `git add index.html`
3. Realize it should not be staged
4. Run `git restore --staged index.html`

[![ver4.png](https://i.postimg.cc/s29Gq0Kx/ver4.png)](https://postimg.cc/nsrLQ2yf)

Result:
- File is unstaged
- Changes remain editable

---

## 2. Sending File from Commit Area to Working Directory

### Scenario
There are three commits:
- Commit A (old)
- Commit B (stable)
- Commit C (latest, problematic)

You want to undo changes introduced in **Commit C**.

---

## Method 1: git revert <commit-id-of-C>

### Command
```
git revert <commit-id-of-C>
```

### Explanation
- Creates a **new commit** that reverses changes from Commit C
- Original commit history remains intact
- Safe for shared repositories

### Result
Commit history:
- A → B → C → Revert Commit

### Use Case
- Best for production
- When history should not be altered

[![ver5.png](https://i.postimg.cc/GpWKbN12/ver5.png)](https://postimg.cc/R3TKRgh5)

---

## Method 2: git reset --hard <commit-id-of-B>

### Command
```
git reset --hard <commit-id-of-B>
```

[![ver-7.png](https://i.postimg.cc/59YHB0vD/ver-7.png)](https://postimg.cc/3WhJ73tZ)

### Explanation
- Moves HEAD to Commit B
- Deletes commits after B (including C)
- Removes changes from:
  - Commit area
  - Staging area
  - Working directory

### Result
- Commit C is completely removed
- Working directory matches Commit B

### ⚠ Warning
- **Data loss occurs**
- Not recommended for shared repositories

---

## Method 3: git reset --soft <commit-id-of-B>

### Command
```
git reset --soft <commit-id-of-B>
```

[![ver6.png](https://i.postimg.cc/zfFvfbsC/ver6.png)](https://postimg.cc/2q3rc68y)

### Explanation
- Moves HEAD to Commit B
- Keeps changes from Commit C in the **staging area**
- No files are deleted

### Result
- Commit C removed from history
- Changes ready to be recommitted

### Use Case
- Modify commit message
- Squash or reorganize commits

---

## Comparison of Revert and Reset

| Command | History Changed | Working Directory | Safe for Shared Repo |
|--------|----------------|-------------------|----------------------|
| git revert | No | No change | Yes |
| git reset --soft | Yes | Preserved | No |
| git reset --hard | Yes | Deleted | No |

---

## Summary
- Git has **three areas**: Working Directory, Staging Area, Commit Area
- `git restore --staged` unstages files safely
- `git revert` safely undoes commits
- `git reset --soft` keeps changes for recomposing commits
- `git reset --hard` completely removes commits and changes

Understanding these commands is essential for **version control mastery** and **safe collaboration**.

---


