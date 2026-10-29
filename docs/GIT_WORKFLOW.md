# Git workflow, branches and commit history

[Overview](../README.md) | [Report](../REPORT.md) | [Git evidence](EVIDENCE.md#q2-version-control)

## Demonstrated sequence

This section reconstructs the workflow already completed. It is not a script to rerun over the existing repository.

```bash
cd ~
mkdir devops-lab1
cd devops-lab1
git init
git branch -M main
# app.py and README.md were created here.
git add app.py README.md
git commit -m "Initial Python application setup"

git switch -c feature-tests
# test_app.py, requirements.txt and .gitignore were added here.
python3 -m venv venv
. venv/bin/activate
pip install -r requirements.txt
pytest -v
git add .gitignore requirements.txt test_app.py
git commit -m "Add unit tests with pytest"
git switch main
git merge feature-tests

git switch -c docs-update
# The CI/CD stages section was added to README.md here.
git add README.md
git commit -m "Document CI/CD pipeline stages"
git switch main
git merge docs-update
git log --oneline --graph --decorate --all
```

The repository's `.gitignore` excludes the virtual environment, Python caches and compiled bytecode. Staging named files avoids accidentally including generated deployment output or credentials.

## GitHub remote and authentication

```bash
git remote add origin https://github.com/ArghyadiptaGoswami4259/DEV-OPS.git
git remote -v
git push -u origin main
```

The practical used GitHub CLI browser authentication after the initial push requested credentials. The [authentication screenshot](evidence/screenshots/ss18-github-auth.png) masks the token. Never commit the local GitHub CLI credential file or paste an access token into the repository URL.

The local `gh` authentication belongs to the developer's Linux user. It is separate from Jenkins's service-account configuration. The public repository can be read without transferring the developer's authentication to Jenkins.

## Recorded commits before this expanded documentation

| Order | Commit | Purpose |
|---|---|---|
| 1 | [04a4b00](https://github.com/ArghyadiptaGoswami4259/DEV-OPS/commit/04a4b00e14eacf823b2cf8974ef4a22d5bd79237) | Initial Python application setup |
| 2 | [0bbd0f0](https://github.com/ArghyadiptaGoswami4259/DEV-OPS/commit/0bbd0f0a771da033480576df6384cef02da0f3e8) | Add unit tests with pytest |
| 3 | [3fa8f1b](https://github.com/ArghyadiptaGoswami4259/DEV-OPS/commit/3fa8f1b32c33c9e86fe7c3b60900b54e953b1bc4) | Document CI/CD pipeline stages |
| 4 | [ed79388](https://github.com/ArghyadiptaGoswami4259/DEV-OPS/commit/ed79388f8fca937a31c45ea88d3a2dfecc6d13f0) | Add Jenkins CI/CD pipeline with Build, Test and Deploy stages |
| 5 | [288b24a](https://github.com/ArghyadiptaGoswami4259/DEV-OPS/commit/288b24af72fcc646e3a61017cee0a9d0fa9fb281) | Add Lab 1 command documentation |
| 6 | [49f7e9c](https://github.com/ArghyadiptaGoswami4259/DEV-OPS/commit/49f7e9cabcc004a4e3cf7f4c03205b99ec436516) | Add Lab 1 CI/CD workflow report |
| 7 | [5fc49ac](https://github.com/ArghyadiptaGoswami4259/DEV-OPS/commit/5fc49acf2eddbdbb7267b6f3ad5bcf71cd4c696f) | Add complete Lab 1 documentation, evidence gallery and screenshots |

The review baseline is commit `5fc49acf2eddbdbb7267b6f3ad5bcf71cd4c696f`. Later documentation commits should be added normally, not used to replace this history.

## Merge history

The `feature/pipeline-docs` branch was merged into `main` with a two-parent merge commit (`767f898`), keeping the branch work visible in the history graph before the branch was deleted locally.

## Local branches versus GitHub branches

The captured local history includes `feature/pipeline-docs`. The GitHub branch listing at the review baseline contains only `main`, because `git push -u origin main` does not publish every local branch.

Publishing the already-existing feature branches is optional but makes them visible to a GitHub-only evaluator:

```bash
cd ~/devops-lab1
git branch --list
# Run only if both names above still exist locally:
git push origin feature-tests docs-update
```

There is no need to delete, recreate, force-push or rewrite branches. The existing screenshots already demonstrate branch creation and the merge operation.

## Capture current history for the submission

The provided helper records fresh Git output from the real repository:

```bash
cd ~/devops-lab1
bash scripts/capture-evidence.sh
```

It does not change Git identity or create commits. Review the generated logs before staging them. The captured history is the state before the subsequent evidence-upload commit; that ordering is normal.
