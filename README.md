# MLB Gameday Dashboard for Home Assistant

Home Assistant Manual Cards for an MLB Gameday Dashboard. Supports any MLB team via a built-in team picker.
Claude AI was used to generate these cards.
Feel free to fork this project and update it as you wish.

## Requirements

- [Home Assistant](https://www.home-assistant.io/) with access to `configuration.yaml`
- [button-card](https://github.com/custom-cards/button-card) custom card installed via HACS

## Setup

### 1. Add the sensors

Copy the contents of `configuration.yaml` into your own Home Assistant `configuration.yaml` (or include it via a `!include` if you split your config).

Restart Home Assistant after saving.

### 2. Pick your team

After restart, a new entity called `input_select.mlb_team` will appear. Set it to any of the 30 MLB teams. All sensors and cards will automatically update to follow the selected team.

The team list and their IDs/divisions are pulled from the official MLB Stats API:

```text
https://statsapi.mlb.com/api/v1/teams?sportId=1
```

### 3. Add the cards to your dashboard

Each `.txt` file contains a YAML definition for a [button-card](https://github.com/custom-cards/button-card) manual card. To add one:

1. In your dashboard, click **Edit** → **Add Card** → **Manual**
2. Paste the contents of the `.txt` file into the card editor
3. Click **Save**

| File | Description |
| ---- | ----------- |
| `Scoreboard Card.txt` | Live scoreboard with score, inning, count, bases, and next game info |
| `PitchingGrid Card.txt` | Current pitcher stats and pitch location grid |
| `BatterCard.txt` | Current batter with season stats |
| `StandingsCard.txt` | Division standings for the selected team's division |
| `RefreshCard.txt` | Button to manually force-refresh all MLB sensors |

### 4. Sensor refresh rates

| Sensor | Interval | Notes |
| ------ | -------- | ----- |
| `mlb_master_data` | 5 seconds | Live game feed — drives most cards |
| `mlb_game_id` | 1 hour | Today's game ID for selected team |
| `mlb_next_game` | 1 hour | Upcoming schedule |
| `mlb_standings` | 30 minutes | Division standings |
| `mlb_teams_data` | 24 hours | Team/division metadata from MLB API |

### 5. Force refreshing data

Add `RefreshCard.txt` to your dashboard. Tapping it calls `script.mlb_refresh`, which immediately re-polls all MLB REST sensors. The refresh is **rate-limited to 2 times per 30 minutes** — the counter resets automatically at every `:00` and `:30`. The card dims and shows a message when the limit is reached.

Hold-tapping the card bypasses the rate limit and specifically refreshes only the teams metadata sensor (`mlb_teams_data`), which otherwise only updates once per day.

You can also trigger the refresh from **Developer Tools → Services** by calling `script.turn_on` with entity `script.mlb_refresh`.

---

## Screenshots

Scoreboard Card on Mobile

![Scoreboard Card](https://github.com/user-attachments/assets/ec910f1a-218d-4d4f-aa88-f27f0b6092f7)

Pitching Grid Card on Mobile

![Pitching Grid Card](https://github.com/user-attachments/assets/059b35ba-eee5-4bd6-9e41-b69378d14bde)

Batter Card on Mobile

![Batter Card](https://github.com/user-attachments/assets/fa0b6bb3-f31f-475a-8b45-f53a15ce1da8)

Division Standings (Including Spring Training) on Mobile

![Division Standings](https://github.com/user-attachments/assets/e6257b53-2b63-411c-9a7f-386364a78511)

Live Game Screenshots

![Live Game 1](https://github.com/user-attachments/assets/182c4b0a-8ac8-4114-9b20-8c4cca416a6b)

![Live Game 2](https://github.com/user-attachments/assets/865cc32c-ba94-4561-adac-01e452dfcd33)

![Live Game 3](https://github.com/user-attachments/assets/2f54f5c4-1020-4018-91d8-b189b1308123)
