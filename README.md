# Backdated Git Commits Script

A bash script that creates backdated Git commits on specific dates by overriding Git's internal author and committer timestamps. Useful for populating a contribution graph or logging activity on past dates.

---

## What it does

The script loops through a predefined list of dates. On each date it:

1. Appends a line to a dummy file called `activity.txt`
2. Stages and commits that change with a spoofed timestamp using `GIT_AUTHOR_DATE` and `GIT_COMMITTER_DATE`
3. On the **last date**, removes `activity.txt` entirely via `git rm` so the file leaves no trace in your repo after all commits are made
4. Pushes all commits to the remote in one go at the end

---

## Requirements

- Git installed and available in your terminal
- A cloned Git repository with a configured remote (`origin`)
- Bash shell (Git Bash on Windows, or any Unix terminal on macOS/Linux)

---

## How to run

**1. Copy the script into your repo**

Save the script as `backdate.sh` in the root of your local repository.

**2. Make it executable** *(macOS/Linux only)*

```bash
chmod +x backdate.sh
```

**3. Run it**

```bash
bash backdate.sh
```

On Windows using Git Bash, open Git Bash inside your repo folder and run the same command.

**4. Verify**

After the script completes, check your commit history:

```bash
git log --oneline
```

You should see one commit per date, each stamped with its respective backdated timestamp.

---

## Configuration

### Changing the branch

By default the script pushes to `main`. If your branch is named differently, update the last line:

```bash
git push origin your-branch-name
```

### Changing the dates

Edit the `DATES` array at the top of the script. Dates must follow the format:

```
YYYY-MM-DDTHH:MM:SS
```

For example:

```bash
DATES=(
  "2026-06-15T12:00:00"
  "2026-07-04T09:30:00"
)
```

### Changing the commit time

All commits default to noon (`T12:00:00`). To use a different time, edit the time portion of any entry in the `DATES` array.

---

## How the backdating works

Git exposes two environment variables that control commit timestamps:

| Variable | Purpose |
|---|---|
| `GIT_AUTHOR_DATE` | When the change was originally written |
| `GIT_COMMITTER_DATE` | When the commit was recorded into the repo |

Setting both to the same value makes the commit appear as if it was made on that date. GitHub's contribution graph reads `GIT_AUTHOR_DATE` when rendering activity.

---

## Notes

- The script must be run from inside a cloned Git repository
- The remote must be accessible (SSH key or HTTPS credentials configured)
- `activity.txt` is created automatically — you do not need to create it beforehand
- The file is cleaned up on the final commit, so it will not remain in your repo
