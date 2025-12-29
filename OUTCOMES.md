# Exercise Outcomes Submission Template

**Student/Group Name**: Francisco Mateos Díaz  
**Level Completed**: master 
**Date**: 29/12/2025

---

## 📋 Exercise Summary

### Exercise: Rewriting History (Rebase and Amend Commits)
**Status**: ✅ Completed

**What I did**:
I completed a comprehensive exercise on Git history rewriting, covering four major areas: amending commits to fix mistakes without creating new commits, using interactive rebase to clean up commit history by squashing and rewording commits, rebasing feature branches onto an updated main branch to maintain linear history, and understanding the critical risks and best practices for safely rewriting Git history. Throughout the exercise, I created multiple test scenarios, manipulated commit history using various techniques, and documented the differences between merge and rebase workflows.

**Commands Used**:
```bash
# Part 1: Amending Commits
git add config.txt
git commit -m "Add configuration file"
git commit --amend -m "Add params to config file"
git commit --amend -m "Add another forgoten param to config file"
git commit --amend -m "Add another config file"
git log --oneline -n 4

# Part 2: Interactive Rebase
git add featureA.txt
git commit -m "Added feature A"
git add featureB.txt
git commit -m "Added feature B"
git add featureC.txt
git commit -m "Added feature C"
git add featureA.txt
git commit -m "Fix typo"
git rebase -i HEAD~4
git log --oneline -n 5
```
**Screenshot of interactive rebase**
![Interactive rebase in vi editor](image.png) 

```
# Part 3: Rebasing a Branch
git checkout -b feature/awesome-feature
git add awesome.txt
git commit -m "Added awesome feature"
git checkout master
git add master-update.txt
git commit -m "Update on master branch"
git checkout feature/awesome-feature
git rebase master
git log --graph --oneline --all -n 10

# Part 4: Understanding Risks
git log --oneline  # To examine SHA changes
```

**Results/Output**:

**Part 1 - After final amend:**
```
$ git log --oneline -n 4
8790f96 (HEAD -> master) Add another config file
b5d8eb6 (origin/master) refactor: consolidate master exercises
960a0a6 docs: Add submission instructions to master level
f0055a0 Update README for master level exercises
```

**Part 2 - After interactive rebase:**
```
$ git log --oneline -n 5
33e2989 (HEAD -> master) Added feature CC
82c2529 Added feature B
c33d230 Added feature A
fdc02b1 Update on master branch
8790f96 Add another config file
```

**Part 3 - After rebasing feature branch:**
```
$ git log --graph --oneline --all -n 10
* 5afcef5 (HEAD -> feature/awesome-feature) Added awesome feature
* fdc02b1 (master) Update on master branch
* 8790f96 Add another config file
* b5d8eb6 (origin/master) refactor: consolidate master exercises
* 960a0a6 docs: Add submission instructions to master level
* f0055a0 Update README for master level exercises
| *   c0da7ff (intermediate) docs: Add intermediate level exercise outcomes
| |\  
| | * 5458b63 (feature/footer) added css style to page.html
| | * 5ea873d added css style to page.html
| * | 7e9f504 (tag: v1.0-test, tag: v1.0) Merge footer solving conflict
```

---

## 🎯 Key Learnings

**Main concepts I learned**:
1. **Commit Amending**: `--amend` rewrites the last commit as if the mistake never happened - perfect for fixing typos, adding forgotten files, or correcting commit messages before pushing
2. **Interactive Rebase Power**: `git rebase -i` opens a powerful editor interface where I can rewrite multiple commits at once - squashing small fixes into main commits, rewording unclear messages, and reordering commits to create logical flow
3. **Squash vs Fixup**: Squash combines commits and lets you edit the message, while fixup automatically discards the second commit's message - fixup is faster for obvious typos and fixes
4. **Rebase vs Merge Philosophy**: Rebase creates linear history by replaying commits on top of another branch, while merge preserves the branching structure with merge commits - rebase makes history cleaner but rewrites commits
5. **SHA Immutability**: Every time you rewrite history, Git creates entirely new commits with different SHA hashes - the old commits are orphaned and eventually garbage collected
6. **Public History Safety**: Never rewrite commits that others have already pulled - it creates conflicts, confusion, and potential data loss for your teammates

**Skills I improved**:
- Reading and interpreting Git's commit graph with `git log --graph --oneline --all`
- Using interactive rebase commands (pick, reword, edit, squash, fixup) confidently
- Understanding when to use rebase over merge based on project workflow needs
- Recognizing the difference between safe local history rewriting and dangerous public history rewriting
- Navigating the vim editor for interactive rebase operations
- Creating clean, professional commit histories that tell a clear story

---

## 🚧 Challenges Faced

### Challenge 1: Understanding when amend creates vs modifies commits
**Problem**: Initially confused about whether `--amend` creates a new commit or modifies the existing one. After the first amend, I saw the commit SHA changed from `91a9e07` to `b38618c`, which seemed to contradict the idea of "modifying" a commit.

**Solution**: Realized that Git commits are immutable - you can't actually "modify" a commit. Instead, `--amend` creates a brand new commit with a new SHA and the old commit becomes orphaned. The term "amend" is somewhat misleading; it's really "replace the last commit with a new one that looks similar."

**Commands/Approach**:
```bash
# Observed SHA changes throughout amending process:
# First commit:  91a9e07 "Add configuration file"
# After amend 1: b38618c "Add params to config file"
# After amend 2: 333990e "Add another forgoten param to config file"
# After amend 3: 8790f96 "Add another config file"
# Each amend created an entirely new commit object
```

---

### Challenge 2: Interactive rebase editor commands
**Problem**: First time using interactive rebase, the vim editor opened with unfamiliar commands like `pick`, `reword`, `edit`, `squash`, and `fixup`. Wasn't sure which command to use for combining the "Fix typo" commit back into "Added feature A".

**Solution**: Learned the distinction between squash and fixup - both combine commits, but squash keeps all messages for editing while fixup discards the second message automatically. Since "Fix typo" wasn't adding meaningful information, I used `fixup` to keep only "Added feature A" message.

**Commands/Approach**:
```bash
# In the interactive rebase editor:
pick c33d230 Added feature A
pick 82c2529 Added feature B
edit 33e2989 Added feature CC
f fdc02b1 Fix typo  # Changed from 'pick' to 'f' for fixup

# Result: "Fix typo" commit was merged into "Added feature A" 
# and its message was discarded
```

---

### Challenge 3: Visualizing rebase vs merge conceptually
**Problem**: Struggled to understand what "replaying commits on top of another branch" actually meant during rebase, and how it differed from merge in terms of the resulting commit structure.

**Solution**: Drew out the commit graph on paper to visualize the transformation:

```
BEFORE rebase:
master:          A - B - C
                      \
feature/awesome:       D

AFTER git rebase master:
master:          A - B - C
                          \
feature/awesome:           D'

The key insight: D becomes D' (new SHA) because Git creates 
a new commit starting from C instead of B.
```

This visualization helped me understand that rebase "moves" the branch starting point, while merge would have created a merge commit connecting both histories.

---

### Challenge 4: Understanding --force-with-lease safety limitations
**Problem**: Initially thought `--force-with-lease` was a completely safe alternative to `--force` that would prevent all conflicts when rewriting shared history.

**Solution**: Discovered that `--force-with-lease` only checks if there are new commits on the remote that you don't have locally - it does NOT check if teammates have already pulled your commits. This means it's only marginally safer than `--force`:

```bash
# Scenario where --force-with-lease fails to protect:
# 1. You and teammate both pull commit abc1234
# 2. You amend it to xyz9876
# 3. git push --force-with-lease checks: "New commits on remote?" 
#    Answer: NO → Allows push
# 4. Teammate's abc1234 is now orphaned → Conflicts!
```

The real safety comes from the workflow rule: never rewrite commits that have been pushed to shared branches, regardless of which force flag you use.

---

## 💭 Personal Reflection

**What surprised me**:
The most surprising discovery was that Git commits are actually immutable - even operations that appear to "modify" commits (like `--amend` and `rebase`) actually create entirely new commits and delete the old ones. This immutability is a strength because everything is recoverable through reflog but also it is a source of confusion for learners. I was also surprised by the use rebase for a cleaner commit history - looking at professional projects on GitHub, the linear commit history created by rebase makes it much easier to understand the evolution of features compared to the ""spaghetti"" of merge commits.

**What I found most difficult**:
The conceptual leap from thinking about commits as mutable objects that can be "edited" to understanding them as immutable snapshots that can only be replaced was the hardest part. This is compounded by Git's terminology - words like "amend" and "rewrite" suggest modification when really we're creating new commits. The interactive rebase interface was also challenging at first, particularly understanding when to use squash vs fixup vs reword, and how the order of commits in the editor affects the final result.

**What I found most useful**:
Interactive rebase (`git rebase -i`) is powerful for crafting professional commit histories. Before pushing feature branches, I can now clean up all my experimental commits, typo fixesninto a logical series of well-documented changes. This makes code reviews much easier for teammates and creates a clearer historical record. The `git log --graph --oneline --all` command has also become essential for visualizing branch relationships and understanding the impact of rebases and merges.

**How I would apply this in real projects**:
In professional development, I would adopt this workflow:

1. **Local feature branches**: Commit frequently and messily during development - "WIP: trying approach A", "oops typo", "this might work" - without worrying about perfect commits
2. **Before pushing**: Use `git rebase -i` to clean up history into logical, well-documented commits that each represent a complete thought
3. **After PR feedback**: Use `git commit --fixup` with the original commit SHA to mark fixes, then `git rebase -i --autosquash` to automatically squash them before pushing
4. **For shared branches**: NEVER rebase or amend - only add new commits to fix mistakes
5. **Documentation**: Always document in PR descriptions when history has been rewritten so reviewers know to `git pull --rebase` instead of regular pull

I would also establish team conventions about when to use rebase vs merge - for example, using rebase for feature branches to keep history linear, but using merge commits for major feature integrations to preserve the context of when features were combined.

**Deeper reflection on history rewriting philosophy**:
After completing this exercise, I understand that Git history is not just a log of what happened - it's a communication tool. Clean, well-organized commit history tells a story about *why* changes were made, making it easier for future developers (including future me) to understand the reasoning behind decisions. Rebase and amend are tools for crafting that story, but they must be used responsibly. The cardinal rule is simple: history rewriting is safe and beneficial on private branches, but dangerous and disruptive on shared branches. This maps to a broader software engineering principle - refactoring is valuable, but only when it doesn't break things for others who depend on your code.

The distinction between `--force` and `--force-with-lease` initially seemed important, but I now understand it's a false sense of security. The real protection comes from team communication and following the workflow rule: don't rewrite pushed commits. If you absolutely must rewrite shared history (rare cases like removing accidentally committed secrets), coordinate with your team explicitly, warn them in advance, and provide clear instructions for how they should re-sync their local branches.

---

## 📊 Self-Assessment

Rate your confidence level for each topic (1-5, where 5 is very confident):

| Topic | Confidence (1-5) | Notes |
|-------|------------------|-------|
| Basic Git commands | 5 | Very comfortable with add, commit, status, log, branch, checkout |
| Branching & merging | 5 | Understand merge strategies, fast-forward vs three-way merges, conflict resolution |
| Remote operations | 4 | Comfortable with push/pull/fetch, but still learning about force-with-lease nuances |
| Conflict resolution | 5 | Can resolve conflicts confidently using merge tools and manual editing |
| History rewriting | 4 | Understand rebase and amend well, but need more practice with complex rebase scenarios |
| Git hooks | 2 | Not covered in this exercise - this is my next learning area |
| Security practices | 4 | Understand dangers of rewriting shared history and proper workflow, still learning about signing commits |

---

## 📊 Detailed Comparisons

### Squash vs Fixup in Interactive Rebase

| Aspect | Squash (s) | Fixup (f) |
|--------|-----------|----------|
| **Purpose** | Combine commits with message editing | Combine commits and discard message |
| **Commit message** | Keeps ALL messages, lets you edit | Discards current message, keeps previous |
| **Editor opens** | Yes, you can edit the combined message | No, automatic (no editing) |
| **Best for** | Important commits with useful details | Small fixes, typos, cleanup commits |
| **Example use** | "Fix bug" + "Add feature" → Combined with both details | "Fix bug" + "oops typo fix" → Just "Fix bug" |
| **Workflow speed** | Slower (requires editing) | Faster (automatic) |

**Practical Example:**
```bash
# SQUASH - when both messages matter
pick abc1234 Implement user authentication
s def5678 Add password hashing
# Editor opens: "Implement user authentication with password hashing"

# FIXUP - when second message is just noise
pick abc1234 Fix login bug
f def5678 Fix typo in previous commit
# Result: "Fix login bug" (typo message automatically discarded)
```

---

### Merge vs Rebase - Comprehensive Comparison

| Feature | Merge | Rebase |
|---------|-------|--------|
| **What it does** | Combines two branches with a merge commit | Replays your commits on top of the target branch |
| **Commit history** | Creates a merge commit, shows both histories | Linear history, no merge commit |
| **Original commits** | Preserved with original SHAs | Rewritten with new SHAs |
| **History visibility** | Shows the merge point and branching clearly | Looks like work happened sequentially |
| **Safety** | Safe for shared/public branches | Only safe for unpushed commits |
| **Result** | Non-destructive, preserves all context | Cleaner, linear history |
| **Conflicts** | Resolved once during merge | May need to resolve same conflict multiple times |
| **Use case** | Team collaboration, shared branches, preserving context | Your own feature branch before push, clean history |
| **Traceability** | Easy to see when branches diverged/merged | Harder to see original branch points |

**Visual Comparison:**
```
MERGE - Preserves branching structure:
main:    A - B - C -------- M (merge commit)
              \           /
feature:       D - E - F

REBASE - Creates linear history:
main:    A - B - C
                  \
feature:           D' - E' - F' (commits replayed)
```

**Simple Analogy:**
- **Merge** = "Let me bring main's changes into my branch AND create a record showing we merged"
- **Rebase** = "Let me replay my work on top of the latest main, as if I started from there"

---

## 🔒 Understanding History Rewriting Risks

### How Commit SHAs Change

Every Git commit has a unique 40-character SHA-1 hash (like `abc1234...`). When you rewrite history, Git creates **completely new commits** with different SHAs:

**Example from my work:**
```bash
# Original commit
91a9e07 Add configuration file

# After first amend
b38618c Add params to config file  ← NEW SHA

# After second amend
333990e Add another forgoten param to config file  ← NEW SHA

# After third amend
8790f96 Add another config file  ← NEW SHA
```

Each amend didn't "modify" commit `91a9e07` - it created a brand new commit and made the old one orphaned. The old commits still exist temporarily in Git's object database (recoverable via `git reflog`) but will eventually be garbage collected.

---

### Why Rewriting Public/Shared History is Dangerous

**The Problem:**
When you rewrite commits that teammates have already pulled, you create a divergent timeline that causes conflicts and confusion.

**Detailed Scenario:**
```
1. Initial state - everyone has commit abc1234:
   YOU:      abc1234 "Fix login bug"
   TEAMMATE: abc1234 "Fix login bug"
   REMOTE:   abc1234 "Fix login bug"

2. You amend the commit locally:
   YOU:      xyz9876 "Fix login bug with tests"  ← NEW
   TEAMMATE: abc1234 "Fix login bug"              ← OLD
   REMOTE:   abc1234 "Fix login bug"              ← OLD

3. You force push:
   YOU:      xyz9876 "Fix login bug with tests"
   TEAMMATE: abc1234 "Fix login bug"              ← ORPHANED!
   REMOTE:   xyz9876 "Fix login bug with tests"  ← REPLACED

4. Teammate tries to pull:
   ERROR! Git sees:
   - Remote has xyz9876 (they don't have)
   - They have abc1234 (remote doesn't have)
   - Git can't fast-forward
   - Creates messy merge conflicts
   - Risk of losing teammate's work
```

---

### The --force-with-lease Limitation

**Common Misconception:** "`--force-with-lease` is safe because it checks before overwriting"

**Reality:** `--force-with-lease` only checks if there are **new commits on the remote** that you don't have, NOT if others have already pulled your commits.

**Example Where It Fails to Protect:**
```bash
# Timeline of events:
1. You push commit abc1234
   $ git push origin main

2. Teammate pulls it
   teammate$ git pull  # Now has abc1234 locally

3. You amend it
   $ git commit --amend
   # Creates xyz9876

4. You push with --force-with-lease
   $ git push --force-with-lease origin main
   # Check: "Does remote have commits I don't have?"
   # Answer: NO → Push allowed ✓

5. Teammate tries to pull
   teammate$ git pull
   # ERROR! Their abc1234 is gone from remote
   # They have orphaned commits
   # Conflicts and confusion!
```

**Conclusion:** `--force-with-lease` is only marginally safer than `--force`. It prevents you from overwriting *new* commits, but doesn't protect against overwriting commits that others have already pulled.

---

### Safe Workflow Rules

#### Rule 1: Only Rewrite Unpushed Commits

```bash
# SAFE - Rewrite before pushing
git checkout feature/my-work
git add file.txt
git commit -m "Add feature"
git commit --amend -m "Add feature with tests"  # Safe - not pushed yet
git rebase -i HEAD~3                             # Safe - not pushed yet
git push origin feature/my-work                  # First push

#  DANGEROUS - Rewrite after pushing
git push origin main                     # Others can pull now
git commit --amend                       # DANGER! Rewriting shared history
git push --force-with-lease origin main # DANGER! Even with lease!
```

#### Rule 2: Fix Mistakes on Shared Branches with New Commits

```bash
#  WRONG - Rewrite (creates problems for team)
git commit -m "Add feature with typo"
git push origin main
git commit --amend -m "Add feature (fixed typo)"
git push --force-with-lease origin main  # Teammates will have conflicts!

# RIGHT - Create a new commit
git commit -m "Add feature with typo"
git push origin main
git commit -m "Fix: correct typo in feature"
git push origin main  # Safe! History intact, everyone happy
```

#### Rule 3: Communicate Before Rewriting Shared History

If you MUST rewrite shared history (e.g., removing accidentally committed secrets):

```bash
# 1. Announce to team in Slack/email:
" I need to rewrite history on branch feature/X to remove secrets.
 
Please save your work NOW:
1. git stash  # Save your uncommitted changes
2. git fetch origin  # After I notify you I've pushed
3. git reset --hard origin/feature/X  # Reset to new history
4. git stash pop  # Restore your changes
 
Don't pull until I confirm I've force-pushed!"

# 2. Wait for team confirmation they've saved work

# 3. Rewrite and force push
git rebase -i HEAD~5  # Remove sensitive commit
git push --force-with-lease origin feature/X

# 4. Notify team
" Force push complete. Please follow steps 2-4 now."
```

---

### Decision Table: When to Rewrite History

| Situation | Action | Safety Level |
|-----------|--------|--------------|
| Local commit, not pushed yet | `git commit --amend` |  PERFECTLY SAFE |
| Multiple local commits, not pushed | `git rebase -i` | PERFECTLY SAFE |
| Mistake on already-pushed shared branch | New commit with fix |  PERFECTLY SAFE |
| Your feature branch, only you work on it | `git push --force-with-lease` after rebase |  USUALLY SAFE |
| Shared feature branch, team coordinates | `git push --force-with-lease` with team sync |  RISKY but manageable |
| Main/master branch | NEVER REWRITE |  EXTREMELY DANGEROUS |

---

### Best Practices Summary

**Golden Rules:**
1. **Never rewrite history on `main`/`master` or shared branches**
2. **Only rewrite commits that haven't been pushed**
3. **If you must force-push, use `--force-with-lease` instead of `--force`**
4. **Communicate with your team before any force push**
5. **When in doubt, create a new commit instead of rewriting**

**Safe Alternative to Rewriting Shared History:**
```bash
# Instead of:
git rebase -i HEAD~3  # Dangerous if already pushed
git push --force-with-lease

# Do this:
git commit -m "Fix: address issues from commits abc, def, ghi"
git push  # Simple, safe, no conflicts
```

**Recovery if You Mess Up:**
```bash
# If you accidentally rewrote shared history:
git reflog  # Find the SHA before your mistake
git reset --hard abc1234  # Go back to that commit
git push --force-with-lease  # Restore the old history
# Then coordinate with team to ensure everyone's in sync
```

---

## 🔗 Evidence/Artifacts

**Key commits demonstrating my work:**
- `8790f96`: Demonstrates multiple amend operations on single commit
- `33e2989`: Result of interactive rebase with fixup operation
- `5afcef5`: Feature branch rebased onto updated master

**Files created:**
- `config.txt`: Configuration file used to practice amending commits
- `params.json`: Additional configuration file added via amend
- `featureA.txt`, `featureB.txt`, `featureC.txt`: Test files for interactive rebase
- `awesome.txt`: Feature file for branch rebasing exercise
- `master-update.txt`: Master branch update to simulate divergence

**Branches created:**
- `feature/awesome-feature`: Demonstrated rebasing feature branch onto updated master

---

## ✅ Completion Checklist

Before submitting, ensure you have:
- [x] Completed the exercise for master level (including all 4 parts)
- [x] Documented all commands used with their outputs
- [x] Described challenges and how I resolved them
- [x] Provided a thoughtful reflection on learning (>200 words)
- [x] Self-assessed confidence in each topic
- [x] Created comparison tables for squash vs fixup and merge vs rebase
- [x] Documented the risks of rewriting history comprehensively
- [x] Explained SHA changes during history rewriting
- [x] Detailed the --force-with-lease limitations
- [x] Provided best practices and decision-making guidelines

---

## 📝 Additional Comments

This master level exercise fundamentally changed how I think about Git. Before this, I viewed commits as mutable records that could be "edited" - now I understand them as immutable snapshots that can only be replaced with new versions. This immutability is what makes Git both powerful (everything is recoverable) and dangerous (rewriting shared history breaks things for others).

The most valuable takeaway is understanding that Git history is a communication tool, not just a log. Clean commit history created through judicious use of rebase and amend makes projects more maintainable and easier for new developers to understand. However, this power must be balanced with responsibility - the social contract of not rewriting shared history is what makes Git-based collaboration work.

I particularly appreciate the nuanced understanding of `--force-with-lease` vs `--force` - the distinction is less important than following the underlying principle of never rewriting commits that others have pulled. The real safety comes from workflow discipline, not from technical flags.


---

**Submission Date**: 29/12/2025  
**Ready for Review**: ✅ Yes