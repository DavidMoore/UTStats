# Unreal Tournament Stats

## Project Goals

Provide Unreal Tournament players with the best stats **recording**, **reporting** and **ranking** possible.

## History

### Unreal Tournament (1999)

The original Unreal Tournament 1999 included integration with NetGamesUSA's
ngStats, a stats provider that also supported MechWarrior and Quake III.

This provided a global record of games, including detailed match stats, and a weekly and quarterly
player leaderboard for the core games types (including Deathmatch and CTF).

NetGamesUSA was acquired by Microsoft in 2000 (http://www.microsoft.com/en-us/news/press/2000/jul00/netgamespr.aspx);
while UT's ngStats remained live, 
the website did degrade over time, and was eventually shut down entirely around about 2002.

### Unreal Tournament 2003 and 2004

For 2003, stats were available at http://ut2003stats.epicgames.com.

Unreal Tournament 2004 stats are available from Epic themselves, and is still live @ http://ut2004stats.epicgames.com/

These emulate the original ngStats functionality.

Stats to record

Event Name|Description|Parameters|Code|UT99 code
----------|-----------|----------|----|---------
item_get|An item is picked up (e.g. ammo, weapon, health, power up etc)|Ammo.uc:55|Weapon.uc:314|Inventory.uc:618|Pickup.uc:43,58,96
Suicide||Player (StatsID from state)|AUTGameMode::ScoreKill|GameInfo.uc:1216,1239,1244
||DamageType||
Kill|Killer|AUTGameMode::ScoreKill|GameInfo.uc:1302
||Victim||
||DamageType||
FirstBlood|Killer|AUTGameMode::ScoreKill|
||Victim||
||DamageType||
TeamKill||Killer||GameInfo.uc:1285
||Victim||
GameStart|||AUTGameMode::BeginGame|

Project overlaps

Matchmaking - Matching players together for a match.
Team balancing
Tourney play
Live casting

Points:
CTF - flag time, defensive shots (e.g. cover)


What kind of numbers are we talking about?

ELO is the ranking method used by ngStats, UT2004Stats, Quake Live stats, Tennis, Chess.