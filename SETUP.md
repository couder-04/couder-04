# Setup notes

This repo powers the live "GitHub Stats" banner in `README.md`. Two things need
to be set up once for the numbers to populate and stay updated.

## 1. Create a repo named exactly `couder-04`

GitHub only shows a profile README if you have a **public** repository whose
name matches your username. Create it at github.com/new, name it `couder-04`,
then push everything in this folder to it.

## 2. Add a Personal Access Token secret

`today.py` reads your stats via the GitHub GraphQL API, which needs a token.

1. Go to **github.com/settings/tokens** → *Fine-grained tokens* → **Generate new token**.
2. Give it access to **All repositories** (or just the ones you want counted).
3. Grant these permissions:
   - Account: `read:Followers`, `read:Starring`, `read:Watching`
   - Repository: `read:Commit statuses`, `read:Contents`, `read:Issues`, `read:Metadata`, `read:Pull Requests`
4. Copy the token.
5. In the `couder-04` repo on GitHub: **Settings → Secrets and variables → Actions → New repository secret**.
   - Name: `ACCESS_TOKEN`
   - Value: (paste the token)

## 3. Set your birthday (for the "Uptime" stat)

Open `today.py` and edit this line near the top with your actual date of birth:

```python
BIRTHDAY = datetime.datetime(2005, 1, 1)  # <-- change this
```

## 4. Let the Action run

The workflow in `.github/workflows/main.yml` runs automatically once a day and
also on every push to `main`. You can trigger it manually anytime from the
**Actions** tab → "Update profile stats SVGs" → **Run workflow**.

Once it runs successfully, `light_mode.svg` and `dark_mode.svg` will be
rewritten with your real stats (uptime, repos, commits, stars, lines of code,
followers) and committed back automatically.

## Optional: test locally

```bash
pip install -r requirements.txt
cp .env.example .env
# fill in ACCESS_TOKEN in .env
python today.py
```
