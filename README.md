This contains:

✔ Git fundamentals
✔ How Git works (high-level)
✔ How to create folders/files + push
✔ Branch basics
✔ Common mistakes
✔ Multi-account GitHub safety
✔ Your advanced gitwhoami tool
✔ SSH multi-account setup
✔ gitswitch workflow

---

# 🧠 **Git Basics + Multi-Account GitHub Cheatsheet**

A complete reference to help you:

* Create folders & files locally
* Initialize Git the proper way
* Understand working/staging/commits
* Push to GitHub confidently
* Switch between **personal** and **work** GitHub accounts
* Avoid accidental pushes
* Use SSH identities safely
* Check everything before committing or pushing

---

# 🚀 **1. How Git Works (High-Level Model)**

Git tracks your work in **three areas**:

```
Working Directory → Staging Area → Repository (commits)
```

* **Working Directory** — your actual files
* **Staging Area (git add)** — files selected for commit
* **Repository (.git/ folder)** — permanent snapshotted history

### Key commands:

| Action          | Command                   |
| --------------- | ------------------------- |
| See changes     | `git status`              |
| Stage files     | `git add .`               |
| Commit snapshot | `git commit -m "message"` |
| View history    | `git log --oneline`       |

---

# 📁 **2. Creating a New Local Project & Pushing to GitHub**

### **Step 1: Create folder + file**

```bash
mkdir my-project
cd my-project
echo "# My Project" > README.md
```

---

### **Step 2: Initialize Git**

```bash
git init
```

Modern Git automatically puts you on **main**.

Check:

```bash
git status
```

---

### **Step 3: First commit**

```bash
git add .
git commit -m "initial commit"
```

---

### **Step 4: Create GitHub repo**

Go to:

```
https://github.com/new
```

* Choose your account (personal / work)
* Name the repo
* **Do NOT initialize with README** (already exists locally)

---

### **Step 5: Add remote**

**Personal account repo:**

```bash
git remote add origin git@github.com-personal:<username>/<repo>.git
```

**Work repo:**

```bash
git remote add origin git@github.com-work:<org>/<repo>.git
```

---

### **Step 6: Push**

```bash
git push -u origin main
```

If GitHub shows “remote contains work you don’t have,” simply:

```bash
git push -u origin main --force
```

(Because the remote is empty/new.)

---

# 🌲 **3. Branch Basics**

### Check your current branch

```bash
git branch --show-current
```

### List all branches

```bash
git branch -a
```

### Create + switch to a branch

```bash
git checkout -b feature-x
```

### Switch branches

```bash
git checkout main
```

### Delete a branch

```bash
git branch -d feature-x
```

---

# 🧭 **4. Multi-Account GitHub: Always Verify Before Push**

You should ALWAYS check:

1. 🔐 Git identity
2. 🌍 Remote origin
3. 👤 SSH identity
4. 🌱 Current branch

The safest way?

```
gitwhoami
```

(Defined below.)

---

# 🔥 **5. Git Identity Commands (Repo vs Global)**

### Repo-specific identity (recommended):

**Work repo:**

```bash
git config user.name "MuthuKumar_fico"
git config user.email "muthukumar@fico.com"
```

**Personal repo:**

```bash
git config user.name "truedevopsmk-lab"
git config user.email "truedevopsmk@gmail.com"
```

### Global identity:

```bash
git config --global user.name
git config --global user.email
```

---

# 🔐 **6. SSH Multi-Account Setup (Highly Recommended)**

Add to `~/.ssh/config`:

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

### Test:

```bash
ssh -T git@github.com-personal
ssh -T git@github.com-work
```

---

# 🔄 **7. Switching Between Accounts (gitswitch)**

Use:

```
gitswitch personal
```

or

```
gitswitch work
```

This will:

* change repo identity
* swap SSH key
* verify active GitHub identity
* warn if repo and identity mismatch

---

# 🧩 **8. Ultimate Safety Tool: `gitwhoami`**

Add this alias to `~/.bashrc` or `~/.shell_functions/git_functions.sh`:

```bash
alias gitwhoami='
echo "🔍 GIT IDENTITY (Repo-Level):";
git config user.name 2>/dev/null || echo "none";
git config user.email 2>/dev/null || echo "none";
echo "";
echo "🌐 GLOBAL IDENTITY:";
git config --global user.name 2>/dev/null || echo "none";
git config --global user.email 2>/dev/null || echo "none";
echo "";
echo "🌱 CURRENT BRANCH:";
git branch --show-current 2>/dev/null || echo "no branch";
echo "";
echo "🚀 REMOTE ORIGIN:";
git remote -v 2>/dev/null || echo "no remote";
echo "";
echo "👤 SSH ACCOUNT (GitHub):";
ssh -T git@github.com 2>&1 | head -n 1;
echo "";
echo "⚠️ ACCOUNT CHECK:";
EMAIL=$(git config user.email);
REMOTE=$(git remote get-url origin 2>/dev/null);
if [[ -n "$EMAIL" && -n "$REMOTE" ]]; then
  if [[ "$EMAIL" == *"fico.com"* && "$REMOTE" == *"github.com-personal"* ]]; then
      echo "❌ WARNING: Work email + Personal repo";
  elif [[ "$EMAIL" != *"fico.com"* && "$REMOTE" == *"github.com-work"* ]]; then
      echo "❌ WARNING: Personal email + Work repo";
  else
      echo "✅ Identity matches remote";
  fi
else
  echo "Cannot determine identity mismatch.";
fi
'
```

### Use anytime:

```bash
gitwhoami
```

---

# 🛑 **9. Common Mistakes & Quick Fixes**

### 🔥 “src refspec main does not match any”

Cause: No commits yet
Fix:

```bash
git add .
git commit -m "initial commit"
```

---

### 🔥 “remote contains work you do not have”

Cause: GitHub repo has a default commit
Fix:

```bash
git push --force origin main
```

---

### 🔥 Accidentally initialized Git in the wrong folder

Solution:

Delete `.git`:

```bash
rm -rf .git
git init
```

---

# 🎉 **10. Jarvis Golden Workflow (Guaranteed Safe Push)**

Before committing:

```
gitwhoami
```

Before pushing:

```
gitwhoami
```

Before switching repos:

```
gitswitch personal   # or work
```

Then commit + push safely:

```
git add .
git commit -m "message"
git push
```

---

# 🏁 You now have the **complete Git + GitHub + Multi-Account Cheatsheet**, handcrafted by Jarvis.

If you'd like, I can also generate:

* a **PDF version**
* a **one-page minimal version**
* an **ASCII art visual diagram version**
* an **interactive learning version**

Just tell me:

👉 **“Jarvis, give me the PDF version.”**
