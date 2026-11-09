# Git Internals, Branching Strategies & Advanced GitHub Workflows

## 1. Introduction & Prerequisites
Git is a distributed content-addressable storage system with a version control system (VCS) user interface. Understanding Git's underlying object model (blobs, trees, commits, annotated tags) transforms Git from a source of anxiety and accidental data loss into an indispensable engineering tool.

- **Prerequisites:** Terminal basics, file navigation.
- **Estimated Study Time:** 10 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Understand Git's internal data model: SHA-1/SHA-256 hashes, Blobs, Trees, Commits, and HEAD references.
- Master the three Git trees: Working Directory, Staging Area (Index), and Commit History (Repository).
- Master interactive rebasing (`git rebase -i`), squashing, cherry-picking, and reflog data recovery.
- Resolve complex merge conflicts safely with 3-way diff analysis.
- Understand enterprise Git workflows: Trunk-Based Development, GitFlow, Pull Request reviews, and CI hooks.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Git Three-Stage Architecture (Working Directory, Staging Index, Repository)
- [ ] Git Object Store Internals (`.git/objects/`, Blobs, Trees, Commits)
- [ ] Branching & Detached HEAD States
- [ ] Fast-Forward vs Three-Way Merges
- [ ] Rebasing vs Merging (`git rebase`, `git merge --no-ff`)
- [ ] Interactive Rebase (`git rebase -i HEAD~5`, squash, fixup, reword)
- [ ] Recovering "Lost" Commits with `git reflog`
- [ ] Selective Patching (`git cherry-pick`, `git add -p`)
- [ ] Submodules vs Subtrees
- [ ] GitHub Collaboration: PR templates, branch protection rules, code owners (`CODEOWNERS`)

---

## 4. Free Documentation Resources
- **Pro Git Book (Official Free Book):** [https://git-scm.com/book/en/v2](https://git-scm.com/book/en/v2)
- **Learn Git Branching (Interactive Visualizer):** [https://learngitbranching.js.org/](https://learngitbranching.js.org/)
- **GitHub Skills (Interactive Labs):** [https://skills.github.com/](https://skills.github.com/)

---

## 5. Free Video Courses & Tutorials
- **freeCodeCamp:** *Git & GitHub Crash Course for Beginners*
- **The Modern Coder:** *Git Internals & How Git Works Under the Hood*
- **Traversy Media:** *Git & GitHub Tutorial for Beginners*

---

## 6. Important Concepts Explained

### The Git Object Model
Every object in Git is identified by the hash of its content plus a small header:
- **Blob:** Stores raw file data without metadata or filenames.
- **Tree:** Represents a directory; maps filenames and permissions to Blob or sub-Tree hashes.
- **Commit:** Stores a pointer to the root Tree, parent commit hash(es), author, committer, timestamp, and message.

```
Commit (hash: 7a8f1b)
  ├── Parent: 3b4c2e
  ├── Author: Adarsh Kumar
  └── Tree: 9c01da
        ├── README.md -> Blob (a1b2c3)
        └── src/      -> Tree (d4e5f6)
```

---

## 7. Practical Code Examples

### Essential Git Diagnostic & Remediation Commands
```bash
# 1. Inspect the last 10 commits with graph visualization
git log --graph --oneline --decorate --all -n 10

# 2. Stage only specific hunks of changes interactively
git add -p src/App.js

# 3. Clean up the last 3 local commits before pushing
git rebase -i HEAD~3

# 4. Emergency data recovery: find commits detached or deleted
git reflog
# Check out the commit SHA found in reflog
git checkout 3f8a91c
# Or restore branch pointer
git branch recovery-branch 3f8a91c

# 5. Stash with descriptive message including untracked files
git stash push -u -m "wip: authentication flow draft"
git stash list
git stash pop
```

---

## 8. Practice Assignments
1. **Assignment 1:** Complete levels 1 through 15 on [learngitbranching.js.org](https://learngitbranching.js.org/) to master rebasing, cherry-picking, and branch manipulation.
2. **Assignment 2:** Intentionally cause a merge conflict between two branches, resolve it using diff markers, verify the commit history with `git log --graph`, and push cleanly.

---

## 9. Mini-Projects
- **Automated Git Hook Setup:** Configure a pre-commit hook using shell scripts to run linting, verify no `.env` files or secret tokens are staged, and format code before allowing commits.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the difference between `git merge` and `git rebase`?
**Answer:** `git merge` creates a new "merge commit" that ties together two parent histories, preserving the exact chronological sequence and branch structure. `git rebase` rewrites the commit history by replaying commits from the current branch onto the tip of the target branch. This results in a clean, linear commit history, but changes commit hashes and should never be used on shared public branches.

### Q2: How does `git reflog` differ from `git log`?
**Answer:** `git log` traverses the commit ancestry starting from the current commit (`HEAD`). If a commit is orphaned (e.g. after a hard reset), it disappears from `git log`. `git reflog` records every update made to local branch pointers and `HEAD` (checkouts, resets, rebases, commits). It acts as an undo log, allowing recovery of virtually any commit created within the local retention window (default 30-90 days).

---

## 11. Revision Notes
- Never rebase commits that have already been pushed to a public/shared branch.
- Use `git commit --amend --no-edit` to add forgotten staged files to the latest commit.
- Use `git restore --staged <file>` to unstage without discarding working directory changes.
- Always configure `.gitignore` before starting a project to avoid committing build artifacts or credentials.

---

## 12. Topic Completion Checklist
- [ ] Understand Blobs, Trees, and Commits
- [ ] Performed interactive rebase (`squash`, `reword`)
- [ ] Recovered a lost commit using `git reflog`
- [ ] Resolved merge conflicts using 3-way merge markers
