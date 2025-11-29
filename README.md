Absolutely, Jarvis here — here is a **clean, polished, copy-paste-ready `README.md` cheatsheet** that you can drop directly into your repo.

---

# 🧭 Git & GitHub Account Verification Cheatsheet

A quick reference to **ensure you're using the correct GitHub account**, **correct identity**, **correct SSH key**, and **correct remote** — so you never accidentally push to the wrong repo.

---

# 🔥 1. Check Current Branch

```bash
git branch --show-current
```

---

# 🔐 2. Check Git Identity (User/Email)

### Repo-level identity:

```bash
git config user.name
git config user.email
```

### Global identity:

```bash
git config --global user.name
git config --global user.email
```

---

# 🌍 3. Check Remote Repository (Work / Personal)

```bash
git remote -v
```

Examples:

**Personal Repo**

```
git@github.com-personal:username/repo.git
```

**Work Repo**

```
git@github.com-work:company/repo.git
```

---

# 👤 4. Check Which GitHub Account You Are Authenticated As

```bash
ssh -T git@github.com
```

Expected output:

* **Personal**

  ```
  Hi personal-username! You've successfully authenticated…
  ```

* **Work**

  ```
  Hi work-username! You've successfully authenticated…
  ```

---

# 🧞 5. Recommended One-Liner Safety Check (Before Every Push)

```bash
git config user.email && git remote -v && ssh -T git@github.com
```

This shows:

* Git identity
* Repo remote
* SSH authenticated GitHub account

---

# 🚀 6. Git Identity Switching (Work ↔ Personal)

### Set identity for *this repo only* (recommended):

**Work**

```bash
git config user.email "muthukumar@fico.com"
git config user.name "MuthuKumar_fico"
```

**Personal**

```bash
git config user.email "yourpersonal@gmail.com"
git config user.name "Your Personal Name"
```

### Set identity globally:

```bash
git config --global user.email "email@example.com"
git config --global user.name "Your Name"
```

---

# 🔁 7. SSH Key Switching (If Using Multiple GitHub Accounts)

### Add keys to your SSH agent:

```bash
ssh-add ~/.ssh/id_ed25519_work
ssh-add ~/.ssh/id_ed25519_personal
```

### Remove all keys:

```bash
ssh-add -D
```

### Force-add correct key:

```bash
ssh-add ~/.ssh/id_ed25519_work
```

or

```bash
ssh-add ~/.ssh/id_ed25519_personal
```

---

# 🏷️ 8. SSH Config (Recommended Setup for Multi-Account Use)

Add to `~/.ssh/config`:

```
# Personal GitHub
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal

# Work GitHub
Host github.com-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
```

Then make your remotes look like:

### Personal:

```
git@github.com-personal:<username>/<repo>.git
```

### Work:

```
git@github.com-work:<org>/<repo>.git
```

---

# 🧩 9. Custom Command: `gitwhoami` (Highly Recommended)

Add this to your `~/.zshrc` or `~/.bashrc`:

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

Reload:

```bash
source ~/.zshrc
```

Run anytime:

```bash
gitwhoami
```

---

# 🏁 Result

You now have:

* A **safe workflow**
* A **clean verification method**
* A **single command** that protects you
* An easy cheatsheet inside your repo



