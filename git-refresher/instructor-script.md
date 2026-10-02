# Git refresher for energy modellers: instructor script

- **Instructor:** Will Usher
- **Length:** 150 minutes, including a 10-minute break
- **Audience:** junior data analysts and energy modellers who already use the `internal_training` notebooks and snakemake pipeline
- **Running example:** learners build a tiny PyPSA model, `dispatch.py`, then explore the real history of `internal_training`

> **Attribution**
>
> This script is derived from [Version Control with Git](https://swcarpentry.github.io/git-novice/),
> Copyright (c) [The Carpentries](https://carpentries.org/), licensed under
> [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
> It is not an official Carpentries lesson, and The Carpentries do not endorse it.
>
> **What changed**
>
> - The recipe example is replaced by a PyPSA dispatch model built from `notebooks/pypsa.ipynb`.
> - Episodes 1 to 9 are condensed into a 150-minute refresher. Episodes 10 to 14 are dropped.
> - SSH key setup moves to a pre-course checklist.
> - Two blocks are new: "Branches and pull requests" and "Explore a real project".
> - Instructor tips are adapted from the lesson's [instructor notes](https://swcarpentry.github.io/git-novice/instructor/instructor-notes.html).

## Contents

- [Original episodes and mapping](#original-episodes-and-mapping)
- [Schedule](#schedule)
- [Pre-course email](#pre-course-email)
- [Instructor prep](#instructor-prep)
- [Block 1. Why version control, setup check, create a repo](#block-1-why-version-control-setup-check-create-a-repo-10-min)
- [Block 2. Tracking changes](#block-2-tracking-changes-15-min)
- [Block 3. Exploring history](#block-3-exploring-history-15-min)
- [Block 4. Ignoring things](#block-4-ignoring-things-10-min)
- [Break](#break-10-min)
- [Block 5. Remotes in GitHub](#block-5-remotes-in-github-20-min)
- [Block 6. Collaborating](#block-6-collaborating-15-min)
- [Block 7. Conflicts](#block-7-conflicts-15-min)
- [Block 8. Branches and pull requests](#block-8-branches-and-pull-requests-20-min)
- [Block 9. Explore a real project: internal_training](#block-9-explore-a-real-project-internal_training-15-min)
- [Block 10. Wrap-up](#block-10-wrap-up-5-min)

## Original episodes and mapping

| Ep | Original episode | Used in block |
|---|---|---|
| 1 | [Automated Version Control](https://swcarpentry.github.io/git-novice/01-basics.html) | 1 |
| 2 | [Setting Up Git](https://swcarpentry.github.io/git-novice/02-setup.html) | 1 |
| 3 | [Creating a Repository](https://swcarpentry.github.io/git-novice/03-create.html) | 1 |
| 4 | [Tracking Changes](https://swcarpentry.github.io/git-novice/04-changes.html) | 2 |
| 5 | [Exploring History](https://swcarpentry.github.io/git-novice/05-history.html) | 3 |
| 6 | [Ignoring Things](https://swcarpentry.github.io/git-novice/06-ignore.html) | 4 |
| 7 | [Remotes in GitHub](https://swcarpentry.github.io/git-novice/07-github.html) | 5 |
| 8 | [Collaborating](https://swcarpentry.github.io/git-novice/08-collab.html) | 6 |
| 9 | [Conflicts](https://swcarpentry.github.io/git-novice/09-conflict.html) | 7 |

If you know the original lesson, this table maps its example onto ours.

| Original lesson | This script |
|---|---|
| `FINAL_rev.22.comments49...` thesis files | `model_run_FINAL_v3_fixed.ipynb`, `costs_2030_new_REALLY.csv` |
| `recipes` repository | `dispatch-model` repository |
| `desserts` nested repo | `git init` inside a cloned `internal_training` |
| `guacamole.md` | `dispatch.py` |
| `groceries.md`, `me.txt` | `README.md` with units |
| `data_cruncher.py` | `plot_dispatch.py` |
| `*.png`, `pictures/` | `*.lp`, `results/` |
| `hummus.md` (collaborator) | solar generator (collaborator) |
| guacamole instruction line (conflict) | gas `marginal_cost` line (conflict) |
| `guacamole.jpg` (binary conflict) | `dispatch.png` |

All model code comes from `notebooks/pypsa.ipynb`, cells `0fd31529` (network), `82db4bbf` (coal and gas),
`5fa7e393` (solar), `968e917d` (snapshots) and `28bb08a6` (time-varying load).
Every stage below was run with PyPSA 1.3.0, linopy 0.9.1 and HiGHS 1.15.1, and solves.

## Schedule

| # | Block | Ep | Min | Starts at |
|---|---|---|---|---|
| 1 | Why version control, setup check, create a repo | 1-3 | 10 | 0:00 |
| 2 | Tracking changes | 4 | 15 | 0:10 |
| 3 | Exploring history | 5 | 15 | 0:25 |
| 4 | Ignoring things | 6 | 10 | 0:40 |
| | Break | | 10 | 0:50 |
| 5 | Remotes in GitHub (SSH done before the session) | 7 | 20 | 1:00 |
| 6 | Collaborating (pairs) | 8 | 15 | 1:20 |
| 7 | Conflicts | 9 | 15 | 1:35 |
| 8 | Branches and pull requests (new) | none | 20 | 1:50 |
| 9 | Explore a real project: `internal_training` (new) | none | 15 | 2:10 |
| 10 | Wrap-up | | 5 | 2:25 |
| | **Total** | | **150** | ends 2:30 |

Sub-step minutes inside a block can add up to less than the block total. Use the spare minutes for that block's challenge.

If you run late, shorten the block 3 challenge and the block 7 binary conflict. Do not cut block 9.

## Pre-course email

Copy and send today.

```text
Subject: Git refresher tomorrow: 15 minutes of setup today, please

Hi all,

Tomorrow's git refresher is hands-on. Please do these five checks today,
so we can start on time. Reply to me if any step fails.

1. Install Git (version 2.28 or newer)
   Check with:   git --version
   Windows: install "Git for Windows" and use Git Bash.

2. GitHub account with two-factor authentication (2FA) switched on
   Send me your GitHub username.

3. SSH key added to GitHub and tested
   ssh-keygen -t ed25519 -C "your.name@openenergytransition.org"
   cat ~/.ssh/id_ed25519.pub
   Paste the key into GitHub > Settings > SSH and GPG keys > New SSH key.
   If GitHub shows "Configure SSO" next to the key, authorise it for
   open-energy-transition.
   Test with:    ssh -T git@github.com
   You should see:
   Hi <your-username>! You've successfully authenticated, but GitHub
   does not provide shell access.

4. Read access to the training repo
   git ls-remote git@github.com:open-energy-transition/internal_training.git
   You should see a few lines ending in HEAD and refs/heads/main.

5. Know your pair
   You will work in pairs. Your partner is listed below.
   Pair 1: ____ and ____
   Pair 2: ____ and ____
   Pair 3: ____ and ____

Optional: run the model
You only need Git tomorrow. If you also want to run the model, install uv
(https://docs.astral.sh/uv/) and fill the cache today:
   uv run --with "pypsa>=1" --with highspy python -c "import pypsa; print(pypsa.__version__)"
It should print 1.x.

Editor: we use nano in the terminal. If you prefer VS Code, check that
"code --version" works in your terminal.

Thanks,
Will
```

## Instructor prep

Do this before learners arrive.

1. Test the network and the projector. Make the terminal font big.
2. Use a short prompt: `export PS1='$ '`.
3. Run the `uv run` command from the email once, so your cache is warm.
4. Make a second folder for solo demos of blocks 6 and 7: you will clone your own repo as `~/Desktop/dispatch-model-at-work`.
5. Keep your own `internal_training` checkout open in a second terminal. Its `snakemake/results/` folder has the five figures for block 9. They are ignored, so a fresh clone does not have them.
6. Paste each snippet into the chat as you reach it, for anyone who falls behind.

---

## Block 1. Why version control, setup check, create a repo (10 min)

Original: [Ep 1](https://swcarpentry.github.io/git-novice/01-basics.html), [Ep 2](https://swcarpentry.github.io/git-novice/02-setup.html), [Ep 3](https://swcarpentry.github.io/git-novice/03-create.html)

### Say

- Version control is an unlimited undo. It also lets many people work in parallel.
- `git config --global` sets your name, email and editor once per machine.
- `git init` creates a `.git` folder. One project, one repository. Never put a repo inside another repo.

### 1.1 Open with a familiar folder (2 min)

Show this listing and ask who has a folder like it.

```text
model_run.ipynb
model_run_FINAL.ipynb
model_run_FINAL_v3_fixed.ipynb
costs_2030.csv
costs_2030_new.csv
costs_2030_new_REALLY.csv
```

Ask: which version produced the figure in last month's report? Git answers that question.

### 1.2 Check the setup (4 min)

Learners type their own name and email.

```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "your.name@openenergytransition.org"
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global core.editor "nano -w"
git config --list --global
```

VS Code users can run `git config --global core.editor "code --wait"` instead of the nano line.

Line endings: Windows users run `git config --global core.autocrlf true`. macOS and Linux users run `git config --global core.autocrlf input`.

Expected output of the last command:

```text
user.name=Your Name
user.email=your.name@openenergytransition.org
init.defaultbranch=main
pull.rebase=false
core.editor=nano -w
```

`pull.rebase false` is not in the original episode 2. Recent Git (tested on 2.43) otherwise stops the block 7 pull with `fatal: Need to specify how to reconcile divergent branches.`

### 1.3 Create the repository (4 min)

```bash
cd ~/Desktop
mkdir dispatch-model
cd dispatch-model
git init
ls -a
git status
```

Expected:

```text
Initialized empty Git repository in /home/you/Desktop/dispatch-model/.git/
```

```text
.  ..  .git
```

```text
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

### Challenge: a repo inside a repo

Sam has `internal_training` cloned at `~/Desktop/internal_training`. Sam runs:

```bash
cd ~/Desktop/internal_training
mkdir my-dispatch
cd my-dispatch
git init
```

What goes wrong, and how does Sam fix it?

<details>
<summary>Solution</summary>

`internal_training` already tracks everything below it. A second `.git` inside it makes a nested repo.
The outer repo lists `my-dispatch/` as untracked, and `git add my-dispatch` fails with:

```text
error: 'my-dispatch/' does not have a commit checked out
fatal: adding files failed
```

Fix: check where you are with `pwd`, then remove only the inner `.git`:

```bash
cd ~/Desktop/internal_training
pwd
rm -rf my-dispatch/.git
```

Better: keep new projects outside other repos, as we did with `~/Desktop/dispatch-model`.

</details>

### Tips

| Problem | Fix |
|---|---|
| Learners copy your name and email from the screen | Check `git config --list --global` on every screen you walk past. |
| Git is older than 2.28, so `init.defaultBranch` does nothing | Run `git branch -M main` after the first commit. |
| Git is older than 2.23, so `git restore` and `git switch` fail | Upgrade Git. Fallbacks: `git checkout -- file` and `git checkout -b name`. |
| macOS shows `.DS_Store` as untracked | Ignore it. Block 4 covers `.gitignore`. |
| Someone ran `git init` in the wrong place, such as their home folder | Move the folder somewhere safe first, then delete the stray `.git`. |

---

## Block 2. Tracking changes (15 min)

Original: [Ep 4: Tracking Changes](https://swcarpentry.github.io/git-novice/04-changes.html)

### Say

- The cycle is: edit, `git add`, `git commit`. The staging area holds what goes into the next snapshot.
- `git diff` shows unstaged changes. `git diff --staged` shows what you are about to commit.
- A good commit message is short and imperative, and says what the change does: "Add coal and gas generators".

### 2.1 First commit: the network (4 min)

```bash
nano dispatch.py
```

Type this. Save with Ctrl+O, Enter. Exit with Ctrl+X.

```python
import pypsa

n = pypsa.Network()

n.add("Bus", "gen_bus", carrier="transmission")
n.add("Bus", "load_bus")
n.add("Load", "load_1", bus="load_bus", p_set=500)
n.add(
    "Link",
    "transmission",
    bus0="gen_bus",
    bus1="load_bus",
    efficiency=0.93,
    p_nom=1000,
)
```

Say: two buses, a 500 MW load, and a 1000 MW link with 93% efficiency.

```bash
git status
git add dispatch.py
git status
git commit -m "Add two-bus network with 500 MW load"
git log
```

Expected (your hashes and dates will differ):

```text
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	dispatch.py
```

```text
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   dispatch.py
```

```text
[main (root-commit) cc6a7df] Add two-bus network with 500 MW load
 1 file changed, 15 insertions(+)
 create mode 100644 dispatch.py
```

```text
commit cc6a7df87f082a6a2bb650ceeb1c7053d9b98687 (HEAD -> main)
Author: Your Name <your.name@openenergytransition.org>
Date:   Thu Oct 1 17:08:50 2026 +0100

    Add two-bus network with 500 MW load
```

### 2.2 Second commit: generators, with the staging area (5 min)

Add these lines at the end of `dispatch.py`:

```python

n.add(
    "Generator",
    "coal",
    bus="gen_bus",
    p_nom=300,
    marginal_cost=3,
)
n.add(
    "Generator",
    "gas",
    bus="gen_bus",
    p_nom=400,
    marginal_cost=4,
)
```

Say: coal is 300 MW at 3 €/MWh. Gas is 400 MW at 4 €/MWh. One argument per line keeps diffs readable.

```bash
git diff
git add dispatch.py
git diff
git diff --staged
git commit -m "Add coal and gas generators"
```

Expected `git diff` before `git add`:

```diff
@@ -13,3 +13,18 @@ n.add(
     efficiency=0.93,
     p_nom=1000,
 )
+
+n.add(
+    "Generator",
+    "coal",
+    bus="gen_bus",
+    p_nom=300,
+    marginal_cost=3,
+)
+n.add(
+    "Generator",
+    "gas",
+    bus="gen_bus",
+    p_nom=400,
+    marginal_cost=4,
+)
```

After `git add`, plain `git diff` prints nothing. `git diff --staged` prints the same diff again.

```text
[main 02da03b] Add coal and gas generators
 1 file changed, 15 insertions(+)
```

### 2.3 Third commit: solve it, on your own (4 min)

Learners do the full cycle alone: edit, `git diff`, `git add`, `git commit`. Add at the end:

```python

n.optimize()
print(n.generators_t.p)
n.model.to_file("dispatch.lp")
```

Suggested message: `git commit -m "Solve dispatch and write LP file"`. Then:

```bash
git log --oneline
```

```text
f6e9d84 Solve dispatch and write LP file
02da03b Add coal and gas generators
cc6a7df Add two-bus network with 500 MW load
```

<details>
<summary>Full dispatch.py after step 2.3</summary>

```python
import pypsa

n = pypsa.Network()

n.add("Bus", "gen_bus", carrier="transmission")
n.add("Bus", "load_bus")
n.add("Load", "load_1", bus="load_bus", p_set=500)
n.add(
    "Link",
    "transmission",
    bus0="gen_bus",
    bus1="load_bus",
    efficiency=0.93,
    p_nom=1000,
)

n.add(
    "Generator",
    "coal",
    bus="gen_bus",
    p_nom=300,
    marginal_cost=3,
)
n.add(
    "Generator",
    "gas",
    bus="gen_bus",
    p_nom=400,
    marginal_cost=4,
)

n.optimize()
print(n.generators_t.p)
n.model.to_file("dispatch.lp")
```

</details>

Bonus, for anyone with uv:

```bash
uv run --with "pypsa>=1" --with highspy python dispatch.py
```

```text
Model status        : Optimal
Objective value     :  1.8505376344e+03
...
name       coal         gas
snapshot
now       300.0  237.634409
```

Say: the load bus needs 500 MW, so the generator bus must supply 500 / 0.93 = 537.6 MW. Coal is cheaper and runs at its full 300 MW. Gas covers the remaining 237.6 MW.
Running the model also creates `dispatch.lp`. Leave it for now. Block 4 deals with it.

### 2.4 Directories (1 min)

Git tracks files, not folders. An empty `results/` folder never shows up in `git status`.
That is why `internal_training` has `snakemake/results/.gitkeep`. We look at it in block 9.

### Challenge: document the units

Create `README.md` that states the units, and commit it. **Everyone does this one**, because block 3 counts commits back from it.

<details>
<summary>Solution</summary>

```bash
nano README.md
```

```markdown
# dispatch-model

Toy economic dispatch model in PyPSA.

Units: power in MW, costs in €/MWh.
```

```bash
git add README.md
git commit -m "Document units in README"
git log --oneline
```

```text
406e709 Document units in README
f6e9d84 Solve dispatch and write LP file
02da03b Add coal and gas generators
cc6a7df Add two-bus network with 500 MW load
```

</details>

### Tips

- Every learner must complete one full cycle alone. That is the point of step 2.3.
- Stuck in the log pager? Press `q`.
- Avoid `git commit -a` for now. Explicit `git add` stops you committing changes you forgot about.
- `git diff --word-diff` helps when only one number on a line changes.
- If the commit opens an editor, the learner forgot `-m`. Type the message, save and exit.
- Running the model prints two `FutureWarning`s (pandas string dtype, `include_objective_constant`) and a warning that `carriers ... are not defined`. All are harmless with PyPSA 1.3.

---

## Block 3. Exploring history (15 min)

Original: [Ep 5: Exploring History](https://swcarpentry.github.io/git-novice/05-history.html)

### Say

- `HEAD` is the latest commit. `HEAD~1` is the one before it. Hashes also work.
- `git restore` puts files back. It changes your working copy, not the history.
- `git revert` adds a new commit that undoes an old one. That is safe once others have pulled your work.

### 3.1 Break the model (3 min)

Open `dispatch.py` and delete the whole coal block (Ctrl+K cuts a line in nano). Save.

```bash
git diff
```

```diff
@@ -14,13 +14,6 @@ n.add(
     p_nom=1000,
 )
 
-n.add(
-    "Generator",
-    "coal",
-    bus="gen_bus",
-    p_nom=300,
-    marginal_cost=3,
-)
 n.add(
     "Generator",
     "gas",
```

Bonus: run it. Gas alone has 400 MW, but the model needs 537.6 MW.

```text
Model status        : Infeasible
...
Empty DataFrame
Columns: []
Index: [now]
```

### 3.2 Compare with older commits (4 min)

```bash
git log --oneline
git diff HEAD~2 dispatch.py
git show HEAD~1
git log --patch dispatch.py
```

`git diff HEAD~2 dispatch.py` compares your working copy with "Add coal and gas generators". It shows the deleted coal block and the added solve lines:

```diff
@@ -14,13 +14,6 @@ n.add(
     p_nom=1000,
 )
 
-n.add(
-    "Generator",
-    "coal",
-    bus="gen_bus",
-    p_nom=300,
-    marginal_cost=3,
-)
 n.add(
     "Generator",
     "gas",
@@ -28,3 +21,7 @@ n.add(
     p_nom=400,
     marginal_cost=4,
 )
+
+n.optimize()
+print(n.generators_t.p)
+n.model.to_file("dispatch.lp")
```

`git show HEAD~1` prints one commit: its message and its diff.

```text
commit f6e9d84...
Author: Your Name <your.name@openenergytransition.org>

    Solve dispatch and write LP file

diff --git a/dispatch.py b/dispatch.py
@@ -28,3 +28,7 @@ n.add(
+
+n.optimize()
+print(n.generators_t.p)
+n.model.to_file("dispatch.lp")
```

`git log --patch dispatch.py` shows every change to the file, newest first. Press `q` to quit.

### 3.3 Restore the last committed version (1 min)

```bash
git restore dispatch.py
git diff
```

`git diff` prints nothing. The coal block is back.

### 3.4 Get an old version of the file (3 min)

Use your own hash of "Add coal and gas generators" from `git log --oneline`.

```bash
git restore -s 02da03b dispatch.py
tail -3 dispatch.py
git status
git restore dispatch.py
tail -3 dispatch.py
```

```text
    p_nom=400,
    marginal_cost=4,
)
```

```text
	modified:   dispatch.py
```

```text
n.optimize()
print(n.generators_t.p)
n.model.to_file("dispatch.lp")
```

Say: `-s` is the source commit. Use the commit *before* the change you want to undo. `git restore -s HEAD~2 dispatch.py` does the same here.

### 3.5 Undo a commit with revert (4 min)

Change the load from 500 to 600 MW and commit it.

```diff
-n.add("Load", "load_1", bus="load_bus", p_set=500)
+n.add("Load", "load_1", bus="load_bus", p_set=600)
```

```bash
git add dispatch.py
git commit -m "Raise load to 600 MW"
git revert HEAD
git log --oneline
```

`git revert` opens nano with the message `Revert "Raise load to 600 MW"`. Save and exit.

```text
[main ec52cb2] Revert "Raise load to 600 MW"
 Date: ...
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```text
ec52cb2 Revert "Raise load to 600 MW"
d7f6149 Raise load to 600 MW
406e709 Document units in README
f6e9d84 Solve dispatch and write LP file
02da03b Add coal and gas generators
cc6a7df Add two-bus network with 500 MW load
```

Say: both commits stay in history. Anyone can see that 600 MW was tried and undone. With 600 MW the model still solves (coal 300, gas 345.2), so only the history tells you it was a mistake.

### Challenge: recover a broken script

Jo has been working on `plot_dispatch.py` for weeks. This morning's edits broke it. Which commands recover the last committed version?

1. `git restore`
2. `git restore plot_dispatch.py`
3. `git restore -s HEAD~1 plot_dispatch.py`
4. `git restore -s <hash of last commit> plot_dispatch.py`
5. Both 2 and 4

<details>
<summary>Solution</summary>

**5.** Options 2 and 4 both restore the latest committed version. `HEAD` is the last commit.
Option 3 restores the version from one commit earlier, which is not what Jo wants.
Option 1 fails:

```text
fatal: you must specify path(s) to restore
```

Use `git restore .` to restore every file.

</details>

### Tips

- **Detached HEAD.** If someone runs `git checkout 02da03b` without a file name, Git says `You are in 'detached HEAD' state`. Get back with `git switch main` (or `git checkout main`).
- **Silent bugs.** Change gas to `marginal_cost=-4,` and run it. It solves: gas 400, coal 137.6. Nothing crashes. Only `git diff` shows the typo.
- **Unstage a file.** `git restore --staged dispatch.py` takes it out of the staging area and keeps your edits.

---

## Block 4. Ignoring things (10 min)

Original: [Ep 6: Ignoring Things](https://swcarpentry.github.io/git-novice/06-ignore.html)

### Say

- Track what you write. Ignore what the model generates or downloads.
- `.gitignore` is a tracked file. The whole team shares the rules.
- `.gitignore` only affects untracked files. A file that is already tracked stays tracked.

### 4.1 Make some outputs (2 min)

If you ran the model, `dispatch.lp` already exists. Otherwise fake the outputs:

```bash
touch dispatch.lp
mkdir results
touch results/network.nc
git status
```

```text
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	dispatch.lp
	results/
```

### 4.2 Ignore them (3 min)

```bash
nano .gitignore
```

```text
*.lp
results/
```

```bash
git status
git add .gitignore
git commit -m "Ignore LP files and results"
git status
```

```text
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore
```

```text
[main f39d0f9] Ignore LP files and results
 1 file changed, 2 insertions(+)
 create mode 100644 .gitignore
```

```text
On branch main
nothing to commit, working tree clean
```

### 4.3 Ignored, not invisible (2 min)

```bash
git add dispatch.lp
git status --ignored
```

```text
The following paths are ignored by one of your .gitignore files:
dispatch.lp
hint: Use -f if you really want to add them.
hint: Turn this message off by running
hint: "git config advice.addIgnoredFile false"
```

```text
Ignored files:
  (use "git add -f <file>..." to include in what will be committed)
	dispatch.lp
	results/
```

### 4.4 The real repo (3 min, on your screen)

In your own `internal_training` checkout:

```bash
head -4 .gitignore
git status --short notebooks/
```

```text
# Snakemake demo
snakemake/results/*
snakemake/data/*
.snakemake
```

```text
?? notebooks/test.lp
?? notebooks/test2.lp
```

| Pattern | What it covers | Why ignore it |
|---|---|---|
| `snakemake/data/*` | downloaded technology-data costs and time series | `get_data.py` downloads them again. They are large, and the source owns them. |
| `snakemake/results/*` | `.nc` networks, figures, `statistics.csv` | The pipeline rebuilds them. |
| `.snakemake` | snakemake metadata, locks and logs | Specific to one machine and one run. |
| `.ipynb_checkpoints` | Jupyter autosave copies | Noise in every diff. |
| `*.lp` | **missing** | The notebook cell `n.model.to_file("test2.lp")` writes LP files, so they show up as untracked. |

### Challenge: keep one result

You want to ignore everything in `results/` except `results/summary.csv`. What goes in `.gitignore`?

<details>
<summary>Solution</summary>

```text
*.lp
results/*
!results/summary.csv
```

`results/` ignores the folder itself, so Git never looks inside it and the `!` rule has no effect.
`results/*` ignores the contents, so the `!` rule can re-include one file.
`internal_training` uses the same `snakemake/results/*` form.

</details>

### Tips

- To stop tracking a file you committed by mistake: `git rm --cached file`, add it to `.gitignore`, and commit.
- Rule order matters. A later `!pattern` overrides an earlier ignore pattern.

---

## Break (10 min)

Before the break, check that every learner has 7 commits in `git log --oneline` and a clean `git status`.

---

## Block 5. Remotes in GitHub (20 min)

Original: [Ep 7: Remotes in GitHub](https://swcarpentry.github.io/git-novice/07-github.html)

### Say

- Git is the tool. GitHub is a company that hosts Git repositories.
- A remote is a named URL. `origin` is only a convention. Your `internal_training` checkout may call it `upstream`.
- `git push` sends your commits. `git pull` fetches commits and merges them.

Draw this on the whiteboard:

```text
  your laptop                      GitHub
 +------------------+    push    +-----------------------+
 | dispatch-model   | ---------> | you/dispatch-model    |
 | remote: origin   | <--------- |                       |
 +------------------+    pull    +-----------------------+
                                        ^        |
                                   push |        | clone, pull
                                        |        v
                                 +-----------------------+
                                 | partner's laptop      |
                                 +-----------------------+
```

### 5.1 Check SSH (2 min)

SSH keys were set up before the session.

```bash
ssh -T git@github.com
```

```text
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

If this fails, pair the learner with a helper and continue. They can use HTTPS for today.

### 5.2 Create an empty repo on GitHub (4 min)

1. On GitHub, click **+** then **New repository**.
2. Owner: your personal account. Name: `dispatch-model`. Private is fine.
3. Leave **README**, **.gitignore** and **licence** unset. The repo must be empty.
4. Click **Create repository**. Copy the SSH URL.

### 5.3 Connect and push (6 min)

```bash
git remote add origin git@github.com:your-username/dispatch-model.git
git remote -v
git push -u origin main
```

```text
origin	git@github.com:your-username/dispatch-model.git (fetch)
origin	git@github.com:your-username/dispatch-model.git (push)
```

```text
To github.com:your-username/dispatch-model.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Say: `-u` links your local `main` to `origin/main`. From now on `git status` tells you if you are ahead or behind.

### 5.4 Look at it on GitHub (4 min)

Refresh the page. Click **commits**. Click a commit to see its diff. Open `.gitignore` and note that `dispatch.lp` is not there.

### 5.5 Pull (1 min)

```bash
git pull origin main
```

```text
From github.com:your-username/dispatch-model
 * branch            main       -> FETCH_HEAD
Already up to date.
```

### Challenge: the README checkbox

What would have happened if you had ticked "Add a README file" when creating the GitHub repo?

<details>
<summary>Solution</summary>

GitHub would have made its own first commit. Your local history and GitHub's history would share no commit.
`git push` is rejected, and `git pull origin main` refuses:

```text
fatal: refusing to merge unrelated histories
```

You can force it with `git pull --allow-unrelated-histories origin main`. Check both sides carefully before you do.
Your own `README.md` from block 2 then clashes with GitHub's README as `CONFLICT (add/add)`, which you resolve like any other conflict.
It is easier to create the GitHub repo empty.

</details>

### Tips

| Problem | Fix |
|---|---|
| Typo in the remote URL or name | Diagnose with `git remote -v`. Fix with `git remote set-url origin <url>` or `git remote rename <old> <new>`. |
| `Permission denied (publickey)` | The SSH key is not on GitHub, or not authorised for the organisation (SSO). |
| Push output looks different from yours | Normal. It varies with Git version and with `-u`. |

---

## Block 6. Collaborating (15 min)

Original: [Ep 8: Collaborating](https://swcarpentry.github.io/git-novice/08-collab.html)

### Say

- In each pair, one person is the **Owner** and one is the **Collaborator**. The Collaborator works in the Owner's repo.
- `git clone` copies a repo and sets up `origin` for you.
- The basic workflow: pull, edit, add, commit, push.

### 6.1 Give access (3 min)

1. **Owner:** on GitHub, open `dispatch-model` > **Settings** > **Collaborators** > **Add people**. Enter the partner's username.
2. **Collaborator:** accept the invite at https://github.com/notifications or from the email.

### 6.2 Collaborator: clone (2 min)

Clone into the Desktop, not inside another repo.

```bash
cd ~/Desktop
git clone git@github.com:owner-username/dispatch-model.git ~/Desktop/owner-dispatch-model
cd ~/Desktop/owner-dispatch-model
```

```text
Cloning into '/home/you/Desktop/owner-dispatch-model'...
```

### 6.3 Collaborator: add solar (4 min)

Add this block after the gas generator and before `n.optimize()`:

```python
n.add(
    "Generator",
    "solar",
    bus="gen_bus",
    p_nom=100,
    p_max_pu=0.5,
    marginal_cost=0,
)

```

```bash
git diff
git add dispatch.py
git commit -m "Add 100 MW solar at 50% availability"
git push origin main
```

```diff
@@ -29,6 +29,15 @@ n.add(
     marginal_cost=4,
 )
 
+n.add(
+    "Generator",
+    "solar",
+    bus="gen_bus",
+    p_nom=100,
+    p_max_pu=0.5,
+    marginal_cost=0,
+)
+
 n.optimize()
 print(n.generators_t.p)
 n.model.to_file("dispatch.lp")
```

```text
To github.com:owner-username/dispatch-model.git
   f39d0f9..d5eed96  main -> main
```

<details>
<summary>Full dispatch.py after step 6.3</summary>

```python
import pypsa

n = pypsa.Network()

n.add("Bus", "gen_bus", carrier="transmission")
n.add("Bus", "load_bus")
n.add("Load", "load_1", bus="load_bus", p_set=500)
n.add(
    "Link",
    "transmission",
    bus0="gen_bus",
    bus1="load_bus",
    efficiency=0.93,
    p_nom=1000,
)

n.add(
    "Generator",
    "coal",
    bus="gen_bus",
    p_nom=300,
    marginal_cost=3,
)
n.add(
    "Generator",
    "gas",
    bus="gen_bus",
    p_nom=400,
    marginal_cost=4,
)

n.add(
    "Generator",
    "solar",
    bus="gen_bus",
    p_nom=100,
    p_max_pu=0.5,
    marginal_cost=0,
)

n.optimize()
print(n.generators_t.p)
n.model.to_file("dispatch.lp")
```

</details>

### 6.4 Owner: review, then pull (4 min)

```bash
git fetch origin
git status
git diff main origin/main
git pull origin main
```

```text
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
```

`git diff main origin/main` shows the solar block before you merge it.

```text
Updating f39d0f9..d5eed96
Fast-forward
 dispatch.py | 9 +++++++++
 1 file changed, 9 insertions(+)
```

Bonus run: solar runs at 50 MW for free, so gas drops to 187.6 MW.

```text
Objective value     :  1.6505376344e+03
...
name       coal         gas  solar
snapshot
now       300.0  187.634409   50.0
```

### Challenge: who added solar?

The Owner wants to know which commit added the solar generator, and who made it, using only the command line.

<details>
<summary>Solution</summary>

```bash
git log -S'solar' --format='%h %an %s'
```

```text
d5eed96 Collaborator Name Add 100 MW solar at 50% availability
```

`git log -S` finds commits that add or remove a string. `git blame dispatch.py` shows the author of every line. We use both in block 9.

</details>

### Tips

- **Most common mistake:** pushing before pulling. Then the next pull may give a conflict. That is block 7.
- Clone from `~/Desktop`, never from inside another repo.
- Working solo? Clone your own repo a second time as `~/Desktop/dispatch-model-at-work` and play both roles.

---

## Block 7. Conflicts (15 min)

Original: [Ep 9: Conflicts](https://swcarpentry.github.io/git-novice/09-conflict.html)

### Say

- A conflict happens when two people change the same lines.
- Git never overwrites someone's work silently. It stops and marks the lines.
- Resolving a conflict is a modelling decision, not a git chore. Agree on the number.

### 7.1 Both: start in sync (1 min)

```bash
git pull origin main
grep -n "marginal_cost" dispatch.py
```

```text
22:    marginal_cost=3,
29:    marginal_cost=4,
38:    marginal_cost=0,
```

### 7.2 Collaborator: cheaper gas (2 min)

A new LNG contract lowers the gas cost. Change line 29:

```diff
-    marginal_cost=4,
+    marginal_cost=3.5,
```

```bash
git add dispatch.py
git commit -m "Lower gas cost to 3.5 EUR/MWh"
git push origin main
```

### 7.3 Owner: dearer gas, without pulling (3 min)

A fuel price rise raises the gas cost. Do **not** pull first. Change the same line:

```diff
-    marginal_cost=4,
+    marginal_cost=5,
```

```bash
git add dispatch.py
git commit -m "Raise gas cost to 5 EUR/MWh"
git push origin main
```

```text
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:owner-username/dispatch-model.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

### 7.4 Owner: pull and see the conflict (2 min)

```bash
git pull origin main
```

```text
Auto-merging dispatch.py
CONFLICT (content): Merge conflict in dispatch.py
Automatic merge failed; fix conflicts and then commit the result.
```

```bash
nano dispatch.py
```

```text
n.add(
    "Generator",
    "gas",
    bus="gen_bus",
    p_nom=400,
<<<<<<< HEAD
    marginal_cost=5,
=======
    marginal_cost=3.5,
>>>>>>> b73fa7368314d1d73c9593a39a3edd19043eac26
)
```

Say: above `=======` is your version (`HEAD`). Below it is the version from GitHub. The hash names the incoming commit.

### 7.5 Owner: resolve, add, commit, push (4 min)

The pair agrees on a value. Here they keep 5 €/MWh. Delete the markers so the block reads:

```python
n.add(
    "Generator",
    "gas",
    bus="gen_bus",
    p_nom=400,
    marginal_cost=5,
)
```

```bash
git add dispatch.py
git status
git commit -m "Merge gas cost change, keep 5 EUR/MWh"
git push origin main
```

```text
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
```

```text
[main 8f4d946] Merge gas cost change, keep 5 EUR/MWh
```

### 7.6 Collaborator: pull the merge (1 min)

```bash
git pull origin main
git log --oneline --graph -5
```

```text
Fast-forward
 dispatch.py | 2 +-
```

```text
*   8f4d946 Merge gas cost change, keep 5 EUR/MWh
|\
| * b73fa73 Lower gas cost to 3.5 EUR/MWh
* | 3bee973 Raise gas cost to 5 EUR/MWh
|/
* d5eed96 Add 100 MW solar at 50% availability
* f39d0f9 Ignore LP files and results
```

### 7.7 What changed in the model (2 min)

| Gas cost | Coal | Gas | Solar | Objective |
|---|---|---|---|---|
| 4 €/MWh (before) | 300.0 | 187.6 | 50.0 | 1650.5 |
| 3.5 €/MWh (Collaborator) | 300.0 | 187.6 | 50.0 | 1556.7 |
| 5 €/MWh (Owner, kept) | 300.0 | 187.6 | 50.0 | 1838.2 |

Say: the dispatch is the same, because gas is still dearer than coal. The total cost differs by 281 €. Git flagged the clash, and the pair had to decide. A silent merge would have hidden that.

### Challenge: a conflict in a plot

Both partners save a plot as `dispatch.png` and commit it. Fake one with random bytes:

```bash
head -c 1024 /dev/urandom > dispatch.png
git add dispatch.png
git commit -m "Add dispatch plot"
```

The Collaborator pushes first. The Owner pushes, gets rejected, and pulls. What happens, and how do you fix it?

<details>
<summary>Solution</summary>

```text
warning: Cannot merge binary files: dispatch.png (HEAD vs. 5f582336992f86f8ce00ff2478d4b67e7a015a65)
Auto-merging dispatch.png
CONFLICT (add/add): Merge conflict in dispatch.png
Automatic merge failed; fix conflicts and then commit the result.
```

Git cannot put markers in an image. Pick one side, then add and commit:

```bash
git checkout --theirs dispatch.png
git add dispatch.png
git commit -m "Use collaborator's dispatch plot"
git push origin main
```

Use `--ours` to keep your own. Better still: plots are outputs. Put them in `results/`, which is ignored.

</details>

### Tips

- **Forgot `git add` after fixing the file?** `git commit` fails with `Committing is not possible because you have unmerged files.` Run `git status`, then `git add dispatch.py`.
- **Want out?** `git merge --abort` returns to the state before the pull.
- **Divergent branches error?** The learner skipped `git config --global pull.rebase false` in block 1. Run it and pull again.
- Expect mistakes here, including your own. Everyone is tired by now.
- VS Code shows "Accept Current / Accept Incoming" buttons above conflict markers. Fine to use.

---

## Block 8. Branches and pull requests (20 min)

No original episode. This block is new.

### Say

- A branch is a movable label on a commit. `main` stays runnable while you try things.
- A pull request (PR) asks to merge a branch, with a review in between.
- After the merge, delete the branch. Its commits live on in `main`.

Swap roles: the **Collaborator** writes the branch and opens the PR. The **Owner** reviews and merges.

### 8.1 Collaborator: create a branch (2 min)

```bash
git switch main
git pull origin main
git switch -c add-time-dimension
git branch
```

```text
Switched to a new branch 'add-time-dimension'
```

```text
* add-time-dimension
  main
```

### 8.2 Collaborator: add time (5 min)

Make three edits. Set the snapshots right after creating the network, then give the load and solar one value per snapshot.

```diff
@@ -1,10 +1,11 @@
 import pypsa
 
 n = pypsa.Network()
+n.snapshots = [1, 2, 3]
 
 n.add("Bus", "gen_bus", carrier="transmission")
 n.add("Bus", "load_bus")
-n.add("Load", "load_1", bus="load_bus", p_set=500)
+n.add("Load", "load_1", bus="load_bus", p_set=[543, 320, 274])
 n.add(
     "Link",
     "transmission",
@@ -34,7 +35,7 @@ n.add(
     "solar",
     bus="gen_bus",
     p_nom=100,
-    p_max_pu=0.5,
+    p_max_pu=[0, 0.5, 0.2],
     marginal_cost=0,
 )
```

```bash
git diff
git add dispatch.py
git commit -m "Add three snapshots with varying load and solar"
git push -u origin add-time-dimension
```

```text
[add-time-dimension e3e08f9] Add three snapshots with varying load and solar
 1 file changed, 3 insertions(+), 2 deletions(-)
```

```text
remote: Create a pull request for 'add-time-dimension' on GitHub by visiting:
remote:      https://github.com/owner-username/dispatch-model/pull/new/add-time-dimension
To github.com:owner-username/dispatch-model.git
 * [new branch]      add-time-dimension -> add-time-dimension
branch 'add-time-dimension' set up to track 'origin/add-time-dimension'.
```

<details>
<summary>Full dispatch.py on the branch</summary>

```python
import pypsa

n = pypsa.Network()
n.snapshots = [1, 2, 3]

n.add("Bus", "gen_bus", carrier="transmission")
n.add("Bus", "load_bus")
n.add("Load", "load_1", bus="load_bus", p_set=[543, 320, 274])
n.add(
    "Link",
    "transmission",
    bus0="gen_bus",
    bus1="load_bus",
    efficiency=0.93,
    p_nom=1000,
)

n.add(
    "Generator",
    "coal",
    bus="gen_bus",
    p_nom=300,
    marginal_cost=3,
)
n.add(
    "Generator",
    "gas",
    bus="gen_bus",
    p_nom=400,
    marginal_cost=5,
)

n.add(
    "Generator",
    "solar",
    bus="gen_bus",
    p_nom=100,
    p_max_pu=[0, 0.5, 0.2],
    marginal_cost=0,
)

n.optimize()
print(n.generators_t.p)
n.model.to_file("dispatch.lp")
```

</details>

### 8.3 Collaborator: open the PR (3 min)

1. Open the link from the push output, or click **Compare & pull request** on GitHub.
2. Base: `main`. Compare: `add-time-dimension`.
3. Title: "Add time dimension". Description: what changed and how you checked it.
4. Under **Reviewers**, pick the Owner. Click **Create pull request**.

### 8.4 Owner: review and merge (5 min)

1. Open the PR. Click **Files changed**. Click a line to leave a comment.
2. Optional: test the branch locally.

   ```bash
   git fetch origin
   git switch add-time-dimension
   uv run --with "pypsa>=1" --with highspy python dispatch.py
   git switch main
   ```

   ```text
   Objective value     :  4.0254838710e+03
   ...
   name            coal         gas  solar
   snapshot
   1         300.000000  283.870968   -0.0
   2         294.086022   -0.000000   50.0
   3         274.623656   -0.000000   20.0
   ```

   Say: in hour 1 there is no sun, so gas runs. In hours 2 and 3 coal and solar cover the lower load. `-0.0` is solver rounding for zero.
3. Click **Review changes** > **Approve** > **Submit review**.
4. Click **Merge pull request** > **Confirm merge** > **Delete branch**.

### 8.5 Both: update and tidy up (3 min)

```bash
git switch main
git pull origin main
git branch -d add-time-dimension
git fetch --prune
git log --oneline --graph --all -4
```

```text
Updating 67ed3f2..0241e67
Fast-forward
 dispatch.py | 5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)
```

```text
Deleted branch add-time-dimension (was e3e08f9).
```

```text
*   0241e67 Merge pull request #1 from owner-username/add-time-dimension
|\
| * e3e08f9 Add three snapshots with varying load and solar
|/
...
```

The Owner has a local `add-time-dimension` only after testing it in 8.4. If `git branch -d` says `branch 'add-time-dimension' not found`, skip it.

### 8.6 The real thing (2 min)

`internal_training` got its snakemake pipeline the same way. In your checkout:

```bash
git log --oneline --graph --all
```

```text
...
* 7719016 Add timeseries and renewable plant to pypsa example
*   a3cd10d Merge pull request #1 from open-energy-transition/snakemake
|\
| * 8f237b7 Add snakemake examples
|/
* 55f69e0 Added simple pypsa implementation
```

Say: PR #1 brought the `snakemake` branch into `main`. GitHub deleted the branch after the merge, but the graph keeps its shape.

### Challenge: delete an unmerged branch

Make a branch, change the load, commit, and switch back to `main`. Is the change in `main`? Now delete the branch.

<details>
<summary>Solution</summary>

```bash
git switch -c try-high-load
nano dispatch.py
git add dispatch.py
git commit -m "Try higher load"
git switch main
grep p_set dispatch.py
git branch -d try-high-load
```

`main` still has `p_set=[543, 320, 274]`. The delete fails, because the commit exists only on that branch:

```text
error: the branch 'try-high-load' is not fully merged.
If you are sure you want to delete it, run 'git branch -D try-high-load'
```

`git branch -D try-high-load` deletes it anyway. Git protects work you have not merged.

</details>

### Tips

- `git switch <hash>` refuses to run. `git checkout <hash>` gives a detached HEAD. Go back with `git switch main`.
- The PR link appears only when pushing to GitHub. It is GitHub's message, not Git's.
- Keep branches short-lived and small. Long branches are where conflicts come from.

---

## Block 9. Explore a real project: internal_training (15 min)

No original episode. This block is new. All hashes below are real.

### Say

- History is a research tool. It tells you what changed, when, and why.
- `git log -S`, `git log --follow` and `git blame` answer "where did this come from?".
- A tracked file stays tracked, whatever `.gitignore` says.

### 9.1 Clone (2 min)

```bash
cd ~/Desktop
git clone git@github.com:open-energy-transition/internal_training.git
cd internal_training
git log --oneline --graph --all
```

```text
* 3d8063b (HEAD -> main, origin/main, origin/HEAD) Add comments showing where to split and aggregate sensitivity
* 38fef9e Allow solve script to run from CLI or snakefile
* abcbb48 Ignore snakemake outputs
* 47e10d3 Retain file structure
* fa6ba51 Add Snakefile
...
*   a3cd10d Merge pull request #1 from open-energy-transition/snakemake
|\
| * 8f237b7 Add snakemake examples
|/
* 55f69e0 Added simple pypsa implementation
* 2029132 Updated economic dispatch example
* bf68e07 Organise notebooks into subfolder
* 249e792 Add economic dispatch example using linopy
* 88b6c1a Add pre-commit configuration
* 18b5a31 Initial commit
```

### 9.2 The smallest diff (1 min)

```bash
git show 9e20ca8
```

```diff
commit 9e20ca89a8390b80ab6d45edb2fc518d756aca53
Author: willu47 <will.usher@openenergytransition.org>
Date:   Fri Sep 25 12:06:10 2026 +0100

    Update readme

diff --git a/README.md b/README.md
index ac75f0e..c1a86d0 100644
--- a/README.md
+++ b/README.md
@@ -1,2 +1,3 @@
 # internal_training
+
 Examples and exercises used for internal training
```

### 9.3 Follow a renamed file (2 min)

`bf68e07` moved the linopy notebook into `notebooks/`. Without `--follow`, its history stops at the move.

```bash
git log --oneline notebooks/economic_dispatch.ipynb
git log --oneline --follow notebooks/economic_dispatch.ipynb
git show --stat bf68e07
```

```text
7719016 Add timeseries and renewable plant to pypsa example
2029132 Updated economic dispatch example
bf68e07 Organise notebooks into subfolder
```

```text
7719016 Add timeseries and renewable plant to pypsa example
2029132 Updated economic dispatch example
bf68e07 Organise notebooks into subfolder
249e792 Add economic dispatch example using linopy
```

```text
 economic_dispatch.ipynb => notebooks/economic_dispatch.ipynb | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
```

### 9.4 A readable two-file diff (2 min)

```bash
git show --stat 38fef9e
git show 38fef9e -- snakemake/Snakefile
```

```text
    Allow solve script to run from CLI or snakefile

 snakemake/Snakefile        |  2 +-
 snakemake/scripts/solve.py | 14 ++++++++------
 2 files changed, 9 insertions(+), 7 deletions(-)
```

```diff
@@ -43,7 +43,7 @@ rule solve:
         rules.add_constraint.output
     output:
         "results/solved_cem_{CO2}.nc"
-    shell: "python scripts/solve.py {input} {output}"
+    script: "scripts/solve.py"
```

Say: one commit, two files, one idea. The Snakefile and the script change together, so nobody can check out a version where they disagree.

### 9.5 Side note: tracked files ignore .gitignore (1 min)

```bash
git show --stat 47e10d3
git ls-files snakemake/results
git check-ignore -v --no-index snakemake/results/.gitkeep
```

```text
    Retain file structure

 snakemake/data/.gitkeep    | 0
 snakemake/results/.gitkeep | 0
```

```text
snakemake/results/.gitkeep
```

```text
.gitignore:2:snakemake/results/*	snakemake/results/.gitkeep
```

Say: `47e10d3` committed `.gitkeep` first. `abcbb48` added the ignore rule later. The rule matches `.gitkeep`, but the file stays tracked, because `.gitignore` only affects untracked files.

### 9.6 Bug hunt (6 min)

Show this on your own screen, since results are ignored and a fresh clone has none:

```bash
cd snakemake
md5sum results/figure_*.png
cat results/statistics.csv
```

```text
f779b5a0d5dc489e71a9371b9ba1e455  results/figure_0.png
f779b5a0d5dc489e71a9371b9ba1e455  results/figure_100.png
f779b5a0d5dc489e71a9371b9ba1e455  results/figure_150.png
f779b5a0d5dc489e71a9371b9ba1e455  results/figure_25.png
f779b5a0d5dc489e71a9371b9ba1e455  results/figure_50.png
```

```text
,battery storage,hydrogen storage underground,offwind,solar
150.0,0.00428947665392,0.040476478598169996,0.041113562731650004,0.0194058989309
100.0,0.00428947665392,0.040476478598169996,0.041113562731650004,0.0194058989309
...
```

Say: five CO2 limits, from 0 to 150 Mt, and five identical results. Something is wrong. Let's use git to find out what.

Learners run these in their clone:

```bash
git log -S'/kW' --oneline
git grep -n '/kW'
git blame -L 37,39 snakemake/scripts/get_costs.py
```

```text
8f237b7 Add snakemake examples
2029132 Updated economic dispatch example
249e792 Add economic dispatch example using linopy
```

```text
snakemake/pypsa-cem.py:101:costs.loc[costs.unit.str.contains("/kW"), "value"] *= 1e3
snakemake/pypsa-cem.py:102:costs.unit = costs.unit.str.replace("/kW", "/MW")
```

```text
8f237b71 (willu47 2026-09-24 23:01:21 +0100 37)     annuity_factor = annuity(costs["discount rate"], costs["lifetime"])
8f237b71 (willu47 2026-09-24 23:01:21 +0100 38)
8f237b71 (willu47 2026-09-24 23:01:21 +0100 39)     costs["capital_cost"] = (annuity_factor + costs["FOM"] / 100) * costs["investment"]
```

Say: `git log -S` lists commits that add or remove a string. `/kW` appears in the linopy notebook and in `pypsa-cem.py`. It never appears in `get_costs.py`.

### Challenge: why are all five runs identical?

`snakemake/pypsa-cem.py` is the original single script. `snakemake/scripts/get_costs.py` is the pipeline step split out of it. Both read the same technology-data CSV. Compare how they prepare costs. What is missing, and what does it do to the model?

<details>
<summary>Solution</summary>

`pypsa-cem.py:101-102` converts every `/kW` value to `/MW`:

```python
costs.loc[costs.unit.str.contains("/kW"), "value"] *= 1e3
costs.unit = costs.unit.str.replace("/kW", "/MW")
```

`get_costs.py` never does this. Investment costs stay in €/kW but are used as €/MW, so every capital cost is 1000 times too low.
Solar comes out at 64.6 €/MW/a instead of about 64,560 €/MW/a.
With capacity that cheap, the model builds only renewables and storage, so the CO2 limit never binds and every run gives the same answer.

Both scripts came in with `8f237b7`, so the bug dates from the split, not from a later edit.

</details>

Do not fix the bug in the main flow. It makes a good homework exercise.

### Optional finale: fix it live with a PR (your call on the day)

```bash
cd ~/Desktop/internal_training
git switch -c fix-kw-units
nano snakemake/scripts/get_costs.py
git diff
```

```diff
     costs = input_data
 
+    costs.loc[costs.unit.str.contains("/kW"), "value"] *= 1e3
+    costs.unit = costs.unit.str.replace("/kW", "/MW")
+
     defaults = {
```

```bash
git add snakemake/scripts/get_costs.py
git commit -m "Convert /kW costs to /MW in get_costs"
git push -u origin fix-kw-units
```

Open the PR and ask a learner to review it. Before merging, rerun the pipeline and check that the five `md5sum` values now differ.
`get_costs` is a `shell:` rule, so force it, for example `snakemake --cores 1 --forcerun get_costs all` from `snakemake/`.

### Tips

- Learners who cloned `internal_training` earlier may have a local `snakemake` branch or a stale `origin/snakemake`. `git fetch --prune` removes the stale remote branch.
- `git blame` prints `8f237b71`, one character longer than `8f237b7` in `git log`. Both name the same commit.
- `git log -p -S'/kW'` shows the matching diffs too.

---

## Block 10. Wrap-up (5 min)

### A typical work session

Ask learners to put the steps in order before you show the table.

| Order | Action | Command |
|---|---|---|
| 1 | Update local | `git pull origin main` |
| 2 | Branch, if the change is bigger than a typo | `git switch -c my-change` |
| 3 | Make changes | `nano dispatch.py` |
| 4 | Stage | `git add dispatch.py` |
| 5 | Commit | `git commit -m "Describe the change"` |
| 6 | Update remote | `git push origin my-change`, then open a PR |
| 7 | Celebrate | |

### Cheat sheet

| Task | Command |
|---|---|
| What changed? | `git status`, `git diff`, `git diff --staged` |
| Save a snapshot | `git add <file>`, `git commit -m "..."` |
| History | `git log --oneline --graph --all`, `git show <hash>` |
| Find a string in history | `git log -S'text' --oneline` |
| Follow a renamed file | `git log --follow <file>` |
| Who wrote this line? | `git blame <file>` |
| Throw away edits | `git restore <file>` |
| Old version of a file | `git restore -s <hash> <file>` |
| Undo a shared commit | `git revert <hash>` |
| Sync | `git pull origin main`, `git push origin main` |
| Branches | `git switch -c <name>`, `git switch main`, `git branch -d <name>` |
| Escape a merge | `git merge --abort` |

### Close

1. Ask each learner for one thing that worked and one thing to improve.
2. Point to the full lesson for self-study: https://swcarpentry.github.io/git-novice/
3. Homework: fix the `/kW` bug in `internal_training` on a branch and open a PR, unless you did it live.
