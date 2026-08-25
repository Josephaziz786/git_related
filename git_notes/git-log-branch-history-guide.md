# Git Log & Branch History: Comprehensive Guide & Reference

> **Based on:** *Git Tutorial (Part 7) – Git Log Fully Explained* (by Ben Kadel, timestamps ~06:00 – 08:30) & Advanced Real-World Mechanics.

---

## 1. Pager Navigation & Commit Ordering (06:00 – 06:30)

When you run `git log`, Git pipes long outputs into a terminal pager program (typically `less`).

### Key Controls
* **Quit Pager:** Press `q` to immediately exit the pager and return to your terminal prompt.
* **Line Navigation:**
  * `j` or `Down Arrow`: Scroll down one line (supports numeric multipliers like `10j`).
  * `k` or `Up Arrow`: Scroll up one line.
  * `Enter`: Scroll down one line (and resets the default step size if given a prefix count).
* **Page Navigation:**
  * `Space` or `f`: Jump forward one full screen/window.
  * `b`: Jump backward one full screen/window.
  * `d` / `u`: Scroll down / up half a window.
* **Jumping to Extremes:**
  * `g` or `gg`: Jump straight to the top (newest commit).
  * `G`: Jump straight to the bottom (oldest/initial commit).
* **Searching:**
  * `/pattern`: Search forward for matching commit messages or hashes.
  * `n`: Go to the next search match.
  * `N`: Go to the previous match.

### Commit Ordering
* Commits are ordered in **reverse chronological order**:
  * **Top:** The most recent commit on the checked-out branch.
  * **Bottom:** The earliest/initial commit that started the repository history.

---

## 2. Core Rule: How `git log` Determines What to Display (06:30 – 08:30)

> **Golden Rule:** By default, `git log` only displays the commits that led up to the current state of the **currently checked-out branch (`HEAD`)**.

### What Happened in the Tutorial Video
1. **On `master`:** Running `git log` produced a long list of commits, including merged Pull Requests (`merge pull request...`), requiring the `less` pager.
2. **Switching to `feature`:** Running `git checkout feature` followed by `git log` displayed **only 5 commits** and did not open a pager.
3. **Why?** 
   * The `feature` branch had branched off early in the repository lifecycle.
   * Work and Pull Requests merged into `master` after that point **do not exist in the ancestry line of `feature`**.
   * Therefore, `git log` on `feature` only walks backwards through its own 5 historical ancestor commits.

---

## 3. Real-World Deep Dive: "Why do I see 1-year-old commits on my feature branch?"

In enterprise and production repositories, running `git log` on a brand new feature branch will still list commits made months or years ago by other developers.

### Why This Happens (Git's Directed Acyclic Graph - DAG)
A Git branch is **not an isolated sandbox or folder**; it is simply a lightweight, movable pointer to a commit hash.

```text
Commit A (1 year ago) ---> Commit B (6 months ago) ---> Commit C (Base commit on main)
                                                              \
                                                               Commit D ---> Commit E (HEAD -> feature)
```

1. When you create `feature` from `main`, `feature` starts at commit `C`.
2. When you run `git log`, Git starts at `HEAD` (Commit `E`) and follows parent commit references backward:
   $$\text{Commit E} \rightarrow \text{Commit D} \rightarrow \text{Commit C} \rightarrow \text{Commit B} \rightarrow \text{Commit A}$$
3. Because Commits `A`, `B`, and `C` are direct ancestors of your branch, Git includes all of them in the log output.

In the tutorial video, the demo repo was created just minutes prior and only had 3 base commits total, making it look like only "feature commits" were shown.

---

## 4. Practical Commands: Viewing Only Your Branch Changes

When working on a feature branch in a large project, use these commands to filter out older historical commits from `main`:

### 1. Show Only New Commits on Your Branch (Excluding `main`)
```bash
# Full details
git log main..HEAD

# Compact single-line summary (Recommended for PR reviews)
git log --oneline main..HEAD
```
* **Explanation:** `main..HEAD` means *"Show all commits reachable from `HEAD` (my current branch) that are NOT reachable from `main`."*

### 2. Visualize Divergence & Branch Graphs
```bash
git log --graph --oneline --all --decorate
```
* Shows a visual ASCII tree of all branches, merges, and commit relationships across the entire repository.

### 3. Filter History by Author
```bash
git log --author="Your Name" --oneline
```

### 4. Limit the Number of Commits Displayed
```bash
# Show only the last 5 commits
git log -n 5 --oneline
```

---

## 5. Summary Reference Table

| Goal | Command |
|---|---|
| View entire branch history from top to initial commit | `git log` |
| View compact one-line log | `git log --oneline` |
| View **only commits created on current feature branch** | `git log --oneline main..HEAD` |
| View visual branch tree graph across all branches | `git log --graph --oneline --all` |
| Exit `git log` pager | Press `q` |
| Jump to oldest commit | Press `G` |
| Jump to newest commit | Press `g` or `gg` |

