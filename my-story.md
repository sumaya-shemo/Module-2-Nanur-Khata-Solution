# Git Practice — Nanu's Khata

## Task A — Configure Git and initialize a repository

```bash
git config --global user.name "sumaya-shemo"
git config --global user.email "sumayashemo13@gmail.com"   


mkdir nanur-khata
cd nanur-khata
git init
```

`git config --global` sets your Git identity. `git init` creates a Git repository in the current folder.

## Task B — Create and stage bhapa-pitha.md

In Git Bash, create the Markdown recipe file:

```bash
echo "# Bhapa Pitha Recipe

- Rice flour: 2 cups
- Gur (date palm jaggery): 2 cups
- Grated coconut: 1 cup
- Salt: A pinch

Instructions:
1. Mix rice flour and salt.
2. Add gur and grated coconut.
3. Steam until cooked." > bhapa-pitha.md
```

Then check and stage it:

```bash
cat bhapa-pitha.md
git status
git add bhapa-pitha.md
git status
```

## Task C — Correct the recipe and compare diffs

Nanu says: **"One and a half cups of gur, not two!"**

Open the file:

```bash
notepad bhapa-pitha.md
```

Change:

```diff
- Gur (date palm jaggery): 2 cups
+ Gur (date palm jaggery): 1.5 cups
```

Then run:

```bash
git diff
git diff --staged
```

- `git diff` compares the working directory against the staging area (unstaged edits).
- `git diff --staged` compares the staging area against the latest commit (staged edits). Before the first commit, it compares staged files against an empty tree.

Because you staged the original file and then edited it, `git diff` shows the gur correction, while `git diff --staged` shows the original staged recipe.

Stage the correction and inspect again:

```bash
git add bhapa-pitha.md
git diff
git diff --staged
```

Now `git diff` should be empty for this file; `git diff --staged` shows the corrected recipe ready to commit.

## Task D — Commit the recipe

```bash
git commit -m "Add bhapa pitha recipe with corrected gur quantity"
git log --oneline
```

Use meaningful commit messages that explain what changed.

## Task E — Unstage bazar-list.txt without deleting it

```bash
echo "Rice flour, gur, coconut" > bazar-list.txt
git add bazar-list.txt
git status
git reset bazar-list.txt
git status
cat bazar-list.txt
```

`git reset bazar-list.txt` removes the file from the staging area, **not** the working directory. Since this file has never been committed, it becomes untracked. A modern alternative is `git restore --staged bazar-list.txt`.

## Command summary

| Command | Purpose |
| --- | --- |
| `git config --global` | Configure name and email |
| `git init` | Initialize a repository |
| `git status` | Inspect working and staging states |
| `git add <file>` | Stage a file |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |
| `git commit -m "message"` | Commit staged changes |
| `git log --oneline` | View short commit history |
| `git reset <file>` | Unstage a file without deleting it |
| `cat <file>` | Display file contents |
__________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

# Nanu's Secret Spice Blend — Git `.gitignore` Practice


## Task A — Create secret files, temporary files, and a draft folder

Open Git Bash and navigate into the repository:

```bash
cd nanur-khata
```

Create the secret spice file:

```bash
echo "Nanu's secret spice blend: Cardamom, Cinnamon, Cloves" > secret-masala.txt
```

Create several temporary files:

```bash
echo "Temporary recipe notes" > recipe.tmp
echo "Temporary cooking data" > cooking.tmp
echo "Temporary shopping list" > shopping.tmp
```

Create a draft folder and sample files:

```bash
mkdir draft
echo "Draft biryani recipe" > draft/biryani.md
echo "Draft halua recipe" > draft/halua.md
```

The project now contains:

```text
nanur-khata/
├── bhapa-pitha.md
├── secret-masala.txt
├── recipe.tmp
├── cooking.tmp
├── shopping.tmp
└── draft/
    ├── biryani.md
    └── halua.md
```

## Task B — Create `.gitignore` and verify

Create the `.gitignore` file in Git Bash:

```bash
cat > .gitignore <<'EOF_IGNORE'
# Nanu's secret recipe
secret-masala.txt

# Ignore all temporary files
*.tmp

# Ignore draft folder
draft/
EOF_IGNORE
```

### Rules explained

| Rule | Effect |
|---|---|
| `secret-masala.txt` | Ignores the secret spice file |
| `*.tmp` | Ignores files with the `.tmp` extension |
| `draft/` | Ignores the draft directory and its contents |

Check the working tree:

```bash
git status
```

If all other files were already committed, the only new untracked file should be `.gitignore`. The secret, temporary, and draft files should not appear in the normal status output.

To see ignored files explicitly:

```bash
git status --ignored
```

To identify the matching ignore rules:

```bash
git check-ignore -v secret-masala.txt recipe.tmp draft/biryani.md
```

Stage and commit the ignore rules:

```bash
git add .gitignore
git commit -m "Add gitignore to protect secret recipes and temporary files"
git status
git log --oneline
```
__________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# The Eid Special — Git Practice Chat Log

Run these commands in Git Bash inside your `nanur-khata` repository.

 Check your branch with `git branch`. This walkthrough uses `main`; replace `main` with `master` throughout if that is your branch name. Start with a clean working directory (`git status`).

### Task A: Create and switch branches

```bash
git branch eid-special
git checkout eid-special
git branch
```

`git branch` creates the branch; `git checkout` switches to it. The asterisk in `git branch` output marks the current branch.

### Task B: Add recipes in two separate commits

Create and commit `biryani.md`:

```bash
echo "# Eid Biryani" > biryani.md
echo "Rice: 2 cups" >> biryani.md
echo "Meat: 1 kg" >> biryani.md

git add biryani.md
git commit -m "Add Eid biryani recipe"
```

Create and commit `shemai.md`:

```bash
echo "# Eid Shemai" > shemai.md
echo "Milk: 1 litre" >> shemai.md
echo "Sugar: half a cup" >> shemai.md

git add shemai.md
git commit -m "Add Eid shemai recipe"
git log --oneline
```

### Task C: Return to main and merge

```bash
git checkout master
ls
```

The files are absent, assuming they did not already exist on `main`. They were committed on `eid-special`, so switching to `main` removes them from your working directory.

```bash
git merge eid-special
ls
git log --oneline --graph --all --decorate
```

Both files should now appear on `main`. If `main` had no new commits, Git will usually perform a fast-forward: it moves `main` to the same commit as `eid-special`, without creating a separate merge commit.

### Challenge: Create and resolve a merge conflict

Continue after Task C. Change the same line in `biryani.md` differently on both branches.

#### 1. Change the recipe on main

```bash
git checkout master

echo "# Eid Biryani" > biryani.md
echo "Rice: 3 cups" >> biryani.md
echo "Meat: 1 kg" >> biryani.md

git add biryani.md
git commit -m "Increase biryani rice to 3 cups"
```

#### 2. Change the same line on eid-special

```bash
git checkout eid-special

echo "# Eid Biryani" > biryani.md
echo "Rice: 4 cups" >> biryani.md
echo "Meat: 1 kg" >> biryani.md

git add biryani.md
git commit -m "Increase Eid biryani rice to 4 cups"
```

#### 3. Merge into main

```bash
git checkout master
git merge eid-special
```

Git reports a conflict because both branches changed `Rice: 2 cups` to different values.

```bash
git status
cat biryani.md
```

The file will contain markers like:

```text
# Eid Biryani
<<<<<<< HEAD
Rice: 3 cups
=======
Rice: 4 cups
>>>>>>> eid-special
Meat: 1 kg
```

| Marker | Meaning |
| --- | --- |
| `<<<<<<< HEAD` | Your current branch's version (`master`) |
| `=======` | Separates the conflicting versions |
| `>>>>>>> eid-special` | The incoming branch's version |

#### 4. Resolve the conflict

Choose the final recipe. For this example, keep 4 cups. Rewrite the file without conflict markers:

```bash
echo "# Eid Biryani" > biryani.md
echo "Rice: 4 cups" >> biryani.md
echo "Meat: 1 kg" >> biryani.md
```

Conflict resolved and complete the merge:

```bash
git add biryani.md
git commit -m "Resolve biryani rice conflict using 4 cups"
```

Verify:

```bash
git status
cat biryani.md
git log --oneline --graph --all --decorate
```
__________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Mitu Joins In — Git Collaboration Chat Log


# Connect your local repository to GitHub
### Task A:
git remote add origin https://github.com/sumaya-shemo/Module-2-Nanur-Khata-Solution.git

# Verify the remote URL
git remote -v

# Upload your commits and set the upstream branch
git push -u origin master

git push -u origin eid-special

### Task B: Play Mitu

```bash
cd ~
git clone https://github.com/sumaya-shemo/Module-2-Nanur-Khata-Solution.git

cd Module-2-Nanur-Khata-Solution

cat > chittagong-mezban.md <<'RECIPE'
# Chittagong Mezban

A traditional beef dish from Chattogram.

## Ingredients
- Beef
- Onion
- Garlic and ginger
- Mezban spice mix
- Salt
- Oil

## Instructions
1. Mix the beef with the spices.
2. Cook slowly until tender.
3. Serve with rice.
RECIPE

git add chittagong-mezban.md
git commit -m "Add Mitu's Chittagong mezban recipe"
git push origin master

```

Cloning automatically configures `origin` and the tracked branch.

### Task C: Fetch, inspect, and merge

```bash
cd ~/Module-2-Nanur-Khata-Solution

git fetch origin
```

Fetch downloads Mitu's commits and updates `origin/master`, but main local `master` and working files stay unchanged.

Inspect the downloaded changes:

```bash
git log --oneline HEAD..origin/master

git diff HEAD origin/master

```

Integrate them:

```bash
git merge origin/master

cat chittagong-mezban.md
```



### Task D: Mitu pushes again; my pull

In Mitu's folder:

```bash
cd ~/Module-2-Nanur-Khata-Solution


echo "" >> chittagong-mezban.md
echo "Tip: Slow cooking gives the meat a tender texture." >> chittagong-mezban.md

git add chittagong-mezban.md
git commit -m "Add slow cooking tip to mezban recipe"
git push origin master
```

Back in my folder:

```bash
cd ~/nanur-khata
git pull --no-rebase origin master

cat chittagong-mezban.md
```

`--no-rebase` explicitly selects merging when Git integrates the downloaded commits.

## User: Follow-up question

**Question:** What is the difference between fetch + merge and pull?

## Explanation:

**Fetch + merge lets you download and integrate changes in two separate steps. Pull performs both steps in one command.**

| Command | Downloads remote commits? | Integrates into your current branch? |
|---|---|---|
| `git fetch origin` | Yes | No |
| `git merge origin/main` | No | Yes |
| `git pull --no-rebase origin main` | Yes | Yes, using merge |

### Two steps: inspect before merging

```bash
git fetch origin
git diff HEAD origin/master
git merge origin/master
```

### One command: fetch and merge together

```bash
git pull --no-rebase origin master
```
__________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Half-Cooked Halua and Time Travel

## Story

Halfway through the halua recipe, Nanu calls:

> "You forgot cardamom in the biryani!"

You need to pause the unfinished halua, fix the biryani, organize the recipe book, inspect the history, and experiment with time travel safely.

## Task A — Stash Halua, Fix Biryani, Restore Halua

Create an unfinished halua recipe:

```bash
echo "# Halua Recipe" > halua.md
echo "Sugar: 1 cup" >> halua.md
echo "Ghee: 2 tablespoons" >> halua.md
git status
```

Stash the work, including the new untracked file:

```bash
git stash push -u -m "Unfinished halua recipe"
git stash list
```

Fix the already-tracked `biryani.md` and commit:

```bash
echo "Cardamom: 4 pieces" >> biryani.md
git add biryani.md
git commit -m "Fix biryani recipe by adding cardamom"
```

Restore unfinished work:

```bash
git stash pop
git status
cat halua.md
```

**Explanation:** `git stash` temporarily stores unfinished changes. `-u` includes untracked files. `git stash pop` reapplies the stash and removes it if the application succeeds. Resolve any conflicts if Git reports them.

## Task B — Organize Recipes and Inspect Changes

Create folders and move recipes:

```bash
mkdir -p recipes/main-dishes recipes/desserts
git mv biryani.md recipes/main-dishes/
git add halua.md
git mv halua.md recipes/desserts/
```

If `old-recipe.md` exists and is tracked, delete it:

```bash
git rm old-recipe.md
```

Commit the organization changes:

```bash
git commit -m "Organize recipes and remove outdated recipe"
git log --stat -M
```

**Explanation:** `git mv` moves or renames tracked files and stages the change. `git rm` deletes tracked files and stages the deletion. `git log --stat -M` shows changed-file statistics and enables rename detection. Skip the `git rm` command if the example file does not exist.

## Task C — Inspect a Commit with `git show`

```bash
git log --oneline
```

Example output (SHAs will differ):

```text
* 8d881ff Fix biryani recipe by adding cardamom
* 7b02fbb Add slow cooking tip to mezban recipe
* 818fc8d Add Mitu's Chittagong mezban recipe
```

Show the actual SHA of the biryani fix:

```bash
git show 8d881ff
```

**Explanation:** `git show <SHA>` displays the commit metadata and patch, letting you see exactly what changed.

## Task D — Time Travel on a Test Branch Only

Create a separate branch:

```bash
git switch -c time-travel-test
```

Make two test commits:

```bash
echo "Experiment 1" > experiment.txt
git add experiment.txt
git commit -m "First experiment"

echo "Experiment 2" >> experiment.txt
git add experiment.txt
git commit -m "Second experiment"

git log --oneline -3
```

Reset the branch to its previous commit:

```bash
git reset --hard HEAD~1
cat experiment.txt
git log --oneline -3
```

Expected file contents:

```text
Experiment 1
```

**What was lost?** The `Experiment 2` line disappears from the tracked file, and the second commit is no longer referenced by this branch. The commit may be recoverable temporarily using `git reflog`, but recovery is not guaranteed forever.

## Question — Why Are Rebase and `reset --hard` Dangerous on a Shared Branch?

### `git rebase`

Rebase can rewrite commits and create new SHAs. If you rebase commits already shared with Mitu, your local history may diverge from hers. Force-pushing the rewritten history can disrupt collaborators and potentially overwrite their work.

```text
A --- B --- C         Mitu's history
 \
  D --- B' --- C'     my rewritten history
```

### `git reset --hard`

Reset moves the current branch pointer and updates tracked files in the index and working tree. It can discard uncommitted changes, and a force push after resetting a shared branch can remove commits from the remote branch's visible history.

```text
Before: A --- B --- C --- D   master
After:  A --- B             master
```

__________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

https://github.com/sumaya-shemo/Module-2-Nanur-Khata-Solution/blob/73ebdb8f2c852fa8d44c037c7b54b97a02811d9d/screenshot_graph_all.png




