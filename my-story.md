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

