# 🧠 Git Basics + Multi-Account GitHub Cheatsheet

A complete guide to safely working with Git, GitHub, multiple accounts (work + personal), SSH keys, GitHub CLI, identity switching, branch basics, and safe workflows.

---

# 🚀 1. How Git Works (High-Level Model)

Git tracks your work in **three stages**:

```
Working Directory → Staging Area → Repository (Commits)
```

* **Working Directory** – your actual files
* **Staging Area (git add)** – files selected for commit
* **Repository (.git/)** – permanent history

### Essential Commands

```
git status
git add .
git commit -m "message"
git log --oneline
```

---

# 📁 2. Creating a New Local Project & Pushing to GitHub

## Option A — Traditional Manual Method

### 1️⃣ Create folder & file

```
mkdir my-project
cd my-project
echo "# My Project" > README.md
```

### 2️⃣ Initialize Git

```
git init
git status   # shows: On branch main
```

### 3️⃣ First commit

```
git add .
git commit -m "initial commit"
```

### 4️⃣ Create repo on GitHub (web or CLI)

### 5️⃣ Add remote

**Personal:**

```
git remote add origin git@github.com-personal:<username>/<repo>.git
```

**Work:**

```
git remote add origin git@github.com-work:<org>/<repo>.git
```

### 6️⃣ Push

```
git push -u origin main
```

If GitHub says remote contains prior commits:

```
git push -u origin main --force
```

---

# ⚡ 3. GitHub CLI Workflow (Recommended)

This avoids creating empty repos manually.

### 🔥 Create repo *AND* push in one command

```
gh repo create <owner>/<repo> --source=. --private --push
```

### Example – Personal repo

```
gh repo create truedevopsmk-lab/myrepo --source=. --private --push
```

### Example – Work repo

```
gh repo create FICO-1ES/myrepo --source=. --private --push
```

### 🔍 Check authentication

```
gh auth status
```

### Switch login

```
gh auth login --hostname github.com
```

Choose SSH + Browser login.

### If you created folder but repo is empty:

```
gh repo create truedevopsmk-lab/myrepo --private
```

Then:

```
git remote add origin git@github.com-personal:truedevopsmk-lab/myrepo.git
git push -u origin main
```

---

# 🌲 4. Branch Basics

### Check current branch

```
git branch --show-current
```

### List branches

```
git branch -a
```

### Create & switch

```
git checkout -b feature-x
```

### Switch

```
git checkout main
```

### Delete

```
git branch -d feature-x
```

---

# 🔐 5. SSH Multi-Account Setup (Personal + Work)

Update `~/.ssh/config`:

```
# Personal GitHub
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes

# Work GitHub
Host github.com-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes
```

### Test identities

```
ssh -T git@github.com-personal
ssh -T git@github.com-work
```

---

# 👤 6. Git Identity Switching (Repo-Specific)

### Work identity

```
git config user.name "MuthuKumar_fico"
git config user.email "muthukumar@fico.com"
```

### Personal identity

```
git config user.name "truedevopsmk-lab"
git config user.email "truedevopsmk@gmail.com"
```

### Global identity

```
git config --global user.name
git config --global user.email
```

---

# 🔄 7. gitswitch — Switch Between Work & Personal Accounts

Use:

```
gitswitch personal
```

or

```
gitswitch work
```

This updates:

* Git identity
* SSH key
* Validates GitHub identity
* Warns on mismatched remotes

---

# 🧩 8. Ultimate Safety Tool — gitwhoami

Run anytime before commit/push:

```
gitwhoami
```

Shows:

* Repo identity
* Global identity
* Current branch
* Remote URL
* SSH GitHub login
* Mismatch warnings

---

# 🛑 9. Common Mistakes & Fixes

### ❌ `src refspec main does not match any`

**Cause:** No commits yet.

```
git add .
git commit -m "initial commit"
```

### ❌ `remote contains work you do not have`

**Cause:** GitHub default commit exists.

```
git push -u origin main --force
```

### ❌ Wrong folder initialized

```
rm -rf .git
git init
```

---

# 🟣 10. Jarvis Golden Workflow (Guaranteed Safe)

Before committing:

```
gitwhoami
```

Before pushing:

```
gitwhoami
```

Switch identity safely:

```
gitswitch personal
# or
gitswitch work
```

Commit:

```
git add .
git commit -m "message"
```

Push:

```
git push
```

---

# 🏁 You Now Have the Ultimate Git + GitHub Cheat Sheet

Comprehensive, multi-account safe, GitHub-CLI enabled, and ready for daily use!

