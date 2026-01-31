# CommitGrinder

GitHub Actions workflow to automate commits.

## Setup

1. **Create a Personal Access Token:**
   - Go to GitHub Settings → Developer settings → Personal access tokens
   - Generate token with `repo` scope
   - Copy the token

2. **Add GitHub Secrets:**
   - Go to your repo → Settings → Secrets and variables → Actions
   - Add these secrets:
     - `COMMIT_TOKEN`: Your PAT token
     - `E1`: Your GitHub email
     - `E2`: Your GitHub username

3. **Update `grind.cfg` in repo root:**
```bash
count= (ex. 10)
file="log.txt"
message="grind"
```

4. **Workflow file (`.github/workflows/grind.yml`):**
```yaml
name: CommitGrinder
on: workflow_dispatch

jobs:
  grind:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.COMMIT_TOKEN }}
      
      - name: Configure Git
        run: |
          git config user.email "${{ secrets.E1 }}"
          git config user.name "${{ secrets.E2 }}"
      
      - name: Run grinder
        run: |
          source grind.cfg
          for i in $(seq 1 $count); do
            echo "Commit $i - $(date)" >> "$file"
            git add "$file"
            git commit -m "$message $i"
          done
      
      - name: Push
        run: git push
```

## Usage

1. Go to your repo → Actions tab
2. Select "CommitGrinder" workflow
3. Click "Run workflow"
4. Watch commits appear

## Warning

This creates fake commit history. Use at your own risk.
