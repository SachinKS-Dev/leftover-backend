# 🔒 Remove serviceAccountKey.json from Git History

## 🚨 **The Problem:**

You accidentally committed `serviceAccountKey.json` to git. GitHub blocked the push to protect your credentials.

---

## ✅ **Quick Fix (Use BFG Repo-Cleaner - Easiest!)**

### **Step 1: Download BFG**

Download from: https://rpo.github.io/bfg-repo-cleaner/

Or using wget:

```powershell
# Download BFG jar file
Invoke-WebRequest -Uri "https://repo1.maven.org/maven2/com/madgag/bfg/1.14.0/bfg-1.14.0.jar" -OutFile "bfg.jar"
```

### **Step 2: Run BFG to Remove File**

```powershell
cd leftoverlink-backend

# Remove serviceAccountKey.json from all history
java -jar bfg.jar --delete-files serviceAccountKey.json

# Clean up
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

### **Step 3: Force Push**

```powershell
git push origin feature_fcm --force
```

---

## ✅ **Alternative: Manual Git Filter (More Complex)**

If you can't use BFG, use this method:

### **Step 1: Remove from History**

```powershell
# Set warning variable
$env:FILTER_BRANCH_SQUELCH_WARNING = "1"

# Remove file from all commits
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch serviceAccountKey.json" --prune-empty --tag-name-filter cat -- --all
```

### **Step 2: Clean Up**

```powershell
# Remove backup refs
Remove-Item -Recurse -Force .git/refs/original/

# Clean up
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

### **Step 3: Force Push**

```powershell
git push origin feature_fcm --force
```

---

## ✅ **Simplest Solution (If Feature Branch is New)**

Since this is a feature branch, you can just recreate it:

### **Step 1: Create New Clean Branch**

```powershell
# Go back to master
git checkout master

# Create new clean branch
git checkout -b feature_fcm_clean

# Cherry-pick commits WITHOUT the bad one
git cherry-pick 99c0e54  # fix: fcm (before the bad commit)
```

### **Step 2: Manually Add Your Changes**

1. Make sure serviceAccountKey.json is in .gitignore
2. Add your code changes
3. Commit (WITHOUT serviceAccountKey.json)

### **Step 3: Push Clean Branch**

```powershell
git push origin feature_fcm_clean
```

### **Step 4: Delete Old Branch**

```powershell
git branch -D feature_fcm
```

---

## 🎯 **Recommended: Use GitHub's Bypass Option (For College Project)**

For a college project where the repository is likely private:

### **Option 1: Click GitHub's Bypass Link**

GitHub gave you this link:

```
https://github.com/SachinKS-Dev/leftover-backend/security/secret-scanning/unblock-secret/345wuqYgX6a8WTyNE3yTD2lAyCK
```

1. Click that link
2. Review the secret
3. Click **"Allow secret"** (only if repository is PRIVATE!)
4. Push again

**⚠️ ONLY do this if:**

- Repository is PRIVATE
- This is for college project only
- You won't share the repo publicly
- You'll rotate the key after project submission

### **Option 2: Generate New Service Account Key**

After allowing/pushing:

1. Go to Firebase Console
2. Generate NEW private key
3. Delete the old one
4. Replace local serviceAccountKey.json
5. Never commit the new one!

---

## 🔒 **Best Practice (After Fixing):**

### **1. Verify .gitignore:**

Make sure `.gitignore` contains:

```
serviceAccountKey.json
*serviceAccountKey*.json
```

### **2. Always Check Before Commit:**

```powershell
# Before committing, check what's staged:
git status

# If you see serviceAccountKey.json:
git reset HEAD serviceAccountKey.json
```

### **3. Use Git Hooks (Optional):**

Create `.git/hooks/pre-commit`:

```bash
#!/bin/sh
if git diff --cached --name-only | grep -q "serviceAccountKey.json"; then
    echo "❌ Error: Attempting to commit serviceAccountKey.json!"
    echo "This file should NEVER be committed."
    exit 1
fi
```

---

## 🎯 **Quick Decision:**

**For College Project (Private Repo):**

```
1. Click GitHub's bypass link (easiest!)
2. Push successfully
3. Generate new service account key after project
```

**For Public Repo or Production:**

```
1. Use BFG Repo-Cleaner
2. Force push cleaned history
3. Rotate service account key immediately
```

---

## ✅ **Immediate Action:**

Since this is a college project, the fastest solution:

1. **Click the bypass link GitHub provided**
2. **Push again:**
   ```powershell
   git push origin feature_fcm
   ```
3. **After your project is graded:**
   - Generate new service account key
   - Delete the exposed one
   - Update your local file

---

**For now, use the bypass link if your repo is private!** This is acceptable for college projects. 🎓
